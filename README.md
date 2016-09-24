# OpenDocument DOMDocument Model
# ODS - Open Document Spreadsheet
Unzip ods 
```bash
$ unzip file.ods
```
##Content of archive
```
├── Configurations2
│   ├── accelerator
│   │   └── current.xml
│   ├── floater
│   ├── images
│   │   └── Bitmaps
│   ├── menubar
│   ├── popupmenu
│   ├── progressbar
│   ├── statusbar
│   ├── toolbar
│   └── toolpanel
├── content.xml
├── manifest.rdf
├── META-INF
│   └── manifest.xml
├── meta.xml
├── mimetype
├── Pictures
│   ├── 100002010000002000000020015CF5170072DC51.png
│   └── 10000201000000200000002089E508A2555BB893.png
├── settings.xml
├── styles.xml
└── Thumbnails
    └── thumbnail.png
```
We need a file `content.xml`

##Create DOM object
```php
<?php
$domDocument = new DOMDocument;
$domDocument->load("content.xml"); 

//or
$domDocument->loadXml( file_get_contents( "content.xml") ); 
```
#Sheets
```xml
<office:document-content>
  <office:scripts/>
  <office:font-face-decls>...</office:font-face-decls>
  <office:automatic-styles>...</office:automatic-styles>
  <office:body>
    <office:spreadsheet>
      <table:calculation-settings table:automatic-find-labels="false"/>
      <table:table table:name="Sheet1" table:style-name="ta1">...</table:table>
      <table:table table:name="Sheet2" table:style-name="ta1">...</table:table>
      <table:table table:name="Sheet3" table:style-name="ta1">...</table:table>
      <table:named-expressions>...</table:named-expressions>
    </office:spreadsheet>
  </office:body>
</office:document-content>
```
This document contain 3 sheets in sections `table:table`
* `Sheet1`
* `Sheet2`
* `Sheet3`

##Access to sheets
```php
<?php
$sheets = $domDocument->getElementsByTagName('table');

//first sheet
$first_sheet = $sheets->item(0);

//or

$first_sheet = $domDocument                                              
  ->getElementsByTagName('table')                           
  ->item(0)
  
$sheet_name = $first_sheet->getAttribute('table:name'); 
```
##Named expressions
```php
<?php
$named_expressions = $domDocument->getElementsByTagName('named-expressions');
```
```xml
<table:named-expressions>
  <table:named-range 
    table:name="GlobalCycleData1" 
    table:base-cell-address="$Sheet1.$A$2" 
    table:cell-range-address="$Sheet1.$A$15:.$E$15" 
    table:range-usable-as="repeat-row"/>
  <table:named-range 
    table:name="LocalNameSxpression" 
    table:base-cell-address="$Sheet1.$A$19" 
    table:cell-range-address="$Sheet1.$A$20:.$E$20" 
    table:range-usable-as="repeat-column"/>
  <table:named-range 
    table:name="NewCycle" 
    table:base-cell-address="$Sheet1.$A$22" 
    table:cell-range-address="$Sheet1.$A$19:.$AMJ$22" 
    table:range-usable-as="repeat-row repeat-column print-range filter"/>
  <table:named-range 
    table:name="DocumentData" 
    table:base-cell-address="$Sheet1.$A$8" 
    table:cell-range-address="$Sheet1.$A$8:.$AMJ$8" 
    table:range-usable-as="repeat-row"/>
</table:named-expressions>
```

#Table is a sheet
```xml
<table:table table:name="Sheet1" table:style-name="ta1">
  <table:table-header-columns>...</table:table-header-columns>
  <table:table-column table:style-name="co1"table:default-cell-style-name="Default"/>
  <table:table-row table:style-name="ro1">...</table:table-row>
  <table:table-row table:style-name="ro2" table:number-rows-repeated="10">...</table:table-row>
  <table:table-row table:style-name="ro2">...</table:table-row>
  <table:table-row table:style-name="ro2">...</table:table-row>
  <table:table-row table:style-name="ro2">...</table:table-row>
  <table:table-row table:style-name="ro2">...</table:table-row>
  <table:table-row table:style-name="ro2">...</table:table-row>
</table:table>
```
#Rows
```php
<?php
$sheet_rows = $sheet->getElementsByTagName('table-row');

//or first sheet

$sheet_rows = $domDocument                                              
  ->getElementsByTagName('table')                           
  ->item(0)
  ->getElementsByTagName('table-row');
```
##Header row aria
It is used in the printed area diapason. In the table
```xml
<table:table table:name="Sheet1" table:style-name="ta1">
    <table:table-column table:style-name="co2" table:default-cell-style-name="Default"/>
    <table:table-column table:style-name="co3" table:default-cell-style-name="Default"/>
    <table:table-row table:style-name="ro1">...</table:table-row>
    <table:table-row table:style-name="ro1">...</table:table-row>
    <table:table-header-rows>...</table:table-header-rows>
    <table:table-row table:style-name="ro1">...</table:table-row>
    <table:table-row table:style-name="ro1">...</table:table-row>
</table:table>
```
Header rows
```xml
    <table:table-header-rows>
        <table:table-row table:style-name="ro1">
            <table:table-cell table:style-name="ce2" office:value-type="string" calcext:value-type="string">...</table:table-cell>
            <table:table-cell table:style-name="ce2" office:value-type="string" calcext:value-type="string">...</table:table-cell>
            <table:table-cell table:style-name="ce2" office:value-type="string" calcext:value-type="string">...</table:table-cell>
            <table:table-cell table:style-name="ce2" office:value-type="string" calcext:value-type="string">...</table:table-cell>
        </table:table-row>
    </table:table-header-rows>
```

##Repeat rows
```xml
<table:table-row 
      table:style-name="ro2" 
      table:number-rows-repeated="10"
      >
  <table:table-cell table:number-columns-repeated="1024"/>
</table:table-row>
```

#Cells
```xml
<table:table-row table:style-name="ro2">
  <table:table-cell table:style-name="ce4" office:value-type="string" calcext:value-type="string">
    <text:p>[modelName.text]</text:p>
  </table:table-cell>
  <table:table-cell table:style-name="ce4" office:value-type="string" calcext:value-type="string">
    <text:p>[model2.id]</text:p>
  </table:table-cell>
  <table:table-cell table:style-name="ce4" office:value-type="string" calcext:value-type="string">
    <text:p>[count]</text:p>
  </table:table-cell>
  <table:table-cell table:style-name="ce4" office:value-type="string" calcext:value-type="string">
    <text:p>[sum]</text:p>
  </table:table-cell>
  <table:table-cell table:style-name="ce6" office:value-type="float" office:value="1234.54" calcext:value-type="float">
    <text:p>1 234,54</text:p>
  </table:table-cell>
  <table:table-cell table:style-name="CycleRow" table:number-columns-repeated="1019"/>
</table:table-row>
```
##Spanned cells
```xml
<table:table-row table:style-name="ro2">
  <table:table-cell 
    table:style-name="ce2" 
    office:value-type="string" 
    calcext:value-type="string" 
    table:number-columns-spanned="2" 
    table:number-rows-spanned="5">
  <text:p>5 row span and 2 col span</text:p>
  </table:table-cell>
  <table:covered-table-cell/>
  <table:table-cell table:number-columns-repeated="1022"/>
</table:table-row>
<table:table-row table:style-name="ro2">
  <table:covered-table-cell table:number-columns-repeated="2"/>
  <table:table-cell table:number-columns-repeated="1022"/>
</table:table-row>
<table:table-row table:style-name="ro2">
  <table:covered-table-cell table:number-columns-repeated="2"/>
  <table:table-cell table:number-columns-repeated="1022"/>
</table:table-row>
<table:table-row table:style-name="ro2">
  <table:covered-table-cell table:number-columns-repeated="2"/>
  <table:table-cell table:number-columns-repeated="1022"/>
</table:table-row>
<table:table-row table:style-name="ro2">
  <table:covered-table-cell table:number-columns-repeated="2"/>
  <table:table-cell table:number-columns-repeated="1022"/>
</table:table-row>
```
##Hidden (covered) cells
This cells is under the spanned aria
```xml
<table:covered-table-cell table:number-columns-repeated="2"/>
```
or
```xml
<table:covered-table-cell/>
```
# Cell types
[OASIS Open Document manual: 19.385 office:value-type](http://docs.oasis-open.org/office/v1.2/os/OpenDocument-v1.2-os-part1.html#__RefHeading__1417680_253892949)

##String cell
attribute          |value
-------------------|------
office:value-type  | string
calcext:value-type | string

```xml
<table:table-cell 
      table:style-name="ce4" 
      office:value-type="string" 
      calcext:value-type="string">
  <text:p>String text</text:p>
</table:table-cell>
```

##Real number cells

attribute          | value   | comment
-------------------|---------|--------
office:value-type  | float   |
office:value       | 1234.54 | Only with dot, without comma or spaces ~~1 234,54~~
calcext:value-type | float   |

```xml
<table:table-cell 
        table:style-name="ce6" 
        office:value-type="float" 
        office:value="1234.54" 
        calcext:value-type="float">
    <text:p>1 234,54</text:p>
</table:table-cell>
```
##Boolean cell
```xml
<table:table-cell 
        table:style-name="ce7" 
        office:value-type="boolean" 
        office:boolean-value="true" 
        calcext:value-type="boolean">
    <text:p>TRUE</text:p>
</table:table-cell>
```
## Cell annotation
```xml
<table:table-cell table:style-name="ce11" office:value-type="string" calcext:value-type="string">
    <office:annotation draw:style-name="gr3" draw:text-style-name="P3" svg:width="65.42mm" svg:height="9.91mm" svg:x="125.3mm" svg:y="26.41mm" draw:caption-point-x="-6.1mm" draw:caption-point-y="15.1mm">
        <dc:date>2016-09-23T00:00:00</dc:date>
        <text:p text:style-name="P2">Annotation text there</text:p>
    </office:annotation>
    <text:p>Cell value</text:p>
</table:table-cell>
```

##Format text cell
```xml
<table:table-cell office:value-type="string" calcext:value-type="string">
    <text:p>
        normal
        <text:span text:style-name="T3">italic</text:span>
        <text:span text:style-name="T1">bold</text:span>
        <text:span text:style-name="T2">undersore</text:span>
    </text:p>
    <text:p>
        Line 2
        <text:span text:style-name="T1">bold</text:span>
    </text:p>
    <text:p>
        Line 3
        <text:span text:style-name="T3">italic</text:span>
    </text:p>
</table:table-cell>
```
##Cell Formula
```xml
<table:table-cell 
        table:formula="of:=[.E10]*[.D10]" 
        office:value-type="string" 
        office:string-value="" 
        calcext:value-type="error">
    <text:p>#ЗНАЧЕН!</text:p>
</table:table-cell>
```

#META-INF/manifest.xml
All files must be list there
```xml
<manifest:manifest xmlns:manifest="urn:oasis:names:tc:opendocument:xmlns:manifest:1.0" manifest:version="1.2">
    <manifest:file-entry manifest:full-path="/" manifest:version="1.2" manifest:media-type="application/vnd.oasis.opendocument.spreadsheet"/>
    <manifest:file-entry manifest:full-path="Thumbnails/thumbnail.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="settings.xml" manifest:media-type="text/xml"/>
    <manifest:file-entry manifest:full-path="Pictures/100002010000010000000100CEAC010BC7EB06A3.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/100002010000001600000016E597C3BB6B89CEFE.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/100002010000001600000016B7AA301CC3D427AE.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/1000020100000030000000308340316B7B9D5CCC.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="content.xml" manifest:media-type="text/xml"/>
    <manifest:file-entry manifest:full-path="meta.xml" manifest:media-type="text/xml"/>
    <manifest:file-entry manifest:full-path="styles.xml" manifest:media-type="text/xml"/>
    <manifest:file-entry manifest:full-path="Configurations2/accelerator/current.xml" manifest:media-type=""/>
    <manifest:file-entry manifest:full-path="Configurations2/" manifest:media-type="application/vnd.sun.xml.ui.configuration"/>
    <manifest:file-entry manifest:full-path="manifest.rdf" manifest:media-type="application/rdf+xml"/>
    <manifest:file-entry manifest:full-path="Pictures/acroread.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/video-display.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/.directory" manifest:media-type="text/plain"/>
    <manifest:file-entry manifest:full-path="Pictures/utilities-file-archiver.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/ktip.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/application-x-zerosize.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/kmplayer.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/preferences-desktop-cryptography.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/application-x-smb-server.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/im-status-message-edit.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/view-pim-tasks.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/application-x-smb-workgroup.png" manifest:media-type="image/png"/>
    <manifest:file-entry manifest:full-path="Pictures/view-pim-notes.png" manifest:media-type="image/png"/>
</manifest:manifest>
```

#Images with anchors
achnor | place `...`
-------|-----
page   | `<table:table><table:shapes>...</table:shapes></table:table>`
cell   | `<table:table><table:table-row><table:table-cell>...</table:table-cell>`

```xml
<table:table table:name="Sheet1" table:style-name="ta1">
  <table:shapes>
    <draw:frame draw:name="Picture page anchor" draw:z-index="0" draw:style-name="gr1" draw:text-style-name="P1" svg:width="8.8mm" svg:height="8.49mm" svg:x="13.21mm" svg:y="4.51mm">
      <draw:image xlink:href="Pictures/100002010000002000000020015CF5170072DC51.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad">
        <text:p/>
      </draw:image>
    </draw:frame>
  </table:shapes>
  <table:table-column table:style-name="co1" table:number-columns-repeated="2" table:default-cell-style-name="Default"/>
  <table:table-row table:style-name="ro1" table:number-rows-repeated="3">...</table:table-row>
  <table:table-row table:style-name="ro1">
    <table:table-cell/>
    <table:table-cell>
      <draw:frame draw:name="Picture cell anchor" table:end-cell-address="Sheet1.B6" table:end-x="16.79mm" table:end-y="0.53mm" draw:z-index="1" draw:style-name="gr1" draw:text-style-name="P1" svg:width="8.8mm" svg:height="8.49mm" svg:x="7.99mm" svg:y="1.07mm">
        <draw:image xlink:href="Pictures/10000201000000200000002089E508A2555BB893.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad">
          <text:p/>
        </draw:image>
      </draw:frame>
    </table:table-cell>
  </table:table-row>
</table:table>
```
One draw frame
```xml
<draw:frame 
    table:end-cell-address="Sheet1.B6" 
    table:end-x="16.79mm" 
    table:end-y="0.53mm" 
    draw:z-index="1" 
    draw:name="Picture cell anchor" 
    draw:style-name="gr2" 
    draw:text-style-name="P1" 
    svg:width="8.8mm" 
    svg:height="8.49mm" 
    svg:x="7.99mm" 
    svg:y="1.07mm">
  <draw:image 
      xlink:href="Pictures/10000201000000200000002089E508A2555BB893.png" 
      xlink:type="simple" 
      xlink:show="embed" 
      xlink:actuate="onLoad">
    <text:p/>
  </draw:image>
</draw:frame>
```
and styles block: 
* `gr1`
* `gr2`

```xml
<office:automatic-styles>
  <style:style style:name="co1" style:family="table-column">...</style:style>
  <style:style style:name="ro1" style:family="table-row">...</style:style>
  <style:style style:name="ta1" style:family="table" style:master-page-name="Default">...</style:style>
  <style:style style:name="gr1" style:family="graphic">
    <style:graphic-properties draw:stroke="none" 
      draw:fill="none" 
      draw:textarea-horizontal-align="center" 
      draw:textarea-vertical-align="middle" 
      draw:color-mode="standard" 
      draw:luminance="0%" 
      draw:contrast="0%" 
      draw:gamma="100%" 
      draw:red="0%" 
      draw:green="0%" 
      draw:blue="0%" 
      fo:clip="rect(0mm, 0mm, 0mm, 0mm)" 
      draw:image-opacity="100%" 
      style:mirror="none"/>
  </style:style>
  <style:style style:name="gr2" style:family="graphic">
    <style:graphic-properties 
      draw:stroke="none" 
      draw:fill="none" 
      draw:textarea-horizontal-align="center" 
      draw:textarea-vertical-align="middle" 
      draw:color-mode="standard" 
      draw:luminance="0%" 
      draw:contrast="0%" 
      draw:gamma="100%" 
      draw:red="0%" 
      draw:green="0%" 
      draw:blue="0%" 
      fo:clip="rect(0mm, 0mm, 0mm, 0mm)" 
      draw:image-opacity="100%" 
      style:mirror="none" 
      style:protect="size"/>
  </style:style>
  <style:style style:name="P1" style:family="paragraph">...</style:style>
</office:automatic-styles>
```
### Image with description
```xml
<draw:frame table:end-cell-address="Sheet1.E23" table:end-x="27.94mm" table:end-y="1.39mm" draw:z-index="1" draw:name="ImageDisabledDB" draw:style-name="gr1" draw:text-style-name="P1" svg:width="18.94mm" svg:height="19.45mm" svg:x="9mm" svg:y="0.01mm">
    <draw:image xlink:href="Pictures/100002010000008000000080CDFEB8A6BBCE3321.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad">
        <text:p/>
    </draw:image>
    <svg:title>conditions</svg:title>
    <svg:desc>
        !empty($data['Report']['disabled']) AND $data['Report']['disabled'] === true
    </svg:desc>
</draw:frame>
```
### Image with 0x0 pixels
Attributes `svg:width` and `svg:height` set to `0.01mm` and image be a hiding from page
```xml
<draw:frame draw:z-index="2" draw:name="ImageNULL" draw:style-name="gr1" draw:text-style-name="P1" 
    svg:width="0.01mm" 
    svg:height="0.01mm" 
    svg:x="135.46mm" 
    svg:y="103.58mm">
        <draw:image xlink:href="Pictures/100002010000008000000080CDFEB8A6BBCE3321.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad">
        <text:p/>
        </draw:image>
</draw:frame>
```
### Delete image by name
Name of picture in LibreOffice or OpenOffice set up by GUI in Calc
```php
<?php
$image_name = "Picture 1";
foreach ($domDocument->getElementsByTagName('frame') as $frame){
    if ($frame->getAttribute('draw:name') == $image_name){
        $frame->parentNode->removeChild($frame);
    }
}   
```

