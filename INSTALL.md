# Installing PHP Draft

 1. After downloading the latest release, extract the project to a temporary local directory.

 1. Create an empty MySQL database (as well as a corresponding MySQL user that has full access to the database) named `phpdraft`. 
     - If your hosting prepends your account name to the database name that's OK, just be sure to edit `initialize.sql` to match (Step 3) and `appsettings.php` (Step 5) with the correct name.

 1. Execute `/api/Domain/Migrations/2.0.0/initialize.sql` on the `phpdraft` database.
     - If your database name is not precisely `phpdraft`, you must edit **Line 4** of this script to reflect this *before* executing the script.

 1. Copy `[temp local directory]/api/config/EXAMPLE_appsettings.php` to one directory above your temporary local directory (so in the folder above `index.html`), renaming it to `appsettings.php`.

 1. Open `appsettings.php` in a text editor, and edit all applicable settings:

    - `DB_HOST`, `DB_PORT`, `DB_USER` and `DB_PASS` will need to be updated to allow PHP Draft to connect to the MySQL database
    - `DB_NAME` corresponds to the database name. In most cases, this can be `phpdraft`
    - `AUTH_KEY` is a secret key that will be used to generate login tokens. Keep it safe (do NOT share it!). If you're not sure what this is, use a "CodeIgniter Encryption Key" generated by www.randomkeygen.com
    - `RECAPTCHA_SECRET` is the private key for your Google reCAPTCHA 2 account. Without it new user registration will fail.
    - `MAIL_SERVER`, `MAIL_USER`, `MAIL_PASS` and `MAIL_PORT` allow PHP Draft to send emails for both new user email verification and password reset emails.
    - `APP_BASE_URL` is the home URL for your PHP Draft installation - for example `http://www.yoursite.com/phpdraft` (whatever you type in to the browser to load PHP Draft). *No trailing slashes!*
    - `API_BASE_URL` is the base url for your PHP Draft API - which should be the same as `APP_BASE_URL` except with `/api` appended to it, like: `http://yoursite.com/phpdraft/api`
    - The rest you can read up and configure as you like, but the above settings are the bare minimum.

 1. If your PHP Draft install will be at the base of the domain or subdomain (as in `your.site.com` or `yoursite.com`), **you may skip this step**. If your base URL looks like yoursite.com/phpdraft, **do the following**:

    - Open `index.html` and find this HTML: `<base href="/">` and change it to match the directory path after the base domain. So if your base URL is `yoursite.com/phpdraft`, then you must edit the HTML like this: `<base href="/phpdraft/">`

 1. Open `js/config.js` and update two values to match your installation:

    - **apiEndpoint** should be a fully qualified URL that points to your PHP Draft API (so `http://www.yoursite.com/phpdraft/api/` is valid - note the trailing slash)
    - **recaptchaPublicKey** should contain the *public key* portion of your Google reCAPTCHA 2 key

 1. In your FTP client, navigate to **one directory level ABOVE where PHP Draft will exist** and upload the `appsettings.php` you edited in Step 5.

    - For example, if my site's absolute server path is `/home/hosting_account/www/`, I need to upload `appsettings.php` to `/home/hosting_account/`. Ideally this file should not be served by the web server, but *must* be readable by PHP scripts in directories below it.

 1. **If you do not have HTTPS enabled**, you must disable the default URL rewrites that PHP Draft ships with.
    - This is not recommended. For your users' security, use HTTPS! *All those who enter abandon hope here*
    - For **Unix-based servers**, comment out **Line 8** of `.htaccess`
    - For **Windows (IIS) servers**, comment out the entire `Redirect to https` rule (**lines 8 - 14**)

 1. With your FTP client upload all contents of your temporary local directory into the directory that PHP Draft will exist.

 1. Go to the base URL of your PHP Draft install and register a new user account with your email address.

 1. In the `phpdraft` database, find the row in the `users` table that corresponds to your newly created account. Make three edits:

    - Change the value in the `enabled` column to `1` (which will enable your account)
    - Change the value in the `roles` column to `ROLE_ADMIN,ROLE_COMMISH` (which will make your account both a site administrator and a commissioner)
    - Change the value in the `verificationKey` to `NULL` (this deletes the key contained in the new user email sent to your email address - you don't need to validate your own email :) )

 1. Log in with your email address and password.

 1. In the top header bar, click on **Admin Stuff** then select **Player Data**. Using the CSV files contained in the `/resources/` directory, update each sport with the matching CSV file:

  Sport                      | CSV File
  -------------              | -------------
  Football (NFL)             | ProPlayers_NFLStandard_20xx.csv
  Football - Extended Rosters (NFL)    | ProPlayers_NFLExtended_20xx.csv
  Baseball (MLB)             | ProPlayers_MLB_20xx.csv
  Basketball (NBA)             | ProPlayers_NBA_20xx.csv
  Hockey (NHL)               | ProPlayers_NHL_20xx.csv
  Rugby (Super15)            | ProPlayers_Super15_20xx.csv

##### Congratulations! You have installed PHP Draft. Enjoy!