---
title: Markdown
tags:
 - markdown

date: 2015-06-28 02:32:22
---
## Markdown

_Before I start to write a blog, I wourld like to do some **Markdown** syntax summary. I think only of this "pen" very skilled master, I can write code to change the world. So, get started._

#### INTRODUCTION

Markdown is a text-to-HTML conversion tool for web writers. Markdown allows you to wirte using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML). For more detailed information of *Markdown* visit [markdown](http://daringfireball.net/projects/markdown/) website.



#### Syntax Cheatsheet:

##### Phrase Emphasis

     *italic*	**bold**
     _italic_	__bold__

##### Links
Inline:

	An [example](http://url.com/ "title")

 Reference-style labels (titles are optional)

	An [example][id]. Then, anywhere else in the doc, define te link:
		
			[id]: https://example.com/ "title"
##### Images
Inline (title are optional):

	    ![alt text](/path/img.png "title")

Reference-style:

	![alt text][id]
	[id]:/url/to/img.png "title"

<!--more-->

##### Headers

Setext-style:

-------
	Header 1
	------
######atx-style (closing #'s are optional):

	# head 1 ##
	## head 2 ##
	###### header 3 ######

##### Lists
Odered, without paragraphs:

	   1. 	foo
	   2. 	bar
Unordered, with paragraphs:

	*	A list itme.
		with multiple paragraphs.
	#	bar
You can nest them:

	*	Abada
		* answer
	*	Baadsa
		1.	bunk
		2.	bupkd
			*  DFSFSIE
		3. busdfa
	*	Cusning

#####Blockquotes

	> Email-style angle brackets
	> are used for blockquotes.

	> > And, they can be nested.
	
	> #### Headers in blockquotes
	
	>
	> * You can quote a list.
	> * etc.
#####Code Spans
	`<code>` spans are delimited by backticks.

	You can include literal backticks like `` `this` ``.

eg:
<code>
public static void main(String[] args){
	System.out.printf("hello world");
}
</code>
or

`` 
`this` 
``

#####Preformatted Code Blocks
Indent every line of a code block by at least 4 spaces or tab.
	
	This is a normal pargraph.

		This is a preformatted
		code block.

#####Horizontal Rules
Three or more dashes or asterisks:

	---
	* * *
	- - - -
example:

--- 
* * *
- - - -

#####Manual Line Breaks
End a line with two or more spaces:

	Roses are read,
	Violets are blue.

![lechance][le-avatar]
[le-avatar]: /images/avatar.png "Lechance"
