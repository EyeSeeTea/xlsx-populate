[![view on npm](http://img.shields.io/npm/v/xlsx-populate.svg)](https://www.npmjs.org/package/xlsx-populate)
[![npm module downloads per month](http://img.shields.io/npm/dm/xlsx-populate.svg)](https://www.npmjs.org/package/xlsx-populate)
[![Build Status](https://travis-ci.org/dtjohnson/xlsx-populate.svg?branch=master)](https://travis-ci.org/dtjohnson/xlsx-populate)
[![Dependency Status](https://david-dm.org/dtjohnson/xlsx-populate.svg)](https://david-dm.org/dtjohnson/xlsx-populate)

# xlsx-populate
Node.js module to populate Excel XLSX templates. This module does not parse Excel workbooks. There are [good modules](https://github.com/SheetJS/js-xlsx) for this already. The purpose of this module is to open existing Excel XLSX workbook templates that have styling in place and populate with data.

## Installation

    $ npm install xlsx-populate

## Usage
Here is a basic example:
```js
var Workbook = require('xlsx-populate');

// Load the input workbook from file.
var workbook = Workbook.fromFileSync("./Book1.xlsx");

// Modify the workbook.
workbook.getSheet("Sheet1").getCell("A1").setValue("This is neat!");

// Write to file.
workbook.toFileSync("./out.xlsx");
```

### Getting Sheets
You can get sheets from a Workbook object by either name or index (0-based):
```js
// Get sheet with name "Sheet1".
var sheet = workbook.getSheet("Sheet1");

// Get the first sheet.
var sheet = workbook.getSheet(0);
```

### Getting Cells
You can get a cell from a sheet by either address or row and column:
```js
// Get cell "A5" by address.
var cell = sheet.getCell("A5");

// Get cell "A5" by row and column.
var cell = sheet.getCell(5, 1);
```

You can also get named cells directly from the Workbook:
```js
// Get cell named "Foo".
var cell = sheet.getNamedCell("Foo");
```

### Setting Cell Contents
You can set the cell value or formula:
```js
cell.setValue("foo");
cell.setValue(5.6);
cell.setFormula("SUM(A1:A5)");
```

### Serving from Express
You can serve the workbook with [express](http://expressjs.com/) with a route like this:
```js
router.get("/download", function (req, res) {
    // Open the workbook.
    var workbook = Workbook.fromFile("input.xlsx", function (err, workbook) {
        if (err) return res.status(500).send(err);

        // Make edits.
        workbook.getSheet(0).getCell("A1").setValue("foo");

        // Set the output file name.
        res.attachment("output.xlsx");

        // Send the workbook.
        res.send(workbook.output());
    });
});
```

## Development

### Running Tests
Tests are run automatically on [Travis CI](https://travis-ci.org/dtjohnson/xlsx-populate). They can (and should) be triggered locally with:
    
    $ npm test

### Code Linting
[ESLint](http://eslint.org/)is used to ensure code quality. To run this:

    $ npm run eslint

### Generating Documentation
The API reference documentation below is generated by [jsdoc-to-markdown](https://github.com/jsdoc2md/jsdoc-to-markdown). To generate an updated README.md, run:
    
    $ npm run docs

# API Reference
## Classes

<dl>
<dt><a href="#Cell">Cell</a></dt>
<dd></dd>
<dt><a href="#Row">Row</a></dt>
<dd></dd>
<dt><a href="#Sheet">Sheet</a></dt>
<dd></dd>
<dt><a href="#Workbook">Workbook</a></dt>
<dd></dd>
</dl>

<a name="Cell"></a>

## Cell
**Kind**: global class  

* [Cell](#Cell)
    * [new Cell(row, cellNode)](#new_Cell_new)
    * [.getRow()](#Cell+getRow) ⇒ <code>[Row](#Row)</code>
    * [.getSheet()](#Cell+getSheet) ⇒ <code>[Sheet](#Sheet)</code>
    * [.getAddress()](#Cell+getAddress) ⇒ <code>string</code>
    * [.getRowNumber()](#Cell+getRowNumber) ⇒ <code>number</code>
    * [.getColumnNumber()](#Cell+getColumnNumber) ⇒ <code>number</code>
    * [.getColumnName()](#Cell+getColumnName) ⇒ <code>number</code>
    * [.getFullAddress()](#Cell+getFullAddress) ⇒ <code>string</code>
    * [.setValue(value)](#Cell+setValue) ⇒ <code>[Cell](#Cell)</code>
    * [.setFormula(formula, [calculatedValue])](#Cell+setFormula) ⇒ <code>[Cell](#Cell)</code>

<a name="new_Cell_new"></a>

### new Cell(row, cellNode)
Initializes a new Cell.


| Param | Type | Description |
| --- | --- | --- |
| row | <code>[Row](#Row)</code> | The parent row. |
| cellNode | <code>Element</code> | The cell node. |

<a name="Cell+getRow"></a>

### cell.getRow() ⇒ <code>[Row](#Row)</code>
Gets the parent row of the cell.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>[Row](#Row)</code> - The parent row.  
<a name="Cell+getSheet"></a>

### cell.getSheet() ⇒ <code>[Sheet](#Sheet)</code>
Gets the parent sheet.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>[Sheet](#Sheet)</code> - The parent sheet.  
<a name="Cell+getAddress"></a>

### cell.getAddress() ⇒ <code>string</code>
Gets the address of the cell (e.g. "A5").

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>string</code> - The cell address.  
<a name="Cell+getRowNumber"></a>

### cell.getRowNumber() ⇒ <code>number</code>
Gets the row number of the cell.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>number</code> - The row number.  
<a name="Cell+getColumnNumber"></a>

### cell.getColumnNumber() ⇒ <code>number</code>
Gets the column number of the cell.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>number</code> - The column number.  
<a name="Cell+getColumnName"></a>

### cell.getColumnName() ⇒ <code>number</code>
Gets the column name of the cell.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>number</code> - The column name.  
<a name="Cell+getFullAddress"></a>

### cell.getFullAddress() ⇒ <code>string</code>
Gets the full address of the cell including sheet (e.g. "Sheet1!A5").

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>string</code> - The full address.  
<a name="Cell+setValue"></a>

### cell.setValue(value) ⇒ <code>[Cell](#Cell)</code>
Sets the value of the cell.

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell.  

| Param | Type | Description |
| --- | --- | --- |
| value | <code>\*</code> | The value to set. |

<a name="Cell+setFormula"></a>

### cell.setFormula(formula, [calculatedValue]) ⇒ <code>[Cell](#Cell)</code>
Sets the formula for a cell (with optional pre-calculated value).

**Kind**: instance method of <code>[Cell](#Cell)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell.  

| Param | Type | Description |
| --- | --- | --- |
| formula | <code>string</code> | The formula to set. |
| [calculatedValue] | <code>\*</code> | The pre-calculated value. |

<a name="Row"></a>

## Row
**Kind**: global class  

* [Row](#Row)
    * [new Row(sheet, rowNode)](#new_Row_new)
    * [.getSheet()](#Row+getSheet) ⇒ <code>[Sheet](#Sheet)</code>
    * [.getRowNumber()](#Row+getRowNumber) ⇒ <code>number</code>
    * [.getCell(columnNumber)](#Row+getCell) ⇒ <code>[Cell](#Cell)</code>

<a name="new_Row_new"></a>

### new Row(sheet, rowNode)
Initializes a new Row.


| Param | Type | Description |
| --- | --- | --- |
| sheet | <code>[Sheet](#Sheet)</code> | The parent sheet. |
| rowNode | <code>Element</code> | The row's node. |

<a name="Row+getSheet"></a>

### row.getSheet() ⇒ <code>[Sheet](#Sheet)</code>
Gets the parent sheet.

**Kind**: instance method of <code>[Row](#Row)</code>  
**Returns**: <code>[Sheet](#Sheet)</code> - The parent sheet.  
<a name="Row+getRowNumber"></a>

### row.getRowNumber() ⇒ <code>number</code>
Gets the row number of the row.

**Kind**: instance method of <code>[Row](#Row)</code>  
**Returns**: <code>number</code> - The row number.  
<a name="Row+getCell"></a>

### row.getCell(columnNumber) ⇒ <code>[Cell](#Cell)</code>
Gets the cell in the row with the provided column number.

**Kind**: instance method of <code>[Row](#Row)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell with the provided column number.  

| Param | Type | Description |
| --- | --- | --- |
| columnNumber | <code>number</code> | The column number. |

<a name="Sheet"></a>

## Sheet
**Kind**: global class  

* [Sheet](#Sheet)
    * [new Sheet(workbook, sheetNode, sheetXML)](#new_Sheet_new)
    * [.getWorkbook()](#Sheet+getWorkbook) ⇒ <code>[Workbook](#Workbook)</code>
    * [.getName()](#Sheet+getName) ⇒ <code>string</code>
    * [.setName(name)](#Sheet+setName) ⇒ <code>undefined</code>
    * [.getRow(rowNumber)](#Sheet+getRow) ⇒ <code>[Row](#Row)</code>
    * [.getCell(address)](#Sheet+getCell) ⇒ <code>[Cell](#Cell)</code>
    * [.getCell(rowNumber, columnNumber)](#Sheet+getCell) ⇒ <code>[Cell](#Cell)</code>

<a name="new_Sheet_new"></a>

### new Sheet(workbook, sheetNode, sheetXML)
Initializes a new Sheet.


| Param | Type | Description |
| --- | --- | --- |
| workbook | <code>[Workbook](#Workbook)</code> | The parent workbook. |
| sheetNode | <code>Element</code> | The node defining the sheet metadat in the workbook.xml. |
| sheetXML | <code>Document</code> | The XML defining the sheet data in worksheets/sheetN.xml. |

<a name="Sheet+getWorkbook"></a>

### sheet.getWorkbook() ⇒ <code>[Workbook](#Workbook)</code>
Gets the parent workbook.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  
**Returns**: <code>[Workbook](#Workbook)</code> - The parent workbook.  
<a name="Sheet+getName"></a>

### sheet.getName() ⇒ <code>string</code>
Gets the name of the sheet.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  
**Returns**: <code>string</code> - The name of the sheet.  
<a name="Sheet+setName"></a>

### sheet.setName(name) ⇒ <code>undefined</code>
Set the name of the sheet.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>string</code> | The new name of the sheet. |

<a name="Sheet+getRow"></a>

### sheet.getRow(rowNumber) ⇒ <code>[Row](#Row)</code>
Gets the row with the given number.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  
**Returns**: <code>[Row](#Row)</code> - The row with the given number.  

| Param | Type | Description |
| --- | --- | --- |
| rowNumber | <code>number</code> | The row number. |

<a name="Sheet+getCell"></a>

### sheet.getCell(address) ⇒ <code>[Cell](#Cell)</code>
Gets the cell with the given address.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell.  

| Param | Type | Description |
| --- | --- | --- |
| address | <code>string</code> | The address of the cell. |

<a name="Sheet+getCell"></a>

### sheet.getCell(rowNumber, columnNumber) ⇒ <code>[Cell](#Cell)</code>
Gets the cell with the given row and column numbers.

**Kind**: instance method of <code>[Sheet](#Sheet)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell.  

| Param | Type | Description |
| --- | --- | --- |
| rowNumber | <code>number</code> | The row number of the cell. |
| columnNumber | <code>number</code> | The column number of the cell. |

<a name="Workbook"></a>

## Workbook
**Kind**: global class  

* [Workbook](#Workbook)
    * [new Workbook(data)](#new_Workbook_new)
    * _instance_
        * [.getSheet(sheetNameOrIndex)](#Workbook+getSheet) ⇒ <code>[Sheet](#Sheet)</code>
        * [.getNamedCell(cellName)](#Workbook+getNamedCell) ⇒ <code>[Cell](#Cell)</code>
        * [.output()](#Workbook+output) ⇒ <code>Buffer</code>
        * [.toFile(path, cb)](#Workbook+toFile) ⇒ <code>undefined</code>
        * [.toFileSync(path)](#Workbook+toFileSync) ⇒ <code>undefined</code>
    * _static_
        * [.fromFile(path, cb)](#Workbook.fromFile) ⇒ <code>undefined</code>
        * [.fromFileSync(path)](#Workbook.fromFileSync) ⇒ <code>[Workbook](#Workbook)</code>
        * [.fromBlank(cb)](#Workbook.fromBlank) ⇒ <code>undefined</code>
        * [.fromBlankSync()](#Workbook.fromBlankSync) ⇒ <code>[Workbook](#Workbook)</code>

<a name="new_Workbook_new"></a>

### new Workbook(data)
Initializes a new Workbook.


| Param | Type | Description |
| --- | --- | --- |
| data | <code>Buffer</code> | File buffer of the Excel workbook. |

<a name="Workbook+getSheet"></a>

### workbook.getSheet(sheetNameOrIndex) ⇒ <code>[Sheet](#Sheet)</code>
Gets the sheet with the provided name or index (0-based).

**Kind**: instance method of <code>[Workbook](#Workbook)</code>  
**Returns**: <code>[Sheet](#Sheet)</code> - The sheet, if found.  

| Param | Type | Description |
| --- | --- | --- |
| sheetNameOrIndex | <code>string</code> &#124; <code>number</code> | The sheet name or index. |

<a name="Workbook+getNamedCell"></a>

### workbook.getNamedCell(cellName) ⇒ <code>[Cell](#Cell)</code>
Get a named cell. (Assumes names with workbook scope pointing to single cells.)

**Kind**: instance method of <code>[Workbook](#Workbook)</code>  
**Returns**: <code>[Cell](#Cell)</code> - The cell, if found.  

| Param | Type | Description |
| --- | --- | --- |
| cellName | <code>string</code> | The name of the cell. |

<a name="Workbook+output"></a>

### workbook.output() ⇒ <code>Buffer</code>
Gets the output.

**Kind**: instance method of <code>[Workbook](#Workbook)</code>  
**Returns**: <code>Buffer</code> - A node buffer for the generated Excel workbook.  
<a name="Workbook+toFile"></a>

### workbook.toFile(path, cb) ⇒ <code>undefined</code>
Writes to file with the given path.

**Kind**: instance method of <code>[Workbook](#Workbook)</code>  

| Param | Type | Description |
| --- | --- | --- |
| path | <code>string</code> | The path of the file. |
| cb | <code>function</code> | A callback. |

<a name="Workbook+toFileSync"></a>

### workbook.toFileSync(path) ⇒ <code>undefined</code>
Writes to file with the given path synchronously.

**Kind**: instance method of <code>[Workbook](#Workbook)</code>  

| Param | Type | Description |
| --- | --- | --- |
| path | <code>string</code> | The path of the file. |

<a name="Workbook.fromFile"></a>

### Workbook.fromFile(path, cb) ⇒ <code>undefined</code>
Creates a Workbook from the file with the given path.

**Kind**: static method of <code>[Workbook](#Workbook)</code>  

| Param | Type | Description |
| --- | --- | --- |
| path | <code>string</code> | The path of the file. |
| cb | <code>function</code> | A callback with the new workbook. |

<a name="Workbook.fromFileSync"></a>

### Workbook.fromFileSync(path) ⇒ <code>[Workbook](#Workbook)</code>
Creates a Workbook from the file with the given path synchronously.

**Kind**: static method of <code>[Workbook](#Workbook)</code>  
**Returns**: <code>[Workbook](#Workbook)</code> - The parsed workbook.  

| Param | Type | Description |
| --- | --- | --- |
| path | <code>string</code> | The path of the file. |

<a name="Workbook.fromBlank"></a>

### Workbook.fromBlank(cb) ⇒ <code>undefined</code>
Creates a blank Workbook.

**Kind**: static method of <code>[Workbook](#Workbook)</code>  

| Param | Type | Description |
| --- | --- | --- |
| cb | <code>function</code> | A callback with the new workbook. |

<a name="Workbook.fromBlankSync"></a>

### Workbook.fromBlankSync() ⇒ <code>[Workbook](#Workbook)</code>
Creates a blank Workbook synchronously.

**Kind**: static method of <code>[Workbook](#Workbook)</code>  
**Returns**: <code>[Workbook](#Workbook)</code> - The new workbook.  
