For using the SLM, please first follow the "SLM Computer with Slmsuite Setting Guide" file in the SLM-200 folder of google drive.  

After you finish all those instructions, your computer is equipped with: (1) software provided by Santec(SLM Pattern Generator, SLM-200 GUI);(2) Software to connect the camera(Vimba Viewer);
(3) a python package called "slmsuite". Please first try to use this softwares and confirm the devices are connected to your computer correctly.  

If they're correctly connected to your computer, please make a few changes to your "slmsuite" package. These little changes make sure the further programs can work properly.  

Correction should be made to slmsuite>holography>algorithms.py
1. In ijcam_to_kxyslm function: 
      change the shape of b by adding a command "<span style="color:red">b = b.reshape(-1)</span>" before calculating the target.
2. Find this para in the algorithms.py:
   ```python
   if self.null_knm is None:
            self.target.fill(0)
        else:
            self.target.fill(np.nan)

            if self.null_region_knm is not None:
                self.target[self.null_region_knm] = 0

            if self.null_knm is not None:
                all_spots = np.hstack((self.null_knm, self.spot_knm))
                w = int(2*self.null_radius_knm + 1)

                for ii in range(all_spots.shape[1]):
                    toolbox.imprint(
                        self.target,
                        (np.around(all_spots[0, ii]), w, np.around(all_spots[1, ii]), w),
                        0,
                        centered=True,
                        circular=True
                    )
     ```
        correct it as:
     ```python
     if self.null_knm is None and self.null_region_knm is None:
         self.target.fill(0)
     else:
         self.target.fill(np.nan)


         if self.null_region_knm is not None:
             self.target[self.null_region_knm] = 0

         if self.null_knm is not None:
             all_spots = np.hstack((self.null_knm, self.spot_knm))
             w = int(2*self.null_radius_knm + 1)

             for ii in range(all_spots.shape[1]):
                 toolbox.imprint(
                     self.target,
                     (np.around(all_spots[0, ii]), w, np.around(all_spots[1, ii]), w),
                     0,
                     centered=True,
                     circular=True
                 )
     ```

     
