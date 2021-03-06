# freemodebus-source
Christian Walter modebus implementation

## Tips

This part provides some tips for using the FreeModbus protocol stack.

### section sec_tips_memory Reducing memory requirements
The memory requirements of FreeModbus can be tuned in the following way. These are basic tricks and can easily be done:

 - Decided if you need RTU, ASCII and TCP at the same time. If not disable
   them in the file mbconfig.h by settings the respective options
   `MB_RTU_ENABLED`, `MB_ASCII_ENABLED` and `MB_TCP_ENABLED` to zero.
 - If you don't need all Modbus functions disable them in the file mbconfig.h.
   This will reduce code requirements.
 - Set the variable `MB_FUNC_HANDLERS_MAX` in mbconfig.h to the number
   of functions codes you want to support.

If you have stronger limits you can also try the following options. Note that this options have an impact on the features of the protocol stack.

 - Use some compiler directive to put the mapping of function codes to
   handler functions into the flash memory of you CPU. You can find this
   table in the file mb.c at the top of the file. The static variable is
   named xFuncHandlers.

 - Reduce the size of the RTU buffer. In this case longer frames will
   result in an error (Your device will drop all these frames). This is
   possible if you will never get read/write requests with that number
   of registers or your total amount of registers is small anyway.

 - You could also remove some function pointers which make the protocol
   stack configurable and replace them by the functions itself. For
   example if you only want to use RTU remove the callback functions from
   the porting layer and fill in the appropriate calls. This will save
   the space for all function pointers.