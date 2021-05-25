# Rack-Type Automatic Tool Changer Module
The ATC Module is integrated into the ProbeApp-Tool Library Manager and can be activated with a license update.

The Rack-ATC Module supports Fork/Finger as well as Drop-In Type racks positioned along the X or Y Axis and on the front, back, left or right side of the table.

With the Rack-ATC Module activated, the Tool Library Manager will expand with a Bin# Column as well as ATC specific Function Keys.

![](/images/pa133.png)

The Rack-ATC Module has its own, built-in Bin Management. No Parameter Changes are required in CNC12 to activate and configure ATC Bin functionality.

There's one simple ATC Configuration screen to fill out. The Rack-ATC module will auto-generate all script files needed for proper ATC operations.

![](/images/pa134.png)


## M6 Tool Change Macro
All that's needed in the Tool Change Macro *mfunc6.mac* to retrieve and store tools in the rack are two simple command lines:

```
G65 "c:\cncm\probing\ATC_return_tool.cnc"            ;will return tool in spindle if needed
G65 "c:\cncm\probing\ATC_get_tool.cnc"               ;will retrive the requested tool

```

Additional logic as described in the [ATC Setup Guide](ATCsetup.md) can be added to provide additional functionality like automated Tool Touch Off for Tools that don't have a valid Tool Height Offset configured yet.

## Advantages of the Rack-ATC Module over standard ATC functionality

All Tool related parameters like Bin#, Diameter, Height Offsets can all be managed from one screen.

When a Tool is requested that's not in the Rack, there's an option to load the Tool manually or to open the Tool Library Manager.
This allows to open the Tool Library Manager while the job is still active in a M6 Tool Change hold. 
It is then possible to re-arrange Tools in the Rack as needed and those changes will instantly be active.
After exiting the Tool Library Manager, the job file will continue with the new Tool Bin arrangements.
This is very convenient if a job requires more tools than can fit into the rack.

Consult the [ATC Setup Guide](ATCsetup.md) for a list of all the Features and how to enable them.



[Back](index.md)

