# TP touch off on TT Cycle Improvements

Most Touch Probes (TP) and Tool Touch Off (TT) devices don't have any significant pre-trip-travel in the axial direction so touching off a TP on top of the TT is not a problem in many cases as it doesn't matter if the TP or the TT is generating the trip signal as long as both devices are connected at the same time.

If your control isn't wired to allow the simultanous connection of the TP and TT, then you need to figure out which device has the weaker spring load as this will be the device that will generate the trip signal and just connect that device. You can test that by placing the TP by hand on top of the TT, push down the TP slightly and see if the stylus of the TP moves up or if the plate of the TT moves down. In most cases, the TP will have the weaker spring than the TT.

Now a problem occurs when the TT does have a pre-trip-travel distance and the TP is creating the trip signal. The offset measurement will be incorrect by the amount of the pre-trip-travel which can be significant. As an example, the very popular, wireless DT02 is such a device:

![](/images/pa174.png)

### Special TP measurement
To compensate for the issue of a TT pre-trip-travel distance, the configuration page of the ProbeApp - Tool Library Manager provides now two options:

![](/images/pa175.png)

* The **Deduct TT-Pre-Travel Distance** option allows to compensate for the TT's pre-travel. Just measure how much the TT travels from the extended position to the point when the trigger signal is beeing generated and enter that distance into the input field. Any touch off with a TP on top of the TT will now be corrected with the Pre-Travel-Distance.

* The **Manual touch off at base of TT** option will not touch off the TP on top of the TT and the user will be asked to touch off the TP on the surface the TT is standing on.
The TT Height value will then be used to calculate the correct Height Offset. Note that the Height value needed here is not the height of the extended TT. This must be the Height from the point where the TT is generating the trigger signal to the base of the TT.



[Back](index.md)

