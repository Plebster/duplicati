# Duplicati Test

This will create a docker container running Duplicati and a container running a webserver that serves a simple webpage. We will backup that web page and then look at restoring it following various failure scenarios.
## Create Backup Test
### Pull git repo
    git clone https://github.com/Plebster/duplicati.git
### Start docker compose
    cd duplicati
    docker-compose up -d
### Create index.html
    echo "<html><body><h1>Hello Dave</h1><h1>Hello HAL</h1></body></html>" > html/index.html
    
OR if you need 'sudo'

    echo "<html><body><h1>Hello Dave</h1><h1>Hello HAL</h1></body></html>" | sudo tee -a html/index.html

We now have a docker container running Duplicati (on http://localhost:8200/ngax/index.html), and another running a webserver that hosts a single page (that we just made). You should be able to see this at http://localhost/

### Setup a Backup
We're going to backup the index.html file we just made and make sure we can recover it.

 - http://localhost:8200/ngax/index.html
 - select "No, my machine has only a single account"
 - select "Add Backup"
 - select "Configure New Backup"
 - select "Next"
 - Fill "General backup settings" (no encryption)
 - select "Next"
 - Backup destination is Computer/backups
 - select "Next"
 - Source location is Computer/source
 - select "Next"
 - Schedule
 - select "Next"
 - General options Backup retention select "Smart backup retention"
 - select "Save"
### Run the Backup Now
- select  "Run Now" 

## Backup the Backup
We save a copy of the configuration details of the Backup, and we take an 'off-site' backup of the backed up files - in case we lost the server completely.

### Export Config
- click the arrow next to backup name
- select "Export" in the configuration section
- Options: to file and export files
- click "Export"
- click "Yes, I understand the risk"
- save file to computer
### Copy Back-up files
(In practice this could be off-site, but for now, just put it somewhere on your machine outside of the docker files)

     cp -R ./duplicati/backups <location>

# Recovery Scenarios
## 1. Local Folder Failure Restore
### Stop lighttpd and Delete/Recreate folder
    docker stop lighttpd
    rm -r html
    mkdir html
### Restore Files
 - http://localhost:8200/ngax/index.html
- click the arrow next to backup name
- select "Restore Files" in the operstions section
- select tick box in file browser
- click "Continue"
- select tick box for Restore read/write permissions
- click "Restore"
### Restart HTML server
    docker-compose up -d

Check your files are back at http://loclahost

## 2. Recreate From Backup Complete Failure
Remove everything (remembering that we have our 'jobs' config and backup files safe elsewhere)

    docker-compose up -d
    cd ..
    sudo rm -rf duplicati

### Pull git repo
Now, we recreate everything (Duplicati and our webserver application) from scratch

    git clone https://github.com/Plebster/duplicati.git
### Start docker compose
    cd duplicati
    docker-compose up -d
    # Kill the webserver for some reason?
    docker stop lighttpd
### Recreate the Backup job in Duplicati
 - http://localhost:8200/ngax/index.html
 - select "No, my machine has only a single account"
 - select "Add Backup"
 - select "Import From a File"
 - select "import metadata"
 - select "save immediately"
 - select "Import"
### Copy our Backup Files into the right place
    cp <location>/* ./duplicati/backups
### Restore our files from the Backup 
 - http://localhost:8200/ngax/index.html
 - select "Restore"
 - select Direct restore from backup files
 - select "Next"
 - select Computer/backups in file browser
 - select "Next"
 - select "Connect"
 - select files to restore in file browser
 - select "Connect"
 - select tick box in Restore read/write permissions
 - select "Restore"
### Restart HTML server
    docker-compose up -d

Check your files are back at http://loclahost
