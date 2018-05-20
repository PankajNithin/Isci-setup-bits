# Iscsi-setup-bits

The following steps were used for establishing  connection between an Iscsi target (Ubuntu system installed over virtualbox in Windows 10) and an Iscsi client (Windows 10)

**Updating the package manager**
---
`sudo apt-get update`<br/>
`sudo apt-get upgrade`
![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step_0.png)

---
**Installing Iscsi target packages for Ubuntu**
---
Installing two packages *iscsitarget* and *iscsitarget-dkms* these two packages and installed to enable an open source iSCSI target with professional features, that works well in enterprise environment under real workload, and is scalable and versatile enough to meet the challenge of future storage needs and developments.

`sudo apt-get install iscsitarget iscsitarget-source iscsitarget-dkms`

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-1.png)

---
**Enabling Iscsi Target feature**
---
Once the required packages are installed, the Iscsi-target feature has to be enabled.Not doing so will not allow the target to start.
This is done by editing *iscsitarget* file in */etc/default*

`sudo nano /etc/default/iscsitarget`

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-2.png)

In the file, we'll set ISCSITARGET_ENABLE=true

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-3.png)


---
**Creating a storage directory**
---
Next step is to create an Iscsi LUN. LUN stands for logical Unit Number (Storage unit).  A LUN represents an individually addressable (logical) SCSI device that is part of a physical SCSI device (target). In this demo, we'll use file based LUN.

We'll use the basic linux `dd` command for creating a file based storage unit inside */media/volume0/* directory.<br/>
`dd if=/dev/zero of=/media/volume0/storlun0.bin count=0 obs=1 seek=1G`<br/>
The above command will create an LUN with a capacity of 1GB.

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-4.png)

---
Creating and Naming Target
---

Next we'll edit the Iscsi Daemon configuration file `/etc/ietd.conf ` by specifying the *IQN* (Iscsi Qualified Name) and the target LUN details.<br/>

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-5.png)

Adding the `Target iqn.2018-05.example.bits:storage.sys0 Lun 0 Path=/media/volume0/storlun0.bin,Type=fileio,ScsiId=lun0,ScsiSN=lun0` to the end of the file.<br/>

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-6.png)

The unique IQN name is given as *iqn.2018-05.example.bits:storage.sys0* and LUN unit (0th LUN) will be the previously created LUN */media/volume0/storlun0.bin* .

---
**Restart to update changes**
---

`sudo service iscsitarget restart`

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-7.png)

---
**Testing with the inbuilt windows Iscsi client**
---

Open the built-in windows Iscsi initiator and provide the target system IP for discovery

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-8.png)

Once connected,click on Done.

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-9.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/step-10.png)

Within the disk management tab, the unallocated Iscsi target will be listed.

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/unallocated.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/computer_management.png)

Create a simple parttioning of the unallocated Iscsi target.

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/a.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/b.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/c.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/d.png)

After partitioning, the newly created drive (D) will be listed.

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/e.png)

![alt text](https://raw.githubusercontent.com/PankajNithin/Isci-setup-bits/master/f.png)


This drive can be treated as a normal physical harddrive as long as the connection to the target is active.


