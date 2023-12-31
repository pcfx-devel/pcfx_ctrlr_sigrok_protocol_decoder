# pcfx_ctrlr_sigrok_protocol_decoder
Protocol decoder for sigrok/pulseview-based logic state analyzers for decoding controller data

## Introduction

The PC-FX controller port is used primarily for reading the FX-PAD, but has generalized
capabilites of communicating 32-bit data streams, bot inbound and outbound. In addition
to joypad, a mouse and a (planned) multitap device were supported.

A summary of the protocol and coapabilities can be found here: [FX_Controllers](https://github.com/pcfx-devel/PC-FX_Info/blob/main/FX_Controllers/README.md)

## The Protocol Decoder

### How To Use

Once you have installed the protocol decoder (see instructions for PulseView or other sigrok-compatible
software), it should be selectable in the list of protocol decoders.

The protocol decoder adds several annotation rows, to be able to interpret the bitstream in several
different ways; each of them can be displayed or suppressed.  These rows include:
 - bit values (electrical or internal representation, as internal is the inverse of electrical)
 - byte values (4 x 8-bit bytes per scan)
 - word values (evaluated as a 32-bit word)
 - controller values (based on controller type, what buttons (or values) are represented ?

Some examples are shown below (using mock data):

### Joypad

(click to show full-size on separate tab)

![Joypad](img/PCFX_Joypad.JPG)

### Mouse

(click to show full-size on separate tab)

![Mouse](img/PCFX_Mouse.JPG)

## Multitap

(click to show full-size on separate tab)

![Multitap](img/PCFX_Multitap.JPG)

### What exactly is this 'sigrok' thing ?

The sigrok project aims at creating a portable, cross-platform, Free/Libre/Open-Source signal analysis software suite that supports various device types (e.g. logic analyzers, oscilloscopes, and many more).
(taken from [the project's home page](https://sigrok.org/wiki/Main_Page) )

The specification for protocol analyzers (for logic state analysis) is extensible and there are many
example protocols available.


### Why write a decoder ?

I had already done signal captures and understood the meanings of various state
transitions of the signals for PC-FX controllers; the usage of the various
individual bits had also been documented.

This was enough to be able to make the [PC-FX mouse adapter](https://github.com/pcfx-devel/PC-FX_Controller_Adapters)
and to be able to transfer data to and from the PC-FX via the [FX uploader](https://github.com/enthusi/pcfx_uploader)

However, it was quite tedious doing debugging of the bit values during the development
of those devices, and I wished I had a protocol decoder like this one at that time. For example,
since it's a serial protocol - but least-significant-bt first, and evaluated based on the inverted bit
values - it's tedious to manually calculate values.  Also, when evaluating the data in sequential
packets - such as in the fx-uploader protocol - this compounds the complexity.

There are also more projects I'd like to try on the PC-FX controller port, so this will come in handy for those.


## NOTES:

While this protocol decoder has been written according to sigrok standards, I have (so far) only
installed and tested on "DSView", used in conjunction with my DreamSource Labs' DSLogic U3Pro32
logic state analyzer, and not PulseView.  HOWEVER, I do intend to obtain a low-cost analyzer and
test with PulseView at some point in the near future.  (I have no reason to believe that it won't
work on the first attempt).

I have not yet submitted this into the sigrok project, as there are still a few framing error
conditions I'd like to identify and display, and I want to do testing on the actual PulseView
software first.

I also may wish to do a little more code cleanup before submission.  I'm not familiar with their
review process, so I have no idea how long it will take to be approved (or receive feedback).

