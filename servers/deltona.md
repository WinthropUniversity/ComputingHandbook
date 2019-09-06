# Deltona

[Deltona](https://deltona.birdnest.org/) (`deltona.birdnest.org`) is a server that provides services for users.

## Services Offered
* Apache/PHP
* [MySQL](https://deltona.birdnest.org/mysql/)


## Apache/PHP

### Accessing Websites
Students can create websites using html and/or php which are accessible via the web.  If a students username is `besmera2` then their personal webspace would be `https://deltona.birdnest.org/~acc.besmera2`.  Note the inclusion of the `~` character and `acc.` prefix.  

### Adding Files
The files served by deltona are served from a special folder within the users linux profile called `public_html`.  Since this folder does not exist by default you may have to create the folder, e.g. `mkdir ~/public_html` or use your favorite editor/ftp client.  Files can be added from any linux machine or through the `SFTP` protocol on port `22` of the [hopper.winthrop.edu](hopper.md) machine.

### Permissions

It is important that permissions on your files are properly applied.  In general, for most use cases, the default of `755` will do.  If you need permissions other then this you should generally understand permission systems and the effect of your settings.  To get everything in your web directory visible on the web you can run the command `chmod -R 755 ~/public_html/` or use your favorite SFTP client to change each file to `755`.  The `public_html` directory must also have `755` or the web server will not be able to traverse the file system and serve your files.

***Note:*** If uploading files *remotely* [two factor authentication](hopper.md#two-factor-authentication-2fa) is needed.  Most clients e.g. filezilla will support this method of transfer and authentication.  To do so you may need to configure specific settings.  For example, in filezilla you would use the site manager to add an entry for hopper and set the login type to `Interactive`.  Further, under the `Transfer Settings` tab you should `Limit number of simultaneous connections` to `1`.  This will prevent multiple popups requesting a 2FA.  Filezilla will reuse the single connection for a period of time allowing you to upload as many files as you need without reprompting for a password or second factor.  If you are uploading files on campus none of these steps would be neccessary.  
