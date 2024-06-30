---
title: JavaScript HTML DOM
date: 2015-12-23 21:55:13
tags:
 - javascript
---
#### HTML DOM

**When the web page is loaded, the browser will create a document object model of the page.**
_HTML DOM Model is constructed as an object tree_

![html-tree](images/html-dom-tree.png)

**Through the programmable object model _JavaScript_ was enough to create dynamic HTML**


> - JavaScript can change all the HTML elements on your page
> -  JavaScript can change all the HTML in the page properties
> -  JavaScript can change all the CSS styles in the page
> -  JavaScript to be able to respond to all events page

##### Finding HTML elements
Typically, You need to manipulate HTML elements with _JavaScript_
_In order to do this, you must first locate the element, There are three ways to do this:_

 * HTML element by ID found
 
```html
var x =document.getElementById("intro");
```

* By label name to find HTML elements

```html
First find the id = "main" element,and then look for the "main" all of the elements

  var x =document.getElementById("main");

  var y =x.getElementsByTagName("p");

```

 * Through the class name to find HTML elements

```html
Find HTML elements by class name is not valid in IE 5,6,7,8
```

##### Change HTML contents

```javascript
document.getElementById("id").innerHTML= "new HTML";
```

##### Change HTML attributes

```javascript
document.getElementById("id").src="landscape.png";
```

##### Change HTML CSS (Cascading Style Sheets)

```javascript
Syntax:
document.getElementById(id).style.property="new style"
```
##### Adding and Removing nodes (HTML element)
----
 - Adding node

<!--more-->

_If you need to add new elements to the HTML DOM, you must first create the element (element node), and then append the element to an existing element._

```html
<div id="div1">
	<p id="p1">this is a paragraph</p>
	<p id="p2">this is an another paragraph</p>
</div>

<script>
	var para=document.createElement("p");
	var node=document.createTextNode("this a new paragraph");
	para.appendChild(node);

	var element=document.getElementById("div1");
	element.appendChild(para);
</script>
```

- Removing node

_If you need to remove HTML elements, you must first obtain the parent element of the element._

```html
<div id="div1">
  <p id="p1">this is a paragraph</p>
	<p id="p2">this is an another paragraph</p>
</div>
<script>
	var parent=document.getElementById("div1");
	var child=document.getElementById("p2");

  parent.removeChild(child);
</script>
```
