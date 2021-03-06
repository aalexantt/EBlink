# EBlink
ARM Cortex-M GDB server with device squirrel scripting  support

EBlink features:
- GDB server with flash caching, with EmBitz live variables/expression support!
- Stand alone flashing tool
- Device support by squirrel scripting, flash algorithm, device reset strategy etc
- Multiple interfaces (right now STlink)

Usage: eblink \<options\>

    -h,           --help            Print this help
    -g,           --nogui           No GUI message boxes
    -v,           --verbose         Specify generally verbose logging
    -D <script>,  --device <script> Select the device script <>.script
    -T <options>, --target          Select target(optional)
    -I <options>, --interf          Select interface
    -F <options>, --flash <options> Target image flashing
    -G [options], --gdb <options>   Launch GDB server

  --device and --interf are mandatory, normally combined with --flash or/and --gdb


==== Interfaces


name: CMSIS-DAP - ARM debug protocol driver 
     Usage -I cmsis-dap[,options]

        <not yet implemented>


name: ST-link - STmicro V2 interface driver 
     Usage -I ST-link[,options]

        rconnect     : Connect under reset
        speed=nnn    : Interface speed
        swd          : use SWD (default)
        jtag         : use Jtag
        device=<usb_bus>:<usb_addr> : Select probe explicit

        e.g.  -I ST-link,rconnect,speed=3000

==== Flash loader
     Usage -F [options]

        erase        : Chip erase the flash
        verify       : Verify flash after upload
        run          : Start image after upload
        file=<file>  : Load the file, <file>.hex  = Intel HEX format
                                      <file>.srec = Motorola srec file format

                                      Default     = ELF file format

        e.g. -F file=test.elf
             -F verify,run,file=test.srec
             -F erase
             -F erase,run,file=test.hex

        Default (without erase) only modified sectors are (re)flashed.



==== GDB server
     Usage -G [options]

        S            : Shutdown after disconnect
        nc           : Don't use EB-link flash cache
        port=ppp     : Select different TCP port, default 4242
        address=x.x.x.x : Select different listen address, default 0.0.0.0

        e.g.  -G S,nc

Examples:

        Connect STmicro device with STlink on GDB server
        => eblink -I st-link -D stm32-auto -G

        Connect silabs device with STlink on bus 2:4 and launch the GDB server on port 4243:
        => eblink -I st-link,device=2:4 -D silabs-auto -G port=4243

        Flash STmicro device with STlink and run
        => eblink -I st-link -D stm32-auto -F verify,run,file=release.elf,        