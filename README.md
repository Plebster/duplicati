# Duplicati Test
## Create Backup Test
### Pull git repo
    git clone https://github.com/Plebster/duplicati.git
### Start docker compose
    cd duplicati
    docker-compose up -d
### Create index.html
    echo "<html><body><h1>Hello Dave</h1><h1>Hello HAL</h1></body></html>" > html/index.html
### Setup Backup
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
### Run Backup Now
- select  "Run Now" 
### Export Config
- click the arrow next to backup name
- select "Export" in the configuration section
- Options: to file and export files
- click "Export"
- click "Yes, I understand the risk"
- save file to computer
### Copy Back-up files
     cp -R ./duplicati/backups <location>
## Local Folder Faliure Restore
### Stop Docker-Compose and Delete/ Recreate folder
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
## Recreate From Backup Complete Failure
    docker-compose up -d
    cd ..
    sudo rm -rf duplicati
### Pull git repo
    git clone https://github.com/Plebster/duplicati.git
### Start docker compose
    cd duplicati
    docker-compose up -d
    docker stop lighttpd
### Recreate  Backup
 - http://localhost:8200/ngax/index.html
 - select "No, my machine has only a single account"
 - select "Add Backup"
 - select "Import From a File"
 - select "import metadata"
 - select "save immediately"
 - select "Import"
### Copy  Backup Files
    cp <location>/* ./duplicati/backups
### Restore  Backup 
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
### Repair Database
This may need to be done to get the backup profile to work again
