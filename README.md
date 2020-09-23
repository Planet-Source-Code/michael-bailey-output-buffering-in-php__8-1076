<div align="center">

## Output Buffering in PHP


</div>

### Description

Unlock the mysteries of output buffering with PHP.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Michael Bailey](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/michael-bailey.md)
**Level**          |Intermediate
**User Rating**    |5.0 (20 globes from 4 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[System Services/ Functions](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/system-services-functions__8-23.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/michael-bailey-output-buffering-in-php__8-1076/archive/master.zip)





### Source Code

Originally posted by author at www.jinxidoru.com
<h2>Introduction</h2>
<p> Today I want to answer a question which has vexed mankind for centuries, "How do I get my suit dry-cleaned without that disgusting chemical smell? And can I get it done overnight?" Unfortunately, I don't have the slightest idea how to answer that question, so I'm forced instead to discuss output buffering in PHP. That's how life is some times.
</p><p> Output buffering is a powerful tool in PHP which allows you to buffer a script's output. You can then edit this buffer before returning it to the client.
</p><p> This tutorial will procede first by enumerating the functions used in output buffering with a brief explanation of each. Then we will examine how these functions are utililized. We will conclude with three brief examples of scenerios where one might use output buffering. Without further ado then, we will procede to...
</p>
<h2>Output Buffering Functions</h2>
<p> Our toolbox of functions dealing with output buffering is fairly small. The following are the most common functions. The full list can be obtained from the PHP website.</p>
<p style='padding-left:15px;'>
  ob_start([callback function]) - <i>Starts an output buffering session.</i><br>
  ob_flush() - <i>Send the contents of the buffer to the client and clear the buffer.</i><br>
  ob_get_contents() - <i>Returns the contents of the buffer. The buffer is not cleared.</i><br>
  ob_end_clean() - <i>Ends the current buffering session and purges the buffer.</i><br>
  ob_end_flush() - <i>Ends the current buffering session and displays the buffer.</i><br>
  ob_get_length() - <i>(Version >= 4.0.2) Return the size of the current buffer.</i><br>
  ob_get_clean() - <i>(Version >= 4.3) Composite of ob_get_contents() and ob_end_clean(). The buffer is returned and the session ends.</i><br>
</p>
<h2>How It's Done</h2>
<b>The Basics</b>
<p>
  We begin with a simple Hello World script.
</p>
<p><table class=code><tr><td><code><font color="#000000"><font color="#FF9900">// Start buffering<br /></font><font color="#0000CC">ob_start</font><font color="#006600">();<br />print&nbsp;</font><font color="#CC0000">"Hello World"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Grab the buffer<br /></font><font color="#0000CC">$buffer&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#0000CC">ob_get_contents</font><font color="#006600">();<br /><br /></font><font color="#FF9900">// Stop buffering<br /></font><font color="#0000CC">ob_end_clean</font><font color="#006600">();<br /><br /></font><font color="#FF9900">// Display the buffer<br /></font><font color="#006600">print&nbsp;</font><font color="#CC0000">"'$buffer'"</font><font color="#006600">;<br /><br /></font></font>
</code></td></tr></table></p><p>
  The output isn't all that exciting, simply Hello World in single quotes. The function ob_start() initiates the output buffering. "Hello World" is then printed, but it does not get sent to the client. Instead it is buffered until we do something with it. That we do by storing the buffer in $buffer by calling ob_get_contents(). ob_end_clean() ends the output buffering, which means that the last print command displays to the client.
</p>
<b>Nested Output Buffering</b>
<p> You might be asking what happens if you call ob_start() twice without closing the buffer. PHP's real smart, so it handles it very nicely by nesting the buffers. The following script shows how this is done.
</p>
<p><table class=code><tr><td><code><font color="#000000"><font color="#FF9900">// Open buffer #1<br /></font><font color="#0000CC">ob_start</font><font color="#006600">();<br />print&nbsp;</font><font color="#CC0000">"Line 1\n"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Open buffer #2<br /></font><font color="#0000CC">ob_start</font><font color="#006600">();<br />print&nbsp;</font><font color="#CC0000">"Line 2\n"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Grab the contents of buffer #2<br /></font><font color="#0000CC">$buf2&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#0000CC">ob_get_contents</font><font color="#006600">();<br /><br /></font><font color="#FF9900">// Close buffer #2<br /></font><font color="#0000CC">ob_end_clean</font><font color="#006600">();<br />print&nbsp;</font><font color="#CC0000">"Line 3\n"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Grab the contents of buffer #1<br /></font><font color="#0000CC">$buf1&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#0000CC">ob_get_contents</font><font color="#006600">();<br /><br /></font><font color="#FF9900">// Close buffer #1<br /></font><font color="#0000CC">ob_end_clean</font><font color="#006600">();<br /><br /></font><font color="#FF9900">// Output the buffer contents<br /></font><font color="#006600">print&nbsp;</font><font color="#0000CC">$buf1</font><font color="#006600">;<br />print&nbsp;</font><font color="#0000CC">$buf2</font><font color="#006600">;<br /><br /></font></font>
</code></td></tr></table></p>
<p> Without knowledge of output buffering, one would expect the lines to be printed in numerical order. This is not the case though. After printing line 1, a second buffer is opened which prints line 2 and captures the buffer. Line 3 is then printed in the first buffer. The first buffer captures lines 1 and 3 while the second captures line 2.
</p><p> What all this means is that you can build functions and scripts with output buffering without worrying about fouling up an algorithm nested below or above in the logic. As long as you always remember to close every buffer you open, you won't have any problems.
</p>
<b>Callback Functions</b>
<p> Arguably the most powerful element of output buffering is the capability of defining callback functions. You can define a function to be called when the buffer is flushed to the screen. This callback function receives the buffer as a parameter. The returned value is then printed.</p>
<p><table class=code><tr><td><code><font color="#000000"><font color="#FF9900">// Start buffering with a callback function<br /></font><font color="#0000CC">ob_start</font><font color="#006600">(</font><font color="#CC0000">"callback"</font><font color="#006600">);<br />print&nbsp;</font><font color="#CC0000">"Line 1\n"</font><font color="#006600">;<br />print&nbsp;</font><font color="#CC0000">"Line 2\n"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Flush the contents<br /></font><font color="#0000CC">ob_flush</font><font color="#006600">();<br />print&nbsp;</font><font color="#CC0000">"Line 3\n"</font><font color="#006600">;<br /><br /></font><font color="#FF9900">// Define the callback function<br /></font><font color="#006600">function&nbsp;</font><font color="#0000CC">callback</font><font color="#006600">(</font><font color="#0000CC">$buffer</font><font color="#006600">)&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;</font><font color="#CC0000">"Here's&nbsp;the&nbsp;buffer:&nbsp;$buffer"</font><font color="#006600">;<br />}<br /><br /></font></font>
</code></td></tr></table></p>
<p> The script displays lines 1 through 3. After printing line 2, the buffer is flushed. This passes the data to the callback function, which prepends to trite little message. Since the buffer isn't closed before the end of the script, PHP automatically flushes the buffer, which calls the callback function once again. One of the examples below deals with callback functions more in depth.
</p><p> There are a couple of things to remember when using callback functions. You have to return what you want to displayed, not print it. The print command doesn't work in the callback function. Also, you cannot use output buffering inside of a callback function. It's because of some United Nations resolution or something. It just can't be done, so don't try it.
</p>
<h2>Examples</h2>
<p>
  Now that we are all experts with output buffering, let's examine a few examples of what output buffering can do for us. We will first examine a simple method of caching pages that require a large amount of processor time to execute, so you only have to update the information when absolutely necessary. We will then see how to catch the output from functions like fileread(). The last example is how to add footer functions.
</p>
<b>Caching with PHP</b>
<p>
  Recently I wrote a script which took a long time to run. There was a very large list of race results for a 5K. The script loaded the results from a CSV spreadsheet file and output them in a visually pleasing format. With the size of the spreadsheet, the parsing took an extremely long time. This definitely wouldn't do, but then my good friend output buffering came and joined my side.
</p><p> The following code shows how output buffering can be employed to cache whole pages or sections of pages. A call is made to parse_spreadsheet() which is not a real function. This simply exists to represent a function which parses the spreadsheet. In testing this snippet, replace this line with a function you have prepared. The name of the cache file and spreadsheet are stored in the constants CACHE_FILE and SPREADSHEET_FILE respectively.
</p>
<p><table class=code><tr><td><code><font color="#000000"&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Define file locations<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">define</font><font color="#006600">(</font><font color="#0000CC">CACHE_FILE</font><font color="#006600">,&nbsp;</font><font color="#CC0000">"./spreadsheet_cache"</font><font color="#006600">);<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">define</font><font color="#006600">(</font><font color="#0000CC">SPREADSHEET_FILE</font><font color="#006600">,&nbsp;</font><font color="#CC0000">"./spreadsheet.csv"</font><font color="#006600">);<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Check which has been updated most recently, the spreadsheet or cache<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#006600">if&nbsp;(@</font><font color="#0000CC">filemtime</font><font color="#006600">(</font><font color="#0000CC">SPREADSHEET_FILE</font><font color="#006600">)&nbsp;&gt;&nbsp;@</font><font color="#0000CC">filemtime</font><font color="#006600">(</font><font color="#0000CC">CACHE_FILE</font><font color="#006600">))&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Initiate output buffering<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">ob_start</font><font color="#006600">();<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Parse the spreadsheet<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">parse_spreadsheet</font><font color="#006600">();<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Write the buffer to the cache file<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">$fh&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#0000CC">fopen</font><font color="#006600">(</font><font color="#0000CC">CACHE_FILE</font><font color="#006600">,&nbsp;</font><font color="#CC0000">"w"</font><font color="#006600">);<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">fwrite</font><font color="#006600">(</font><font color="#0000CC">$fh</font><font color="#006600">,&nbsp;</font><font color="#0000CC">ob_get_contents</font><font color="#006600">());<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">fclose</font><font color="#006600">(</font><font color="#0000CC">$fh</font><font color="#006600">);<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// End the output buffer and display the contents<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">ob_end_flush</font><font color="#006600">();<br />&nbsp;&nbsp;&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// If the cache is newer than the spreadsheet, simply display<br />&nbsp;&nbsp;&nbsp;&nbsp;// the cache file rather than parsing again<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#006600">}&nbsp;else&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">readfile</font><font color="#006600">(</font><font color="#0000CC">CACHE_FILE</font><font color="#006600">);<br />&nbsp;&nbsp;&nbsp;&nbsp;}<br /><br /></font></font>
</code></td></tr></table></p>
<p> One should not think that this technique is limited to spreadsheets only. Another useful example is when data must be retrieved from a database. This works well in any situation where a lot of data needs to be parsed on a regular basis which is possibly very time consuming. Simply replace parse_spreadsheet() with the appropriate display function and the if statement with an appropriate method of detecting whether the data should be updated or if the cache can be used.
</p>
<b>Catching Text from Functions</b>
<p> There aren't too many things that annoy me. My entire list could be enumerated as follows: cats, people who buy 300 individual cans of soup yet count it as one item in the express lane, and functions like fileread(). I can't offer any solutions for the first two (no legal solutions at least), but the third I can.
</p><p> By functions like fileread() I am implying functions which simply dump their output to the output buffer rather than returning their output as a string. A brief list (by no means comprehensive) of such functions is: passthru(), fpassthru(), system(), etc. These functions are extremely annoying when you are simply trying to obtain and parse the output. Thankfully, our good friend output buffering comes to the rescue.
</p><p> The following snippet shows how to read an entire file into a variable very easily.</p>
<p><table class=code><tr><td><code><font color="#000000"&nbsp;&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Start output buffering<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">ob_start</font><font color="#006600">();&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Load file contents<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">readfile</font><font color="#006600">(</font><font color="#CC0000">"data.txt"</font><font color="#006600">);<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Transfer the contents into a variable<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">$data&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#0000CC">ob_get_contents</font><font color="#006600">();<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Close the output buffering<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">ob_end_clean</font><font color="#006600">();<br /><br /></font></font>
</code></td></tr></table></p>
<p> The entire contents of data.txt are now stored in $data. It is obvious how one could easily replace readfile() with any of the previously mentioned functions to capture any sort of function output into a variable. For example, to run a shell script and suppress the results can be accomplished in the same way but without placing the output buffer contents in a variable.
</p>
<b>Footers</b>
<p> In most PHP applications header files are included at the beginning of each script to handle such things as initiating database connections, loading libraries, instantiating classes, etc. There are often things which should be done at the end of each script as well, such as parsing templates, closing connections, etc. In the past, I have been forced to explicitly call these functions in each seperate script. I didn't know of anyway to globally set such footer functions, until now.
</p><p> Output buffering with the use of a callback function allows the use of footer functions without having to edit the php.ini file. We will first examine a very simple example. Including the following snippet at the top of a script will cause a copyright message to be appended to the bottom of the page.
</p>
<p><table class=code><tr><td><code><font color="#000000"&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Start output buffering with a callback function<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">ob_start</font><font color="#006600">(</font><font color="#CC0000">"footer_copyright"</font><font color="#006600">);<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Function called when the output buffer is flushed<br />&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#006600">function&nbsp;</font><font color="#0000CC">footer_copyright</font><font color="#006600">(</font><font color="#0000CC">$buffer</font><font color="#006600">)&nbsp;{<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Generate the copyright message<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#0000CC">$copyright&nbsp;</font><font color="#006600">=&nbsp;</font><font color="#CC0000">"&lt;p align=center&gt;&lt;small&gt;Copyright &amp;copy; "<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#006600">.&nbsp;</font><font color="#0000CC">implode</font><font color="#006600">(</font><font color="#CC0000">","</font><font color="#006600">,&nbsp;</font><font color="#0000CC">range</font><font color="#006600">(</font><font color="#0000CC">2002</font><font color="#006600">,&nbsp;</font><font color="#0000CC">date</font><font color="#006600">(</font><font color="#CC0000">'Y'</font><font color="#006600">)))<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.&nbsp;</font><font color="#CC0000">"&lt;/small&gt;&lt;/p&gt;"</font><font color="#006600">;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#FF9900">// Return the composite buffer<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</font><font color="#006600">return&nbsp;</font><font color="#0000CC">$buffer</font><font color="#006600">.</font><font color="#0000CC">$copyright</font><font color="#006600">;<br />&nbsp;&nbsp;&nbsp;&nbsp;}<br /><br /><br /></font></font>
</code></td></tr></table></p><p>
The footer function need not only deal with adding text to the page. You can perform any operation within this footer function as you would anywhere else. Therefore, you could increment counters, close file handles, finalize object instances, etc. The only thing you cannot do inside a output buffering handler is output buffering. This should not be too large of a restriction though.
</p>
<b>Conclusion</b>
<p>
  Output buffering is one of those parts of PHP which often gets overlooked. This is unfortunate since wise use of output buffering can simplify many projects. Once you learn to use it proficiently, you will find numerous scenarios where it can be used that you'd never thought of before.
</p>
 <br>

