# UAL Geoportal Migration Project

**PRIVATE** DO NOT MAKE PUBLIC

This repo documents processes taken to migrate UAL spatial data collection to new server (CEREUS) a CyVerse managed server housed at UITS.

# background
This project was initiated to move spatial data off UAL servers (sequoia, geo, imagery) that have reached EOL. Migrating data is the first step in decomissioning these servers and sunsetting the Spatial Data Explorer (opengeoportal) web application. CEREUS was purchased through a TRIF-WEES grant in collaboration between T. Swetnam, D. LeBauer, K. Carini and others(?) to house spatial data for campus in a location that makes these data readily available for cloud-based, intensive analysis. 

Data migration is conducted through server to server transfer using ????. 

# initial setup




# tmux  

Always use terminal multiplexing (`tmux`) to ensure that if you lose connection to your server, your terminal window does not die.

`tmux` opens a tmux window`

`tmux ls` lists your open windows

`tmux attach -t 0` attaches you to the window `0` 

To detach from a tmux window, hold your Ctrl + B buttons together, release Ctrl then alone press D

# IRODS setup on `sequoia`

Steps
* log into `sequoia`
* change to the `irods` user: ```sudo su irods```
* check icommands: ```ils```
* if icommands is not authenticated to your user profile, run ```iinit```
* run ```ils``` again to make sure it worked

# iCommands transfers to CyVerse

After you've authenticated through iRODS search for a folder path that you want to move

When you run the command, it is best practice to use the full path and not just a `~/` 

```iput -KPbvrf /sequioa/UAL_Vault/GeoArchive/public/<directory-to-be-copied/ /iplant/home/shared/ual/```

`iput` moves the file from your server to CyVerse, `iget` downloads a file from CyVerse to your server.

`-K` is for Checksum of the file to make sure it transfered with no errors
`-P` is for progressive download information used with verbose
`-b` is for bulk transfer
`-r` is for recursive folder creation
`-v` is for verbose
`-f` is to force the overwrite

We have created the CyVerse user space `ual` which is in the Community Released Folders, its full path is `/iplant/home/shared/ual` 

# Sync
