# osPID Firmware

## Feature Summary
- Two new profiles added:
 - Type 4: **step-output-period** - set power Output (%) to a specified level for a specified period.
 - Type 5: **step-output-until\_crossing\_temp** - set power Output (%) to a specified level until a specified Input temperature is attained.
- Version info is now stored in a string. This ensures the same version number is reported in both the controller LCD and the Front-End terminal window.
- Controller is reverted to automatic mode after stopping any profiles - important because Profile Types 4 and 5 place the osPID controller into MANUAL mode.

## Why this fork?

After a not-so-short period of time spent watching an  [Elgento E048 9L Mini Oven](http://direct.asda.com/Elgento-E048-9L-Mini-Oven/001575061,default,pd.html), (controlled by an osPID controller running osPID-Firmware v1.6) cycle through a known good reflow soldering profile - while making small and not so small tweaks to the PID settings - it became apparent that I understood more about how the controller should be modulating its Output than it did, and less about the beautiful mathematics of PID required to make the controller 'do it's thing'.

 Determined nevertheless, to use the osPID controller to build the equally open source [OTM-02](https://github.com/hairykiwi/OTM-02) digital watch module prototypes, and spurred on by the goodly advice (KISS, essentially) of [MikesElectricStuff](http://www.electricstuff.co.uk/) in his video: [Some random hints for quick hand SMD assembly](http://youtu.be/pdGSFc7VjBE?t=14m) the following profiles were created:

- Type 4: **step-output-period** - set the power Output (%) to a specified level for a specified period.
- Type 5: **step-output-until\_crossing\_temp** - set the power Output (%) to a specified level until a specified Input temperature is attained.
  
  Note: - Both the above profile types can be mixed with existing PID ones.

With regard to reflow soldering, Type 4 was more of a learning step on the way to creating Type 5, though no doubt it could be useful for some other applications. Type 5 came about because the existing Type 2 - **Wait**, contains no 'desired temperature' data.

The Type 5 profile is very much a dirty hack, in that the variable normally used to store a time period (milliseconds) is reutilised to store a desired 'crossing temperature' (°C). Although the controller functions properly, the knock-on consequences are that the data sent to the Front-End 'Status' window - and also the resulting profile graph created under the PROFILE tab - are at present, less than ideal:

- What data that does appear in the Status window should not be used for *anything*, and
- The profile as displayed under the PROFILE tab currently mixes Output (%) and temperature (°C) units, if the original profile types are mixed with Type 4 or 5 profiles.

### Benefits
- Using Type 5 profiles for reflow soldering could require less experimentation time during initial set-up than using a PID solution.
- Smoother temperature ramp-rates and smoother transitions between different ramp-rates are possible, because the Ouput is (typically) held constant over a longer period.

## Useage - Reflow soldering
After determining an oven's maximum ramp rate and the change in ramp-rate over the entire temperature range (note, this is generally non-linear for small, poorly insulated ovens) a user can begin to appreciate just how much power is required to be supplied to the oven in order to produce the desired ramp rates for each of the three heating phases as recommended by the IPC/JEDEC J-STD-020 reflow soldering profile.

## Dependencies
To load a profile incorporating a Type 4 or Type 5 profile you will need:

- [hairykiwi/osPID-Front-End](https://github.com/hairykiwi/osPID-Front-End.git)  - please note this is very much a work in progress. This repo also contain some example profiles using Type 4 and Type 5 profiles. [This profile](https://github.com/hairykiwi/osPID-Front-End/blob/master/osPID_FrontEnd/profiles/RoHS_Elgento024.txt) was successfully used to solder the first [OTM-02](https://github.com/hairykiwi/OTM-02) prototype.

## Version
See **firmwareVersion** string in osPID_Firmware.ino. This ensures the same version number is reported in both the controller LCD and the Front-End terminal window.

## Credits
This Firmware is based on:

    /********************************************************
    * osPID Firmware,  Version 1.6
    * by Brett Beauregard & Rocket Scream
    * License: GPLv3 & BSD License (For autotune)
    * August 2012
    ********************************************************/
