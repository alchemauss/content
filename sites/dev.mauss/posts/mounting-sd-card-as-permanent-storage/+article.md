---
date: "2019-01-12T17:38:25+07:00"
title: How to Mount your SD Card as Permanent Storage for Additional Space
tags: [tutorial, windows]
---

As a thin and light laptop user which typically has 128gb to 256gb of SSD storage I find that too small for my taste and usage, especially when you're installing a lot of programs, games, and/or syncing your cloud storage offline.

Fortunately, most laptop nowadays has provided slots for SD Cards which in my case was a microSD slot. In this tutorial, I will show how I can use it to expand my storage (it would work the same with a normal sized SD Card but you might want to get the half sized card so it would sit flush on the side of your laptop). Right now, I'm using a 128gb microSD Card from Samsung and I would say it helped a lot to keep my files and workspace organized.

## Objective

- Install programs to your SD Card
- Put your cloud storage directory in your SD Card
- Pretty much anything you could normally do in a permanent storage

<!-- content -->

### STEP 1 &bull; Precautions

- Install the latest builds and drivers
    1. Update and install the latest drivers for your Windows
    2. Update your [Windows OS](/posts/get-windows-update-asap)

### STEP 2 &bull; Reformatting

- Connect SD Card and format to the NTFS file system
    1. Open **This PC**
    2. Right-click on the SD Card and select **Format**
    3. Choose **NTFS** for the **File system** (Optional) Rename it with the **Volume Label**, *e.g. EXT*
    4. Click on **Quick Format** checkbox

### STEP 3 &bull; Mounting

- Create a mount point
    1. Open **This PC**
    2. Right-click on the **C: drive**
    3. Create a new folder and name it the same as your SD Card, *e.g. EXT*
- Mount SD Card
    1. Press the **WinKey + R** and type in **diskmgmt.msc**
    2. Right-click on SD Card and select **Change Drive Letters and Path**
    3. Click **Add**, select **Mount in the following empty NTFS folder** and click **Browse**
    4. Browse your previously created folder in **C: drive**
    5. Click **OK**
