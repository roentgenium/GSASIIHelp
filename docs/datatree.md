# GSAS-II data tree items

## Notebook

This window provides a place for you to enter whatever text commentary you wish. Each time you enter this window, a date/time entry is provided for you. A possibly useful technique is to select a portion of the project.lst file after a refinement completes (it will contain refinement results with residuals, new values & esds) and paste it into this Notebook window so it becomes a part of your project file. Some GSAS-II operations (e.g., structure refinement & fourier map calculation) will add entries to the notebook.

### What can I do here?

Use the notebook to keep track of information related to how you use GSAS-II. 

## Controls

This window provides access to the controls that determine how GSAS-II performs minimizations as well as few global parameters for GSAS-II. Note that many other customization settings are set as configuration variables in the Preferences menu. (See the Programmer's documentation for a description of those.)

1. **Refinement Controls**: These controls determine how refinements are performed. The first determines the computational engine used to minimize the structure.

    * **Refinement type** - This determines how structure refinements are performed. The choices are:
        * analytic Hessian: This is the default option and is usually the most useful. It uses a custom-developed least-squares minimizer that uses singular-value decomposition (SVD) to reduce the errors caused by correlated variables and the Levenberg-Marquardt algorithm to up-weight diagonal Hessian terms when a refinement step fails to lower χ2.
        * analytic Jacobian: This uses a numpy-provided leastsq minimizer, which not applicable for problem with a large number of histograms as it requires much more memory than the Hessian routines. This because it creates a Jacobian matrix (J) that is shaped N x M (N parameters x M observations) while the Hessian method create a Jacobian matrix only for each histogram; the N x N Hessian is the made from summing the J x JT products across the histograms
        * numeric: This also uses the numpy leastsq minimizer and is also not applicable for larger problems. Unlike, the "analytic Jacobian", numerical derivatives are computed rather than use the analytical derivatives that are coded directly into GSAS-II. This will be slower than the analytical derivatives and will is often less accurate which results in slower convergence. It is typically used for code development to check the accuracy of the analytical derivative formulations.
        * Hessian SVD: This is very similar to analytic Hessian but does not include the Levenberg-Marquardt algorithm. It can be faster but is more prone to diverge when severe correlation is present. It is possible that it might be better for single-crystal refinements.

    Note that the Jacobian refinement tools are the Fortran MINPACK lmdif and lmder algorithms wrapped in python as provided in the numpy/Scipy package. The Hessian routines were developed for GSAS-II based on routines in numpy and scipy and used material from Numerical Recipes (Press, Flannery, Teulosky & Vetterling) for the Levenberg-Marquardt algorithm. The lmdif and lmder routines were written by Burton S. Garbow, Kenneth E. Hillstrom, Jorge J. More (Argonne National Laboratory, 1980).

    * **Min delta-M/M** - A refinement will stop when the change in the minimization function (M=Σ[w(Io-Ic)2]) is less than this value. The allowed range is 10-9 to 1.0, with a default of 0.001. A value of 1.0 stops the refinement after a single cycle. Values less than 10-4 cause refinements to continue even if there is no meaningful improvement.
    * **Max cycles** - This determines the maximum number of refinement cycles that will be performed. This is used only with the "analytical Hessian" and "Hessian SVD" minimizers.
    * **Initial lambda** - Note that here λ is the Marquardt coefficient, where a weight of 1+λ is applied to the diagonal elements of the Hessian. When λ is large, this down-weights the significance of the off-diagonal terms in the Hessian. Thus, when λ is large, the refinement is effectively one of steepest-descents, where correlation between variables is ignored. Note that steepest-descents minimization is typically slow and may not always find the local minimum. This is only used when with the "analytical Hessian" minimizer is selected.
    * **SVD zero tolerance** - This determines the level where SVD considers values to be the same. Default is 10-6. Make larger to where problems occur due to correlation. This is used only with the "analytical Hessian" and "Hessian SVD" minimizers.
    * **Initial shift factor** - This provides an initial scaling ("damping") for the first cycle of refinement. Only available for "analytic Jacobian" and "numeric" minimizers. 

2. **Single Crystal Refinement Settings**: A set of controls is provided for control of single-crystal refinements. These only appear when single crystal (HKLF) histograms are present in the project.

    * **Refine HKLF as F^2?** - When checked, refinements are against F2 rather than \|F\|.
    * **Min obs/sig** - Conventional cutoff for single crystal refinements as to what reflections should be considered observed, typical values are 2.0 (2 sigma) or 3.0 (3 sigma).
    * **Min extinct.** - Reflections with extinction corrections larger than this value are ignored.
    * **Max delt-F/sig** - Removes reflections that are very poorly fit. Should be used only with extreme care, since poorly-fit reflections could be an indication that the structure is wrong.
    * **Max d-spacing** - Reflections with d-space values larger than this value are ignored.
    * **Min d-spacing** - Reflections with d-space values smaller than this value are ignored. 

3. **Sequential Settings**: A set of controls is for sequential refinement. Settings here determine if a "normal" or "sequential" refinement is performed. If no datasets are selected here, then all histograms linked to phases in the project and that are flagged as "used" are included in one potentially large (combined) refinement. However, if any number of histograms are selected here, then a sequential refinement is performed, where a fit is made to the structural model(s) fitting each selected histogram in turn. Only the first item below is shown in "normal" mode.

    * **Select datasets/Reselect Datasets** - This brings up a menu where histograms can be selected, which potentially switches between a normal and a sequential refinement. If one or more histograms are selected, a sequential refinement is used. If none are selected, then the refinement be set as "normal". The button is labeled "Select" when in normal refinement mode and "Reselect" in sequential refinement mode.
    * **Reverse order?** - Normally, in a sequential refinement histograms are fit in the order they are in the data tree (which can be reordered by dragging tree items), but when this option is selected, the sequential fit is performed with the last tree entry first.
    * **Copy results to next histogram?** - When this option is selected, the fitted parameters from each refinement are copied to the next histogram, so that the starting point for each refinement will be the results from fitting the previous. This works well for parametric experiments where parameters such as the lattice parameters change gradually over the course of successive measurements. This option is usually used only for the initial refinement after a sequential fit is started and the setting is cleared once that refinement is completed. For subsequent refinements, it is usually better to start with the results from the previous fit.
    * **Clear previous seq. results** - When this button is pressed, the "Sequential Results" entry with the results from the last sequential fit is deleted from the tree.

4. Global Settings: This is a location for parameters that apply to an entire project. At present there is only one.

    * **CIF Author** - The value provided here is used when creating a CIF of an entire project. 

### What can I do here?

This offers a place to change how GSAS-II performs refinements but has no specific menu commands or graphics.

## Covariance

This window contains residual information after the last refinement. When this tree item is selected, the GSAS-II Plots window shows '**Covariance**', with a graphical representation of the variance-covariance matrix. The text in the data window shows statistical values related to the current fit. At the bottom of this list are buttons that display a horizontal bar chart with the shift to standard uncertainty ratio for each parameter.

* The button labeled "last refinement" shows these ratios based on the differences between each parameter value from the beginning of the refinement run and the value at the end of the refinement run.
* The button labeled "last cycle" shows these ratios based on the differences between each parameter value at the beginning of the last refinement least squares cycle and the value at the end of the cycle (and the refinement run). (These "last cycle" values are only available when the Controls are set for an "analytic Hessian" refinement.) 

### What is plotted here?

The variance-covariance matrix as a color-coded array is shown on this page. The color bar to the right shows the range of covariances (-1 to 1) and corresponding colors. The parameter names are to the right and the parameter numbers are below the plot. 

### What can I do with the plot?

* Move the mouse cursor across the plot. If on a diagonal cell, the parameter name, value and esd is shown both as a tool tip and in the right-hand portion of the status bar. If the cursor is off the diagonal, the two parameter names and their covariance are shown in the tool tip and the status bar.
* Use the Zoom and Pan buttons to focus on some section of the variance-covariance matrix.
* Press 's' – A color scheme selection dialog is shown. Select a color scheme and press OK, the new color scheme will be plotted. The default is 'RdYlGn'.
* Press 'p' – Saves the covariance values in a text file.

## Constraints

This window shows the constraints to be used in a refinement. Constraints are divided with a tab for each type: Phase, Histogram/Phase, Histogram, Global and Sym-Generated. Note that the standard parameters in GSAS-II are divided into three classes and appear respectively on the Phase, Histogram and Histogram/Phase tabs:

* those pertaining to quantities in each phase (naming pattern "p::name"); examples include atom coordinates, thermal motion and site fraction parameters;
* those pertaining to quantities in each histogram (naming pattern ":h:name"); such parameters are those that depend only on the data set: the scale factor and profile coefficients (e.g. U, V, W, X and Y);
* and those pertaining to quantities defined for each histogram in each phase (naming pattern "p:h:name"); these parameters are quantities that can be dependent on both the phase properties and the sample or dataset used for the measurement. Examples include phase fractions and sample-broadening coefficients such as microstrain and crystallite size; they are found in the Data tab for each phase.

The following types of constraints may be specified by users:

* **Holds** - Use this to prevent a parameter from being refined. Most valuable when refinement of a parameter is selected in a group for refinement (such as x, y & z for an atom or unit cell parameters) and one must be fixed. For example, if the space group for a phase has a polar axis (e.g., the b-axis in P21), then the origin with respect to b is arbitrary and it is not possible to refine the y coordinates for all atoms. Place a Hold on any one atom y coordinate to keep the structure from drifting up or down the y-axis during refinement.
* **Equivalence assignments** - Determines a set of parameters that should have values with a specified ratio (except for atom coordinates, where the ratios are specified for the applied shifts, not the actual coordinate values) Examples for typical use are sets of atoms that should be constrained to have the same displacement parameters (aka thermal motion, Uiso, etc.) or sets of profile coefficients U,V,W across multiple data sets. Note that the first selected parameter is treated as independent, and the remainder are "slaved" to that parameter as "dependent parameters." All parameters in an equivalence must be varied. If any parameter is not varied or is given a "hold," a warning is displayed and none of the parameters are refined.
* **Constraint Equations** - Defines a set of parameters whose sum (with possible non-unitary multipliers) will be equal to a constant. For example, a common use for this is to specify the sum of occupancies for atoms sharing a site have a sum fixed to unity or so that the sum of occupancies for an atom type that is occurs on several sites is fixed to match a composition-determined value. Note that all parameters in the equation are considered as "dependent parameters." If a parameter in a constraint equation is held or is not varied, that parameter is removed from the equation (the sum value is modified accordingly). If no parameters remain the equation is ignored.
* **New Var assignment** - These are similar to constraint equations in that they define a set of parameters and multipliers, but rather than specifying a value for the expression, a new parameter is assigned to that sum and these constraints have a very different function. This replaces a degree of freedom from the original variables with a new one that modifies the parameters where the shift is applied according to the ratio specified in the expression. This can be used to create new parameters that redefine the relationships between items such as coordinates or magnetic moments. The new parameter may optionally be named by the user. The new var expression creates a new global parameter, where that new parameter is independent, while all the parameters in the expression are considered as dependent. The setting of the refine flags for the dependent parameters is not used. Only if the new var parameter is marked as refine then it will be refined. However, if any dependent variable is set as "hold," the new var parameter will not be refined.

Note that when new var and constraint equation constraints are defined, they create new global parameters. Constraints on these will be rare, but can be managed on the Globals tab. Finally, some constraints are defined automatically based on restrictions determined by space group symmetry. These constraints can be seen, but not changed, using the Sym-Generated tab. Other constraints (holds) will be created when rigid bodies are specified.

New Var constraints are generated when ISODISTORT is used to develop mode distortions from a comparison of a high symmetry parent structure (e.g. cubic perovskite) with a distorted child substructure. They are developed for the phase imported from the special cif file produced by ISODISTORT from a mode distortion analysis.
What can I do here?

### What can I do here?

Select the tab for the parameter type(s) you wish to constrain then create new parameters using the "Edit Constr." menu commands:

* **Add Hold** - Select a parameter that you wish to remain fixed. If selected, a dialog box will appear showing the list of available parameters; select one and then OK to implement it, Cancel will cancel the operation. The held parameter will be shown in the constraint window with the keyword 'FIXED'.
* **Add equivalence** - Select the independent parameters and press OK; a second dialog box will appear with only those parameters that can be made equivalent to the first one. Choose those and press OK. Cancel in either dialog will cancel the operation. The equivalenced parameters will show as an equation of the form M1*P1+M2*P2=0; usually M1=1.0 and M2=-1.0 but can be changed via the 'Edit' button. The equation(s) are shown in the window tagged by 'EQUIV' to mark it as an equivalence assignment.
* **Add constraint** - If selected, a dialog box will appear with a list of the available parameters. Select one and press OK; a second dialog box will appear with only those parameters that can be used in a constraint with the first one. Choose those and press OK. Cancel in either dialog will cancel the operation. The equivalenced parameters will show as an equation of the form M1\*P1+M2\*P2+…=C; the multipliers M1, M2, … and C can be changed via the 'Edit' button. The equation is shown in the window tagged by 'CONSTR' to mark it as a constraint equation assignment.
* **Add New Var** - This behaves very much like the "Add constraint" menu command except that it defines a new parameter rather than define a value for the expression. That new var parameter can optionally have a named assigned. The expression is displayed with the keyword 'New Var' to mark its type. Note that a 'Refine?' box is included for this type of constraint.
* **Make atoms equivalent** - This provides a shortcut for establishing constraints when two share a single site. Coordinates and Uiso values are constrained to be the same and site fractions are constrained to add to 1.
* **Show ISODISTORT modes** - Used after a CIF from the ISODISTORT web site is read, which will display the values for the normal modes from representational analysis from the coordinates.

In addition to menu commands, this window also offer the following actions by pressing buttons:

* **Show Errors** - this button will be active if serious errors -- that would prevent a refinement from being performed -- are encountered processing the constraints.
* **Show Warnings** - this button will be active if correctable problems are encountered in processing the constraints, such as a constraint being rejected because a parameter is not varied. These warning may indicate that the choice of which parameters will be refined is not what was planned.
* **Show Generated Constraints** - After constraints have been processed, a series of relationships are developed to determine new variables from the current parameters and "inverse" equations that determine dependent parameters from the new variables and independent parameters. This shows the resulting relationships, as well as any "Hold" variables.
* **Delete Selected** - This button will cause all the selected constraints on the current tab to be deleted.
Sequential Refinement Constraints

While all the general information on constraints (above) applies to sequential refinements, the sequential refinement is performed by fitting each histogram individually and this affects how constraints are defined and processed for parameters keyed to a particular constraint number. When sequential refinement is selected (via the Controls tree item), it becomes possible to define constraints of form "p:*:name" and ":*:name" (where "p" is a phase number and name is a parameter name). The "*" here is called a wildcard, and in a constraint or equivalence will cause that to be used for every histogram in turn.

In sequential refinement mode, two additional controls are shown in the Constraints window. The first, which is labeled wildcard use, specifies what is done when a specific histogram is referenced by number in a constraint or equivalence by number rather than by wildcard. This offers three modes:

* **Set hist # to \*** - Any constraints previously specified with a specific histogram number will be changed to apply to all histograms. This is the default for new projects.
* **Ignore unless hist=\*** - Any constraints previously specified with a specific histogram number will be ignored and constraints with a "*" where for a histogram number will be used. Note that constraints on phase parameters (of form "p::name" -- without a histogram number specified) will be used normally. Note that this was the normal operating mode for GSAS-II in earlier versions.
* **Use as supplied** - If different constraints are to be applied to different histograms, it becomes necessary to create constraints with specific histogram numbers. In this mode, constraints specified with a specific histogram are applied only to that histogram while wildcarded ones are applied to all histograms. Note that one should not specify two constraints on a single parameter, one with a wildcard and one with a specific histogram number as both will be applied to the specified histogram which will result in an unsatisfiable conflict.

Also included when sequential refinement is selected is a menu button labeled "Selected histogram." With this it is possible to look at constraint problems when processing a specific histogram.

## Restraints

This window shows the restraints to be used in a refinement for each phase (if more than one). It is organized into several tabbed pages, one page for each type of restraint. Restraints are developed for an individual phase and act as additional observations to be "fitted" during the refinement.

### What can I do here?

* Select the tab for the restraint type you wish to use. Each will have the same possibilities in the 'Edit' menu.
* You can change the Restraint weight factor – this is used to scale the weights for the entire set of restraints of this type. Default value for the weight factor is 1.0.
* You can choose to use or not use the restraints in subsequent refinements. Default is to use the restraints.
* You can change the search range used to find the bonds/angles that meet your criteria for restraint.
* You can examine the table of restraints and change individual values; grayed out regions cannot be changed. The 'calc' values are determined from the atom positions in your structure, 'obs' values are the target values for the restraint and 'esd' is the uncertainty used to weight the restraint in the refinement (multiplied by the weight factor).
* Menu 'Edit' – some entries may be grayed out if not appropriate for your phase or for the selected restraint.

    * **Add restraints** - this takes you through a sequence of dialog boxes which ask for the identities of the atoms involved in the restraint and the value to be assigned to the restraint. The esd is given a default value which can be changed after the restraints are created.
    * **Add residue restraints** - if the phase is a 'macromolecule' then develop the restraints from a selected 'macro' file based on those used in GSAS for this purpose. A file dialog box is shown directed to /GSASIImacros; be sure to select the correct file.
    * **Add MOGUL restraints** - add restraints in the style of the MOGUL program
    * **Plot residue restraints** - if the phase is a 'macromolecule' and the restraint type is either 'Torsion restraints' or 'Ramachandran restraints', then a plot will be made of the restraint distribution; torsions as 1-D plots of angle vs. pseudopotential energy and Ramachandran ones as 2-D plot of psi vs phi. In each case a dialog box will appear asking for the residue types or specific torsion angles to plot. Each plot will show the observed distribution (blue) obtained from a wide variety of high-resolution protein structures and those found (red dots) for your structure. The restraints are based on a pseudopotential (red curve or contours – favorable values at the peaks) which has been developed from the observed distributions for each residue type.
    * **Change value** - this changes the 'obsd' value for selected restraints; a dialog box will appear asking for the new value.
    * **Change esd** - this changes the 'esd' value for selected restraints; a dialog box will appear asking for the new value.
    * **Delete restraints** - this deletes selected restraints from the list. A single click in the blank box in the upper left corner of the table will select/deselect all restraints.

## Rigid bodies

This window shows the rigid body models that have been entered into GSAS-II for this project. There are two tabs; one is for vector style rigid bodies and the other is for flexible "Residue" rigid bodies. Note that these rigid bodies must be inserted into one of the phases before it can take effect in the crystal structure description.

### What can I do here?

* Select the tab for the rigid body type you wish to use. Each will have the different possibilities in the 'Edit' menu depending on whether a rigid body has been defined.
* Menu '**Edit Vector Body**' or '**Edit Residue Body**' – the entries listed below depend on which type of rigid body is selected.

    * **Add rigid body** - (Vector rigid bodies) this creates a vector description of a rigid body. A dialog box asks the number of atoms (>2) and the number of vectors required to create the rigid body. An entry will be created showing a magnitude with the vector set to be applied for each vector needed to develop the rigid body.
    * **Import XYZ** - (Residue rigid bodies) this reads a text file containing a set of Cartesian coordinates describing a rigid body model. Each line has atom type (e.g. C, Na, etc.) and Cartesian X, Y and Z.
    * **Define sequence** - (Residue rigid bodies) this defines a variable torsion angle in a sequence of dialog boxes. The first one asks for the origin and the second asks for the pivot atom for the torsion from the nearest neighbors to the origin atom; the atoms that ride on the selected torsion are automatically found from their bond lengths.
    * **Import residues** - (Residue rigid bodies) this reads a predetermined macro file that contains standard (Engh & Huber) coordinates for the amino acids found in natural proteins along with predetermined variable torsion angle definitions.

* Once a rigid body is defined you can plot it, change its name or manipulate any torsion angle to see the effect on the plot.
* The translation magnitudes in a vector rigid body can be refined.

## Sequential Refinement Results

This tree entry becomes available after a sequential fit has been run. Note there are the following types of sequential fits:

1. Rietveld: Sequential results
2. PDF: Sequential PDFfit2 results
3. Peak fit: Sequential peak fit results
4. Small angle: Sequential SASD fit results
5. Reflectometry: Sequential REFD results
6. Image (strain): Sequential strain fit results
7. Image (calibration): Sequential image calibration results 

Each sequential fitting process within GSAS-II will have its own differently named set of sequential results, as listed above. When any of these tree items is selected, the window tabulates the sequential fit results. The columns are the parameter names; the naming convention is generally 'p:h:name:n' where 'p' is the phase number,' h' is the histogram number, 'name' is the parameter name, and 'n' (if needed) is the item number (e.g. atom number). The rows are the data sets used in the sequential refinement.

For a sequential Rietveld refinement, set the PWDR histograms to be used in the sequential refinement in the Controls tree item. Note that the Calculate/Refine menu command will be renamed as "Sequential Refine." Since not all histograms need be used in a sequential Rietveld fit, after a sequential fit, histograms that have been fit will be included in the table. Previously fit histograms will not be removed unless the table is cleared (in the Controls tree item). In this way, a large sequential fit may be worked on in sections. The first column of the table will list the histogram number and the number will be shown in red if it was not fit in the last sequential refinement.

### What can I do here?

* **Select a row** - a right mouse button will display the variance-covariance matrix for the refinement with that data set; a left mouse button will display its powder data fit.
* **Select a column** - this will display a plot of that parameter across the sequence of data sets. Error bars for each value are also shown. Selecting multiple columns (hold Ctrl key down for subsequent picks) will plot all as individual curves.

* Menu '**Columns/Rows**' -

    * **Set used** - this allows you to select in a dialog which entries to use; those not used are not plotted or used in further processing.
    * **Update phase from row** - this updates the phase parameters from the entries in the selected row. Normally the phase parameters at the end of a sequential fit are those obtained from the last histogram.
    * **Set phase vals** - same as previous except you can pick which parameters to update.
    * **Plot selected cols** - plots the selected columns (redundant as selecting the columns automatically plots them)
    * **Rename selected cols** - can change column names with this
    * **Save selected as text** - gives a txt file with columns of data from those selected.
    * **Save selected as CSV** - gives a comma separated values (CSV) file of selected columns.
    * **Compute average** - gives average(esd) for selected column values.
    * **Hide columns** - you can select/deselect columns to not show in table.
    * **Save all as CSV** - gives a CSV file for all table entries.

* Menu '**Pseudo Vars**' – this is used to create derived results from sequentially refined parameters; new columns are the result.

    * **Add Formula** - create a formula used to make a derived result.
    * **Add Distance** - adds a new column for a specific interatomic distance.
    * **Add Angle** - adds a new column for a specific 3-atom angle.
    * **Delete** - to remove a pseudo variable formula.
    * **Edit** - to change a selected formula.

* Menu '**Parametric Fit**' - this is used to create fitting models for any column of sequential results.

    * **Add equation** - add a parametric fitting equation. At the end of this step, it will be used to give refined values of the coefficients with esds based of a full error propagation from the variance-covariance matrices from the individual refinements.
    * **Copy equation** - make a copy of a parametric equation.
    * **Delete equation** - to remove a parametric equation.
    * **Edit equation** - to edit an equation.
    * **Fit to equation(s)** - do the fitting of the parametric equations to the data.
* Menu '**Seq export**' –
    * **Project as** - only choice is as a full cif file.
    * **Phase as** - either a "quick' cif or a CSV file
    * **Powder as** - either a powder pattern cif, a histogram CSV file or a reflection list CSV file.
    * **Save table as CSV** - same as Save all as CSV above.

### What can I do with the plot?

By default, the plot shows the variation of the selected parameters across the sequence of histograms used in the sequential fit. Each point that was fitted shows as an x with a vertical bar indicating the standard error from the fit for that value. There are some key commands:

* Press 'l' – toggles display of connecting lines between the data points
* Press 's' – this presents a choice of parameters from the table columns to be used for the x-axis. Typically, this is used to show parameter variation with e.g. temperature.
* Press 't' – this provides access to all three titles of the plot.

## Cluster Analysis

Cluster analysis is a suite of data survey techniques where data are grouped by some measure of their similarity. Thus, it can be used as a preliminary survey of a large number of data sets in e.g. preparation of detailed examination of representative members. In the case of powder diffraction pattern (PWDR) data or pair distribution (PDF) data, their similarity is determined by considering each pattern as a hyper-dimensional vector with one dimension for each data point and then computing some measure of how parallel pairs of these vectors are. Consequently, it can be used to survey PWDR data entries that have identical scan characteristics (e.g. instrument type, step size, radiation type, wavelength) or multiple PDF G(R) entries created with the same step sizes and using the same radiation from data collected with identical instrument configurations. Cluster analysis is available in GSAS-II after it is initiated by the main menu command **Calculate/Setup Cluster Analysis**. The cluster analysis routines used here are from the scipy library and (if available) the scikit-learn library.  If scikit-learn is absent, an attempt is automatically made to install the latter via the conda system from Anaconda. The scipy library provides some cluster analysis tools while the scikit-learn package provides others. If you use results from scikit-learn, please cite the following in any publication that uses it:

"Scikit-learn: Machine Learning in Python", Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., Blondel, M., Prettenhofer, P., Weiss, R., Dubourg, V., Vanderplas, J., Passos, A., Cournapeau, D., Brucher, M., Perrot, M. and Duchesnay, E., (2011). Journal of Machine Learning Research 12, 2825-2830.

### What can I do here?

## Cluster Analysis with scipy

Doing cluster analysis in GSAS-II requires several steps; new steps will become visible in the GUI as previous ones are completed. Redoing earlier steps may clear subsequent ones. In order of their appearance, the following GUI commands are:

* **Select datasets** - this brings up a selection tool for PWDR (& PDF, if present) entries in the GSAS-II data tree. Your selection must be either PWDR or PDF data; otherwise, there is no check on data similarity so be careful with your selections. Multi-bank TOF data should not be mixed for cluster analysis nor should laboratory and synchrotron data. Cluster analysis on fewer than 5-10 data sets is probably not useful but should be used when you have dozens or even hundreds of data sets.
* **Data limits** - selection of data is followed by entries for the minimum and maximum data limits; the defaults are taken from the data Limits imposed on the original PWDR data or the r-range for the PDF G(R) data. The units are degrees 2Q, TOF in μs, or Å, as appropriate. Refer to any PWDR (or PDF) plot to select these values; leading background should be skipped, and the upper limit chosen from a relatively clear point where there are still significant peaks. Values will be used to give the cluster analysis input data matrix size.
* **Make Cluster Analysis data array** - this button forms the data matrix for cluster analysis; it is number of data sets times number of data points between the limits in size. the next item will appear in the GUI.
* **Select cluster analysis distance method** - there are several choices as what is meant by "distance" between all pairwise selection of data vectors (u & v). They are (as taken from scipy):

    * **braycurtis** – Computes the Bray-Curtis distance between the data vectors as

    $$
    d(u,v) = \frac{\sum_i | u_i - v_i | }{\sum_i | u_i + v_i |}
    $$

    * **canberra** – Computes the Canberra distance between data vectors as:

    $$
    d(u,v) = \sum {\frac{ | u_i - v_i | }{| u_i | + |v_i |} }
    $$

    * **chebyschev** – Computes the Chebyschev distance between data vectors as:

    $$
    d(u,v) = \max { | u_i - v_i | }
    $$

    * **cityblock** (sometimes called "Manhattan") – Computes the city block distance between data vectors as:

    $$
    d(u,v) = \sum { | u_i - v_i | }
    $$

    * **correlation** – Computes the correlation distance between data vectors as:

    $$
    d(u,v) = 1 - \frac{ ( u - \bar{u} ) \cdot ( v - \bar{v} ) }{ \sqrt{ ( u - \bar{u} )^2 ( v - \bar{v} )^2}}
    $$

    * **cosine** – Computes the cosine squared between the data vectors as:

    $$
    d(u,v) = 1 - \frac{ u \cdot v }{ \sqrt{ u^2 v^2 } }
    $$

    *  **euclidian** (default) – Computes the Euclidian distance between the data vectors as:

    $$
    d(u,v) = \sqrt{ \sum_i ( u_i - v_i )^2 }
    $$

    * **jensenshannon** – Computes the Jensen-Shannon distance between the data vectors

    * **minkowski** – Computes the Minkowski distance between the data vectors as:

    $$
    d(u,v) = \sqrt[p]{ \sum_i {( u_i - v_i )^p } }
    $$

    where the exponent, p, = 2 by default; this is identical to the Euclidian formula. Some choices for p: 1 is the same as city block, and 10 (~  ∞) is essentially the same as Chebyschev. The others (3 & 4) give distance results that are between Euclidian (p=2) and Chebyschev (p=10 ~ ∞).

    * **seculidian** – Computes the standardized Euclidian distance between the data vectors as:

    $$
    d(u,v) = \sqrt{ \sum_i {( u_i - v_i )^2 }/V[X_i] }
    $$

    where the variance, V[xi], is computed automatically as the variance in the data point values for each data position (i.e. 2Q) across the entire data array.

    * **sqeuclidian** – Computes the squared Euclidian distance between the data vectors as:

    $$
    d(u,v) =  \sum_i {( u_i - v_i )^2 }
    $$

Changing the method results in an automatic calculation of the distances; the Compute button is provided for convenience. The result of this calculation is displayed as the 1st plot in a plot tab named for the selected distance method; this facilitates comparison between methods for your data. Also shown in this plot tab is a 3D plot of the result of a Principal Component Analysis (PCA) of the distance data; it shows the location of each data set in this space. Clusters may be evident from this plot; variable temperature scans tend to show a complex path of distance points with cluster grouping corresponding to phases. Since data sets may be in a series, a plot of the serial distances across the suite of data is shown; spikes in a temperature series may indicate phase changes. The GUI will be extended to show more steps in cluster analysis.

* **linkage method for hierarchical clustering** - there are several choices for linkage in determining the hierarchical relationship (if any) between the data sets and the algorithm will use the distance matrix determined above to determine the data hierarchy. The distances are used by the linkage method to group similar data into clusters; these are successively combined until just a single cluster is obtained. A dendrogram is displayed showing the progression of this clustering. Each cluster is given a mean position, s or t, to compare to the others. The linkage methods for calculating the distance (using the distance method, dist, as selected above) between each pair of clusters are:

    * **single** – computes the linkage as:

    $$
    d(s,t) = \min(dist(u(s)_i,v(t)_j))
    $$

    * **complete** – computes the linkage as:

    $$
    d(s,t) = \max(dist(u(s)_i,v(t)_j)
    $$

    * **average** (default) – computes the linkage as (Ns, Nt are numbers of members in cluster s & t, respectively):

    $$
    d(s,t) = \sum_{ij} {\frac{  dist(u(s)_i,v(t)_j)}{N_s N_t}}
    $$

    * **weighted** – computes the linkage when s is formed with clusters u & v and t is another cluster as:

    $$
    d(s,t) = (dist(u,t)+dist(v,t))/2
    $$

    * **centroid** ("UPGMC") – computes the linkage when cs & ct are the centroids of clusters s & t, respectively as:

    $$
    d(s,t) = (dist(c_s, c_t)
    $$

    * **median** ("WPGMC") – computes the linkage when ms & mt are the medians for all member pairs in clusters s & t as:

    $$
    d(s,t) = (dist(m_s, m_t)
    $$

    * **ward** – computes the linkage when s is formed with clusters u & v and t is another cluster as (Nu, Ns, Nt are numbers of members in cluster u, s & t, respectively, & T=Nu+Nv+Nt):

    $$
    d(s,t) = \sqrt{ \frac{N_t+N_u}{T}dist^2(t,u) + \frac{N_t+N_v}{T} dist^2(t,v) - \frac{N_t}{T} dist^2(u,v) } 
    $$

    Changing the linkage method results in an automatic recalculation of the hierarchical clustering; a Compute button is provided for convenience. The result of this calculation is shown as a dendrogram in the same plot tab; the 4th plot shows the percentage contribution of the leading terms in the PCA to the distance data. Usually, 2-3 terms are sufficient to describe the distribution.

* **Select number of clusters** for K-means clustering (scipy algorithm). The algorithm attempts to group the data points (e. g. as in the PCA plot) into the requested number of clusters based on Euclidian distances on a "whitened" data array (i. e. not the distance matrix). To whiten the data matrix the suite of values at each position (e. g. at each 2Q) are divided by its standard deviation; this reduces the scale of the PWDR & PDF observations to just numbers of standard deviations from zero. Use the Compute to repeat the K-means clustering; the start points are randomly selected and will sometimes yield different results. Cluster populations are shown in the GUI, clusters are colored to match the data point colors in the PCA plot.

    * **Select cluster to list members** – Shows a colored list of the data items that belong to the selected cluster.
    * **Select cluster member** (use mouse RB on item in displayed list) – Displays the PWDR (or PDF) data on the Powder Pattern plot tab for the selected item.

* **Plot selection** – changes the displayed plots:
    * **All** – All four plots are shown
    * **Distances** – Only the distance matrix is shown
    * **Dendogram** – Only the hierarchical dendrogram is shown.
    * **3D-PCA** – Only the 3D representation of the Principal Component Analysis is shown.
    * **Diffs** – Only the serial differences are shown.

## Cluster Analysis with scikit-learn

The next section of the GUI only appears if the scikit-learn package is installed. It has multiple algorithms for doing clustering and detecting outliers (i. e. bad data) in the suite of PWDR or PDF patterns. Changing the method or number of clusters results in an automatic calculation; the **Compute** button is provided for convenience. There is a reminder to properly cite Scikit-learn if you use it.

* **Select clustering method** – some may also require selecting number of clusters

    * **K-Means** (requires number of clusters) – Uses the scikit-learn "K-means++" algorithm for clustering; this gives a better starting position and usually succeeds on the 1st try. It uses the "whitened" data matrix.
    * **Affinity propagation** – It uses the distance matrix computed above.
    * **Mean-shift** – It uses the "whitened" data matrix.
    * **Spectral clustering** (requires number of clusters) – It uses the "whitened" data matrix.
    * **Agglomerative clustering** (requires number of clusters) – It uses the distance matrix computed above.

For details of these methods, please see [2.3. Clustering — scikit-learn 1.1.2 documentation](https://scikit-learn.org/stable/modules/clustering.html). After completion, Cluster populations are shown in the GUI and clusters are colored to match the data point colors in the PCA plot.

## Outlier Analysis with scikit-learn

After selection of the PWDR or PDF data and doing the distance calculation, one can examine the distance data for possible "bad" data items. These outliers can be detected by a choice of methods that make different assumptions how the data "should" be clustered; any data that do not fall within them are flagged as outliers and are colored different in the resulting 3D PCA plot from all others that would be in clusters. Although the chosen distance method affects the appearance of the 3D PCA plot, the three outlier methods all use the original data, thus are independent of any selected distance method. The GUI is refreshed showing a listing of the outlier data; selection of any, displays that data item in the powder pattern plot tab. Any previous cluster identification, e. g. by K-means, is erased. The outlier detection methods are:

* **One-Class SVM** - Attempts to form boundaries around the clusters; outliers are items that fall outside the boundaries.
* **Isolation Forest** - Similar to the above but uses a different algorithm.
* **Local Outlier Factor** - Uses the local density of other points about each point to determine if it is within a high-density area, i. e. in a cluster, or not.

Further details of these methods can be found at [2.7. Novelty and Outlier Detection — scikit-learn 1.1.2 documentation](https://scikit-learn.org/stable/modules/outlier_detection.html). The current GSAS-II implementation of these methods all use the default settings for any of their respective parameters.

### What can I do with the plots?

For each selection of distance method, i.e. "Euclidian", a plot tab is created with 2 or 4 plots. They are: 1\) the distance matrix displayed in the same way the refinement covariance matrix is displayed (default coloring is "paired" – same parameter as the powder pattern contour plot); 2\) the 3D PCA analysis plot; 3\) the hierarchical dendrogram plot and 4\) the PCA percent contribution plot. Each can be zoomed independent of the others and the 1st three can be selected to show as a single plot in the tab (see **Plot selection** above). A LB mouse selection (& hold button down) of a 3D PCA point will show the data set name in the plot status line. If clusters are determined by e. g. K-means, the 3D PCA points will be colored by cluster membership.
