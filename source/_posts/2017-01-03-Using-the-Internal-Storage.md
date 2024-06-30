title: Using the Internal Storage
date: 2017-02-27 20:53:36
comments: true
categories: Android
tags:
- internal storage
- android
---

### Using the Internal Storage

_You can save files directly on the devices's internal storage. By default, files saved to the internal storage are private to your application and other applications cannot access them (nor can the user). When the user uninstalls your application, these files are removed._

* To create and write a private file to the internal storage:
	1. Call <em>openFileOutput()</em> with the name of the file and the operating mode. This retures a <em>FileOutputStream</mark>.
	2. Write to the file with <em>write()</em>.
	3. Close the stream with <em>close()</em>.

For example:
```java

String  FILENAME = "hello_file";
String string = "hello world";

FileOutputStream fos = openFileOutput(FILENAME, Context.MODE_PRIVATE);
fos.write(string.getBytes());
fos.close();

```
<em>MODE_PRIVATE</em> will create the file (or replace a file of the same name) and make it private to your application. Other modes available are: <em>MODE_APPEND, MODE_WORLD_READABLE, and MODE_WORLD_WRITEABLE</em>.

* To create a file from internal storage:
	1. Call `openFileInput()` and pass it the name of the file to read, This return a `FileInputStream`.
	2. Read bytes from the file with `read()`.
	3. Then close stream with `close()`.

><b>Tip:</b> If you want to save a static file in your applcation at compile time, save the file in your project `res/raw/` directory. You can open it with `openRawResource()`, passing the `R.raw.<filename>` resource ID. This method returns an `InputStream` that you can use to read the file (but you cannot write to the original file).

### Saving Cache files
If you'd like to cache some data, rather than store it persistently, you should use `getCacheDir()` to open a `File` that represents the internal directory where your application should save temporary cache files.

When the device is low on internal storage space, Android may delete these cache files to recover space. However, you should not rely on the system to clean up these files for you. You should always maintain the cache files yourself and stay within a reasonable limit of space consumed, such as 1MB. When the user uninstalls your application, these files are removed.

### Other useful methods of <ins>Context</ins> Object

* <b> `getFilesDir()`</b> - Gets the absolute path to the filesystem directory where your internal files are saved.

* <b> `getDir()`</b> - Creates (or opens an existing) directory within your internal storage space.

* <b> `deleteFile()`</b> - Deletes a file saved on the internal storage.

* <b> `fileList()`</b> - Returns an array of files currently saved by your application.

Refer to : https://developer.android.google.cn - Guide!