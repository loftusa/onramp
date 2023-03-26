The easiest way to send files to collaborators is to post them on [[Websites under u and p directories]].  If you need to control the privacy of those files, you can just set up an `.htaccess` file with password permissions; [Google can easily show you how](https://www.google.com/search?q=htaccess+password).

Some collaborators will want to send you files that are too big for email, but they might not be able to post them on a webserver so easily. For receiving such files, we run a little file-upload web service on `files.baulab.us`.

However, it is primitive and requires a little manual setup.

 1. Choose a username and password for your collaborators.
 2. Use the tool at [`https://tinyfilemanager.github.io/docs/pwd.html`](https://tinyfilemanager.github.io/docs/pwd.html) to generate a hash of the password.
 3. `ssh bauserver` and edit the file `/var/www/files/index.php`.  (You will need `sudo` to do this, or ask David).
 4. Add a line like `'signify' => '$2y$10$w7sCk7pEdWujLPKex2cXwof9e41llRG1T8cfabw9QcFX55CarTcae'` to the users array.

Like `.htaccess` passwords, the passwords here are not supersecure, but enough for one-time sharing of proprietary things that aren't super-secret and things that are not used to secure other secrets.