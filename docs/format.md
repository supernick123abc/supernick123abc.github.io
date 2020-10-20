# Formatting a Drive with Diskpart
###### *Source: [https://www.windowscentral.com/how-clean-and-format-storage-drive-using-diskpart-windows-10](https://www.windowscentral.com/how-clean-and-format-storage-drive-using-diskpart-windows-10)*

---

1. Open Command Prompt as Administrator. *Right Click > Run as Administrator*
2. Run the following command to get to diskpart:

    `diskpart`
    #


3. Use the following command to list the active disks on the system:

    `list disk`
    #


4. Type the following command to select the disk you want to work with:

    `select disk *Disk Number Here*` 

    <img src="https://www.windowscentral.com/sites/wpcentral.com/files/styles/w830/public/field/image/2020/04/diskpart-clean-drive-windows-10-cmd_.jpg?itok=UaskNSDG">

    ---

5. Completely wipe the drive of it's contents with the clean command:

    `clean`
    #

6. Check the disks again:

    `lisk disk`
    #

7. Create a new partition with the following command:

    `create partition primary`
    #

8. Select the newly created partition:

    `select partition 1`
    #


9. Now make the selected partition the active one:

    `active`
    #
    <img src="https://www.windowscentral.com/sites/wpcentral.com/files/styles/xlarge/public/field/image/2020/04/diskpart-create-primary-partition-windows-10_.jpg?itok=tel70nRb">

    ---

10. Type the following to format with NTFS and add an optional label:

    `format FS=NTFS label=LabelName quick`
    #

11. Assign a letter to the drive so that Windows can use it alongside other drives:

    `assign letter=g`
    #

12. Close both diskpart and cmd with the exit command:

    `exit`
    #