# Rack-ATC Module Setup Instructions

## Configuring Rack ATC Parameters

### 1. Prerequisites
Before the Rack-ATC Module can be configured these steps must be completed first:

* If an older Version of the ProbeApp was already installed, complete these [ProbeApp-Update Instructions](update.md)
* If this is a new Install, complete these [ProbeApp-Installation Instructions](install.md)
* Start CNC12 and make sure the ProbeApp Button on the VCP is launching the ProbeApp correctly
* All Probing Devices like Touch Probe (TP) as well as movable and fix Tool Touch Off devices (TT) are wired and configured and confirmed working in CNC12

## 2. Start the ProbeApp with the VCP ProbeApp Button
From the ProbeApp Main Screen, select the **Tool Library Mgr** Button.

If this is the first time the Tool Library Manager is being started, the **Guided Setup** will open to configure all Probing Devices and setup CNC12 with the correct Tool Height Offset Method:

![](/images/pa102.PNG)

If you are getting the Guided Setup as shown in the Picture above, follow the [Guided Setup Instructions](ToolOffsetter.md) first and then come back here to continue with the next steps.

If the base Configuration of the Tool Library Manager has been completed and the Rack-ATC Module license is active, the following screen will be shown:

![](/images/pa135.png)

Press the **ATC Configuration** Button to open  the Configuration Screen.

## 3. Rack-ATC Configuration

![](/images/pa136.png)

Fill out the configuration parameters. If you need Help with any of the Parameters, select the *?* on the top right of the Configuration Screen and then hoover with the *?* over the field in question and a Balloon Help Message will show up.

If the Spindle requires Tool Orientation, check the box **Requires Tool Orientation** and enter the required commands.

Make sure the check box **ATC Functions Enabled** is checked before Saving the Configuration. This check box allows to turn off the ATC functionallity if required for some reasons.

## 4. Bin# Assignment
After the ATC Parameters have been saved, the Tool Library Manager Screen will show all the ATC Functions and Tools can now be assigned to Bin#'s.

![](/images/pa137.png)

Tool# to Bin# assignment scripts will instantly be updated after a change is being made and will be available in CNC12 after exiting the Tool Library Manager.

## 5. Configuring the M6 Tool Change Macro (mfunc6.mac)
The last piece in the Puzzle to get the Rack-ACT Module fully working is to configure the M6 Tool Change command **mfunc6.mac** in the CNC12 Folder **C:\cncm**.



The Rack-ATC Module is doing its own Bin Management. No Parameter Changes in CNC12 to activate and configure ATC Bin functionality are needed.
The ATC related CNC12 parameters can all be set to 0:

* Parameter 6 = 0
* Parameter 160 = 0
* Parameter 161 = 0



## M6 Tool Change Macro
All that's needed in the Tool Change Macro *mfunc6.mac* to retrieve and store tools in the rack are two simple command lines:

```
G65 "c:\cncm\probing\ATC_return_tool.cnc" ;will return tool in spindle if needed
G65 "c:\cncm\probing\ATC_get_tool.cnc"    ;will retrive the requested tool

```

Additional logic as described in the [ATC Setup Guide](ATCsetup.md) can be added to provide additional functionality like automated Tool Touch Off for Tools that don't have a valid Tool Height Offset configured yet.



[Back](index.md)

