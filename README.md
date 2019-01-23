##Configuring a new HIV-NGS galaxy instance 

The instructions below were tested on silverback as a newly created user "galaxydev" for starting dev.galaxy.hyphy.org

### Basic Install and config
`git clone http://github.com/veg/galaxy`   
`cd galaxy`  
`git checkout <desired release branch>` in this case “release_18.09”    
`cp <location of desired galaxy.yml> ./config/galaxy.yml`  
`cp <location of desired welcome.html> ./static/welcome.html`  
Uncomment the test tool shed in `./config/tool_sheds_conf.xml`  

### Add Tools (use [Ephemeris](https://ephemeris.readthedocs.io/en/latest/index.html))
`shed-tools install -g http://dev.galaxy.hyphy.org -u <userEmail> -p <password> -a <apiKey> -t <location of tool_list.yml>`

The `tool_list.yml` file can be generated using the ephemeris `get-tool-list` command


##TODO:
### Workflows

### Database

### Cluster Connection