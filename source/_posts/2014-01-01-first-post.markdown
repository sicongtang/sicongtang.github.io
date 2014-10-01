---
layout: post
title: "First Post"
date: 2014-01-01 11:32:03 +0800
comments: true
categories: test
---
# MarkDown Template

## Blockquotes

> ## Header inside blockquote
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
> >  This is nested blockquote.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");

## List

Markdown supports ordered (numbered) and unordered (bulleted) lists.
Unordered lists use asterisks, pluses, and hyphens — interchangably — as list markers:

* Red
+ Green
- Blue

1.  Bird
2.  McHale
8.  Parish

To put a code block within a list item, the code block needs to be indented twice — 8 spaces or two tabs:

* A list item with a code block:
		<codes goes here with two leading tabs>

## Code Blocks
To produce a code block in Markdown, simply indent every line of the block by at least 4 spaces or 1 tab. For example, given this input:

## Horizontal Rules
* * *

***

*****

- - -

---------------------------------------

## Links
### inline links
This is [an example](http://example.com/ "Title") inline link.
See my [About](/about/) page for details.   
### reference links
This is [an example][id] reference-style link.
[id]: http://example.com/  "Optional Title Here"
### implicit link
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

[google]: http://google.com/        "Google"
[yahoo]:  http://search.yahoo.com/  "Yahoo Search"
[msn]:    http://search.msn.com/    "MSN Search"

## Emphasis
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

un*frigging*believable

\*this text is surrounded by literal asterisks\*

## Code
Use the `printf()` function.
``There is a literal backtick (`) here.``
Please don't use any `<blink>` tags.

## Images
![Alt text](/path/to/img.jpg "Optional title")

![Alt text][id]
[id]: ../images/email.png  "Optional title attribute"

## Miscellaneous
### Automatic link
<http://daringfireball.net/projects/markdown/syntax#em>
<address@example.com>
### Blackslash escapes
	\   backslash
	`   backtick
	*   asterisk
	_   underscore
	{}  curly braces
	[]  square brackets
	()  parentheses
	#   hash mark
	+   plus sign
	-   minus sign (hyphen)
	.   dot
	!   exclamation mark


###### This is an H6