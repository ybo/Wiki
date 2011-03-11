================================================================================
Installing MarkUs on Windows
================================================================================

.. contents::


(Note: Though there have been instances where developers have been able to get
a MarkUs development environment running on Windows, it is more difficult than
an install on either Linux or OS X.  We suggest developing on either of these
two other platforms.)

Installing InstantRails
================================================================================

Paraphrased from the "Agile Web Development with Rails" textbook by Dave
Thomas and David Heinemeier Hanssson, 2nd edition.

 * Download [[InstantRails |
 * http://instantrails.rubyforge.org/wiki/wiki.pl"]],
    version 2.0 as of the time of this writing.
 * Create a folder on your system, but make sure there are no spaces in the
   path. For example, do **not** use "C:/Program Files/InstantRails".
 * Unzip the download into the folder that you created.
 * Run it by clicking on the InstantRails icon, (the ugly red I).
 * Say "OK" when prompted about regenerating the configuration files.
 * By now you should see the InstantRail window on which you can monitor all your application.

We require a gem with version > 1.3.x and Rails version > 2.2, which InstantRails 2.0 does not come with. To upgrade these:

* Click on the black I button in your Instant Rails window. Click on Rails Applications > Open Ruby Console Window. You should see a window that looks just like the Windows command prompt, but it is much more useful.
* Run `gem update --system` to upgrade the Gem. This may take a few minutes, so be patient.
* Run `gem update rails` to upgrade the Rails version.

Installing a Database
================================================================================

You can use several types of databases with MarkUs.  The recommended database
is Postgres.

Using MarkUs with Postgres
================================================================================

Installing the Database
--------------------------------------------------------------------------------

 * Install [[PostgreSQL | http://www.postgresql.org/]].
   (At the time of writing Version 8.3 works well, version 8.4 may have some
    problems installing on Windows).
 * The default user is "postgres", and you will be prompted to set a password.
 * Locate the \bin directory in the location in which you installed PostgreSQL.
   Note the path.
 * Add this path to the PATH environment variable: Start > Control Panel >
   System > Advance > Environment Variables. Select the PATH variable, and the
   append the `bin` directory path to the end.
 * Start the Postgres server by: Start > Programs > Postgres > Start Server.  If you are using Vista or Windows 7, you will need to run it as an administrator (right click and select "Run as administrator").
 * Connect to the server: Start > Programs > Postgres > Start Server > SQL Shell. Hit enter to each prompt until you're asked for the password. You should now be in the interactive terminal.

## Installing the Driver

 * Run InstantRails
 * InstantRails > Rails Applications > Open Ruby Console Window
 * The command `gem install postgres-pr` will install the Postgres driver

If RubyGems is not updated to the latest version, it is possible that this
command will hang as there are some memory problems with RubyGems prior to
version 1.2. Make sure RubyGems is up to date with the command `gem update
--system` as instructed earlier.

Using MarkUs with MySQL
================================================================================

Only follow these steps if you want to use MySQL (included in InstantRails)
instead of Postgres.

 * From this folder, run `mysql --user root`
 * Once you're in, run `create user '<your-desired-new-user>'@'localhost' identified by '<your-desired-password>';`
 * To give yourself useful permissions: `GRANT ALL ON *.* TO 'youruser'@'localhost';`
 * Log out of root and then log yourself in: (it'll prompt you for your password) `mysql --user youruser --password`
 * Let's test our permissions:

    * run `create database test_rythmbox;` If this succeeds, we're good.
    * drop it: `drop database test_rythmbox;`

 * Now you're done with the database.


Installing MarkUs
================================================================================

Once you have the required software installed, it should be pretty
straight-forpward to install MarkUs.

In order to checkout Markus you will need to have [[subversion |
http://subversion.apache.org/]] installed on Windows.  The
standard command line client is fine, but it is also possible to integrate
subversion into the Windows shell using [[ Tortoise SVN |
http://tortoisesvn.net/]].

 * Go to the folder where you'd like to put the MarkUs source code (a folder called "markus_trunk" is recommended) and checkout the MarkUs source code.  The source code can be found at `https://stanley.cdf.toronto.edu/svn/csc49x/olm_rails/trunk`. The command to checkout is:

    `svn co https://stanley.cdf.toronto.edu/svn/csc49x/olm_rails/trunk markus_trunk`

 * If you are using Rails > 2.3.2 comment out the line containing
    `RAILS_GEM_VERSION="2.3.2"` in config/environment.rb

 * You need to select the proper database connection settings file to match the database you are using.  To do this, you must copy the appropriate file in the `config` folder to `database.yml`:

      If using PostgreSQL::

        cp config/database.yml.postgresql config/database.yml

      If using MySQL::

        cp config/database.yml.mysql config/database.yml

 * Edit config/database.yml and be sure that:

      - "development" section is uncommented
      - username/password is the same as the one used for mysql/PostgreSQL install

 * Create the database for MarkUs (do these steps in the ruby console
   window)::
      `rake db:create`

If you have a problem executing the above command then it is likely that there is something wrong with the file `database.yml` that you just edited.

 * Install other gems required for Markus development:
      - `gem install ruby-debug`

Note: use `gem install ruby-debug -v 0.9.3` to install an earlier version of ruby-debug if the latest version will not install.
      - `gem install fastercsv`
      - `gem install selenium_client`
      - `gem install throughbot-shoulda`
      - `gem install will_paginate`
      - `gem install machinist`
      - `gem install faker`
      - `gem install factory_data_preloader`
      - `gem install rubyzip`
      - `gem install ya2yaml`

If you get a message saying "Missing these required gems", then it is likely
that some new gems have been integrated into Markus development and also need
to be installed using `gem install` as described above (or use `rake
gems:install`; Note: rake gems:install might not work correctly, because of a
rake chicken/egg problem. It seems rake requires the environment task, but
this doesn't work, because there are missing required gems. See ticket #635).
Edit this page to include them!

 * Now you need to install the Ruby/Subversion bindings. ( <http://danintouch.blogspot.com/2008/08/svn-151-ruby-bindings-on-windows.html> for original tutorial and <http://subversion.tigris.org/servlets/ProjectDocumentList?folderID=8100> for the latest downloads)

       - Download and install subversion if necessary: <http://subversion.tigris.org/files/documents/15/46906/Setup-Subversion-1.6.6.msi>
          Download and unzip: <http://subversion.tigris.org/files/documents/15/46881/svn-win32-1.6.6_rb.zip>
       - From the zip - copy ruby/lib/svn into InstantRails/ruby/lib/ruby/site_ruby/1.8/svn
       - From the zip – copy ruby/ext/svn/ext into InstantRails/ruby/lib/ruby/site_ruby/1.8/svn/ext
       - copy libeay32.dll and ssleay32.dll from your subversion 1.6 directory into InstantRails/ruby/bin
       - If you want to make sure it works, run irb from the ruby console window and test with: `require 'svn/core'`


 * Load the database schema for MarkUs:
      `rake db:schema:load`

 * I recommend that you restart your computer at this point, especially if you get a error about an invalid Win32 application when trying the next step.

 * Create an instructor (for initial login):
      `rake markus:instructor first_name="test" last_name="test" user_name="markus"`

 * If you want you can populate MarkUs with sample data:
      `rake db:populate`

 * Because Markus uses an external password validation program, authenticating
   a user externally only works on * nix platforms and not on Windows.  To
   bypass the authentication for development purposes:

      - go into app/models/user.rb
      - comment out the lines:

            if RUBY_PLATFORM =~ /(:?mswin|mingw)/
               return AUTHENTICATE_BAD_PLATFORM
            end

      - Just above the line that says `pipe = IO.popen(VALIDATE_FILE...`, add the line
            `return AUTHENTICATE_SUCCESS`

 * Change the REPOSITORY_STORAGE constant value in 'config/environment.rb' to a Windows-like value. Mine is `C:/markus-src/svn-repos-root` for example. If the MarkUs rails app is currently running, restart it since you've changed configuration.

 * Markus Installation is now done!!!

 * See if it works:

    - Start InstantRails
    - Start the servers
    - Open the Ruby console window (InstantRails black I button > Rails Applications > Open Ruby Console Window)
    - cd to the directory where you put the markus source code you checked out
    - type `ruby script/server` and wait until it is started. can take 10 to 15 seconds.
    - In your web browser, http://localhost:3000
    - use the instructor user name that you set up before (for example "markus") and any number of characters > 0 as password

 * If you see MarkUs login screen, Congratulations!! Your installation is a success!!

Optional: Install an Editor
================================================================================

There is no official IDE for rails, so you can use whichever editor you prefer.  Some options that work with Ruby and Rails include RadRails, [[NetBeans]], Komodo Edit, [jEdit](wiki:JEdit) and Wing.  Try out your own editor!
