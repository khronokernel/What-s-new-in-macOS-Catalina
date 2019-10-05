# What's new in macOS Catalina

* [What has changed on the surface](README.md#)
   * Sidecar
   * Death of iTunes
   * iCloud Drive folder sharing
   * Screen Time
   * Apple Watch Authentication
* [What's new under the hood](README.md#)
   * All system files were moved to a read-only partition
   * 32bit apps support
   * Modification of the AirportBCRM4331.kext
   * Reinjection of AirportAtheros40 kext
   * Removal of MacPro4,1 and 5,1
   * Restore macOS from a snapshot
   * Kexts moved out of kernel space
   * Support for Catalyst based apps
* [Current issues with Catalina](README.md#current-issues-with-Catalina)
   * Lilu loading
   * AppleACPICPU stalling
   * Can't write to Library/Extensions to add my kexts
   * Requiring EC device
* [Should you update and how to proceed](README.md#)
* [What's new in the hackintosh scene?](README.md#)
   * A new ~~foe~~ Boot Loader has appeared!
   * AptioMemoryFix is dead, long live FwRuntimeServices!
   * Clover folder structre changes
   * Where's the Navi support?
   * Chrome being dumb again
   * Stolen work of hard working hackinshers
* [What's new with the subreddit?](README.md#)
   * Catalina GPU Buyers Guide
   * Wireless Buyers Guide
   * Updated Logos, banners and flairs


# So where are the Nvidia Drivers?

There are none, stop asking. There will be no support for non-Kepler GPUs in Catalina, let it go man. If you need a new GPU, please refer to the new [GPU Buyers Guide](https://khronokernel-3.gitbook.io/catalina-gpu-buyers-guide/) and the [Disable your unsupported GPU Guide](https://khronokernel-4.gitbook.io/disable-unsupported-gpus/)


# What has changed on the surface

**Sidecar**

This is probably the feature users are most excited for, what Sidecar allows us to do now is run a supported iPad running iPadOS 13 to be used as a secondary display to either your Mac or Hackintosh with minimal latency compared to [duet](http://www.duetdisplay.com/?gclid=CjwKCAjwx_boBRA9EiwA4kIELkwxgPYZMk-z68nN1hh0wWuJC2nkk7SREpKdYkMyTEhFskFcwZRscxoCofUQAvD_BwE) and [Astropad](https://astropad.com) with full Apple Pencil Passthrough. 

> But what's the catch?

Well, the catch with this is that there are a few requirements to run Sidecar officially:

* A GPU supporting H.265/HEVC encoding, Skylake+ iGPUs or Polaris/Vega dGPUs
* An iPad with an A8X CPU
* A [supported wireless card(mainly bluetooth matters)](https://khronokernel-7.gitbook.io/wireless-buyers-guide/) or Lighting cable(Sidecar can operate both wirelessly and wired)
* A SMBIOS supporting Sidecar
* Both machines on the same Apple ID

While it is possible to get around the SMBIOS limitation, it can be quite unstable for some users so we advise against modify the Sidecar framework unless you know what you're doing. And users have reported in laer betas that these hacks no longer work.

Bypass the SMBIOS restriction:
```
defaults write http://com.apple .sidecar.display allowAllDevices -bool YES
```
Unlocking the System Preferance Pane:
```
defaults write com.apple.sidecar.display hasShownPref -bool true
```
[Source](https://twitter.com/stroughtonsmith/status/1136413491462594560)

SMBIOS which officially support Sidecar:

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

* iMacPro1,1

Mac Pro:

* MacPro7,1

**Death of itunes**

While for some reason this is what all the headlines want to talk about, the idea of iTunes is not dead. Instead, it's more of rebirth and cleaning of iTunes by separating it into 4 apps:

* Music
* Podcast
* AppleTV 
* Finder

And the only real groundbreaking change from this is that your iPhone will now be synced through Finder instead of iTunes.

**iCloud Drive folder sharing**

This one is my personal favourite as this will very useful for sharing files when troubleshooting Hackintoshes on the subreddit. But unfortunately were pretty stubborn in our ways so we'll probably still be dealing with sketchy websites when downloading EFIs, but a slav can dream.

**Screen Time**

Have a serious problem trying to do work on your mac/hack without getting distracted? Well, now you can finally curb a bit of that addiction and be a bit more productive as long as you have the willpower not to disable Screentime. We'll see how useful/annoying this becomes

**Apple Watch Authentication**

Pretty straight forward, macOS will allow you to replace your password with your Apple Watch in more places. Do keep in mind this requires a [supported wireless card](https://khronokernel-7.gitbook.io/wireless-buyers-guide/).

# What's new under the hood

<img src="symlinkMeme.jpg" width="100" height="100">
**All system files were moved to a read-only partition**

Probably the biggest change to Catalina is the separation of user files from the systems. Where this becomes a real problem is users upgrading to Catalina from an existing partition as files can become damaged/corrupted while the conversion happens.

To avoid this, make a new APFS volume just for Catalina and install fresh onto there. From there run migration assistant off your old partition and then delete it.

**32bit apps support**

Another potential issue is that 32bit apps no longer work, the people who are most likely to be affected by this are laptop users who use USB wireless dongles that require old 32bit apps. And these apps are likely not to be updated.

But the upside to this AMD users will have uch better AMD support for many apps, problem is that Apple that still call upon Intel specific functions still break. So depending on how Adobe handles this, Lightroom and others may finally be fixed(or we realize that there's more code tied to Intel than we expected).

**Modification of the AirportBCRM4331.kext**

Quite an odd change is how the AirportBCRM4331 kext was merged into the AirPortBrcm4360 kext. While no hardware was dropped an odd side-effect came that AirPortBrcm4360 wouldn't load even when BCRM4331 based hardware was installed. From my tests, an authentic Apple Airport BCM94331 would load without issues but third party models seem to be a hit or miss.

A solution to this is to force-load the AirPortBrcm4360 kext.

**Reinjection of AirportAtheros40 kext**

Well for those running older Atheros cards may have a rude wake up call, reinjecting the old AirportAtheros40.kext seems to fail on many systems. A partial solution is to actually port the entire IO80211 framework but this is not ideal for many reasons...

Only good solution is looking for a new card: [Wireless Buyers Guide](https://khronokernel-7.gitbook.io/wireless-buyers-guide/).

**Removal of MacPro4,1/5,1 and the death of Dual Socket Systems**

Press F to pay respect for our fallen Cheesegrater, you've served us well but it is time for you to go now and rest. One odd quirk to Catalina is that now no systems are supporting Dual Socket CPUs in Apple's line up and because of this they've decided to change the AppleIntelMCEReporter.kext. This change causes errors when booting iMacPro1,1 or MacPro6,1 SMBIOS with Dual CPU configurations so we need to overcome this by blocking it. There’s multiple methods including making a [blocker kexts](https://github.com/khronokernel/DisableMCEReporter-/releases), blocking via OpenCore or using [CPUFriend](https://github.com/acidanthera/CPUFriend/releases). As things currently stand, CPUFriend cannot block but this may change in future releases.

[Source](https://www.insanelymac.com/forum/topic/339035-pre-release-macos-catalina/?do=findComment&comment=2681826)

**Restore macOS from snapshot**

OS Update break literally everything? Well now you don't need to rely on third party software for snapshots, now you can easily return to a state where macOS wasn't broken. This is done within the recovery partition.

**Kexts moved out of kernel space**

While this won't directly affect us in Catalina, kexts running in kernel space will now be deprecated. What this means is kexts like [Lilu](https://github.com/acidanthera/Lilu) won't have the low-level access it requires to perform its necessary patches. But deprecation doesn't mean removed, Clover's kext injection system has been deprecated since 10.7 but Apple has yet to remove it from macOS(well 10.16 might finally change that). So don't fear too much but keep this as a reminder that not everything will last as it is. Most if not all of Apple's kernel extensions are still in kernel space so they got a shit ton of work to do themselves.

**Support for Catalyst based apps**

Get ready for a flood of iOS apps on the AppStore cause now everyone's a Mac developer! There's no real hardware requirement besides natively supported hardware, that means users who have been patching their GPU drivers from High Sierra may have issues running Metal based applications.

# Current issues with Catalina

* Lilu won't load.
   * This means Lilu has not been updated for Catalina, either update [Lilu](https://github.com/acidanthera/Lilu/releases) or add flag `-lilubetaall`.
* ~~It doesn't fill the void within you~~
   * ~~2 parts Green Apple Vodka, 1 part Clorox. Repeat every 15 min~~
* Stalling on `kextd stall[0]: AppleACPICPU`.
   * SMC emulator isn't loading, make sure VirtualSMC and Lilu are updated. Temporary solution is `-lilubetaall`, update your kexts or swap for [FakeSMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/)
* Can't write to Library/Extensions to add my kexts.
   * While quite rare to need kexts in L/E, for those needing to do so can run `sudo mount -uw /` to mount the drive for read/write.
* Stalling on `apfs_module_start...`, `Waiting for Root device`, `Waiting on...IOResources...`, `previous shutdown cause...`
    * So with macOS catalina, there were some changes in how AppleACPIEC works which makes it so when it doesn't pass the checks and therefore stall. Specifically what seems to happen:

1. AppleACPIPlatform.kext loads and sets all devices with the ACPI name of `EC__` the property of `boot-ec`
2. It then hands off control to its plugin, AppleACPIEC.kext, and starts a probe for either `PNP0C09` or `boot-ec`
3. When loaded, it will then verify for the other meaning we must have both `PNP0C09` and `boot-ec`. If not, macOS will just get stuck but due to the nature of parallel kext loading we don't explicitly see the error instead seeing errors such as `apfs_module_start...`, `Waiting for Root device`, `Waiting on...IOResources...`, `previous shutdown cause...`, etc. And guess what, PCs don't have their embedded controller named `EC__` instead known by `EC0_`, `H_EC` or `ECDV`.
    
To get around these problems, we have a couple options:

* Block `com.apple.driver.AppleACPIEC`.
   * AppleACPIEC is used on laptops so blocking it can cause serious issues but on desktops there is no issue.
* Turn off your real EC and set a fake EC(we still need an EC present for AppleBusPower).
   * Recommended method for desktops, can severly screw up laptops.
      * [SSDTTime](https://github.com/corpnewt/SSDTTime)(Use this for when you only have access to the systems DSDT, `F4` at Clover scrren will dump it to EFI/CLOVER/ACPI/origin)
      * [USBmap](https://github.com/corpnewt/USBMap)(Some may already have an SSDT-EC in their EFI if they ran USBmap sometime after Nov 18, 2018)
      * [SSDT-EC-USBX.dsl](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/SSDT-EC-USBX.dsl) (Skylake and newer)
      * [SSDT-EC.dsl](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/SSDT-EC.dsl) (Haswell and older)
* Rename your EC device.
   * Not recommened for desktops as this still loads AppleACPIEC which is incompatible with desktop PCs, ideal solution for laptops. 

|Comment|Find\*\[HEX\]|Replace\[HEX\]|
|:-|:-|:-|
|change EC0 to EC|4543305f|45435f5f|
|change H\_EC to EC|485f4543|45435f5f|
|change ECDV to EC|45434456|45435f5f|

> But how do I know what EC I have? 

Easy to tell, open up your DSDT and search for `PNP0C09`. Most will just return 1 device but for those with 2 need to see which is the main. To check, make sure it has the following properties:
* `_HID`
* `_CRS`
* `_GPE`
Do note if you only have 1 `PNP0C09` device you will not need to check as even systems with only 1 won't always have all the proper properties.
    
Sources: 
* [AppleLife](https://applelife.ru/posts/807985)
* [Platform hardware implementation requirements](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/platform-hardware-implementation-requirements)
    
# Should you update and how to proceed

No, the majority shouldn't update as this is a .0 release meaning there's going to still be a lot of bugs hiding within potentially causing many headaches. The best practice is to wait until either 10.15.1 or even 10.15.2 as the majority of bugs will have been fixed by then.

Regarding how to update, make check the following:

* Backup EVERYTHING including your EFI before you updated your kexts or Clover
* Update Clover
* Update kexts
* [Verify if your EC device is setup correctly](https://github.com/khronokernel/What-s-new-in-macOS-Catalina#current-issues-with-catalina)(An absolute must, or else no Catalina for you)
* Do a once over of the [Vanilla Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)(so you can see what methods have been removed and are no longer needed)
* Make a new APFS volume to install Catalina on(never upgrade, always install fresh)
   * DiskUtility -> SSD -> + -> Add APFS Volume to Container
* Install Catalina and use migration assistant on the old drive

For those who want the terminal command for the USB:

```
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
```


# What's new in the Hackintosh scene?

**A new ~~foe~~ Boot Loader has appeared!**

From the folks that brought you the many wonders of the Hackintosh world as Lilu, VirtualSMC, WhateverGreen, AptioMemroyFix and so much more are here to present the next step in hackintoshing!: [OpenCore](https://github.com/acidanthera/OpenCorePkg). Currently in beta, what OpenCore wishes to accomplish is to make the Hackintosh environment more like a real mac by adopting many of these features and guidelines like Bless support and little to no ACPI patches. As things are now, most users have no reason to switch to OpenCore but as time goes on Clover may become more and more aged to the point that for many it will become too unstable. 


The most interesting part about OpenCore for AMD users is that this will alliviate most of the fear around macOS updates, specifically that the kernel patches will no longer need to be updated for every single minor OS update. The reason being is that the actual patches around AMD CPUs rarely had drastic change, instead just needing to  re-caluculate the offset for each update. Well with OpenCore, we're now able to specify a mask meaning that it'll figure out the offset for us. But this still isn't utimately perfect, reason being is Apple can always throw a curve ball like they did with `10.13.1-.3` vs `10.13.4-.6` but this is still great news. But the downside is that Clover users won't be getting macOS Catalina as AlGrey has no intention to work with Clover ever again instead lettting others convert the patches, so better learn how to make your patches!(Or wait for others to do the work)

For those who are wanting a Vanilla Desktop Guide for OpenCore are in luck!:
* [OpenCore Vanilla Desktop Guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/)

Please note that we're currently in beta but hoping to be in 1.0.0 release by the end of the month.

**AptioMemoryFix is dead, long live FwRuntimeServices!**

So as some of you may know or learning just now, AptioMemoryFix is now dead and will no longer receive any future support.
If you've checked the GitHub page for AptioFixPkg, you probably read the following:

> AptioFixPkg is now part of AppleSupportPkg and OpenCore, please use them instead:

> AptioMemoryFix.efi is split into OpenCore and FwRuntimeServices.efi.

The problem with this is that this means support for Clover is completely dead and if you want the latest in firmware fixes you'll need to completely ditch clover for OpenCore. And you may read the FwRuntimeServices part but this is also exclusive to OpenCore so don't even try loading that with clover. 

But AptioMemoryFix won't be gong anywhere thankfully, you can still download the final release from the [github](https://github.com/acidanthera/AptioFixPkg/releases).

> Are there alternatives?

Not really, your other option being osxaptiofix3drv but that's not ideal for many people. As things stand, AptioMemoryFix still works perfectly fine but do note that for both future releases of macOS and newer hardware may not work correctly with AptioMemoryFix

**Clover folder structre changes**

A fairly simple change but one that will catch people off guard, so starting with Clover r4985 we see that the placement of .efi drivers have changed a bit:

* `drivers64UEFI` -> `drivers/UEFI`
* `drivers64` -> `drivers/BIOS`

But do not fret, the old paths will still work as long as there's no files present in the new directories
* [Clover's source code](https://sourceforge.net/p/cloverefiboot/code/HEAD/tree/rEFIt_UEFI/refit/main.c#l1508)

**Where's the Navi support?**

Well just like with Mojave, talks of Navi support is sparce. Within the AMDRadeonX5000HWServices.kext, we can see 3 kexts that hint at full Navi line up support:

* AMDRadeonX5100HWLibs.kext
* AMDRadeonX5400HWLibs.kext
* AMDRadeonX5700HWLibs.kext

When taking a deeper look into these kexts, they show us that the code is only partially there requiring a master kext to handle them like an AMDRadeonX6000HWServices.kext. What early Polaris/Vega drivers have shown us before is that we might be waiting until either the end of this year or early next before we get support for Navi. At that point, we might be seeing Navi 20 which might finally come around to beat the pure compute champ that is Vega 20(Radeon VII). But then there's rumblings of Vega 30 so who knows. ¯\_(ツ)_/¯

Wanting to know when Navi support comes? Run the following pointed as S/L/E:
```
grep -ir "731F" .
```
This just checks if the 5700XT PCI ID is referenced anywhere, as of the GM there still is nothing that would hook onto a Navi GPU. 

**Chrome being dumb again**

Got SIP off and updated chrome? Well shit likely broke, I'll point you to some posts that are better at explaining this than me:

* [Google saying they fucked up and you need to reinstall](https://support.google.com/chrome/thread/15235262)
* [u/Fargo_Newb's work arounds](https://www.reddit.com/r/hackintosh/comments/d8tm8z/psa_google_chrome_updaterkeystone_rendering/)

**Stolen work of hard working hackintoshers**

Well you can never escape the scum that hide within our community, and today we talk about the disappointment that is the youtube channel Morgonaut. In a recent video of hers, Teress takes the hard work of Shannee, XNLC and AlGrey's kernel patches and tries to sell them in a pre-built EFI. I'll let a friend and one of the moderators of AMD OS X Discord take it away:

> **About Morgonaut, and why we don't support most YouTube tutorials**

> With the recent release of Morgonaut's video about a "vanilla" Ryzen hackintosh build, we thought we would take the time to provide some background about Morgonaut and their videos.

>Morgonaut is a YouTuber that steals credit for things, and when confronted, denies it. (for example, they made a video using CorpNewt's guide, but tell everyone to skip everything and just use the sample configs - they also claim CorpNewt is one of their "users", which is far from the truth.)

> They also provide "premium" support that is bullshit, and charge for things that you can get for free.
The Ryzen video specifically puts their EFI folder (something that shouldn't be shared as it's specific to every build) behind a paywall, including the patches.

> Oh, and they don't even link to the vanilla patches, so anyone who follows their video will be completely lost when it doesn't work because of a macOS update.

> And because you don't learn properly with a guide, you will be completely lost if your hack breaks.
This is why you shouldn't use Morgonaut, or any other YouTuber that just links to an EFI without actually explaining what you're doing.
- [DhinakG](https://github.com/dhinakg)

It's sad that we're just repeating what has already happened with the Tonymacx86 stealing tools tools, Rehabman stealing PJALM's configs and InasenlyMac Discord stealing our guides. Please don't support people who take the work of others, instead link to those who've put work into making this community better.

[The **real** Vanilla AMD Guide](https://vanilla.amd-osx.com/)

And going forward we would like users to not recommend such videos as they both neither teach you the proper process and generally just steal the works of others. Sidebar has everything you'd need.

# What's new with the subreddit?

This is more of a mini update from us, things that have changed:

* Updated sidebar with a new [Catalina GPU Buyers Guide](https://khronokernel-3.gitbook.io/catalina-gpu-buyers-guide/)
* New [Wireless Buyers Guide](https://khronokernel-7.gitbook.io/wireless-buyers-guide/)
* Updated Logos, banners and flairs(for those who want them: [DropBox](https://www.dropbox.com/sh/2ms4cm9x2nyhhpn/AABo9et1egeod2mcj5Mcspdta?dl=0))
* ~~New ways to indoctrinate users into the Vanilla Cult includng threat of castration~~


Hope you found this not so little post helpful, 

- Your local neighbourhood Hackintosh Slav


Credit:
* AppleLife for original EC accomidations
* OpenCorePkg team for EC SSDTs
* AlGrey for the AMD Kernel Patches
