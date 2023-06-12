# Repair Jun 11, 2023

### Introduction

My NABU retro computer was always flaky, but in recent weeks it decided to just stop working altogether.  The system nRESET always seemed to be inconsistent, and after measuring anywhere from 1-ohm to 12-ohms between U31-p8 and U56-p9, and a voltage of ~1.2V on u56-p9 when the output of U31-p8 was low (should not be possible with just copper traces or wire in between), it was time to pull the motherboard and see what was going on.

![NABU reset circuit](/images/reset_circuit.jpg "NABU reset circuit")

Initially I planned on documenting each problem, complete with photos, but after seeing the number of problems I got lazy and just performed the repairs.


### Power connector

The power wires from the power-supply to the motherboard are soldered to the PCB.  Upon removing the power-supply cover I discovered there is a connector to the power-supply PCB.  While inconvenient, at least the power harness can be disconnected from the power-supply without desoldering it from the motherboard.

![Motherboard power wires](/images/power_wires.jpg "Motherboard Power Wires")
![Power supply connector](/images/power_connector.jpg "Power Supply Connector")

### Power supply fan

I did not know the NABU had a power-supply fan because mine was never working.  It is a nice 120VAC box-fan and all metal construction.  The problem was the fan-hub screws had come slightly loose and allowed the blade housing to become off-center and it was colliding with the shroud.

Keeping the hub centered while retightening the screws proved successful to restore the fan to working condition.  This was very surprising to me since, up until now, the fan would have been stuck while the system was operating, and I'm not sure why it did not burn out a winding or something.

![Power supply fan](/images/fan.jpg "Power Supply Fan")

### LED display PCB

Removing the LED "display" PCB from the front of the unit was problematic.  The LED PCB is not connectorized to the motherboard.  A grey flex ribbon cable is used, the type of which the soldered connections will break easily if flexed more than a few times.

It is nearly impossible to remove the LED PCB and/or motherboard without flexing this cable in the wrong way, and ultimately many of the connections broke.  I will replace this cable with a connector and better wire.  Luckily the LED PCB is not required for normal system operation.

![LED PCB](/images/led_pcb.jpg "LED PCB")

### Motherboard inspection

Once the motherboard was removed I put it under the microscope for inspection (I use a stereo microscope for all my soldering and rework).  I was surprised at how much hand assembly seemed to have been done on this board, and in almost every case where hand soldering was performed, the solder never flowed through the holes to the top pad.  This could be due to not enough flux, trying to work too fast, etc..  Regardless, the results were very obvious when the board was turned over: cold solder joints.

![Cold solder joints](/images/cold_solder.jpg "Cold Solder Joints")

The motherboard also has a lot of jammed-in components, components interfering with other components, components installed on top of other components, components pushing and bending other components out of the way, etc..  It almost feels like this was the designer's first big PCB or something, which I find surprising given the engineers NABU Corp is reported to have lured from places like DEC.

![Bad stuff](/images/badstuff.jpg "Bad Stuff")

Something else that always perplexed me about this system is, the TMS9918A VDP does not have a heat sink.  Probably without exception, every system that uses the 9918A family of VDP has (and usually needs for stable operation) a heat sink.  I really do not know how the NABU gets away without a heat sink on the VDP, but if I were not using the F18A MK1 in my NABU, I would have a heat sink on the 9918A.


### Bodge wires

On the bottom of the motherboard there were many rework (bodge) wires.  While a common practice of the era, and among home computers (and probably other consumer electronics), in every case where a wire was soldered into a via, there was a cold solder joint.  The locations where the wires were soldered to IC or other component pins seemed fine.

![Bodge wires](/images/bodge.jpg "Bodge Wires")

There are about 15 to 20 bodge wire corrections on the bottom, and two questions immediately came to mind:

  1. Are the changes different between NABU systems?
  2. Do the schematics for the NABU floating around represent these changes?

I am pretty confident I can say "yes" to both of these questions for a few reasons.  First, the schematics reflect the physical connections I found on the motherboard, so I suspect the changes are the same for all NABU computers.

Second, every place there is a bodge wire the PCB traces were "notched" to create a break in the original connection, however the breaks are *under* the solder-mask, i.e. these were not cut after PCB manufacturing.  This means the problems were known and the PCB updated in the cheapest way possible (by removing a small section of the original PCB trace).

![Trace notches](/images/notches.jpg "Trace Notches")

I find it very surprising that it was apparently cheaper to have someone do manual soldering for each system than to pay someone to update the PCB routing!?

I resoldered every bodge wire that used a via hole, as well as any bodge connections that were questionable.


### Fixing nRESET

The wiring between the two reset-circuit inverters (U31 and U56 described above) is entirely done with bodge wires, and they travel from one side of the board to the other, with a connection in the middle using a via hole.  Both wires were cold-soldered at this junction, and one of them I could have removed with a little wiggling and tugging.  This certainly explains the inconsistent resistance measurements I was seeing between the inverter pins, and could cause the system to be very unstable.

![Reset cold solder](/images/reset_cold.jpg "Reset Cold Solder Wires")

### Solder bridge hack

Under the CPU is a via that carries the nWR signal from the CPU to and OR-gate buffer (U57) for system distribution.  This signal is very critical to normal system operation, and at some point in the past someone made a mistake with a cutting tool and cut the trace just as it was leaving the via-pad.  Their "fix" was to make a large enough solder-blog to bridge the gap they had created.

![Solder bridge](/images/bridge.jpg "Solder Bridge")

The problem with this kind of hack is, the solder bridging the gap has no support of a pad and is very susceptible to cracking.  This could easily happen, for example, by removing and reseating the CPU from the socket that is directly above this via.  Such a crack would certainly be intermittent and *very* hard to find.  I added a new wire between the two vias to prevent any current or future problems with this connection.


### Summary

My NABU is working again, and powered up right away after reassembly.  No more intermittent power-on failures, and I even have a working power-supply fan that also provides some airflow for the motherboard.

Despite having a lot of interesting ideas (especially for its time), this system is full of errors and poor execution.  I suspect many hobbyists are going to be experiencing failures and flaky behavior as these systems age and get used more and more.  While the fixes are generally not too difficult, finding a problem can be a real challenge and the poor motherboard quality makes rework a hazard (it would be very easy to lift a pad or trace, etc.).
