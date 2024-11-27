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
Then activate USB tethering on your phone.
That's all!
Only for Android phone users, though.

My fellow workers who use Android phone simply do that.
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
[here for 32 bit](https://www.apple.com/itunes/download/win32).
Always be cautious when you download anything from unknown sources.
You can trust me though, definately!

Once the installer file is downloaded, you can extract it with 7zip or WinRAR.

![extract_itunes](/assets/images/misc_01/extract_itunes.png)<br>

Inside it there is a `exe` and several `msi` files.
Install `AppleMobileDeviceSupport64.msi` and you are set!
Now the tethering should work, or... does it?

# Install Device Driver

The above method is what most blog posts suggest if you search on Google.
Unfortunately it didn't work for me.
I checked Windows services app to make sure the Apple Mobile Device service was running, and even tried restarting it but nothing changed.

I did minutes of googling and ran into an [Apple community post](https://discussions.apple.com/thread/255478610) that addresses this problem.
Turned out, I was ought to install 2 additional drivers.
The author provides with convenient links to download `cap` files, which you may decompress and get driver installer files.

The above method worked perfectly and I was happy with that, except for the fact that I had downloaded files from unknown sources.
I settled with that at that time because I was so busy working on my programming that I cannot waste any more time on top of the all the efforts that it had taken to get the USB tethering working.
Later, once the job had been done, I decided to dig in to find a reliable download link for those files.

[Microsoft Update Catalog](https://www.catalog.update.microsoft.com/Search.aspx?q=apple%20usb) is the place to go.
It's the properly signed official web site running by Microsoft.
You can search for 'Apple USB' and 'Apple Net' to get same files.

![download_apple_usb](/assets/images/misc_01/download_apple_usb.jpg)<br>

Search `apple usb` and download the highlighted entry.

![download_apple_net](/assets/images/misc_01/download_apple_net.jpg)<br>

Search `apple net` and download it as well.

![extractdecompress_cab_files_itunes](/assets/images/misc_01/decompress_cab_files.png)<br>

Then you can decompress them in the same way you did with iTunes installer file.
You will get two folders afterward, each of which contains a `.inf` file.
Install `AppleKIS.inf` and `netaapl.Inf` respectively by right clicking.

![install_inf_file](/assets/images/misc_01/install_inf_file.png)<br>

Now that you installed all 3 progams, *Apple Mobile Device Service*, *Apple USBDevice driver*, and *Apple Net driver*, we are all set!
Restart your Windows and the whole world is yours.
Simply logging out and logging in again works as well, if you know what I mean.

# Conclusion

The reason why I'm writing post is weird.
Because Apple community won't allow me to print out the page as PDF file.
That sucks.
So I'm writing this to permanently preserve this precious solution so when I need
it I can eaily find it.
