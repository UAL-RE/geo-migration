# UAL Geoportal Migration Project

**PRIVATE** DO NOT MAKE PUBLIC

This repo documents processes taken to migrate UAL spatial data collection to new server (CEREUS) a CyVerse managed server housed at UITS.

# background
This project was initiated to move spatial data off UAL servers (sequoia, geo, imagery) that have reached EOL. Migrating data is the first step in decomissioning these servers and sunsetting the Spatial Data Explorer (opengeoportal) web application. CEREUS was purchased through a TRIF-WEES grant in collaboration between T. Swetnam, D. LeBauer, K. Carini and others(?) to house spatial data for campus in a location that makes these data readily available for cloud-based, intensive analysis.

**TODO**: Provide a high-level description of the process, only needs to be two to three sentences

**TODO**: Add outline of the steps necessary to complete the task. One implementation of this might be:

1. Dependencies
2. Setup
3. Migration
4. QA/QC
5. Miscellaneous debris

The remainder of this document should follow this enumeration, using the same headings.

1. Dependencies should be any software than needs to be installed locally or on the server, in order for this to work. Should probably just be a list.
2. Setup should be things like setting up IRODS on the server and the directory structure of cyverse (there is a line about this in the **iCommands transfers...** section that would move to **Setup**).
3. Migration would be the step-by-step process, starting up tmux, logging into the server, and getting things going. This will likely be the largest part of the documentation and cover the process starting at tmux, logging into IRODS, and running `iput`.
4. QA/QC includes methods of assessing progress and success/failure of transfers; much of the information in the **Sync** section would go here
5. Miscellaneous debris doesn't need to be called this, but is a place for anything that might be relevant but might not explicitly be part of the MVP of the migration process.





# tmux  
Terminal multiplexing (tmux) ensures that if the connection to the server is lost, the terminal window does not die. **TODO**: Does this mean the _process_ doesn't die?

**TODO**: Add steps necessary to get terminal multiplexing up and running, i.e.
provide a code block that one might use such as:
```
# list open windows
tmux ls
# attaches to window "0"
tmux attach -t 0
```

To detach from a tmux window, hold your Ctrl + B buttons together, release Ctrl then alone press D

# IRODS setup on `sequoia`
**TODO**: Provide example output of `ils` when user is _not_ icommands authenticated and an example of `ils` when user is authenticated to use icommands
Steps
* log into `sequoia`
* change to the `irods` user: `sudo su irods`
* check icommands: `ils`
* if icommands is not authenticated to your user profile, run `iinit`
* run `ils` again to make sure it worked

# iCommands transfers to CyVerse

After you've authenticated through iRODS search for a folder path that you want to move

When you run the command, it is best practice to use the full path and not just a `~/`

```
iput -KPbvrf /sequioa/UAL_Vault/GeoArchive/public/<directory-to-be-copied/ /iplant/home/shared/ual/
```

`iput` moves the file from your server to CyVerse, `iget` downloads a file from CyVerse to your server.

`-K` is for Checksum of the file to make sure it transfered with no errors
`-P` is for progressive download information used with verbose
`-b` is for bulk transfer
`-r` is for recursive folder creation
`-v` is for verbose
`-f` is to force the overwrite

We have created the CyVerse user space `ual` which is in the Community Released Folders, its full path is `/iplant/home/shared/ual`

# Sync


To check that everything transferred properly run `irsync` [(read more)](https://docs.irods.org/master/icommands/user/#irsync) which "synchronizes recursively the data from the local directory foo1 to the iRODS collection foo2 and the command"

Example:

`irsync -rKv -R cereusRes /PAGimagery/PAG/ i:/iplant/home/shared/ual/public/PAG/files/`

This will fix any files that incorrectly transferred and transfer any files that weren't.
