# Importing new flow geometries and flow data

Return to [README](../README.md#new-geometries-and-flow-simulations)

Return to [root](..)

## 1. Add the simulation to the simulator database
- Create a new folder in `geometry_data`: `geometry_data/NEW-FOLDER`
- Move the VTU (.vtu) file to the new folder. 
## 2. Convert the VTU file into a compatible MATLAB file.
- Navigate to the `geometry_data` folder (this should be the MATLAB working directory).
- Modify the script `vtu2matlab.m` ([vtu2matlab.m](../geometry_data/vtu2matlab.m)).
- Specify the name of the newly created subfolder and of the VTU file.
- Complete the metadata.
- Run the script. This will automatically save a `vtu.mat` file in the newly created folder. For large VTU files, this may take some time.
- This code can also generates an STL representation of the geometry, a png image, and all the necessary files for the simulator. You can add you own STL file if you prefer. Such an STL file can readily be generated by ParaView as well.

**Notes:**
- If you intend to run a new CFD simulation on an existing geometry, you will need to use the original .stl files of the mesh, since the .stl file generated by the simulator upon import (see subsection 2) is coarsened.
- Adjust the parameter `vtuProperties.inletDiameter` to approximately match the size of your flow inlet. 
- You may also need to adjust the parameter `vtuProperties.lengthUnit` depending on the length unit of your simulation.
- You may also need to adjust the parameter `vtuProperties.velocityUnit` depending on the velocity unit of your simulation.
- If you encounter an error in the readVTK function, we suggest to open the file with ParaView (free software) and save it as a new VTU file. This should correct the formatting of the simulation file (this manipulation only needs to be done once for a new geometry)
    - Open in ParaView.
    - Click **file**, then **save data**.
    - Select the **.vtu** format

## 3.	Backpropagation to the inlet
The simulator requires a representation of the inlet of the vasculature structure. This representation entails a spatial description of the inlet as well as a statistical distribution of the injection points for the bubbles so as to cover the full vasculature. This is achieved by propagating flow tracers homogeneously distributed in the vasculature back to the inlet using the negative velocities of the CFD simulation files.
- Run the script `backpropagation.m` in the folder `streamline-module`.
- When prompted, navigate to the `geometry_data` folder and select the `vtu.mat` file corresponding to your geometry. (This file was created in the previous step).
- This will create a `backpropagation_points.mat` file recording the location of the end points of the backpropagated streamlines.

Not all backpropagation end points will lie on the inlet surface.
- Run the script `filter_points.m` to apply a set of filters to keep only those backpropagation points that lie on the inlet surface.
- When prompted, select the `vtu.mat` file corresponding to your geometry.
- This will create a file `inlet_points.mat` storing the filtered backpropagation points.

**Note:** You may need to adjust `framerate` depending on the typical velocity in your geometry and `Nstreamlines` depending on your geometry.

## 4.	Generate the density map

To convert the point distribution into a continuous probability density distribution, run the `get_probability_density.m` script located in `streamline-module` folder.
When prompted, navigate to the `geometry_data` folder and select the folder corresponding to your geometry. (The file was created in this folder in the previous steps).

## 5.	Selecting the geometry in the GUI
You can now select the new geometry in the GUI.