# Rack-ATC Module Setup Instructions

![](/images/pa133.png)

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

