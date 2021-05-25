# Rack-ATC Module Setup Instructions

## Configuring Rack ATC Parameters

### Prerequisites
Before the Rack-ATC Module can be configured these steps must be completed first:

* If an older Version of the ProbeApp was already installed, complete these [ProbeApp-Update Instructions](update.md)
* If this is a new Install, complete these [ProbeApp-Installation Instructions](install.md)
* All Probing Devices like Touch Probe (TP) as well as movable and fix Tool Touch Off devices (TT) are wired and configured and confirmed working in CNC12
* Press the **ProbeApp** Button on the VCP and make sure the ProbeApp launches correctly
* Make sure the CNC12 ATC related parameters 6, 160 and 161 are all set to 0 as they are not needed

## 1. Start the ProbeApp with the VCP ProbeApp Button
Pressing the ProbeApp Button on the VCP should bring up the Main Screen of the ProbeApp:

![](/images/pa138.png)

On the ProbeApp Main Screen, press the **Tool Library Mgr** Button.

If this is the first time the Tool Library Manager is being started, the **Guided Setup** will open to configure all Probing Devices and setup CNC12 with the correct Tool Height Offset Method:

![](/images/pa102.PNG)

If you are getting the Guided Setup as shown in the Picture above, follow the [Guided Setup Instructions](ToolOffsetter.md) first and then come back here to continue with the next steps.

If the base Configuration of the Tool Library Manager has been completed and Saved and the Rack-ATC Module license is active, the following screen will be shown:

![](/images/pa135.png)

Press the **ATC Configuration** Button to open  the ATC-Configuration Screen.

## 2. Rack-ATC Configuration

![](/images/pa136.png)

Fill out the configuration parameters. If you need Help with any of the Parameters, select the *?* on the top right of the Configuration Screen and then hoover with the *?* over the Input-field you need Help with and a Balloon Help Message will provide more information.

If the Spindle requires Tool Orientation, check the box **Requires Tool Orientation** and enter the required commands.

Make sure the check box **ATC Functions Enabled** is checked before Saving the Configuration. This check box allows to turn off the ATC functionallity if required for some reasons.

## 3. Bin# Assignment
After the ATC Parameters have been saved, the Tool Library Manager Screen will show all the ATC Functions and Tools can now be assigned to Bin#'s.

![](/images/pa137.png)

Tool# to Bin# assignment scripts will instantly be updated after a change is being made and will be available in CNC12 after exiting the Tool Library Manager.

## 4. Configuring the M6 Tool Change Macro (mfunc6.mac)
The last piece in the Puzzle to get the Rack-ACT Module fully working is to configure the M6 Tool Change command **mfunc6.mac** in the CNC12 Folder **C:\cncm**.

The ProbeApp installation process will install a template file of the M6 Tool Change macro into the **C:\cncm** folder named:

```
C:\cncm\mfunc6.mac.customize-for-ProbeApp-ATC

```

Use this template, add all the commands that might be needed for your machine into the commented sections and then save the file as **mfunc6.mac** to activate it as the new M6 Tool Change macro.

This is how the M6 template file looks like:

```
;------------------------------------------------------------------------------
; File     : mfunc6.mac - to be used with ProbeApp ATC Module
; Purpose  : ATC Tool change macro for CNC12 using ProbeApp ATC module
;
; Usage    : The section required for the ProbeApp - ATC Module are between the ### lines
;            Add additional commands if needed for your Tool Change
; Author   : -swissi
; Date     : 15 May 2021
;------------------------------------------------------------------------------

IF #50001                          ;Force lookahead to stop processing 
IF #4201 || #4202 THEN GOTO 10000  ;Skip when graphing or searching

;------------------------------------------------------------------------------
; Turn Off Spindle 
;------------------------------------------------------------------------------
M5                 

;------------------------------------------------------------------------------
; Add Commands here that need to happen before getting Tool from ATC
;------------------------------------------------------------------------------
; Turn off Stuff: Coolant (M9), Mist (M8) etc.
; Automatic Dust Boot Parking?

;------------------------------------------------------------------------------
; Save XYZ Positions for machine to return to after Tool Retrieval
; Activate this Section if wanted. You need to activate also the Return at the end
;------------------------------------------------------------------------------
If #50001
;#101 = #5021
;#102 = #5022
;#103 = #5023

;##############################################################################
;------------------------------------------------------------------------------
; Start of Section required for ProbeApp - ATC Module
;------------------------------------------------------------------------------

#31998 = #4006    ;Save current G20/G21 mode
#31997 = #4003    ;Save current G90/G91 mode
G[#25001]         ;Force Machines Default Unit of Measure 

;------------------------------------------------------------------------------------------
; Force opening of ProbeApp-Tool Library Manager by Turning on Worklight 
; Activate these lines if this function is wanted
;------------------------------------------------------------------------------------------
;If #61112 != 0 Then #29500 = 12                     ;sets Flag to open ProbeApp-Tool Manager Library
;If #61112 != 0 Then M58                             ;opens ProbeApp-Tool Manager Library

If #50001
G65 "c:\cncm\probing\ATC_return_tool.cnc"            ;will return tool in spindle if needed
If #50001
G65 "c:\cncm\probing\ATC_get_tool.cnc"               ;will retrive the requested tool

;------------------------------------------------------------------------------------------
; Activate an automated Tool Height Offset Measurement Cycle with TT at fix Location 
; if the requested Tool has no Height Offset set in the Tool Offset Library.
;------------------------------------------------------------------------------------------
;If #[10000+#[12000+[#4120]]] == 0 Then #29500 = 40  ;sets Flag for Auto Tool Height Offset Measurement
;If #[10000+#[12000+[#4120]]] == 0 Then M58          ;activates an automated Tool Height Offset Measurement Cycle with fix TT

;------------------------------------------------------------------------------------------
; Force opening of ProbeApp-Tool Library Manager by Turning on Worklight 
; Activate these lines if this function is wanted
;------------------------------------------------------------------------------------------
;If #61112 != 0 Then #29500 = 12                     ;sets Flag to open ProbeApp-Tool Manager Library
;If #61112 != 0 Then M58                             ;opens ProbeApp-Tool Manager Library

;------------------------------------------------------------------------------------------
; Restore G20/G21 and G90/G91 Mode
;------------------------------------------------------------------------------------------
If #50001
G[#31998]  ;Restore G20/G21 mode
G[#31997]  ;Restore G90/G91 mode

;------------------------------------------------------------------------------
; End of Section required for ProbeApp - ATC Module
;------------------------------------------------------------------------------
;##############################################################################

;------------------------------------------------------------------------------
; Add Commands here that need to happen after getting Tool from ATC
;------------------------------------------------------------------------------
; Retrieve parked Dust Boot?

;------------------------------------------------------------------------------
; Return to Position where M6 Tool Change command was issued
; Activate this Section if wanted
;------------------------------------------------------------------------------
;G90 G53 Z#103  
;G53 X#101 Y#102 

N10000     ;End of macro  

```

Some of the additional functions are commented out with a **;** and are inactive.
Remove the **;** at the beginning of the line if you would like to activate the function.

Only activate the automated Tool Height Offset Measurement function if there is a configured, fully operational fix Tool Touch Off device available on the machine.

The Rack-ATC Module should now be fully operational.

## 5. Testing the ATC Functinality
Until you have confirmed that all your Bin Locations are accurate and all the other ATC Configuration parameters are correct, be **very careful** when testing the ATC functionality.

I suggest that you turn the Feedrate Overwrite all the way down and always keep a finger on the **Cycle Cancel** or the **Emergency Stop** button.

Also Note that the **Feed Hold** Button is inactive while CNC12 is in a M6 Tool Change command so all the moves that occure in a M6 Tool Change cannot be stopped with the **Feed Hold** button. You have to use **Cycle Cancel** to stop the move.

### Testing the ATC Functionality
* Assign all Bin#'s to a Tool# in the Bin# Column. To mix things up a little you can assign Tool #10 to Bin #1, Tool #20 to Bin #2 etc. until you have all Bins assigned to a tool.

* If your system doesn't use any sensors if a tool is actually in a bin or if a tool is loaded in the spindle or not, you can start the tests with an empty spindle and empty Bins just to verify if all the moves are correct. If your machine does have sensors, you might test with an empty tool holder.

* Use the **Load Tool** and **Unload Tool** Buttons on the **Tool Library Manager** screen to test out all Bins.

* If you have to fine tune any Bin coordinates, Bin Height or any other ATC Configuration parameter, just press the **ATC Configuration** button, modify the parameter and press the **Save Configuration** button. All changes will be immediately active.

* Now try to load a Tool# that does not have a Bin# assigned. A manual Tool Change at the configured **Manual Tool Change Position** should now occur.

* If there is a Tool Touch Off device configured on the system, try the **Measure** button of a tool to measure the Height Offset value of the tool. The cycle will use the fix TT if one is available, otherwise the movable TT will be used and you will be asked to jog the tool over the movable TT.

* If the system has a fix TT, load a tool in each Bin and make sure the tools are correctly assigned to the Bin# in the Tool Library Manager. Then use the **Measure all Tools in ATC** button to measure the Height Offset of all tools in the rack.

* Make sure Tool #1 and #2 are assigned to a Bin# and that those tools are in the assigned Bins. Then use the **Load Tool** button to load T1. Tool in Spindle should no show T1. If that's the case, exit the **Tool Library Manager**.

### Testing M6 ATC Tool Change Functionality
Open the MDI from the CNC12 Main screen.

* In the MDI, enter a Tool Change command for Tool #1 (T1 M6) and Press Cycle Start. T1 should already be in the spindle from the test step before. The M6 Tool Change macro should recognize that T1 is already in the spindle and the M6 macro should just finish without any tool return/retrieval function.

* Enter a Tool Change for Tool #2 (T2 M6) and press Cycle Start. The machine should now return T1 to its Bin and then load T2. Make sure any other action configured in the M6 macro like parking and retrieving a Dust Shoe etc, are occuring and in the correct sequence.

* Enter a Tool Change for a Tool # that is not in the rack. Tool #2 should be returned to its Bin and Message will be shown that the requested tool is not in the rack. You will have the option to do a manual tool change or to open the Tool Library Manager where you can re-arrange Tool# to Bin# assignments. Select the option to open the Tool Library Manager. Now assign a Bin# to that tooll and place that tool into the specified Bin. Exit the Tool Library Manager, the tool change should now continue, retrieving the tool from the now assigned Bin.

* Enter again a Tool Change for a Tool # that is not in the rack. This time select the option of a manual tool change and verify if the manual tool change is happening correctly at the configured location.

If this all works correctly, the Rack-ATC Module should be ready for real action.

## Changing ATC Configuration Parameters
If any adjustments to the Rack-ATC Module need to be made, just press the **ATC Configuration** Button on the **Tool Library Manager** screen.

Any changes made to the ATC Configuration Parameters will instantly be active after pressing the **Save Configuration** button.

Note that changing the number of Bins will reset all Bin Coordinate Data in the table.

If the check box **ATC Functions enabled** is unchecked, all ATC Functions will be disabled and all Bin# will be reset to 0 which will force all manual Tool Changes.
In order to fully remove the Rack-ATC Module, the M6 Tool Change Macro mfunc6.mac should be exchanged with a macro that's setup for manual tool changes and the Rack-ATC Module license needs to be removed.

## Other Tool Library Manager Functionality
The Rack-ATC Module is an extension of the **ProbeApp-Tool Library Manager***.

For details about all the non-ATC Module related functionality and configuration of the **Tool Library Manager**, consult the [Tool Library Manager](ToolOffsetter.md) Chapter. 





[Back](index.md)

