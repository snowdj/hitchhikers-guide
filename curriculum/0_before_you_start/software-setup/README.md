# Software Setup Session

## Motivation

Every team will settle on a specific setup with their tech mentors. This setup will determine, for example:
- which Python version to use
- versioning via virtual environments
- maintaining package dependencies
- continuous testing tools
- your version control workflow.

Today, we're **not** doing that. We're making sure that everybody has some basic tools they will need for the tutorials and the beginning of the fellowship, and that you can log into the server and database.

### Format

Work through the prerequisites below, making sure that all the software you installed works.

Affix three kinds of post-it notes to your laptop:
- one with your operating system, e.g. 'Ubuntu 14.04'
- if you get an app working (e.g. ssh), write its name on a **green** post-it and stick it to your screen
- if you tried - but the app failed - write its name on a **red** post-it

If you're stuck with a step, ask a person with a corresponding green post-it (and preferrably your operating system) for help.

The tech mentors will hover around and help with red stickers.

You will need a few credentials for training accounts. We'll post them up front.

**Important**: The notes below aren't self-explanatory and will not cover all (or even the majority) of errors you might encounter. Make good use of the people in the room!

Let's get this over with!

## Getting a terminal environment on a Windows Computer:

You're going to want to access the terminal. Unfortunately, windows computers don't have this by default. Fortunately, there are a couple options for obtaining a terminal-like environment, such as the following:

- [git-bash](http://www.techoism.com/how-to-install-git-bash-on-windows/)
- [Cygwin](https://cygwin.com/install.html)
    - If you go with cygwin, make sure to choose all git packages when you're in the package menu portion of the setup

If you're a windows user, make sure to download one of these.

## Package Manager

A package manager will make your life easier.
- on Mac, install [Brew](http://brew.sh/)
    - **Testing**: To check that it installed, run the command `which brew` in the terminal. If it returns: `/usr/local/bin/brew`, it means that homebrew is installed; if it returns `brew not found`, it means homebrew is not installed.
- on Linux, you probably already have yum or apt
    - **Testing**: run `yum help`
- ask Windows users around you for their preferred way to manage packages. And tell us, so we can add them here!

## Git and GitHub Account

1. If you don't have a GitHub account, [make one](https://github.com/join?source=header-home)!

2. Go to [this site](https://dssg-github-invite.herokuapp.com/), input your username, and click "Add me to organization". Your username will be automatically added to the DSSG organization on GitHub.

3. Install Git

**Mac**: 

 In the terminal, type:

 ```
 brew update
 brew install git
 ```
 
 **Linux**: 
 
 (^^ similar with `yum` or `apt`). 
 
 **Windows**:
 
On Windows, you should already have git. (Either you installed **git-bash**, which is part of git, or you should have downloaded git in the **cygwin** package menu.)

4. Test your installation. For example, create a directory, and make it a git repo:
 ```
 mkdir mytestdir
 cd mytestdir/
 git init
 ```
 ```
 > Initialized empty Git repository in [...]/mytestdir/.git/
 ```
You can un-git the directory by deleting the `.git` folder: `rm -r .git` (or simply delete `mytestdir` entirely with command `rmdir mytestdir`).

## Python

As said, your team will decide on which Python version (and versioning) to install. Thus, if you have any working setup already, don't break it (for now)! Just make sure you have the packages listed below installed.

1. If you have no Python installed at all, [Anaconda](https://www.continuum.io/downloads) makes installing a lot of packages easy, and it comes with `jupyter notebook`. Go for Python 3.6.

2. If you have a working Python installation, but you don't have `jupyter notebook`, [install jupyter](https://jupyter.readthedocs.io/en/latest/install.html).

3. Make sure you have the following packages installed. If you use Anaconda, most of them will already be included. Simply call `conda install <packagename>`. Outside of conda, you might want to use `pip` to install packages instead. (Linux users might need to install the packages `libfreetype6-dev` and `libpng-dev` before installing the packages below.) You'll need:
  *   numpy
  *   scipy
  *   pandas
  *   matplotlib
  *   seaborn
  *   scikit-learn
  *   psycopg2
  *   statsmodels
  *   csvkit

4. It's time to test! In order to test that both jupyter and the python packages installed appropriately, you should do the following:

- Follow [this link](https://drive.google.com/drive/folders/1PtA4Io49v25TRRibtJdpfwCLnCYeJTzZ?usp=sharing) and download the `SoftwareSetup.ipynb`. You can leave the file in your downloads, or move it to whatever folder you like.
- In the terminal, navigate to the folder with the `SoftwareSetup.ipynb` file.
- In the terminal, type `jupyter notebook` and hit enter. This should launch a jupyter notebook tab in your browser. The page should look something like this:
<img src="imgs/jupyter.png" width="900px;"/>
- Click on `SoftwareSetup.ipynb` to open the notebook
- Follow the instructions in the notebook to run each cell.

## SSH / Putty

1. Use your username and server's address to ssh into the server:
 ```
 ssh yourusername@serverurl
 ```
 Once you enter your password, you should be dropped into a shell on the server:
 ```
 > jsmith@servername: ~$
 ```

## PSQL

The database server runs Postgres 9.5.10.

#### Windows users: 

Windows users should skip the steps below, and instead use [DBeaver](http://dbeaver.jkiss.org/). When setting up the connection in DBeaver, you will need to specify the SSH tunnel; the database credentials are the ones we shared with you, and the SSH tunnel credentials are the ones you used in the previous step to SSH into the training server. Alternatively, everybody can access `psql` from the training server: SSH into the training server as in the step before, then, on the server's shell, call `psql -h POSTGRESURL -U USERNAME -d DBNAME`, where you need to substitute `POSTGRESURL` with the postgres server's address, `USERNAME` with your database username, and `DBNAME` with the name of the database.

For Windows - or if you want a graphical interface to databases - you might  want to use [DBeaver](http://dbeaver.jkiss.org/).

#### Non-Windows Users:

For all non-Windows users, also do these steps to access the Postgres server from your local machine:

1. Make sure you have the `psql` client installed; on Mac, this would be
 ```
 brew install postgresql
 ```
 On Ubuntu: ```apt-get install postgresql-client-9.4```. This won't work for  Ubuntu <= 14.4, which by default only packages 9.3; in that case follow [these  instructions](https://www.postgresql.org/download/linux/ubuntu/). (Though the  older client seems to be happy to connect to the server, too.)

 Some Linux distributions might need `libpq`:
 ```
 sudo apt-get install libpq-dev
 ```
 or
 ```
 sudo yum install libpq-devel
 ```
2. Once you have the postgres client installed, you can access the training database with it. However, the database server only allows access from the training server. Thus, you need to set up an SSH tunnel through the training server to the Postgres server:
 ```
 ssh -NL localhost:8888:POSTGRESURL:5432 ec2username@EC2URL
 ```
 where you need to substitute `POSTGRESURL`, `ec2username`, and `EC2URL` with the postgres server's URL, your username on the training server, and the training server's URL respectively. Also, you should substitute `8888` with a random number in the 8000-65000 range of your choice (port `8888` might be in use already). 
 
 This command forwards your laptop's port 8888 through your account on the EC2 (EC2URL) to the Postgres server port 5432. So if you access your local port 8888 in the next step, you get forwarded to the Postgres server's port 5432 - but from the Postgres server's view, the traffic is now coming from the training server (instead of your laptop), and the training server is the only IP address that is allowed to access the postgres server.

3. Connect to the Postgres database on the forwarded port
 ```
 psql -h localhost -p 8888 -U USERNAME -d DBNAME
 ```
 where you need to replace `USERNAME` with the postgres [!] username, `DBNAME` with the name of your database, and the `8888` with the number you chose in the previous step. You then get prompted for a password. This is now the postgres server asking, so you need to reply with the corresponding password!

 This should drop you into a SQL shell on the database server.
 
You'll need to explicitly assume a role to do anything beyond connecting to the database. To make changes to the training database, use the `training_write` role. Let's test it by creating and dropping a schema:
 ```
 set role training_write;
 create schema jsmith;
 drop schema jsmith;
 ```

Note: After installing Postgres, you might have to add Postgres to your `PATH` (at least on OS X). You'd do this by adding a line to your `.bashrc`, similar to `export PATH=$PATH:/path/to/your/postgres/binaries`, and then re-loading your `.bashrc` file: `source ~/.bashrc`
