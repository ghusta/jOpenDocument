# jOpenDocument for

This is fork of jOpenDocument project from http://www.jopendocument.org/

## Change list

### 1.4 rc 3
#### Added
- OpenDocument v1.3

### 1.4 rc 2
#### Added
- Span class
- ODNodeDesc class
- Style.resolveReference()

#### Improved
- ODPackage.validateSubDocuments() now handles extended documents (OpenDocument v1.2 §2.2.2)
- TextNode no longer restricted to TextDocument
- Support for multiple lines in cells

#### Fixed
- Remove hard limit for ODXMLDocument.findUnusedName(), now print a warning
- Handles Vertical Tab, see TextNode.VERTICAL_TAB_CHAR

### 1.4 rc 1

#### Added
- Cell.getFormulaAndNamespace() and MutableCell.setFormula().
- Class Length to efficiently and accurately handle computations and conversion between different units.

#### Improved
- Google Drive compatibility : sheets now always have display=true ; use centimetres by default for lengths.
- LibreOffice compatibility : by default no longer include years, months or days for durations. See MutableCell.getTimeValueMode().
- Framework base classes (XMLVersion, ODValueType, Style, etc) are now thread safe. This allows 2 threads to use 2 documents concurrently, but one document still cannot be used by 2 threads. 
- ODPackage.validateSubDocuments() now also validates the manifest.

#### Fixed
- Handle missing version in documents as it's optional in OpenDocument 1.0 and 1.1 (e.g. Microsoft Office 2007 and 2010 support v1.1 and omit it)
- No longer write time zone part, allow to configure the framework to behave like LO<4.1 or LO>=4.1 with regards to parsing dates with time zone (see ODValueType.isTimeZoneIgnored()).
See also FixTimeZone for existing documents.
- Column.setWidth() now updates the width of its table.
- Handle Range with only one cell.

#### API changed (incompatible with previous revision):
- Cell.getFormula() now throws an exception if the name space isn't OpenFormula. The old behaviour is available as getRawFormula()
- A lot of style classes now return Length instead of Float or BigDecimal.

### 1.3
#### Added:
- Table.removeRows()
- getRangesNames() and getRange() in SpreadSheet and Table

#### Fixed:
- Handle times with no seconds in cells with no format
- Remove stale document statistics in meta.xml (since page count cannot be computed)

### 1.3 rc 2
#### Added:
- Table.createColumnStyle(width)
- TextDocument.getCharacterContent()

#### Improved:
- LengthUnit.format() now takes an arbitrary Number instead of BigDecimal (allow to avoid turning 2.357f into 2.3570001125335693)
- Table.setColumnCount() can now take a ColumnStyle (allow to create a column of arbitrary width)
- SpreadSheet.create() adds an automatic table style with display=true (LO always generates one, and Google Docs expects it)

#### Fixed:
- ODSingleXMLDocument.add() correctly orders body children 

### 1.3 rc 1
#### Added:
- Support for opening and saving flat XML with binary data (e.g. pictures)
- SpreadSheet.create(int sheetCount, int colCount, int rowCount)
- StyleStyle.getParentStyle()
- StyleDesc.createCommonStyle() to add styles
- TextDocument.getParagraphCount()
- Scripts (macros) and event listeners are now supported : added ODPackage.readBasicLibraries(), addBasicLibraries(), removeBasicLibraries() and readEventListeners()

#### Improved:
- OOConnexion handles UNC paths.
- OOInstallation handles LibreOffice 3.5 and 3.6.
- Optimized MutableCell.setValue().
- Various getTextValue() methods (using TextNode.getCharacterContent()) now correctly ignore graphical elements anchored to the paragraph.

#### Fixed:
- Cells with conditional styles having values that cannot be evaluated by conditions (e.g. a cell with >=5 condition and 'foo' value).
- Searching for the value of a formatting property (see §16.2 of OpenDocument v1.2, we now look in ancestor styles and in enclosing elements).

#### API changed (incompatible with previous revision):
- ODSingleXMLDocument.saveAs() now saves to a flat XML, use saveToPackageAs() to continue to save to a package.
- All XML entries in ODPackage are now parsed to Document (e.g. Basic/script-lc.xml).
Previously only sub-documents (content.xml, styles.xml...) were parsed, all other entries were byte[].

### 1.3 beta 1
#### Added:
- ODPackage.createFromStream()/createFromFile() handling both package and flat XML files
- Validator to unify JAXP and DTD validation
- XMLFormatVersion to handle differences in office versions
- Instructions in the README on how to validate XML

- Support for row and column groups
- Table.getUsedRange() to find out the range that covers all used cells
- Table.getCurrentRegion() to find out the range containing the passed cell and completely surrounded by empty rows and columns
- Cell.getError() to find out if a computation resulted in an error

- Support for default-style
- Support for data styles, MutableCell now correctly formats its value (including parsing of conditions)


#### Improved:
- added POINT and PICA in LengthUnit
- handle arbitrary table name (i.e. quotes)
- Table.duplicateRows() can now update shapes coordinates and handle merged cells
- performance: no longer expand repeated rows and lots of other small improvements
- ODDocument is now a superclass of SpreadSheet & TextDocument

#### Fixed:
- ODPackage now saves valid OpenDocument package files (i.e. split flat XML before zipping)
- index bug in Sheet.move()
- handle white space encoding and decoding in cells according to office version


### 1.2

- Table.updateWidth() now supports columns without style
- Fix Sheet.createEmpty()


### 1.2 beta 3

#### Fixed: 
- boolean issue on RhinoTemplate
Improved:
- ODS viewer on cell layout
- when changing cells value reuse the first paragraph to keep style
- ODPackage.getStyle() handles styles with the same name in content.xml and styles.xml
- ODPackage now keeps the entry method (compressed or not) 
- various speed optimizations
#### Added: 
- Table.getHeaderRowCount() and getHeaderColumnCount()
- Sheet.move() & .copy()
- Cell.getValueType() public as requested 
- Cell.getTextValue()
- MutableCell.merge()
- Frames can now be found using getDescendantByName()
- LengthUnit to handle conversions
- GraphicStyle, SyleGraphicProperties and SyleTableCellProperties
- StyledNode.getPrivateStyle() which allow to safely modify any style property by duplicating it if necessary 
#### API changed (incompatible with previous revision):
- numeric values are now mapped to BigDecimal instead of Float for more precision 
- use java.awt.Color instead of String for color properties
- renamed NS to XMLVersion and made it an enum

### 1.2 beta 2
- build system fixed (jdk 5 compilation & broken properties)
- code cleaning
- mimetype attribute added in spreadsheets
- fixed: timezone format


### 1.2 beta 1
- name accessors for spreadsheets
- add/remove table from text documents and sheet from spreadsheet documents
- getTableModel() on a named range (see Modify an existing spreadsheet)
- unmerge on cells
- remove columns
- improved cell styles
- paragraph and heading creation for text documents
- various fixes and performance improvements

### 1.1

Small bug fixes in JavaScriptFileTemplate and convenient methods (putAll in DataModel).


### 1.1 beta 3
- a new API for metadata manipulation 
- a new tutorial about metadata
- full support of measure units
- row duplication improvements
- an useful XML validator ( isValid() on XMLDocument )
- a convenient superclass for styles
- an unified table handling and a long awaited setColumnCount() method on Tables
