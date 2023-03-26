The easiest way to send files to collaborators is to post them on [[Websites under u and p directories]].  If you need to control the privacy of those files, you can just set up an `.htaccess` file with password permissions; [Google can easily show you how](https://www.google.com/search?q=htaccess+password).

Some collaborators will want to send you files that are too big for email, but they might not be able to post them on a webserver so easily. For receiving such files, we run a little file-upload web service on `files.baulab.us`.

## Using the guest account for file upload

Tell your collaborator to visit `https://files.baulab.us/` and log in using the username `guest` and the password `177huntington`.

And tell them to create a new subdirectory for their content (to avoid confusion with anything anybody else might upload) and upload any files into that new subdirectory.

Things that they create will be deposited into the directory `/share/projects/baulab/files/guest`, and you can copy things out from there.  You may want to delete the uploaded files from that directory after you have gotten them (you might need `sudo` for this to do it on the fileserver, but you can also delete them using the web interface at http://files.baulab.us/).

## Setting up a new account for file upload

It is also possible to set up a private account for a collaborator who might need to do this a lot.  However, it is primitive and requires a little manual setup.

 1. Choose a username and password for your collaborators.
 2. Use the tool at [`https://tinyfilemanager.github.io/docs/pwd.html`](https://tinyfilemanager.github.io/docs/pwd.html) to generate a hash of the password.
 3. `ssh bauserver` and edit the file `/var/www/files/index.php`.  (You will need `sudo` to do this, or ask David).
 4. Add a line like `'signify' => '$2y$10$w7sCk7pEdWujLPKex2cXwof9e41llRG1T8cfabw9QcFX55CarTcae'` to the users array.

Like `.htaccess` passwords, the passwords here are not supersecure, but enough for sharing of proprietary data that you just want to keep off the public internet.