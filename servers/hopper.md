# Hopper

Hopper (`hopper.winthrop.edu`) is available from both on campus and off campus via ssh.  It uses your standard winthrop username and password to login.  If you are off campus you will be required to use two factor authentication (2FA) in order to login.

## Using SSH (Terminal Access)

### Windows Users

Windows users can use [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) to connect using the SSH protocol to the host `hopper.winthrop.edu`.

### Mac/Linux Users

Mac and Linux users have a built in ssh client.  Simply open a terminal and type `ssh YOURUSERNAME@hopper.winthrop.edu`.

**After machines have been reimaged you will have problems connecting.**  I recommend removing the offending entry, the ssh tool should tell you how to do that.  If it does not, or you do not know how to do this, simply delete the file `~/.ssh/known_hosts` and you will again be able to connect to the server.

## Remote Desktop

During the Fall 2022-Spring 2023 school year, remote desktops to the linux lab/machines are available.  Virtual desktops are a quick way to work on a lab machine from anywhere on campus including classrooms, labs, and study spaces.  Further, you can access the lab machines/virtual desktops from home provided you have configured 2FA.  In order to do this you will need to install the NoMachine Enterprise Client which can be found at the [NoMachine Download Page](https://www.nomachine.com/download-enterprise#NoMachine-Enterprise-Client). Once it is installed setup is fairly easy:
1) Click `+Add`
2) Set `Name` to `Hopper`
3) Set `Host` to `hopper.winthrop.edu`
4) On the `Protocol` dropdown set the protocol to `SSH` which should automatically change `Port` to `22`
5) Click `Connect`

Once you are connected you will see all the currently available desktop servers.  Each server can support up to 4 clients so unless you have a specific reason to need a specific server you should locate one with less than 4 clients to start your desktop on.  With the exception of virtual machines (which should be created using virtualbox), all files are shared amoung all desktops.  Thus, which you choose in most cases is irrelevant.  Two machines: `thur114-hp1` and `thur114-hp2` notably have 128GB of RAM, a 10C i9 processor, and an Nvidia GeForce RTX 3090.  

## Getting Access

Access to hopper (and all linux machines) is restricted to students and faculty who need access.  Typically access is granted through enrollment as part of a CSCI, DIFD, or MATH course. The current list of linux eligible courses is `'CSCI101','CSCI207','CSCI208','CSCI243','CSCI271','CSCI290','CSCI311','CSCI355','CSCI365','CSCI411','CSCI431','CSCI466','CSCI475','CSCI476','DIFD451','MATH370','MATH570'`.  Please contact the chair of the CS department if you feel your course should be added to this list.

Once you are granted access you will maintain that access until you leave the university. If you believe you should have access but do not please contact [helpdesk@winthrop.edu](mailto://helpdesk@winthrop.edu) to request it.  The request will need be approved.

## Two Factor Authentication (2FA)

Hopper uses 2FA when connecting in remotely.  If you are on campus 2FA is not required to use the server.

### 2FA Setup

In order to setup 2FA you will have to physically visit any one of the lab machines and login using your campus username and password and perform the following steps:  **Note: Until Jan 1 2019 you may simply sign into hopper using your username and password and then continue these instructions.**

1. Install a Google Authenticator Client available on:
	* [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=en_US)
	* [iPhone](https://itunes.apple.com/us/app/google-authenticator/id388497605?mt=8)
	* [Google Chrome](https://chrome.google.com/webstore/detail/authenticator/bhghoamapcdpbohphigoooaddinpkbai) - ***Useful for those without a phone.***
2. Sign into a machine while on campus.  You can use any machine in Thurmond 114, Carroll 215, or even a virtual machine in any of the ACC labs.
3. Open the terminal and run `google-authenticator`
4. I recommend you accept all the defaults and indicate `Yes` for any questions asked of you.
5. Scan the QR code or use the secret to manually create your entry into the Google Authenticator.
6. That's it. Now anytime you login to hopper remotely you will be asked for a `Verification Code:`, just enter what the Google Authenticator indicates is your current second factor.  *Note:* This second factor changes every 30 seconds but if you accepted the default there is a little wiggle room on either end of the 30 seconds so you don't need to rush.
