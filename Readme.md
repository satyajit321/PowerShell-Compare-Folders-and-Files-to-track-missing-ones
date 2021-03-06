<p>This is a straight forward and simple script that allows you to quickly compare a failed copy folder attempt, which ofcourse resulted you dangling half the way.</p>
<p>For smaller copies you can start again select skip, but if the data is huge and lots of files, its not very predictable or wise to copy it again.</p>
<p>Or maybe you just want to validate the contents of a folder with some source.</p>
<p>&nbsp;</p>
<p>The script is basically using DIR or Get-ChildItem command to list out folders and files of source and destination. And then compares the objects.</p>
<p>&nbsp;</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>PowerShell</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span></div>
<span class="hidden">powershell</span>
<pre class="hidden">&lt;#CompareSearch-Missingfiles.ps1
#File/folder comparison tool to check a source and destination for missing files/folders
#Reports missing source files, ignores extra files on the destination

#Imporovement scopes:
*Make it efficient by skipping missing folder files, folder is missing so files will be as well
#Satyajit321
4:39 PM 18/1/2016

.EXAMPLE
Search-MissingFiles.ps1 | Out-File MissingSource.txt
#&gt;


$SourcePath = "C:\Test\CopyData"
$DestPath = "C:\Test\CopiedData"

#Equivalent cmd for 'dir /b /s'
$Source = (dir $SourcePath -Recurse).FullName
$Dest = (dir $DestPath -Recurse).FullName

#1/-1 - Different, 0 - Same
#$Source[1].CompareTo($Dest[1])

#$Source[1].CompareTo($Source[1])

#Cleanup to make the paths appear same
#Basically removing the uncommon source and destination paths portion
for($i=0;$i -lt $Source.Count;$i++)
{
$Source[$i] = $Source[$i].Replace($SourcePath, "")
}

for($i=0;$i -lt $Dest.Count;$i++)
{
    $Dest[$i] = $Dest[$i].Replace($DestPath, "")
}



#Loop the Source files
foreach ($fileS in $Source)
{
    #Counter for match
    $Found = $false

    #Loop destination files to Compare with each source
    foreach ($fileD in $Dest)
    {
        #Check Exact Match
        if($fileD.CompareTo($fileS) -eq 0)
            {$Found = $true}
    
    }
    #Writeout missing files
    if(-not $Found)
        {"$SourcePath$fileS"}

}

</pre>
<div class="preview">
<pre class="powershell"><span class="powerShell__mlcom">&lt;#CompareSearch-Missingfiles.ps1&nbsp;
#File/folder&nbsp;comparison&nbsp;tool&nbsp;to&nbsp;check&nbsp;a&nbsp;source&nbsp;and&nbsp;destination&nbsp;for&nbsp;missing&nbsp;files/folders&nbsp;
#Reports&nbsp;missing&nbsp;source&nbsp;files,&nbsp;ignores&nbsp;extra&nbsp;files&nbsp;on&nbsp;the&nbsp;destination&nbsp;
&nbsp;
#Imporovement&nbsp;scopes:&nbsp;
*Make&nbsp;it&nbsp;efficient&nbsp;by&nbsp;skipping&nbsp;missing&nbsp;folder&nbsp;files,&nbsp;folder&nbsp;is&nbsp;missing&nbsp;so&nbsp;files&nbsp;will&nbsp;be&nbsp;as&nbsp;well&nbsp;
#Satyajit321&nbsp;
4:39&nbsp;PM&nbsp;18/1/2016&nbsp;
&nbsp;
.EXAMPLE&nbsp;
Search-MissingFiles.ps1&nbsp;|&nbsp;Out-File&nbsp;MissingSource.txt&nbsp;
#&gt;</span>&nbsp;
&nbsp;
&nbsp;
<span class="powerShell__variable">$SourcePath</span>&nbsp;=&nbsp;<span class="powerShell__string">"C:\Test\CopyData"</span>&nbsp;
<span class="powerShell__variable">$DestPath</span>&nbsp;=&nbsp;<span class="powerShell__string">"C:\Test\CopiedData"</span>&nbsp;
&nbsp;
<span class="powerShell__com">#Equivalent&nbsp;cmd&nbsp;for&nbsp;'dir&nbsp;/b&nbsp;/s'</span>&nbsp;
<span class="powerShell__variable">$Source</span>&nbsp;=&nbsp;(<span class="powerShell__alias">dir</span>&nbsp;<span class="powerShell__variable">$SourcePath</span>&nbsp;<span class="powerShell__operator">-</span>Recurse).FullName&nbsp;
<span class="powerShell__variable">$Dest</span>&nbsp;=&nbsp;(<span class="powerShell__alias">dir</span>&nbsp;<span class="powerShell__variable">$DestPath</span>&nbsp;<span class="powerShell__operator">-</span>Recurse).FullName&nbsp;
&nbsp;
<span class="powerShell__com">#1/-1&nbsp;-&nbsp;Different,&nbsp;0&nbsp;-&nbsp;Same</span>&nbsp;
<span class="powerShell__com">#$Source[1].CompareTo($Dest[1])</span>&nbsp;
&nbsp;
<span class="powerShell__com">#$Source[1].CompareTo($Source[1])</span>&nbsp;
&nbsp;
<span class="powerShell__com">#Cleanup&nbsp;to&nbsp;make&nbsp;the&nbsp;paths&nbsp;appear&nbsp;same</span>&nbsp;
<span class="powerShell__com">#Basically&nbsp;removing&nbsp;the&nbsp;uncommon&nbsp;source&nbsp;and&nbsp;destination&nbsp;paths&nbsp;portion</span>&nbsp;
<span class="powerShell__keyword">for</span>(<span class="powerShell__variable">$i</span>=0;<span class="powerShell__variable">$i</span>&nbsp;<span class="powerShell__operator">-</span>lt&nbsp;<span class="powerShell__variable">$Source</span>.Count;<span class="powerShell__variable">$i</span><span class="powerShell__operator">+</span><span class="powerShell__operator">+</span>)&nbsp;
{&nbsp;
<span class="powerShell__variable">$Source</span>[<span class="powerShell__variable">$i</span>]&nbsp;=&nbsp;<span class="powerShell__variable">$Source</span>[<span class="powerShell__variable">$i</span>].Replace(<span class="powerShell__variable">$SourcePath</span>,&nbsp;<span class="powerShell__string">""</span>)&nbsp;
}&nbsp;
&nbsp;
<span class="powerShell__keyword">for</span>(<span class="powerShell__variable">$i</span>=0;<span class="powerShell__variable">$i</span>&nbsp;<span class="powerShell__operator">-</span>lt&nbsp;<span class="powerShell__variable">$Dest</span>.Count;<span class="powerShell__variable">$i</span><span class="powerShell__operator">+</span><span class="powerShell__operator">+</span>)&nbsp;
{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__variable">$Dest</span>[<span class="powerShell__variable">$i</span>]&nbsp;=&nbsp;<span class="powerShell__variable">$Dest</span>[<span class="powerShell__variable">$i</span>].Replace(<span class="powerShell__variable">$DestPath</span>,&nbsp;<span class="powerShell__string">""</span>)&nbsp;
}&nbsp;
&nbsp;
&nbsp;
&nbsp;
<span class="powerShell__com">#Loop&nbsp;the&nbsp;Source&nbsp;files</span>&nbsp;
<span class="powerShell__keyword">foreach</span>&nbsp;(<span class="powerShell__variable">$fileS</span>&nbsp;<span class="powerShell__keyword">in</span>&nbsp;<span class="powerShell__variable">$Source</span>)&nbsp;
{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__com">#Counter&nbsp;for&nbsp;match</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__variable">$Found</span>&nbsp;=&nbsp;<span class="powerShell__variable">$false</span>&nbsp;
&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__com">#Loop&nbsp;destination&nbsp;files&nbsp;to&nbsp;Compare&nbsp;with&nbsp;each&nbsp;source</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__keyword">foreach</span>&nbsp;(<span class="powerShell__variable">$fileD</span>&nbsp;<span class="powerShell__keyword">in</span>&nbsp;<span class="powerShell__variable">$Dest</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__com">#Check&nbsp;Exact&nbsp;Match</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__keyword">if</span>(<span class="powerShell__variable">$fileD</span>.CompareTo(<span class="powerShell__variable">$fileS</span>)&nbsp;<span class="powerShell__operator">-</span>eq&nbsp;0)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<span class="powerShell__variable">$Found</span>&nbsp;=&nbsp;<span class="powerShell__variable">$true</span>}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__com">#Writeout&nbsp;missing&nbsp;files</span>&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span class="powerShell__keyword">if</span>(<span class="powerShell__operator">-</span>not&nbsp;<span class="powerShell__variable">$Found</span>)&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{<span class="powerShell__string">"$SourcePath$fileS"</span>}&nbsp;
&nbsp;
}&nbsp;
&nbsp;
</pre>
</div>
</div>
</div>
<div class="endscriptcode"></div>
<p>&nbsp;</p>
<div class="endscriptcode"></div>
<div class="mcePaste" id="_mcePaste" style="left: -10000px; top: 444px; width: 1px; height: 1px; overflow: hidden;"></div>
