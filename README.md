Download Link: https://assignmentchef.com/product/solved-cs4390-homework1-the-basic-http-get-response-interaction
<br>
Having gotten our feet wet with the Wireshark packet sniffer in the introductory lab, we’re now ready to use Wireshark to investigate protocols in operation. In this lab, we’ll explore several aspects of the HTTP protocol: the basic GET/response interaction, HTTP message formats, retrieving large HTML files, retrieving HTML files with embedded objects, and HTTP authentication and security. Before beginning these labs, you might want to review Section 2.2 of the text.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>

<h1 style="margin-left: -.25pt;">The Basic HTTP GET/response interaction</h1>

Let’s begin our exploration of HTTP by downloading a very simple HTML file – one that is very short, and contains no embedded objects.  Do the following:

<ol>

 <li>Start up your web browser.</li>

 <li>Start up the Wireshark packet sniffer, as described in the Introductory lab (but don’t yet begin packet capture). Enter “http” (just the letters, not the quotation marks) in the display-filter-specification window, so that only captured HTTP messages will be displayed later in the packet-listing window.  (We’re only interested in the HTTP protocol here, and don’t want to see the clutter of all captured packets).</li>

 <li>Wait a bit more than one minute (we’ll see why shortly), and then begin Wireshark packet capture.</li>

 <li>Enter the following to your browser <u>http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html</u> Your browser should display the very simple, one-line HTML file.</li>

 <li>Stop Wireshark packet capture.</li>

</ol>

Your Wireshark window should look similar to the window shown in Figure 1.  If you are unable to run Wireshark on a live network connection, you can download a packet trace that was created when the steps above were followed.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>

<strong>Figure 1:</strong> Wireshark Display after http://gaia.cs.umass.edu/wireshark-labs/ HTTPwireshark-file1.html has been retrieved by your browser

The example in Figure 1 shows in the packet-listing window that two HTTP messages were captured: the GET message (from your browser to the gaia.cs.umass.edu web server) and the response message from the server to your browser.  The packet-contents window shows details of the selected message (in this case the HTTP OK message, which is highlighted in the packet-listing window).  Recall that since the HTTP message was carried inside a TCP segment, which was carried inside an IP datagram, which was carried within an Ethernet frame, Wireshark displays the Frame, Ethernet, IP, and TCP packet information as well.  We want to minimize the amount of non-HTTP data displayed (we’re interested in HTTP here, and will be investigating these other protocols is later labs), so make sure the boxes at the far left of the Frame, Ethernet, IP and TCP information have a plus sign or a right-pointing triangle (which means there is hidden, undisplayed information), and the HTTP line has a minus sign or a down-pointing triangle (which means that all information about the HTTP message is displayed).




(<em>Note:</em> You should ignore any HTTP GET and response for favicon.ico.  If you see a reference to this file, it is your browser automatically asking the server if it (the server) has a small icon file that should be displayed next to the displayed URL in your browser.  We’ll ignore references to this pesky file in this lab.).

By looking at the information in the HTTP GET and response messages, answer the following questions.  When answering the following questions, you should print out the GET and response messages (see the introductory Wireshark lab for an explanation of how to do this) and indicate where in the message you’ve found the information that answers the following questions. When you hand in your assignment, annotate the output so that it’s clear where in the output you’re getting the information for your answer (e.g., for our classes, we ask that students markup paper copies with a pen, or annotate electronic copies with text in a colored font).

<ol>

 <li>Is your browser running HTTP version 1.0 or 1.1? What version of HTTP is the server running?</li>

 <li>What languages (if any) does your browser indicate that it can accept to the server?</li>

 <li>What is the IP address of your computer? Of the gaia.cs.umass.edu server?</li>

 <li>What is the status code returned from the server to your browser?</li>

 <li>When was the HTML file that you are retrieving last modified at the server?</li>

 <li>How many bytes of content are being returned to your browser?</li>

 <li>By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window? If so, name one.</li>

</ol>




In your answer to question 5 above, you might have been surprised to find that the document you just retrieved was last modified within a minute before you downloaded the document. That’s because (for this particular file), the gaia.cs.umass.edu server is setting the file’s last-modified time to be the current time, and is doing so once per minute. Thus, if you wait a minute between accesses, the file will appear to have been recently modified, and hence your browser will download a “new” copy of the document.

2. The HTTP CONDITIONAL GET/response interaction




Recall from Section 2.2.6 of the text, that most web browsers perform object caching and thus perform a conditional GET when retrieving an HTTP object. Before performing the steps below, make sure your browser’s cache is empty. (To do this under Firefox, select <em>Tools-&gt;Clear Recent History</em> and check the Cache box, or for Internet Explorer, select <em>Tools-&gt;Internet Options-&gt;Delete File; </em>these actions will remove cached files from your browser’s cache.) Now do the following:

<ul>

 <li>Start up your web browser, and make sure your browser’s cache is cleared, as discussed above.</li>

 <li>Start up the Wireshark packet sniffer</li>

 <li>Enter the following URL into your browser <u>http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file2.html</u> Your browser should display a very simple five-line HTML file.</li>

 <li>Quickly enter the same URL into your browser again (or simply select the refresh button on your browser)</li>

 <li>Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed later in the packet-listing window.</li>

 <li>(<em>Note:</em> If you are unable to run Wireshark on a live network connection, you can use the http-ethereal-trace-2 packet trace to answer the questions below; see footnote 1. This trace file was gathered while performing the steps above on one of the author’s computers.)</li>

</ul>




Answer the following questions:

<ol start="8">

 <li>Inspect the contents of the first HTTP GET request from your browser to the server. Do you see an “IF-MODIFIED-SINCE” line in the HTTP GET?</li>

 <li>Inspect the contents of the server response. Did the server explicitly return the contents of the file? How can you tell?</li>

 <li>Now inspect the contents of the second HTTP GET request from your browser to the server. Do you see an “IF-MODIFIED-SINCE:” line in the HTTP GET? If so, what information follows the “IF-MODIFIED-SINCE:” header?</li>

 <li>What is the HTTP status code and phrase returned from the server in response to this second HTTP GET? Did the server explicitly return the contents of the file?</li>

</ol>

<h1>3. Retrieving Long Documents</h1>

In our examples thus far, the documents retrieved have been simple and short HTML files. Let’s next see what happens when we download a long HTML file.  Do the following:

<ul>

 <li>Start up your web browser, and make sure your browser’s cache is cleared, as discussed above.</li>

 <li>Start up the Wireshark packet sniffer</li>

 <li>Enter the following URL into your browser <u>http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html</u> Your browser should display the rather lengthy US Bill of Rights.</li>

 <li>Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed.</li>

 <li>(<em>Note:</em> If you are unable to run Wireshark on a live network connection, you can use the http-ethereal-trace-3 packet trace to answer the questions below; see footnote 1. This trace file was gathered while performing the steps above on one of the author’s computers.)</li>

</ul>

In the packet-listing window, you should see your HTTP GET message, followed by a multiple-packet TCP response to your HTTP GET request.  This multiple-packet response deserves a bit of explanation.  Recall from Section 2.2 (see Figure 2.9 in the text) that the HTTP response message consists of a status line, followed by header lines, followed by a blank line, followed by the entity body.  In the case of our HTTP GET, the entity body in the response is the <em>entire</em> requested HTML file.  In our case here, the HTML file is rather long, and at 4500 bytes is too large to fit in one TCP packet.  The single HTTP response message is thus broken into several pieces by TCP, with each piece being contained within a separate TCP segment (see Figure 1.24 in the text). In recent versions of Wireshark, Wireshark indicates each TCP segment as a separate packet, and the fact that the single HTTP response was fragmented across multiple TCP packets is indicated by the “TCP segment of a reassembled PDU” in the Info column of the Wireshark display.  Earlier versions of Wireshark used  the “Continuation” phrase to indicated that the entire content of an HTTP message was broken across multiple TCP segments..  We stress here that there is no “Continuation” message in HTTP!

Answer the following questions:

<ol start="12">

 <li>How many HTTP GET request messages did your browser send? Which packet number in the trace contains the GET message for the Bill or Rights?</li>

 <li>Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request?</li>

 <li>What is the status code and phrase in the response?</li>

 <li>How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?</li>

</ol>

<h1>4. HTML Documents with Embedded Objects</h1>

Now that we’ve seen how Wireshark displays the captured packet traffic for large HTML files, we can look at what happens when your browser downloads a file with embedded objects, i.e., a file that includes other objects (in the example below, image files) that are stored on another server(s).




Do the following:

<ul>

 <li>Start up your web browser, and make sure your browser’s cache is cleared, as discussed above.</li>

 <li>Start up the Wireshark packet sniffer</li>

 <li>Enter the following URL into your browser <u>http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html</u> Your browser should display a short HTML file with two images. These two images are referenced in the base HTML file. That is, the images themselves are not contained in the HTML; instead the URLs for the images are contained in the downloaded HTML file. As discussed in the textbook, your browser will have to retrieve these logos from the indicated web sites.   Our publisher’s logo is retrieved from the www.aw-bc.com web site.  The image of the cover for our 5<sup>th</sup> edition (one of our favorite covers) is stored at the manic.cs.umass.edu server.</li>

 <li>Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed.</li>

 <li>(<em>Note:</em> If you are unable to run Wireshark on a live network connection, you can use the http-ethereal-trace-4 packet trace to answer the questions below; see</li>

</ul>

footnote 1. This trace file was gathered while performing the steps above on one of the author’s computers.)




Answer the following questions:

<ol start="16">

 <li>How many HTTP GET request messages did your browser send? To which Internet addresses were these GET requests sent?</li>

 <li>Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel?</li>

</ol>




<h1>5 HTTP Authentication</h1>




Finally, let’s try visiting a web site that is password-protected and examine the sequence of HTTP message exchanged for such a site.  The URL

http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wireshark-file5.html is password protected.  The username is “wireshark-students” (without the quotes), and the password is “network” (again, without the quotes).  So let’s access this “secure” password-protected site.  Do the following:

<ul>

 <li>Make sure your browser’s cache is cleared, as discussed above, and close down your browser. Then, start up your browser</li>

 <li>Start up the Wireshark packet sniffer</li>

 <li>Enter the following URL into your browser <u>http://gaia.cs.umass.edu/wireshark-labs/protected_pages/HTTP-wiresharkfile5.html</u></li>

</ul>

Type the requested user name and password into the pop up box.

<ul>

 <li>Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed later in the packet-listing window.</li>

 <li>(<em>Note:</em> If you are unable to run Wireshark on a live network connection, you can use the http-ethereal-trace-5 packet trace to answer the questions below; see footnote 2. This trace file was gathered while performing the steps above on one of the author’s computers.)</li>

</ul>

Now let’s examine the Wireshark output.  You might want to first read up on HTTP authentication by reviewing the easy-to-read material on “HTTP Access Authentication Framework” at <u>http://frontier.userland.com/stories/storyReader$2159</u>

Answer the following questions:

<ol start="18">

 <li>What is the server’s response (status code and phrase) in response to the initial HTTP GET message from your browser?</li>

 <li>When your browser’s sends the HTTP GET message for the second time, what new field is included in the HTTP GET message?</li>

</ol>




The username (wireshark-students) and password (network) that you entered are encoded in the string of characters (d2lyZXNoYXJrLXN0dWRlbnRzOm5ldHdvcms=) following the “Authorization: Basic” header in the client’s HTTP GET message.  While it may appear that your username and password are encrypted, they are simply encoded in a format known as Base64 format. The username and password are <em>not</em> encrypted!  To see this, go to  <u>http://www.motobit.com/util/base64-decoder-encoder.asp</u> and enter the base64-encoded string d2lyZXNoYXJrLXN0dWRlbnRz and decode.  <em>Voila!  </em>You have translated from Base64 encoding to ASCII encoding, and thus should see your username!  To view the password, enter the remainder of the string Om5ldHdvcms= and press decode.  Since anyone can download a tool like Wireshark and sniff packets (not just their own) passing by their network adaptor, and anyone can translate from Base64 to ASCII (you just did it!), it should be clear to you that simple passwords on WWW sites are not secure unless additional measures are taken.




Fear not! As we will see in Chapter 8, there are ways to make WWW access more secure.  However, we’ll clearly need something that goes beyond the basic HTTP authentication framework!







<a href="#_ftnref1" name="_ftn1"><strong>[1]</strong></a> References to figures and sections are for the 6<sup>th</sup> edition of our text, Computer Networks, A Top-down Approach, 6<sup>th</sup> ed., J.F. Kurose and K.W. Ross, Addison-Wesley/Pearson, 2012.

<a href="#_ftnref2" name="_ftn2"><strong>[2]</strong></a> Download the zip file <u>http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces.zip</u> and extract the file http-ethereal-trace-1. The traces in this zip file were collected by Wireshark running on one of the author’s computers, while performing the steps indicated in the Wireshark lab. Once you have downloaded the trace, you can load it into Wireshark and view the trace using the File pull down menu, choosing Open, and then selecting the http-ethereal-trace-1 trace file.  The resulting display should look similar to Figure 1. (The Wireshark user interface displays just a bit differently on different operating systems, and in different versions of Wireshark).