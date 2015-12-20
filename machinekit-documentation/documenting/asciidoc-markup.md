This document is a guide for using asciidoc markup

Master Document file Headers, first line replace en with two letter language code
---------------------------------------------------------------------------------

Getting Started V, 2015-12-20

The EMC Team

The parameters are interpreted well before the load files lang-xx.conf and the other files .conf It is particularly important for the parameters as :lang:

The typical authorized attributes : doctype, lang, encoding, icons, data-uri, toc, numbered

More information here <http://www.methods.co.nz/asciidoc/userguide.html#_introduction> 8.2 Header

[caption](#anchor)

Example

[bombs](#sub:torpodeos)

only bombs will show in the text with an underline so always provide a caption for a link

---- &lt; 4 dashes start of block lines of code … more code ---- &lt; 4 dashes end of block &lt; 1 blank line after

1.  &lt; 4 periods start the block literal block will show up just as you type it.

2.  &lt; 4 periods end the block &lt; 1 blank line after

-   bullet

    -   bullet

        1.  numbered

            1.  numbered

List Item Continuation \* List item paragaph one continued with a single plus hard left.

+ List item one continued with a second paragraph and first line of second paragraph must be hard left.

HAL pins and parameters use the following format for consistency \* *parport.&lt;p&gt;.pin-&lt;n&gt;-out* (bit) Drives a physical output pin. bulleted list with the pin name emphasized by surrounding it with single quotes.

put all images in the images subdirectory

![&lt;attributes&gt;](<target>)

example <span class="image"> ![](:images/axis.png) </span> <span class="image"> ![Tiger image](:images/tiger.png) </span>

If the image is small and displays too big in the pdf you can use the scaledwidth="60%" attribute

Same as images except only one : is used and no attributes are used between the \[\]

My text has an image in it <span class="image"> ![](images/axisbutton.png) </span>

Use the following format for figures so any links to the anchor will move to correct place. Only create a link to a figure if it is somewhere else and not the next thing you see when scrolling down. Just say something like "in the following figure…"

![](images/parport-block-diag.png)

Figure 1. Parport Block Diagram

Inline example NOTE: This is an example note.

Paragraph example

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">This is an example note of the paragraph style.</td>
</tr>
</tbody>
</table>

Styles include NOTE TIP IMPORTANT WARNING CAUTION

Normally characters like ' and \* would not print unless you use \\' or \\\* then they will print as normal characters.

Making a horizontal labeled list.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Label<br />
</td>
<td align="left"><p>The text about the label</p></td>
</tr>
</tbody>
</table>

Latexmath is broken at the moment and trashes the HTML. A work around is to use open office math to create the math image and gimp to screen capture and save to a png.

check asciidoc cheatsheet <http://powerman.name/doc/asciidoc> or user manual <http://www.methods.co.nz/asciidoc/userguide.html> for details about format.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


