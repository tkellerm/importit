# Infosystem importit
This is the source code of an Infosystem to import master data to abas ERP.
It is build on the abas Essentials SDK.

## General setup
Add a gradle.properties file to your $GRADLE_USER_HOME.

```
#If you use a proxy add it here
http.proxyHost=webproxy.abas.de
http.proxyPort=8000
https.proxyHost=webproxy.abas.de
https.proxyPort=8000

mavenSnapshotURL=https://nexus.abas-usa.com:8443/nexus/content/repositories/abas.snapshots
mavenReleaseURL=https://nexus.abas-usa.com:8443/nexus/content/repositories/abas.releases
mavenUser=<NexusUsername>
mavenPassword=<NexusPassword>
```

To create the common development setup for eclipse run
```shell
gradle eclipse
```

## Installation
To install the project make sure you are running the docker-compose.yml file or else change the gradle.properties file accordingly to use another erp client (you will still need a nexus server, but it can of course also be installed in your erp installation or elsewhere as long as it is configured in the gradle.properties file).

To use the project's docker-compose.yml file, in the project's root directory run:
```shell
docker-compose up
```
Note: You have to be authenticated on docker hub and have the appropriate rights to download the abas/pi-erp image needed for the erp demo installation.

To initialize a gradle.properties for your environment in the project run
```shell
./initGradleProperties.sh
```

Once it's up, you need to load all the $HOMEDIR/java/lib dependencies into the Nexus Server. This is only necessary once as long as the essentials_nexus container is not reinitialized. Run the following gradle command to upload the dependencies to the Nexus Server:
```shell
gradle publishHomeDirJars
```

Now you can install the project as follows:
```shell
gradle fullInstall
```
## Development
If you want to make changes to the project before installing you still need to run the docker-compose.yml file or at least have a Nexus Server set up to work with.

Then run
```shell
gradle publishHomeDirJars
```

You also need to run
```shell
gradle publishClientDirJars
gradle eclipse
```
to upload the $MANDANTDIR/java/lib dependencies to the Nexus Server and set eclipse up to work with the uploaded dependencies.

After that the code should compile both with gradle and in eclipse and you are set up to work on the code or resource files as you want.
