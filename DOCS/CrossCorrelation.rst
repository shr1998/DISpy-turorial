

CrossCorrelation
==================================================

1. Parameters Introduction
-----------------
When using the cross-correlation module in practice, users must first configure the calculation parameters correctly. Parameters can be initialized either by loading an external parameter file or by directly setting and modifying them in UI. After reading the parameters, the software will automatically complete the data search, reading, and cross-correlation calculation process according to the user's settings. The meanings of the main parameters and recommended settings are explained below.

2. Parameter Descriptions
-----------------

**datatype** (str)
  This parameter specifies the format type of input data. Since DAS systems exhibit significant differences in hardware architecture, data acquisition methods, and data encapsulation formats, DAS devices from different manufacturers often employ different data storage formats. To accommodate this diversity, the cross-correlation module provides a datatype option to clarify the basic format type of the current data. It is recommended that users communicate with the device manufacturer or consult the equipment documentation before using the software to confirm the actual data storage format and select the correct datatype setting accordingly, ensuring that data can be accurately parsed and read.

**data_path** (str)
  This parameter specifies the root directory or main directory where DAS data is stored. In practical applications, DAS data is typically segmented and stored according to a certain time period, such as by day, hour, or acquisition batch, distributed in different subdirectories with a multi-level nested structure. For such multi-layer directory organizations, data_path should be set to the outermost directory containing all target data with the longest level hierarchy to clarify the data range covered by the cross-correlation calculation. The software will automatically traverse all data files meeting the criteria from this path, ensuring the completeness of data used in the cross-correlation calculation in both time and space domains.

**files_jug** (str)
  This parameter indicates the distribution of data files in the directory structure, determining whether all files to be processed are stored in the same directory level. When the directory specified by data_path contains only data files at a single level without subdirectories requiring further traversal, set this parameter to ``single``; when data_path contains multiple subdirectories with valid data files scattered among them (e.g., organized by date, hour, or acquisition batch), set it to ``mul``. Through this parameter setting, the software can automatically select an appropriate directory traversal strategy, ensuring correct and complete reading of all target data files under different data organization methods.

**shape** (str)
  This parameter describes the naming rules of individual data files, primarily used to identify and filter available data files for calculation in the specified directory. In practical applications, data file names typically contain time information, channel numbers, or other identifiers, but often contain a common portion that remains consistent across all files. Users only need to extract this consistent part and describe it using wildcard format to complete data file matching and identification. For example, for data files named in the format ``20260101_xx.h5``, the shape parameter can be set to ``20*`` to match all valid data files starting with the year. This design effectively avoids the tedious operation of specifying complete file names one by one, while also enhancing the software's adaptability to different naming conventions.

**cc_len** (int)
  This parameter sets the time window length used in the cross-correlation calculation and is one of the key control parameters in the cross-correlation module. This parameter directly determines the maximum length of a single cross-correlation result on the time axis, affecting the maximum time delay range and effective signal information that can be analyzed in the final cross-correlation function. In practical applications, the setting of cc_len requires comprehensive consideration of time resolution, signal-to-noise ratio, and available data length. On one hand, a longer time window can retain more complete cross-correlation information, facilitating the acquisition of effective signal features within a larger time delay range. On the other hand, when individual data segment length is limited or total data volume is insufficient, an excessively large cc_len will significantly reduce the number of time windows available for stacking, thereby reducing the stability and signal-to-noise ratio of the cross-correlation result. It is also important to note that cc_len should not be set too small. If the time window is too short, it may fail to cover the main energy range of the target signal, resulting in insufficient effective signal in the cross-correlation function, which may impact the accuracy of subsequent frequency dispersion analysis and inversion calculations. It is recommended that users make a reasonable choice of the cc_len parameter based on specific experimental objectives, research scale, and data sampling characteristics, to achieve a balance between the number of stacking windows and effective signal length, thereby obtaining stable and physically meaningful cross-correlation results.

**c_range** (int)
  This parameter sets the length of the sub-array used in the cross-correlation calculation and is one of the important parameters controlling the spatial scale and computational effectiveness of cross-correlation. The software's cross-correlation calculation is based on the sub-array concept, which involves selecting a certain length of continuous channels from the overall channel array to form a sub-array, and performing cross-correlation calculation and result stacking within the sub-array to improve the stability and robustness of the computational results. The setting of sub-array length directly affects the number of stacking results and spatial resolution of the cross-correlation. On one hand, a shorter sub-array can form more sub-array positions within the overall array, significantly increasing the number of cross-correlation result stackings, which helps improve the statistical stability and signal-to-noise ratio of the results. On the other hand, an excessively short sub-array reduces the effective aperture of the array, possibly resulting in degraded spatial resolution and limiting the ability to characterize distant or low-frequency signal features. Conversely, a longer sub-array, while improving spatial resolution, reduces the number of sub-arrays that can be formed and decreases stacking times, potentially making cross-correlation results more sensitive to noise. Therefore, when setting the c_range parameter, users need to comprehensively consider the total number of data channels, target analysis scale, cc_len, and other cross-correlation parameters, selecting a sub-array length that balances resolution and stability. It should also be noted that c_range cannot exceed the total number of data channels; otherwise, effective sub-arrays cannot be constructed, causing the calculation to fail. Proper setting of this parameter is crucial for obtaining high-quality, interpretable cross-correlation results.

**dr** (float)
  This parameter describes the spatial sampling rate of the array, i.e., the physical spacing between adjacent channels, and is one of the fundamental parameters for characterizing the array's spatial structure. The value of dr directly determines the coordinate scale of the cross-correlation result in the spatial domain and has important implications for the spatial resolution in subsequent frequency dispersion analysis, wave velocity estimation, and inversion calculations. Generally, dr is determined by the actual channel spacing or equivalent channel distance of the DAS system, and users should set it accurately based on instrument configuration or data documentation. If this parameter is set incorrectly, it will cause deviation in spatial coordinate calibration, affecting the physical interpretation of wave field propagation characteristics.

**step** (int)
  This parameter sets the movement interval of the virtual source on the array and is an important parameter controlling the density of sub-array division and calculation scale. When performing cross-correlation calculation on the entire array, the software selects multiple positions along the array direction as virtual sources and constructs corresponding sub-arrays to obtain cross-correlation function sets at different virtual source positions. The step parameter determines the spacing between adjacent virtual sources, i.e., the sliding step length of sub-arrays on the array. A smaller step value means virtual source positions are more densely distributed, allowing acquisition of higher spatial density cross-correlation results, facilitating detailed characterization of spatial variations in subsurface media. However, this also significantly increases the number of sub-arrays and overall computation load, placing higher demands on computing device resources. Conversely, a larger step value effectively reduces the computational scale and improves execution efficiency, but may sacrifice certain spatial continuity and resolution. Therefore, when setting this parameter, users should comprehensively consider computing device performance, data scale, and research objectives, making reasonable trade-offs between computational efficiency and result refinement.

**outputdir** (str)
  This parameter specifies the output path for result files generated during cross-correlation calculation and related processing. During software execution, in addition to the final cross-correlation results, certain intermediate files or auxiliary data may be generated for result caching, debugging analysis, or direct invocation by subsequent modules. By setting a unified outputdir, users can centrally manage output data, facilitating result storage, backup, and subsequent analysis. It is recommended that users verify sufficient storage space in this path and appropriate read-write permissions before calculation to avoid interruption due to path or permission issues.

**example_path** (str)
  This parameter specifies a sample data file used to describe the internal storage structure and basic properties of the DAS data being used. The software needs to understand the organization of variables, data dimensions, and key metadata (such as number of channels, sampling rate) within the data file during the data reading stage. Therefore, a sample file conforming to the actual data specification is required as a reference. Users can select any one of the valid DAS data files as the sample file to set. The software will automatically parse the data structure based on this sample file and apply the parsed results to batch reading of similar data files, improving the generality and stability of data reading.

**time_step** (int)
  This parameter sets the step length of the time window sliding along the time axis during cross-correlation calculation, i.e., the time interval between adjacent calculation time windows. This parameter directly affects the number of available time windows in cross-correlation calculation, thus having important implications for result stacking times and stability. It should be noted that when time_step is smaller than cc_len, there will be a certain degree of overlap between adjacent time windows. This time window overlap is allowed in cross-correlation calculation and typically does not produce obvious adverse effects on final results. Conversely, when total data duration is limited or valid data segments are sparse, by appropriately reducing time_step, the number of time windows participating in cross-correlation calculation can be significantly increased, thus increasing stacking times and enhancing the stability and signal-to-noise ratio of cross-correlation results. Of course, the setting of time_step also needs to consider computational efficiency comprehensively. While smaller steps facilitate increased stacking times, they also significantly increase computation and execution time. It is therefore recommended that users reasonably configure time_step based on total data length, cc_len parameter, and available computing resources, ensuring result reliability while considering overall computational efficiency.

**smooth_N** (int)
  This parameter controls the degree of smoothing when normalizing data before cross-correlation calculation. Specifically, it smooths spectral amplitude by setting the size of a sliding averaging window, reducing short-term sharp fluctuations and noise components in the spectrum, and improving the stability and reliability of the normalization process. Reasonable selection of smooth_N value can effectively suppress spectral peaks and irregular variations, avoiding over-amplification of noise or anomalous signals during normalization and ensuring the quality and consistency of cross-correlation results. A window that is too small may result in insufficient smoothing effect with the spectrum retaining more high-frequency fluctuations; a window that is too large may cause over-smoothing, reducing the resolution of signal details. It is recommended to adjust smooth_N based on specific data characteristics and signal quality, combined with experimental experience, to achieve optimal normalization effect.

**freqmin** (float)
  This parameter sets the lower frequency limit of the frequency band range of interest to the user. Together with freqmax, it defines the frequency band range participating in cross-correlation calculation. Properly setting the lower frequency limit helps suppress the impact of low-frequency noise on calculation results.

**freqmax** (float)
  This parameter sets the upper frequency limit of the frequency band range of interest to the user. By working in conjunction with freqmin, it effectively controls the analysis frequency band, avoiding high-frequency noise or invalid frequency components from entering the cross-correlation calculation.

**b_ch** (int)
  This parameter sets the starting point of the channel range participating in cross-correlation calculation, i.e., the starting channel number of the selected channel interval. Users can flexibly adjust this parameter based on actual array length and research requirements.

**e_ch** (int)
  This parameter sets the ending point of the channel range participating in cross-correlation calculation, i.e., the ending channel number of the selected channel interval. The selected channel range must be within the actual number of channels in the data.

**freq_norm** (str)
  This parameter specifies the method used for frequency domain normalization of data before cross-correlation calculation. Different normalization strategies have different effects on spectral amplitude distribution and the stability of cross-correlation results. Users can choose based on data characteristics and experimental objectives.

**substack** (Boolean)
  This parameter sets whether to employ the common-offset stacking method during cross-correlation calculation. This method can effectively improve the signal-to-noise ratio of cross-correlation signals, but in certain situations may also amplify common-mode noise components. Users should make reasonable settings for this parameter based on data quality and research objectives.

**meta_flag** (Boolean)
  This parameter indicates whether the DAS data includes a corresponding channel location information file. When DAS data is equipped with an independent channel location file, set this parameter to ``True``; if the data does not contain channel location information, set it to ``False``.

**meta_file** (str)
  This parameter specifies the path to the channel location file corresponding to DAS data. This file typically contains spatial location information for each channel, used for subsequent spatial coordinate calibration and analysis. When meta_flag is set to ``False``, this parameter does not need to be filled.

**samplerate_in_use** (int)
  This parameter sets the time domain sampling frequency actually used in the calculation. This parameter allows users to adjust the sampling rate used in the calculation without changing the original data sampling rate, balancing computational accuracy and efficiency. The value of samplerate_in_use cannot be less than one-tenth of the original sampling rate (samplerate), otherwise time resolution may be insufficient, affecting the reliability of cross-correlation results.

**whole_array** (Boolean)
  This parameter sets whether to calculate cross-correlation functions based on the entire array range. When this parameter is set to ``True``, cross-correlation calculation will cover all available channels; when set to ``False``, calculation is performed only on the sub-array specified by the user through channel range parameters.

**time_range** (int)
  This parameter sets the length range of the final saved cross-correlation function on the time axis. This parameter is used to truncate the interested time window in the cross-correlation result, and its value must be less than half of cc_len to ensure symmetry and validity of the cross-correlation function on the time axis.

**sta_file** (str)
  This parameter specifies the path to the seismic station (seismometer) data file being used. When the STA parameter is set to ``True``, meaning seismic station data is enabled, this parameter needs to be filled. If STA is set to ``False``, this parameter does not need to be filled.

**STA** (Boolean)
  This parameter sets whether to incorporate seismic station data in the cross-correlation calculation. When set to ``True``, the software will simultaneously use DAS data and seismic station data for cross-correlation calculation; when set to ``False``, only DAS data is used to complete the cross-correlation calculation.

3. Workflow
-----------------

|cc|

This section describes the operational workflow of DISpy from program launch 
to cross-correlation result export. The workflow integrates parameter loading, 
data processing, runtime monitoring, visualization, and result storage into 
a unified and structured procedure.

3.1 Workflow Overview
~~~~~~~~~~~~~~~~~~~~~~~~~

The overall processing sequence is summarized as follows:

+----------------------------+----------------------------------------------+
| Stage                      | Core Function                                |
+============================+==============================================+
| Program Initialization     | Enter main interface                         |
+----------------------------+----------------------------------------------+
| Parameter Loading          | Import and verify configuration file         |
+----------------------------+----------------------------------------------+
| Cross-Correlation Start    | Launch computation workflow                  |
+----------------------------+----------------------------------------------+
| Runtime Monitoring         | Track execution status and logs              |
+----------------------------+----------------------------------------------+
| Real-Time Visualization    | Inspect cross-correlation results            |
+----------------------------+----------------------------------------------+
| Noise Suppression          | Apply time-domain cut for common-mode noise  |
+----------------------------+----------------------------------------------+
| Safe Termination           | Gracefully stop computation if necessary     |
+----------------------------+----------------------------------------------+
| Result Export              | Save cross-correlation outputs               |
+----------------------------+----------------------------------------------+


3.2 Detailed Operational Procedure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Program Initialization
^^^^^^^^^^^^^^^^^^^^^^^^

After launching DISpy, the user enters the main interface, 
which serves as the unified control center for parameter configuration, 
computation management, and execution monitoring.

Parameter Loading
^^^^^^^^^^^^^^^^^^^^^^^^

Click the ``paraload`` button (Fig. (a)) to load the predefined parameter file.  
The imported parameters are displayed in the parameter panel.  
Users may review and adjust the configuration according to specific experimental 
requirements before starting computation.

Cross-Correlation Computation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After verifying parameter settings, click ``RunCC`` (Fig. (b)) to initiate 
the cross-correlation workflow.

During execution, the status bar sequentially reports the current processing stage:

- HDF5 (h5) data loading  
- Data preprocessing  
- Cross-correlation calculation  

Meanwhile, the report window (lower-right panel) provides real-time log output 
and progress information.

Data Attribute Verification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once data loading is completed, the data property panel  displays 
key attributes, including:

- Start and end time  
- Actual sampling rate  
- Number of channels  

This panel allows users to verify consistency between data and parameter 
settings before full-scale computation proceeds.

Real-Time Visualization
^^^^^^^^^^^^^^^^^^^^^^^^^^

DISpy supports multi-threaded computation. After each time-window segment 
is processed, users can visualize the subarray noise cross-correlation 
function corresponding to a selected virtual source.

Two display modes are provided:

- ``line`` mode  
- ``imshow`` mode  

Users may switch between modes via the upper-right control buttons.  
Subarray indices can be browsed using the ``last`` and ``next`` controls.

Common-Mode Noise Suppression
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In practical DAS processing, cross-correlation results may contain 
common-mode noise caused by instrumental self-noise or system characteristics.

DISpy provides a basic time-domain removal function:

- When the ``cut`` button is green, the function is enabled.
- Users may interactively select the time range to be removed.
- Click ``CUT`` to confirm and apply the removal operation.

This process helps suppress common-mode artifacts in visualization results.

Computation Termination
^^^^^^^^^^^^^^^^^^^^^^^^^^

If computation needs to be stopped, click ``StopCC``.  
The program terminates safely after finishing the current time window, 
preventing unexpected interruption or data corruption.

Result Export
^^^^^^^^^^^^^^^^

After obtaining satisfactory cross-correlation results, click 
``Save cc files`` to export outputs in the standardized DISpy format.  
The saved results can be directly used for subsequent dispersion 
analysis, inversion computation, or external program integration.
