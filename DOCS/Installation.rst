Installation
=============================================

1. Clone or download the DISpy repository from `Github <https://github.com/shr1998/DISpy?tab=readme-ov-file>`_;

2. Install the following dependency packages:numpy,numba,scipy,obspy,cupy,pyqt5,nptdms,h5py;

3. Compile main.py using pyinstall with coda:

```pyinstaller main.py   --name DASpy  --hidden-import  'cupy_backends.cuda.api._runtime_enum' --hidden-import  'cupy_backends.cuda.stream' --hidden-import 'cupy._core._carray' --hidden-import 'fastrlock' --hidden-import 'fastrlock.rlock' --hidden-import 'cupy_backends.cuda.api._driver_enum' --hidden-import  'cupy._core._ufuncs' --hidden-import 'cupy._core._cub_reduction' --hidden-import 'cupy._core._routines_sorting' --hidden-import 'cupy._core.flags'  --hidden-import 'cupy_backends.cuda._softlink' --hidden-import 'cupy.cuda.common' --hidden-import 'cupy._core.new_fusion' --hidden-import 'cupy._core._fusion_trace' --hidden-import 'cupy._core._fusion_variable' --hidden-import  'cupy._core._fusion_op' --hidden-import 'cupy._core._fusion_optimization' --hidden-import 'cupy._core._fusion_kernel' --additional-hooks-dir=.```
4. or just run：
  ```python3 main.py
