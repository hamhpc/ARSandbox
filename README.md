# ARSandbox Installation on CentOS 7

The ARSandbox is an application provided by UC Davis [https://arsandbox.ucdavis.edu/](https://arsandbox.ucdavis.edu/). It's used to build a Linux based computer that can control a projector and a Xbox Kinect camera. Using the two it can project and measure the sand in the box and draw the geological contours of the map in real time. 

[YouTube Video](https://www.youtube.com/embed/j9JXtTj0mzE)

[![AR Sandbox Video](http://img.youtube.com/vi/j9JXtTj0mzE/0.jpg)](http://www.youtube.com/watch?v=j9JXtTj0mzE)


# Why CentOS? 

Why do I want this installed on CentOS if the developer shows installing on Ubuntu? This is a common issue within the world of Open Source Software. Many developers make the choice to use Ubuntu to develop their applications. It's a choice based on the fact that Ubuntu has a better environment as a Desktop computer system. 

However, in Academics and Academic research programs, many Systems/IT Environments are heavily invested in Red Hat Enterprise Based Operating systems. So Ubuntu is something that isn't as common in the Data Center and skill-sets of IT. As a System's person myself, I do what I can to facilitate this gap between developer's and systems people. This helps everyone as when a researcher comes to IT for help with a project (like this one ARSandbox) it makes it more difficult for IT to try to manage an Operating System they aren't as familiar with.  

So, using my expertise with Red Hat based systems and kickstart I have built this procedure so to be able to give back to the open source project (ARSandbox). This contribution will allow other researchers who would like to utilize this research to be able to install and run it on a platform that IT is more familiar with so as to be able to get help from their local IT staff at their organization. 

It also helps minimize the steps needed to get this up and running since this automatic procedure will compile and build the software as part of the Operating System installation. This may reduce the need for assistance from IT to assist faculty with their own attempts to build one of these platforms. 


# Requirements 

To make this process as simple as possible, to build the Linux host all you need to do is download the CentOS 7 minimal ISO installer. 

You can download it from one of these [mirrors](http://isoredirect.centos.org/centos/7/isos/x86_64/)

## Create CDROM

Burn the ISO to a CD so you can boot from it. This is a useful document for [burning ISO's](https://www.centos.org/docs/5/html/CD_burning_howto.html).

On a PC, you can right click the ISO file and choose "Burn image to disk".

## Booting into a CD/DVD

Most PC BIOS's will allow you to temporarily boot from another medium (USB, CD, etc) by pressing the F12 (or possibly another F key) when the system boots up. This will provide you with a menu of choices to boot from. Select the CDROM/DVD device. 


# Installation

The machine will get built according to this [ARSandbox machine kickstart file](https://raw.githubusercontent.com/hamhpc/arsandbox/master/ks_arsandbox.cfg)

In order to get the installer to use this configuration we need to supply additional arguments to the bootloader. 

## 1.) Boot the host from the CDROM

![Boot the host from CDROM](/assets/images/boot-cdrom.png)

## 2.) Press TAB at installation menu

![Press Tab at installation menu](/assets/images/press-tab-at-boot-screen.png)

## 3.) Enter ks bootloader argument
after the word "quiet" add the following: 

```
ks=https://raw.githubusercontent.com/hamhpc/arsandbox/master/ks_arsandbox.cfg
```

![Enter ks bootloaer argument](/assets/images/add-kickstart-configuration.png)

Press Enter to continue. 

## 4.) Select Installation Destination
This step may be optional depending on how your disks are discovered. If it detects a previously installed OS you'll need to tell the software to over-write this old installation. 

![Select Installation Destination](/assets/images/select-installation-destination.png)

### 4a.) Configure for Automatic partition configuration

![Configure for Automatic partition configuration](/assests/images/auto-configure-disks.png)

### 4b.) Delete All

![Delete All](/assets/images/delete-all.png)

### 4c.) Reclaim Space

![Reclaim Space](/assets/images/reclaim-space.png)

## 5.) Begin installation

![Begin Installation](/assets/images/begin-installation.png)

## 6.) Enter Root Password

![Enter Root Password](/assets/images/root-password.png)

## 7.) Take a Break
 While the machine is building itself .. take a break as this could take a few minute to complete. Once it's up we'll be ready to log in and configure the ARSandbox software and Devices. 

![Starting Installation](/assets/images/starting-installation.png)

## 8.) Reboot when complete
Make sure you remove the CDROM before rebooting the machine. 

![Reboot when complete](/assets/images/reboot.png)

# Initial Login

The host is configured to automatically log into the arsandbox user account. 

## 1.) Select Konsole
 Right click the desktop and choose Konsole

![Boot the host from CDROM](/assets/images/open-konsole.png)

## 2.) Start typing commands

![Type commands](/assets/images/command-window.png)

# Hardware Calibration

## 1.) Get Intrinsic Calibration Parameters

Plug in your first-generation Kinect device and download intrinsic calibration parameters directly from its firmware. In a terminal window, run:

```
  % sudo /usr/local/bin/KinectUtil getCalib 0
```

Note: This might ask you for your password again; if so, enter it (arsandbox) to continue.

## 2.) Physically Align Camera

Align your camera so that its field of view covers the interior of your sandbox. Use RawKinectViewer to guide you during alignment. To start it, run in a terminal window:

```
  % sudo /usr/local/bin/RawKinectViewer -compress 0
```

While looking at the camera image in RawKinectViewer make sure the Kinect sensor is lined up with the sides of the sandbox. If the sandbox appears off from the viewport of the Kinect camera, tilt the Kinect camera until you can get these aligned. 

## 3.) Calculate Base Plane

 This step will calculate the plane (base level) of the SandBox. 

 If your box has sand in it put a sheet of cardboard or other flat surface over the top of the box. If you use this method, remember to measure the distance (in cm) from the cardboard to the bottom of the sandbox. In this case, the bottom of the box was 16cm below the cardboard. When you get the base plane equation from the rest of this step remember to add the value, from your measurement, to the last number in the equation. It's all explained in the following video:

[YouTube Video](https://www.youtube.com/watch?v=EW2PtRsQQr0)

[![Calculate Base Plane](http://img.youtube.com/vi/EW2PtRsQQr0/0.jpg)](http://www.youtube.com/watch?v=EW2PtRsQQr0)


Calculate your sandbox’s base plane, by following the instructions in the AR Sandbox Calibration video (above) that shows all required calibration steps in one. You can use the already-running instance of RawKinectViewer.

### 3a.) Average Frames

Select average frames

### 3b.) Define Base Plane

Extract Planes 

## 4.) Enter Equation into BoxLayout.txt

You need to enter the base plane equation (and the 3D sand surface extents in the next step) into the BoxLayout.txt file in /usr/local/etc/SARndbox-2.3

```
 % cd /usr/local/etc/SARndbox-2.3
 % kwrite BoxLayout.txt
```
Now enter the base plane equation as described in the video. To copy text from a terminal window, highlight the desired text with the mouse, and then either right-click into the terminal window and select “Copy” from the pop-up menu that appears, or press Shift-Ctrl-c. To paste into the text editor, use the “Edit” menu, or press Ctrl-v.

Pro tip: The quickest way to copy&paste text from any window into any other window is: 

  - Highlight text in source window with the mouse. 
  - Move mouse to destination window and to the location where you want to paste the text, and click the middle mouse button.

The equation will look something like the following from the RawConnectViewer output in the terminal: 

```
  (-0.0076185, 0.0271708, 0.999602) = -98.0000
```

Be sure to replace the = with a , and add the measured value from the cardboard to the bottom of the sandbox from above. The last number in this sequence is the depth. So in our example -98 becomes -114. So the first line of our BoxLayout.txt file has this:
 
 ```
  (-0.0076185, 0.0271708, 0.999602), -114.0000
```  

## 5.) Measure 3D extents of the Sand Surface

Note: Be sure to remove the cardboard covering the sandbox from the previous step. 

 In the newly-released Kinect-3.2 package, this can be done inside RawKinectViewer as well by [following the instructions in this video](https://youtu.be/EW2PtRsQQr0?t=4m10s), starting at 4:10. Make sure to measure the box corners in the order lower-left, lower-right, upper-left, upper-right. After you have copied the box corner positions into the text editor as described in the video, save the file (via the “File” menu or by pressing Ctrl-s), and quit from the text editor (via the “File” menu or by pressing Ctrl-q).

[![Measure 3D Extents](http://img.youtube.com/vi/EW2PtRsQQr0/0.jpg)](http://www.youtube.com/watch?v=EW2PtRsQQr0?t=4m10s)

In the terminal window from where you executed the RawConnectViewer you'll have the following four corner points: 

```
  (  -48.6846899089,  -36.4482382583,    -94.8705084084)
  (   48.3846899089,  -34.3992382583,    -89.3885084084)
  (  -50.6746899089,   35.8072382583,    -97.4085084084)
  (   48.7946899089,   36.4782382583,    -91.7415084084)
```

Add these to the BoxLayout.txt file after the plane equation from the last step. No changes need to be made to the formatting from the terminal. 

You can now Exit RawKinectViewer and save BoxLayout.txt

## 6.) Align Projector

Align your projector such that its image fills the interior of your sandbox. You can use the calibration grid drawn by Vrui’s XBackground utility as a guide. In a terminal, run:

```
  % /usr/local/bin/XBackground
```

After the window showing the calibration grid appears, press the “f11” key to toggle it into full-screen mode. Ensure that the window really covers the entire screen, i.e., that there are no title bar, desktop panel, or other decorations left.

Press Esc to close XBackground’s window when you’re done.

## 7.) Calibrate the Projector

Determine the resolution that your projector is running at. In our case it is running at 1024 X 768. 

Create the calibration tool. This is made up of a CD disc with blank sheet of paper glued to it. On the paper you have two lines perpendicular to one another, one horizontally and one vertically so that they make up a + sign with a point in the center of the disc. Using a wire coat hanger proved to work the best to form a handle to use when taking reference points. 

Calibrate the Kinect camera and the projector with respect to each other by running the CalibrateProjector utility:

```
 % /usr/local/bin/CalibrateProjector -s <width> <height>
```
  
where <width> <height> are the width and height of your projector’s image in pixels. For example, for an XGA projector like the recommended BenQ, the command would be:

```
 % /usr/local/bin/CalibrateProjector -s 1024 768
```

Very important: switch CalibrateProjector’s window to full-screen mode by pressing F11 before proceeding. Then [follow the instructions in this video](https://youtu.be/EW2PtRsQQr0?t=10m10s), starting at 10:10.

[![Calibrate Projector](http://img.youtube.com/vi/EW2PtRsQQr0/0.jpg)](http://www.youtube.com/watch?v=EW2PtRsQQr0?t=10m10s)

Create a capture tool and bind it to the "2" key. This will allow you to re-capture a background image after you've changed the topography of the sand. 

Pressing the "1" key will take each reference point by lining up your crosshairs from the CD to the lines drawn in the sandbox. 


## 8.) Run AR Sandbox

Finally, run the main AR Sandbox application:

```
  % /usr/local/bin/SARndbox -uhm -fpv &
```



# Software Information

  * Vrui is located at [https://github.com/KeckCAVES/Vrui](https://github.com/KeckCAVES/Vrui)
  * Kinect is located at [https://github.com/KeckCAVES/Kinect](https://github.com/KeckCAVES/Kinect)
  * SARndbox is located at [https://github.com/KeckCAVES/SARndbox](https://github.com/KeckCAVES/SARndbox)
  * [Complete Installation Instructions](https://arsandbox.ucdavis.edu/forums/topic/complete-installation-instructions/)

# Installation Notes

  * The arsandbox user password is "arsandbox"
  * The original sources are installed in /opt
  * The applications are compiled in /opt and then installed into the /usr/local directories. 
  * The user arsandbox owns the files in /opt and /usr/local so that account has full control over these installations to make changed as needed.
  * SELinux and Firewall is disabled so remember this isn't a secure as it could be if you leave it plugged into the network.
  * The user arsandbox is also a sudoer.. meaning if you need root access you can become root using the following command:

```
  % sudo su -
```

# Troubleshooting
