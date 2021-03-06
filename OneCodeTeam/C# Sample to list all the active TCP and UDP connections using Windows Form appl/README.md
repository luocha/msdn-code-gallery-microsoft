# C# Sample to list all the active TCP and UDP connections using Windows Form appl
## Requires
- Visual Studio 2010
## License
- MIT
## Technologies
- Networking
## Topics
- Networking
## Updated
- 01/19/2016
## Description

<h1>Introduction</h1>
<p><em><span>The C# sample code developed in .NET Framework 4.0 would list the active TCP and UDP connections in a DataGridView. It shows information that describes an IPv4 TCP connection with IPv4 addresses, ports used by the TCP connection and the specific
 process ID PID) associated with connection along with the process name.</span></em></p>
<h1><span>Building the Sample</span></h1>
<p><em><span>Step 1: Open the &quot;SocketConnection.sln&quot; file using VS 2010 or above.&nbsp;</span><br>
<span>Step 2: Build the code by pressing &quot;Ctrl&#43; Shift&#43; B&quot; key combination.&nbsp;</span><br>
<span>Step 3: Execute the code either by clicking the F5 button or Ctrl &#43; F5.</span></em></p>
<h2>Using the Code</h2>
<p>Below is the code snippet to retrieve all the active TCP and UDP connections.</p>
<p>GetAllTcpConnections() function</p>
<p></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">编辑脚本</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;This&nbsp;function&nbsp;reads&nbsp;and&nbsp;parses&nbsp;the&nbsp;active&nbsp;TCP&nbsp;socket&nbsp;connections&nbsp;available</span><span class="cs__com">///&nbsp;and&nbsp;stores&nbsp;them&nbsp;in&nbsp;a&nbsp;list.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span><span class="cs__com">///&nbsp;&lt;returns&gt;</span><span class="cs__com">///&nbsp;It&nbsp;returns&nbsp;the&nbsp;current&nbsp;set&nbsp;of&nbsp;TCP&nbsp;socket&nbsp;connections&nbsp;which&nbsp;are&nbsp;active.</span><span class="cs__com">///&nbsp;&lt;/returns&gt;</span><span class="cs__com">///&nbsp;&lt;exception&nbsp;cref=&quot;OutOfMemoryException&quot;&gt;</span><span class="cs__com">///&nbsp;This&nbsp;exception&nbsp;may&nbsp;be&nbsp;thrown&nbsp;by&nbsp;the&nbsp;function&nbsp;Marshal.AllocHGlobal&nbsp;when&nbsp;there</span><span class="cs__com">///&nbsp;is&nbsp;insufficient&nbsp;memory&nbsp;to&nbsp;satisfy&nbsp;the&nbsp;request.</span><span class="cs__com">///&nbsp;&lt;/exception&gt;</span><span class="cs__keyword">private</span><span class="cs__keyword">static</span>&nbsp;List&lt;TcpProcessRecord&gt;&nbsp;GetAllTcpConnections()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">int</span>&nbsp;bufferSize&nbsp;=&nbsp;<span class="cs__number">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&lt;TcpProcessRecord&gt;&nbsp;tcpTableRecords&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;List&lt;TcpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Getting&nbsp;the&nbsp;size&nbsp;of&nbsp;TCP&nbsp;table,&nbsp;that&nbsp;is&nbsp;returned&nbsp;in&nbsp;'bufferSize'&nbsp;variable.</span><span class="cs__keyword">uint</span>&nbsp;result&nbsp;=&nbsp;GetExtendedTcpTable(IntPtr.Zero,&nbsp;<span class="cs__keyword">ref</span>&nbsp;bufferSize,&nbsp;<span class="cs__keyword">true</span>,&nbsp;AF_INET,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TcpTableClass.TCP_TABLE_OWNER_PID_ALL);&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Allocating&nbsp;memory&nbsp;from&nbsp;the&nbsp;unmanaged&nbsp;memory&nbsp;of&nbsp;the&nbsp;process&nbsp;by&nbsp;using&nbsp;the</span><span class="cs__com">//&nbsp;specified&nbsp;number&nbsp;of&nbsp;bytes&nbsp;in&nbsp;'bufferSize'&nbsp;variable.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IntPtr&nbsp;tcpTableRecordsPtr&nbsp;=&nbsp;Marshal.AllocHGlobal(bufferSize);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">try</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;The&nbsp;size&nbsp;of&nbsp;the&nbsp;table&nbsp;returned&nbsp;in&nbsp;'bufferSize'&nbsp;variable&nbsp;in&nbsp;previous</span><span class="cs__com">//&nbsp;call&nbsp;must&nbsp;be&nbsp;used&nbsp;in&nbsp;this&nbsp;subsequent&nbsp;call&nbsp;to&nbsp;'GetExtendedTcpTable'</span><span class="cs__com">//&nbsp;function&nbsp;in&nbsp;order&nbsp;to&nbsp;successfully&nbsp;retrieve&nbsp;the&nbsp;table.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;GetExtendedTcpTable(tcpTableRecordsPtr,&nbsp;<span class="cs__keyword">ref</span>&nbsp;bufferSize,&nbsp;<span class="cs__keyword">true</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AF_INET,&nbsp;TcpTableClass.TCP_TABLE_OWNER_PID_ALL);&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Non-zero&nbsp;value&nbsp;represent&nbsp;the&nbsp;function&nbsp;'GetExtendedTcpTable'&nbsp;failed,</span><span class="cs__com">//&nbsp;hence&nbsp;empty&nbsp;list&nbsp;is&nbsp;returned&nbsp;to&nbsp;the&nbsp;caller&nbsp;function.</span><span class="cs__keyword">if</span>&nbsp;(result&nbsp;!=&nbsp;<span class="cs__number">0</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">return</span><span class="cs__keyword">new</span>&nbsp;List&lt;TcpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Marshals&nbsp;data&nbsp;from&nbsp;an&nbsp;unmanaged&nbsp;block&nbsp;of&nbsp;memory&nbsp;to&nbsp;a&nbsp;newly&nbsp;allocated</span><span class="cs__com">//&nbsp;managed&nbsp;object&nbsp;'tcpRecordsTable'&nbsp;of&nbsp;type&nbsp;'MIB_TCPTABLE_OWNER_PID'</span><span class="cs__com">//&nbsp;to&nbsp;get&nbsp;number&nbsp;of&nbsp;entries&nbsp;of&nbsp;the&nbsp;specified&nbsp;TCP&nbsp;table&nbsp;structure.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MIB_TCPTABLE_OWNER_PID&nbsp;tcpRecordsTable&nbsp;=&nbsp;(MIB_TCPTABLE_OWNER_PID)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.PtrToStructure(tcpTableRecordsPtr,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">typeof</span>(MIB_TCPTABLE_OWNER_PID));&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IntPtr&nbsp;tableRowPtr&nbsp;=&nbsp;(IntPtr)((<span class="cs__keyword">long</span>)tcpTableRecordsPtr&nbsp;&#43;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.SizeOf(tcpRecordsTable.dwNumEntries));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Reading&nbsp;and&nbsp;parsing&nbsp;the&nbsp;TCP&nbsp;records&nbsp;one&nbsp;by&nbsp;one&nbsp;from&nbsp;the&nbsp;table&nbsp;and</span><span class="cs__com">//&nbsp;storing&nbsp;them&nbsp;in&nbsp;a&nbsp;list&nbsp;of&nbsp;'TcpProcessRecord'&nbsp;structure&nbsp;type&nbsp;objects.</span><span class="cs__keyword">for</span>&nbsp;(<span class="cs__keyword">int</span>&nbsp;row&nbsp;=&nbsp;<span class="cs__number">0</span>;&nbsp;row&nbsp;&lt;&nbsp;tcpRecordsTable.dwNumEntries;&nbsp;row&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MIB_TCPROW_OWNER_PID&nbsp;tcpRow&nbsp;=&nbsp;(MIB_TCPROW_OWNER_PID)Marshal.&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;PtrToStructure(tableRowPtr,&nbsp;<span class="cs__keyword">typeof</span>(MIB_TCPROW_OWNER_PID));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpTableRecords.Add(<span class="cs__keyword">new</span>&nbsp;TcpProcessRecord(&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">new</span>&nbsp;IPAddress(tcpRow.localAddr),&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">new</span>&nbsp;IPAddress(tcpRow.remoteAddr),&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BitConverter.ToUInt16(<span class="cs__keyword">new</span><span class="cs__keyword">byte</span>[<span class="cs__number">2</span>]&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpRow.localPort[<span class="cs__number">1</span>],&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpRow.localPort[<span class="cs__number">0</span>]&nbsp;},&nbsp;<span class="cs__number">0</span>),&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BitConverter.ToUInt16(<span class="cs__keyword">new</span><span class="cs__keyword">byte</span>[<span class="cs__number">2</span>]&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpRow.remotePort[<span class="cs__number">1</span>],&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpRow.remotePort[<span class="cs__number">0</span>]&nbsp;},&nbsp;<span class="cs__number">0</span>),&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tcpRow.owningPid,&nbsp;tcpRow.state));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tableRowPtr&nbsp;=&nbsp;(IntPtr)((<span class="cs__keyword">long</span>)tableRowPtr&nbsp;&#43;&nbsp;Marshal.SizeOf(tcpRow));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">catch</span>&nbsp;(OutOfMemoryException&nbsp;outOfMemoryException)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBox.Show(outOfMemoryException.Message,&nbsp;<span class="cs__string">&quot;Out&nbsp;Of&nbsp;Memory&quot;</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBoxButtons.OK,&nbsp;MessageBoxIcon.Stop);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">catch</span>&nbsp;(Exception&nbsp;exception)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBox.Show(exception.Message,&nbsp;<span class="cs__string">&quot;Exception&quot;</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBoxButtons.OK,&nbsp;MessageBoxIcon.Stop);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">finally</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.FreeHGlobal(tcpTableRecordsPtr);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">return</span>&nbsp;tcpTableRecords&nbsp;!=&nbsp;<span class="cs__keyword">null</span>&nbsp;?&nbsp;tcpTableRecords.Distinct()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.ToList&lt;TcpProcessRecord&gt;()&nbsp;:&nbsp;<span class="cs__keyword">new</span>&nbsp;List&lt;TcpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</pre>
</div>
</div>
</div>
<p></p>
<p>GetAllUdpConnections() function</p>
<p></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">编辑脚本</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;This&nbsp;function&nbsp;reads&nbsp;and&nbsp;parses&nbsp;the&nbsp;active&nbsp;UDP&nbsp;socket&nbsp;connections&nbsp;available</span><span class="cs__com">///&nbsp;and&nbsp;stores&nbsp;them&nbsp;in&nbsp;a&nbsp;list.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span><span class="cs__com">///&nbsp;&lt;returns&gt;</span><span class="cs__com">///&nbsp;It&nbsp;returns&nbsp;the&nbsp;current&nbsp;set&nbsp;of&nbsp;UDP&nbsp;socket&nbsp;connections&nbsp;which&nbsp;are&nbsp;active.</span><span class="cs__com">///&nbsp;&lt;/returns&gt;</span><span class="cs__com">///&nbsp;&lt;exception&nbsp;cref=&quot;OutOfMemoryException&quot;&gt;</span><span class="cs__com">///&nbsp;This&nbsp;exception&nbsp;may&nbsp;be&nbsp;thrown&nbsp;by&nbsp;the&nbsp;function&nbsp;Marshal.AllocHGlobal&nbsp;when&nbsp;there</span><span class="cs__com">///&nbsp;is&nbsp;insufficient&nbsp;memory&nbsp;to&nbsp;satisfy&nbsp;the&nbsp;request.</span><span class="cs__com">///&nbsp;&lt;/exception&gt;</span><span class="cs__keyword">private</span><span class="cs__keyword">static</span>&nbsp;List&lt;UdpProcessRecord&gt;&nbsp;GetAllUdpConnections()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">int</span>&nbsp;bufferSize&nbsp;=&nbsp;<span class="cs__number">0</span>;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;List&lt;UdpProcessRecord&gt;&nbsp;udpTableRecords&nbsp;=&nbsp;<span class="cs__keyword">new</span>&nbsp;List&lt;UdpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Getting&nbsp;the&nbsp;size&nbsp;of&nbsp;UDP&nbsp;table,&nbsp;that&nbsp;is&nbsp;returned&nbsp;in&nbsp;'bufferSize'&nbsp;variable.</span><span class="cs__keyword">uint</span>&nbsp;result&nbsp;=&nbsp;GetExtendedUdpTable(IntPtr.Zero,&nbsp;<span class="cs__keyword">ref</span>&nbsp;bufferSize,&nbsp;<span class="cs__keyword">true</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AF_INET,&nbsp;UdpTableClass.UDP_TABLE_OWNER_PID);&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Allocating&nbsp;memory&nbsp;from&nbsp;the&nbsp;unmanaged&nbsp;memory&nbsp;of&nbsp;the&nbsp;process&nbsp;by&nbsp;using&nbsp;the</span><span class="cs__com">//&nbsp;specified&nbsp;number&nbsp;of&nbsp;bytes&nbsp;in&nbsp;'bufferSize'&nbsp;variable.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IntPtr&nbsp;udpTableRecordPtr&nbsp;=&nbsp;Marshal.AllocHGlobal(bufferSize);&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">try</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;The&nbsp;size&nbsp;of&nbsp;the&nbsp;table&nbsp;returned&nbsp;in&nbsp;'bufferSize'&nbsp;variable&nbsp;in&nbsp;previous</span><span class="cs__com">//&nbsp;call&nbsp;must&nbsp;be&nbsp;used&nbsp;in&nbsp;this&nbsp;subsequent&nbsp;call&nbsp;to&nbsp;'GetExtendedUdpTable'</span><span class="cs__com">//&nbsp;function&nbsp;in&nbsp;order&nbsp;to&nbsp;successfully&nbsp;retrieve&nbsp;the&nbsp;table.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;GetExtendedUdpTable(udpTableRecordPtr,&nbsp;<span class="cs__keyword">ref</span>&nbsp;bufferSize,&nbsp;<span class="cs__keyword">true</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;AF_INET,&nbsp;UdpTableClass.UDP_TABLE_OWNER_PID);&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Non-zero&nbsp;value&nbsp;represent&nbsp;the&nbsp;function&nbsp;'GetExtendedUdpTable'&nbsp;failed,</span><span class="cs__com">//&nbsp;hence&nbsp;empty&nbsp;list&nbsp;is&nbsp;returned&nbsp;to&nbsp;the&nbsp;caller&nbsp;function.</span><span class="cs__keyword">if</span>&nbsp;(result&nbsp;!=&nbsp;<span class="cs__number">0</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">return</span><span class="cs__keyword">new</span>&nbsp;List&lt;UdpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Marshals&nbsp;data&nbsp;from&nbsp;an&nbsp;unmanaged&nbsp;block&nbsp;of&nbsp;memory&nbsp;to&nbsp;a&nbsp;newly&nbsp;allocated</span><span class="cs__com">//&nbsp;managed&nbsp;object&nbsp;'udpRecordsTable'&nbsp;of&nbsp;type&nbsp;'MIB_UDPTABLE_OWNER_PID'</span><span class="cs__com">//&nbsp;to&nbsp;get&nbsp;number&nbsp;of&nbsp;entries&nbsp;of&nbsp;the&nbsp;specified&nbsp;TCP&nbsp;table&nbsp;structure.</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MIB_UDPTABLE_OWNER_PID&nbsp;udpRecordsTable&nbsp;=&nbsp;(MIB_UDPTABLE_OWNER_PID)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.PtrToStructure(udpTableRecordPtr,&nbsp;<span class="cs__keyword">typeof</span>(MIB_UDPTABLE_OWNER_PID));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IntPtr&nbsp;tableRowPtr&nbsp;=&nbsp;(IntPtr)((<span class="cs__keyword">long</span>)udpTableRecordPtr&nbsp;&#43;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.SizeOf(udpRecordsTable.dwNumEntries));&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">//&nbsp;Reading&nbsp;and&nbsp;parsing&nbsp;the&nbsp;UDP&nbsp;records&nbsp;one&nbsp;by&nbsp;one&nbsp;from&nbsp;the&nbsp;table&nbsp;and</span><span class="cs__com">//&nbsp;storing&nbsp;them&nbsp;in&nbsp;a&nbsp;list&nbsp;of&nbsp;'UdpProcessRecord'&nbsp;structure&nbsp;type&nbsp;objects.</span><span class="cs__keyword">for</span>&nbsp;(<span class="cs__keyword">int</span>&nbsp;i&nbsp;=&nbsp;<span class="cs__number">0</span>;&nbsp;i&nbsp;&lt;&nbsp;udpRecordsTable.dwNumEntries;&nbsp;i&#43;&#43;)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MIB_UDPROW_OWNER_PID&nbsp;udpRow&nbsp;=&nbsp;(MIB_UDPROW_OWNER_PID)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.PtrToStructure(tableRowPtr,&nbsp;<span class="cs__keyword">typeof</span>(MIB_UDPROW_OWNER_PID));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;udpTableRecords.Add(<span class="cs__keyword">new</span>&nbsp;UdpProcessRecord(<span class="cs__keyword">new</span>&nbsp;IPAddress(udpRow.localAddr),&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;BitConverter.ToUInt16(<span class="cs__keyword">new</span><span class="cs__keyword">byte</span>[<span class="cs__number">2</span>]&nbsp;{&nbsp;udpRow.localPort[<span class="cs__number">1</span>],&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;udpRow.localPort[<span class="cs__number">0</span>]&nbsp;},&nbsp;<span class="cs__number">0</span>),&nbsp;udpRow.owningPid));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tableRowPtr&nbsp;=&nbsp;(IntPtr)((<span class="cs__keyword">long</span>)tableRowPtr&nbsp;&#43;&nbsp;Marshal.SizeOf(udpRow));&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">catch</span>&nbsp;(OutOfMemoryException&nbsp;outOfMemoryException)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBox.Show(outOfMemoryException.Message,&nbsp;<span class="cs__string">&quot;Out&nbsp;Of&nbsp;Memory&quot;</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBoxButtons.OK,&nbsp;MessageBoxIcon.Stop);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">catch</span>&nbsp;(Exception&nbsp;exception)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBox.Show(exception.Message,&nbsp;<span class="cs__string">&quot;Exception&quot;</span>,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MessageBoxButtons.OK,&nbsp;MessageBoxIcon.Stop);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">finally</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Marshal.FreeHGlobal(udpTableRecordPtr);&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">return</span>&nbsp;udpTableRecords&nbsp;!=&nbsp;<span class="cs__keyword">null</span>&nbsp;?&nbsp;udpTableRecords.Distinct()&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.ToList&lt;UdpProcessRecord&gt;()&nbsp;:&nbsp;<span class="cs__keyword">new</span>&nbsp;List&lt;UdpProcessRecord&gt;();&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</pre>
</div>
</div>
</div>
<p></p>
<p>Structures containing information that describes an IPv4 TCP and UDP connection with IPv4 addresses, ports used by the TCP/UDP connection and the specific process ID (PID) associated with connection.</p>
<pre>TCP socket connection structures</pre>
<p></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">编辑脚本</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;The&nbsp;structure&nbsp;contains&nbsp;information&nbsp;that&nbsp;describes&nbsp;an&nbsp;IPv4&nbsp;TCP&nbsp;connection&nbsp;with</span><span class="cs__com">///&nbsp;IPv4&nbsp;addresses,&nbsp;ports&nbsp;used&nbsp;by&nbsp;the&nbsp;TCP&nbsp;connection,&nbsp;and&nbsp;the&nbsp;specific&nbsp;process&nbsp;ID</span><span class="cs__com">///&nbsp;(PID)&nbsp;associated&nbsp;with&nbsp;connection.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;[StructLayout(LayoutKind.Sequential)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">struct</span>&nbsp;MIB_TCPROW_OWNER_PID&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span>&nbsp;MibTcpState&nbsp;state;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">uint</span>&nbsp;localAddr;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MarshalAs(UnmanagedType.ByValArray,&nbsp;SizeConst&nbsp;=&nbsp;<span class="cs__number">4</span>)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">byte</span>[]&nbsp;localPort;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">uint</span>&nbsp;remoteAddr;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MarshalAs(UnmanagedType.ByValArray,&nbsp;SizeConst&nbsp;=&nbsp;<span class="cs__number">4</span>)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">byte</span>[]&nbsp;remotePort;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">int</span>&nbsp;owningPid;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;The&nbsp;structure&nbsp;contains&nbsp;a&nbsp;table&nbsp;of&nbsp;process&nbsp;IDs&nbsp;(PIDs)&nbsp;and&nbsp;the&nbsp;IPv4&nbsp;TCP&nbsp;links&nbsp;that</span><span class="cs__com">///&nbsp;are&nbsp;context&nbsp;bound&nbsp;to&nbsp;these&nbsp;PIDs.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;[StructLayout(LayoutKind.Sequential)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">struct</span>&nbsp;MIB_TCPTABLE_OWNER_PID&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">uint</span>&nbsp;dwNumEntries;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MarshalAs(UnmanagedType.ByValArray,&nbsp;ArraySubType&nbsp;=&nbsp;UnmanagedType.Struct,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SizeConst&nbsp;=&nbsp;<span class="cs__number">1</span>)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span>&nbsp;MIB_TCPROW_OWNER_PID[]&nbsp;table;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}</pre>
</div>
</div>
</div>
<p></p>
<pre>UDP socket connection structures</pre>
<p></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">编辑脚本</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">csharp</span>

<div class="preview">
<pre class="csharp"><span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;The&nbsp;structure&nbsp;contains&nbsp;an&nbsp;entry&nbsp;from&nbsp;the&nbsp;User&nbsp;Datagram&nbsp;Protocol&nbsp;(UDP)&nbsp;listener</span><span class="cs__com">///&nbsp;table&nbsp;for&nbsp;IPv4&nbsp;on&nbsp;the&nbsp;local&nbsp;computer.&nbsp;The&nbsp;entry&nbsp;also&nbsp;includes&nbsp;the&nbsp;process&nbsp;ID</span><span class="cs__com">///&nbsp;(PID)&nbsp;that&nbsp;issued&nbsp;the&nbsp;call&nbsp;to&nbsp;the&nbsp;bind&nbsp;function&nbsp;for&nbsp;the&nbsp;UDP&nbsp;endpoint.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;[StructLayout(LayoutKind.Sequential)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">struct</span>&nbsp;MIB_UDPROW_OWNER_PID&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">uint</span>&nbsp;localAddr;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MarshalAs(UnmanagedType.ByValArray,&nbsp;SizeConst&nbsp;=&nbsp;<span class="cs__number">4</span>)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">byte</span>[]&nbsp;localPort;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">int</span>&nbsp;owningPid;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__com">///&nbsp;&lt;summary&gt;</span><span class="cs__com">///&nbsp;The&nbsp;structure&nbsp;contains&nbsp;the&nbsp;User&nbsp;Datagram&nbsp;Protocol&nbsp;(UDP)&nbsp;listener&nbsp;table&nbsp;for&nbsp;IPv4</span><span class="cs__com">///&nbsp;on&nbsp;the&nbsp;local&nbsp;computer.&nbsp;The&nbsp;table&nbsp;also&nbsp;includes&nbsp;the&nbsp;process&nbsp;ID&nbsp;(PID)&nbsp;that&nbsp;issued</span><span class="cs__com">///&nbsp;the&nbsp;call&nbsp;to&nbsp;the&nbsp;bind&nbsp;function&nbsp;for&nbsp;each&nbsp;UDP&nbsp;endpoint.</span><span class="cs__com">///&nbsp;&lt;/summary&gt;</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;[StructLayout(LayoutKind.Sequential)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">struct</span>&nbsp;MIB_UDPTABLE_OWNER_PID&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span><span class="cs__keyword">uint</span>&nbsp;dwNumEntries;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[MarshalAs(UnmanagedType.ByValArray,&nbsp;ArraySubType&nbsp;=&nbsp;UnmanagedType.Struct,&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SizeConst&nbsp;=&nbsp;<span class="cs__number">1</span>)]&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="cs__keyword">public</span>&nbsp;UdpProcessRecord[]&nbsp;table;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}</pre>
</div>
</div>
</div>
<p></p>
<p>To select the protocol click on the combo box present on the top left corner of the application screen and choose the desired option.</p>
<p>To start listening TCP / UDP socket connections click on the green color button&nbsp;&nbsp;<img id="147557" src="147557-image004.png" alt="" width="21" height="21">&nbsp;present on the top as a ToolStrip menu item.</p>
&nbsp;
<p>To stop listening TCP / UDP socket connections click on the red color button&nbsp;<img id="147559" src="147559-image006.png" alt="" width="21" height="21">&nbsp;&nbsp;present&nbsp;on the top as a ToolStrip menu item.</p>
&nbsp;
<p>To copy all the data populated in the DataGridView click on the copy button <img id="147560" src="147560-image008.jpg" alt="" width="20" height="21">&nbsp; present on the top as a ToolStrip menu item which will save all the data in the
 Clipboard.</p>
<p>&nbsp;</p>
<p><img id="147561" src="147561-image012.jpg" alt="" width="1105" height="128"></p>
