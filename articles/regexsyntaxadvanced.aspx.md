---
title: C# Regular Expression (Regex) Examples in .NET
layout: post
---

<h2>More Advanced Regular Expression Syntax</h2>

<p>
  This article continues from
  <a href="../regexlearnsyntax.aspx/"
    >Learn Regular Expression (Regex) syntax with C# and .NET</a
  >
  and covers character escapes, match grouping, some C# code examples, matching
  boundaries and RegexOptions.
</p>
<h2>Matching special characters with character escapes</h2>

<p>
  Special characters such as Tab and carriage return are matched using character
  escapes. The syntax is similar to C and C#. The common character escapes are
  listed below.
</p>
<table border="0" cellpadding="2" cellspacing="0" class="Content">
  <thead>
    <tr>
      <th>Special Character</th>
      <th>Description</th>
    </tr>
  </thead>
  <tr>
    <td valign="top" width="1">\t</td>
    <td>Matches a tab</td>
  </tr>
  <tr>
    <td valign="top">\r</td>
    <td>Matches a carriage return</td>
  </tr>
  <tr>
    <td valign="top">\n</td>
    <td>Matches a new line</td>
  </tr>
  <tr>
    <td valign="top">\u0020</td>
    <td>
      Matches a Unicode character <br />
      using hexadecimal representation. <br />
      Exactly four digits must be specified.
    </td>
  </tr>
</table>
<p>
  <br />
  In this example, the Regular Expression pattern matches one or more word
  characters followed by a carriage return then a new line.
</p>
<pre class="Code">Text:    an anaconda <b>ate</b><br/>Anna Jones
Regex:   \w+\r\n
Match:
ate</pre>
<p>
  Depending on your operating system you might have to combine the
  <code>\r</code> and <code>\n</code> character escapes to create the correct
  new line sequence for your platform. For Microsoft Windows systems you should
  generally use <code> \r\n</code> which is a carriage return then line feed
  (CRLF). To simply match the end of a line or string use the dollar sign
  (<code>$</code>).
</p>
<h2>Match Grouping</h2>

<p>
  Groups perform a few different functions. They allow the quantifiers (such as
  plus and star) to be applied to sections of the match instead of just
  individual characters.
</p>
<p>
  A group is specified by the round brackets <code>(</code> and <code>)</code>.
  If you want to match the round bracket characters you must use the escape
  character before the bracket e.g. <code>\(</code> or <code>\)</code>.
</p>
<p>
  This regex matches &#39;<code>http://</code>&#39; optionally followed by
  &#39;<code>www.</code>&#39; then starts a group and matches one or more of any
  character that is not a full stop/period (<code>.</code>) closes the group
  then matches &#39;<code>.com</code>&#39;.
</p>
<pre>
Text:    http://www.yahoo.com/index.html and http://yahoo.com
Regex:   http://(www\.)?([^\.]+)\.com 
Matches: 
http://www.yahoo.com
http://yahoo.com</pre
>
<p>
  The question mark after the group <code>(www\.)</code> applies to the whole
  group making it optional.
</p>
<h2>An example in C#</h2>

<p>
  The regular expression classes are in the
  <code>System.Text.RegularExpressions</code> namespace.
</p>
<pre>using System.Text.RegularExpressions;</pre>
<p>
  The <code>Regex</code> class represents a regular expression. A regular
  expression pattern must be specified when creating a
  <code>Regex</code> object. The pattern cannot be changed.
</p>
<pre>
Regex exp = new Regex(
    @&quot;http://(www\.)?([^\.]+)\.com&quot;,
    RegexOptions.IgnoreCase);

string InputText = &quot;http://www.yahoo.com/&quot;;</pre
>
<p>
  The <code>MatchCollection</code> class stores a list of successful matches
  found by applying the regular expression pattern to an input string.
</p>
<pre>
MatchCollection MatchList = exp.Matches(InputText);
Match FirstMatch = MatchList[0];

Console.WriteLine(FirstMatch.Value);</pre
>
<p>
  The <code>Group</code> class represents a group within the regex pattern. Each
  <code> Match</code> object has a <code>Groups</code> collection.
</p>
<pre>
Group GroupCurrent;
for (int i = 1; i &lt; FirstMatch.Groups.Count; i++)
{
    GroupCurrent = FirstMatch.Groups[i];</pre
>
<p>
  The <code>Success</code> property on the group can be used to check if the
  <code> Group</code> matched or not.
</p>
<pre>
if (GroupCurrent.Success)
{
    {
        Console.WriteLine(&quot;\tMatched:&quot; + GroupCurrent.Value);
    }
    else
    {
        Console.WriteLine(&quot;\tGroup didn&#39;t match&quot;);
    }
}</pre
>
<p>
  Groups within a Match can be referenced by number or by name (see below).
</p>
<pre>
if (MatchList.Count &gt; 0)
{
    if (MatchList[1].Success)
    {
        Console.WriteLine(&quot;Group 1 matched&quot;);
    }
}</pre
>
<p>
  Matches also allow sections of the match to be used in replacement expressions
  when using <code>Regex.Replace()</code>.
</p>
<p></p>
<h2>Named Groups</h2>

<p>
  Groups can be named to allow easier identification with the following syntax.
</p>
<p>
  <code>(?&lt;NameOfGroup&gt;expression)</code>
</p>
<h2>Matching boundaries between words</h2>

<p>
  To match a boundary between a word character (<code>\w</code>) and a non-word
  character (<code>\W</code>) use <code>\b</code>. The match will occur at the
  first or last character in words separated by any nonalphanumeric characters.
  For example, the following Regular Expression matches one or more word
  characters followed by a word boundary followed by a hyphen (-) followed by
  another word boundary followed by one or more word characters.
</p>
<pre
  class="Code"
>Text:    Anna Jones and John William-Scott went to lunch- with an anaconda
Regex:   \w+\b-\b\w+<br/>Options: IgnoreCase
Matches: Anna Jones and John <strong>William-Scott</strong> went to lunch- with an anaconda
William-Scott</pre>
<p>
  Use <code>\B</code> to specify that a match must not occur on a
  <code>\b</code> boundary.
</p>
<p></p>
<h2>Regular Expression Options</h2>
<p>
  Regular Expression Options can be used in the constructor for the
  <code>Regex</code> class.
</p>
<p><b>RegexOptions.None</b> - Specifies that no options are set.</p>
<p><b>RegexOptions.IgnoreCase</b> - Specifies case-insensitive matching.</p>
<p>
  <b>RegexOptions.Multiline</b> - Multiline mode. Changes the meaning of ^ and $
  so they match at the beginning and end, respectively, of any line, and not
  just the beginning and end of the entire string.
</p>
<p>
  <b>RegexOptions.Singleline</b> - Specifies single-line mode. Changes the
  meaning of the dot (<code>.</code>) so it matches every character (instead of
  every character except <code>\n</code>).
</p>
<p>
  <b>RegexOptions.ExplicitCapture</b> - Specifies that the only valid captures
  are&nbsp; groups that are explicitly named or in the form
  <code>(?&lt;name&gt;...)</code>.
</p>
<p>
  <b>RegexOptions.IgnorePatternWhitespace</b> - Eliminates unescaped white space
  from the pattern and enables comments marked with the hash sign
  (<code>#</code>).
</p>
<p>
  <b>RegexOptions.Compiled</b> - Specifies that the regular expression is
  compiled to an assembly. The regular expression will be faster to match but it
  takes more time to compile initially. This option (although tempting) should
  only be used when the expression will be used many times. e.g. in a
  <code>foreach</code> loop
</p>
<p>
  <b>RegexOptions.ECMAScript</b> - Enables ECMAScript-compliant behavior for the
  expression. This flag can be used only in conjunction with the IgnoreCase,
  Multiline, and Compiled flags. The use of this flag with any other flags
  results in an exception.
</p>
<p>
  <b>RegexOptions.RightToLeft</b> - Specifies that the search will be from right
  to left instead of from left to right.
</p>
