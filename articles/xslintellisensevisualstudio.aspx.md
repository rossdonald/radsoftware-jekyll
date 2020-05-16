---
title: Get Intellisense when editing XSL/XSLT in Visual Studio .NET
layout: post
---

<h2>Background</h2>
<p>
  It has always frustrated me that there is no intellisense when&nbsp;&nbsp;
  working with XSL/XSLT&nbsp; documents in Visual Studio .NET 2003. I read an
  interesting&nbsp;&nbsp; weblog article&nbsp;that&nbsp;&nbsp; talks about
  creating schemas for custom file types. From this&nbsp;I realised&nbsp;I could
  make a schema definition for XSL/XSLT that would enable intellisense in Visual
  Studio .NET.
</p>
<h2>Download</h2>
<P>
  For those who don&#39;t want to write the schema themselves, the schema for
  XSL/XSLT is provided as a download.
</P>
<P>
  <a href="XslSchema.zip">
    <img src="../images/download.gif" align="absmiddle" />XslSchema.zip</a
  >
  (~10 KB)
</P>
<h2>How to install</h2>
<ol>
  <li>Download the <STRONG>XslSchema.zip</STRONG> file from the link above</li>
  <li class="Code">
    For <STRONG>Visual Studio .NET 2002</STRONG> unzip the contents of the zip
    file into<br />
    <pre>
C:\Program Files\Microsoft Visual Studio .NET\Common7\Packages\schemas\xml</pre
    >
    or the appropriate location if you have an non-default installation<br />
  </li>
  <li>
    For <STRONG>Visual Studio .NET 2003</STRONG> unzip the contents of the zip
    file into<br />
    <pre>
C:\Program Files\Microsoft Visual Studio .NET 2003\Common7\Packages\schemas\xml</pre
    >
    or the appropriate location if you have an non-default installation<br />
  </li>
  <li>
    Create a new XSL/XSLT document&nbsp;in Visual Studio .NET
  </li>
</ol>

<h2>What does it look like?</h2>
<p>
  <IMG src="../images/XslSchema_Shot1.gif" /><br />
  Screenshot 1
</p>
<P
  ><IMG src="../images/XslSchema_Shot2.gif" /><br />
  Screenshot 2
</P>

<h2>
  More Information
</h2>
<p>
  This topic is covered in more depth in the MSDN article
  <strong> &quot;Visual Studio&nbsp;.NET Schema Annotations&quot;</strong>
  (search for &#39;requireattributequotes&#39; in MSDN)
</p>
