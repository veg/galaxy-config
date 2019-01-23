## Configuring a new HIV-NGS galaxy instance 

The instructions below were tested on silverback as a newly created user "galaxydev" for starting dev.galaxy.hyphy.org

### Basic Install and config
`git clone http://github.com/veg/galaxy`   
`cd galaxy`  
`git checkout <desired release branch>` in this case “release_18.09”    
`cp <location of desired galaxy.yml> ./config/galaxy.yml`  
`cp <location of desired welcome.html> ./static/welcome.html`  
Uncomment the test tool shed in `./config/tool_sheds_conf.xml`  

### Database
__setup the postgres database:__  
For galaxy, the postgres database, database user and schema should all be the same as the linux user that is running galaxy; in this example the linux user was "galaxydev". "galaxydev" would be replaced with "galaxy" for production instances.  
`sudo su - postgres`  
`psql` This gets you into the postgres interface/relp... the below command are typed into this interface:  
`CREATE USER galaxydev PASSWORD ‘<chose a pasword>’;`  
`CREATE SCHEMA galaxydev;`  
`GRANT ALL ON ALL TABLES IN SCHEMA galaxydev to galaxydev;`  
`CREATE DATABASE galaxydev;` 

__Modify the Config File:__  
the line in the `config/galaxy.yml` file that lists `database_connection` should be: `database_connection: postgresql:///galaxydev?host=/var/run/postgresql` (again "galaxydev" in this example would be just "galaxy" in production)

### Add Tools (use [Ephemeris](https://ephemeris.readthedocs.io/en/latest/index.html))
`shed-tools install -g http://dev.galaxy.hyphy.org -u <userEmail> -p <password> -a <apiKey> -t <location of tool_list.yml>`

The `tool_list.yml` file can be generated using the ephemeris `get-tool-list` command


### Workflows
The ephemeris command (`workflow-install`) wasn't working but they are easy to just upload by hand as long as you have the *.ga files (saved in the `workflows\` directory of this repo.

## TODO:

### Cluster Connection