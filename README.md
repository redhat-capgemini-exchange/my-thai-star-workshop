# "Road to cloud-native" workshop

The "Road to cloud-native" workshop is a 1-day event with a series of hands-on-labs which are designed to familiarize participants with cloud-native concepts and give them a taste of using OpenShift for building and managing cloud-native applications.

### Outline of the day

* Overview (60min)
* Intro to the lab environment (30min)
* Labs (240min)
* Additional labs, if time permits

## Pre-requisites

### Audience

* Architects
* Developers

### Preparation

* Complete “Red Hat OpenShift 3 Foundations”
* Bring your own laptop

# Workshop Content

## Lab 0 - Setup the workspace

### Introduction to the lab environment

* Overview CodeReady Workspaces
* Overview of other CI/CD tools

### Introduction to devonfw and the reference app

* https://github.com/devonfw
* https://github.com/devonfw/my-thai-star

### Introduction to workshop assets

* https://github.com/redhat-capgemini-exchange
* https://github.com/redhat-capgemini-exchange/my-thai-star
* https://github.com/redhat-capgemini-exchange/my-thai-star-workshop
  
### Setup your own development workspace

1. Create a user in Gogs
2. Create a fork of the “My Thai Star” reference application code
3. Create a new Java workspace
4. Import code
5. Finalize the setup
    1. config Git
    2. config Maven
    3. modify POM to use Nexus

## Lab 1 - A first build and first steps in OpenShift

### In CodeReady workspace

* Manually build the java code

```shell
mvn install
```

* Launch the java API service
* Test access to the service
```shell
curl http://localhost:8080
```

* Terminate the running process
 
### Access OpenShift from the command line
* Open a terminal window
* Login into OpenShift from command line
```shell
oc login https://<url_of_cluster>
```
* Create a new project from the comand line
```shell
oc new-project userXY
```

### A first deployment on OpenShift
* Open the OpenShift developer console
* Navigate to the new project
* Manually create a new Java app
* Complete the configuration wizzard
  - Name of the app
  - Point to you forked app code
  - Advanced options: git branch, context dir
* Watch the build process

**EXPECT AN ERROR !!**

### Introduction to S2i builds
* Add missing config variables
* Restart the build

## Lab 2 - Automation of the setup, build & deployment

* Import ‘redhat-capgemini-exchange/my-thai-star-workshop’ into the CodeReady workspace
* Create a new project: oc new-project my-thai-star-userXY
* Introduce templates
  - build
  - deploy
  - others (service, routes, storage etc)
* Add build & deployment templates to the code base
  - customize Git references etc
* Deploy build templates
  - start build
* Create a deployment
* Add a route

## Lab 3 - Customizing the S2I build process

* Create a build setup for the frontend
  - based on the default node.js builder image
* Explain how to customize the S2I process with scripts
  - Show the original assemple & run script
  - Show the matching docker files from the reference app
  - Add custom scripts
* Discuss single page app issues
  - how to customize the build to reflect the deployment
  - how to serve content
* Build and deploy the frontend

**SHOW THE RUNNING APP !**

## Lab 4 - CI/CD setup and Gitflow

### Explain & discuss build triggers
* Add webhooks to Git

### Discuss Gitflow
* Explain branching, feature branches etc
* Create a branch
* Make a code change and commit
* Watch the build & deplyment cycle

## Lab 5 - Setup of a deployment pipeline
  
### Setup TEST and PROD projects

* Introduction to the integrated build pipeline & Jenkins
* Discuss forks, feature branches & pull requests
  - Create a pull request against the upstream repo
  - Merge changes in upstream

### End-to-end CICD

* Setup CI/CD
  - Image sharing
  - Blue / green deployment concept
  - Setup CD pipeline
* Trigger CI/CD
  - Merge a pull request

**MONITOR THE PROGRESS**

## Missing parts

* Config Maps, Secrets
* Monitoring endpoints
* Logging
* Storage
* Using existing containers, e.g. databases

## Additional Labs

* App monitoring and autoscaling
* Logging & distributed tracing
* Build a custom S2I container

## Links and resources

#### OpenShift Console
https://master.$CLUSTER.openshiftworkshop.com

#### CodeReady Workspace
http://codeready-workspaces.apps.$CLUSTER.openshiftworkshop.com

#### Gogs
http://gogs-workspaces.apps.$CLUSTER.openshiftworkshop.com

#### Nexus
http://nexus-workspaces.apps.$CLUSTER.openshiftworkshop.com
http://nexus-workspaces.apps.$CLUSTER.openshiftworkshop.com/nexus/#welcome

#### Devonfw on GitHub
https://github.com/devonfw/my-thai-star

## Workshop setup

NOTE: The workshop targets OpenShift 3.11 at the moment. A version supporting OpenShift 4 will follwo later.

### Deploy an OpenShift cluster using RHPDS

Go to https://rhpds.redhat.com/ and create an OpenShift 3.11 cluster.

### Deploy the CodeReady Workspaces and other tools

Run the setup script:

```shell
cd scripts
./setup.sh <name_of_the_cluster> <namespace>
```

### Prepare Git

* Create a new organization 'devonfw'
* Import the upstream 'My Thai Star' reference app
* Import the upstream workshop project
