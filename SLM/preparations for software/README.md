For using the SLM, please first follow the "SLM Computer with Slmsuite Setting Guide" file in the SLM-200 folder of google drive.  

After you finish all those instructions, your computer is equipped with: (1) software provided by Santec(SLM Pattern Generator, SLM-200 GUI);(2) Software to connect the camera(Vimba Viewer);
(3) a python package called "slmsuite". Please first try to use this softwares and confirm the devices are connected to your computer correctly.  

If they're correctly connected to your computer, please make a few changes to your "slmsuite" package. These little changes make sure the further programs can work properly.  

Correction should be made to slmsuite>holography>algorithms.py
1. In ijcam_to_kxyslm function: 
      change the shape of b– <span style="color:red">b = b.reshape(-1)</span> before calculating the target.
2. The null_region parameter shouldn’t be bool type.
