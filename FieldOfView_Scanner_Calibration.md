# Scanner Field of View Calibration

<font size="2"> __Last Updated:__ December 12, 2024

__Prep__: The analysis requires FIJI with the BAR plugin. This is installed already on the 1P computer. If using your own computer for analysis, open FIJI and go to Help -> Update -> Manage Update Sites. Check "BAR". Click "Close" then "Apply Changes". You may need to restart FIJI for the plugin to work.

You will also need the Excel spreadsheet called `FOV_Calibration_Template.xlsx` which can be found in the resources folder. There is a sample spreadsheet from a previous calibration in the examples folder.

### Measurement
- Start the system and turn on the reflectance and wavefront sensing sources
- Start the FrameGrabber imaging software
- Use the 80 lines/mm Ronchi ruling grid (usually found in the cabinet by the door)
- For aligning the slow scan, put the grid horizontally where the model eye paper is horizontal. For aligning the fast scan, put the grid in vertically.
- Ensure the lines are as straight as possible in the reflectance image (use a small FOV)
- For the fast scan: Keep V fixed in the FrameGrabber and adjust H from 55-245 in increments of 10. At each H value, take a 2 second reflectance video (update video name to include H value and "horizontal" each time to avoid later confusion and the need to go back and recollect data).
- For the slow scan: Keep H fixed in the FrameGrabber, and adjust V from 1-20 or so, but can go higher if necessary. Again, take a 2 second reflectance video for each V value and ensure it's named correctly (include value and "vertical").
- The model eye is stable, but we sometimes register anyway to increase precision.

### Analysis
> The relationship between scanner voltage and FOV should be linear. If you have a measurement that is clearly an outlier, trust a linear fit through the other points.
- If you registered the videos, drag the `_frame.tif` file into FIJI. Otherwise, drag the video into FIJI, then take an average Z-stack to get a single image.
- Draw a rectangle across the entire set of black and white fringes in FIJI
- Click "Analyze -> Plot Profile". If the fringes are horizontal and your rectangle is vertical, you will need to hold "ALT" as you click "Analyze -> Plot Profile" so that it knows to take the vertical direction.
- Click "BAR -> Data Analysis -> Find Peaks", then set the minimum peak as desired (just in case the data were noisy for some reason)
- The program will show you all the maxima and minima that it found. You can check by eye to make sure it got all the ones near the edges.
  - Tip: H values can be interpolated somewhat, but because the spacing for V values is unity, you are stuck with the degrees you get from each V value.
- Open the Excel spreadsheet `FOV_Calibration_Template.xlsx` and save as a copy with the date appended (e.g., `FOV_Calibration_20250410.xlsx`).
- The number of minima and maxima added together is the "number of complete black and white cycles" value that you will enter into the spreadsheet for each H/V value. You will also need the pixel value of the first and last peaks.
- The spreadsheet will then calculate each field of view in degrees for you with the following formula: $2*atan(h/f)$ where $f$ is the focal length (19 mm) and $h$ is the half height of the image in mm (calculated as $N/LP$ where $LP$ is the line pairs per mm (80) and $N$ is the number of peaks you identified).
- Plot the voltage vs. calculated FOV to check for outliers.
  - Fit a linear trendline to the plotted data ("Chart Tools, Design tab > Add chart element > Trendline > Linear").
  - Open the "Format Trendline" window and check "Display Equation on Chart".
- The equation can be used to calculate the FOV for voltages you did not measure or got an outlier measurement.
  - This is especially useful for the fast scanner, which has a wider range of valid voltages (55-245) that we measure in increments of 10. You can use the equation to obtain FOV values closer to those you were previously using.
  - The measurement spacing between voltages for the galvo scanner is unity (1-20), so you will have to work with whatever FOV values you get from that calibration.

### Updating the Imaging Software UI
- Go to the "real-time" folder, right click on the correct software version (currently, we use "Tracking_Sara") and click "Open file location". Navigate up a folder and click on `Tracking-AO-SLO-GUI.sln`. This should open it up in Visual Studio.
- Open `AO-SLO_Imager\AO-SLO_ImagerView.cpp` (or CTRL+F and search `m_strScannerFOV` to reach the file).
- Navigate to the definition of the `m_strScannerFOV` variable, which should be around line 140. Each entry defines the values in the FOV dropdown menu. You want to match these values as closely as possible, but may need to change them slightly, especially the V values that can't be interpolated (see above).
    ```cpp
    m_strScannerFOV         = new CString [16]; // Increase if you need >16 values
    m_strScannerFOV[0]      = _T("0.60 x 0.60 (496 lines)");
    m_strScannerFOV[1]      = _T("0.98 x 0.98 (496 lines)");
    m_strScannerFOV[2]      = _T("1.58 x 1.58 (496 lines)");
    // And so on...
    ```
    The first number is vertical, the second number is horizontal. Once the FOV dimensions (e.g., 0.60 x 0.60) have been adjusted, I usually copy this block into a Notepad window as it will be useful to have easily accessible during the next step.
- Navigate to the function called `SetScannerFOV` that defines the voltages associated with each FOV in `m_strScannerFOV`. It should be around line 4400 so use CTRL+F. Update the comment and the variables `fovV`, `fovH`, `fV`, and `fH`.
    ```cpp
    void CAOSLO_ImagerView::SetScannerFOV(int index)
    {
        // The voltages for a given FOV
        BYTE    fovH, fovV;
        // The degrees for a given FOV (V x H)
        float   fH, vH;

        switch (index) {
            case 0:
                // m_strScannerFOV[0]   = _T("0.60 x 0.60 (496 lines)");
                fovV    = 3;
                fV      = 0.60;
                fovH    = 23;
                fH      = 0.60;
                break;
            case 1:
            // And so on...
        }
    }
    ```
   The number after `case` corresponds to the index in `m_strScannerFOV`. If there are too many, delete from the end rather than leaving them un-updated.
- Build the solution and then open the imaging software to see your changes. Sometimes I find the changes are not reflected until the 2nd time I reopen the imaging software.
- Save the Excel file and include the date in the title to document the calibration.
