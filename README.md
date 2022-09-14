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
### Stop Docker-Compose and Delete folder
    docker-compose down
    cd ..
    sudo rm -r duplicati
