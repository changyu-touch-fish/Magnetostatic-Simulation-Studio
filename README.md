# Magnetostatic-Simulation-Studio
This desktop application is used for magnetostatic simulation of hard magnets, current-carrying coils, soft magnets, and an optional uniform external magnetic field.

Please keep this whole folder together after extraction.


How To Run
----------

1. Extract the full MagnetostaticSimulation folder.
2. Do not delete or move the _internal folder.
3. Double-click:

   MagnetostaticSimulation.exe


Main Capabilities
-----------------

The program can calculate:

- magnetic field and field gradient at a selected point
- force on a selected magnetic component
- force from all other enabled components on hard magnet 1/2/3 or coil 1/2/3
- force on the soft magnet
- a 3D all-field cloud around the enabled magnetic components
- 2D field slices on x, y, or z planes, with contour map and B-direction arrows
- CSV data output for point sequence, current sequence, and all-field results


Supported Components
--------------------

Hard magnet:
- up to 3 hard magnets
- cuboid
- sphere
- cylinder
- ring cylinder
- STL mesh

Coil:
- up to 3 coils
- circular coil
- rectangular coil
- fixed current or linear current sequence

Soft magnet:
- one soft magnet only
- cuboid
- sphere
- cylinder
- ring cylinder
- STL mesh

Only one soft magnet is supported because the self-consistent demagnetization
kernel is assembled for one soft-magnet voxel set.


Uniform External Field
----------------------

In Global Output Settings, the optional Uniform field B [T] can be enabled.

Default:
- disabled
- Bx = 0
- By = 0
- Bz = 0

When enabled, the uniform B field is added to point output and all-field output.
It is also included in the soft-magnet self-consistent magnetization solve.

The uniform field has no direct force term because its gradient is zero. It can
still affect force indirectly by changing the soft magnetization.


Location And Current Sequences
------------------------------

Location mode:
- fixed location
- sequence location

For sequence location, positions can be edited directly in the position table.
The table supports copy and paste from spreadsheet software such as Excel.
Paste full x/y/z tables or a single selected column.

Current mode:
- fixed
- linear

When location mode is sequence location, current mode is locked to fixed to
avoid nested location-current sweeps.


All Field Output
----------------

Select output object = all field to calculate a field cloud around the enabled
magnetic components.

The program automatically creates a cube around the components, samples field
points, skips points inside magnetic components, and writes a CSV file.

Preview display supports:
- 3D point cloud
- 2D slice
- x, y, or z plane selection
- optional requested slice value, automatically snapped to the nearest computed
  layer

The preview image can be saved with the Save Preview button.


Soft Magnet Kernel Data
-----------------------

The soft-magnet solver uses the demagnetization kernel file:

   _internal\data\kernel_array_40.npz

The default kernel path is resolved automatically from the extracted application
folder. Users normally do not need to browse for this file manually.

Do not delete the _internal\data folder.


STL Geometry Notes
------------------

The program uses SI units internally:

- length: meter
- current: ampere
- magnetization: A/m
- B field: tesla
- H field: A/m
- force: newton

If your STL file was created in millimeters, set:

   STL scale = 1e-3

If your STL file was created in meters, set:

   STL scale = 1

Incorrect STL scale will cause wrong geometry size and wrong simulation results.

For STL soft magnets with separated bodies in one STL file, preview and
voxelization can take longer than regular primitive shapes.


Geometry Check
--------------

Always click Preview Geometry before Run Calculation.

The preview step checks for component placement problems, including:

- physical overlap or contact between hard magnets, coils, and the soft magnet
- nearly coincident source/sample points that may cause singular field
  evaluation

Soft magnets may be placed very close to a coil or hard magnet if the real
geometric outlines do not overlap. The check is based on real component
geometry instead of voxel-expanded boundaries to reduce false alarms.


Output Files
------------

Default output files are written under the application outputs folder:

- sequence_location_results.csv
- current_sequence_results.csv
- all_field_results.csv

The output path can be changed in the UI when the corresponding output mode is
active.


Troubleshooting
---------------

If the program cannot run:
- keep the full extracted folder together
- make sure _internal was not deleted
- avoid running the EXE directly from inside a compressed ZIP file

If soft-magnet calculation reports a missing kernel:
- check that _internal\data\kernel_array_40.npz exists
- keep the original folder structure from the ZIP package

If STL geometry looks wrong:
- check STL scale
- click Preview Geometry before running calculation
- confirm the imported part appears at the expected size and location

