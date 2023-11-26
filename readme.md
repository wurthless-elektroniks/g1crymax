# G1 negative voltage generation circuit for CRT displays

This circuit is a general purpose mod for CRT displays, allowing negative voltages to be applied to the G1 pin to help
fine-tune the display's brightness and sharpness, especially when used with a tube that wasn't intended for use with
that specific design.

This, of course, is a fancy way of saying "here's my shitty amateurish take on a circuit that every CRT hobbyist has built already".

Circuit is analog so you only get the Gerbers.

## WARNING: CRTS ARE DANGEROUS

Obligatory CRT health and safety warnings:

* Do not poke the CRT's butthole or you will get shit on. Make sure to discharge the CRT before performing any sort of action on, in, or around its butthole. Otherwise, something to the tune of 25 to 35 kV will go through your body, causing injury and/or death. Remember that CRTs, as well as other capacitive devices inside your display, can hold painful and occasionally lethal high voltages long after the power has been turned off. **Don't poke the chassis when it's live, either!**
* Do not apply pressure to the glass of the CRT. If the CRT is punctured, it will explode, and glass will spray everywhere, causing serious injury to the eyes and genitals. If you value your eyes and dick, wear protective safety gear when transporting a CRT.
* Many CRT displays, particularly from the 1980s and earlier, do not isolate their chassis from the mains, presenting a serious fire and shock hazard. When in doubt, use an isolation transformer.
* Do not hold the CRT by the neck except if you are really drunk, Australian, and have nothing else you can use in a bar fight.
* Other things may happen as a result of using this mod that will cause smoke and kabooms. This means that...
* **THESE FILES ARE PROVIDED TO YOU WITH ABSOLUTELY NO WARRANTY OR SUPPORT, OR ANY GUARANTEE THAT IT WILL WORK AT ALL.** It's up to you to decide if it's safe or not to install. Use it at your own risk!

## Why?

Enterprising jänkermeisters may be aware that cathode ray tubes, once a venerable display device, are no longer being manufactured.
The ones that are often in operation in arcade monitors are burned to a crisp with the HUDs and mazes of the games of yesteryears.
Other times, a tube will die, and a replacement must be sourced from a donor display. Frequently, a tube must be used with an
arbitrary chassis, and although most tubes have standardized voltages (25 kV for 19" tubes, heater voltage usually 6.3 volts),
the G2 (SCREEN) and G3 (FOCUS) voltages can vary between tubes. The result will be a picture that is too bright (G2 voltage
too high) or out of focus (G3 voltage not exact). The usual way to hack around this is by adjusting the B+ voltage, but this
can cause the chassis to run too hot, or possibly cause cathode poisoning if the voltages dip too low.

Most displays, however, do not use the G1 pin. This is the control grid, and on almost all consumer-grade equipment,
it is tied to ground (i.e., 0 volts), allowing the electron guns to fire directly at the screen. If a negative voltage is placed on G1,
then it will limit how many electrons strike the screen. While this will make the screen darker, it also allows us to fine-tune
the SCREEN and FOCUS values.

It just so happens that many displays have large negative voltages present on their flyback transformer, but they can't be fed
directly to the G1 pin because it won't be stable (or even safe). So, it's time to fix that.

This is a follow-on project to [Hatsune Mike's infamous Nanao MS9 G1 mod](https://mikejmoffitt.com/pages/ms9-hax/#g1mod), which I
had to do to convert my Wells Gardiner WGM2775 to a MS9 chassis. The circuit it implements is roughly the same way the MS9 does it
(if Nanao had implemented the circuit): diode, current limiting resistor, capacitor, discharge resistor. However, a potential divider,
a variable resistor and a zener are added to the circuit for finer control over what kind of voltage is sent to the G1 pin.

## Schematic

![](schematic.png)

## Bill of Materials

There are two zips in the gerbs/ directory. One is "g1 crymax.zip", which contains space for the potentiometer and potential divider circuits.
The other is "g1 crymax lite.zip", which only implements the basic circuit. **The Gerbers are still a work in progress and these PCBs are not tested yet.**

The basic circuit consists of:

* R3: 1M ohm pullup resistor (SMD 2512)
* R4: 620 ohm current limiting resistor (SMD 2512)
* C1: Any cap works here (*as long as it's within tolerance of the voltage applied to it!*), but 250V 1uF is the standard (SMD 1812)
* D1: Plain ol' 1 amp general purpose diode (SMC package)

Optionally, you can add a potential divider across R1 and R2. **Be careful when selecting resistors and potentiometers! The circuit's only designed for half-watt resistors.** However, the G1 pin will only draw current on the order of microamps, so you can use lower wattage resistors if you're feeling lucky.

Components are beefy to add lots of tolerance and to make it easier to solder. Boards are designed with a minimum clearance of 1.25mm.

## Installation procedure

**BEFORE YOU TRY THIS MOD:** If your display has the usual geometry or color bleeding issues, make sure to recap the appropriate circuits and/or recalibrate convergence. This mod won't do anything to fix those issues, although it will improve picture quality.

1. Open your display and look at the neckboard. If the G1 pin isn't going to ground, then your display already feeds negative voltage to G1 and there's no point doing this mod.
2. Assemble the basic circuit (D1/R4/R3/C1) and connect GND to whichever pin on the flyback is ground.
3. **Being extremely careful not to cause kabooms or kill yourself**, connect VR_RETURN to pins on the flyback that aren't the heater feed or B+, and measure the resulting voltage between G1 and ground. When you see a decent negative voltage in range, note it.
4. If the voltage needs to be dropped, populate resistors on R1 and R2 to form a potential divider.
5. If you want to use a potentiometer, then connect VR_SEND and VR_RETURN as appropriate.
6. Cut the G1 lead on the neckboard and route your new G1 feed to it. Route other wires as necessary.
7. Power up monitor, then calibrate G1, SCREEN and FOCUS. Colors might not need recalibration, but do that if you want to.

If you don't have an oscilloscope (which accurately describes 90% of electronics hobbyists), you can probe for negative voltages with a capacitor and diode. Here's how to do it:

![](probe.jpg)

Further resources to check:
* The [HR Diemen database](https://www.hrdiemen.com/search/index) lists many CRT displays and their flybacks. You might be able to find some useful info here, but don't count on it because **the HR Diemen database is known to be horribly wrong**.
* The [Tubular database](https://tubular.atomized.org) contains a wealth of information on tubes and, more importantly, the safe operating G1 voltage limits. **When in doubt, do not go past -50VDC.**

## Example installations

See installations/ directory.

## License

Public domain

What, you think I could possibly claim copyright for a fucking resistor divider?

## Actual schematic

![](realschematic.png)
