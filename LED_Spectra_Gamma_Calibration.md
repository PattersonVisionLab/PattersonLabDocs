# Spectra and Gamma Calibration

<font size="2"> _Uses the Ocean Optics STS-VIS_

__Last Updated:__ December 12, 2024


> _If you change anything about the collection optics of the spectrometer (e.g., take a cosine corrector on/off), you need to do the [Spectrometer Calibration](Spectrometer_Calibration.md) before continuing!_
> _The streamlined procedure for measuring all 3 LEDs at once requires clean separation of each LED's spectral distribution. This is currently true but may change if the filters or LEDs are changed._

### Light source calibration
_Goal is to measure both spectral distribution and the nonlinear relationship between input command (e.g., LED voltage) and the light source's output (power in uW) using the [Ocean Optics ST-VIS](https://www.oceanoptics.com/spectrometer/st-vis/) spectrometer._
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
_In general, we aim to utilize the full dynamic range of each LED by having them near 50% of their max output when showing an achromatic background. If one were at just 10% of their max output, all stimuli modulating around an achromatic background would be utilizing just a small portion of that LED's dynamic range._

- Determine what % increase or decrease in maximum power is needed for the LED.
- On the LED driver, turn the dial all the way up (5V) and switch the output mode to CW to keep it on without input from the PC.
- Set up the model eye to do a power measurement of the  at the LED's peak wavelength.
  - Tip: You will need to see the optometer's output while standing next to the LEDs. After setting the wavelength, I usually rotate the box to face the side where the LED drivers are located, so I can look under the top table and see the output.
- On one side of the LED driver is a potentiometer for adjusting the LED's current limit ([image](img/LEDD1B.png)).
- Rotate a small (can't remember size off the top of my head) hex key until the optometer reads whatever % of the original power level was your target.
- Remeasure the gamma ramp

Beware: the green LED becomes _very_ nonlinear when set to the bottom of its range, so I usually keep it a bit higher, even if it means using less of the LED's dynamic range.

### Analysis
This step requires MATLAB with the Optimization Toolbox and Curve Fitting Toolbox and the following Github repository: [aoslo-calibration-tools](https://github.com/PattersonVisionLab/aoslo-calibration-tools)


- Add the code to your search path and import the package (not necessary but reduces typing).
    ```matlab
    addpath(genpath('..\aoslo-calibration-tools'));
    import pattersonlab.core.color.*;
    ```
- Check out the class documentation for information
    ```matlab
    help pattersonlab.core.color.GammaRampMeasurement
    ```
- For each LED, create a class to process each LED's response. The documentation explains each input, but briefly they are a name for the LED, the folder containing the calibrations, the voltages associated with each file and the date of the calibration. The min and max WL are used to set the range to include just one of the 3 LEDs:
    ```matlab
    blueLED = GammaRampMeasurement('420nm', '..\MySpectraFolder', 5:-0.1:0, 2, '20220422', ...
        'MinWL', 400, 'MaxWL', 450);
    % Check the results
    blueLED.plotSpectra('ShowCutoffs', true);
    blueLED.plotLUT(0:0.1:5);
    % Write the lookup table and normalized spectra
    blueLED.writeLUT('..\MySpectraFolder', 0:0.1:5);
    blueLED.writeSpectra('..\MySpectraFolder');
    ```
- You may have outliers or slight noise in your LUT measurement. Consider fitting the data to either a linear function (works for red and blue LEDs) or a polynomial (works for green LED).
    ```matlab
    blueLED.setLutFitType('linear'); % other options: none, poly8
    blueLED.plotLUT(0:0.1:5);
    ```
- Pass all 3 LEDs to the `pattersonlab.core.color.LedCalibration`
  ```matlab
  ledObj = LedCalibration([redLED, greenLED, blueLED]);
  ```

- Save as a JSON file for subsequent stimulus generation steps and to keep a record of the calibration in a non-proprietary file format. It will save to the `output` directory in the MATLAB calibration package and contains your LUTs, normalized spectra, and other parameters from the calibration calculations.
    ```matlab
    ledObj.exportToJSON();

    % Later you can recreate the full LedCalibration object
    ledObj = patterson.core.color.initFromJSON('LedSpectra_20220422_0ndf.json');
    ```

### Neutral density filters
There are two options for incorporating NDFs. Measuring the full gamma ramp with a high NDF in place reduces the spectras' SNR considerably.

##### Theoretical measurements
The MATLAB code includes ThorLab's transmission spectra for the NDFs we most commonly use ([source](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_id=266&pn=NE03A#811)). This will not be identical to the transmission spectra of the specific LED in our system, but it is close. For cone-isolating stimuli, I aim for empirical measurements instead.

The code below is identical to what was used above, but with an OD 1.0 NDF specified.
```matlab
blueLED2 = GammaRampMeasurement('420nm', '..\MySpectraFolder', 5:-0.1:0, 2, '20220422', ...
    'MinWL', 400, 'MaxWL', 450, 'NDF', 1);
fprintf('Max power with NDF (%.3f) and without NDF(%.3f)\n',...
    blueLED2.maxPower, blueLED.maxPower);
```


##### Empirical measurements
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
</font>

__See also:__ [OceanView Manual](resources/OceanViewManual.pdf)
