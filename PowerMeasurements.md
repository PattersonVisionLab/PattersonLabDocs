# Power Measurements

<font size="2"> __Last Updated:__ April 14, 2025

Record power measurements in the [Imaging Coversheet](/resources/1P_NHP_imaging_notes_coversheet.pdf) the morning of an experiment.


- Attach the model eye to the Velmex stage with the two 1/4-20 bolts (should be in-place)
- Place the Newport power meter (on mobile cart - meter should be in basket behind screens, main device on the shelf under the table) onto the magnetic mount. When looking at the model eye from the side, the power meter should be colinear with the back of the aperture on the model eye (where it is flush against the cage plate)
- Turn on a visible source to align the power meter by moving the XY position of the model eye. The alignment does not need to be perfect - you can fine-tune when making the first power measurement (described below).
    - If the pellicle is in but not the beam block, make sure you are using the light from the last flat, not the transmitted light from the pellicle.
- Stop emission on all light sources (or block visible ones with shutter). Make sure only one light source is on at a time when measuring the power.
- Turn on the power meter controller. Press "Zero" until the output reads near zero.
- Press the "wavelength" button and use the arrow keys to change the wavelength to match that of the first source you want to measure.
    - Mustang: 488 nm, Toptica: 488, 561 or 640 nm, Reflectance: 796 nm, Wavefront sensing source: 847 nm
- For the Toptica and Mustang measurements, check "Calibration" on the Imaging Software**.
    - The "Calibration" checkbox on the Imaging Software for the visible lasers keeps them on continuously (normally laser is turned off during the backscan of the resonant scanner to reduce exposure). The power measurement is easier this way, but the light safety calculations take into account exposure per unit time, so divide the measured power in half when using the REL calculator.
- You can make fine adjustments to the alignment of the beam on the sensor by moving the model eye to maximize the power reading.
- If using the Maxwellian View, make sure the NDF you intend to use is in place (screw into diffuser on iris). Turn on the first LED controller box by increasing the dial to "Limit" and moving the switch to "CW".
    - If the LEDs are adjusted to hit the left or right side of the FOV, the beam center will not be the same as the AOSLO light sources. Adjust the model eye position again to maximize the power output to get an accurate measurement.
    - For current LEDs, filters and pellicle - Red: 660 nm, Green: 540 nm, Blue: 420 nm
- Switch each LED controller's input switch back to "MOD" when done measuring, but leave the dial turned up to "Limit" - this will allow the computer to control the LED's output.
- Remove the power meter and place in basket on the back of the mobile cart.
- Move the model eye stage's horizontal position all the way to the center of the AOSLO to move out of the stereotax cart's way.

