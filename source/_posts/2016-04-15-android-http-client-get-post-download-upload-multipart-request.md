title: ANDROID HTTP CLIENT-GET,POST,DOWNLOAD,UPLOAD,MULTIPART REQUEST
date: 2016/04/15 23:05
categories:
- Android
- Network
tags:
- android
- http
- HttpURLConnection
---

通常Android应用程序要与远程服务器进行信息交换．最简单的方式是以HTTP协议作为基础来传输信息．在某些环境下使用HTTP协议是非常有用的，比如从远程服务器下载图片或者上传二进制数据到服务器．Android应用程序通过执行GET和POST请求(request)来发送数据．在这篇帖子，我想要分析分析一下如何使用`HttpURLConnection`与远程服务器进行通信．

### GET和POST请求
GET和POST请求在HTTP协议中是基块(the base blocks)。为了生成这类请求我们首先需要打开一个指向远程服务器的链接:
```java
HttpURLConnection con = (HttpURLConnection) (new URL(url)).openConnection();
con.setRequestMethod("POST");
con.setDoInput(true);
con.setDoOutput(true);
con.connect();
```

首先，我门通过`url`参数构建一个`Url`对象，通过这个对象的`openConnection`方法得到`HttpURLConnection`,代码第二行我们设置请求的方式为Post，最后connect到服务器。

一旦我们持有一个打开了的链接我们就可以使用`OutputStream`进行写入我们的数据了。
```java
con.getOutputStream().wirte(("name="+name).getBytes());
```
对于我们已经知道的参数我们可以使用键值对来写入。

最后一步是使用`InputStream`读取响应数据。
```java
InputStream ins = con.getInputStream();
byte[] b = new byte[1024];
while(ins.read(b) != -1)
    buffer.append(new String(b));
con.disconnect();
```

到目前为止所有的工作都非常简单，但是需要记住的一点是：使用`HTTP`链接是一个耗时的操作，有时它可能需要很长的时间，因此我们不能把它放在UI线程里运行，否则我们会得到一个ANR(Application Not Responding)对话框。

解决这个问题的方法是我们可以使用`AsyncTask`
```java
private class SendHttpRequestTask extends AsyncTask<String, Void, String>{

    @Override
    protected String doInBackground(String... params){
	String url = params[0];
	String name = params[1];
	String data = sendHttpRequest(url, name);
	return data;
    }

    @Override
    protected void onPostExecute(String result){
	edtResp.setText(result);
	item.setActionView(null);
    }
}
```


### 从服务器下载数据
一个最普遍的情景是应用程序从远程服务器下载一些数据。我们假设要从服务器下载一张图片，在这例子里我们依旧使用`AsyncTask`来完成我们的操作，代码如下：
```java
public byte[] downloadImage(String imgName){
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    try{
	System.out.println("URL ["+url+"] - Name ["+imgName+"]");

	HttpURLConnection con = (HttpURLConnection) (new URL(url)).openConnection();
	con.setRequestMethod("POST");
	con.setDoInput(true);
	con.setDoOutput(true);
	con.connect();
	con.getOutputStream.write(("name=" + imgName).getBytes());

	InputStream is = con.getInputStream();
	byte[] b = new byte[1024];

	while( is.read(b) != -1)
	    baos.write(b);

	con.disconnect();
    }catch(Throwable t){
	t.printStackTrace();
    }
    return baos.toByteArray();
}
```
这个方法以下面这样的方式调用：
```java
private class SendHttpRequestTask extends AsyncTask<String, Void, String>{

    @Override
    protected byte[] doInBackground(String...params){
	String url = params[0];
	String name = params[1];

	HttpClient client = new HttpClient(url);
	byte[] data = client.downloadImage(name);

	return data;
    }

    @Override
    protected void onPostExecute(byte[] result){
	Bitmap img = BitmapFactory.decodeByteArray(result, 0, result.length);
	imgView.setImageBitmap(img);
	item.setActionView(null);
    }
}
```


### 使用MultipartRequest上传数据到服务器
这是在处理`http`链接里最复杂的部分，本地的`HttpUrlConnection`不处理这类请求,一个Android应用程序上传一些二进制数据到服务器时它会发生。`MultipartRequest`类主要对文件上传进行操作，在上传时，编码格式为`enctype="multipart/form-data"格式，以二进制方式提交，提交方式为post方式。以上传一张图片为例子，在种情况下请求会有点复杂，因为一个“一般”的请求满足不了我门的需求，因此我们创建一个`MultipartRequest`.

`MultipartRequest` 是一个通过像参数和二进制数据这样不同的部分构造的请求。

首先第一步是打开一个链接，通知(informing)服务器我们要发一些二进制数据。
```java
public void connectForMultipart() throws Exception{
    con = (HttpURLConnection) (new URL(url)).openConnection();
    con.setRequestMethod("POST");
    con.setDoInput(true);
    con.setDoOutput(true);
    con.setRequestProperty("Connection", "Keep-Alive");
    con.setRequestProperty("Content-Type", "multipart/form-data"; boundary=" + boundary);
    con.connect();
    os = con.getOutputStream();
}
```

在第７行我们指定了请求的`content-type`和另一个字段*boundary*,　这个字段是一个字符序列用于分隔不同的部分。

对于每个部分，如果它是一个文本像提交的参数或者是一个文件（二进制数据）就需要指定想要添加的部分。
```java
public void addFormPart(String paramName, String value)throws Exception{
    writeParamData(paramName, value);
}

private void writeParamData(String paramName, String value) throws Exception{
    os.wirte( (delimiter + boundary + "\r\n").getBytes());
    os.write( "Content-Type: text/plain\r\n".getBytes());
    os.wirte( ("Content-Disposition: form-data; name=\"" +paramName + "\"\r\n").getBytes());
    os.write( ("\r\n" + value + "\r\n").getBytes());
}
```

这里：
```java
private String delimiter = "--";
private String boundary = "SwA"+ Long.toString(System.currentTimeMillis())+"SwA";
```
使用下面的方式添加文件部分：
```java
public void addFilePart(String paramName, String fileName, byte[] data) throws Exception{
    os.write( (delimiter + boundary + "\r\n").getBytes());
    os.write( ("Content-Disposition: form-data; name=\""+ paramName + "\"; filename=\"" + fileName + "\"\r\n").getBytes());
    os.write( ("Content-Type: application/octet-stream\r\n").getBytes());
    os.write( ("Content-Transfer-Encoding: binary\r\n").getBytes());
    os.write("\r\n".getBytes());

    os.write(data);
    os.write("\r\n".getBytes());
}
```
因此，使用在我们的例子里：
```java
private class SendHttpRequestTask extends AsyncTask<String, Void, String>{

    @Override
    protected String doInBackground(String... params){
	String url = params[0];
	String param1 = params[1];
	String param2 = params[2];
	Bitmap b = BitmapFactory.decodeResource(UploadActivity.this.getResources(), R.drawable.logo);

	ByteArrayOutputStream baos = new ByteArrayOutputStream();
	b.compress(CompressFormat.PNG, 0, baos);

	try{
	    HttpClient client = new HttpClient(url);
	    client.connectForMulitipart();
	    client.addFormPart("param1", param1);
	    client.addFormPart("param2", param2);
	    client.addFilePart("file", "logo.png", baos.toByteArray());
	    client.finishMultipart();
	    String data = client.getResponse();
	}catch(Throwable t){
	    t.printStackTrace();
	}
	return null;
	}

	@Override 
	protected void onPostExecute(String data){
	    item.setActionView(null);
	}
}

```
