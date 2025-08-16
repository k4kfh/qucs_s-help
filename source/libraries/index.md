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

The output of this "compilation" process is the Library file (``.lib`` file), which contains all the Subcircuits of the source Project.

```{warning}
While the Library syntax is XML-like, the exact syntax is not documented at this time, and the files are not intended to be human-editable. Attempting to manually edit a "compiled" Library file may result in unexpected behavior.
```

### Example: Creating a New Library Manually

To create a _Library_, you must first create an empty _Project_. You can do this with ``Project > New Project`` in the menu at the top of the main QUCS-S window. Name the project whatever you like, it does not matter.

```{figure} /libraries/images/creating-new-project.png
---
class: with-border
---

An example of the dialog for creating a new Project in QUCS-S.
```

Once you have an empty _Project_, you need to import a _Schematic_ file (``.sch``), configured for use as a _Subcircuit_, for each component you want to include in your _Library_. There are two methods to do this:
1. If you have not already created any ``.sch`` files for this Library, create them as you would in any normal project. Follow the [documentation on Subcircuits](/subckts-and-ext-models/working-with-subcircuits) if you are unsure how to do this.
2. If you already have ``.sch`` files prepared for your parts, you can include them in your project by simply copying the files into your project's folder on your filesystem.
   * This will be something like ``$HOME/QucsWorkspace/MyNewProject_prj``, where ``$HOME`` is your operating system home directory, and ``MyNewProject`` is the name you assigned when you created your project.
   * If you choose this approach, you will need to force QUCS-S to "refresh" the Project after you copy the files into it. The easiest way to do this is to close the project (Project > Close), then re-open it by double-clicking on ``MyNewProject_prj`` in the [Projects Tab](/overview/interface-overview.md#projects-tab) at the left side of the window.

The figure below shows an example of what a Project might look like when it is properly prepared to be "compiled" into a Library.

```{figure} /libraries/images/project-prepped-for-library.png
---
class: with-border
---

An example of a Project that is ready to be "compiled" into a Library. In this example, the resulting Library would contain two parts, ``LM358.sch`` and ``SPK.sch`` (not necessarily under these names - names can be configured during the Library creation process).
```

Once your Project is prepared with all the parts you wish to include (as their own ``.sch`` files), navigate to ``Project > Create Library``. This will open the Library Creation dialog (shown in the figure below), which allows you to name your Library, and choose which ``.sch`` files from your Project should be included in the compiled Library file.

```{figure} /libraries/images/library-creation-dialog-step1.png
---
class: with-border
---

The first dialog in the Library compilation process, which prompts you to select the ``.sch`` files for inclusion in the Library, and give the Library a name.
```

After clicking "Next", you will be given the opportunity to assign a description to each component (``.sch`` file) in the library. These can be relatively long descriptions. They will be visible in the bottom left of the QUCS-S window whenever a component is selected for insertion into your schematic (see the figure below for an example).

```{figure} /libraries/images/library-description-diagram.drawio.png
---
class: with-border
---

Annotated screenshots showing where you can specify a long text description of each Library component, and where these descriptions are available once your Library has been created.
```

After you specify a description for every _Component_ being included in your _Library_, the actual "compilation" process will begin. Relevant logs and errors from this process are shown in the pop-up window (as shown in the figure below).

```{figure} /libraries/images/library-compilation-log.png
---
class: with-border
---

Example of the logs from the "compilation" of a new Library. The "no digital model" errors can be disregarded since, in this example, we are creating models for analog components.
```

```{tip}
If you see errors like ``ERROR: Component "Example" has no digital model``, you can usually disregard them. This is just QUCS-S warning you that no VHDL or Verilog model for your components. If you are creating a Library of analog components, such a model is obviously not required.
```

After the compilation process completes, your new _Library_ should be usable from the "User Libraries" section of the [Libraries Tab in the main window,](/overview/interface-overview.md#libraries-tab) as shown in the figure below.

```{figure} /libraries/images/library-creation-success-example.drawio.png
---
class: with-border
---

Example of what a successfully-created Library would look like. This example shows a library ``ExampleLibrary`` containing a single part, ``voltage-divider``. The Library has been set up as a _User Library_, which means it's available in all QUCS-S projects opened on this PC. See the next section for more information on this.
```

### Example: Creating a New Library from SPICE .MODEL Data with QucsConv

TODO: Write about using QucsConv

## Using Libraries

Once you have a Library, you can do one of two things with it:

1. Set it up as a **User Library**, which is available across _all_ QUCS-S projects opened on your PC.
    * _User Libraries_ will NOT travel with your QUCS-S projects when transferred from one computer to another.
    * **This is what QUCS-S does with new Libraries by default.**
2. Set it up as a **Project Library**, which is stored within another QUCS-S _Project_.
    * _Project Libraries_ are "portable". They travel anywhere your _Project_ goes, including to another PC.

### Setting up a User Library

If you are creating your own brand new Library, it will be configured as a User Library by default. Simply follow the steps [in the previous section.](#making-libraries)

If you are importing a pre-existing Library (for example, if a peer has sent you a ``.lib`` file that you want to use), simply copy the file into ``$HOME/QucsWorkspace/user_lib/`` and restart QUCS-S for the changes to take effect.

### Setting up a Project Library

Project Libraries have the advantage of being "portable" with the rest of your QUCS-S Project, unlike User Libraries. This is because Project Libraries are stored in a [QUCS-S Project folder](/overview/understanding-file-structure.md#projects), and therefore travel alongside the rest of the Project (in contrast to a User Library which is stored at the system level).

To configure a Library as a Project Library, simply paste the ``.lib`` file into the Project folder. You may need to close and re-open the Project in QUCS-S for the Library to be recognized.

To access a Project library, first open the Project it's stored in (via the Projects tab). Then, navigate to the Libraries tab. Any Project Libraries should be listed in the "Project Libraries" section of the Libraries tab, as shown in the figure below.

```{figure} /libraries/images/project-library-example.drawio.png
---
class: with-border
---

Example of what a Library configured as a Project Library would look like. This example shows a library ``ExampleLibrary`` containing a single part, ``voltage-divider``. It is stored within the ``SimulationTypes_Examples`` project.
```