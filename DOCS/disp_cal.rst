.. |disp| image:: ../FIGS/disp.png
    :height: 400

Disp_Cal
==================================================

1. Module Introduction
-----------------

The dispersion module is designed to extract surface-wave dispersion 
characteristics from previously computed noise cross-correlation results. 
It maintains structural consistency with the CrossCorrelation module and 
adopts a modular three-panel layout, ensuring operational continuity 
throughout the processing workflow.

The interface mainly consists of three functional components:

- Dispersion image visualization and manual picking window  
- Dispersion parameter configuration window  
- Automatic dispersion picking window  

Dispersion analysis must be performed after stable and reliable 
cross-correlation results have been obtained. The module transforms 
time-domain cross-correlation signals into frequency–velocity or 
frequency–wavenumber domain representations, enabling quantitative 
characterization of surface-wave propagation properties.


2. Parameter Descriptions
-----------------

**Range** (array)  
  Specifies the spatial range or sub-array index range involved in 
  dispersion energy calculation. For long DAS arrays, restricting 
  the calculation range can significantly reduce computational load 
  during parameter testing.

**fmin** (float)  
  Lower bound of the frequency band used in dispersion calculation.  
  Helps suppress low-frequency noise and long-period interference.

**fmax** (float)  
  Upper bound of the frequency band used in dispersion calculation.  
  A wider frequency range increases the number of sampling points 
  and computational cost.

**vmin** (int)  
  Lower bound of the phase velocity search range.  
  Should be estimated based on regional geological conditions.

**vmax** (int)  
  Upper bound of the phase velocity search range.  
  Controls the maximum velocity displayed in the dispersion spectrum.

**dv** (float)  
  Sampling resolution along the phase velocity axis.  
  Smaller values improve resolution but increase runtime and memory usage.

**df** (float)  
  Sampling resolution along the frequency axis.  
  Higher resolution enhances image clarity but reduces computational efficiency.

**dt** (float)  
  Time sampling interval of the input cross-correlation data.  
  Must remain consistent with the actual sampling interval.

**cc_len** (int)  
  Total time length of the input cross-correlation record.

**time_range** (int)  
  Effective time window used for dispersion analysis.  
  Should fully cover the surface-wave arrival window.

**dr** (float)  
  Spatial sampling interval (channel spacing).  
  Determines spatial coordinate calibration accuracy.

**c_range** (int)  
  Sub-array aperture length used in dispersion calculation.  
  Influences energy focusing capability and resolution.

**Part** (str)  
  Specifies which branch of the cross-correlation function is used 
  (causal or acausal). Selection should consider noise source distribution.

**save_path** (str)  
  Directory used to store dispersion energy spectra and picking results.


Automatic Picking Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~

**picker**  
  A collection of control parameters for automatic picking strategy.

**device** (str)  
  Specifies execution device for automatic picking.  

  - ``cuda`` — GPU acceleration  
  - ``cpu`` — CPU execution  

**weight_path** (str)  
  Path to the trained neural network weight file used for 
  automatic dispersion curve extraction.


3. Workflow
-----------------

This section describes the operational procedure of the dispersion module, 
from cross-correlation data loading to manual or automatic picking.

3.1 Workflow Overview
~~~~~~~~~~~~~~~~~~~~~~~~~

+----------------------------+----------------------------------------------+
| Stage                      | Core Function                                |
+============================+==============================================+
| Data Preparation           | Load cross-correlation results               |
+----------------------------+----------------------------------------------+
| Parameter Verification     | Confirm calculation settings                 |
+----------------------------+----------------------------------------------+
| Method Selection           | Choose Phase Shift or F–K method             |
+----------------------------+----------------------------------------------+
| Dispersion Computation     | Generate dispersion energy spectrum          |
+----------------------------+----------------------------------------------+
| Visualization              | Display and adjust dispersion image          |
+----------------------------+----------------------------------------------+
| Picking                    | Manual or automatic curve extraction         |
+----------------------------+----------------------------------------------+
| Result Saving              | Export dispersion data                       |
+----------------------------+----------------------------------------------+


3.2 Detailed Operational Procedure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Data Preparation
^^^^^^^^^^^^^^^^^^^^^^^^

Before dispersion calculation, cross-correlation data must be available.

If cross-correlation was performed in the same DISpy session, 
results are automatically linked. Otherwise, click ``cc_load`` 
to manually import cross-correlation files.

Parameter Verification
^^^^^^^^^^^^^^^^^^^^^^^^

After data loading, verify all parameters in the configuration panel, 
including spatial sampling, sub-array length, and processing range.

Method Selection
^^^^^^^^^^^^^^^^^^^^^^^^

DISpy currently provides two dispersion calculation methods:

- Phase Shift method  
- F–K method  
- CC-FJpy 

Users should select the appropriate method based on data quality 
and research objectives.

Dispersion Computation
^^^^^^^^^^^^^^^^^^^^^^^^

Click ``Run DISP`` to start computation.  
The progress bar beneath the visualization window displays runtime status.

After completion, click ``Plot DISP`` to visualize the dispersion energy 
spectrum at a specified channel or position.  
Use ``last`` / ``next`` to browse adjacent locations.

Visualization Adjustment
^^^^^^^^^^^^^^^^^^^^^^^^^^

Users may adjust display parameters such as colorbar range 
or colormap to enhance clarity before saving results.

Results are saved in raw dispersion data format rather than 
direct image format.

Manual Picking
^^^^^^^^^^^^^^^^

Enable picking mode via the ``pick`` button (green indicates active).  
Users may manually select dispersion points directly on the image.

Automatic Picking
^^^^^^^^^^^^^^^^^^

Open the Pick Window.

1. Click ``Model Load`` to load the trained model.
2. Click ``PICK`` to initiate automatic curve extraction.
3. Use ``Plot DISP`` to visually inspect and validate results.
