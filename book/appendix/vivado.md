# Xilinx Vivado

## Board files

If you have not already installed the board files (should have been done as part of Lab0),
**make sure Vivado is closed**, and then copy
the **basys3** directory from Teams.

## Creat a new Vivado Project

-   Launch Vivado (yes, sometimes it takes a minute) o Click
    FileProject, New o First Window

    -   Click "Next" o Second Window

    -   **Project Name:** ICE_2

    -   **Location:** Click the "..." and navigate to your git
        repository

    -   Leave "Create project subdirectory" checked

    -   Click "Next"

![](img/ice3_image1.jpg){width="4.895138888888889in"
height="2.3193635170603675in"}

-   Third Window (Project Type)  Select "RTL Project"

    -   Ensure "Do not specify sources at this time" is checked  Click
        "Next"

![](img/ice3_image2.jpg){width="5.929340551181102in"
height="1.645138888888889in"}

-   Fourth Window (Default Part)

    -   Click on **Boards**

    -   Select **Basys3** from the "Name" drop down menu **or** search
        for Basys3 ![](img/ice3_image3.png){width="6.503333333333333in"
        height="3.45in"}

    -   Click "Finish"

(manual-add-to-vivado-project)=
## Manually add files to Vivado Project

By convention (and for working nicely with git) we add
source files to `src/` and if it is a hardware description file then to `src/hdl/`
(there are other source files in advnaced projects that
are not HDL).

Once a file is in that location, add it to a Vivado Project by:

1. On the leftmost pane (*Flow Navigator*) of Vivado,
under PROJECT MANAGER, click on **Add Sources**
2. Select the appropriate radio button.
    - "Add or creae constraints" for an `.xdc` file
    - "Add or create design sources" for source file
    - "Add or create simulation sources" for a `_tb.vhd` file
3. Click on **Add Files**
4. Select your file
5. Make sure to **UNCHECK** "Copy sources into project"
6. Click Finish

You should now see the file in the Sources tab.

The VHDL module should now be listed in your Sources sub-window.
The **bolded** name is based on the entity name.

```{warning}
If you plan on pushing the project to git and rebuilding it elsewhere
with `build.bat` and `build.tcl` then you must rewrite the TCL file. See {ref}`write-tcl-file`.
```

(write-tcl-file)=
## Write a TCL file for use with Git
