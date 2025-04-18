# Source Arm Alignment

<font size="2"> __Last Updated:__ April 14, 2025

Complete before the [AOSLO system alignment](AOSLO_Alignment.md) or to determine whether one may be necessary.

### Check source collimation
_There are two options for source collimation documented below. The shear plate is faster but less precise and harder with long-wavelength sources._

- Remove beam block from magnetic mount
- Set the Toptica, Mustang and reflectance source positions to their model eye values
- _Option 1: Portable Wavefront Sensor_
  - Put the pinhole with red tape directly against the WFS cage bracket that also has red tape. The side of the pinhole with the red tape should face away from the source.
  - Place the WFS on a short TR post in an adjustable base right after the magnetic mount where the beam block is typically placed.
  - Plug into the 1P AOSLO laptop and open "ThorLabs Wavefront Sensor" software.
  - Turn off room lights.
  - Center the WFS beacon (height and left/right) and make sure the rotation of the portable WFS minimizes tip/tilt.
  - For each additional source:
    - Use pinhole or adjust aperture to ensure the beam is smaller than the the WFS detector. May also need NDFs to avoid saturation.
    - Optimize source Z position (via Kinesis software on PC3) until defocus reaches zero. The WFS beacon shouldn't require adjustment, but check and confirm.
- _Option 2: Shear Plate_: The laser reflects off the front and back side of a wedge within the shear plate; these two reflected beams interfere with each other and create a fringe pattern. If the beam is collimated, the fringe pattern will be oriented along the direction that the input beam travelled. If the beam is converging or diverging (a.k.a. not collimated), the fringe will tilt. If you aren't familiar with shear plates, ThorLabs has a nice [video](https://www.youtube.com/watch?v=iNgD4UKAuXg) illustrating how to interpret the fringes.
  - Place the shear plate on the magnetic mount (it doesn't have a magnetic bottom, but fortunately this is about the right height). If there are any posts attached to the shear plate, remove them.
  - It may be necessary to increase/decrease the laser intensity to view the fringes.
  - Adjust the source position using the ThorLabs Kinesis software until the fringes are parallel to the input beam's path. Use pinholes or decrease the aperture for each source.

### Coalign sources with the WFS beacon
_The WFS source (847 nm) is the furthest right and serves as the reference for positioning the other 3 sources. The other sources, from right to left, are the reflectance source (796 nm), the Toptica (488/561/640 nm - we usually use 561 nm for alignment), the Mustang (488 nm)._


Set up the co-alignment camera system
- Remove the partition between the PMTs and source arm.
- Use two 1/4-20 bolts to mount the breadboard with the source alignment cage system (beamsplitter cube with two arms, one that has a lens and one that does not) to the optical table.
  - This can be mounted at the end of the table downstream from the removed magnetic mounted beam block.
  - To have more control over the position of the beam on the cameras, put the magnetic mounted flat mirror on to the left of the beamsplitter and place the breadboard to align with the beam reflecting off this mirror. In this configuration, watch out for secondary reflections!
- If the cameras aren't attached, slide them on to the end of each rail until the rod passes through the CP36 cage plate and reaches the camera. Use the set screws to lock into place.
- Check the alignment of one of the visible sources with the cage system using t-shirt targets in front of both cameras and in front of the beamsplitter cube.
- Place an NDF (1.0-2.0 OD) on a drop-in mount into the arm with the lens.
- Plug the cameras into the 1P laptop and open ThorCam. Open a window for each camera and set them side-by-side.
- Turn off room lights

Mark the wavefront sensing source position

- Put the red pinhole on the WFS source cage with the tape facing away from the source. Block all other sources with the shutter and/or cards.
- You will need to turn the WFS source down considerably, or use a drop-in ND filter to see the center of the beam clearly.
- In the ThorCam software, change the annotation color to something like red that is visible against black and white (default is yellow, which is hard to see against white).
- Click the circle annotation and draw one around the WFS source Airy disc. This can take a few tries to get right because the circle is drawn by dragging out from a center point. Sometimes, I add crosshairs too, especially on the arm with the lens.

Align each source to the WFS beacon. Start with the Mustang, then the Toptica and finally the reflectance source. _The mounts in the source arm are located close together, so consider wearing gloves for this step._
- Put the yellow pinhole on the source cage with the tap facing away from the source.
  - __Important:__ Push down on the pinhole to ensure it is fully attached and slide the pinhole up to the nearest cage component (to ensure repeatability).
- Block the WFS source with a card and compare the beam locations to the reference circle annotations.
- Adjust the source power to be visible in both arms (reflectance should be on "low" setting, Toptica often as low as 0.6% on the Toptica software and ~30% on the imaging software).
- The arm with the lens is the "far-field" and the arm without the lens is the "near-field". Adjust the mirror nearest to the source to align the beam in the near-field camera. To align the beam in the arm with the lens (far-field), adjust the dichroic (or pellicle for Toptica) which is farther from the source. There are labels on the optical table identifying the mirror and beamsplitter corresponding to each source.
  - __Important__: Do not change the Z axis screw on the mirrors!!! If the mirror has 3 adjustment knobs in an L-shaped arrangement, the Z-axis screw is the middle one.
  - For the dichroics combining the reflectance source and the visible light sources with the WFS, you will need a 5/64" hex key to adjust the mount.
  - Tap each mirror mount/adjuster with a small hex key after finishing adjustments to ensure mount does not "settle" into a different position.
- Iterate between the two until the beam is aligned with the circle annotations in both camera windows. Overshooting can make the process faster.
- As you work through the sources, ensure the previous ones are blocked with cards or by closing the visible shutter.

Go to lunch/coffee or head home for the day, then come back and ensure the sources are still coaligned.


### Adjust the half-wave plates
Both the Mustang and Toptica have [AHWP10M-600](https://www.thorlabs.com/thorproduct.cfm?partnumber=AHWP10M-600) in CRM1T mounts - the Mustang's is on the cage prior to the aperture, the Toptica's is post-mounted before the pellicle. The half-wave plate for the reflectance source is post-mounted after the first flat ([AHWP05M-980](https://www.thorlabs.com/thorproduct.cfm?partnumber=AHWP05M-980) in an [RSP1](https://www.thorlabs.com/thorproduct.cfm?partnumber=RSP1) mount).

- Place the Newport power meter on the magnetic base for the beam block
- Measure the power. See [power measurement](PowerMeasurements.md) protocol for more details. Briefly:
    - Block all the sources with cards/shutter and press "Zero"
    - Unblock one source at a time and adjust the wavelength for the power meter accordingly.
- Unlock the rotation control for each half-wave plate mount
- Adjust the rotation to maximize power.
- Remember to lock the rotation mount once done and ensure that didn't change the power significantly.

### Restore default state
- Replace the beam block on the magnetic mount
- Remove cards blocking sources
- Make sure drop-in NDFs are removed from the source cages
- Check apertures for each source are at their default values of 9/10.
- Remove pinhole from wavefront sensing source and whichever source you recently coaligned (unless you plan to use one on the Toptica source for the AOSLO alignment)
- Remove breadboard with camera cage system (put in engineering room cabinet).
- Replace the cardboard partition between the source arm and the PMTs

Next: [AOSLO System Alignment](AOSLO_Alignment.md)