# Main GSAS-II menu commands

## **File** Menu

* **Open project...** - Open a previously saved GSAS-II project file ({project}.gpx). If you currently have a project file open, you are asked if it is OK to overwrite it; Cancel will cancel the read process.

    !!! Note
        Note that as files are saved during a structure refinement, copies of the previous version are saved as backup files, named as {project}.bak{i}.gpx, where i starts as 0 and is increased after each save operation. NB: you may open a backup .gpx file (e.g. name.bak3.gpx) to return to a previous version of your project, but if you do so, it is best to immediately use the Save As... menu command (you may wish to use name.gpx to overwrite the current version or select a new name.) If you forget specify a project name, then name.bak3 will be considered the project name and backups will then be named name.bak3.bak0.gpx, etc.

* **Save project** - Save the current project. If this is a new project that has not yet been saved, you will be prompted for a new name in a file dialog (you may optionally change the directory in that dialog). If the file exists, you will be asked if it is OK to overwrite it. Once a file name has been used to read or save a project, the name is shown after 'Loaded Data:' in the first item in the data tree.
* **Save Project as...** - Save the current project in a specified project file. You will be prompted for a new name in a file dialog (you may optionally change the directory in that dialog). If the file exists, you will be asked if it is OK to overwrite it. The current project will be now named as the saved project name.
* **New Project** - Discards any changes made to the current project since the last save and creates a new empty project.
* **Preferences** - Provides access to GSAS-II configuration settings, as described in the Configuration Variables section.
* **Edit proxy...** - Provides method for changing proxy address for GSAS-II (usually not required – see your computer administration if needed).
* **Ipython console** - Debugging tool.
* **wx.inspection tool** - Debugging tool.
* **Quit** - Exit the GSAS-II program. You will be asked if the project should be saved or not (Cancel aborts the quit). You can also exit GSAS-II by pressing the red X in the upper right (Windows) or left (Mac). Pressing the red X on the console will kill the GSAS-II run without any save.

## **Data** Menu

* **Read Powder Pattern Peaks...** - Read in a list of powder pattern peak positions as either a d-spacing or 2Q position table; these can be used in GSAS-II powder pattern indexing. They are distinguished by their order (highest d or smallest 2Q first in table).
* **Sum or average powder data** - Form the sum of previously read powder patterns; each with a multiplier. Can be used to accumulate data, subtract background or empty container patterns, etc. Patterns used to form the sum must be of identical range and step size. Result is a new PWDR entry in the GSAS-II data tree.
* **Sum image data** - Form the sum of previously read 2-D images; each with a multiplier. Can be used to accumulate data, subtract background or empty container patterns, etc. Images used to form the sum must be of identical size and source. Result is a new IMG entry in the GSAS-II data tree, and a GSAS-II image file is written for future use as a python pickle file.
* **Add new phase** - This begins the creation of a new phase in the data tree (under Phases). You are first prompted in a dialog box for a name to be assigned to the new phase. Then the Phase/General tab is opened for this phase, where you should first select the phase type, the enter the space group symbol and then the lattice parameters.

    !!! Note
        Nonstandard space group symbols are permitted; space group names must have spaces between the axial fields (e.g. use 'P n a 21' not 'Pna21').

* **Delete phase entries** - This will remove a phase from the data tree. A dialog box will present the list of phases; pick one (or more) to delete.
* **Rename tree entry** - Rename a histogram entry. This should only be done before the histogram is used in any phases: e.g. only rename data immediately after reading.
* **Delete data entries** - This will remove a data (e.g. PWDR) item from the data tree. A dialog box with a list of choices for histograms is presented; it has filter capability to ease this process. Be sure to remove histograms from all phases before deleting them from the tree.
* **Delete plots** - This will remove plots from the plot window. A dialog box with a list of choices for plots is presented.
* **Delete sequential result entries** - This will remove any sequential results from the tree. A dialog box with a list of choices for entries is presented.
* **Expand tree item** - This will show child entries for specified type of items (IMG, PWDR, etc.)
* **Move tree item** - Move classes of Tree items (IMG, PWDR, Phase, etc.) around in the tree. Individual top-level tree items can be moved using the right mouse button.

## **Calculate** Menu

* **Setup PDFs** - This creates the pair distribution function (PDF) controls for each powder pattern selected in the dialog box, but does not compute the PDF, which must be done from PDF tree entries. See PDF Controls for information on the PDF input.
* **View LS parms** - This shows a dialog box with all the parameters for your project; those to be refined are flagged 'True', otherwise 'False'. Blanks indicate parameters that are not refinable. The total number of refined parameters is also shown at the top of the list. The value of each parameter is also given. The parameter names are of the form 'p:h:name:id' where 'p' is the phase index, 'h' is the histogram index and 'id' is the item index (if needed). Indexes all begin with '0' (zero).

    !!! Note
        Note that for atom positions, the coordinate values (named as 'p::Aw:n', where p is the phase number, n is the atom number and w is x, y or z) is not a refinable parameter, but the shift in the value is. The refined parameters are 'p::Aw:n'. The reason this is done is that by treating an atom position as x+dx,y+dy,z+dz where the “d” values indicate shifts from the starting position and the shifts are refined rather than the x,y, or z values is that this simplifies symmetry constraints. As an example, suppose we have an atom on a symmetry constrained site, x,1/2-x,z. The process needed to define this constraint, so that if x moves positively y has to move negatively by the same amount would be messy. With refinement of shifts, all we need to do is constrain dy ( '0::dAy:n' ) to be equal to -dx (-1 * '0::dAx:n' ).

        Press the window exit button to exit this dialog box.

* **Refine/Sequential refine** - This performs the refinement (Pawley/Rietveld or single crystal) according to the controls set in the Controls data tree item. This menu item name will reflect the choice of doing a sequential refinement selected in the Controls data tree item.
* **Compute partials** - This runs a zero-cycle refinement where the contributions from each phase (phase partial intensities) are written for each histogram and each phase in that histogram into a single file named project.partials where project is the GSAS-II project (.gpx) name. This file is intended for internal use in GSAS-II and will be deleted if additional refinements are performed (making the information in them obsolete; use this menu command to recreate them if needed.) When the .partials file is created, the user can optionally choose to export the intensity information in a series of ASCII files named prefix_part_N.csv, which can be read by spreadsheets and most scientific software.
* **Run Fprime** - This run the utility routine Fprime that displays real and imaginary components of the x-ray form factors for user selected elements as a function of wavelength/energy. Allows an informed choice of wavelength for resonant x-ray scattering experiments
* **Run Absorb** - This runs the utility routine Absorb that displays the x-ray absorption for a user selected sample composition as a function of wavelength/energy.
* **Run PlotXNFF** - This runs the utility routine PlotXNFF which displays resonant (if any) neutron scattering lengths for all isotopes of a selected element. It also displays the x-ray and magnetic neutron form factors for all valences (if any) for this element.

## **Import** Menu

GSAS-II uses separate routines to read in information from external files that can be created and customized easily. See the GSAS-II Import Modules section of the Programmers documentation for more information on this. Since it is easy to support new formats, the documentation below may not list all supported formats.

* **Image** - Read in 2-D powder diffraction images (multiple patterns can be selected). A submenu appears with choices for import of data. Each entry when selected with the mouse shows further submenus with specific imports that are available. Any of these files can be accessed from a zip file. GSAS-II can read many different image file formats including MAR345 files, Quantum ADSC files, and tiff files from Perkin-Elmer, Pilatus, and GE. Although many of these formats have data fields that should contain relevant information for the exposure (e.g. wavelength), these are rarely filled in correctly by the data acquisition software. Thus, you should have separately noted this information as it will be needed. In some cases, this information may be in a separate “metadata” file; GSAS-II will look for this and attempt to open it as well as the image file.

    !!! Note
        gain maps can be imported but they must be 1000*the gain value (typically ~1) as integers; if used, GSAS-II will rescale the gain map by 1/1000 and apply it to the image.

* **Phase** - Creates a new phase by reading unit cell/symmetry/atom coordinate information. GSAS-II can read information from several different format files including:
    * **GSAS.EXP** - This reads one phase from a (old) gsas experiment file (name.EXP). The file name is found in a directory dialog; you can change directories as needed. Only .EXP (or .exp) file names are shown. If the selected file has more than one phase, a dialog is shown with the choices; only one can be chosen. If you want more than one, redo this command. After selecting a phase, a dialog box is shown with the proposed phase name. You can change it if desired.
    * **PDB file** - This reads the macromolecular phase information from a Protein Data Base file (name.PDB or name.ENT). The file name is found in a directory dialog; you can change directories as needed. Only .PDB (or .pdb) or .ENT (or .ent) file names are shown. Be careful that the space group symbol on the 'CRYST1' record in the PDB file follows the GSAS-II conventions (e.g. with spaces between axial fields). A dialog box is shown with the proposed phase name. You can change it if desired.
    * **CIF file** - This reads one phase from a Crystallographic Information File ({name}.CIF (or .cif). The file name is found in a directory dialog; you can change directories as needed. If the selected file has more than one phase, a dialog is shown with the choices; only one can be chosen. If you want more than one, redo this command. After selecting a phase, a dialog box is shown with the proposed phase name. You can change it if desired.
    * **GSAS-II .gpx file** - This reads one phase from a GSAS-II project file ({name}.gpx). The file name is found in a directory dialog; you can change directories as needed. If the selected file has more than one phase, a dialog is shown with the choices; If you want more than one, redo this command. After selecting a phase, a dialog box is shown with the proposed phase name. You can change it if desired.
    * **guess format from file** - This attempts to read one phase from a file trying the formats as described above. On occasion, this command may not succeed in correctly determining a file format. If it fails, retry by selecting the correct format from the list.

Other formats currently available for import include JANA m50, ICDD str, SHELX ins & res (NB: SHELX files do not contain the space group symbol; you must set it after import), & RMCProfile rmc6f files.

* **Powder Data** - Reads a powder diffraction data set in a variety of formats. Results are placed in the GSAS-II data tree as 'PWDR file name'. Information needed for processing a powder diffraction data set, such as data type, calibration constants (such as wavelength) and default profile parameters are read from a separate file, either a (old) GSAS instrument parameter file (usually .prm, .ins or .inst extension) or a new GSAS-II .instparm file.

    !!! Note
        Note that It is possible to apply corrections to the 2-theta, intensity or weight values by adding a Python command(s) to the instrument (.instprm) parameter with a variable named CorrectionCode. See the CorrectionCode.instprm.sample file provided in the GSAS-II source directory for an example of how this is done.

    * **CIF file** - This reads one powder pattern (histogram) from a Crystallographic Information File ({name}.CIF). The file name is found in a directory dialog; you can change directories as needed. Only one .cif can be chosen. If you want more than one, redo this command.
    * **GSAS-II .gpx file** - This reads powder patterns from a previously created GSAS-II gpx project file. If the selected file has more than one powder pattern, a dialog is shown with the choices; one or more can be selected. It will ask for an appropriate instrument parameter file to go with the selected powder data sets.
    * **GSAS .fxye files** - This reads powder patterns (histograms) from the defined GSAS format powder data files. GSAS file types STD, ESD, FXY and FXYE are recognized. Neutron TOF data with a 'TIME-MAP' are also recognized. The file names are found in a directory dialog; you can change directories as needed. If the selected files have more than one powder pattern, a dialog is shown with the choice(s).
    * **TOPAS .xye files** - This format is a simple 3-column (2-theta, intensity & sig) text file. The file names are found in a directory dialog; you can change directories as needed.
    * **guess format from file** - This attempts to read one data set from a file trying the formats as described above. On occasion, this command may not succeed in correctly determining a file format. If it fails, retry by selecting the correct format from the list.
    * **other supported formats** - Bruker .RAW; FullProf .dat; Panalytical .xrdml; Comma-separated .csv; Rigaku .ras & .txt

    Other formats that are supported, but are only documented in the Powder Data Imports section of the Programmers documentation.

    Note that there are also three separate "special" import items, as documented below:

    * **Simulate a dataset** - This creates a histogram with all zero intensity values in it using a set of instrument parameters that are read in from a file or one of the default sets provided when Cancel is pressed at the prompt to select a file. The user is able to specify the data range and step size. One or more crystalline phases must also be provided to perform a crystallographic simulation. When the "Refine" menu command is initially used, the intensities are computed from these phases and the "observed" intensity values are set from these computed values, with superimposed "random" noise added based on the calculated values as σ=sqrt(I) (increase the histogram scale factor to change this, if desired). Further refinements can then be used to fit to these simulated data. To reset the "observed" intensity values back to zero, to recompute them, use the "Edit range" button on the "PWDR" tree item that is created by this option.

    * **Auto Import** - This brings up a window that reads in powder diffraction files as they are added to a directory. The file extension must determine the importer that will be used and a filter pattern is specified to determine which files will be read (e.g. use "*June23*.fxye" so that only files that contain the string "June23" will be read.

    * **Fit Instr. profile from fundamental parms...** - This option is used to compute instrument parameters from a set of fundamental parameters that describe a constant wavelength (most likely Bragg-Brentano) powder diffraction instrument. The user must first specify the data range to be used and then a set of FP (fundamental parameter) values. The FP values and a source spectrum can be supplied using a nomenclature similar to Topas (described further below). They will then be converted to the SI units and parameter names used in the NIST FPA code. Alternately a file can be supplied with the parameter values used directly in that program. With this input, a series of peaks are computed across the specified data range and the Instrumental Parameters that determine the instrumental profile (U, V, W, X, Y and SH/L) are determined from those peaks. These values are then saved in an instrument parameter file that can be used when reading in new datasets or for pattern simulation.

## Topas-style Fundamental parameters

Description of the Topas-style fundamental parameters used as FPA input for GSAS-II

### Basic Bragg-Brentano parameters

* **Parameter name (Units)** - Description
* **divergence (degrees)** - Angle in equatorial plane describing the sample illumination for a Bragg-Brentano instrument
* **soller_angle (degrees)** - Angular limit for divergence in equatorial plane as limited by Soller collimator(s)
* **Rs (mm)** - Diffractometer radius: source to sample and sample to detector distance
* **filament_length (mm)** - Length of x-ray filament when used in “line-focus” (filament oriented along the axial direction)
* **sample_length (mm)** - Illuminated sample length in axial direction. Typically the same as filament_length.
* **receiving_slit_length (mm)** - Length of the receiving slit in axial direction. Typically the same as filament_length.
* **LAC_cm (cm<sup>-1</sup>)** - :	The linear absorption coefficient adjusted for the sample packing density.
* **sample_thickness (mm)** - Thickness of sample measured along the radial direction in the equatorial plane
* **convolution_steps (none)** - The number of steps used for convolution for each step in the diffraction pattern. This results in more smooth convolutions. 
* **source_width (mm)** - Width of x-ray filament in projection in the equatorial plane.
* **tube-tails_L-tail (mm)** - Width for x-ray intensity occurring beyond the Wehnelt shadow as a projection in the axial direction and measured in the positive two-theta direction.
* **tube-tails_R-tail (mm)** - Width for x-ray intensity occurring beyond the Wehnelt shadow as a projection in the axial direction and measured in the negative two-theta direction.
* **tube-tails_rel-I (none)** - Fractional of x-ray intensity found in the tube tails vs. the main peak. Note that tube tails are modeled as a step function.

### Point detector parameter

* **receiving_slit_width (mm)** - Width of receiving slit placed in front of detector or possibly the diffracted beam monochromator (analyzer) measured in the equatorial plane

### Linear position-sensitive detector parameter

* **SiPSD_th2_angular_range (degrees)** - Angular (two-theta) range in equatorial plane that the entire Si PSD subtends (not implemented in Topas)

### Incident-beam monochromator (IBM) parameters

* **src_mono_mm (mm)** - Distance between the x-ray source (filament) and the monochromator, measured in the equatorial plane
* **focus_mono_mm (mm)** - Distance from monochromator crystal to focus slit, measured in the equatorial plane
* **passband_mistune (none)** - Offset for the tuning of the IBM to the center of the reference line of the spectrum, as a fraction of the IBM bandwidth
* **mono_src_proj_mn (micron)** - Bandwidth setting for the monochromator as set by the projection width of the xray source on the monochromator along beam direction and in the equatorial plane
* **passband_shoulder (none)** - Width of transition region from high-intensity, roughly flat region of the x-ray tube output to the to the tube tails region as a fraction of the IBM bandwidth
* **two_theta_mono (degrees)** - The full diffraction angle of the IBM crystal. This will be double the Bragg two-theta angle for the monochromator
* **mono_slit_attenuation (none)** - The attenuation of the Cu K alpha 2 source lines relative to the K alpha 1 lines as determined by the focal slit

If you use this, please cite M.H. Mendenhall, K. Mullen & J.P. Cline (2015), J. Res. of NIST, 120, p223. [DOI: 10.6028/jres.120.014](https://doi.org/10.6028/jres.120.014). If the incident beam monochromator model is used, please also cite: M.H. Mendenhall, D. Black & J.P. Cline (2019), J. Appl. Cryst., 52, p1087. [DOI: 10.1107/S1600576719010951](https://doi.org/10.1107/S1600576719010951).

* **Structure Factor** - Reads single crystal input from a variety of file types. Results are placed in the GSAS-II data tree as 'HKLF file name'
    * **F\*\*2 HKL file** - This reads squared structure factors (as F\*\*2) and sig(F\*\*2) from a SHELX format .hkl file. The file names are found in a directory dialog; you can change directories as needed. You must know the file contains structure factors (as F\*\*2) as the file itself has no internal indication of this.
    * **F HKL file** - This reads structure factors (as F) and sig(F) from a SHELX format .hkl file. The file names are found in a directory dialog; you can change directories as needed. You must know the file contains structure factors (as F values) as the file itself has no internal indication of this.
    * **CIF file** - This reads structure factors (as F\*\*2 or F) and sig(F\*\*2 or F) from a .CIF (or .cif) or .FCF (or .fcf) format file. The file names are found in a directory dialog; you can change directories as needed. The internal structure of this file indicates in which form the structure factors are used.
    * **guess format from file** - This attempts to read one data set from a file trying the formats as described above. However, since it cannot be determined if SHELX format .hkl contains F or F**2 values, do not use this command for those files. On occasion, this command may not succeed in correctly determining a file format. If it fails, retry by selecting the correct format from the list.

There are specific importers for incommensurate or twinned single crystal data as well as data from specific neutron diffractometers.

* **Small Angle Data** - Reads small angle scattering data from files. Results are placed in the GSAS-II data tree as 'SASD file name'. The data are in 'QIE' form as q-stepped data of intensities and optional sig(I) as 3 (or) 2 columns. Data may be preceded by comment records. Importers are for x-ray or neutron data with q in Å-1 or nm-1; data will be stored in Å-1. The data type is either 'LXC' or 'LNC'
* **Reflectometry Data** - Reads x-ray or neutron reflectometry data from files. Results are placed in the GSAS-II data tree as 'REFD file name'. The data are in 'QIE' form as q-stepped data of intensities and optional sig(I) as 3 (or) 2 columns. Data may be preceded by comment records. The data type is either 'RXC' or 'RNC'.
* **Powder Peak Position Data** - Reads ordered peak positions as 2Q or d-spacing from .txt files. Results are placed in the GSAS-II data tree as 'PKS file name'. The data format consists of optional comments (each line starts with '#') followed by positions in a single column. If 1st position is larger than last, they are interpreted as d-spacings, otherwise as 2Q. A second column of intensities is optional.
* **PDF G(R) Data** - Reads pair distribution data for possible analysis by PDFfit from within GSAS-II.

## **Export** Menu

GSAS-II uses separate routines to write out files with information inside GSAS-II. These routines can be created and customized easily. See the GSAS-II Export Modules section of the Programmers documentation for more information on this. Since it is easy to support new formats, the documentation below may not list all supported formats.

* **Entire project as** - At present there are two supported formats for a project. One is a Full CIF file, which brings up a separate window where information such as ranges for bond distances and angles can be selected. The other is a 2-column text file of parameter name and value(esd), suitable for cutting/pasting into manuscripts.
* **Phase as** - Phases can be exported in a variety of formats including a simplified CIF file that contains only the unit cell, symmetry and coordinates.
* **Powder data as** - Powder data can be exported in number of formats. Note that this menu can also be used to export reflection lists from Rietveld and Pawley fits.
* **Small angle data** - This is exported only as a csv text file.
* **Reflectometry Data** - This is exported only as a csv text file.
* **Single crystal data as** - Single crystal reflection lists can be exported as text files or as a simplified CIF file that contains only structure factors.
* **Image data** - This exports selected images as a portable networks graphics format (PNG) file. Alternately, the image controls and masks can be written for selected images. If strain analysis has been performed on images, the results can also be exported here as a spreadsheet (.csv file).
* **Maps as** - Fourier maps can be exported here.
* **Export all Peak Lists...** - This allows peak lists from selected powder histograms to be written to a simple text file. There will be a heading for each PWDR GSAS-II tree item and columns of values for position, intensity, sigma and gamma follow.
* **Export HKLs** - This allows single crystal reflection lists from selected histograms to be written to a file.
* **Export MTZ file** - This exports macromolecular structure information in a commonly recognized format for input to other macromolecular packages.
* **Export PDF...** - This allows computed PDFs peak lists from selected histograms to be written as two simple text files, {name}.gr and {name}.sq, containing g(r) and s(q), respectively as 2 columns of data; a header on each indicated the source file name and the column headings. The file name comes from the PDF entry in the GSAS-II data tree.

## **Help** Menu

The help menu allows for updating or selecting a GSAS-II version, access to GSAS-II tutorials, or access to documentation on GSAS-II.

* **Check for updates** - This accesses the internet to see if a newer version of GSAS-II is available for download. This will appear as inactive (greyed out) if you do not have write access to the directory where GSAS-II is installed and will not appear in the menu if the subversion (svn) program needed to make updates is not found.
* **Regress to old GSAS-II version** - This allows the GSAS-II version to be set to an older version. One might want to do this to confirm that the result obtained previously is reproduced in the latest code or to avoid a bug that has inadvertently been introduced. Please note that if there is a reason why you feel that an older GSAS-II version does something better than the latest version, please get in touch with the software authors (use of the mailing list, see [https://advancedphotonsource.github.io/GSAS-II-tutorials/mailinglist.html](https://advancedphotonsource.github.io/GSAS-II-tutorials/mailinglist.html) ) is a good way to do this.) Bugs are usually not addressed unless reported and if "improvements" are problematic we want to know this. This will appear as inactive (greyed out) if you do not have write access to the directory where GSAS-II is installed and will not appear in the menu if the subversion (svn) program needed to make updates is not found.
* **Tutorials** - GSAS-II offers more than 50 tutorials on different aspects of the program. If this menu command is used, one can view a tutorial, optionally downloading it and the input files needed to run the tutorial.
* **Help on GSAS-II** - Opens this file, with GSAS-II documentation, in a browser.
* **Help on current data tree item** - Opens the GSAS-II documentation to the code section describing the last selected item in the Data Tree.
