 # PFDMaker
**PFDMaker** is a suite of [B4J](https://www.b4x.com/b4j.html) applications to create, view or operate **P**rocess **F**low **D**iagrams (PFD).

Developed as a personal challenge (and for personal use) to create all kind of PFDs.
The main focus has been on **Chemical Engineering Diagrams**, but evolving for **Flowcharts** and more like **Electrical** shapes.

There are 4 applications:
* **PFDCreator** - create PFDs using objects drag & dropped on a canvas.
* **PFDViewer** - view PFDs created by the PFDCreator.
* **PFDOperator** - operate or control PFDs created by the PFDCreator.
* **PFDShapeMaker** - create object shapes used by the PFDCreator (not started).

![1734684964174](https://github.com/user-attachments/assets/6f591361-94a7-4de9-9ae3-b75f1b0f7ee2)

🚧 The **PFDMaker** developments are in progress, but decided to share the source code of the B4J project **PFDCreator**.  
The source code of this B4J project can found in the archive pfdcreator.zip and it requires the [B4J](https://www.b4x.com/b4j.html) to run.
Read next.
***

## PFDCreator
An application to create all kind of simple **P**rocess **F**low **D**iagrams (PFDs) using objects (shapes) which are dragged & dropped on a Drawing Board (Canvas).
A PFD contains objects which are shapes with properties.
The properties define the object shape position, size, colors, text & format, but also its value, unit, operate flag and more.
There are a number of predefined shapes, the basic shapes but also dedicated shape types to create flowcharts, process instrumentation & piping diagrams or electrical circuits.

Several Screenshots
#### Main Page
![1734686420262](https://github.com/user-attachments/assets/28af6257-755b-490c-bbaa-07c834c87037)

#### Shape Types with Shapes
![1734685315414](https://github.com/user-attachments/assets/a6f9722f-535d-4aee-8752-d0c05fe6b2f8)

#### Resize Object and Show Index
![1734685142652](https://github.com/user-attachments/assets/ead04045-639b-4305-87b5-499d52cdf2a1)

#### B4XDialog Examples
![1734685249422](https://github.com/user-attachments/assets/60d4e002-6b1d-45ef-91b9-3fa1cfb8d4c9)

### PFD Shape Types
The PFDCreator has initially following shape types:
* **Basic** - Common shapes which can be used by all PFD types.
* **Flowchart** - Shapes, like terminator, process, decision used to create flowcharts.
* **PID** - Shapes for Process Instrumentation & Piping (PID) diagrams.
* **Electrical** - Shapes to create electrical circuit diagrams.

**Notes**
* The PFDCreator application is provided as a B4J project with source code which requires [B4J](https://www.b4x.com/b4j.html) to run (Windows).
* The PFDCreator has been evolved from the idea to create simple PIDs. The application has become rather complex and has room for improvements.
* The included predefined shapes, created as methods use different ways to draw on the canvas. This is because evolving how to draw shapes. Considering using B4XPath as method only.
* To enhance the PFDCreator with more shape types and shapes, lookup the development notes (DEVNOTES.md).
* This B4J project is provided AS-IS and be aware that developments are ongoing which might lead to concept or code changes.

### Source Code
The source code of the B4J Project **PFDCreator** is included.
Created with B4J v10.0 (64 bit), Java JDK 19.0.2.
Internal libraries: jCore, jFX, B4XPages, XUI Views, Json, DesignerUtils, ByteConverter, BCTextEngine.
Additional libraries: [AsyncCanvas](https://www.b4x.com/android/forum/threads/asynccanvas-b4xcanvas-wrapper-with-invalidate-for-b4j.148736/), [jReflection](https://www.b4x.com/android/forum/threads/jreflection-library.35448/).

**Installation**
Unzip pfdcreator.zip to a folder of choice.
Copy the files from the AdditionalLibraries folder to the B4J Additional libraries folder.
Load PFDCreator.b4j in the B4J IDE.

### Documentation
* DEVNOTES.MD - Comprehensive information about the PFDCreator concept, development and how to enhance.
* TODO.md - Actions for the next version.
* IDEAS.md - Ideas to be considered for implemention.
* CHANGELOG.md - Log of version changes.
* PFDCreator.hlp - User guide.

### Video
A [video](https://1drv.ms/v/s!AhNDg9iSqrdPwNU1YZQVEQ1bd0BCnA) demonstrates how to create a very simple flowchart using the PFDCreator.

## PFDViewer (development in progress)
View PFDs created by the PFDCreator.

## PFDOperator (development in progress)
Operate or control PFDs created by the PFDCreator.

## PFDShapeMaker (development not started
Create object shapes used by the PFDCreator.

## Licence
GNU General Public License v3.0 - Developed for personal use only. See file LICENSE.

## Credits
To [Anywhere Software](https://www.b4x.com) and the [B4X Community](https://www.b4x.com/android/forum/) for sharing libraries, hints&tips.
