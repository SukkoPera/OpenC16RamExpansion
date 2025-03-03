
# OpenC16RamExpansion
OpenC16RamExpansion is an Open Hardware 64 Kb RAM Expansion board for the Commodore 16.

![Board](https://raw.githubusercontent.com/SukkoPera/OpenC16RamExpansion/master/img/render-top.png)

## Summary
The board fits the cartridge/expansion port and, in theory, it should instantly give a C16 64 kilobytes of RAM. Unfortunately, the reality is quite different since the C16 was deliberately designed not to be easily expandable, so that the larger RAM would be a selling point of the Plus/4.

This must have been a long-debated feature, since half of the job is done while the other half is missing. In particular, the /RAS, /CAS and MUX signals that are needed to control RAM chips have been brought to the cartridge slot and these cannot actually serve many other purposes. On the other hand, there is no way to disable the internal RAM when an expansion is plugged in the cartridge slot which means that both RAMs will fight for the bus, amounting to a disasterous effect. 

## So What!?
Old expansions used to come with instructions saying how to disable the internal RAM, which was usually obtained by barbarically cutting the power pins altogether. This had the obvious side effect that at that point the machine could ONLY work with the external expansion plugged in, preventing the use of [other cartridges](https://github.com/SukkoPera/OpenC16Cart) (not that many were made, but what about a [SID Card](https://github.com/SukkoPera/ReSeed) or a [User Port Card](https://github.com/SukkoPera/16up)?).

Other boards had output transistors that would somehow "overpower" the outputs of the internal RAM or - worse - force the internal /RAS signal to a high level.

Since both these things are quite likely to damage the - already precarious - internals of your C16, I didn't even think about replicating them. Instead, I have come up with a not-too-hard modification implementing the missing half of the job that Commodore left to be desired. It can be carried out on all C16s in order to let them work with both the internal or external RAM when plugged in. **If you want to use this Expansion, you will have to perform the following modification**:

1. Connect pin Z of the expansion slot to pin 11 of U12 with a wire.
2. Disconnect pin 1 of U5 and U6 from the mainboard.
3. Connect pin 13 of U12 to pin 1 of U5 and U6.
4. Blob together pins 12 and 13 of U12.
5. Solder a 10k resistor between that blob and pin 14 of U12.

This will disable the internal RAM whenever pin Z on the expansion connector is grounded. This is already done on OpenC16RamExpansion but it should be easy to modify any old cart this way.

Now, if you start up your C16 without any expansion plugged in, it should work as usual and report 12277 bytes free. If you start it up with an expansion, you should get 60671. Actually, this is not yet enough to say that the extra RAM is working properly, since you would get the same even without performing any modification. To make sure it is, enter the following BASIC instructions:

POKE 100, 42
PRINT PEEK 16484

If you get **anything else than 42**, then the expansion is working correctly. If you get 42, it is not (that would mean the "second" 16kB of RAM are a replica of the first 16kB).

If you are using a [LittleSixteen V3 board](https://github.com/SukkoPera/LittleSixteen), there is no need to perform any modification, as it is already built-in.


## Releases
If you want to get this board produced, you are recommended to get [the latest release](https://github.com/SukkoPera/OpenC16RamExpansion/releases) rather than the current git version, as the latter might be under development and is not guaranteed to be working.

Every release is accompanied by its Bill Of Materials (BOM) file and any relevant notes about it, which you are recommended to read carefully.

## License
The OpenC16RamExpansion documentation, including the design itself, is copyright &copy; SukkoPera 2019-2021 and is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).

This documentation is distributed *as is* and WITHOUT ANY EXPRESS OR IMPLIED WARRANTIES whatsoever with respect to its functionality, operability or use, including, without limitation, any implied warranties OF MERCHANTABILITY, SATISFACTORY QUALITY, FITNESS FOR A PARTICULAR PURPOSE or infringement. We expressly disclaim any liability whatsoever for any direct, indirect, consequential, incidental or special damages, including, without limitation, lost revenues, lost profits, losses resulting from business interruption or loss of data, regardless of the form of action or legal theory under which the liability may be asserted, even if advised of the possibility or likelihood of such damages.

## Support the Project
If you want to get some boards manufactured, you can get them from PCBWay through this link:

[![PCB from PCBWay](https://www.pcbway.com/project/img/images/frompcbway.png)](https://www.pcbway.com/project/shareproject/OpenC16RamExpansion_V2.html)

You get my gratitude and cheap, professionally-made and good quality PCBs, I get some credit that will help with this and [other projects](https://www.pcbway.com/project/member/shareproject/?bmbid=41100). You won't even have to worry about the various PCB options, it's all pre-configured for you!

Also, if you still have to register, [you can use this link](https://www.pcbway.com/setinvite.aspx?inviteid=41100) to get some bonus initial credit (and yield me some more).

You can also buy me a coffee if you want:

<a href='https://ko-fi.com/L3L0U18L' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

## Credits
This board was laid out only by looking at [various C16/Plus/4 schematics](http://www.zimmers.net/anonftp/pub/cbm/schematics/computers/plus4/index.html) and pictures (mostly low-resolution) of [similar expansions from back in the day](http://plus4world.powweb.com/hardware) I found on the Net, I have never seen one in real life:
* [Rex Speichererweiterung](https://plus4world.powweb.com/hardware/Rex_Speichererweiterung)
* [Jurek's 64K Memory Expansion](https://plus4world.powweb.com/hardware/Jureks_64K_Memory_Expansion)
* [Dela 32K](https://plus4world.powweb.com/hardware/Dela_32K)

So thanks to the makers of those expansions and to Zimmers and Plus/4 World for cataloguing them.
