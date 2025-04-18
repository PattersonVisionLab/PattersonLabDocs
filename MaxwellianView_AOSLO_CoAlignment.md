## Maxwellian View Coalignment with AOSLO

<font size="2"> __Last Updated:__ November 27, 2024


### Positioning the stimulus within the FOV
This relies on a series of near/far-field alignments. We typically image on one side of the FOV and provide stimuli on the other side, so the goal is to place the stimulus to the left or right. Making the correct determination on stimulus position is easiest if you have a specific imaging region or stimulus/imaging layout in mind.

1. Center the stimulus on L4.
   - Hold a card or lens paper after the 2nd flat mirror and adjust the first flat to center the beam.
   - Hold the card/paper near the final lens and adjust the 2nd flat mirror to center it on the lens
   - Iterate.
2. Position the beam within the FOV.  _Note: the stimulus will no longer pass through the center of L4. This hasn't been an issue with the small displacements we typically use but could be fixed in the future with the addition of another flat to provide an additional degree of freedom._
    - Turn on the 488 nm laser and set the FOV to whatever size you plan to use for physiology. Adjust the laser intensity to ensure the Maxwellian View stimulus is visible. I usually use just the red LED for this step.
    - Hold a card at the final flat and compare the size of the stimulus to the 488 nm FOV. If the stimulus is too large or too small, make a coarse adjustment to the iris between F1 and F2. There will be an opportunity to make finer adjustments to stimulus size later.
    - Hold a card against the final flat of the AOSLO and adjust F2 to position the beam to the left/right of the FOV formed by the 488 nm laser.
    - Hold the card at the aperture of the model eye and adjust the pellicle to position the beam to the same position within the FOV. Also check when the card is further back (e.g., turn off lights and look at projection on wall).
    - Iterate.
3. Confirm position by imaging the stimulus in the model eye.
    - Remove any fluorescence filters from in front of the visible PMT. This step is also easier if you remove any NDFs in the Maxwellian View.
    - Turn all three LEDs up (use CW setting) and turn the 488 nm laser off (extent is determined by the FOV you're imaging with, so we know it would fill the image). The room lights should also be turned off.
    - Turn up the visible PMT gain (may need to be quite high; 0.8-0.9). You should at least see increased noise in the region where the stimulus is present
    - Take a 10-20 second video, drag into ImageJ and take the Standard Deviation z-stack of the video which will reveal the stimulus even if it wasn't very visible in an individual frame.
     - Make fine adjustments to stimulus size, if needed, using the iris between F1 and F2.
4. Iterate the two previous steps if the model eye position wasn't ideal. On the side where the stimulus borders the imaging region, I focus on on ensuring there is no overlap between the imaging region and the stimulus. On the other side, at the edge of the FOV, I often let the stimulus run past the edge of the FOV a little to ensure stimuli reach the full fovea.


### Empirical confirmation of alignment during an experiment
The stimulus should be fully covering the receptive fields of the RGCs being imaged. The problem with misalignment isn't just that some RGCs don't get the stimulus; it's that the edge of the stimulus moves across the retina with eye motion (even when paralyzed there's still some due to respiration) and that moving edge is a strong stimulus for RGCs. This is a confound that will require omitting large numbers of RGCs from analysis, so confirming alignment is good to do at the beginning of each experiment.

The stimulus position in the model eye should be a good reference (see pre-experiment optimization protocols), but absolutely needs to be confirmed after alignments, in new imaging regions or when using new FOV sizes. I check in the model eye before each experiment (usually identifies times when pellicle has been bumped) and while running basic stimuli as described below.

When addressing misalignments in vivo, I wouldn't recommend adjusting mirror or the pellicle if you are aiming for publishable data as this will change your calibration. Instead adjust the pitch/rotation of the cart to move the imaging region towards ganglion cells with receptive fields. Anything more than small adjustments of the imaging region location indicates a larger problem that should be addressed offline.

These steps use the custom `stimResponseMap.ijm` macro for ImageJ which can be found in my [imagej-tools](https://github.com/sarastokes/imagej-tools) repository.

1. __Coarse alignment and system validation.__
    - Run the intensity increment.
    - Register the video and drag it into ImageJ.
    - Run the ImageJ macro and use the dialog box to ensure the stimulus frames are correct (for the standard intensity increment, it should be 500-750 frames).
    - You should clearly see the responsive cells in the resulting onset map (almost all respond to this intensity increment in some way)
2. __Fine alignment to fully center stimulus and remove edges.__
    - Run the "LightsOn" step stimulus which gives the cells time to adapt to a new light level. Then run the "Baseline" stimulus.
    - Drag the _unregistered_ fluorescence video into ImageJ
    - Run the ImageJ macro and set the stimulus frames to something like 500-1500.

</font>

__Next__: [LED Spectra Calibration](LED_Spectra_Gamma_Calibration.md)