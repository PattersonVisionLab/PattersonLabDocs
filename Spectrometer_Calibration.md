# Spectrometer Calibration

<font size="2"> __Last updated:__ November 13, 2024


__When to perform:__ Any time the collection optics of the spectrometer change (e.g., cosine corrector is taken on/off).


- If you are using the cosine corrector (default for Maxwellian View LED calibration), screw it onto the loose end of the fiber, loosen the locking screw on the calibration lamp, then insert the fiber with the cosine corrector into the calibration lamp and re-lock. If you are using the bare fiber, screw the fiber into the calibration lamp.
- Turn on the calibration lamp and __wait 15 minutes__ for it to warm up. After 50 hours of total runtime, the lamp requires recalibration so log use by adding a tally mark to the box.
- Start OceanView software on 1P AOSLO laptop and plug in the spectrometer
- Select the Spectroscopy application wizard and choose "Absolute Irradiance"
- Select "Active Acquisition"
- Open the shutter on the back of the cal lamp, and click "Auto" for integration time on the Ocean View software.
- Click "Next" and choose "New Calibration"
- Click "Store Reference Spectrum" and then click "Next". Because this is absolute irradiance, the reference spectrum doesn't matter
- Close the lamp shutter and click "Store Background Spectrum"
- Open the lamp shutter again, load the appropriate cosine corrector (cc) or bare fiber (bfw) `.lmp` lamp calibration file
  - `089430516_HL-3 plus-CAL_bfw_20190130_VIS.lmp` or `089430516_HL-3 plus-CAL_cc_20190130_VIS.lmp`
- Enter the fiber diameter (3900 microns for the cosine corrector, 400 microns for the bare fiber)
- Save the calibration in the "Spectrometer files" folder with the date in the file name (e.g., "20241013")

__See also:__ [OceanView Manual](resources/OceanViewManual.pdf)
__Next:__ [LED Spectra and Gamma Calibration](LED_Spectra_Gamma_Calibration.md)