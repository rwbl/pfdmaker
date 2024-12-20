# DEVNOTES - Project PFDCreator
The PFDCreator is developed with B4J v10.0 and Java JDK 19.0.2.

## Intro
These are development notes for the Process Flow Diagram (aka PFD) Creator application (PFDCreator) and contains

* Conceptual Design
* Process Flow Diagram Definition: How a PFD flow is defined & maintained.
* Process Flow Diagram Shape Definition: How the PFD shape types and associated shapes are defined & maintained.

## Conceptual Design
The PFDCreator application is a B4XPages application with a single mainpage (the B4XMainPage with MainPage layout).
The mainpage is responsible for handling the creation of PFDs and drawing on the PFD Drawing Board (an ASyncCanvas object embedded in a pane).

The PFDBase class defines and handles the PFDObject (TPFDObject which is a type with many object properties) and redraws the PFD Drawing Board.
The PFDObject is key as being used throughout the application, to create, change and draw objects, export or create own objects.

The PFDCommon code module defines the global constants, fonts, PFD Drawing Board grid&scale plus common drawing helpers used by the various shape type code modules.

The shapes are drawn from modules defined in dedicated shape type code modules:
* PFDShapesBasic - Basic shapes like arrows, lines, rectangles
* PFDShapesFlowchart - Flow chart specific shapes, like terminator, process, decision, document
* PFDShapesPID - Process Instrumentation & Piping shapes, like indicator, instrument, pump, Valve
* PFDShapesElectrical - Electrical shapes, like resistor, diode = just a few as a starter

A shape module template is available, to create own shape modules.

Each shape module has the same setup, by defining the shapes as globals, draw the shapes from the list and draw a shape.
The shape method (subroutine) to draw a shape has for all shapes the same definition with parameter DrawSHAPENAME(cvs as ASyncCanvas, obj as TPFDObject).
The shape types with properties are defined in an external JSON structured file PFDCreator.shp.
This file is used to populate the mainpage Shape Selector and for inserting an object with its shape = defaults are set.

Each PFD is stored in an external JSON structured file containing the object defintions (same as used in the shape defintion file, but with objects values set in the mainpage object properties.

The code module "Helper" contains public helper methods which are used by all modules.
This module can also be used by other application, but check out the dependency on the PFDCommon code module (constants).

## Process Flow Diagram
Bevor outlining, just a word of clarity:
A Process Flow Diagram aka PFD contains objects which are shapes with properties.
The properties define the object shape position, colors, text & format, but also its value, unit, operate flag and more.

## Process Flow Diagram Definition
A PFD is defined as JSON format in an external file located in the application folder.
The JSON structure contains the PFD type and an array of objects with properties.
When selecting, the PFD the structure is parsed to create the PFD with the objects.
Each object has a unique index which is set automatically when redrawing the PFD after add or changing an object.
The object properties are the same for all object, but set/used depending of the object.
For example:
The DECISION type can not have a direction - set to NONE, whereas the LINESOLID has a direction HORIZONTAL or VERTICAL.

### Example PFD Definition File Type Flowchart
An example of a simple process flow diagram with objects.
This example PFD has 4 objects:
```
terminator (1) > process (2) > terminator (3) connected with solid horizontal line (4).
```

File: flowchartsimple.pfd located in the application folder.
```
{
"pfddescription":"Simple flowchart example",
"pfdlastchange":"11/20/2024 09:43:11",
"pfdbackgroundcolor":"FFE0E0E0",
"objects":
[
{"index":0,"pfdobject":"PROCESS","tag":"PROCESS-MAIN","description":"Process","value":"Process","unit":"","direction":"NONE","arrow":"NONE","image":"","centerx":300,"centery":130,"width":100,"height":40,"x1":250,"y1":110,"x2":350,"y2":110,"font":"DEFAULT","fontsize":12,"fillcolor":"FFFFFFFF","bordercolor":"FF000000","textcolor":"FF000000","textalignment":"CENTER","strokewidth":2,"cornerradius":0,"separator":"false","operate":"false"},
{"index":1,"pfdobject":"TERMINATOR","tag":"TERMINATOR-END","description":"Terminator","value":"END","unit":"","direction":"NONE","arrow":"NONE","image":"","centerx":300,"centery":220,"width":80,"height":40,"x1":260,"y1":200,"x2":340,"y2":200,"font":"DEFAULT","fontsize":12,"fillcolor":"FFFFFFFF","bordercolor":"FF000000","textcolor":"FF000000","textalignment":"CENTER","strokewidth":2,"cornerradius":0,"separator":"false","operate":"false"},
{"index":2,"pfdobject":"TERMINATOR","tag":"TERMINATOR-START","description":"Terminator start","value":"START","unit":"","direction":"NONE","arrow":"NONE","image":"","centerx":300,"centery":50,"width":80,"height":40,"x1":260,"y1":30,"x2":340,"y2":30,"font":"DEFAULT","fontsize":12,"fillcolor":"FFFFFFFF","bordercolor":"FF000000","textcolor":"FF000000","textalignment":"CENTER","strokewidth":2,"cornerradius":0,"separator":"false","operate":"false"},
{"index":3,"pfdobject":"LINESOLID","tag":"LS-MAIN","description":"Line Solid","value":"","unit":"","direction":"VERTICAL","arrow":"NONE","image":"","centerx":301,"centery":135,"width":2,"height":130,"x1":300,"y1":70,"x2":302,"y2":70,"font":"","fontsize":8,"fillcolor":"FF000000","bordercolor":"00000000","textcolor":"00000000","textalignment":"","strokewidth":2,"cornerradius":0,"separator":"false","operate":"false"}
]
}
```
The index of an object is generated by the PFDCreator and changes every time the PFD is saved.
**Hint**
The PFD file has text format and can be edited to change (add, update, remove) objects.
__Warning__
Be careful making changes to ensure proper JSON format.
Checkout if the JSON definition is OK with an external JSON parser, like [B4X JSON Tree Example](https://b4x.com:51041/json/index.html).

## PFD Shape Type Definition
The PFD shapes are defined as JSON format in the file pfdcreator.shp in the application folder.
The shapes are grouped by PFD Shape Type, like Basic, Flowchart, PID, Electrical or other as defined.

JSON Structure with array of shape types containing an array of shape properties.
```
[
{"shapetype":"Basic", "shapes":[{shape 1 with properties},{shape N with properties}]},
{"shapetype":"Flowchart", "shapes":[{shape 1 with properties},{shape N with properties}]},
{"shapetype":"PID", "shapes":[{shape 1 with properties},{shape N with properties}]},
{"shapetype":"Electrical", "shapes":[{shape 1 with properties},{shape N with properties}]}
]
```

For each of the shape types, the shapes are defined with defaults.
The shapes ae used for the PFD Shape Selector and to add a selected shape to the PFD Drawing Board (canvas).
When adding an object, the object shape defaults are used to set the object properties and object location on the PFD Drawing Board.
The shapes are listed on the application mainpage, in a customlistview, called the Shape Selector.
Clicking on a shape, adds the shape to the PFD Drawing Board at the x,y position with the defined defaults like width, height, fillcolor and more.
The properties are also used to set the shape properties UI Views to enabled or disabled depending the shape properties.

### Default Shape Properties
Per default all properties are assigned to a shape and all PFD UI Views are enabled.
Optional properties can be removed.
List of shape properties with example values.
```
{
	"index":0,
	"pfdobject": "PROCESS",
	"tag":"PROCESS-MAIN",
	"description":"Process",
	"value":"Process",
	"unit":"",
	"direction":"NONE",
	"arrow":"NONE",
	"image":"",
	"centerx":300,
	"centery":130,
	"width":100,
	"height":40,
	"x1":250,
	"y1":110,
	"x2":350,
	"y2":110,
	"font":"DEFAULT",
	"fontsize":12,
	"fillcolor":"FFFFFFFF",
	"bordercolor":"FF000000",
	"textcolor":"FF000000",
	"textalignment":"CENTER",
	"strokewidth":2,
	"cornerradius":0,
	"separator":"false",
	"operate":"false"
}
```

**IMPORTANT**
The property **pfdobject** must be unique within the PFD Shape Definition.
There can not be a shape (shapetype.property) defined as basic.pfdobject="ARROW" and a flowchart.pfdobject="ARROW".
Correct would be for example: basic.pfdobject="ARROW" and a flowchart.pfdobject="FLOWARROW".

### Change Shape Definition
The shape definition file "pfdcreator.shp" can be manually changed using any external text editor.
With the text editor, shapes and shape properties can be added, changed or removed.
__Warning__
Be careful making changes to ensure proper JSON format.
Checkout if the JSON definition is OK with an external JSON parser, like [B4X JSON Tree Example](https://b4x.com:51041/json/index.html).

### PFD Shape Type Module
Each of the shape types must be defined in a dedicated PFD shape type code module (B4X).
As an example, the **Basic** shape type (see PFDCreator.shp) is defined in the B4X code module **PFDShapesBasic.bas**.
**Hint**
This is an example extract of a shape type definition in PFDCreator.shp (Basic.ARROW).
```
[
{
	"shapetype":"Basic",
	"shapes":
		[
			{"pfdobject":"ARROW","tag":"ARW-N","description":"Arrow","direction":"RIGHT","centerx":100,"centery":50,"arrow":"NONE","width":20,"height":20,"textcolor":"FF000000","fontsize":12,"bordercolor":"FF000000","fillcolor":"FFFFFFFF","font":"DEFAULT","strokewidth":2,"operate":false,"method":"DrawArrow","hint":"Arrow with 4 directions left,right,up,down. Textalignment center"},
			...
		]
},
...
]
```

The structure of a shape type code module has 3 sections:
* Process Globals (1)
* Draw All Shapes Methods (2)
* Create & Draw Shape Method (3)

#### Process_Globals (1)
Define the shape object names (pfdobject).
```
Sub Process_Globals
	'Core
	Private xui As XUI

	'PFD Basic Shapes
	Public BASIC_SHAPE_MYSHAPE As String = "MYSHAPE"	'must be unique across all shapes
	...
End Sub
```

#### Draw All Shapes Methods (2)
Then 2 public methods are defined to draw the shapes from a list of objects and draw a shape.
These methods are the same for all shape code modules, but with different pfdobjects.
```
Public Sub DrawShapesFromList(cvs As AsyncCanvas, pfdobjects As List)
	'Loop over the pfd objects and draw the object shape 
	For Each pfdobject As TPFDObject In pfdobjects
		DrawShape(cvs, pfdobject)
	Next
End Sub
```

```
Public Sub DrawShape(cvs As AsyncCanvas, obj As TPFDObject)
	'Select Type
	Select obj.pfdobject
		Case BASIC_SHAPE_MYSHAPE:
			DrawMyShape(cvs, obj)
		'and many more
	End Select
End Sub
```

#### Create & Draw Shape Method (3)
Next are the methods to create & draw the various shapes.
A draw method for a shape has allways the two arguments "cvs" (ASyncCanvas) and "obj" (TPFDObject). There is no exception.
```
cvs As AsyncCanvas	'the canvas to draw to. This can be the Shape Selector customlistview or the PFD Drawing Board canvas.
obj As TPFDObject	'the initialized object.
```

**Example MyShape**
```
#Region MYSHAPE
'Draw a circle.
'Properties: centerx,centery,width,height,fillcolor,bordercolor,strokewidth.
'Notes: Set height=width.
Public Sub DrawCircle(cvs As AsyncCanvas, obj As TPFDObject)
	Dim x,y As Int
	Dim r As Float

	'Set the center pos and the radius
	x = obj.centerx
	y = obj.centery
	r = (obj.width - obj.strokewidth)/2
	
	'Draw the inner shape
	cvs.DrawCircle(x, y, r, obj.fillcolor, True, 0)
	'Draw the outer shape
	If obj.strokewidth > 0 Then cvs.DrawCircle(x, y, r, obj.bordercolor, False, obj.strokewidth)		

	'Commit the drawing is done in method ReDraw in PFDBase.
	'cvs.Invalidate
End Sub
```
**Add the shape definition to PFDCreator.shp**
Example adding the new shape to the shape type "Basic" in the PFDCreator.shp.
**Hint**
To add a new shape, copy any shape and set properties.
The property "pfdobject" must in UPPERCASE and unique across all shapes defined in PFDCreator.shp.
This means there is only a single Basic.MYSHAPE.
```
[
{
	"shapetype":"Basic",
	"shapes":
		[
			{"pfdobject":"MYSHAPE","tag":"MYS-N","description":"MyShape","direction":"RIGHT","centerx":100,"centery":50,"arrow":"NONE","width":20,"height":20,"textcolor":"FF000000","fontsize":12,"bordercolor":"FF000000","fillcolor":"FFFFFFFF","font":"DEFAULT","strokewidth":2,"operate":false,"method":"DrawMyShape","hint":"This is myshape."},
			...
		]
},
...
]
```

## Add Objects to the PFD shape type code module
These are the steps for adding shapes to a PFD shape type code module, like PFDShapesPID, PFDShapesFlowchart and other.
This is an example for adding a new shape BOILER defined in the PFD shape type code module PFDShapesPID.
### Step 1
Add the PID_SHAPE_NAME to the Process Globals.
Example:
```
Public PID_SHAPE_BOILER As String = "BOILER"
```

### Step 2
Add the method DrawBoiler to the public global method DrawShape with the mandatory two arguments cvs and obj.
Example:
```
Public Sub DrawShape(cvs As AsyncCanvas, obj As TPFDObject)
	Select obj.pfdobject
		Case PID_SHAPE_BOILER:
			DrawBoiler(cvs, obj)
		...
	End Select
End Sub
```

### Step 3
Create the method DrawBoiler.
Define the region with same name as the object (Capital letters).
The draw method has the name DrawObject in Captital Letters, like DrawBoiler.
Set the mandatory draw method arguments cvs As AsyncCanvas, obj As TPFDObject).
Example code snippet.
```
#Region Boiler
'Draw a boiler.
'PID Abbreviation: B
Public Sub DrawBoiler(cvs As AsyncCanvas, obj As TPFDObject)
	Dim x0, y0, x1,y1,x2,y2,x3,y3 As Int

	'Set the font
	Dim fnt As B4XFont = PFDCommon.SetFont(obj)

	Dim segment As Int = obj.height / 10

	'Left starting at the bottom
	x0 = obj.centerx - (obj.width/2) + obj.strokewidth
	y0 = obj.centery + (obj.height/2)

	...
	
	'Commit the drawing is done in method ReDraw in PFDBase.
	'cvs.Invalidate
End Sub
#End Region
```

### Step 4
Add the object to the shapes definition file PFDCreator.shp.
```
{
	"shapetype":"PID",
	"shapes":
		[
			{"pfdobject":"BOILER","tag":"B-NN","shapeselector":"","description":"Boiler","value":"","unit":"","direction":"VERTICAL","arrow":"NONE","centerx":50,"centery":50,"width":60,"height":80,"font":"DEFAULT","fontsize":14,"fillcolor":"FF000000","bordercolor":"FF000000","textcolor":"FF000000","textalignment":"CENTER","strokewidth":2,"cornerradius":0,"separator":false,"operate":false,"method":"","hint":"PID Boiler"},
			...
		]
}
```

## Fonts
In PFDCommon, global fonts are defined.
Supported are: DEFAULT, DEFAULTBOLD, DEFAULTITALIC, DIGITAL.
A font is set in the Object Properties field Font.

### Font Methods
The code module PFDCommon contains common methods to define a font in an object draw method.
Comment properties out if not used, like font height.
```
'Set the font
Dim fnt As B4XFont = PFDCommon.SetFont(obj)

'Font dimension - comment out if not used.
Dim fntrect As B4XRect = cvs.MeasureText(obj.value, fnt)
'Value font height
Dim th As Int = Round(fntrect.Height)
'Value font height margin calculated from the textbox height
Dim tm As Int = (obj.height - th) / 2
Dim tl As Int = fntrect.Width
```

Usage:
```
cvs.DrawText(obj.tag, obj.centerx, obj.centery, fnt, obj.textcolor, obj.textalignment)
```

## Settings
The PFDCreator has a settings file, pfdcreator.set, located in the application folder.
The settings are stored in JSON format with key/value pairs:
* pfdfile: (string) The last PFD file loaded. Example flowchart1.pfd.
* showgrid: (boolean) Show (true) or hide (false) the PFD Drawing Board Grid.
* directupdate: (boolean) Direct update the PFD Drawing Board (canvas) in case an object property has changed.
* debug: (boolean) Option (true or false) to set the debug flag for detailed logging.
```
{
  "pfdfile": "e-battery.pfd",
  "debug": false,
  "showgrid": false,
  "directupdate": true
}
```

## Create Shapes from PFD Drawing Board
This is an experimental feature to create B4X code for a shape from one or more objects selected.
The menu Tools > Select to Code asks for a shape name and creates B4X method code which can be viewed and copied from a dialog.
This code can be inserted in PFDShapes modules.
Lets create a shape MyCircle.

### Step 1
On the PFD Shape Selector, add a basic shape Circle.
### Step 2
Place the circle at any position and set optionally the size to for example width & height 50.
### Step 3
Select the circle by using the Draw Mode Select (use the icon or menu Draw > Select).
**Note:** The selection snaps to the grid.
### Step 4
Select menu option Tools > Select to Code.
### Step 5
In the dialog, give the shape the name MyShape.
### Step 6
The code is created and shown in a dialog from which the code can be copied to the clipboard.
In addition the B4X code is to the file MyShape.sub in the application folder.
**B4X Code Example**
```
#Region MyShape
'MyShape
'Base Rectangle: width=60.0,height=60.0
'Note: resizing the shape might not work properly - this will require manual code change.
Public Sub DrawMyShape(cvs As AsyncCanvas, obj As TPFDObject)
	Dim obj9 as TPFDObject
	obj9.initialize
	obj9.arrow = "NONE"
	obj9.bordercolor = -16777216 'FF000000
	obj9.centerx = obj.centerx - (0.0)
	obj9.centery = obj.centery - (0.0)
	obj9.cornerradius = 0
	obj9.description = "Circle"
	obj9.direction = "NONE"
	obj9.fillcolor = -16776961 'FF0000FF
	obj9.font = "DEFAULT"
	obj9.fontsize = 14
	obj9.height = obj.height * 0.8333333 '50.0
	obj9.image = ""
	obj9.operate = false
	obj9.pfdobject = "CIRCLE"
	obj9.separator = false
	obj9.strokewidth = 2
	obj9.tag = "CIR-NN"
	obj9.textalignment = "CENTER"
	obj9.textcolor = -16777216 'FF000000
	obj9.unit = ""
	obj9.value = ""
	obj9.width = obj.width * 0.8333333 '50.0
	obj9.x1 = obj9.centerx - (obj9.width / 2)
	obj9.y1 = obj9.centery - (obj9.height / 2)
	obj9.x2 = obj9.centerx + (obj9.width / 2)
	obj9.y2 = obj9.centery + (obj9.height / 2)
	PFDShapesBasic.DrawCircle(cvs, obj9)

	'Commit the drawing is done in method ReDraw in PFDBase.
	'cvs.Invalidate
End Sub
#End Region
```

The shape method DrawMyCircle uses as parameter an object (obj) representing the properties of the shape.
Within the method DrawMyCircle the values of the parameter obj are assigned to a local object (obj9) used as an argument
for the PFDShapesBasic (B4X Code Module) method DrawCircle.

This example uses only one shape, but of course an own object can have many shapes (which then will use for each a dedicated local object).

To iterate the steps to add this shape to the PFDCreator:
#### Step 1
From the B4J IDE, open the B4J project PFDCreator.
#### Step 2
Select the B4X shape code module which is in this case PFDShapesBasic because this is a new basic shape.
#### Step 3
Add the shape name in Sub Process_Globals
```
Public BASIC_SHAPE_MYSHAPE As String = "MYSHAPE"
```
The prefix BASIC_SHAPE_ is mandatory.
#### Step 4
Add the call to draw the new shape in Public Sub DrawShape(cvs As AsyncCanvas, obj As TPFDObject)
```
	Select obj.pfdobject
		Case BASIC_SHAPE_MYSHAPE:
			DrawMyShape(cvs, obj)
```
#### Step 5
Add the method DrawMyShape in PFDShapesBasic - recommend in alphabetical order.
Because DrawMyShape is added to PFDShapesBasic, remove
PFDShapesBasic from PFDShapesBasic.DrawCircle(cvs, obj9)
#### Step 6
Add the object properties to the shape defintion file PFDCreator.shp (in the application folder) for the key shapetype Basic.
Modify the properties accordingly.
```
	"shapetype":"Basic",
	"shapes":
		[
			{"pfdobject":"MYSHAPE","tag":"MS-N","description":"MyShape","direction":"NONE","centerx":100,"centery":50,"arrow":"NONE","width":60,"height":60,"textcolor":"FF000000","fontsize":12,"bordercolor":"FF000000","fillcolor":"FFFFFFFF","font":"DEFAULT","strokewidth":2,"operate":false,"method":"DrawArrow","hint":"This is myshape."},
```
#### Step 7
In the B4J IDE, compile & run the project PFDCreator.b4j.
The new shape can be found in the shape selector, in this case at first position.

## Create Standalone Package
These are the steps to create a standalone package PFDCreator under Windows.
Tested with Windows 11.
#### Step 1
From the B4J IDE, open the B4J project PFDCreator.
#### Step 2
Select menu Project > Build Standalone Package
The package is created in folder (as an example):
```
c:\data\projects\applications\PFDMaker\PFDCreator\B4J\Objects\temp\
```
#### Step 3
Copy PFDCreator files from the project Objects folder to the package folder Objects\temp\build\bin\.
Following files are required:
* PFDCreator.shp
* PFDCreator.hlp

Use the batch file:
packagerupdate.bat which has content:
```
@echo off
echo PFDCreator Packager Update v20241213
REM Copy mandatory files from the Objects folder to the packager bin folder.
REM In addition add shape definition files (.pfd) or images (.png).
REM Run this batch from the B4J project Objects folder after creating the package.

echo Copying files...
copy PFDCreator.hlp temp\build\bin\*.*
copy PFDCreator.shp temp\build\bin\*.*
REM Add more like shape definition files (.pfd)
REM Add more like images (.png)
echo Copy completed

pause
```
#### Step 4
Test the application by running from the folder Objects\temp\build the application PFDCreator.exe.
#### Step 5
Copy the application from the Objects\temp\build to a target folder like c:\program files\PFDCreator.

### Hint debug
To run the PFDCreator in debug mode, set the options Tools > Debug and run run_debug.bat (instead PFDCreator.exe).
The debug option is stored in the PFDCreator settings (PFDCreator.set).


