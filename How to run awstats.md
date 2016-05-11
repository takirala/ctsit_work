## How to run awstats to analyze apache logs

AWStats is a free powerful and featureful tool that generates advanced web, streaming, ftp or mail server statistics, graphically. This log analyzer works as a CGI or from command line and shows you all possible information your log contains, in few graphical web pages

### Installation of tool

1. `awstats` comes as a handy zip file which can be downloaded from the awstats download page ([here](http://www.awstats.org/#DOWNLOAD) at the time of this writing)
2. All you need to do is extract the folder to a location. (Recommended directory :  **/Library/WebServer/awstats**)
3. You should run the awstats_configure.pl script to do several setup actions.
4. The set up actions are explained in more details [here](http://www.awstats.org/docs/awstats_setup.html#INSTALL) at awstats documentation page.


### Configuration options
1. You would have created a configuration option for analyzing the logs in the above installation process.
2. In the tools directory of awstats, run the command

        perl ./awstats_configure.pl
        
3. You can follow the CLI options and created a configuration. Let's say you created a config called vivo.ufl.edu
4. You can read more on the config option from awstats docs and from this [blog article](http://www.onehippo.com/en/resources/blogs/2012/07/Generating+Reports+from+Web+Logs+with+AWStats.html)
        
        $ perl ./awstats_configure.pl
        
        Do you want to continue setup from this NON standard directory [yN] ? N
        
        -----> Need to create a new config file ?
        Do you want me to build a new AWStats config/profile file (required if first install) [y/N] ? y
        
        -----> Define config file name to create
        What is the name of your web site or profile analysis ?
        Example: www.mysite.com
        Example: demo
        Your web site, virtual server or profile name:
        > vivo.ufl.edu
        
        Press ENTER to continue... 
        
        Press ENTER to finish...

5. Installation of awstats can be integrated with apache for more advanced options.
6. In this case , we are just installing AWStats to generate reports offline from access log files without installing onto Apache Web Server for simplicity.
7. The above execution will generate the configuration into the `{AWSTATS_INSTALL_DIR}/wwwroot/cgi-bin/awstats.vivo.ufl.edu.conf` file. 

###  Config options to run the tool 
1. Download the logs from the apache to your local machine.
2. Say the local directory where the access log is located is at /home/user/logs/access.log
3. You need to make the tool aware of the location of the logs directory it is processing. 
4. This can be done by editing the configuration file created above. Open the onfiguration file located at `{AWSTATS_INSTALL_DIR}/wwwroot/cgi-bin/awstats.vivo.ufl.edu.conf`.
5. Make the below changes to this file:
        # Set the access log file path here
        LogFile="/home/user/logs/access.log"
        
        # Possible values: 1,2,3,4 or "your_own_personalized_log_format"
        LogFormat=1
        
        SiteDomain="vivo.ufl.edu"

        # Set the data directory where AWStats internal data files are stored.
        DirData="/home/user/logs/data"
6. The log file will be read from *LogFile* and the internal data will be written in to *DirData*

### Running the tool

1. Navigate to `{AWSTATS_INSTALL_DIR}/wwwroot/cgi-bin/` and run the command `perl awstats.pl -config=vivo.ufl.edu -update`. Your output should look something like this.

         takirala@mac [/Library/WebServer/awstats/wwwroot/cgi-bin]$ perl awstats.pl -config=vivo.ufl.edu -update
              Create/Update database for config "./awstats.vivo.ufl.edu.conf" by AWStats version 7.5 (build 20160301)
              From data in log file "/Users/takirala/vagrants/awstats/access.log.may1_2"...
              Phase 1 : First bypass old records, searching new record...
              Searching new records from beginning of log file...
              Phase 2 : Now process new records (Flush history on disk after 20000 hosts)...
              Jumped lines in file: 0
              Parsed lines in file: 56498
               Found 3 dropped records,
               Found 0 comments,
               Found 0 blank records,
               Found 217 corrupted records,
               Found 0 old records,
               Found 56278 new qualified records.
2. This just means that your data is ready to be processed.
3. Before running the tool, add the cgi-bin path directory to your environment. If you have followed the installation directory convention mentioned in this document, run the command:

        PATH=$PATH:/Library/WebServer/awstats/wwwroot/cgi-bin/
4. You can run the tool to generate the reports by using the command:

        perl awstats_buildstaticpages.pl -config=vivo.ufl.edu -month=all -year=2016 -dir=/home/user/logs/reports
   
   You are giving the dir argument to specify the outpur directory where the reports will be stored. Running the above command should generate an output that looks something similar to:

        takirala@mac [/Library/WebServer/awstats/wwwroot/cgi-bin]$ sudo perl awstats_buildstaticpages.pl -config=vivo.ufl.edu -month=all -year=2016 -dir=/tmp
            Build main page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output
            Build alldomains page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=alldomains
            Build allhosts page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=allhosts
            Build lasthosts page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=lasthosts
            Build unknownip page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=unknownip
            Build allrobots page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=allrobots
            Build lastrobots page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=lastrobots
            Build session page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=session
            Build urldetail page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=urldetail
            Build urlentry page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=urlentry
            Build urlexit page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=urlexit
            Build osdetail page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=osdetail
            Build unknownos page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=unknownos
            Build browserdetail page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=browserdetail
            Build unknownbrowser page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=unknownbrowser
            Build downloads page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=downloads
            Build refererse page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=refererse
            Build refererpages page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=refererpages
            Build keyphrases page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=keyphrases
            Build keywords page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=keywords
            Build errors400 page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=errors400
            Build errors403 page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=errors403
            Build errors404 page: "awstats.pl" -config=vivo.ufl.edu -staticlinks -month=all -year=2016 -output=errors404
            23 files built.
            Main HTML page is 'awstats.vivo.ufl.edu.html'.
5. And, you can use your favourite web browser to view the generated reports.
