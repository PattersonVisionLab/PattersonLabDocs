# AOSLO Alignment
__Last Updated:__ November 11, 2024

First: [Source Coalignment](Source_Arm_Alignment.md)
Next: [AO Calibration](AO_Calibration.md)

##### General tips
- Do not touch the 90:10 beamsplitter unless absolutely necessary: this will alter the beam's path going back to the 90:10 and require aligning the detection arm.
- For mounts with more than two adjustable knobs, use the two on opposite corners of the mount.
- Do not adjust the height of a mount. Sometimes a ruler can be helpful: Level 1 is ~2.5 in, Level 2 is ~5.25 in
- Remove jewelery that could reflect light. Consider wearing a mask if your face will be close enough to the optics to breathe on them. Gloves when working where there are tight clearances between adjustment screws and mounted optics.

##### Setup
- Get the cardboard box containing the alignment targets from the cabinet near the 1P door
- Turn on the Toptica 561 nm (or 640 nm) laser and ensure the scanners are on. Align using the lowest power necessary and do not look directly at a beam
- Make sure the system is fully turned on, especially the scanners.
- The DM needs to be flat. If you have TsunamiWave open and previously had an AO loop closed, make sure to reset the actuators ("WS" tab, press "Reset").
- Easier to remove the pellicle beamsplitter between M8 and the last flat from its magnetic mount (LEDs will need to be aligned and recalibrated anyway).


##### Alignment

> M1-M8 refer to the eight curved mirrors forming the relays that image the pupil onto different components in the system. The two flat mirrors (one between M1 and M2, one after M8) aren't numbered. See the simplified [system diagram](img/SystemFigure.png).
> The marmoset arm adds an additional relay (M9 and M10) and 3 flat mirrors, which are aligned after the AO calibration (see [Marmoset Relay Alignment](Marmoset_Relay_Alignment.md)).

- Adjust M1 (f=600mm) to align on the center of the second mirror (flat).
    - _Note:_ Large deviations of M1 position could require realigning the detection arm
- Align the flat mirror to the center of M2 (f = 225 m) using target
- Align to the resonant scanner by adjusting M2
    - Increase power to be able to see the back reflection and use lens paper to see the spot on the scanner. Alternatively, this can be checked  with the pupil camera - looking right above the pupil camera will show the back reflection.
- Align to M3 by adjusting the resonant scanner
  - The adjustments are on the back of the scanner mount beneath silver screws and in middle top on side and require the smallest hex key. It is easiest to do this on a stool from the side of the optical table near M8 and the LED controller boxes.
- Align to M4 (f=150 mm) using the taller target (may need to hold against the mirror)
- Check pupil camera for image of the resonant scanner before going on to the galvo. An AC254-100-A-ML lens should be placed in front of the camera.
    - Put the extra flat mirror with yellow tape onto the magnetic mount with yellow tape. Shine a penlight or phone flashlight onto the resonant scanner and check to see if the DM is in the center of the resonant scanner.
    - If necessary use the LA1131PC f=50 mm lens to focus in front of the resonant scanner and coarse align the beam
- Shine light onto the galvo back through system to check image.
    - Won't see edges because of the galvo's larger size so count the turns across from edge to edge and try to center the image using M4
    - Right-left centered is more important than up/down because this is the horizontal scanner.
    - Might just be easier to align M4 onto the galvo by eye...
- Align to the center of M5 (f=150) by adjusting the galvo mount
- Moving the galvo mount slightly adjusts the position of the beam onto the galvo so you may need to iterate between the two steps above.
- Align to the center of M6 (f=376 mm) using M5.
- Align to the center of the DM (uncapped) using M6. Reduce/increase iris on source and increase power in order to see the spot on the DM. Return iris size and power after.
    - Note: don't use the dot on the DM cap as a target, it isn't actually where we want the beam centered.
- Align to the center of M7 (f=1000 mm) using the DM controls
- Align to the center of M8 (f=520 mm) using M7. No target but a ruler placed in front of the mirror works (add the distance to the bottom of the mirror to half the mirror diameter)
- Align to the center of the final flat mirror by eye.


Next: [AO Calibration](AO_Calibration.md)