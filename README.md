# Windows10 Dev Terminal
A guide for my friends and peers to setup the WSL environment and install Oh My Zsh, Node and other development software


## Installfest for Windows 10

### 1. Enable “Windows Subsystem for Linux” feature
Go to the Start menu and search for PowerShell. Run it as administrator:
Once you have the PowerShell running, use the command below:

	Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

You’ll be asked to confirm your choice. Type Y or press enter:
Now you should be asked to reboot. Even if you are not asked to, you must restart your system.

### 2. Download a Linux system from the Windows store
Once your system has rebooted, go to the Windows Store and search for “Linux.”
You’ll see the option to install Ubuntu or other linux flavors
I advise going for Ubuntu in this case. Suggest 18.04 LTS

### 3. Find Ubunutu in start menu
The first time it runs it will takea few minutes
It will take some time installing and then you’ll have to set up the username and password
Don’t worry, it’s just for the first run. Bash shell will be available for use directly from the next time onwards.

### 4. Install git
 	sudo apt install git

### 5. Install curl (if not installed)
	sudo apt install curl

### 6. Install ZSH
Check if you have zsh is installed:

	zsh --version

Assuming it is not installed, run the below command to install it

	sudo apt install zsh

Verify it is installed with 

	zsh --version

Make it your default shell

	chsh -s $(which zsh)


### 7. Install Oh My ZSH
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

### 8. Install NVM
	curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash


### 9. Install NODE via NVM
	nvm install node 

### 10. Install PostgreSQL
Run the following to confirm the version of postgresql you already have

    pg_lsclusters

You should see an output like the following:

    Ver Cluster Port Status Owner    Data directory              Log file
    11  main    5432 down   postgres /var/lib/postgresql/11/main /var/log/postgresql/postgresql-11-main.log

Run the following command to start the version 11 server but make sure to use sudo

    sudo pg_ctlcluster 11 main start

Running pg_lsclusters again you should see that the Status is no longer down but online

    Ver Cluster Port Status Owner    Data directory              Log file
    11  main    5432 online postgres /var/lib/postgresql/11/main /var/log/postgresql/postgr esql-11-main.log

If you try to build a new rails app and try something like rails db:create you will get an error like this:

    PG::InsufficientPrivilege: ERROR:  permission denied to create database

To fix this try the following. Use this command before you want to use psql

    sudo -u postgres -i

You will get dumped at a new prompt. Run this:

    psql

Then this. new_user should be replaced with your linux sub system username:

    ALTER USER new_user CREATEDB;

If you need to grant permissions to a specific database you can use the below:

    GRANT ALL PRIVILEGES ON  DATABASE database_name to new_user;

    ALTER DATABASE database_name owner to new_user;


### 11. Install Mongo DB
TBA
### 12. Install Ruby and Ruby on Rails
Run the following to make sure your packages are up to date:

    sudo apt update
Install the dependencies for Ruby

    sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev

Run this to make sure you can get the gem pq installed
    
    sudo apt-get install postgresql-client libpq5 libpq-dev

Confirm Ruby, RVM and RBENV are not install by running these each at the command line

    ruby

    rvm

    rbenv

Each should return something like this indicating they are not presently installed

    zsh: command not found: rvm

Install RBENV to be your Ruby Version Manager. Clone it into ~/.rbenv 

    git clone https://github.com/rbenv/rbenv.git ~/.rbenv

Update your ZSH Shell to be able to use RBENV commands

    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
and

    echo 'eval "$(rbenv init -)"' >> ~/.zshrc

Open a new terminal window and try out the following commands

    rbenv versions

It should report that you have no versions of Ruby as per below output.

    Warning: no Ruby detected on the system

Lets fix that and install Ruby 2.6.5. First lets install a plugin (ruby-build) to make installs easier

    git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

Now lets install Ruby 2.6.5 for real

    rbenv install 2.6.5

This is going to take while to complete... hang in there

Once it is done you should be able to re run the versions command from above to see 2.6.5. But now we need set it as the global default version.

    rbenv global 2.6.5

Once that is done if you again rerun the versions command you should get an output similar to the below

    * 2.6.5 (set by /home/trev/.rbenv/version)

Now we should be able to install a few useful gems. Installing the rails gem will also take a while...

    gem install pry
    gem install bundler
    gem install rails
    
Use this to make sure you can get the gem pq installed

    sudo apt-get install postgresql-client libpq5 libpq-dev

That should be it. You should not be able to run commands like: 


### X. Backup/Exporting your WSL
https://winaero.com/blog/export-import-wsl-linux-distro-windows-10/
From Powershell or plain old CMD run the following command:
	
    wsl.exe --export Ubuntu c:\ubuntu.tar

See the link for importing the backup file


## OPTIONAL FOR COOL TERMINAL (Recommended)
### 1. Download Windows Terminal from the Windows store
https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab

Edit your settings to allow copy and paste via the regular CTRL-C and CTRL-V. Add the below to the keybindings array in your settings JSON file (full file is at the bottom)

    { "command": "copy", "keys": ["ctrl+c"] },
    { "command": "paste", "keys": ["ctrl+v"] }


### 2. Download and install jetbrain mono font
https://www.jetbrains.com/lp/mono/
Go to terminal settings (from the down arrow in terminal), this will open a json file you can edit.
See my below full json file but for the font alone you can add the following line to your ubuntu terminal profile
	
    "fontFace": "JetBrains Mono",

### 3. Download and install powerlevel10k oh my zsh plugin
Run the following command to get it
	
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
then set 
	
    ZSH_THEME="powerlevel10k/powerlevel10k" 

in ~/.zshrc.



## References:
https://itsfoss.com/install-bash-on-windows/ (Steps 1, 2 and 3)

https://git-scm.com/book/en/v2/Getting-Started-Installing-Git (Step 4)

https://linuxize.com/post/how-to-install-and-use-curl-on-ubuntu-18-04/ (Step 5)

https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH (Step 6)

https://github.com/ohmyzsh/ohmyzsh (Step 7)

https://github.com/nvm-sh/nvm#installing-and-updating (Step 8)

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04 (Step 10)

https://stackoverflow.com/questions/28116927/postgres-permission-denied-to-create-database-on-rake-dbcreateall (Step 10)

https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-18-04 (Step 12)

https://github.com/sirredbeard/Awesome-WSL (Collection of WSL related links)




## Cool Terminal:
https://github.com/romkatv/powerlevel10k#oh-my-zsh (Step 3)

## Cache you git hubs creds
https://help.github.com/en/github/using-git/caching-your-github-password-in-git
https://stackoverflow.com/questions/35942754/how-to-save-username-and-password-in-git-gitextension



## The below is my settins json file for terminal


    // To view the default settings, hold "alt" while clicking on the "Settings" button.
    // For documentation on these settings, see: https://aka.ms/terminal-documentation

    {
        "$schema": "https://aka.ms/terminal-profiles-schema",

        "defaultProfile": "{2c4de342-38b7-51cf-b940-2309a097f518}",

        "profiles":
        {
            "defaults":
            {
                // Put settings here that you want to apply to all profiles
            },
            "list":
            [
                {
                    // Make changes here to the powershell.exe profile
                    "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                    "name": "Windows PowerShell",
                    "commandline": "powershell.exe",
                    "hidden": false
                },
                {
                    // Make changes here to the cmd.exe profile
                    "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                    "name": "cmd",
                    "commandline": "cmd.exe",
                    "hidden": false
                },
                {
                    "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
                    "hidden": false,
                    "name": "Ubuntu",
                    "source": "Windows.Terminal.Wsl",
                    "fontFace": "JetBrains Mono",
                    "fontSize": 18,
                    "colorScheme": "Monokai",
                    //"colorScheme": "Solarized Dark",
                    "useAcrylic": true,
                    "historySize": 9001,
                    "icon": "ms-appdata:///roaming/terminalicon.png",
                    "padding": "8, 8, 8, 8",
                    "startingDirectory":"c:\\Users\\Trev\\Desktop"
                },
                {
                    "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                    "hidden": false,
                    "name": "Azure Cloud Shell",
                    "source": "Windows.Terminal.Azure"
                }
            ]
        },

        // Add custom color schemes to this array
        "schemes": [
            {
                "name": "Monokai",
                "foreground": "#f1ebeb",
                "background": "#272822",
                "black": "#48483e",
                "red": "#dc2566",
                "green": "#8fc029",
                "yellow": "#d4c96e",
                "blue": "#5555ff",
                "purple": "#9358fe",
                "cyan": "#56b7a5",
                "white": "#acada1",
                "brightBlack": "#76715e",
                "brightRed": "#fa2772",
                "brightGreen": "#a7e22e",
                "brightYellow": "#e7db75",
                "brightBlue": "#66d9ee",
                "brightPurple": "#ae82ff",
                "brightCyan": "#66efd5",
                "brightWhite": "#cfd0c2"
            }],

        // Add any keybinding overrides to this array.
        // To unbind a default keybinding, set the command to "unbound"
        "keybindings": [
        { "command": "copy", "keys": ["ctrl+c"] },
        { "command": "paste", "keys": ["ctrl+v"] }]
    }


