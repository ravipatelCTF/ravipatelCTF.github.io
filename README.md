# Hello I am Ravi
I am 3rd year BCA student. I like to build software products.
I have experience building REST APIs using spring boot framework.

## Contacts
## Projects
## Education

The Google Sheets API is a RESTful interface that lets you read and modify a spreadsheet's data. The Sheets API lets you:

* Create spreadsheets
* Read and write spreadsheet cell values
* Update spreadsheet formatting
* Manage Connected Sheets

The following is a list of common terms used in the Sheets API:

#### Spreadsheet
* The primary object in Google Sheets. It can contain multiple Sheets, each with structured information contained in Cells. The spreadsheets resource represents a spreadsheet. It contains a unique spreadsheetId value.

##### Spreadsheet ID
* The unique identifier for a spreadsheet. It's a particular string containing letters, numbers, and some special characters that reference a spreadsheet and it can be derived from the spreadsheet's URL. Spreadsheet IDs are stable, even if the spreadsheet name changes.

`https://docs.google.com/spreadsheets/d/SPREADSHEET_ID/edit?gid=SHEET_ID#gid=SHEET_ID`

#### Sheet
* A page or tab within a spreadsheet. The Sheets resource represents a sheet. It contains a unique numeric sheetId value and sheet title as part of the SheetProperties object.

##### Sheet ID
* The unique identifier for a specific sheet within a spreadsheet. It's a particular string containing letters, numbers, and some special characters that reference a sheet and it can be derived from the spreadsheet's URL. Sheet IDs are stable, even if the sheet name changes. For an example, see Spreadsheet ID.
Cell
An individual field of text or data within a sheet. Cells are arranged in rows and columns, and can be grouped as a range of cells. The Cells resource represents each cell, but it doesn't have a unique ID value. Instead, row and column coordinates identify the cells.

A1 notation
A syntax used to define a cell or range of cells with a string that contains the sheet name plus the starting and ending cell coordinates using column letters and row numbers. This method is the most common and useful when referencing an absolute range of cells.
Show examples
R1C1 notation
A syntax used to define a cell or range of cells with a string that contains the sheet name plus the starting and ending cell coordinates using row numbers and column numbers. This method is less common than A1 notation, but can be useful when referencing a range of cells relative to a given cell's position.
Show examples
Named range
A defined cell or range of cells with a custom name to simplify references throughout an application. A FilterView resource represents a named range.
Protected range
A defined cell or range of cells that cannot be modified. A ProtectedRange resource represents a protected range.
Related topics
To learn about developing with Google Workspace APIs, including handling authentication and authorization, refer to Develop on Google Workspace.

To learn how to configure and run a Sheets API app, try the JavaScript quickstart.

