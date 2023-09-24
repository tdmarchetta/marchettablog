There are several ways to potentially fix this problem at this time I don't have a Flowchart that can be followed i'm just collecting a bunch of Solutions in order to solve the problem.

>[!error]
> Status: **Agent Unreachable**

---

##### Logs

`C:\ProgramData\VMware\VDM\logs`

---

##### Collect the logs

###### Obtaining diagnostic information from the Horizon Agent
1.  Log in to a virtual machine with Horizon Agent installed.
2.  Open a command prompt and execute:

```cmd
cd %USERPROFILE%/Desktop
```

```cmd
C:\Program Files\VMware\VMware View\Agent\DCT\support.bat
```    
3.  On the desktop, the folder `vdm-sdct` contains zipped log files with the date of generation in the filename.

###### Enabling advanced debug logging
1.  Open a command prompt and run:
```cmd
"C:\Program Files\VMware\VMware View\Agent\DCT\support.bat" loglevels
```

2.  When prompted, press `2` to enable advanced logging.

>[!note]
> Enabling advanced debug logging will only ensure that any future events are logged at a high level of log granularity. Please only enable logging above this when directed to by VMware Support.

###### Collecting Agent logs remotely from the Connection Server

1.  Run this command in a command prompt:
```cmd
cd C:\Program Files\VMware\Vmware View\Server\tools\bin
```

```cmd
vdmadmin -A -getDCT -outfile file_name.zip -d pool_name -m virtual_machine_name
```

By default, this utility is located at `C:\Program Files\VMware\Vmware View\Server\tools\bin` in the Connection Server.

>[!note]
>Please use a unique, descriptive name for the file name – Ideally including the machine name and a timestamp.

2.  More detailed examples are available here: [Configuring Logging in Horizon Agent Using the -A Option](https://kb.vmware.com/s/article/1017939).

>[!note]
>-   The support bundle is created in the directory where the command is run.
>- The pool name and virtual machine name are both case-sensitive.
>-   This command may be run while a user is connected and working on the virtual machine.

[Collecting VMware Horizon View log bundles](https://kb.vmware.com/s/article/1017939?lang=en_US)

---

##### Powered on

Is the desktop (VM) is powered on?

---

##### Registry Editor

Confirm in the registry appropriate values are set for your brokers.

1. Login the VM Machine open the Registry editor run as administrator.
2. Go to the following path `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc\VMware VDM\Agent\Configuration`
3. Look for the registry *String Value (REG_SZ)*: Broker: Make sure the broker names are Entered 

>[!Example]
>Broker : < server.domain.com server2.domain.com >

---

##### Networking
1. IP address was obtained?
2. Ping both Server and VM
3. There is connectivity between the View Connection server and VDI

>[!note]
>When a virtual machine installed with Horizon view agent has more than one NIC, you must configure the subnet that Horizon view agent uses by modifying registry settings as:

1. Registry Configs
	- Horizon 7 onwards: `HKLM\Software\VMware, Inc.\VMware VDM\IpPrefix = n.n.n.n/m (REG_SZ)`
	- Horizon 6 and earlier: `HKLM\Software\VMware, Inc.\VMware VDM\Node Manager\subnet = n.n.n.n/m (REG_SZ)`

---

##### Resetkey

VMware Horizon ResetKey Agent is a command-line utility designed to reset the security key on a virtual machine running VMware Horizon Agent. The security key is a unique identifier that is used to establish a secure communication channel between the Horizon Agent and the Connection Server. In some cases, such as when a virtual machine is cloned or when the security key is out of sync, you may need to reset the security key to resolve connection issues.

From a Connection (broker) server open CMD run as a administrator.

```cmd
cd "C:\Program Files\VMware\VMware View\Server\tools\bin"
```

```cmd
vdmadmin -A -d < desktop-pool-name > -m < name-of-machine-in-pool > -resetkey
```

---

##### Sevices

All Horizon View Agent services have been started?
- VMware Horizon View Agent

---

##### Domain secure connection

Desktop’s domain secure connection is not broken?

---