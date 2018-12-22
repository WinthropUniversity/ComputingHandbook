# Hopper

Hopper (`hopper.winthrop.edu`) is available from both on campus and off campus via ssh.  It uses your standard winthrop username and password to login.  If you are off campus you will be required to use two factor authentication (2FA) in order to login. 

## Using SSH

### Windows Users

Windows users can use [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) to connect using the SSH protocol to the host `hopper.winthrop.edu`.

### Mac/Linux Users

Mac and Linux users have a built in ssh client.  Simply open a terminal and type `ssh YOURUSERNAME@hopper.winthrop.edu`.  

**After machines have been reimaged you will have problems connecting.**  I recommend removing the offending entry, the ssh tool should tell you how to do that.  If it does not, or you do not know how to do this, simply delete the file `~/.ssh/known_hosts` and you will again be able to connect to the server.

## Getting Access

Access to hopper (and all linux machines) is restricted to students and faculty who need access.  Typically access is granted through enrollment as part of a CSCI, DIFD, or MATH course.  Once you are granted access you will maintain that access until you leave the university. If you believe you should have access but do not please contact [helpdesk@winthrop.edu](mailto://helpdesk@winthrop.edu) to request it.  The request will need be approved.

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
