## A capture of my efforts to install an SInfoE SP.

# Plan A - Vagrant Install

As the SInfoE repo is private, we need to use a GitHub personal access token (CLASSIC) with the appropriate scope (repo) to clone a local copy.
(https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

git clone https://GITHUB_USERNAME:GITHUB_PAT@github.com/SCP-Development/mdis-vagrant.git

The SP is provided as a complete demonstration environment within a Vagrant image, and therefore Vagrant is a prerequisite. 
(https://developer.hashicorp.com/vagrant/install)

There are several quirks with the poor HashiCorp documentation, but that's not a battle to document here. It can be done. Even the VMware plug-in.

The end of this adventure however is a virtualisation host error. Our corporate Dev laptops have secure boot enabled, and no access to the UEFI is permitted. It is not possible to sign drivers (https://knowledge.broadcom.com/external/article?legacyId=2146460) and so vmmon and vmnet cannot load.
A similar issue occurs with VirtualBox.


# Plan B - Manual Install

For my first SP installation, I selected worldmap_sp (as it's best to see something working, right.)

The first requirement is to get the code from the private GitHub repo, using a CLASSIC PAT:

git clone https://GITHUB_USERNAME:GITHUB_PAT@github.com:SCP-Development/worldmap-sp.git

The code is reliant on many other modules, and it is important to ensure all of these are also installed. 
Some are public, such as Flask, Flask-SocketIO, loguru, 
others, such as dcs, scp, scp-json-validator, scptracks, worldmap-sp, must be downloaded from GitHub.

The SP requires a config file (config.json), the path to which is passed as a command line argument.
(If running the code in VS, launch.json must be edited accordingly!)

There are a few variations of config in the examples on GitHub, but my config.json file contains:

{

  "worldmap_port": "8080",
  
  "listen_url": "http://worldmap-sp:20200",
  
  "implement": ["consumer"],
  
  "node": {
  
    "scp_version": "SCP-0.1-poc",
    
    "local_pop": "http://pop:19316",
    
    "metadata_sp": "metadataSP.json",
    
    "service_discovery": {
    
        "query_string": "rs_title == 'TracksService'",
        
        "query_interval": 5,
        
        "query_timeout": 100
        
    },
    
    "known_services_store": "~/var/known_services_store.json"
    
  },
  
  "log_level": "DEBUG",
  
  "log_file": "~/var/worldmap_sp.log",
  
  "db_path": "~/var/SubscriptionPersistence.db",
  
  "transport": "httptransport"
  
}


* NOTE:  I have changed the log and db file path from /var to ~/var. /var is owned by root and permissions to create files will be denied.
sudo allows access, but as sudo does not inherit user environment variables, the python venv will not be used and the installed modules will be unavailable.

It should now be possible to run the SP with python:

python3 sp.py config.json

* NOTE: The python code needs to be called from somewhere. If you are not calling it from elsewhere then add a run() command at the end of the listing.

* NOTE: A POP must be running or a communication error will be thrown. Also, as the SP connects using the pop hostname rather than IP address it must be resolvable. I added an entry to the system hosts file. Similarly, worldmap_sp also needs an entry.

The WorldMap SP should now be running, and browsing to http://localhost:8080 should display an interactive world map!

<br>

Having successfully installed a consumer SP, a producer SP should be easy right? Erm... 

<br>

All SPs do not use the same versions of python libraries!

Start by creating a new python venv and installing the required packages, noting you may need old versions of some packages.

New config files will be required. I will upload examples once I am sure everything is working correctly.

An entry will be required in the system hosts file with the SP name. Check the name carefully as there can be variation amongst SPs, e.g. use of - and _ within the name.


TBC.../

