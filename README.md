# info-display

Configuration for [Raspbian](https://www.raspbian.org/) GNU/Linux 8 ("Jessie")
running on Raspberry Pi Model B to display information during certain hours
of the day.

The Raspberry Pi Model B combined with Raspbian GNU/Linux
is an excellent base on which to build low cost, energy efficient,
display solutions for public service announcement, product information,
advertisement and similar purposes.

To get started, put
[a fresh image of Jessie](https://www.raspberrypi.org/downloads/raspbian/)
on an SD-card, insert that into your Raspberry Pi Model B and power on your Pi.

At the desktop, open a terminal and then resize the SD card
using the `raspi-config` tool.

```bash
sudo raspi-config
```

Select Expand Filesystem, tell it Ok, then Finish.
When asked whether or not to reboot, select yes.
The Raspberry Pi will then reboot.

Once the Pi has rebooted and you're back at the desktop,
open a terminal, update package index, install newest versions
of packages and then install the Debian fork of Mozilla Firefox
known as Iceweasel.

```bash
sudo apt-get update && \
sudo apt-get -y upgrade && \
sudo apt-get -y install iceweasel
```

Now from your main or managment computer, do `ssh-copy-id` to the Pi,
verify that you are able to log in over SSH using public key authentication,
then disable password login over SSH on the Pi.

Next, clone this repository in the home directory of the pi user.

```bash
git clone https://github.com/en90/info-display.git
```

Copy config files.

```bash
cp -r info-display/.mozilla .
cp info-display/.config/lxsession/LXDE-pi/autostart .config/lxsession/LXDE-pi/
```

Edit the newly copied file `~/.config/lxsession/LXDE-pi/autostart`
to display the website *you* want to show instead of mine, e.g.

```bash
vi ~/.config/lxsession/LXDE-pi/autostart
```

Install crontab, edit the crontab according to your needs and then reboot.

```bash
tail -n4 info-display/var/spool/cron/crontabs/pi | crontab -
crontab -e
sudo reboot
```

You will now find that when the Raspberry Pi reboots, Iceweasel starts
with your selected website in fullscreen and no other GUI software is started.

All that remains is to adjust the font-size so it fits nicely to the display,
then disconnect your keyboard and mouse and enjoy the info display.

Note that any time the Pi boots, the display will be left on
until it is turned off by the crontab or by you. This is on purpose
because whenever we power it on, we want to see that it is still working.

To turn off the display manually, SSH into the Pi as the pi user
and issue the following command:

```bash
sudo tvservice -o && pkill iceweasel
```

PS: It is assumed that your monitor will automatically go to sleep
whenever the HDMI signal from the Pi is turned off, and likewise,
that it will wake up once the HDMI signal from the Pi is turned on.
Verifying that such is the case is your first order of business.
If it does not, return the monitor and buy another which does.

PPS: Don't depend on an internet connection. Set up a local web server,
either on the Pi itself or elsewhere on the local network. Use Nginx.
