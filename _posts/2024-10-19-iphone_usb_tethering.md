---
layout: post
title:  "iPhone USB tethering on Windows PC"
use_math: false
comments: false
---

> In a nutshell, install those Apple device drivers:
`AppleMobileDeviceSupport64`, `Apple - USBDevice`, `Apple - Net`.
You never heard of the last 2? Then read along!

Because of the nature of my job (software development for military), often I get
to do coding on computers that are not connected to Internet.
Since I use CMake FetchContent feature nearly everywhere, the lack of Internet
really hurts my coding experience.
I don't know the exact condition, but CMake invalidates cache so often that I
frequenctly get to redownload all open source libraries again and again.
This is troublesome when Internet isn't available.

Of course there's a CMake option `FETCHCONTENT_FULLY_DISCONNECTED` that will
allow the build system to just proceed, assuming all required files are
correctly downloaded and available in filesystem.
This fixes the issue in some degree, but what if CMake conplains that the entire
caches are invalid and you need to delete the whole cache folder?
Maybe I can figure out the exact epicenter of the problem and just delete that
one.
All the headaches would be none had I just got Internet in the first place.

When we need Internet and it doesn't violate any security policy, one can just
bring their phone and turn on mobile Hot Spot or Tethering.
If the computer has Wi-Fi reciever then Hot Spot would work ok, or else get a
C-to-A or C-to-C USE cable and connect the phone to the computer.
Then activate USE tethering on your phone.
That's all!
Only for Android phone users, I guess.

My fellow workers who use Android phone simply does that.
So when I need Internet, I often ask them to please allow me to use your mobile
data.
That's a shame, because iPhone does not offer this easy USB Tethering option.
Plug in an USB cable just to see nothing actually happening!

# Install Apple Mobile Service

Well, that's because the computer in question doesn't have iPhone device driver
installed on the system.
You need to download drivers and manually intall them.
I don't know why Android phones don't have this problem but, hey, that's what it
costs to be a proper Apple iPhone owner.

Go to Apple [iTunes download page](https://www.apple.com/itunes/).
By default it suggests you to download it from Microsoft Store.
Ignore the button and scroll down a bit and You'll see
*Looking for other versions?* and some links.

![itunes_windows](/assets/images/misc_01/itunes_windows.jpg)<br>

Click *Windows* and you can get executable installer just like the old days.
Or download it [here for 64 bit](https://www.apple.com/itunes/download/win64) or
[here for 32 bit](https://www.apple.com/itunes/download/win32) in case you fully
trust me.
Always be cautious when you download anything from unknown sources. (which is,
for now, me)

Once the installer file is downloaded, you can extract it with 7zip or WinRAR.

![extract_itunes](/assets/images/misc_01/extract_itunes.png)<br>

Inside it there is a `exe` and several `msi` files.
Install `AppleMobileDeviceSupport64.msi` and you are set!
Now the tethering should work, or... does it?

# Install Device Driver

The above method is what most blog posts suggest if you search on Google.
I guess they all tested it on Internet available environment, where Windows OS
can automatically get and install device drivers from Internet.

For me, it was not possible because Internet was not an option, when this [Apple
community post](https://discussions.apple.com/thread/255478610) came into my
sight.

The author provides with two links that directly download some `cab` files.
(Always be cautious!!)
You can extract those as well.
There is a `inf` file in each `cab` file that
you need to install by right clicking it.

I'll leave the direct download links here.
* [Apple USB Drivers](https://catalog.s.download.windowsupdate.com/d/msdownload/update/driver/drvs/2020/11/01d96dfd-2f6f-46f7-8bc3-fd82088996d2_a31ff7000e504855b3fa124bf27b3fe5bc4d0893.cab)
* [Apple Tether USB Drivers](https://catalog.s.download.windowsupdate.com/c/msdownload/update/driver/drvs/2017/11/netaapl_7503681835e08ce761c52858949731761e1fa5a1.cab)

I tried them myself on disconnected Windows computer and it worked!
I didn't need to call my coworkers anymore.

It seems the original author downloaded the driver files from
[Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=apple%20usb)
but I'm not sure because I didn't tested it myself.
If you don't trust a random Apple community user, and want to get the file
directly from Microsoft, please try this ones.
You can search for 'Apple USB' and 'Apple Net' to get same files.

# Conclusion

The reason why I'm writing post is weird.
Because Apple community won't allow me to print out the page as PDF file.
That sucks.
So I'm writing this to permanently preserve this precious solution so when I need
it I can eaily find it.
