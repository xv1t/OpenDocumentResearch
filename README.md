# OpenDocumentResearch with PHP XML DOMDocument
#Enter to file
Unzip ods 
```bash
$ unzip file.ods
```

Target file
```bash
"content.xml"
```
Create XML DOM object
```php
<?php
$domDocument = new DOMDocument;
$domDocument->load("content.xml"); 

//or

$domDocument->loadXml( file_get_contents( "content.xml") ); 
```

##Sheets
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
###Access to sheets
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
###Named expressions
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
    table:range-usable-as="repeat-row"/>
</table:named-expressions>
```

##Table is a sheet
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
##Rows
```php
<?php
$sheet_rows = $sheet->getElementsByTagName('table-row');

//or first sheet

$sheet_rows = $domDocument                                              
  ->getElementsByTagName('table')                           
  ->item(0)
  ->getElementsByTagName('table-row');
```
####Repeat rows
```xml
<table:table-row 
      table:style-name="ro2" 
      table:number-rows-repeated="10"
      >
  <table:table-cell table:number-columns-repeated="1024"/>
</table:table-row>
```

##Cells
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
###Spanned cells
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
###Hidden (covered) cells
This cells is under the spanned aria
```xml
<table:covered-table-cell table:number-columns-repeated="2"/>
```
or
```xml
<table:covered-table-cell/>
```

###String cell
```xml
<table:table-cell 
      table:style-name="ce4" 
      office:value-type="string" 
      calcext:value-type="string">
  <text:p>String text</text:p>
</table:table-cell>
```

###Real number cells
String value are mirror of attribute *office:value* and attribute **calcext:value-type** set to "float"
```xml
<table:table-cell 
        table:style-name="ce6" 
        office:value-type="float" 
        office:value="1234.54" 
        calcext:value-type="float">
    <text:p>1 234,54</text:p>
</table:table-cell>
```
##Images with anchors
```xml
<table:table table:name="Sheet1" table:style-name="ta1">
  <table:shapes>
    <draw:frame draw:z-index="0" draw:name="Picture page anchor" draw:style-name="gr1" draw:text-style-name="P1" svg:width="8.8mm" svg:height="8.49mm" svg:x="13.21mm" svg:y="4.51mm">
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
      <draw:frame table:end-cell-address="Sheet1.B6" table:end-x="16.79mm" table:end-y="0.53mm" draw:z-index="1" draw:name="Picture cell anchor" draw:style-name="gr1" draw:text-style-name="P1" svg:width="8.8mm" svg:height="8.49mm" svg:x="7.99mm" svg:y="1.07mm">
        <draw:image xlink:href="Pictures/10000201000000200000002089E508A2555BB893.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad">
          <text:p/>
        </draw:image>
      </draw:frame>
    </table:table-cell>
  </table:table-row>
</table:table>
```
