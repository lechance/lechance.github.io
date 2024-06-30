---
title: HTML Encoding (Character Sets)
date: 2016-03-08 09:37:33
categories:
 - Web
tags:
 - html
 - encoding
---

To display an `HTML` page correctly, a web browser must know the character set(character encoding) to use.

##### What is Character Encoding?
ASCII(American Standard Code for Information Interchange) was the first *character encoding standard (also called `character set`), It defines 127 different alphanumeric characters that could be used on the internet.

ASCII supported numbers(0-9), English letters(A-Z), and some special character like ! $ + - ( ) @ < > .

ASCII(Windows-1252) was the original Windows character set. It supported 256 different character codes.

ISO-8859-1 was the default character set for HTML 4. It also supported 256 different character codes.

Because ANSI and ISO were limited, the default character encoding was chanaged to UTF-8 in HTML 5.

UTF-8(Unicode) covers almost all of the characters and symbols in the world.

>All HTML 4 processors also support UTF-8.

##### The HTML charset Attribute
To display an HTML page correctly, a web browser must know the charachter set used in the page.

This is specified in the `<meta>` tag:

For HTML4:
```html
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
```
For HTML5:
```html
<meta charset="UTF-8">
```

`If a browser detects ISO-8859-1 in a web page, it defaults to ANSI, because ANSI is indentical to ISO-8859-1 except that ANSI has 32 extra characters.
