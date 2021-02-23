# FreeTier - JVM - Vaadin
Workshop for the JFrog FreeTier - JVM - Vaadin

This module is the follow-up of the Basic - Module you will find
[here](https://github.com/Java-Workshops/JFrog-FreeTier-JVM).
In this Module here we will create a web-app based on Vaadin - Flow to demonstrate the
security scan over different technologies.

## Requirements on your side

### The IDE
You need an IDE that supports Java and maven.
I am personally using IntelliJ and the screencasts are based on this IDE.
The free version is perfect for this workshop. But you can use whatever IDE fits your needs.

#### IDE Support for Xray
To have IDE Support based on the Xray Plugin from JFrog, you need one of listed IDEs.
The actual list is under the
following URL [https://www.jfrog.com/confluence/display/JFROG/IDE+Integration](https://www.jfrog.com/confluence/display/JFROG/IDE+Integration)

### Docker
You need Docker on your machine for some parts of this tutorial.
It is possible to skip this element during the workshop.

### Java and maven
This tutorial is based on Java version 11 or higher and maven. All JDK´s should fit, there are no special
requirements that I am aware of.
Maven is used in a version higher 3.3. Please make sure you have access to it.
You can use the bundled maven version. The archive is inside the folder **_data/maven** and contains
the maven version 3.6.3 as tar.gz file.

You can install the JDK for example with the OpenSource tool called 
**[Jabba](https://github.com/shyiko/jabba)**

For the installtion of **maven** you can use **brew** on Linux/OSX with the command ```brew install maven```.


### NPM 
We are working with npm in the background to generate the UI based on Vaadin flow.
For this, install the tools NPM / PNPM on your machine. 
For Linux and OSX you can do it for example with the command **brew**
To delete the local npm cache, use the following command **npm cache clean --force**

```bash
brew install npm
brew install pnpm
```

## Timetable and Agenda
The tutorial is planned to be done in approx 2h in total.

The BirdEyeView:
* Intro into the JFrog Platform
* Maven and the Webapp
* Vulnerabilities and how to visualize them
* Create the Docker Repositories
* using Artifactory as Docker Image Proxy
* Build the Docker Images for the WebApp
(optional) * Bonus work - choose an OpenSource Project
and bring it into a fresh new FreeTier


## DevOps versus DevSecOps and the cloud native world
Short intro into the basic concepts of DevSecOps and how JFrog will provide support for it.
If you want to get more informations checkout my Youtube channel

For example the Video about the
**Low Hanging Fruits Of DevSecOps**

EN version -
[![Low hanging Fruits of DevSecOps - english](http://img.youtube.com/vi/lNqADishl8w/mqdefault.jpg)](https://youtu.be/lNqADishl8w "DevSecOps - What are the Low hanging Fruits? - english - 4k")

DE version -
[![Low hanging Fruits of DevSecOps - german](http://img.youtube.com/vi/cgaiBwRjtSk/mqdefault.jpg)](https://youtu.be/cgaiBwRjtSk "DevSecOps - What are the Low hanging Fruits? - german - 4k")

## The tutorial itself
Lets start with the tutorial now! The plan is, to finish all steps in approx 2h.
I will give you from time to time some
additional links to videos to provide additional informations.

### Access to the JFrog Platform
For this tutorial you can use the JFrog Platform based on the FreeTier offer.
This you will find under the URL [http://bit.ly/SvenYouTube](http://bit.ly/SvenYouTube).
You do not need credit card informations for this and the FreeTier itself is not limited in time.
If you want to see how to activate the FreeTier from JFrog, have a look at the following video on Youtube.

[![Activate the JFrog FreeTier](http://img.youtube.com/vi/2mQe_WA8Wmw/mqdefault.jpg)](http://www.youtube.com/watch?v=2mQe_WA8Wmw "JFrog HowTos - 003 - Free Tier Activation")
[![Activate the JFrog FreeTier](http://img.youtube.com/vi/OjKbxekJhrc/mqdefault.jpg)](http://www.youtube.com/watch?v=OjKbxekJhrc "JFrog HowTos - 003 - Free Tier Activation")

## Platform Overview
Short overview of the Platform

[![JFrog HowTos - 006 - Platform Overview](http://img.youtube.com/vi/SCtZ097DSs8/mqdefault.jpg)](http://www.youtube.com/watch?v=SCtZ097DSs8 "JFrog HowTos - 006 - Platform Overview")
[![JFrog HowTos - 006 - Platform Overview](http://img.youtube.com/vi/M1PYnM7MXq4/mqdefault.jpg)](http://www.youtube.com/watch?v=M1PYnM7MXq4 "JFrog HowTos - 006 - Platform Overview")

## Clone the project
Clone the repository from Githb or download the sources if you don´t use git.
```
git clone https://github.com/Java-Workshops/JFrog-FreeTier-JVM-Vaadin.git
```

## Prepare the project for maven
For this workshop we need some maven repositories. Start creating them and use the same names, please.

### create a local maven repo
Create a local Maven Repos called **maven-local-release** and **maven-local-snapshot**
The docu is here [JFrog website](https://www.jfrog.com/confluence/display/RTF6X/Maven+Repository)

[![JFrog HowTos - Create a maven repo](http://img.youtube.com/vi/Jja5XMLcSe0/mqdefault.jpg)](http://www.youtube.com/watch?v=Jja5XMLcSe0 "JFrog HowTos - Create a Maven Repo")
[![JFrog HowTos - Create a maven repo](http://img.youtube.com/vi/Fbp1HddiUrI/mqdefault.jpg)](http://www.youtube.com/watch?v=Fbp1HddiUrI "JFrog HowTos - Create a Maven Repo")

1) add  local repo maven-local-snapshots
1) add  local repo maven-local-release

### create a remote maven repo
Create the needed remote Maven Repositories:
1) add  remote repo maven-remote-mavencentral / https://repo1.maven.org/maven2/
1) add  remote repo maven-remote-apache / https://repo.maven.apache.org/maven2/
1) add  remote repo maven-remote-vaadin / https://maven.vaadin.com/vaadin-addons

### create a virtual maven repo
Create virtual Maven Repos to aggregate the created repositories.
1) add  virtual repo maven-release
1) add  virtual repo maven-snapshots

## Use the Maven repos
Go to the file **pom.xml** and change it in the way that you are
using your new created virtual maven repositories.
Rename your local **.m2/repository** folder to a name like **repository_backup** and
test your config with a **mvn clean verify**.
Maven should load all dependencies from your new repository now.

To have access to these repositories you have two different solutions.
a) you can give the right to consume all remote repositories to the user **anonymous**
b) you cann add the credentials to your **settings.xml** inside the folder **.m2**

## (optional for the fast) create a generic repo for Maven-Binary
Create a generic repo called **generic-local-maven**.
In this repo you will find the file inside the folder **_data/maven/** called **apache-maven-3.6.3-bin.tar.gz**

Upload the file from your folder **_data/_maven**
[https://github.com/Java-Workshops/JFrog-FreeTier-JVM/blob/master/_data/_maven/apache-maven-3.6.3-bin.tar.gz](https://github.com/Java-Workshops/JFrog-FreeTier-JVM/blob/master/_data/_maven/apache-maven-3.6.3-bin.tar.gz)
Try to download one file with **curl** and verify that it is a valid archive.

## Prepare NPM/PNPM
We need NPM repositories for the webb app as well.
Please, create the following repositories
1) add remote repo npm-remote-npmjs https://registry.npmjs.org
1) add virtual repo npm

### configuration of npm

#### Configuration
1) npm config set registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm
1) npm config set @vaadin:registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm/
1) npm config set @polymer:registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm/
1) npm config set @babel:registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm/
1) npm config set @webcomponents:registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm/
1) npm config set @types:registry http://## your server name ##.jfrog.io/artifactory/api/npm/npm/
1) npm cache clean --force

#### Access
1) got to the set me up button and generate the following lines incl your credentials
1) add this to the file ~/.npmrc -
``` 
_auth="## your hashed password"
email=## your email ##
always-auth=true
//## your server name ##.jfrog.io/artifactory/api/npm/npm/:_password="## your hashed password ##"
//## your server name ##.jfrog.io/artifactory/api/npm/npm/:username=## your username ##
//## your server name ##.jfrog.io/artifactory/api/npm/npm/:email=## your email ##
//## your server name ##.jfrog.io/artifactory/api/npm/npm/:always-auth=true
```

## Check if everything is working 
```bash 
mvn clean verify
npm install
npm ls
```

### Project Base for a Vaadin application
This project can be used as a starting point to create your own Vaadin application.
It has the necessary dependencies and files to help you get started.
It requires Java 11 or newer and node.js 10.16 or newer.

To run the project, run `mvn jetty:run` and open [http://localhost:8080](http://localhost:8080) in browser.

To update to the latest available Vaadin release, issue
`mvn versions:update-properties`


## FreeTier Maintenance
To maintain your Platform instance it is good to know that you have the functionality
of a TrashCan. This is good to un-delete a file. On the other side it is
good to clear the caches from time to time and make sure you are not hitting your limits soon.

For this, check inside the Administration Menu your current Storage usage.
Delete the TrashCan and find out how to clear the cached content from a remote Repository.

## IDE Plugin - Scan for Vulnerabilities
In this section wwe want to scan our dependencies to learn more about vulnerabilities.
The tool that we are using is Xray and integrated into the JFrog Platform.
It is able to scan all elements that are in Artifactory. Binaries, Buildinfos and so on.
Your task now: Install the JFrog IDE Plugin and connect to your FreeTier installation.

[![JFrog HowTos - 001 - Xray IDE Plugin v1.6.2](http://img.youtube.com/vi/PsghzAf-ODU/mqdefault.jpg)](http://www.youtube.com/watch?v=PsghzAf-ODU "JFrog HowTos - 001 - Xray IDE Plugin v1.6.2")
[![JFrog HowTos - 001 - Xray IDE Plugin v1.6.2](http://img.youtube.com/vi/zRzjFhR1OjQ/mqdefault.jpg)](http://www.youtube.com/watch?v=zRzjFhR1OjQ "JFrog HowTos - 001 - Xray IDE Plugin v1.6.2")

If you want to see it in action scanning a Vaadin webapp check out the following video.

[![Series - DevSecOps - HowTo harden Vaadin Flow](http://img.youtube.com/vi/h11zwbamskA/mqdefault.jpg)](http://www.youtube.com/watch?v=h11zwbamskA "Series - DevSecOps - HowTo harden Vaadin Flow")
[![Series - DevSecOps - HowTo harden Vaadin Flow](http://img.youtube.com/vi/sTXoWYzSZpI/mqdefault.jpg)](http://www.youtube.com/watch?v=sTXoWYzSZpI "Series - DevSecOps - HowTo harden Vaadin Flow")

## Xray - Watches
Inside the JFrog Platform you can analyse your binaries as well.
For this you need to create Rules, Policies and Watches.
Your next task is:
- create a Rule called **rule-catch-everything** that will scan for all Vulnerabilities.
- based on this Rule, create a Policy called **policy-overview** that consumes the Rule called **rule-catch-everything**
- Now it is time to combine the Policy with your repositories. Call the Watch **panic-watch**
  The last step is to trigger the watch to see the results. For this start the re-calculation of the watch manually.

the official docu is here:
Policies and Rules [https://www.jfrog.com/confluence/display/JFROG/Creating+Xray+Policies+and+Rules](https://www.jfrog.com/confluence/display/JFROG/Creating+Xray+Policies+and+Rules)
Watches [https://www.jfrog.com/confluence/display/JFROG/Configuring+Xray+Watches](https://www.jfrog.com/confluence/display/JFROG/Configuring+Xray+Watches)
of check out my short video:

Rules and Policies

[![JFrog HowTo - Rules and Policies](http://img.youtube.com/vi/msoURDK1ruU/mqdefault.jpg)](http://www.youtube.com/watch?v=msoURDK1ruU "JFrog HowTo - Rules and Policies")
[![JFrog HowTo - Rules and Policies](http://img.youtube.com/vi/M6u4mO6Seh0/mqdefault.jpg)](http://www.youtube.com/watch?v=M6u4mO6Seh0 "JFrog HowTo - Rules and Policies")

Watches

[![JFrog HowTo - Watches](http://img.youtube.com/vi/R9745buR2ZU/mqdefault.jpg)](http://www.youtube.com/watch?v=R9745buR2ZU "JFrog HowTo - Watches")
[![JFrog HowTo - Watches](http://img.youtube.com/vi/plISaOONSb0/mqdefault.jpg)](http://www.youtube.com/watch?v=plISaOONSb0 "JFrog HowTo - Watches")


## AdHoc Vulnerabilities Report
Inside the JFrog Platform you can create AdHoc Vulnerabilities Reports.
Create a Report called **ad-hoc-panic-report**, select the scope **Repositories**
and choose all maven and docker repositories we created today. After this press the button **Generate Report**
Select the report and export it as pdf.
The official docu is here: [https://www.jfrog.com/confluence/display/JFROG/Vulnerabilities+Report](https://www.jfrog.com/confluence/display/JFROG/Vulnerabilities+Report)
If you are to lazy to read, try my JFrog HowTo.

[![JFrog HowTos - 002 - Vulnerabilities Report](http://img.youtube.com/vi/ieB1u_e4jKs/mqdefault.jpg)](http://www.youtube.com/watch?v=ieB1u_e4jKs "JFrog HowTos - 002 - Vulnerabilities Report")
[![JFrog HowTos - 002 - Vulnerabilities Report](http://img.youtube.com/vi/0UAVl0cACPQ/mqdefault.jpg)](http://www.youtube.com/watch?v=0UAVl0cACPQ "JFrog HowTos - 002 - Vulnerabilities Report")


## Conclusion
You have now all steps in your hand to build repositories,
build your projects and scann for known vulnerabilities.
You are able to run your project inside a DevSecOps environment.

Your next task is to choose a small
OpenSource project and try to get it running inside a fresh new FreeTier instance.
If your are done with this, share it with the OpenSource project owner. ;-)

## Who is Sven?
Sven Ruppert has been coding Java since 1996 in industrial projects and is working as Developer Advocate for JFrog.
He is Oracle Developer Champion, regularly speaking at Conferences worldwide and contributes to IT periodicals, as well as tech portals.

Over 15 years he was working as a consultant worldwide
in industries like Automotive, Space, Insurance, Banking, UN and WorldBank
he still enjoying his main topic DevSecOps, Mutation Testing of Web apps
and Distributed UnitTesting besides his evergreen topics Core Java and Kotlin.

Since we are all a bit more home, I started with my Youtube channels
and combined my hobby bushcrafting with my work.
The result are Outdoor-style IT Videos ;-)
I really would appreciate to have YOU as my new subscriber!

Youtube Channel - Sven Ruppert - English
[![Youtube Channel English](https://yt3.ggpht.com/ytc/AAUvwniR1nyALB7XJIAL49WrhFCMjf39ALwQiAbUoOF1=s176-c-k-c0x00ffffff-no-rj)](https://bit.ly/Outdoor-Nerd "Youtube Channel - Outdoor Nerd")

Youtube Channel - Sven Ruppert - German
[![Youtube Channel German](https://yt3.ggpht.com/ytc/AAUvwnikcyASO4g2KHeCbCouznJ7oxIdBfUimaAVOC3CGFc=s176-c-k-c0x00ffffff-no-rj)](https://www.youtube.com/user/svenruppert "Youtube Channel - Sven Ruppert")

Youtube Channel - Sven Ruppert - Outdoor
[![Youtube Channel German](https://yt3.ggpht.com/ytc/AAUvwnhZCtF1ebTwYW849Uu3l8VdQj62T_eaLsRJy6w2uA=s176-c-k-c0x00ffffff-no-rj)](https://www.youtube.com/channel/UCwFH8F7TNzY5Qi2K98ddNgw "Youtube Channel - Sven Ruppert private")
