---
title: Get Intellisense for Web.config and App.config in Visual Studio .NET
layout: post
---

<h2>Overview</h2>

<p>
  <code>Web.config</code> and <code>App.config</code> have can have many
  different configuration options but it is difficult to remember what some of
  the less commonly used options are. This article provides a schema definition
  for .NET configuration files such as <code>Web.config</code> and
  <code>App.config</code>. The schema makes Visual Studio .NET help you out by
  displaying intellisense when working in these files.
</p>
<h2>Extend Visual Studio .NET with XML Schema</h2>

<p>
  Visual Studio .NET has an extensible model that provides intellisense in XML
  and HTML-based documents based on XML schema. To get intellisense for
  <code>Web.config</code> or <code>App.config</code> I created a schema for the
  .NET configuration files and and put the schema into a folder under the Visual
  Studio .NET installation.
</p>
<p>
  Visual Studio .NET then reads the schema file and lists the schema in the
  <code>targetNamespace </code>drop-down list when editing the properties of the
  document. Selecting the schema from the list adds the
  <code> xmlns</code> attribute to the root element of the document.
</p>
<div class="Warning">
  <h2>Warning!</h2>

  <p>
    There is a problem with this method that you need to be aware of. To get
    intellisense in Visual Studio .NET, the <code>xmlns</code> attribute is
    added to the root element of the document so that Visual Studio .NET knows
    which schema to use for the document.
  </p>
  <p>
    ASP.NET and Winforms applications will throw a runtime error if the
    <code> &lt;configuration&gt;</code> element in <code>web.config</code> or
    <code>app.config</code> has the <code>xmlns</code> attribute when you run
    your application. e.g. This will give an error.
  </p>
  <p>
    <code>&lt;configuration xmlns=&quot;SchemaForWebConfig&quot;&gt;</code>
  </p>
  <p>
    <b
      >You must remember to remove the <code>xmlns</code> attribute when you are
      finished using intellisense in your config file.</b
    >
    If you do not, you will get an error in your application!
  </p>
</div>
<h2>Download</h2>

<p>
  <a href="../CLRconfigSchema.zip">
    <img
      src="../images/download.gif"
      align="absmiddle"
      alt="download"
    />CLRconfigSchema.zip</a
  >
  (~10 KB)
</p>
<h2>How to install</h2>

<ol>
  <li>
    Download the <b>CLRconfigSchema.zip</b> file from the link above <br />
    &nbsp;
  </li>
  <li class="Code">
    For <strong>Visual Studio .NET 2002</strong> unzip the contents of the zip
    file into<br />
    <pre>
C:\Program Files\Microsoft Visual Studio .NET\Common7\Packages\schemas\xml</pre
    >
  </li>
  <li>
    For <strong>Visual Studio .NET 2003</strong> unzip the contents of the zip
    file into<br />
    <pre>
C:\Program Files\Microsoft Visual Studio .NET 2003\Common7\Packages\schemas\xml</pre
    >
  </li>
  <li>
    Open <b>Web.config</b> or <b>App.config</b> in Visual Studio .NET<br />
    &nbsp;
  </li>
  <li>
    View the properties for the document<br />
    &nbsp;
  </li>
  <li>
    Select <b>.NET Configuration File</b> from the list for the
    <b> targetSchema</b> property<br />
    <br />
    <img
      src="../images/app-config-intellisense.gif"
      width="457"
      height="235"
    /><br />
    &nbsp;
  </li>
  <li>
    Edit <b>Web.config</b> or <b>App.config<br /> </b><br />
    <img
      src="../images/web-config-intellisense.gif"
      width="385"
      height="139"
    /><br />
    &nbsp;
  </li>
  <li>
    Remember to remove the <code>xmlns</code> attribute from the
    <code> &lt;configuration&gt;</code> element when you have finished editing
    your <code> Web.config</code> file!<br />
    <br />
    <img
      src="../images/visual-studio-intellisense.gif"
      width="682"
      height="199"
    />
  </li>
</ol>
<h2>
  More Information on <strong>Visual Studio .NET Schema Annotations</strong>
</h2>
<p>
  This topic is covered in more depth in the MSDN article
  <strong>&quot;Visual Studio .NET Schema Annotations&quot;</strong> (search for
  &#39;requireattributequotes&#39; in MSDN)
</p>
