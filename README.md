# Pi-hole Installation

This repo contains instructions for setting up a [Pi-hole](https://pi-hole.net) DNS server hosted on a Raspberry Pi and connecting to a home wireless network to block ads.

## Pre-Installation

Pi-hole is a [DNS server](https://www.cloudflare.com/learning/dns/what-is-a-dns-server/), so your wireless router must be configured to use it.  Whether your wireless router is builtin to the modem provided by your ISP, or whether you have your own wireless router that you have connected to the modem, you must be able to change its configuration settings.

* Find the instructions that were either provided by your ISP or came with your wireless router.  Log into the administration console.  This might be done via an app on your phone or via a webpage that is hosted on the router.
* Find the settings that allow you to specify your DNS server.  Write the existing IP address, the one specified by your ISP, down somewhere.  If things don't work, you'll want to restore this setting.  (To be precise, there are many DNS servers on the Internet, and you don't have to use the one provided by your ISP.)  Here is what the [DNS settings](dnsaddress.png) look like for my [Orbi wireless router](https://www.amazon.com/dp/B0D526GSYY).
* Don't change anything yet.  Once your Pi-hole is setup, you will come back to this setting and change the IP address to that of the Pi-hole.
* If your wireless router does not allow you to change the DNS server, you cannot use a Pi-hole.

Pi-hole requires a fixed IP address.  Whether you own your own modem or are renting one from your ISP, you must be able to change its configuration settings.

* Find the instructions that were either provided by your ISP for your modem or came with your modem.  Log into the administration console.  This might be done via an app on your phone or via a webpage that is hosted on the modem.
* Find the settings that allow you to allocate a fixed IP address.  Here is what the [IP allocation settings](ipallocation.png) look like for my AT&T modem.
* Don't change anything yet.  Once your Pi-hole is setup, you will come back here and allocate a fixed IP for the Raspberry Pi.

Purchase a Raspberry Pi.  Things to consider when selecting a Raspberry Pi:

* The [Pi-hole website](https://docs.pi-hole.net/main/prerequisites/) defines the minimum requirements.  Whatever you buy must satisfy them; I recommend purchasing a Pi-hole that exceeds the minimum.
* Buying a kit is convenient, as it should include everything you need, like a case, a power supply, and an SD card or SSD.
* Buying a kit where Raspberry Pi OS has already been installed on an SD card or  SSD is very convenient.  This guide assumes this is what you have. 
* I purchased [this one from CanaKit](https://www.amazon.com/dp/B0DKQS5CXB), and it's both overkill and expensive.  Given that my current Pi-hole has been running for 8 years, I think buying a "future proof" Raspberry is a smart investment.

Things you will need just for setup:
* HDMI monitor
* USB-A mouse
* USB-A mouse

One other thing you will need:
* Ethernet cable

## Installation

The initial configuration of the Raspberry Pi requires a monitor, keyboard, and mouse.  Once Pi-hole is running, they will no longer be necessary, since you can remote connect to the Raspberry Pi via ssh and via a dashboard.  More on that later.

* Connect peripherals to the Raspberry Pi.  
    * Connect a monitor to the HDMI port.  My Raspberry Pi has 2 HDMI mini ports, and the kit included an HDMI to HDMI mini cable.
    * Connect a mouse and keyboard to the USB ports.  My Raspberry Pi has 4 USB-A ports, so I connected an older mouse and keyboard with USB-A cables.

* Plug in the power supply to the DC power port, which on my Raspberry is also the USB-C port. The Raspberry Pi does not have an on/off button; once you connect power, it will turn on.  A small green light on the front will indicate that it has power and is booting up.

* Your monitor should show the Raspberry Pi desktop and a welcome message.  (Change to the correct input source on your monitor if necessary.)  Use your mouse to click "Next" and start setup.
    * Set your country, language, and timezone.
    * Create a username and password.  This is for an account on the Raspberry Pi OS.  Store them in a secure location.  (A different account will be created later for Pi-hole.)
    * Select a WiFi network and enter the password.
    * Select Chromium as the default and check the option to uninstall the unused browser.
    * Allow the software update.
    * Once it's up to date, restart.

* Once it restarts, you should see the Raspberry Pi OS desktop; it is similar to a MacOS or Windows.  You need to enable ssh.
    * Start the terminal.  It's a black icon in the upper left corner.  Alternatively, try CTRL + ALT + T.
    * Make sure that the OS is completely updated by running this command in the terminal.  If you are not aware, prefacing a command with "sudo" means "run this command as the administrator.  When I ran this, it did not need any updates.
        ```
        sudo apt update && sudo apt upgrade -y
        ```
    * Start the ssh service:
        ```
        sudo systemctl enable --now ssh
        ```
    * Verify that it is "active (running)".  After you verify, you may need to CTRL-C to return to the prompt.
        ```
        systemctl status ssh
        ```

* Connect your Raspberry Pi to your modem with an ethernet cable.

* Install Pi-hole with this command:
    ```
    curl -sSL https://install.pi-hole.net | bash
    ```
    * You should see a window with the Pi-hole Automated Installer.  If it's all black, use your mouse to slightly enlarge the window.
    * Choose eth0 as the interface.
    * Choose "Quad9 (filtered, DNSSEC)" for the Upstream DNS provider.
    * Select "Yes" to include StevenBlack's Unified Hosts List.
    * Select "Yes" to enable query logging.
    * Select "Show everything" for the privacy mode.
    * Pi-hole should install itself.  Write down the information on the Installation Complete screen, namely the:
        * IPv4
        * Web interface URLs
        * Login password
    * This password is the password for Pi-hole; it is not the same as the password for the Raspberry Pi OS (but more on that in a second).

* Verify Pi-hole is running via the terminal.  You should see a lot of green checks.
    ```
    sudo pihole status
    ```

* The password created by the installer is hard to remember.  If you want to change it (perhaps to the same password as that for the Raspberry Pi OS, just to make life easy), run this command on the terminal (and save the new password):
    ```
    sudo pihole setpassword
    ```

* ssh to the Raspberry Pi from your computer's terminal using the username and password you set previously and IPv4 address noted above:
    * Use this command, where "ipaddress" is the value of IPv4 above:
        ```
        ssh username@ipaddress
        ```
    * When questioned about the key, select yes.
    * Enter the password.  
    * If this works, it allows you to make changes to your Raspberry Pi and the Pi-hole software without having to connect it to a monitor, keyboard, and mouse.  This is convenient.

* On the Raspberry Pi, verify Pi-hole is working.  Start the web browser (the world icon in uppper left) and go to http://ipaddress/admin, where "ipaddress" is the value of IPv4 above.  Login with the Pi-hole password.  You should see the Pi-hole dashboard.
    * Repeat this from a different computer on your network.  You should get the same dashboard.  This allows you to check on your Pi-hole and make updates without having to connect it to a monitor, keyboard, and mouse.  This is also convenient.

* Login to the modem control panel and allocate the IP of the Raspberry Pi so that it always has a fixed IP address.  The modem may need to restart.
  
* Login to the router control panel and set its DNS to the IP of the Raspberry Pi.  This is what makes the adblocker work.  The wireless router may need to restart.
    * To test this, go to a web page that normally serves a lot of annoying ads.  The ads should be replaced with white space.
    * Note: Pi-hole cannot block all ads.  YouTube ads, for example, are not blocked.

* If you ever need to turn off your Raspberry Pi, ssh to it and run this command and when it finishes, unplug the power cable.
    * Shutdown the computer:
        ```
        sudo shutdown now
        ```
    * To start it up again, simply plug in the power cable.

* Now that everything is running, remove the mouse, keyboard, and HDMI cables from the Raspberry Pi.  It should be able to work months at a time without changes.

## Administration

