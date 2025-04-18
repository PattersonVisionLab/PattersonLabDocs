# AO Calibration Protocol
<font size="2"> Last Updated: November 11, 2024


### Step 1: Align the model eye
- Install the model eye and turn the system on.
- On the SLO PC, open the FrameGrabber software (folder: /Desktop/software/real-time), load `mlprimate506 496 lines` instead of the Imaging Software, and turn on both scanners.
- On the AO PC, open TsunamiWave (_do not load previous AO settings!_).
- Also on the AO PC, open the Magnifier software (programs -> accessories -> accessibility). Set the magnification in a small window to ensure the defocus, tip, tilt and max pixel value numbers at the bottom of the AO software window are easily visible when standing by the model eye.
- Make sure the pinhole has been removed from the WFS source (may still be there after the source alignment)
- Start the camera in the AO software ("Cameras" tab, "Camera Live" button) and adjust the position of the model eye until the pattern is uniform.
  - Tip: Checking the crosshair option on the AO software will help here and in subsequent steps
- Decrease the exposure time to 20 ms on the AO software and check to see if the pattern is static and stable. If not, the pupil plane of the model eye may be off.
- Focus the model eye pupil plane by adjusting the Z position using the model eye controller (may need to hold one of the top buttons while adjusting to switch from XY to Z position control). You will see the SHWS spots flicker when not conjugate with the galvo.
  - Tip: To fine-tune, decrease exposure time further to 15 msec and increase the galvo scan rate on the FrameGrabber software.
- Set the exposure time back to 70 ms and adjust the power of the WFS source until the max pixel value is less than 255 (~220-230)
- Focus the model eye retina plane with the z-micrometer at the back of the model eye. The point of maximum pixel value should be roughly the same point as where spherical aberration is minimized.
- If necessary, adjust the SHWS to center the image and then lock it back down.
- Go to the WC tab and press Reset Actuators (they should all be green)
- Go to the SHWS tab, estimate lattice, validate box redux factor, find reference spots
  - Tip: Usually reference spots are findable with threshold = 0.4, but you can decrease it to 0.3 or lower if necessary to find all the spots.
- Save WS settings as `step_1_paper`. The date/time will be added automatically.

### Step 2: Align the reference beam
- Turn down power to zero temporarily
- Place the two flat mirrors (mirror with green goes on magnetic mount with green tape, same for orange)
- Plug in the optical fiber (green tip) that contains the reference beam for the WFS source. Make sure to align the lock gently and tighten screw.
- Go to the SHWS tab and click WS Live. Again the magnifier will be helpful here.
- Optimize defocus to 0.0 D by moving the lens in front of the fiber (to make small movements, tap it lightly with a hex key). Optimize tip/tilt to ~0.01 or lower by adjusting the flat mirror closest to you.
  - Tip: There is some crosstalk between the lens/defocus and mirror/tiptilt so get both close and then fine tune.
  - Tip: Increasing exposure time and/or WFS source power slightly can help with this step.
- Turn off WS Live, find reference spots again (ok to miss a few here)
- Save WS settings as `step_2_flatmirror`

### Step 3: Determine the system pupil
- Turn the WFS source power down to zero temporarily to avoid damaging the fiber
- Take out the two flat mirrors and remove the fiber
- Turn both scanners down to zero in the FrameGrabber software.
- Use the Toptica 561 nm laser to coarsely align the system output to the leftmost cage on the model eye, using the aperture as a guide. If the pellicle is in, check and make sure it's not clipping the beam (use card placed after the pellicle or just look at the edges of the pellicle). When done, turn the Toptica off completely and/or block beam, ensuring no light enters the main system. All other sources should be off and blocked as well.
- Connect the fiber used in step 2 (green) to the fiber extender, then attach the output fiber (white) to the leftmost cage on the model eye so that the reference beam is directed to the DM.
- Go to the SHWS tab, click "WS Live" and turn the power up to get WFS spots.
- Check centering of the beam by moving the lens in front of the cage plate holding the fiber or opening/closing the aperture. Looking for center of beam to stay in about the same place, especially when the aperture is small. When aperture is large, looking for no clipping.
  - _Explain clipping_
  - Tip: To distinguish beam clipping due to cage from beam clipping due to pellicle or other part of system, put a card at the system's output and look at it with an IR viewer. If the beam isn't clipped, it's the cage (see rotation info below).
- Adjust the lens in front of the fiber to reach 0.0 D sphere, then lock it it
- Adjust the XY controls on the cage plate holding the fiber until tip/tilt is ~0.03 or better.
- Click "Determine System Pupil" in the AO software
- Save as `step_3_system_pupil`.

### Step 4: Calculate the influence matrix
- Go to the AO tab and make sure the settings are 30 calibration stpes, condition number threshold = 95, intensity threshold = 0.1, 10-15 settling time.
- Place sign on door indicating NOT to knock, turn all room lights off and turn off the 2nd monitors facing the system.
- Click "Estimate WC Response Matrix". The program will iterate over every actuator and the full process will take about 10 minutes. During this time, you will want to remain relatively quiet and still (no walking around).
  - Tip: If you get an error, you may just need to turn up the power on the WFS source.
- Save as `step_4_almost_done`

### Step 5: Calculate the control matrix
- Click "Calculate WC Control Matrix for Current Subject". Look at the condition number and especially the number of modes. There is 1 mode per actuator (97) but the final modes are mainly noise and won't help the control loop. Adjust the condition # threshold until you get to 89 - 92.
- Remove/recap the reference beam fiber
- Turn the power up to the model eye value for imaging and click "AO Live". Make sure nothing crazy happens.
  - Tip: something is wrong if: large sections of red/blue appear, there are oscillations, things look good for a moment but inevitably degrade.
- Save AO settings as `ready_2_go`. This is the file you will load when using the system until the next alignment. At this point, you should be good to move the files from the last AO calibration into the "Old files" subfolder so someone doesn't accidentally click on the wrong thing at the beginning of an imaging session.

### Set the best model eye focus to 0 D
- Open TsunamiWave and load the new AO settings.
- Center the model eye (center cage) using "WS Live", then switch to "AO Live".
- Optimize the reflectance source and PMT positions. You may need to adjust the defocus on the AO software as well.
  - Tip: Optimize mean pixel value for the PMT positions and image focus/sharpness for the source position.
- If the defocus necessary to get the model eye paper in focus is way off, you may need to "walk" it to 0 D. Start at whatever defocus is needed to get the model eye in focus, step it towards 0, optimize the source and PMT positions, then repeat...

</font>

__Next__: [Field of View Calibration](FOV_Calibration.md)