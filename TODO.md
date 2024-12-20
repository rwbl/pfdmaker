# TODO - Project PFDCreator
Actions to complete till next version of the application.
Any ideas to be captured in IDEAS.md.

## UPD: Object Property Index
The index (readonly) of the object.
The number is created automatically by the PFDCreator, is not fixed and can not be changed.
## Status
Started exploring best way to set a fix index number.

## UPD: Object Property Enabled/Disabled
For now all properties are enabled for all objects.
This means that changing a property might not have an affect on the selected object.
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
All drawings do not used invalidate, but invalidate is called when method ReDraw is called > this solution has improved speed.
**Idea**
When adding or moving objects (dragging), save the current PFD as bitmap and show the bitmap whilst dragging.
When dragging complete, remove the image and redraw the PFD from the list of objects.
### Status
20241212: Replaced B4Xcanvas with ASyncCanvas. Results OK so far.
