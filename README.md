# maven-release-plugin-sample
Sample application featuring maven-release-plugin

## Generate Java Project

```
mvn archetype:generate -DgroupId=com.example.samples -DartifactId=maven-release-plugin-sample
```

## Requirements

SCM configuration

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <scm>
     <connection>scm:git:https://github.com/simaosoares/maven-release-plugin-sample.git</connection>
     <developerConnection>scm:git:https://github.com/simaosoares/maven-release-plugin-sample.git</developerConnection>
     <url>https://github.com/simaosoares/maven-release-plugin-sample</url>
     <tag>HEAD</tag>
  </scm>
  ...
</project>
```

* connection: requires read access for Maven to be able to find the source code (for example, an update). 
* developerConnection: requires a connection that will give write access. 

The Maven project has spawned another project named Maven SCM, which creates a common API for any SCMs that wish to implement it. The most popular are CVS and Subversion, however, there is a growing list of other supported SCMs. All SCM connections are made through a common URL structure.
```
scm:[provider]:[provider_specific]
``` 

* tag: Specifies the tag that this project lives under. HEAD (meaning, the SCM root) should be the default.
* url: A publicly browsable repository.

## Maven release plugin basic commands

```
mvn release:clean
```
Cleaning a release goes through the following release phases:
* Delete the release descriptor (release.properties)
* Delete any backup POM files

```
mvn release:prepare
```
Preparing a release goes through the following release phases:
* Check that there are no uncommitted changes in the sources
* Check that there are no SNAPSHOT dependencies
* Change the version in the POMs from x-SNAPSHOT to a new version (you will be prompted for the versions to use)
* Transform the SCM information in the POM to include the final destination of the tag
* Run the project tests against the modified POMs to confirm everything is in working order
* Commit the modified POMs
* Tag the code in the SCM with a version name (this will be prompted for)
* Bump the version in the POMs to a new value y-SNAPSHOT (these values will also be prompted for)
* Commit the modified POMs

```
mvn release:prepare -DdryRun
mvn release:prepare -DdryRun=true
```
Dry run: don't checkin or tag anything in the scm repository, or modify the checkout. Running mvn -DdryRun=true release:prepare is useful in order to check that modifications to poms and scm operations (only listed on the console) are working as expected. Modified POMs are written alongside the originals without modifying them.

```
mvn release:perform
```
Performing a release runs the following release phases:
* Checkout from an SCM URL with optional tag
* Run the predefined Maven goals to release the project (by default, deploy site-deploy)

## References

* https://maven.apache.org/maven-release/maven-release-plugin/index.html
* https://maven.apache.org/pom.html#SCM