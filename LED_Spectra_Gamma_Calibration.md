# Spectra and Gamma Calibration

<font size="2"> _Uses the Ocean Optics STS-VIS_

__Last Updated:__ December 12, 2024

__Warnings__:
- _If you change anything about the collection optics of the spectrometer (e.g., take a cosine corrector on/off), you need to do the [Spectrometer Calibration](Spectrometer_Calibration.md) before continuing!_
- _The streamlined procedure for measuring all 3 LEDs at once requires clean separation of each LED's spectral distribution. This is currently true but may change if the filters or LEDs are changed._
- _If the stimulus has uniform chromaticity before the pellicle but not after, get a new pellice (ThorLabs [BP208](https://www.thorlabs.com/thorproduct.cfm?partnumber=BP208))._

### Light source calibration
_Goal is to measure both spectral distribution and the nonlinear relationship between input command (e.g., LED voltage) and the light source's output (power in uW) using the Ocean Optics STS-VIS._
- Check the spectrometer and ensure the cosine corrector is in place. If not, add the cosine corrector and complete the [Spectrometer Calibration](Spectrometer_Calibration.md).
- Remove any NDFs, but keep the diffuser in place and turn the LEDs all the way up.
- Start OceanView software on 1P AOSLO laptop and plug in the spectrometer.
- Use the magnetic mount on the model eye stage to place the V-mount in the "LED calibration stand" drawer. Clamp down the spectrometer, ensuring the detector is in the same plane as the model eye lens (pupil plane) and straight rather than angled up/down/left/right. The cord needs to be propped up on the other components to ensure its weight doesn't tilt the spectrometer during the calibration.
- Center the LEDs on the detector by moving the model eye stage with the XY controller.
  - Tip: turning down the LED power with the dial on the driver can help centering, but remember to turn it back up once done.
- Turn off the room lights
- Go to the OceanView software, select the Spectroscopy application wizard and choose "Absolute Irradiance"
- Select "Active Acquisition"
- Set the number of averages between 3-5 and boxcar width to 2-3. Check "Baseline Correction" and "Nonlinearity".
- Set the integration time between 500-1000 ms (this can be changed for brighter or dimmer light sources).
- Switch all LED drivers to "CW" and check the measured spectra. If any of the LEDs' peaks exceed the horizontal line in the preview box, reduce the integration time.
- If measuring a full gamma ramp, make sure to switch the LED drivers back to "MOD" so that the computer can control their output.
- Click load irradiance calibration and load the most recent `.cal` file from the [Spectrometer Calibration](Spectrometer_Calibration.md) protocol (as long as the collection optics haven't changed!!).
- Block the LEDs (if on) and click "Store Background Spectrum"
- Click the Configure Graph Saving button on the toolbar. Change the folder where measurements are saved to your newly created subfolder. Add a descriptive name (e.g., 3LED_Gamma or NDF) and the following non-default settings: format = "ASCII (plain)", padding digits = 4, file suffix = file counter.
- Put the spectrometer in "Single Acquisition Mode" (_toolbar button to the right of play_).
- Load the correct LED voltage calibration stimulus file under the "LED2" tab of the AOSLO software and press play. I start with 5 V and decrease to 0 V in 0.2 V steps.
- Press the single acquisition button and wait for the spectra to update.
- Save the spectra.  Split the computer screen between the target folder and the OceanView software. This way you can confirm each file saved.


### NDF Calibration
_Calculating an "attenuation factor" for each LED which is the reduction in a light source's power associated with a specific ND filter. This can be approximated by taking the attenuation over wavelength data from the NDF's manufacturer (e.g., the graph tab for [ThorLabs' AR-coated ND filters](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=6272)), but it is also nice to obtain empirical measurements for each specific NDF used in the system._
- Create a subfolder in your calibration folder called "NDF" or something along those lines
- Set all 3 LEDs to 5 V
- Add each NDF individually and acquire the spectra. _Make sure to write down which files correspond to which NDFs!!!_ The analysis software will sort them by the number after "AbsoluteIrradiance__".
- Take a background measurement with the LEDs completely off (turn dial all the way down on LED drivers until you hear a click). You can also use a previously acquired one if you have already run a gamma ramp.


### Adjusting the LED current limit
_In general, we aim to utilize the full dynamic range of each LED by having them near 50% of their max output when showing an achromatic background. If one were at just 10% of their max output, all stimuli modulating around an achromatic background would be utilizing just a small portion of that LED's dynamic range. This isn't a huge issue because  command voltages to the LEDs are analog which mitigates the risk somewhat._

- Determine what % increase or decrease in maximum power is needed for the LED.
- On the LED driver, turn the dial all the way up (5V) and switch the output mode to CW to keep it on without input from the PC.
- Set up the model eye to do a power measurement of the  at the LED's peak wavelength.
  - Tip: You will need to see the optometer's output while standing next to the LEDs. After setting the wavelength, I usually rotate the box to face the side where the LED drivers are located, so I can look under the top table and see the output.
- On one side of the LED driver is a potentiometer for adjusting the LED's current limit.
- Rotate a small (can't remember size off the top of my head) hex key until the optometer reads whatever % of the original power level was your target.
- Remeasure the gamma ramp

Beware: the green LED becomes _very_ nonlinear when set to the bottom of its range, so I usually keep it a bit higher, even if it means using less of the LED's dynamic range.

### Analysis
This relies on the `GammaRampMeasurement` class in my [facile-tools](https://github.com/sarastokes/facile-tools) package on Github and the "Receptor Isolate Rochester" folder on Box.

- Add the facile-tools package and Receptor Isolate Rochester (which contains Psychtoolbox) to your MATLAB search path.
    ```matlab
    addpath(genpath('..\facile-tools'));
    addpath(genpath('..\Receptor Isolate Rochester'))
    ```
- Check out the class documentation for information
    ```matlab
    doc GammaRampMeasurement
    ```
- For each LED, create a class to process each LED's response. The documentation explains each input, but briefly they are a name for the LED, the folder containing the calibrations, the voltages associated with each file and the date of the calibration. The min and max WL are used to set the range to include just one of the 3 LEDs:
    ```matlab
    blueLED = GammaRampMeasurement('420nm', '..\MySpectraFolder', 5:-0.1:0, 2, '20220422', ...
        'MinWL', 400, 'MaxWL', 450);
    % Check the results
    blueLED.plotSpectra();
    blueLED.plotLUT(0:0.1:5);
    % Write the lookup table and normalized spectra
    blueLED.writeLUT('..\MySpectraFolder', 0:0.1:5);
    blueLED.writeSpectra('..\MySpectraFolder');
    ```
- Repeat with the NDF-specific calibrations. I number each NDF spectra and make sure 0 corresponds to the measurement made when the LEDs were off/at 0 V.
    ```matlab
    % For example, 4 measurements where the first is 5V at 0 NDF,
    % the second and third correspond to two different NDFs and the
    % fourth corresponds to a 0 V measurement
    blueNDF = GammaRampMeasurement('420nm', '..\MySpectraFolder\NDF',...
        [3 2 1 0], 2, '20220422', 'MinWL', 400, 'MaxWL', 450);
    ndfLUT = blueNDF.getLUT();
    % This shows the resulting powers
    openvar('ndfLUT')
    ```
- Calculate the attenuation of the 5 V measurement at 0 ndf vs. the target NDF (or combination of NDFs) and rewrite the LUT. I aim for a combination of NDFs that places the red LED between 1-1.5 uW ideally, though slightly higher is fine.
    ```matlab
    LUT = blueLED.getLUT(0:0.1:5);
    LUT.Power = LUT.Power * scaleFactor;
    writetable(LUT, '..\MySpectraFolder\LUT_420nm_20220422_10ndf.txt',...
        'Delimiter', '\t');
    ```
- Update `ConeIsolation` to reflect the LUT and normalized spectra files produced in your calibration (`DEFAULT_LED_DIR`, `DEFAULT_LED_FILES`, `DEFAULT_LUT_FILES`). The property `DEFAULT_LED_POWERS` should be the values of the 3 LEDs at 5V in your LUT files. __The order of LEDs is always Red, Green, Blue__ (If you were to change the connectivity between the LEDs and the LED drivers numbered 1, 2, 3; you would need to change this order to reflect that).
    ```matlab
    obj = ConeIsolation();
    ```
- Check the "Background 1931 xy chromaticity" output and adjust `DEFAULT_BACKGROUND` (a value 0-1 to multiply each LED by to reach ~0.33, 0.33). You can rapidly iterate through values by using the optional `Bkgd` argument and setting `DEFAULT_BACKGROUND` once happy. If the LED weights required to :
    ```matlab
    obj = ConeIsolation('Bkgd', [0.5 0.5 0.5]);
    ```
- Save as a JSON file for subsequent stimulus generation steps.  It will save to your current directory (`cd`) and contain your LUTs, normalized spectra, and other parameters from the `ConeIsolation` class. I hope to improve the `ConeIsolation` routine at some point so am using the JSON file to decouple it from the steps that follow.
    ```matlab
    saveCalibrationFile(obj, 'LedCalibration20240422.json');
    ```
</font>
