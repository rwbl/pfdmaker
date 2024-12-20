# IDEAS - Project PFDCreator
Any ideas captured which might get implemented in the next version of the application.

## NEW: ISA-101
Define the shapes according to the [ISA-101](https://www.isa.org/standards-and-publications/isa-standards/isa-standards-committees/isa101) Human-Machine Interfaces standard.
### Status
Exploring the ISA-101 standard (plus Process HMI Style Guide) and started implementing step-by-step the recommendations.
Example:
Canvas Background Color: RGB(221,221,221) set in PFDBase ReDraw().

## NEW: Object Property Option Show or Hide Tag
Option to show / hide the tag of an object.
### Status
Not started.

## NEW: List Operate Objects
Show the list of operate objects in a dialog with a listtemplate.
Menu > Object > List Operate.
### Status
20241218: Completed.

## NEW: Additional Fonts
Consider setting dedicated fonts for a tag or value or unit.
### Status
Not started.

## NEW: Set Dedicated Font Tag/Value/Unit
Consider setting dedicated fonts for a tag or value or unit.
Current solution is that a font applies to tag, value and unit.
### Status
Not started.

## NEW: Electrical Shapes
Create electrical symbols in a dedicated code module PFDShapesElectrical.
### Status
20241218: Started. See code module PFDShapesElectrical.

## NEW: Check PFD Shape Definition Duplicate PFDObject
Check if the PFD shape definition file, pfdcreator.shp, contains duplicate pfdobject property.
The property **pfdobject** must be unique within the PFD Shape Definition.
There can not be a shape (shapetype.property) defined as basic.pfdobject="ARROW" and a flowchart.pfdobject="ARROW".
Correct would be for example: basic.pfdobject="ARROW" and a flowchart.pfdobject="FLOWARROW".
### Status
Not started.

## NEW: Input Field Object
Just an idea if this is possible using the canvas containing an object with input field.
### Status
Not started.

## NEW: Traffic Light Object
Rectangle with 3 filled circles without borderline with colors RED-YELLOW-GREEN (RGB).
Direction horizontal or vertical.
Value set as CSV string R;G;B in object.value properties.
XUI colors: RED = xui.color_red, YELLOW = xui.color_rgb(255,255,0), GREEN = xui.color_rgb(50,205,50) = lime or color_green
### Status
Not started.

## NEW: Font Digital
Add additional external font Digital.
### Status
20241218: Started.
Tested using digital.ttf stored in the dirassets folder.
Font can be selected via B4XComboBox in the object properties.
First tests did slow down the canvas drawing. Not sure if this is caused by digital font.
Need to explore further.

## UPD: Grid with bold short lines at scale text position
Draw short line in bold, half grid size at scale pos.
```
Private GRID_LINE_COLOR_BOLD As Int = xui.Color_DarkGray	'ignore

'Draw horizontal lines with scale text
For i = 0 To horlines
	cvs.DrawLine(0, i * GRID_SIZE, width, i * GRID_SIZE, GRID_LINE_COLOR, 1)
	If i Mod 5 == 0 And i > 0 Then
		>>> cvs.DrawLine(0, i * GRID_SIZE, GRID_SIZE/2, i * GRID_SIZE, GRID_LINE_COLOR_BOLD, 2)
		cvs.DrawText(i * GRID_SIZE, 5, i * GRID_SIZE + 4, fnt, GRID_TEXT_COLOR, "LEFT")
	End If
Next
'Draw vertical lines with scale text
For j = 0 To vertlines
	cvs.DrawLine(j * GRID_SIZE, 0, j * GRID_SIZE, height, GRID_LINE_COLOR, 1)
	If j Mod 5 == 0 And j > 0 Then
		>>> cvs.DrawLine(j * GRID_SIZE, 0, j * GRID_SIZE, GRID_SIZE/2, GRID_LINE_COLOR_BOLD, 2)
		cvs.DrawTextRotated(j * GRID_SIZE, j * GRID_SIZE - 4, 5, fnt, GRID_TEXT_COLOR, "LEFT", 90)
	End If
Next
```
### Status
Tested but not implemented. Does not look that pretty.

## NEW: Multi Select Object Move with Mouse
The multi selected object are moved using the Object Properties Arrow Symbols (at bottom right).
Consider using the mouse.
### Status
Not started.

## NEW: Move/Resize Object(s) using Keyboard Arrow Keys
In addition to the controls, use the keyboard arrow keys to move or resize an object.
### Status
Not started.

## UPD: PFDCommon.DrawValueInObject
Draw the value in the object vertical centered.
Improve wrap. For now only the first space is wrapped to CRLF.
### Status
Not started.

## NEW: Draw Connector Line
Create a connector line which consists out of multiple lines.
Example: Line connection between point x,y 100,100 and 200,200 results in 3 lines:
100,100 to 150,100 to 150,200 to 200,200
Define the obj.value as string with CSV: (100,100);(150,100);(150,200);(200,200)
### Status
Testing with example code:
```
Public Sub DrawConnectorLine(cvs As AsyncCanvas, obj As TPFDObject)
	'Get the lines with X,Y coordinates from CSV string
	Dim lines() As String = Regex.Split(";",obj.value)
	
	'Build the lines
	Dim x1,y1,x2,y2 As Int = 0
	For i = 0 To lines.Length - 1

		Dim line As String = lines(i)
		line = line.Replace("(","").replace(")","")

		Dim lineto() As String = Regex.Split(",",line)
		x1 = x2
		y1 = y2
		x2 = lineto(0)
		y2 = lineto(1)
		
		Select i
			Case 0
				'Draw start as pixel
				cvs.DrawPixel(x2, y2, obj.bordercolor)
			Case Else
				'Draw line from previous point
				cvs.DrawLine(x1, y1, x2, y2, obj.bordercolor, obj.strokewidth)
		End Select
	Next
End Sub
```
See also
Create PFD object "ConnectorLine" to connect 2 objects.
Possible solution could be to use the value with CSV string containing LineTo X,Y coordinates.
Example: (100,100);(100,150);(200,150). Start at X=100,Y=100 > LineTo X=100,Y=150 > LineTo X=200,Y=150.
```
Dim positions() as string = regex.split(";", obj.value)
for each position as string in positions
	position = poisition.replace("(","").replace(")","")
	dim linetos() as string = regex.split(",", position)
next
for i = 0 to linetos.length - 1
	...
next
```

## NEW: Explore using Javaobjects from JavaFX Class GraphicsContext
Reference: Class [GraphicsContext](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/canvas/GraphicsContext.html).
The class GraphicsContext methods and attributes to draw on the canvas.
This offers the possibility to replace drawing objects using AsyncCanvas methods by direct calling class methods.
## Status
Exploring. Example creating a non-filled rectangle.
```
DrawRectJO(mCvs, obj.centerx, obj.centery, obj.width, obj.height, obj.bordercolor, False, obj.strokewidth)
```

```
'Draw a rect using JavaObjects.
'Example: DrawRectJO(mCvs, obj.centerx, obj.centery, obj.width, obj.height, obj.bordercolor, False, obj.strokewidth)
Public Sub DrawRectJO(cvs As AsyncCanvas, x As Double, y As Double, Width As Double, Height As Double, Color As Int, Filled As Boolean, LineWidth As Double)
	Dim joCanvas As JavaObject = cvs.GetNativeCanvas
	Dim joGraphicsContext As JavaObject = joCanvas.RunMethod("getGraphicsContext2D", Null)

	If Filled = False Then
		joGraphicsContext.RunMethod("setStroke", Array As Object(fx.Colors.From32Bit(Color)))
		joGraphicsContext.RunMethod("setLineWidth", Array As Object(LineWidth))
		joGraphicsContext.RunMethod("strokeRect", Array As Object(x, y, Width, Height))
	Else
		joGraphicsContext.RunMethod("setFill", Array As Object(fx.Colors.From32Bit(Color)))
		joGraphicsContext.RunMethod("fillRect", Array As Object(x, y, Width, Height))
	End If
End Sub
```

## UPD: Object Property Index
The index (readonly) of the object.
The number is created automatically by the PFDCreator, is not fixed and can not be changed.
## Status
Started exploring best way to set a fix index number.

## UPD: Object Property Enabled/Disabled
For now all properties are enabled for all objects. This means that changing a property might not have an affect on the selected object.
There are a number of properties, which apply to specific objects, like for PID diagrams.
Look up, the hint of the object property.
## Status
Not started.

## UPD: Object Property Direction
Revise the number of directions = simplify.
## Status
Not started.

## UPD: Improve Drawing Speed
Using the standard B4XCanvas:
When a PFD contains many objects, drawing & moving is getting slower.
**Test**
Replace B4XCanvas with external library [AsyncCanvas](https://www.b4x.com/android/forum/threads/asynccanvas-b4xcanvas-wrapper-with-invalidate-for-b4j.148736/).
All drawings do not used invalidate, but invalidate is called when method ReDraw is called.
This has improved speed.
**Idea**
When adding or moving objects (dragging), save the current PFD as bitmap and show the bitmap whilst dragging.
When dragging complete, remove the image and redraw the PFD from the list of objects.
### Status
20241212: Replaced B4Xcanvas with ASyncCanvas. Results are OK so far.
