
# Install Java SDK

brew install openjdk@8   

sudo ln -sfn /usr/local/opt/openjdk@8/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-8.jdk 

NOTE: the above "sudo ln" is required; otherwise, IntelliJ will complain not able to find the installed java SDK. 

# Install Maven

brew install maven 

create a folder ~/.m2, download the settings.xml and put the file in ~/.m2.
# Install IntelliJ

Go to https://www.jetbrains.com/idea/download/#section=mac to download the installation image.   

IntelliJ Configuration.   
To install Maven Plugins.  
Maven Helper.  
Eclipse Code Formatter.  
SonarLint.  
GitToolBox.  
Rainbow Bracket.  
MyBatisX.  
- This plugin is able to do code navigation to the MyBatis xml definition.   
- After installing, make the following configuration

# Maven Configuration

User Settings file:/Users/mike/.m2/settings.xml.  
Local Repository: /Users/mike/.m2/repository   

Build,Execution,Deployment->Build Tolls->Maven->Runner: Choose Run in background

# Setup Code Style
download the code style file code-style.xml.  
# To manage multiple microservice applications with a single IDEA project

To create the IDEA project and load the first java microservice applications.   
To create a 'workspace' folder somewhere on your laptop.  
For example, ~/projects/workspaces.  
To create an empty IDEA project inside the 'workspace' folder.  

To add modules to this IDEA project.   

Now the application and all its modules are added to the IDEA project.   

To add more applications to the IDEA project
Open 'Maven Helper' to add more applications

To double check the IDEA project and loaded applications are GOOD.
To check the project settings

To check the IDEA preferences






