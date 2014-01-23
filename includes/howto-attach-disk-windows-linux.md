#How to Attach a Data Disk to a Virtual Machine




- [Concepts](#concepts)
- [How to: Attach an existing disk](#attachexisting)
- [How to: Attach an empty disk](#attachempty)
- [How to: Initialize a new data disk in Windows Server 2008 R2](#initializeinWS)
- [How to: Initialize a new data disk in Linux](#initializeinlinux)



<h2><a id="concepts"></a>Concepts</h2>



You can attach a data disk to a virtual machine to store application data. A data disk is a Virtual Hard Disk (VHD) that you can create either locally with your own computer or in the cloud with Windows Azure. You manage data disks in the virtual machine the same way you do on a server in your office.

You can upload and attach a data disk that already contains data to the virtual machine, or you can attach an empty disk to the machine. The virtual machine is not stopped to add the disk. You are limited in the number of disks that you can attach to a virtual machine based on the size of the machine. The following table lists the number of attached disks that are allowed for each size of virtual machine. For more information about virtual machine and disk sizes, see [Virtual Machine Sizes for Windows Azure](http://go.microsoft.com/FWLink/p/?LinkID=294683).

**Data Disk vs. Resource Disk**  
Data Disks reside on Windows Azure Storage and can be used for persistent storage of files and application data.

Each virtual machine created also has a temporary local *Resource Disk* attached. Because data on a resource disk may not be durable across reboots, it is often used by applications and processes running in the virtual machine for transient and temporary storage of data. It is also used to store page or swap files for the operating system.

On Windows, the Resource Disk is labeled as the **D:** drive.  On Linux, the Resource Disk is typically managed by the Windows Azure Linux Agent and automatically mounted to **/mnt/resource** (or **/mnt** on Ubuntu images). Please see the [Windows Azure Linux Agent User Guide](http://www.windowsazure.com/en-us/manage/linux/how-to-guides/linux-agent-guide/) for more information.

For more information about using data disks, see [Manage disks and images](http://msdn.microsoft.com/en-us/library/windowsazure/jj672979.aspx).




<h2><a id="attachexisting"></a>How to: Attach an existing disk</h2>




1. If you have not already done so, sign in to the [Windows Azure Management Portal](http://manage.windowsazure.com).



2. Click **Virtual Machines**, and then select the virtual machine to which you want to attach the disk.

3. On the command bar, click **Attach**, and then select **Attach Disk**.


	![Attach data disk](./media/howto-attach-disk-window-linux/AttachExistingDiskWindows.png)

	The **Attach Disk** dialog box appears.



	![Enter data disk details](./media/howto-attach-disk-window-linux/AttachExistingDisk.png)

3. Select the data disk that you want to attach to the virtual machine.
4. Click the check mark to attach the data disk to the virtual machine.
  
 
	You will now see the data disk listed on the dashboard of the virtual machine.


	![Data disk successfully attached](./media/howto-attach-disk-window-linux/AttachSuccess.png)

<h2><a id="attachempty"></a>How to: Attach an empty disk</h2>



When you create a new data disk, you decide the size of the disk. After you attach an empty disk to a virtual machine, the disk will be offline. You must connect to the virtual machine, and then use Server Manager to initialize the disk before you can use it.

**Note:** Windows Azure storage supports blobs up to 1TB in size, which accommodates a VHD with a maximum virtual size 999GB. When you enter "1TB" in Hyper-V for a new hard disk, the value represents the virtual size.  This yields a file that is oversized by 512 bytes.



1. Click **Virtual Machines**, and then select the virtual machine to which you want to attach the data disk.



2. On the command bar, click **Attach**, and then select **Attach Empty Disk**.


	![Attach an empty disk](./media/howto-attach-disk-window-linux/AttachDiskWindows.png)



	The **Attach Empty Disk** dialog box appears.



	![Attach a new empty disk](./media/howto-attach-disk-window-linux/AttachNewDiskWindows.png)



3. In **Storage Location**, select the storage account where you want to store the VHD file that is created for the data disk.
 


4. In **File Name**, either accept the automatically generated name or enter a name that you want to use for the VHD file that is stored. The data disk that is created from the VHD file will always use the automatically generated name.



5. In **Size**, enter the size of the data disk. 



6. Click the check mark to attach the empty data disk.

	You will now see the data disk listed on the dashboard of the virtual machine.


	![Empty data disk successfully attached](./media/howto-attach-disk-window-linux/AttachEmptySuccess.png)



<h2><a id="initializeinWS"></a>How to: Initialize a new data disk in Windows Server 2008 R2</h2>



The data disk that you just attached to the virtual machine is offline and not initialized after you add it. You must access the machine and initialize the disk to use it for storing data. 



1. Connect to the virtual machine by using the steps listed in [How to log on to a virtual machine running Windows Server][logon].



2. After you log on to the machine, open **Server Manager**, in the left pane, expand **Storage**, and then click **Disk Management**.



	![Open Server Manager](./media/howto-attach-disk-window-linux/ServerManager.png)



3. Right-click **Disk 2**, and then click **Initialize Disk**.



	![Initialize the disk](./media/howto-attach-disk-window-linux/InitializeDisk.png)

4. Click **OK** to start the initialization process.



5. Right-click the space allocation area for Disk 2, click **New Simple Volume**, and then finish the wizard with the default values.
 

	![Initialize the volume](./media/howto-attach-disk-window-linux/InitializeDiskVolume.png)



	The disk is now online and ready to use with a new drive letter.



	![Volume successfully initialized](./media/howto-attach-disk-window-linux/InitializeSuccess.png)




<h2><a id="initializeinlinux"></a>How to: Initialize a new data disk in Linux </h2>



1. Connect to the virtual machine by using the steps listed in [How to log on to a virtual machine running Linux][logonlinux].



2. In the SSH window, type the following command, and then enter the password for the account that you created to manage the virtual machine:

	`sudo grep SCSI /var/log/messages`

	You can find the identifier of the last data disk that was added in the messages that are displayed.



	![Get the disk messages](./media/howto-attach-disk-window-linux/DiskMessages.png)



3. In the SSH window, type the following command to create a new device, and then enter the account password:

	`sudo fdisk /dev/sdc`



4. Type **n** to create a new partition.


	![Create new device](./media/howto-attach-disk-window-linux/DiskPartition.png)

5. Type **p** to make the partition the primary partition, type **1** to make it the first partition, and then type enter to accept the default value for the cylinder.


	![Create partition](./media/howto-attach-disk-window-linux/DiskCylinder.png)



6. Type **p** to see the details about the disk that is being partitioned.


	![List disk information](./media/howto-attach-disk-window-linux/DiskInfo.png)



7. Type **w** to write the settings for the disk.


	![Write the disk changes](./media/howto-attach-disk-window-linux/DiskWrite.png)

8. You must create the file system on the new partition. Type the following command to create the file system, and then enter the account password:

	`sudo mkfs -t ext4 /dev/sdc1`


	![Create file system](./media/howto-attach-disk-window-linux/DiskFileSystem.png)

9. Type the following command to make a directory for mounting the drive, and then enter the account password:

	`sudo mkdir /mnt/datadrive`



10. Type the following command to mount the drive:

	`sudo mount /dev/sdc1 /mnt/datadrive`

	The data disk is now ready to use as **/mnt/datadrive**.



11. Add the new drive to /etc/fstab:

	To ensure the drive is re-mounted automatically after a reboot it must be added to the /etc/fstab file. In addition, it is highly recommended that the UUID (Universally Unique IDentifier) is used in /etc/fstab to refer to the drive rather than just the device name (i.e. /dev/sdc1). To find the UUID of the new drive you can use the **blkid** utility:
	
	`sudo -i blkid`

	The output will look similar to the following:

		`/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"`
		`/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"`
		`/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"`

	**Note:** blkid may not require sudo access in all cases, however, it may be easier to run with `sudo -i` on some distributions if /sbin or /usr/sbin are not in your `$PATH`.

	**Caution:** Improperly editing the /etc/fstab file could result in an unbootable system. If unsure, please refer to the distribution's documentation for information on how to properly edit this file. It is also recommended that a backup of the /etc/fstab file is created before editing.

	Using a text editor, enter the information about the new file system at the end of the /etc/fstab file.  In this example we will use the UUID value for the new **/dev/sdc1** device that was created in the previous steps, and the mountpoint **/mnt/datadrive**:

	`UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e    /mnt/datadrive    ext4    defaults    1    1`

	If additional data drives or partitions are created you will need to enter them into /etc/fstab as well.

	You can now test that the file system is mounted properly by simply unmounting and then re-mounting the file system, i.e. using the example mount point `/mnt/datadrive` created in the earlier steps: 

		`sudo umount /mnt/datadrive`
		`sudo mount /mnt/datadrive`

	If the second command produces an error please check the /etc/fstab file for correct syntax.





[logon]: /en-us/manage/windows/how-to-guides/log-on-a-windows-vm/
[logonlinux]: /en-us/manage/linux/how-to-guides/log-on-a-linux-vm/
[Manage disks and images]: http://go.microsoft.com/fwlink/?LinkId=263439

[Volume successfully initialized]: ./media/howto-attach-disk-window-linux/InitializeSuccess.png
[Write the disk changes]: ./media/howto-attach-disk-window-linux/DiskWrite.png

