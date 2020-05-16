---
title: Using the CommandBuilder to generate SQL Statements
layout: post
---

<h2>Introduction</h2>
<p>
  The <strong>CommandBuilder</strong> class is a part of the .NET Framework. Its
  purpose is to automatically build SQL INSERT, UPDATE,&nbsp;and DELETE
  statements for a DataTable based on a SQL SELECT statement. It helps the
  developer write cleaner and more readable code by the avoiding code with
  embedded SQL statements.
</p>
<p>
  There are different CommandBuilder classes for the different native data
  access libraries. Use the <strong>OleDbCommandBuilder</strong> when connecting
  to Databases via OLEDB and the <strong>SqlDataAdapter</strong> for Microsoft
  SQL Server Databases.
</p>
<h2>Getting started with the CommandBuilder</h2>
<p>
  When creating a CommandBuilder a DataAdapter is passed as a parameter to the
  constructor. This links the CommandBuilder to a DataAdapter allowing it to
  read the SELECT command that has been associated with the DataAdapter. It will
  use the SELECT command to extract the schema information from the Table in the
  Database. Using this schema information it can automatically build INSERT,
  UPDATE and DELETE Command objects and the required SQL statements. e.g.
</p>
<pre
  class="code"
><span class="CodeKeyword">Dim</span> adpAccess <span class="CodeKeyword">As</span> New OleDbDataAdapter( _
	<span class="CodeLiteral">&quot;SELECT * FROM [schemaDatabases]&quot;</span>, _
	cnnDBAccessAuditorData) 
<span class="CodeKeyword">Dim</span> cmdbAccessCmdBuilder <span class="CodeKeyword">As</span> New OleDbCommandBuilder(adpAccess)</pre>
<p>
  Note that the <strong>QuotePrefix</strong> and
  <strong>QuoteSuffix</strong> must be set when updating Access or SQL Server
  Databases to avoid errors with columns that have the same name as reserved
  words or column names that contain spaces. E.g.
</p>
<pre
  class="code"
>cmdbAccessCmdBuilder.QuotePrefix = <span class="CodeLiteral">&quot;[&quot;</span>
cmdbAccessCmdBuilder.QuoteSuffix = <span class="CodeLiteral">&quot;]&quot;</span></pre>
<p>
  To save the Inserts, Updates and Deletes from the DataTable or DataSet to the
  Database, set the <strong>InsertCommand</strong>,
  <strong>UpdateCommand</strong> and <strong>DeleteCommand</strong> properties
  of the DataAdapter from the CommandBuilder. e.g.
</p>
<pre class="code">
adpAccess.InsertCommand = cmdbAccessCmdBuilder.GetInsertCommand()</pre
>
<p>
  I have only set the InsertCommand as I am only adding rows, if you are
  updating and deleting rows then UpdateCommand and DeleteCommand must be set
  also. E.g.
</p>
<pre class="code">
adpAccess.UpdateCommand = cmdbAccessCmdBuilder.GetUpdateCommand()
adpAccess.DeleteCommand = cmdbAccessCmdBuilder.GetDeleteCommand()</pre
>
<p>Then the Update method of the DataAdapter can be called. E.g.</p>
<pre class="code">adpAccess.Update(dtbschemaDatabases)</pre>
<p>
  For each row that was updated the UpdateCommand will be called, for the new
  rows InsertCommand will be called and for rows that have been deleted the
  DeleteCommand will be executed.
</p>
<h2>Full Code Listing</h2>
<pre
  class="Code"
><span class="CodeKeyword">Dim</span> cnnDBAccessAuditorData <span class="CodeKeyword">As</span> _
&nbsp;&nbsp;<span class="CodeKeyword">New</span> OleDbConnection(gstrCnnStrAccessAuditorData)
<span class="CodeKeyword">Dim</span> adpAccess <span class="CodeKeyword">As</span> New OleDbDataAdapter( _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;SELECT * FROM [schemaDatabases]&quot;</span>, _
&nbsp;&nbsp;cnnDBAccessAuditorData)
&nbsp;&nbsp;
adpAccess.MissingSchemaAction = MissingSchemaAction.AddWithKey
<span class="CodeKeyword">Dim</span> cmdbAccessCmdBuilder <span class="CodeKeyword">As</span> New OleDbCommandBuilder(adpAccess)
cmdbAccessCmdBuilder.QuotePrefix = <span class="CodeLiteral">&quot;[&quot;</span>
cmdbAccessCmdBuilder.QuoteSuffix = <span class="CodeLiteral">&quot;]&quot;</span>
<span class="CodeKeyword">Dim</span> dtbschemaDatabases <span class="CodeKeyword">As</span> New DataTable(&quot;schemaDatabases&quot;)
<span class="CodeKeyword">Dim</span> objFindKey(1) <span class="CodeKeyword">As</span> Object

<span class="CodeComment">' Get a list of all databases to add to schemaDatabases table
' generally it is not recommended to access the system tables directly</span>
cmdDBReportSelect.CommandText = _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;SELECT &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;SERVERPROPERTY('ServerName') AsDatabaseServer, &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;name As DatabaseName, &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;'SQLDMOCompLevel_'&quot;</span> _
  <span class="CodeLiteral">&quot; + CONVERT(VARCHAR(2), cmptlevel) AS Version, &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;databasepropertyex(name, 'Collation') AS Collation &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;FROM master..sysdatabases &quot;</span> &amp; _
&nbsp;&nbsp;<span class="CodeLiteral">&quot;ORDER BY name&quot;</span>

cmdDBReportSelect.Connection = cnnSQLServer

cnnSQLServer.Open()

drdDBInfo = cmdDBReportSelect.ExecuteReader

adpAccess.Fill(dtbschemaDatabases)

<span class="CodeComment">' loop through the list of databases</span> 
<span class="CodeComment">' and add any that are not in the list</span>
<span class="CodeKeyword">While</span> drdDBInfo.Read
&nbsp;&nbsp;<span class="CodeComment">' Check if the row exists</span>
&nbsp;&nbsp;objFindKey(0) = drdDBInfo(<span class="CodeLiteral">&quot;DatabaseServer&quot;</span>).ToString()
&nbsp;&nbsp;objFindKey(1) = drdDBInfo(<span class="CodeLiteral">&quot;DatabaseName&quot;</span>).ToString()
&nbsp;&nbsp;drwCurr = dtbschemaDatabases.Rows.Find(objFindKey)
&nbsp;&nbsp;<span class="CodeKeyword">If</span> drwCurr <span class="CodeKeyword">Is Nothing Then</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="CodeComment">' Row does not exist so add it in</span>
&nbsp;&nbsp;&nbsp;&nbsp;drwCurr = dtbschemaDatabases.NewRow()
&nbsp;&nbsp;&nbsp;&nbsp;<span class="CodeKeyword">With</span> drwCurr
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.BeginEdit()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Item(<span class="CodeLiteral">&quot;DatabaseServer&quot;</span>) = _
        drdDBInfo(<span class="CodeLiteral">&quot;DatabaseServer&quot;</span>).ToString()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Item(<span class="CodeLiteral">&quot;DatabaseName&quot;</span>) = drdDBInfo(&quot;DatabaseName&quot;).ToString()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Item(<span class="CodeLiteral">&quot;Version&quot;</span>) = drdDBInfo(<span class="CodeLiteral">&quot;Version&quot;</span>).ToString()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.Item(<span class="CodeLiteral">&quot;Collation&quot;</span>) = drdDBInfo(<span class="CodeLiteral">&quot;Collation&quot;</span>).ToString()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.EndEdit()
&nbsp;&nbsp;&nbsp;&nbsp;<span class="CodeKeyword">End With</span>
&nbsp;&nbsp;&nbsp;&nbsp;dtbschemaDatabases.Rows.Add(drwCurr)
&nbsp;&nbsp;<span class="CodeKeyword">End If</span>
<span class="CodeKeyword">End While</span>

adpAccess.InsertCommand = cmdbAccessCmdBuilder.GetInsertCommand()

adpAccess.Update(dtbschemaDatabases)

cnnDBAccessAuditorData.Close() 
</pre>
<h2>Conclusion</h2>
<p>
  The CommandBuilder class allows the developer to get away from worrying about
  the detail of SQL and concentrate on the other things. But there are a few
  things to think about before using the CommandBuilder.
</p>
<p>
  One of the tradeoffs is speed as the dynamically generated SQL from the
  CommandBuilder might be slower than using stored procedures for data access.
  The CommandBuilder also has to make an extra call to the database to retrieve
  the schema information for the Database Table.
</p>
<p>
  Another issue is that the programmer also does not have any control over the
  SQL that the CommandBuilder generates, so if you do not like the SQL there are
  not many options.
</p>
