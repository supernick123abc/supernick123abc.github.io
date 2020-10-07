# Cleaning C Drive
[Source](https://superuser.com/questions/1433475/my-c-drive-is-full-without-reason)    

## Get an overview about your disk space
If you want general information about what is using disk space on your computer, you can use tools like [WinDirStatPortable](https://portableapps.com/apps/utilities/windirstat_portable). Select the drive(s) you want to have information about and start the analysis. The result is pretty much self-explanatory. You get an overview of directories and files sorted by size. Additionally, you get a visual representation of the used disk space.

#### WinDirStat
<img src="/img/wd.jpg">

## Use Storage Sense
You find that under Settings > System > Storage, or just type Storage after you opened your Win 10 Start Menu

<img src="/img/ss.png">

## cleanmgr.exe
Use Disk Cleanup tool to get rid of temporary files

<img src="/img/dc.png">

## Clean up the Windows Component Store
Open a Powershell session (as Admin) and analyze your component store by running

    dism /online /Cleanup-image /AnalyzeComponentStore
This can take several minutes to complete. If it gives you the advise to cleanup the component store, run

    dism /online /Cleanup-Image /StartComponentCleanup

You get more information by running
    
    dism /online /Cleanup-Image /?


## Check for the existence of volume shadow copies
List information about the shadow storage

    vssadmin list shadowstorage
Or get information about the shadow files

    vssadmin list shadows
Delete the oldest one on your C drive by running

    vssadmin delete shadows /for=c: /oldest
Alternatively, you could delete them all

    vssadmin delete shadows /all

## Disable and re-enable hibernation 

The Hiberfil.sys hidden system file is located in the root folder of the drive where the operating system is installed. It is approximately as big as the amount of random access memory (RAM) installed on the computer, as it stores a copy of the system memory on your hard disk when the hybrid sleep setting is turned on.

Disable hibernation
    
    powercfg /h off

Enable hibernation

    powercfg /h on