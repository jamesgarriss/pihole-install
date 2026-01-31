# pihole-install

This repo contains instructions for setting up a [PiHole](https://pi-hole.net) DNS server hosted on a Raspberry Pi and connecting to a home wireless network to block ads.

## Pre-Installation

* PiHole is a [DNS server](https://www.cloudflare.com/learning/dns/what-is-a-dns-server/), so your wireless router must be configured to use it.  Whether your wireless router is builtin to the modem provided by your ISP, or whether you have your own wireless router that you have connected to the modem, you must be able to change its configuration settings.
    * Following the instructions that were either provided by your ISP or that came with your wireless router and log into the administration console.  This might be done via an app on your phone or via a webpage that is hosted on the router.
    * Find the settings that allow you to specify your DNS server.  Write the existing IP address, the one specified by your ISP, down somewhere.  If things don't work, you'll want to restore this setting.  Here is what the [DNS settings](orbi.png) look like for my [Orbi wireless router](https://www.amazon.com/dp/B0D526GSYY).
    * Once your PiHole is setup, you will come back to this setting and change the IP address to that of the PiHole.
    * If your wireless router does not allow you to change the DNS server, you cannot use a PiHole.
* Purchase a Raspberry Pi.  Things to consider when selected a Raspberry Pi:
    * The [PiHole website](https://docs.pi-hole.net/main/prerequisites/) defines the requirements; whatever you buy must satisfy them.
    * Buying a kit is convenient, as it should include everything you need.
    * Buying a kit where Raspberry Pi OS has already been installed on the SD card or the SSD makes things a lot easier. 
    * I purchased [this one from CanaKit](https://www.amazon.com/dp/B0DKQS5CXB), and it's both overkill and expensive.  Given that my current PiHole has been running for 8 years, I think buying a "future proof" Raspberry is a smart investment.

## Installation

* Connect peripherals to the Raspberry Pi.
    * Connect a monitor to the HDMI port.  My Raspberry Pi has 2 HDMI mini ports, and the kit included an HDMI to HDMI mini cable.
    * Connect a mouse and keyboard to the USB ports.  My Raspberry Pi has 4 USB-A ports, so I connected an older mouse and keyboard with USB-A cables.
* Plug in the power supply to the DC power port, which on my Raspberry is also the USB-C port. The Raspberry Pi does not have an on/off button; once you connect power, it will turn on.  A small green light on the front will indicate that it has power and is booting up.
* Your monitor should show the Raspberry Pi desktop and a welcome message.  Use your mouse to click "Next" and start setup.
    * Set your country, language, and timezone.
    * Create a username and password.  Store them in a secure location.
    * Select a WiFi network and enter the password.
    * Select Chromium as the default and check the option to uninstall the unused browser.
    * Allow the software update.



## Administration