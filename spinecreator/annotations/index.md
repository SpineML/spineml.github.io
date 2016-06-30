Developer Information: SpineCreator Annotations and Project file
================================================================

SpineCreator Annotations
------------------------

SpineCreator stores information about the 2D layout of networks, the 3D layout of neurons, the 2D routing of inputs and projections, and python scripts used to generate some ConnectionList data, as Annotations using the new SpineML Annotations tags. Annotations about an object (population/projection etc...) are stored inside that tag, and SpineCreator will respect existing Annotations in the tags it modifies. In order for other software to be built that can utilise the SpineCreator information we present a guide to the tags and information that can be stored in Annotations through SpineCreator.

### Population

An annotations tag within a Population stores the following information.

`       `<LL:Annotation>
`           `<SpineCreator>
`               `<xPos value="-11.2353"/>
`               `<yPos value="12.5792"/>
`               `<animSpeed value="0.2"/>
`               `<aspectRatio value="1.66667"/>
`               `<colour red="0" green="0" blue="0"/>
`               `<size value="1"/>
`               `<tag value="2"/>
`               `<x3D value="0"/>
`               `<y3D value="0"/>
`               `<z3D value="0"/>
`               `<is_visualised value="0"/>
`           `</SpineCreator>
`       `</LL:Annotation>

#### xPos

| Contains      | Description                         |
|---------------|-------------------------------------|
| @value::float | The 2D X position of the Population |

#### yPos

| Contains      | Description                         |
|---------------|-------------------------------------|
| @value::float | The 2D Y position of the Population |

#### animSpeed

| Contains      | Description                                                |
|---------------|------------------------------------------------------------|
| @value::float | The speed that a moved Population follows the mouse cursor |

#### aspectRatio

| Contains      | Description                                                 |
|---------------|-------------------------------------------------------------|
| @value::float | The ratio of the width to the height of the Population icon |

#### colour

| Contains     | Description                                               |
|--------------|-----------------------------------------------------------|
| @red::unit   | The red component of the Population icon colour (0:255)   |
| @green::unit | The green component of the Population icon colour (0:255) |
| @blue::unit  | The blue component of the Population icon colour (0:255)  |

#### size

| Contains      | Description                     |
|---------------|---------------------------------|
| @value::float | The size of the Population icon |

#### tag

| Contains    | Description                        |
|-------------|------------------------------------|
| @value::int | Not currently used by SpineCreator |

#### x3D

| Contains    | Description                                                                   |
|-------------|-------------------------------------------------------------------------------|
| @value::int | The initial 3D X offset for drawing the neurons in the Population in 3D space |

#### y3D

| Contains    | Description                                                                   |
|-------------|-------------------------------------------------------------------------------|
| @value::int | The initial 3D Y offset for drawing the neurons in the Population in 3D space |

#### z3D

| Contains    | Description                                                                   |
|-------------|-------------------------------------------------------------------------------|
| @value::int | The initial 3D Z offset for drawing the neurons in the Population in 3D space |

### Projection

An annotations tag within a Projection stores the following information.

`           `<LL:Annotation>
`               `<SpineCreator>
`                   `<DrawOptions style="0" showlabel="0"/>
`                   `<start x="9.27878" y="7.23497"/>
`                   `<curves>
`                       `<curve>
`                           `<C1 xpos="9.4914" ypos="7.23497"/>
`                           `<C2 xpos="9.4914" ypos="7.22533"/>
`                           `<end xpos="9.56297" ypos="7.22533"/>
`                       `</curve>
`                   `</curves>
`               `</SpineCreator>
`           `</LL:Annotation>

#### DrawOptions

| Contains         | Description                                                                    |
|------------------|--------------------------------------------------------------------------------|
| @style::int      | Identifies the style used to draw the Projection in SpineCreator               |
| @showlabel::bool | Used to indicate if a label describing the Projection type should be displayed |

#### start

| Contains  | Description                                                                           |
|-----------|---------------------------------------------------------------------------------------|
| @x::float | The X start point of the bezier curves used to describe the 2D form of the Projection |
| @y::float | The Y start point of the bezier curves used to describe the 2D form of the Projection |

#### curves

| Contains        | Description                                                                          |
|-----------------|--------------------------------------------------------------------------------------|
| curve \[1:...\] | Container for the bezier curve points used to describe the 2D form of the Projection |

#### curve

| Contains  | Description                                                                                           |
|-----------|-------------------------------------------------------------------------------------------------------|
| C1 \[1\]  | Bezier cubic curve C1 control point (See Qt documentation for QPainterPath::cubicTo for more details) |
| C2 \[1\]  | Bezier cubic curve C2 control point (See Qt documentation for QPainterPath::cubicTo for more details) |
| end \[1\] | Bezier cubic curve end point (See Qt documentation for QPainterPath::cubicTo for more details)        |

#### C1

| Contains     | Description                   |
|--------------|-------------------------------|
| @xpos::float | X location of C1 bezier point |
| @ypos::float | Y location of C1 bezier point |

#### C2

| Contains     | Description                   |
|--------------|-------------------------------|
| @xpos::float | X location of C2 bezier point |
| @ypos::float | Y location of C2 bezier point |

#### end

| Contains     | Description                    |
|--------------|--------------------------------|
| @xpos::float | X location of end bezier point |
| @ypos::float | Y location of end bezier point |

### Input

An annotations tag within a Input stores the following information.

`               `<LL:Annotation>
`                   `<SpineCreator>
`                       `<start x="9.21702" y="5.48872"/>
`                       `<curves>
`                           `<curve>
`                               `<C1 xpos="8.94197" ypos="6.10971"/>
`                               `<C2 xpos="8.94197" ypos="6.10971"/>
`                               `<end xpos="8.66691" ypos="6.7307"/>
`                           `</curve>
`                       `</curves>
`                   `</SpineCreator>
`               `</LL:Annotation>

For descriptions of these tags please refer to the Projection section

### ConnectionList

An annotations tag within a ConnectionList stores the following information.

`                       `<SpineCreator>
`                           `<Script text="#!/usr/bin/python&#10;&#10;#PARNAME=size #LOC=1,1&#10;#PARNAME=scale #LOC=2,1&#10;#PARNAME=inhib #LOC=3,1&#10;#HASWEIGHT&#10;&#10;def connectionFunc( srclocs, dstlocs, size, scale, inhib ): &#10;..."/>
`                           `<Config weightProperty="w"/>
`                       `</SpineCreator>

#### Script

| Contains     | Description                                                                  |
|--------------|------------------------------------------------------------------------------|
| @text:string | The text describing a script for generating a ConnectionList of connectivity |

#### Config

| Contains               | Description                                                                                        |
|------------------------|----------------------------------------------------------------------------------------------------|
| @weightProperty:string | A reference to the WeightUpdate Property name where an ExplictList is created by the Python script |

SpineCreator Project file
-------------------------

SpineCreator stores the details of the xml files that compose the current Project in a file with the .proj extension. The format of this files is as follows:

``` xml
<SpineCreatorProject>
 <Network>
  <File name="model.xml"/>
 </Network>
 <Components>
  <File name="LIN_adap.xml"/>
  <File name="LIF_det.xml"/>
  <File name="LIF_det1.xml"/>
  <File name="LIFposneg.xml"/>
  <File name="LIF1.xml"/>
 </Components>
 <Layouts>
  <File name="none.xml"/>
  <File name="hexagonal.xml"/>
 </Layouts>
 <Experiments>
  <File name="experiment0.xml"/>
  <File name="experiment1.xml"/>
 </Experiments>
</SpineCreatorProject>
```

#### SpineCreatorProject

| Contains          | Description                                      |
|-------------------|--------------------------------------------------|
| Network \[1\]     | Contains the SpineML Network for the project     |
| Conponents \[1\]  | Contains the SpineML Components for the project  |
| Layouts \[1\]     | Contains the SpineML Layouts for the project     |
| Experiments \[1\] | Contains the SpineML Experiments for the project |

#### Network

| Contains   | Description           |
|------------|-----------------------|
| File \[1\] | The network file name |

#### Components

| Contains       | Description              |
|----------------|--------------------------|
| File \[1:...\] | The component file names |

#### Layouts

| Contains       | Description           |
|----------------|-----------------------|
| File \[1:...\] | The layout file names |

#### Experiments

| Contains       | Description               |
|----------------|---------------------------|
| File \[1:...\] | The experiment file names |

#### File

| Contains     | Description        |
|--------------|--------------------|
| @name:string | The name of a file |
