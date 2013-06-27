# Installing a custom runtime (node.js) on CloudBees

These are a set of samples that walk through setting up customized runtimes on CloudBees. Customized runtimes are called [ClickStacks](http://developer.cloudbees.com/bin/view/RUN/ClickStack) on the CloduBees environment.

## Requirements
Clone the Genapp utility and install it as per requirements (needs Erlang, Curl and Python (for sample)) 
* [Source](https://github.com/cloudbees/genapp)
* [Documentation](http://genapp-docs.cloudbees.com/). Read the overview and the requirements chapter.

    cp $GENAPP_SRC/dev.config.in $GENAPP_SRC/dev.config. 

Modify devmode to true and specify where the apps will be deployed on a local instance (/Users/harpreet/genapp/apps) for all examples checked in here. Replace with your own directory.

    %%% -*-erlang-*-
    [{genapp,
    [
        {devmode, true},
        {apps_home, "/Users/harpreet/genapp/apps"}
    ]}].

    cd $GENAPP_SRC
    make 
    make shell             //starts the shell - the local environment
    genapp:query_apps([]). // Queries the local environment. The trailing period is important.
	
## Optional
View a list of ClickStacks on the [CloudBees community page](https://github.com/CloudBees-community/)

## Overview
We install a runtime in our local environment first and then push it to a public s3 bucket so that it can be used with bees app:deploy command.
Follow the samples in the order given below.

### Local Environment

#### Sample v0: Verify whether Genapp has been installed right.
Sets up a simple_http runtime (CloudBees parlance). This is the sample python example which ships with genapp. 

    cd v0/simple_http
	$GENAPP_SHELL> genapp:deploy("/Users/harpreet/genapp/v0/simple_http").
	
This outputs a unique id ($ID) where the app is deployed. To run the app:

    $ID/.genapp/control/start
	curl localhost:$PORT  //$PORT output by the start command
	
#### Sample standalone_node_test_app: Test out if node works on your system
Install node.js

    cd standalone_node_test_app
	node server.js 7000
	
#### Sample v1: Modify simple_http to run node instead of Python

    cd v1/nodejs_plugin
	$GENAPP_SHELL> genapp:deploy("/Users/harpreet/genapp/v1/test_app").
    $ID/.genapp/control/start
	curl localhost:$PORT  //$PORT output by the start command

#### Sample v2: Modify v1 so that node is not picked from your own machine

    cd v2/nodejs_plugin
	$GENAPP_SHELL> genapp:deploy("/Users/harpreet/genapp/v2/test_app").
    $ID/.genapp/control/start
	curl localhost:$PORT  //$PORT output by the start command

#### Sample v2: Modify v1 so that node is not picked from your own machine

    cd v2/nodejs_plugin
	$GENAPP_SHELL> genapp:deploy("/Users/harpreet/genapp/v2/test_app").
    $ID/.genapp/control/start
	curl localhost:$PORT  //$PORT output by the start command

I have a mac, so the node installed is for a Mac machine. Modify the curl command in the Makefile if you have another platform.

#### Sample final: Test with CloudBees RUN@cloud and not on your local environment.

I had to modify the curl command to use Linux so that the binaries are appropriate for ArchLinux images used by CloudBees.
 
    cd final/nodjs_plugin/
	make publish // publishes to my s3; modify to upload to a publicly accessible URL
	
	bees app:deploy -a $CLOUDBEES_ACCOUNT/nodecs -t nodejs_plugin  -RPLUGIN.SRC.nodejs_plugin=http://PATH_TO_PUBLICLY_ACCESSIBLE_URL/mynode.zip test.zip
	
	curl $WHATEVER_URL_SPIT_FROM_APP:DEPLOY






