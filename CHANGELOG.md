# CHANGELOG - Project PFDCreator
PFDCreator is a B4J application created with B4J v10.0 (64 bit), Java JDK 19.0.2.

## v1.00 (Build 20241220)
* NEW: Key management - Support for some keys, like Move object(s) using the arrow keys; Control+O, Control+S; Delete.
* NEW: Resize an object (after selection) via mouse touched by using 5 rectangles = object + 4 corners.
* NEW: Font DEFAULTITALIC, DIGITAL (slows down moving objects).
* NEW: Menu Object > Remove Selected - Remove objects selected with drawmode Selected.
* NEW: Menu Tools > Show Index - Helper to show the object index within a red circle below the object.
* NEW: Menu Tools > PFD Background Color - Colorpicker dialog to set the background color of the PFD canvas. ISA-101 recommendation is RGB 221,221,221 (224 also used) or 192,192,192.
* NEW: Menu Tools > Select to Code - Create a shape, select and generate B4X subroutine code, i.e. DrawMyShape.
* NEW: Menu Tools > Debug - Enable debug info to the log, added to the settings as "debug": false.
* NEW: Menu Help > Help & About: Help file using custom dialog with BBCodeView.
* NEW: Toolbar > Main - Open PFD file.
* NEW: PFD Definition File - Properties: "pfdbackgroundcolor", with value ARGBD, like "FFE0E0E0" (i.e. RGB 224,224,224); "hint" displayed at bottom Object Properties; "method" used to create new shape from select area.
* NEW: Object Properties - Hint label which is defined as key "hint" in the PFD shapes definitiion file pfdcreator.shp.
* NEW: PFDShapesBasic - Shapes: Connector lines between 2 objects: CONNECTORLLINE, CONNECTORULINE, CONNECTORZLINE; Solid line rotated; Pixel; TextArrow; Path.
* NEW: PFDShapesBasic - DrawTextRotated used for all shapes having text, like label, textbox.
* NEW: PFDShapesPID - TrendIndicator to use for operate, data set in object value in JSON format. This is experimental.
* NEW: PFDShapesPID - ISA-101 Shapes BINARYSTATEINDICATOR, DATACONNECTORLINE, NUMERICDATA, PRIMARYPROCESSLINE, SECONDARYPROCESSLINE.
* NEW: PFDShapesElectrical - Shape type Electrical. Started with some shapes (to play with): 7SEGMENTDISPLAY, CAPACITOR, CIRCUITCONNECTOR, RESISTOR, RESISTORALTERNATE, VALUEUNIT.
* NEW: Helper - Many additionals methods, to mention WordWrapString used by all shapes having text; Various color conversions painttoint and intto paint.
* NEW: Application icon (PNG 16x16px).
* NEW: Started writing development notes in file DEVNOTES.md (planned to create PDF format).
* UPD: Moved menu Tools > PFD Description to menu File > PFD Description.
* UPD: Mainform - Changed size to width 1600, height 1000. Panes for object list, shape selector, object properties width 250.
* UPD: Helpfile - Reworked the help file using BBCodeView.
* UPD: Various fixes.
* UPD: Shared on the [B4J Forum](https://www.b4x.com/android/forum/threads/pfdmaker-create-view-or-operate-process-flow-diagrams.162893/) POST #4.

## v0.20 (Build 20241024)
* UPD: Highlights: added menu-bar, CLV shape selector, object properties enhanced, reworked B4X module structure, Draw mode Select.
* UPD: Shared on the [B4J Forum](https://www.b4x.com/android/forum/threads/pfdmaker-create-view-or-operate-process-flow-diagrams.162893/) POST #3.

## v0.10 (Build 20240903)
* NEW: Early preview version shared on the [B4J Forum](https://www.b4x.com/android/forum/threads/pfdmaker-create-view-or-operate-process-flow-diagrams.162893/) POST #1.

## v0.00 (Build 20240801)
* NEW: Started with some first ideas.
