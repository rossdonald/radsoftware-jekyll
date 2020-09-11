---
title: Create a Windows Clipboard Monitor in C# using SetClipboardViewer API
layout: post
---

<h2>Introduction</h2>
<p>
  Applications such as
   GetRight and
   ClipMate seem to
  magically know when something is copied to the Clipboard. How is it done? This
  example shows you how to use the Win32 API function
  <b><code>SetClipboardViewer</code></b>
  to create a Clipboard Viewer application.
</p>
<h2>Using SetClipboardViewer</h2>
<p>
  The Win32 API contains a function <code>SetClipboardViewer</code
  ><b> </b> which allows an application to register itself as a viewer of
  Clipboard events. When an item is copied to the Clipboard, windows notifies
  the Clipboard Viewer by sending the <b> WM_DRAWCLIPBOARD</b> message. The
  application can then use the appropriate function to get the contents of the
  Clipboard and perform processing.
</p>
<h2>Example Application</h2>
<p>
  I had a great idea one day (after copying and pasting internet links a few
  hundred times) to create a small application that would read the Clipboard
  when something was copied to it and extract any Internet URLs, UNC paths or
  filenames that it found and put them on a menu in the system tray.
</p>
<p>
  I spent a little time on it and it is in a working state but it is not
  finished. One day I will get around to finishing it off but for the moment it
  is a good sample to learn how <code> SetClipboardViewer</code> works.
</p>
<table
  class="Content"
  border="0"
  id="table1"
  cellspacing="6"
  style="border-collapse: collapse;"
>
  <tr>
    <td>
      <img src="/articles/images/download.gif" align="absmiddle" />
    </td>
    <td>
      C# Source Code:
      <a href="/articles/RAD_ClipboardMonitorExampleSrc.zip">
        RAD_ClipboardMonitorExampleSrc.zip</a
      >
      (~50 KB)
    </td>
  </tr>
  <tr>
    <td>
      <img src="/articles/images/download.gif" align="absmiddle" />
    </td>
    <td>
      VB.NET Example:
      <a target="_blank" href="/articles/ClipboardMonitor_VB.txt">
        ClipboardMonitor_VB.txt</a
      >
    </td>
  </tr>
</table>
<h2>The SetClipboardViewer Function</h2>
<p>
  The <code>SetClipboardViewer</code> function adds the specified window to the
  chain of Clipboard Viewers. Clipboard Viewer windows receive a
  <code>WM_DRAWCLIPBOARD</code>
  message whenever the content of the Clipboard changes.
</p>
<p>
  The <code>SetClipboardViewer</code> API declaration has the following syntax
  in C#
</p>
<pre
  class="Code"
>[DllImport(<font COLOR="#800000">&quot;User32.dll&quot;</font>, CharSet=CharSet.Auto)]
<font COLOR="#0000ff">public</font> <font COLOR="#0000ff">static</font> <font COLOR="#0000ff">extern</font> IntPtr SetClipboardViewer(IntPtr hWndNewViewer);</pre>
<p>
  The <code>hWndNewViewer</code> parameter is the handle to the window to be
  added to the Clipboard chain.
</p>
<p>
  When <code>SetClipboardViewer</code> is successful, the return value
  identifies the next window in the Clipboard Viewer chain. This is important as
  a Clipboard Viewer is required to pass Clipboard messages to the next window
  in the Clipboard chain.
</p>
<h2>Setting up the Clipboard Viewer</h2>
<p>
  I need to override the <code><strong>WndProc</strong></code> method of the
  underlying Form so that the main form can respond to window messages from the
  operating system. Normally .NET applications do not need to use
  <code>WndProc</code>
  as the .NET Framework takes care of these events.
</p>
<pre><font COLOR="#0000ff">protected</font> <font COLOR="#0000ff">override</font> <font COLOR="#0000ff">void</font> WndProc(<font COLOR="#0000ff">ref</font> Message m)
{
</pre>
<p>
  Next, call <b><code>SetClipboardViewer</code> </b>to register the application
  as a Clipboard Viewer. The window will now receive the
  <code>WM_DRAWCLIPBOARD</code> message. Applications do not normally receive
  the <code> WM_DRAWCLIPBOARD</code> message, only those that call
  <code>SetClipboardViewer</code>.
</p>
<pre
  class="Code"
>_ClipboardViewerNext = SetClipboardViewer(<font COLOR="#0000ff">this</font>.Handle);</pre>
<h2>Responding to the WM_DRAWCLIPBOARD message</h2>
<p>
  After calling SetWindowLong the <b>WindowProc</b> method will receive the
  <code>WM_DRAWCLIPBOARD</code>
  message when something is copied to the Clipboard.
</p>
<pre
  class="Code"
><font COLOR="#0000ff">protected</font> <font COLOR="#0000ff">override</font> <font COLOR="#0000ff">void</font> WndProc(<font COLOR="#0000ff">ref</font> Message m)
{
    <font COLOR="#0000ff">switch</font> ((Win32.Msgs)m.Msg)
    {
        <font COLOR="#0000ff">case</font> Win32.Msgs.WM_DRAWCLIPBOARD:</pre>
<p>
  This is the place that I do my processing of the Clipboard data. The .NET
  Framework has the <b>Clipboard</b>, <b>DataObject</b> and
  <b>DataFormats </b>classes for working with the Clipboard.
  <b> Clipboard.GetDataObject </b>gets the current contents of the Clipboard as
  an <b>IDataObject</b>.
</p>
<pre
  class="Code"
>IDataObject iData = <font COLOR="#0000ff">new</font> DataObject();

iData = Clipboard.GetDataObject();</pre>

<p>
  As the Clipboard can contain different types of data, use the
  <b> DataObject.GetDataPresent</b> method to check for a certain type of data.
  It returns true if the contents of the Clipboard can be converted to the
  specified type. The <b>DataFormats</b> class contains constants that define
  the names of some of the more common Clipboard formats.
</p>
<pre
  class="Code"
><font COLOR="#0000ff">if</font> (iData.GetDataPresent(DataFormats.Rtf))
{
    // ...</pre>
<p>
  Once I have determined that the format is available then the
  <b> DataObject.GetData</b>
  method can be used to get the Clipboard content in the required format. The
  following example gets the contents in Rich Text Format (RTF).
</p>
<pre
  class="Code"
>ctlClipboardText.Rtf = (<font COLOR="#0000ff">string</font>)iData.GetData(DataFormats.Rtf);</pre>
<p>
  After performing any processing that the application requires the next window
  in the Clipboard Viewer chain must be notified of the Clipboard change. This
  is done by calling the Windows API function <b><code>SendMessage</code></b
  >. The same parameters that were passed to the <code>WndProc</code> method are
  passed to <code>SendMessage</code> to be used by the next Clipboard Viewer in
  the chain.
</p>
<pre>SendMessage(_ClipboardViewerNext, m.Msg, m.WParam, m.LParam);</pre>
<p>
  When overriding <code>WndProc</code> I must pass any unhandled messages to the
  base class by calling <code>base.WndProc</code>
</p>
<pre>protected <font COLOR="#0000ff">override</font> <font COLOR="#0000ff">void</font> WndProc(<font COLOR="#0000ff">ref</font> Message m)
{
    <font COLOR="#0000ff">switch</font> ((Win32.Msgs)m.Msg)
    {
        <font COLOR="#0000ff">case</font> Win32.Msgs.WM_DRAWCLIPBOARD:
            <font color="#008000">// ... process Clipboard</font>

        <font COLOR="#0000ff">default</font>:

<font color="#008000"> </font><font color="#008000">// unhandled window message</font>
<font COLOR="#0000ff">base</font>.WndProc(<font COLOR="#0000ff">ref</font> m);
<font COLOR="#0000ff">break</font>;</pre>

<h2>The WM_CHANGECBCHAIN Message</h2>
<p>
  The <code>WM_CHANGECBCHAIN</code> message is sent when a window is being
  removed from the chain of Clipboard Viewers. Note that the message is only
  sent to the first window in the Clipboard Viewer chain. The first window and
  subsequent windows in the chain must use <code>SendMessage</code> to pass the
  message along the chain.
</p>
<p>
  If <code>wParam</code> is equal to the handle of the next Clipboard Viewer
  then the next Clipboard Viewer is being removed and I need to update the
  pointer that refers to the next window in the chain. <code>m.LParam</code> is
  a pointer to the Clipboard Viewer in the chain after the one being removed.
</p>
<pre><font COLOR="#0000ff">if</font> (m.WParam == _ClipboardViewerNext)
{
    _ClipboardViewerNext = m.LParam;
}<font COLOR="#0000ff">
else</font>
{
    SendMessage(_ClipboardViewerNext, m.Msg, m.WParam, m.LParam);
}</pre>
<p>
  This message can be seen by starting multiple copies of the Clipboard Viewer
  and watching the Debug output.
</p>
<h2>Unregistering the Application as a Clipboard Viewer</h2>
<p>
  When the application closes I make sure I call <b> ChangeClipboardChain</b> to
  remove the application from the Clipboard chain.
</p>
<pre
  class="Code"
>ChangeClipboardChain(<font COLOR="#0000ff">this</font>.Handle, _ClipboardViewerNext);</pre>
<h2>And Finally...</h2>
<p>
  There are a few tricks to using the <code>SetClipboardViewer</code>
  function but it allows your application to be notified of Clipboard events.
</p>
<p></p>
