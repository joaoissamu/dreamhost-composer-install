Installing Composer on a shared Dreamhost Server
==========================
Some people have had a hell of a time installing Composer on a shared DH account, well here's how I did it. 
I'm going to assume you know what a shell user is and how to use basic terminal

-----------------------

Requirements
-------------------------
1. A shared Dreamhost account
2. Make sure your user is sftp (shell access)
3. I suggest updating your domain that your installing composer on to run PHP 7.2 w/ FastCGI
4. Wait 5-10 minutes for everything to move to 7.2 (if your not there already)


Step 1: Running PHP 7.x in CLI (Command Line Interface)
-------------------------

1. Login via SSH (if you are not logged in already)
2. Navigate to ~/ (`$ cd ~/`)
3. Create or edit the existing file (`$ vi .bash_profile`)
4. On a new line copy and paste this code
```
#.bash_profile
...
export PATH=/usr/local/php72/bin:$PATH
```
5. Save the file and exit (CTRL+O then CTRL+X)
6. Run source .bash_profile (`$ source .bash_profile`) to load file configuration to current session
7. Run php -v to (`$ php -v`) confim PHP CLI version

It should look something like this:
```
PHP 7.2.11 (cli) (built: Oct 15 2018 20:26:47) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.11, Copyright (c) 1999-2018, by Zend Technologies
```
8. **If you've decided to run PHP 7.0 instead of PHP 7.2 change *php72* to *php70* in the path**

Step 2: Enable the Phar Extension
-------------------------

1. Login via Putty or whatever flavor you like
2. Navigate to ~/ (`$ cd ~/`)
3. Create the folder ~/.php (`$ mkdir -p ~/.php/7.2`), if folder exists, go to next step
4. Create the file ~./.php/7.2/phprc (`$ nano ~/.php/7.2/phprc`), if file exists, go to next step
5. Pop these jewels into the file on seperate lines
```
#phprc
...
extension = phar.so
suhosin.executor.include.whitelist = phar
memory_limit = 512M
upload_max_filesize = 100M
post_max_size = 105M
max_execution_time = 500
max_input_time = 500
```
6. Save the file and exit (CTRL+O then CTRL+X)
7. Confirm Phar running command php -m | grep Phar (`$ php -m | grep Phar`)
It should look something like this:
> Phar
8. **If you've decided to run PHP 7.0 instead of PHP 7.2 change *7.2* to *7.0* in the path**


Step 3: Hurray!
-------------------------
1. After you've completed steps 1 and 2, create the folder ~/.php/composer (`$ mkdir -p ~/.php/composer`)
2. Navigate to directory ~/.php/composer (`$ cd ~/.php/composer`)
3. Install composer with this command: curl -sS https://getcomposer.org/installer | php (`$ curl -sS https://getcomposer.org/installer | php`)
It should look something like this:
```
All settings correct for using Composer
Downloading...

Composer (version 1.7.2) successfully installed to: /home/fringe_prx/.php/composer/composer.phar
Use it: php composer.phar
```
4. Rename executable: mv ~/.php/composer/composer.phar ~/.php/composer/composer (`$ mv ~/.php/composer/composer.phar ~/.php/composer/composer`)
5. Add composer to $PATH: Edit the existing file ~/.bash_profile (`$ nano ~/.bash_profile`) 
6. On a new line copy and paste this code
> export PATH=~/.php/composer:$PATH
7. Save the file and exit (CTRL+O then CTRL+X)
8. Run source .bash_profile (`$ source .bash_profile`) to load file configuration to current session
9. Confirm composer version: ($ composer)
It should look something like this:
```
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.7.2 2018-08-16 16:57:12

Usage:
  command [options] [arguments]
...
```

10. I'll assume from here on our you can read the composer documentation and usage :)
