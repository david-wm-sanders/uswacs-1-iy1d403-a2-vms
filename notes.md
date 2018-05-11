# Research and Notes

## Task 1

#### VMware vSphere 6 Enterprise Plus Serial No.:
* M522H-XYL8Q-W8X80-0X9RH-8E8H5

#### Credential/Device Guard VMware Issue:
* Resolved with https://kb.vmware.com/s/article/2146361 - steps 1-2 + restart.
* Try each step individually to see if using only one step, group policy or Hyper-V, works.

#### ESXi Login Credentials:
root:54321qwert

#### ESXi IP Addresses:
http://192.168.254.136/  
http://[fe80::20c:29ff:fe6b:7530]/ (STATIC)  
I should probably use the static IPv6 address to avoid issues with VMware DHCP changing the IP if I restart the VM.

#### ESXi Web Interface:
https://[fe80::20c:29ff:fe6b:7530]/ui/

#### Justification for choosing to use the vSphere Web Client instead:
* From `task1_4_vsphereclientinstall_4.PNG`
* "All vSphere features introduced in vSphere 5.5 and beyond are available only through the vSphere Web Client. The traditional vSphere Client will continue to operate, supporting the same feature set as vSphere 5.0."

#### VMware-vSphere documentation
* https://docs.vmware.com/en/VMware-vSphere/index.html

#### ESXi Firewall
I could detail how to improve security by modifying the firewall rules to only allows vSphere Web Client connections from specific IPs.

## Task 2

#### Uploading Windows Server 2016 ISO to ESXI datastore1:
Uploading via the vSphere Web Client doesn't work - frozen at 0%
##### Workaround Process:
1. In the Web Client, enable SSH via `Host > Actions > Services > Enable Secure Shell (SSH)`
2. Download PuTTY/pscp.exe from https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
3. In the Web Client, find the path to `datastore1` by going to `Storage > datastore1` and viewing the `Location`.
4. Use pscp.exe to push, over SSH, the ISO to the `isos` folder in `datastore1`.

From the isos directory on workstation host:
`C:\tools\pscp.exe -6 .\14393.0.161119-1705.RS1_REFRESH_SERVER_EVAL_X64FRE_EN-US.ISO root@[fe80::20c:29ff:fe6b:7530]:/vmfs/volumes/5af1d3e9-abd56f67-0cdf-000c296b7530/isos`

#### vSphere Web Client IPv6 Console Issue
* Attempting to connect to a console via the Web Client using IPv6 fails - see `task2_6_winserver2016_11_dc_issue1_2.PNG`  
See https://communities.vmware.com/thread/542726 for further information.  
* I could try upgrading the ESXi Host Client or I could just use the IPv4 address to access the consoles...  
I think the latter option is easier.  
`https://192.168.254.136/ui/#/console/1`
* Alternatively, I could just use the vSphere Client (desktop) for this part.

#### Difference between Windows Server 2016 Standard and Datacenter
https://blogs.technet.microsoft.com/ausoemteam/2017/03/03/comparison-of-standard-and-datacenter-editions-of-windows-server-2016/

#### Windows 2016 Administrator Password:
`54321qwert*`  
I had to add a symbolic character to meet the complexity requirements...

#### ESXi can't see the internet issue...
I can't `ping 8.8.8.8` as there is "No route to host". I think this is probably because I added the NAT network adaptor in VMware after installing ESXi.  
* Can see network information for ESXi using commands at https://communities.vmware.com/thread/187557
* I need to find a way to get ESXi to pick up and configure the NAT.

##### Get the NAT to work with ESXi:
* See screenshots task2_8*

#### DC Setup
* https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/
* https://ittutorials.net/microsoft/windows-server-2016/setting-up-active-directory-ad-in-windows-server-2016/

##### DSRM PW:
54321qwert*

## Task 3

#### Setup Win10 Enterprise Client in VMware
Done: use Domain Join when installing, create a local `Client` account from which to change the DNS to point towards the DC.

## **todo:**
* Do Task 3
 * Create Active Directory accounts so we can login from the client
