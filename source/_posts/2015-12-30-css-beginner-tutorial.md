---
title: Css Beginner Tutorial
date: 2015-12-30 00:28:04
tags:
 - css
categories:
- Web
---

**CSS**, or **Cascading Styles Sheets**, is a way to style and prsent HTML.  Whereas the HTML is the **meaning** or **content**, the style sheet is the **presentation** of that document.

Styles don't smell or taste anything like HTML, they have a format of **property:value** and most properties can be applied to most HTML tags.

### Contents

#### `Applying CSS` - The different ways you can apply CSS to HTML

>There are three ways to apply CSS to HTML tags using the `style` attribute.

*	**In-line**
In-line styles are plonked straight into the HTML tags use the `style` attribute.
They look something like this:
```css
   <p style="color: read">text</p>
```
This will make that specific paragraph red.

  But, if you remember, the best-practice approach is that the HTML should be a stand-alone, **presentation free** document, and so in-line styles should be avoided wherever possible.

*	**Internal**

Embeded, or internal, styles are used for the whole page. Inside the `head` element, the `style` tags surround all of the styles for the page.

```html
<!DOCTYPE html>
<html>
<head>
<title>Css Example</title>
<style>
	p{
		color: read;
	}

	a{
		color: blue;
	}
</style>
...
```

This willl make all of the paragraphs in the page red and all of the links blue.

Although preferable to soiling our HTML with inline presentation, It is similarly usually preferable to keep the HTML and the CSS files separate, and so we are left with our savior...


*	**External**

External styles are used for the whole, mutiple-page website. There is a **separate CSS file**, which will simply look something like:

```css
	p{
		color: read;
	}

	a{
		color: blue;
	}
```

If this file is saved as "styles.css" in the same directory as your HTML page then it can be linked to in the HTML like this:

```html
<!DOCTYPE html>
<html>
<head>
	<title>CSS Example</title>
	<link rel="stylesheet" href="style.css">
...
```

*	<h5><p style="color:#8B2"><b>Apply</b></p></h5>

To get the most from this guide, it would be a good idea to try out the code as we go along. so start a fresh new file with your text-editor and save the blank document as "style.css" in the same directory as your HTML file.

Now change your HTML file so that it starts something like this:

```html
<!DOCTYPE html>
<html>
<head>
	<title>My first web page</title>
	<link rel="stylesheet" href="style.css">
</head>
...
```
Save the HTML file. This now links to the CSS file, which is empty at the moment, so won't change a thing. As you work you way throwgh the CSS Beginner Tutrial, you will be able to add to and change the CSS file and  see the results by simply refreshing the browser window that has the HTML file in it, as we did before.

<!--more-->

#### `Selectors, Properties, and Values` - The bits that make up CSS.

_Whereas HTML has tags, CSS has selectors. Selectors are the names given to styles in internal and external style sheets. In this CSS Beginner Tutorial we will be concentrating on HTML selectors, which are simply the names of HTML tags and are used to change the style of a specific type of element._

For each selector there are **“properties”** inside **curly brackets**, which simply take the form of words such as color, font-weight or background-color.

A **value** is given to the property following a **colon** (NOT an “equals” sign) and **semi-colons** separate the properties.

```css
body {
    font-size: 14px;
    color: navy;
}
```

This will apply the given values to the `font-size` and `color` properties to the `body` selector.

So basically, when this is applied to an HTML document, text between the body tags (which is the content of the whole window) will be 14 pixels in size and navy in color.

##### Lengths and Percentages

There are many property-specific units for values used in CSS, but there are some general units that are used by a number of properties and it is worth familiarizing yourself with these before continuing.

* `px` (such as font-size: 12px) is the unit for pixels.
* `em` (such as font-size: 2em) is the unit for the calculated size of a font. So “2em”, for example, is two times the current font size.
* `pt` (such as font-size: 12pt) is the unit for points, for measurements typically in printed media.
* `%` (such as width: 80%) is the unit for… wait for it… percentages.

Other units include `pc` (picas), `cm` (centimeters), `mm` (millimeters) and `in` (inches).

When a value is `zero`, you do not need to state a unit. For example, if you wanted to specify no border, it would be `border: 0`.

_“px” in this case, doesn’t actually necessarily mean pixels - the little squares that make up a computer’s display - all of the time. Modern browsers allow users to zoom in and out of a page so that, even if you specify font-size: 12px, or height: 200px, for example, although these will be the genuine specified size on a non-zoomed browser, they will still increase and decrease in size with the user’s preference. It’s a good thing. Trust me._


#### `Colors` - How to use color.

_CSS brings 16,777,216 colors to your disposal. They can take the form of a name, an RGB (red/green/blue) value or a hex code._

The following values, to specify full-on as red-as-red-can-be, all produce the same result:

*    red
*    rgb(255,0,0)
*    rgb(100%,0%,0%)
*    \#ff0000
*    \#f00

Predefined color names include <h5 style="color:green">aqua, black, blue, fuchsia, gray, green, lime, maroon, navy, olive, purple, red, silver, teal, white, and yellow. transparent </h5>is also a valid value.

_With the possible exception of black and white, color names have limited use in a modern, well-designed web sites because they are so specific and limiting._

The three values in the RGB value are from 0 to 255, 0 being the lowest level (no red, for example), 255 being the highest level (full red, for example). These values can also be a percentage.

Hexadecimal (previously and more accurately known as “sexadecimal”) is a base-16 number system. We are generally used to the decimal number system (base-10, from 0 to 9), but hexadecimal has 16 digits, from 0 to f.

The hex number is prefixed with a hash character (#) and can be three or six digits in length. Basically, the three-digit version is a compressed version of the six-digit (#ff0000 becomes #f00, #cc9966 becomes #c96, etc.). The three-digit version is easier to decipher (the first digit, like the first value in RGB, is red, the second green and the third blue) but the six-digit version gives you more control over the exact color.

#### Text - How to manipulate the size and shape of text

_You can alter the size and shape of the text on a web page with a range of properties._

##### <div style="color:#8b4">font-family</div>

This is the font itself, such as Times New Roman, Arial, or Verdana.

The user’s browser has to be able to find the font you specify, which, in most cases, means it needs to be on **their** computer so there is little point in using obscure fonts that are only sitting on **your** computer. There are a select few “**safe**” fonts (the most commonly used are Arial, Verdana and Times New Roman), but you can specify more than one font, separated by commas. The purpose of this is that if the user does not have the first font you specify, the browser will go through the list until it finds one it does have. This is useful because different computers sometimes have different fonts installed. So font-family: arial, helvetica, serif, will look for the Arial font first and, if the browser can’t find it, it will search for Helvetica, and then a common serif font.

Note: if the name of a font is more than one word, it should be put in quotation marks, such as font-family: "Times New Roman".

You can use a wider selection than the “safe” fonts using several methods outlined in the CSS Advanced Tutorial but if you’re just getting to grips with CSS, we suggest sticking with this basic standard approach for the moment.

##### <div style="color:#8b4">font-size</div>

font-size sets the size of the font. Be careful with this — text such as headings should not just be an HTML paragraph (p) in a large font - you should still use headings (h1, h2 etc.) even though, in practice, you could make the font-size of a paragraph larger than that of a heading (not recommended for sensible people).

##### <div style="color:#8b4">font-weight</div>

font-weight states whether the text is bold or not. Most commonly this is used as font-weight: bold or font-weight: normal but other values are bolder, lighter, 100, 200, 300, 400 (same as normal), 500, 600, 700 (same as bold), 800 or 900.

_Play around with these font-weight values if you want see their effect but, keep in mind, that some older browsers become a little confused with anything other than bold and normal so we suggest sticking to those unless you’re a typography ninja._

##### <div style="color:#8b4">font-style</div>

font-style states whether the text is italic or not. It can be font-style: italic or font-style: normal.

##### <div style="color:#8b4">text-decoration</div>

text-decoration states whether the text has got a line running under, over, or through it.

    text-decoration: underline, does what you would expect.
    text-decoration: overline places a line above the text.
    text-decoration: line-through puts a line through the text (“strike-through”).

This property is usually used to decorate links and you can specify no underline with text-decoration: none.

Underlines should only really be used for links. They are a commonly understood web convention that has lead users to generally expect underlined text to be a link.

##### <div style="color:#8b4">text-transform</div>

text-transform will change the case of the text.

    text-transform: capitalize turns the first letter of every word into uppercase.
    text-transform: uppercase turns everything into uppercase.
    text-transform: lowercase turns everything into lowercase.
    text-transform: none I’ll leave for you to work out.

So, a few of these things used together might look like this:

```css
body {
    font-family: arial, helvetica, sans-serif;
    font-size: 14px;
}

h1 {
    font-size: 2em;
}

h2 {
    font-size: 1.5em;
}

a {
    text-decoration: none;
}

strong {
    font-style: italic;
    text-transform: uppercase;
}
```

##### <div style="color:#8b4">text-spaceing</div>

Before we move on from this introduction to styling text, a quick look at how to space out the text on a page:

The letter-spacing and word-spacing properties are for spacing between letters or words. The value can be a length or normal.

The line-height property sets the height of the lines in an element, such as a paragraph, without adjusting the size of the font. It can be a number (which specifies a multiple of the font size, so “2” will be two times the font size, for example), a length, a percentage, or normal.

The text-align property will align the text inside an element to left, right, center, or justify.

The text-indent property will indent the first line of a paragraph, for example, to a given length or percentage. This is a style traditionally used in print, but rarely in digital media such as the web.

```css
p {
    letter-spacing: 0.5em;
    word-spacing: 2em;
    line-height: 1.5;
    text-align: center;
}
```

#### Margins and Padding	- How to space things out

_margin and padding are the two most commonly used properties for spacing-out elements. A margin is the space outside something, whereas padding is the space inside something._

Change the CSS code for *h2* to the following:

```css
h2 {
    font-size: 1.5em;
    background-color: #ccc;
    margin: 20px;
    padding: 40px;
}
```

This leaves a 20-pixel width space around the secondary header and the header itself is fat from the 40-pixel width padding.

The four sides of an element can also be set individually. <h5 style="color:#8b4">margin-top, margin-right, margin-bottom, margin-left, padding-top, padding-right, padding-bottom and padding-left</h5> are the self-explanatory properties you can use.

**The Box Model**

Margins, padding and borders are all part of what’s known as **the Box Model.** The Box Model works like this: in the middle you have the content area (let’s say an image), surrounding that you have the padding, surrounding that you have the border and surrounding that you have the margin. It can be visually represented like this:

<div id="boxmodel" style="color: #a06700; margin: 20px 0; overflow: auto; padding: 10px 20px 20px 20px; background-color: #e90;">
    Margin box
    <div style="padding: 10px 20px 20px 20px; margin: 10px 20px 20px 20px; background-color: #f4bb55;">
        Border box
        <div style="padding: 10px 20px 20px 20px; margin: 10px 20px 20px 20px; background-color: #f9ddaa;">
            Padding box
            <div style="padding: 20px; margin: 10px 20px 20px 20px; background-color: #fff;">
                Element box
            </div>
        </div>
    </div>
</div>

You don’t have to use all of these, but it can be helpful to remember that the box model can be applied to every element on the page, and that’s a powerful thing!

#### Borders - Erm. Borders. Things that go around things.

_Borders can be applied to most HTML elements within the body._

To make a border around an element, all you need is **border-style**. The values can be **solid, dotted, dashed, double, groove, ridge, inset** and **outset**.

**border-width** sets the width of the border, most commonly using pixels as a value. There are also properties for **border-top-width, border-right-width, border-bottom-width and border-left-width**.

Finally, `border-color` sets the **color**.

Add the following code to the CSS file:

```css
h2 {
    border-style: dashed;
    border-width: 3px;
    border-left-width: 10px;
    border-right-width: 10px;
    border-color: red;
}
```

This will make a red dashed border around all HTML secondary headers (the h2 element) that is 3 pixels wide on the top and bottom and 10 pixels wide on the left and right (these having over-ridden the 3 pixel wide width of the entire border).

####  Putting It All Together - Throwing all of the above ingredients into one spicy hotpot.

_You should already have an HTML file like the one made at the end of the HTML Beginner Tutorial, with a line that we added at the start of this CSS Beginner Tutorial, linking the HTML file to the CSS file._

The code below covers all of the CSS methods in this section. If you save this as your CSS file and look at the HTML file then you should now understand what each CSS property does and how to apply them. The best way to fully understand all of this is to mess around with the HTML and the CSS files and see what happens when you change things.

```css
body {
    font-family: arial, helvetica, sans-serif;
    font-size: 14px;
    color: black;
    background-color: #ffc;
    margin: 20px;
    padding: 0;
}

/* This is a comment, by the way */

p {
    line-height: 21px;
}

h1 {
    color: #ffc;
    background-color: #900;
    font-size: 2em;
    margin: 0;
    margin-bottom: 7px;
    padding: 4px;
    font-style: italic;
    text-align: center;
    letter-spacing: 0.5em;
    border-bottom-style: solid;
    border-bottom-width: 0.5em;
    border-bottom-color: #c00;
}

h2 {
    color: white;
    background-color: #090;
    font-size: 1.5em;
    margin: 0;
    padding: 2px;
    padding-left: 14px;
}

h3 {
    color: #999;
    font-size: 1.25em;
}

img {
    border-style: dashed;
    border-width: 2px;
    border-color: #ccc;
}

a {
    text-decoration: none;
}

strong {
    font-style: italic;
    text-transform: uppercase;
}

li {
    color: #900;
    font-style: italic;
}

table {
    background-color: #ccc;
}
```
