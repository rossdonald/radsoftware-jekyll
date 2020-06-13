---
title: Learn Regular Expression (Regex) syntax with C# and .NET
layout: post
---

<h2>What are Regular Expressions?</h2>
<p>
  Regular Expressions are a powerful pattern matching language that is part of
  many modern programming languages. Regular Expressions allow you to apply a
  <b> pattern</b>
  to an input string and return a list of the matches within the text. Regular
  expressions also allow text to be replaced using replacement patterns. It is a
  very powerful version of find and replace.
</p>
<p>
  There are two parts to learning Regular Expressions;
</p>
<ul>
  <li>
    learning the Regex syntax
  </li>
  <li>
    learning how to work with Regex in your programming language
  </li>
</ul>
<p>
  This article introduces you to the Regular Expression syntax. After learning
  the syntax for Regular Expressions you can use it many different languages as
  the syntax is fairly similar between languages.
</p>
<p>
  Microsoft&apos;s .NET Framework contains a set of classes for working with
  Regular Expressions in the <b>System.Text.RegularExpressions</b> namespace.
</p>
<h2>Download the Regular Expression Designer</h2>
<p>
  When learning Regular Expressions, it helps to have a tool that you can use to
  test Regex patterns. Rad Software has a
  <strong><a href="../../regexdesigner/"> Free Regular Expression Tool</a></strong>
  available for download that will help as you go through the article.
</p>
<h2>The basics - Finding text</h2>
<p>
  Regular Expressions are similar to find and replace in that ordinary
  characters match themselves. If I want to match the word &quot;went&quot; the
  Regular Expression pattern would be &quot;went&quot;.
</p>
<pre class="Code">
Text:    Anna Jones and a friend went to lunch
Regex:   went
Matches: Anna Jones and a friend <b>went</b> to lunch
went</pre
>
<p>
  The following are special characters when working with Regular Expressions.
  They will be discussed throughout the article.
</p>
<pre class="Code">. $ ^ { [ ( | ) * + ? \</pre>
<h2>Matching any character with dot</h2>
<p>
  The full stop or period character (<code>.</code>) is known as dot. It is a
  wildcard that will match any character except a new line (<code>\n</code>).
  For example if I wanted to match the &apos;a&apos; character followed by any
  two characters.
</p>
<pre class="Code">
Text:    abc def ant cow
Regex:   a..
Matches: <b>abc</b> def <b>ant</b> cow
abc
ant</pre
>
<p>
  If the <b>Singleline </b>option is enabled, a dot matches any character
  including the new line character.
</p>
<h2>Matching word characters</h2>
<p>
  Backslash and a lowercase &apos;w&apos; (<code>\w</code>) is a character class
  that will match any word character. The following Regular Expression matches
  &apos;a&apos; followed by two word characters.
</p>
<pre class="Code">
Text:    abc anaconda ant cow apple
Regex:   a\w\w
Matches: <b>abc</b> <b>ana</b>conda <b>ant</b> cow <b>app</b>le
abc
ana
ant
app</pre
>
<p>
  Backslash and an uppercase &apos;W&apos; (<code>\W</code>) will match any
  non-word character.
</p>
<h2>Matching white-space</h2>
<p>
  White-space can be matched using <code>\s</code> (backslash and
  &apos;s&apos;).<b> </b>The following Regular Expression matches the letter
  &apos;a&apos; followed by two word characters then a white space character.
</p>
<pre class="Code">
Text:    &quot;abc anaconda ant&quot;
Regex:   a\w\w\s
Matches: 
&quot;abc &quot;</pre
>
<p>
  Note that <b>ant</b> was not matched as it is not followed by a white space
  character.
</p>
<p>
  White-space is defined as the space character, new line (<code>\n</code>),
  form feed (<code>\f</code>), carriage return (<code>\r</code>), tab
  (<code>\t</code>) and vertical tab (<code>\v</code>). Be careful using \s as
  it can lead to unexpected behaviour by matching line breaks (<code>\n</code>
  and <code>\r</code>). Sometimes it is better to explicitly specify the
  characters to match instead of using \s. e.g. to match Tab and Space use
  <code>[\t\0x0020]</code>
</p>
<h2>Matching digits</h2>
<p>
  The digits zero to nine can be matched using <code>\d</code> (backslash and
  lowercase &apos;d&apos;). For example, the following Regular Expression
  matches any three digits in a row.
</p>
<pre class="Code">
Text:    123 12 843 8472
Regex:   \d\d\d
Matches: <b>123</b> 12 <b>843</b> <b>847</b>2
123
843
847</pre
>
<h2>Matching sets of single characters</h2>
<p>
  The square brackets are used to specify a set of single characters to match.
  Any single character within the set will match. For example, the following
  Regular Expression matches any three characters where the first character is
  either &apos;d&apos; or &apos;a&apos;.
</p>
<pre class="Code">
Text:    abc def ant cow
Regex:   [da]..
Matches: <b>abc</b> <b>def</b> <b>ant</b> cow
abc
def
ant</pre
>
<p>
  <span style="font-weight: 400;"
    >The caret (<code>^</code>) can be added to the start of the set of
    characters to specify that none of the characters in the character set
    should be matched. The following Regular Expression matches any three
    character where the first character is not &apos;d&apos; and not
    &apos;a&apos;.</span
  >
</p>
<pre class="Code">
Text:    abc def ant cow
Regex:   [^da]..
Matches: 
&quot;bc &quot;
&quot;ef &quot;
&quot;nt &quot;
&quot;cow&quot;</pre
>
<h2>Matching ranges of characters</h2>
<p>
  Ranges of characters can be matched using the hyphen (<code>-</code>). the
  following Regular Expression matches any three characters where the second
  character is either &apos;a&apos;, &apos;b&apos;, &apos;c&apos; or
  &apos;d&apos;.
</p>
<pre class="Code">
Text:    abc pen nda uml
Regex:   .[a-d].
Matches: <STRONG>abc</STRONG> pen <STRONG>nda</STRONG> uml
abc
nda</pre
>
<p>
  Ranges of characters can also be combined together. the following Regular
  Expression matches any of the characters from &apos;a&apos; to &apos;z&apos;
  or any digit from &apos;0&apos; to &apos;9&apos; followed by two word
  characters.
</p>
<pre class="Code">
Text:    abc no 0aa i8i
Regex:   [a-z0-9]\w\w
Matches: <b>abc</b> no <b>0aa</b> <b>i8i</b>
abc
0aa
i8i</pre
>
<p>The pattern could be written more simply as <code>[a-z\d]</code></p>
<h2>Specifying the number of times to match with Quantifiers</h2>
<p>
  Quantifiers let you specify the number of times that an expression must match.
  The most frequently used quantifiers are the asterisk character
  (<code>*</code>) and the plus sign (<code>+</code>). Note that the asterisk
  (<code>*</code>) is usually called the star when talking about Regular
  Expressions.
</p>
<h2>Matching zero or more times with star (*)</h2>
<p>
  The star tells the Regular Expression to match the character, group, or
  character class that immediately precedes it <b>zero or more times</b>. This
  means that the character, group, or character class is optional, it can be
  matched but it does not have to match. The following Regular Expression
  matches the character &apos;a&apos; followed by zero or more word characters.
</p>
<pre class="Code">
Text:    Anna Jones and a friend owned an anaconda
Regex:   a\w*
Options: IgnoreCase
Matches: <b>Anna</b> Jones <b>and</b> <b>a</b> friend owned <b>an</b> <b>anaconda</b>
Anna
and
a
an
anaconda</pre
>
<h2>Matching one or more times with plus (+)</h2>
<p>
  The plus sign tells the Regular Expression to match the character, group, or
  character class that immediately precedes it <b>one or more times</b>. This
  means that the character, group, or character class must be found at least
  once. After it is found once it will be matched again if it follows the first
  match. The following Regular Expression matches the character &apos;a&apos;
  followed by at least one word character.
</p>
<pre class="Code">
Text:    Anna Jones and a friend owned an anaconda
Regex:   a\w+
Options: IgnoreCase
Matches: <b>Anna</b> Jones <b>and</b> a friend owned <b>an</b> <b>anaconda</b>
Anna
and
an
anaconda</pre
>
<p>
  Note that &quot;a&quot; was not matched as it is not followed by any word
  characters.
</p>
<h2>Matching zero or one times with question mark (?)</h2>
<p>
  To specify an optional match use the question mark (<code>?</code>). The
  question mark matches <b>zero or one </b>times. The following Regular
  Expression matches the character &apos;a&apos; followed by &apos;n&apos; then
  optionally followed by another &apos;n&apos;.
</p>
<pre class="Code">
Text:    Anna Jones and a friend owned an anaconda
Regex:   an?
Options: IgnoreCase
Matches: <b>An</b>n<b>a</b> Jones <b>an</b>d <b>a</b> friend owned <b>an</b> <b>ana</b>cond<b>a</b>
An
a
an
a
an
an
a
a</pre
>
<h2>Specifying the number of matches</h2>
<p>
  The minimum number of matches required for a character, group, or character
  class can be specified with the curly brackets (<code>{<em>n</em>}</code>).
  The following Regular Expression matches the character &apos;a&apos; followed
  by a minimum of two &apos;n&apos; characters. There must be two &apos;n&apos;
  characters for a match to occur.
</p>
<pre class="Code">
Text:    Anna Jones and Anne owned an anaconda
Regex:   an{2}
Options: IgnoreCase
Matches: <b>Ann</b>a Jones and <b>Ann</b>e owned an anaconda
Ann
Ann</pre
>
<p>
  A range of matches can be specified by curly brackets with two numbers inside
  (<code>{<em>n</em>,<em>m</em>}</code>). The first number (n) is the minimum
  number of matches required, the second (m) is the maximum number of matches
  permitted. This Regular Expression matches the character &apos;a&apos;
  followed by a minimum of two &apos;n&apos; characters and a maximum of three
  &apos;n&apos; characters.
</p>
<pre class="Code">
Text:    Anna and Anne lunched with an anaconda annnnnex
Regex:   an{2,3}
Options: IgnoreCase
Matches: <b>Ann</b>a and <b>Ann</b>e lunched with an anaconda <b>annn</b>nnex
Ann
Ann
annn</pre
>
<p>
  The Regex stops matching after the maximum number of matches has been found.
</p>
<h2>Matching the start and end of a string</h2>
<p>
  To specify that a match must occur at the beginning of a string use the caret
  character (<code>^</code>). For example, I want a Regular Expression pattern
  to match the beginning of the string followed by the character &apos;a&apos;.
</p>
<pre class="Code">
Text:    an anaconda ate Anna Jones
Regex:   ^a
Matches: <b>a</b>n anaconda ate Anna Jones
&quot;a&quot; at position 1</pre
>
<p>
  The pattern above only matches the a in &quot;an&quot;.
</p>
<p>
  Note that the caret (<code>^</code>) has different behaviour when used inside
  the square brackets.
</p>
<p>
  If the <b>Multiline</b> option is on, the caret (<code>^</code>) will match
  the beginning of each line in a multiline string rather than only the start of
  the string.
</p>
<p>
  To specify that a match must occur at the end of a string use the dollar
  character (<code>$</code>). If the Multiline option is on then the pattern
  will match at the end of each line in a multiline string. This Regular
  Expression pattern matches the word at the end of the line in a multiline
  string.
</p>
<pre class="Code">
Text:    &quot;an anaconda
ate Anna
Jones&quot;
Regex:   \w+$
Options: Multiline, IgnoreCase
Matches: 
Jones
</pre>
<p>
  Microsoft have an online reference for Regex in .NET:
  <a
    href="https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference"
    target="_blank"
  >
    Regular Expression Syntax</a
  >
</p>

<p>
  To learn more about Regular Expression syntax see the next article:
  <a href="../regexsyntaxadvanced.aspx/">
    C# Regular Expression (Regex) Examples in .NET</a
  >
</p>
