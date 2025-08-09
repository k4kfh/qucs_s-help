# Libraries in QUCS-S

To simulate "real-world" circuit designs, it is usually necessary to incorporate models of "real-world" components. For example, you may need a model for a specific voltage regulator IC from Texas Instruments, or a specific diode from OnSemi (as opposed to a genericized/ideal model).

[Bringing external SPICE models into QUCS-S simulations was discussed in earlier sections of the documentation.](/subckts-and-ext-models/spice-models/index) But for common parts used across many projects, it's a waste of time to go back through the process of importing the model and configuring the symbol for each project. This is where Libraries come in - they allow you to build a collection of frequently-used "real-world" models, complete with all the necessary symbols to use them in QUCS-S simulations.

## Making Libraries

### Compilation Process

You can think of QUCS-S Libraries as a "compiled" product. Creating a Library is a one-way process, just like compiling source code. If you wish to update a library, you cannot do so with just the "compiled" library file, you need the "source code".

```{figure} /libraries/images/library-compilation-process.drawio.png
---
class: with-border
---

Process diagram, showing the one-way process of creating _Libraries_ from _Projects_ containing _Subcircuits_.
```

The input to the "compilation" (i.e. the "source code") is [a standard QUCS-S "Project"](/overview/understanding-file-structure.md#projects), containing multiple ``.sch`` files which are [set up as Subcircuits](/subckts-and-ext-models/working-with-subcircuits). There should be one of these ``.sch`` files for each Device you want in your Library. The Subcircuits should have all the necessary symbols and ports configured - all the Library will do is make it easier to drop these Subcircuits into your projects.

```{tip}
There's no special/quick process to generate these Subcircuits for use in a Library. It's manual, and you cannot use [the new "SPICE Library Device" component to accelerate the process.](/subckts-and-ext-models/spice-models/using-spice-subckt-models.md#importing-subckt-models-with-spice-library-device-recommended)

You can follow [this section of the Subcircuit documentation](/subckts-and-ext-models/spice-models/using-spice-subckt-models.md#importing-subckt-models-with-spice-file-component) to learn how to create a subcircuit with a customized symbol based on an external SPICE model.
```

The output of this "compilation" process is the Library file, which contains all the Subcircuits of the source Project.

```{warning}
While the Library syntax is XML-like, the exact syntax is not documented at this time, and the files are not intended to be human-editable. Attempting to manually edit a "compiled" Library file may result in unexpected behavior.
```

### Example: Creating a Library

TODO: rework example from Habr tutorial into this section

## Using Libraries

Once you have a Library, you can do one of two things with it:

1. Set it up as a **User Library**, which is available across _all_ QUCS-S projects opened on your PC.
    * _User Libraries_ will NOT travel with your QUCS-S projects when transferred from one computer to another.
2. Set it up as a **Project Library**, which is stored within another QUCS-S _Project_.
    * _Project Libraries_ are "portable". They travel anywhere your _Project_ goes, including to another PC.

### Setting up a User Library

TODO: how to take a library file and make it a User Library

### Setting up a Project Library

TODO: how to take a library file and make it a Project Library