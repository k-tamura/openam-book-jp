### OpenAMのアンインストール

This chapter shows you how to uninstall OpenAM core software. See the OpenAM Web Policy Agent User's Guide, or the OpenAM Java EE Policy Agent User's Guide for instructions on removing OpenAM agents.

手順. OpenAMのアンインストールする

After you have deployed and configured OpenAM core services, you may have as many as four locations where OpenAM files are stored on your system.

Following the steps below removes the OpenAM software and the internal configuration store. If you used an external configuration store, you can remove OpenAM configuration data after removing all the software.

1. Shut down the web application container in which you deployed OpenAM.

    ```bash
    $ /etc/init.d/tomcat stop
    Password:
    Using CATALINA_BASE:   /path/to/tomcat
    Using CATALINA_HOME:   /path/to/tomcat
    Using CATALINA_TMPDIR: /path/to/tomcat/temp
    Using JRE_HOME:        /path/to/jdk/jre
    Using CLASSPATH:       /path/to/tomcat/bin/bootstrap.jar:
     /path/to/tomcat/bin/tomcat-juli.jar
    ```
    
2. Unconfigure OpenAM by removing the configuration files found in the $HOME directory of the user running the web application container.

    A full uninstall of OpenAM core services and configuration files consists of removing the following directories:
    - The configuration directory, by default $HOME/openam. If you did not use the default configuration location, check the value of the Base installation directory property under Deployment > Servers > Server Name > General > System.
    - The hidden directory that holds a file pointing to the configuration directory. For example, if you are using Apache Tomcat as the web container, this file could be $HOME/.openamcfg/AMConfig_path_to_tomcat_webapps_openam_ OR $HOME/.openssocfg/AMConfig_path_to_tomcat_webapps_openam_. 

    ```bash
    $ rm -rf $HOME/openam $HOME/.openamcfg
    ```
    
    Or:

    ```bash
    $ rm -rf $HOME/openam $HOME/.openssocfg
    ```
    
    If you used an external configuration store, you must remove the configuration manually from your external directory server. The default base DN for the OpenAM configuration is dc=openam,dc=forgerock,dc=org.
    > **Note**  
    > At this point, you can restart the web container and configure OpenAM anew if you only want to start over with a clean configuration rather than removing OpenAM completely.

3. Undeploy the OpenAM web application.  
    For example, if you are using Apache Tomcat as the web container, remove the .war file and expanded web application from the container.

    ```bash
    $ cd /path/to/tomcat/webapps/
    $ rm -rf openam.war openam/
    ```
