# OpenDocumentResearch with PHP XML DOMDocument
#Enter to file
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
## Cell types
[OASIS Open Document manual: 19.385 office:value-type](http://docs.oasis-open.org/office/v1.2/os/OpenDocument-v1.2-os-part1.html#__RefHeading__1417680_253892949)

###String cell
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

