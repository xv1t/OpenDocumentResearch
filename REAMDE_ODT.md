#ODT
LibreOffice/OpenOffice Writer files

#Main schema of the content
```xml
<office:document-content xmlns:office="urn:oasis:names:tc:opendocument:xmlns:office:1.0" office:version="1.2">
  <office:scripts/>
  <office:font-face-decls>...</office:font-face-decls>
  <office:automatic-styles>...</office:automatic-styles>
  <office:body>
    <office:text text:use-soft-page-breaks="true">
      <text:sequence-decls>...</text:sequence-decls>
      <text:p text:style-name="P1">Page one. First line</text:p>
      <table:table table:name="MyTable1" table:style-name="MyTable1">...</table:table>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1">...</text:p>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1"/>
      <text:p text:style-name="P1">...</text:p>
      <text:p text:style-name="P3">Header</text:p>
      <text:h text:style-name="P8" text:outline-level="1">Header 1</text:h>
      <text:p text:style-name="P4">...</text:p>
      <text:p text:style-name="P5">right align</text:p>
      <text:list xml:id="list563510848885106870" text:style-name="L1">...</text:list>
      <text:p text:style-name="P6"/>
      <text:p text:style-name="P2">Page two</text:p>
      <text:p text:style-name="P2">Page three</text:p>
    </office:text>
  </office:body>
</office:document-content>
```

## Para
```xml
<text:p text:style-name="P1">Page one. First line</text:p>
```

### Empty para
Empty line
```xml
<text:p text:style-name="P1"/>
```

## Format text
```xml
<text:p text:style-name="P4">
  Text text.
  <text:span text:style-name="T1">Bold</text:span>
  <text:span text:style-name="T3">italic</text:span>
  <text:span text:style-name="T4">. H</text:span>
  <text:span text:style-name="T5">2</text:span>
  <text:span text:style-name="T4">O. cm</text:span>
  <text:span text:style-name="T6">3</text:span>
</text:p>
```

## Tables
```xml
<table:table table:name="MyTable1" table:style-name="MyTable1">
  <table:table-column table:style-name="MyTable1.A" table:number-columns-repeated="2"/>
  <table:table-row>
    <table:table-cell table:style-name="MyTable1.A1" office:value-type="string">
    <text:p text:style-name="Table_20_Contents"/>
    </table:table-cell>
    <table:table-cell table:style-name="MyTable1.B1" office:value-type="string">...</table:table-cell>
  </table:table-row>
  <table:table-row>...</table:table-row>
</table:table>
```

## List
```xml
<text:list xml:id="list563510848885106870" text:style-name="L1">
  <text:list-item>
    <text:p text:style-name="P7">List item A</text:p>
  </text:list-item>
  <text:list-item>
    <text:p text:style-name="P7">List item B</text:p>
  </text:list-item>
  <text:list-item>
    <text:p text:style-name="P7">List item C</text:p>
  </text:list-item>
</text:list>
```

## Images
```xml
<text:p text:style-name="P1">
  <draw:frame draw:style-name="fr2" draw:name="Изображение2" text:anchor-type="as-char" svg:width="3.614cm" svg:height="3.614cm" draw:z-index="1">
    <draw:image xlink:href="Pictures/1000020100000030000000308FCEF0E25987FC0B.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad"/>
  </draw:frame>
</text:p>
```

### Image anchor as char
```xml
<style:style style:name="fr2" style:family="graphic" style:parent-style-name="Graphics">
  <style:graphic-properties style:vertical-pos="top" style:vertical-rel="baseline" style:horizontal-pos="center" style:horizontal-rel="paragraph" style:mirror="none" fo:clip="rect(0cm, 0cm, 0cm, 0cm)" draw:luminance="0%" draw:contrast="0%" draw:red="0%" draw:green="0%" draw:blue="0%" draw:gamma="100%" draw:color-inversion="false" draw:image-opacity="100%" draw:color-mode="standard"/>
</style:style>
```
Attribute `text:anchor-type` = `as-char`
```xml
<text:p text:style-name="P1">
  <draw:frame draw:style-name="fr2" draw:name="Изображение2" text:anchor-type="as-char" svg:width="3.614cm" svg:height="3.614cm" draw:z-index="1">
    <draw:image xlink:href="Pictures/1000020100000030000000308FCEF0E25987FC0B.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad"/>
  </draw:frame>
</text:p>
```

## Image anchor to para
Attribute `text:anchor-type` = `paragraph`
```xml
<text:p text:style-name="P1">
  <draw:frame draw:style-name="fr1" draw:name="Изображение1" text:anchor-type="paragraph" svg:width="0.903cm" svg:height="0.903cm" draw:z-index="0">
    <draw:image xlink:href="Pictures/100002010000003000000030D32A68C399F720AB.png" xlink:type="simple" xlink:show="embed" xlink:actuate="onLoad"/>
  </draw:frame>
  Para1
</text:p>
```


## Page breaks
Styles section `P2`
```xml
<office:automatic-styles>
  ...
  <style:style style:name="P2" style:family="paragraph" style:parent-style-name="Standard">
    <style:paragraph-properties fo:break-before="page"/>
    <style:text-properties officeooo:rsid="00054d71" officeooo:paragraph-rsid="00054d71"/>
  </style:style>
  ...
</office:automatic-styles>
```

Para with `P2`
```xml
<text:p text:style-name="P2">Page three</text:p>
```
