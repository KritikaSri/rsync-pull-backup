# Readme under construction!

## rsync-pull-backup

## Intention 

The scripts have been designed for server based time machine like backup of user data/documents. They are NOT designed for a full system backup. For this purpose other tools exist.

The scripts should work with all clients which support a ssh server.

Backups are important - but creating them takes time. I've been looking for a way how I can automate my backups and don't need to think about them anymore after setup. Nice would also be to have a time line of backups so I can restore a single, accidently changed/deleted/... file without the need to restore from an archive system. 

Next request has been to make it save so it cannot be changed on/from my local systems. Because I have a NAS (in this case a BananaPi with OMV) the idea came up to start the backups from the NAS using rsync. This script collection is the result.

## Content/Scripts
The collection contains three scripts:
* AddToBackupPipeline.sh: Adds a backup request to a queue
* rsyncPollScript.sh: Processes all requests from the queue
* checkLogfile.sh: Checks the log files of the backups for errors. Because the OMV crontab can be easily configured to send a mail notification on cron job outputs there is no mail component in the script.
 
All scripts are started by crontab on the NAS so nothing needs to configure anything on the clients except of the ssh server (and also the scripts/configurations of the backup cannot be changed by a malware).

**Be aware that a ssh server is a potential risk.** To make it more secure I disabled password login on the clients sshd configuration and changed the owner of the authorized_keys file of the user to root so nobody can access the PC without an interaction of somebody with root access. At least for me this is save enough. 

## Features
* Copy secured by ssh connection
* Each source system has its own configuration (and might also have multiple)
* batch or single mode
* Processing can be stopped by setting a stop file.
* Basic check for changed file types after a backup. If the type of a file e.g changes from text to something else the backup will not be integrated into the time line.
* Automatic aging/deleting of older backups. Each backup job may have its own max. age.
* Each backup job may have its own exclude file list
* For each backup job you can set different user rights
* Script does not neeed to run as root on the server
* If the client is not available the script postpones the backup until the client is back again. No folders in the backup time line will be created.
* Detailed logging of all actions and changed files

## Requirements
* nc needs to be installed on the server



## Content/Scripts


## Usage
### Installation

* Copy the sh files to i.e. /usr/local/bin
* Create a user (should be a dedicated user) for backups
* Create a root folder for backups and make the change the owner to the backup user 
* Create a sub folder conf for the configuration files
* Create a sub folder .ssh for key files
* Create a key pair in the .ssh folder

All other needed folders will be created by the scripts on demand/first usage.

Make sure you can reach the client with ssh and shared key authentication.

## Configuration

Check the EXAMPLE.conf file for all and min-EXAMPLE.conf for a min. set of options.
