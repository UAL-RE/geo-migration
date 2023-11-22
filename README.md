# UAL Geoportal Migration Project

This repo documents processes taken to migrate UAL spatial data collection to 
new server (CEREUS) a CyVerse managed server housed at UITS.

# background
This project was initiated to move spatial data off UAL servers (sequoia, geo, 
imagery) that have reached EOL. Migrating data is the first step in 
decommissioning these servers and sunsetting the Spatial Data Explorer 
(opengeoportal) web application. CEREUS was purchased through a TRIF-WEES grant 
in collaboration between T. Swetnam, D. LeBauer, K. Carini and others to 
house spatial data for campus in a location that makes these data readily 
available for cloud-based, intensive analysis.

Data migration was conducted through server to server transfer using iRODS 
icommands.

# initial setup

1. Dependencies
2. Setup
3. Migration
4. QA/QC
5. Miscellaneous debris

# tmux  

Terminal multiplexing (`tmux`) ensures that if the connection to server is 
lost, the local terminal window does not die.

## common commands

`tmux` opens a tmux window`

`tmux ls` lists open windows

`tmux attach -t 0` attaches to the window `0`

To detach from a tmux window, hold Ctrl + B buttons together, release Ctrl then 
alone press D

# IRODS setup on sequoia

Steps

* log into sequoia
* change to the `irods` user: `sudo su irods`
* check icommands: `ils`
* if icommands is not authenticated to your user profile, run `iinit`
* run `ils` again to make sure it worked

# iCommands transfers to CyVerse

After authenticating through iRODS, search for a folder path to move

When possible, use absolute, not relative, paths, e.g. 


`iput -KPbvrf /sequioa/UAL_Vault/GeoArchive/public/<directory-to-be-copied/ /iplant/home/shared/ual/`

`iput` moves the file from local to CyVerse

`iget` downloads a file from CyVerse to local

## `iput` flags

* `-K` is for Checksum of the file to make sure it transfered with no errors
* `-P` is for progressive download information used with verbose
* `-b` is for bulk transfer
* `-r` is for recursive folder creation
* `-v` is for verbose
* `-f` is to force the overwrite

The CyVerse user space `ual` is in the Community Released Folders, its full 
path is `/iplant/home/shared/ual`

# Sync

To check that everything transferred properly run `irsync` 
[(read more)](https://docs.irods.org/master/icommands/user/#irsync) which 
"synchronizes recursively the data from the local directory foo1 to the iRODS 
collection foo2 and the command"

Example:

`irsync -rKv -R cereusRes /PAGimagery/PAG/ i:/iplant/home/shared/ual/public/PAG/files/`

This will fix any files that incorrectly transferred and transfer any files 
that did not transfer for whatever reason. 
