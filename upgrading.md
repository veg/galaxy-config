## Working notes on upgrading a galaxy instance to a newer version of Galaxy
There is very good documentation from the galaxyproject team on upgrading an instance (some links listed below); these notes are just from my experience upgrading our development instance from 18.09 to 19.01
### links:
 - https://galaxyproject.github.io/training-material/topics/admin/tutorials/upgrading/slides.html#1
 - https://galaxyproject.org/admin/maintenance/

### Basic Steps:
1. Backup
    - Save backup of database files `database/files/000`
    - Save backup of database itself `pg_dump dbname > dumpfile` (https://www.postgresql.org/docs/current/backup.html)
    - Make sure the current state of the code base is tracked in git
    - As an optional extra safety step, copy the whole directory somewhere to back-up things that are git-ignored
2. Prep For Upgrade
    - Redirect the URL to a maintenance page
    - Stop the galaxy process `sh run.sh —stop-daemon`
3. Upgrade
    -  Upgrade with git: The specifics may not always be the same… be conscious of what changes you have, what changes you want, which branch you’re own, which remotes you have, etc.
    -  Make sure all the configuration is correct for our instance (see https://github.com/veg/galaxy-config)
    -  Update the database with the script `sh manage_db.sh upgrade -c $GALAXY_CONFIG_FILE` (this can’t be done prior to upgrading the codebase... you need the new script)
    -  Restart Galaxy
    -  Test “locally”
    -  If everything is working, redirect the URL from the maintenance page to the site.

When I upgraded from 18.09 to 19.01 I had issues where the tool pane did not show our tools (even though the tools were installed). I unistalled the tools through the admin pane; reinstalled the tools with ephemeris and restarted but then _only_ our tools showed up and no other tools (i.e. no get data, etc.). I tried removing `integrated_tool_panel.xml` and `config/shed_tools_config.xml` and redoing the steps in various orders but couldn't figure out what was going on. I ended up simply removing the whole repo, cloning a fresh repo and starting over again... this worked fine.