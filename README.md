# What's new in macOS Catalina

* [What has changed on the surface](README.md#)
   * Sidecar
   * Death of itunes
   * iCloud Drive folder sharing
   * Screen Time
   * Apple Watch Autentication
* [What's new under the hood](README.md#)
   * All system files were moved to a read-only partition
   * 32bit apps support
   * Modification of the AirportBCRM4331.kext
   * Removal of MacPro4,1 and 5,1
   * Restore macOS from snapshot
   * Kexts moved out of kernel space
   * Support for Catalist based apps
* [Current issues with Catalina](README.md#)
* [Should you update and how to proceed](README.md#)
* [What's new with the subreddit?](README.md#)


# What has changed on the surface

**Sidecar**

This is probably the feature users are most excited for, what Sidecar allows us to do now is run a supported iPad running iPadOS 13 to be used as a secondary display to either your mac or hackintosh with minimal latency compared to [duet](http://www.duetdisplay.com/?gclid=CjwKCAjwx_boBRA9EiwA4kIELkwxgPYZMk-z68nN1hh0wWuJC2nkk7SREpKdYkMyTEhFskFcwZRscxoCofUQAvD_BwE) and [Astropad](https://astropad.com) with full Apple Pencil Passthrough. 

> But what's the catch?

Well the catch with this is that there are a few requirements to run Sidecar officially:

* Skylake or newer CPU which supports h.265/HEVC encoding
* An iPad with a A8X CPU
* A compatible Wireless card or Lighting cable(Sidecar can operate both wirelessly and wired)
* A SMBIOS supporting Sidecar

While it is possible to get around the SMBIOS limitation, it can be quite unstable for some users so we advise against modify the Sidecar framework unless you absoultely know what you're doing

Bypass the SMBIOS retriction:

`defaults write http://com.apple .sidecar.display allowAllDevices -bool YES`

Unlocking the System Preferance Pane:

`defaults write com.apple.sidecar.display hasShownPref -bool true`

[Source](https://twitter.com/stroughtonsmith/status/1136413491462594560)

SMBIOS which offically support Sidecar:

MacBook:

* MacBook8,1
* MacBook9,1
* MacBook10,1

MacBook Air:

* MacBookAir8,1

MacBook Pro:

* MacBookPro13,1
* MacBookPro13,2
* MacBookPro13,3
* MacBookPro14,1
* MacBookPro14,2
* MacBookPro14,3
* MacBookPro15,1
* MacBookPro15,2

Mac Mini:

* MacMini8,1

iMac:

* iMac17,1
* iMac18,1
* iMac18,2
* iMac18,3
* iMac19,1
* iMac19,2

iMac Pro:

* iMacpro1,1

Mac Pro:

* MacPro7,1

**Death of itunes**

While for some reason this is what all the headlines want to talk about, the idea of iTunes is not dead. Instead it's more of a rebirth and clening of iTunes by seperating it into 4 apps:

* Music
* Podcast
* AppleTV 
* Finder

And the only real ground breaking change from this is that your iPhone will now be synced through Finder instead of iTunes

**iCloud Drive folder sharing**

This one is my personal favorite as this will very useful for sharing files when troubleshooting hackintoshes on the subreddit. But unfortunaltely were pretty stuborn in our ways so we'll probably still be dealing with sketchy websites when downloading EFIs, but a Slav can dream

**Screen Time**

Have a serious problem trying to do work on your mac/hack without getting distracted? Well now you can finally curb a bit of that addiction and be a bit more productive as long as you have the willpower not to disable Screentime. We'll see how useful/annoying this becomes

**Apple Watch Autentication**

Pretty straight forward, macOS will allow you to replace your password with your Apple Watch in more places. Do keep in mind this requires a supported wireless card

# What's new under the hood

![symlinkMeme](symlinkMeme.jpg)
**All system files were moved to a read-only partition**

Probably the biggest change to Catalina is the seperation of user files from the systems. Where this becomes a real problem is users upgrading to Catalina from an existing partition as files can become damadged/corrupted while the conversion happens.

To avoid this, make a new APFS volume just for Catalina and install fresh onto there. From there run migration assistant off your old partition and then delete it

**32bit apps support**

Another potential issue is that 32bit apps no longer work, the people who are most likely to be affected by this are laptop users who use USB wireless dongles that require old 32bit apps. And these apps are likely not to be updated

**Modification of the AirportBCRM4331.kext**

Quite an odd chnage is how the AirportBCRM4331 kext was merged into the AirPortBrcm4360 kext. While no hardware was dropped an odd side-effect came that AirPortBrcm4360 wouldn't load even when BCRM4331 based hardware was installed. 

Solution to this is to force-load the AirPortBrcm4360 kext

**Removal of MacPro4,1 and 5,1**

**Restore macOS from snapshot**

**Kexts moved out of kernel space**

**Support for Catalist based apps**




# Current issues with Catalina



# Should you update and how to proceed


# What's new with the subreddit?

This is more of a mini update from us, things that have changed:
* Updated sidebar with a new Catalina GPU Buyers Guide
* New Wireless Buyers Guide
* Updated Logos, banners and flairs

