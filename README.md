## Configuring a new HIV-NGS galaxy instance 

The instructions below were tested on silverback as a newly created user "galaxydev" for starting dev.galaxy.hyphy.org

1. Basic Install and Config
2. Setup Database
3. Add Tools
4. Add Workflows
5. Connect to Cluster


### 1. Basic Install and Config
`git clone http://github.com/veg/galaxy`   
`cd galaxy`  
`git checkout <desired release branch>` in this case “release_18.09”    
`cp <location of desired galaxy.yml> ./config/galaxy.yml`  
`cp <location of desired job_conf.xml> ./config/job_conf.xml`  
`cp <location of desired welcome.html> ./static/welcome.html`  
Uncomment the test tool shed in `./config/tool_sheds_conf.xml`  

### 2. Setup Database
__setup the postgres database:__  
For galaxy, the name of the postgres database, database user and schema should all be the same as the linux user that is running galaxy; in this example the linux user was "galaxydev". "galaxydev" would be replaced with "galaxy" for production instances.  
`sudo su - postgres`  
`psql` This gets you into the postgres interface/repl... the below command are typed into this interface:  
`CREATE USER galaxydev PASSWORD ‘<chose a pasword>’;`  
`CREATE SCHEMA galaxydev;`  
`GRANT ALL ON ALL TABLES IN SCHEMA galaxydev to galaxydev;`  
`CREATE DATABASE galaxydev;` 

__Modify the Config File:__  
The line in the `config/galaxy.yml` file that lists `database_connection` should be: `database_connection: postgresql:///galaxydev?host=/var/run/postgresql` (again "galaxydev" in this example would be just "galaxy" in production).

### 3. Add Tools (use [Ephemeris](https://ephemeris.readthedocs.io/en/latest/index.html))
Note: you may need to copy the correct section tags from `integrated_tool_panel.xml` before installing tools to ensure that the tools go to the correct sections in the tool pane.
`shed-tools install -g http://dev.galaxy.hyphy.org -u <userEmail> -p <password> -a <apiKey> -t <location of tool_list.yml>`

The `tool_list.yml` file can be generated using the ephemeris `get-tool-list` command


### 4. Add Workflows
The ephemeris command (`workflow-install`) wasn't working but they are easy to just upload by hand as long as you have the *.ga files (saved in the `workflows\` directory of this repo).


### 5. Connect to Cluster (with torque)
Following the instructinos in the galaxy docs: https://docs.galaxyproject.org/en/latest/admin/cluster.htmlhttps://docs.galaxyproject.org/en/latest/admin/cluster.htmlhttps://docs.galaxyproject.org/en/latest/admin/cluster.html

The `job_conf.xml` file specifies configuration options specific to torque (this file was coppied in step 1).  
As described in the docs, connecting galaxy to torque requires pbs_python which should be installed like so:
```
galaxy_user@galaxy_server% git clone https://github.com/ehiggs/pbs-python
galaxy_user@galaxy_server% cd pbs-python
galaxy_user@galaxy_server% source /clusterfs/galaxy/galaxy-app/.venv/bin/activate
galaxy_user@galaxy_server% python setup.py install
```
