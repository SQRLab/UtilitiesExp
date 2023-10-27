For using the SLM, please first follow the "SLM Computer with Slmsuite Setting Guide" file in the SLM-200 folder of google drive.  

After you finish all those instructions, your computer is equipped with: (1) software provided by Santec(SLM Pattern Generator, SLM-200 GUI);(2) Software to connect the camera(Vimba Viewer);
(3) a python package called "slmsuite". Please first try to use this softwares and confirm the devices are connected to your computer correctly.  

If they're correctly connected to your computer, please make a few changes to your "slmsuite" package. These little changes make sure the further programs can work properly.  

Correction should be made to slmsuite>holography>algorithms.py
1. In algorithms.py, find the definition of function ijcam_to_knmslm:
   ```python
   def ijcam_to_knmslm(self, img, out=None, blur_ij=None, order=3):
      ''' codes above'''
      if blur_ij > 0:
            img = sp_gaussian_filter(img, (blur_ij, blur_ij), output=img, truncate=2)

        cp_img = cp.array(img, dtype=self.dtype)
        cp.abs(cp_img, out=cp_img)
        
        # Perform affine.
        target = cp_affine_transform(
            input=cp_img,
            matrix=M,
            offset=b,
            output_shape=self.shape,
            order=order,
            output=out,
            mode="constant",
            cval=0,
        )
   ```
   add a sentence of `b = b.reshape(-1)` in the middle to get:
   ``` python
   if blur_ij > 0:
            img = sp_gaussian_filter(img, (blur_ij, blur_ij), output=img, truncate=2)

        cp_img = cp.array(img, dtype=self.dtype)
        cp.abs(cp_img, out=cp_img)
        
        b = b.reshape(-1)

        # Perform affine.
        target = cp_affine_transform(
            input=cp_img,
            matrix=M,
            offset=b,
            output_shape=self.shape,
            order=order,
            output=out,
            mode="constant",
            cval=0,
        )
   ```

3. Find this para in the algorithms.py:
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

     
