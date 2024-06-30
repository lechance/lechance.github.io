title: Html 5 Web Workers
date: 2017-02-12 12:37:42
categories: Html
tags:
- html5
- web worker
comments: true

---

> A #web worker# is a JavaScript script executed from an HTML page that runs in the background, independently of other user-interface scripts that may also have been executed from the same HTML page. Web workers are often able to utilize multi-core CPUs more effectively.
>
>The simplest use of workers is for performing a computationally expensive tasks without interrupting user interface.

## Check Web Worker Support

Before creating a web worker. check whether the uesr's browser support it:
```html
if(typeof(Worker) !== "undefined"){
    //Yes, web worker support!
    //do something with your code...
} else {
    //Sorry! No web worker support...
}
```"

## Create a Web Worker File
Now, Let's create our web worker in an external JavaScript.

```javascript
var i = 0;

function timedCount() {
    i = i + 1;
    postMessage(i);	//which is used to post a message back to the HTML page.
    setTimeout("timedCount", 500);
}

timedCount();
}
```

**Note:**Normally web workers are not used for such simple scripts, but for more CPU intensive tasks.

## Create a Web Worker Object

## Terminate a Web Worker

## Reuse the Web Worker

## Full Web Worker Example Code

## Web Worker and the DOM

Since web workers are in external files, they do not have access to the following JavaScript objects:

* The **window** object
* The **document** object
* The **parent** object


_Refer to Internet_
