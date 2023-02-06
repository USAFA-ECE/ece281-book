# Manually Setup a Vivado Project

At this point you should NOT have a ICE2 directory in your git
repository. Instead, Vivado will create that directory for you when you
create a project.

-   If you have not already installed the board files (should have been
    done as part of Lab0), **make sure Vivado is closed**, and then copy
    the **basys3** directory from: o [Teams]{.underline}

-   [Provided Install Directory]{.underline} to your Vivado installation
    at:

-   C:\\Xilinx\\Vivado\\2018.2\\data\\boards\\board_files

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

## 2. CREATE CODE AND IMAGES DIRECTORIES AND ADD FILES TO THEM

Navigate to your ICE2 folder, in your git repository, in Windows
Explorer (file manager) and download/extract the following files from
Teams (Class Materials/ICEs/ICE02):

-   A **code** directory with the following given files

    -   **halfAdder.vhd** and **halfAdder_tb.vhd**

>  In future labs, you might copy in the ECE_template.vhd and
> ECE_template_tb.vhd files instead (and then rename them)

-   Constraints file (Basys3_Master.xdc)

**Add/stage and commit your changes**

-   Prepend all commit statements for ICE2 with "ICE2 -- ". For
    instance, your first commit might be: o "**ICE2 -- created initial
    project files and README**"

**If you did not copy the provided .gitignore file to your git repo
folder ([not]{.underline} the ICE2 folder), please do so now.** This
will force Git to ignore all files and directories other than the ones
we specify. Alternatively, when you add files, only add the files you
wish to commit. This is important as Vivado will create a LOT of files
that we do not want in your repo. Alternatively, only commit files you
specifically want. If you ever find that git won't let you add a file,
check to see if the .gitignore will allow it to pass.

## 3. CREATE DESIGN

ADD SOURCES (DESIGN, SIMULATION, AND CONSTRAINTS)

On the leftmost pane (*Flow Navigator*) of Vivado, click on **Add
Sources**

> ![](img/ice3_image4.jpg){width="1.7743055555555556in"
> height="2.2951213910761155in"}

**Add Sources window**

-   Select the "Add or create **design sources**" radio button and click
    Next. o Click on "**Add Files"** button

-   Navigate to your ICE2/code folder, select halfAdder.vhd, and click
    OK

-   **Ensure Copy sources into project** is **[Unchecked]{.underline}**
    (we want to edit the file in the **code** folder) and click
    **Finish**

> ![](img/ice3_image5.jpg){width="6.442361111111111in"
> height="4.3887171916010494in"}

The VHDL module should now be listed in your Sources sub-window. The
**bolded** name is based on the entity name,

![](img/ice3_image6.jpg){width="5.4375in" height="2.0in"}

Add another source, but this time select "Add or create **simulation**
sources" in the initial window o Add **halfAdder_tb.vhd** ("tb" stands
for test bench)

> Again, make sure "Copy sources into project" is **unchecked**

The test bench file should now be added into your Simulation Sources
(scroll down and/ or re-size the sub-window to see the Simulation
Sources):

> ![](img/ice3_image7.jpg){width="3.5541666666666667in"
> height="2.287938538932633in"}

If for some reason, it is not the top Simulation Source as shown above,
right-click on it and select "Set as Top".

Add the final source, but this time select "Add or create
**constraints**" in the initial window o Add **Basys3_Master.xdc**

-   Make sure "Copy constraints files into project" is **unchecked**

The constraints file should now be added into your Constraints:

![](img/ice3_image8.jpg){width="3.527083333333333in"
height="2.799134951881015in"}
