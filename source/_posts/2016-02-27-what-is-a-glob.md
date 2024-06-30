---
title: What is a Glob [Java]
date: 2016-02-27 11:20:34
tags:
 - java
 - tutorial
categories:
 - Java

----

_from [Glob](https://docs.oracle.com/javase/tutorial/essential/io/fileOps..html#atomic)_

Two methods in the `Files` class accept a __glob__ argument, but what is __glob__?

You can use _glob_ syntax to specify pattern-matching behavior.

A _glob_ pattern is specified as a string and is matched against other strings,
such as directory or file names. Glob syntax follows several simple rules:

* An asterisk, *, matches any number of characters (including none).
* Two asterisks, `**`, works like `*` but crosses directory boundaries.This syntax is generally used for matching complete paths.
* A question mark, ?, matches exactly one character.
* Braces specify a collection of subpatterns.

    - For example:

        {sun,moon,stars} matches "sun", "moon", or "stars".

        {temp*,tmp*} matches all strings beginning with "temp" or "tmp".

* Square brackets convey a set of single characters or, when the hyphen character (`-`) is used, a range of characters.
    - For example:

        [aeiou] matches any lowercase vowel.

        [0-9] matches any digit.

        [A-Z] matches any uppercase letter.

        [a-z,A-Z] matches any uppercase or lowercase letter.

    >Within the square brackets, *, ?, and \ match themselves.

* All other characters match themselves.

    To match *, ?, or the other special characters, you can escape them by using the backslash character, `\`.
    For example: \\ matches a single backslash, and \? matches the question mark.
