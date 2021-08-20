# Simple instructions


## Construct EC2:
1. Guide here, but with some following changes: https://medium.com/@philipaffulnunoo/how-to-deploy-meteor-1-4-app-to-aws-ec2-in-2017-bfea1a7c308a
2. Create key pair and download ppk first.
3. Create an EC2 instance, get an Ubuntu 14 or 16 server
4. Add security groups that allow HTTP and HTTPS traffic 
5. Free tier (micro) works (pretty slow), but strongly recommend small. 


## Connect to AWS:
1. ssh or PuTTY to EC2 instance
2. If use PuTTY, change IP, add ppk in connection -> SSH -> Auth -> browse, username: ubuntu

## Deploy Meteor app to AWS:
1. Install docker:
    ~~~ 
    sudo apt-get update   # update repo
    sudo apt install docker.io   # install docker
    sudo usermod -aG docker ${USER}   # add user to docker user group, so that you don't have to use sudo
    exit
    # restart
    docker info   # to check that your user has the right permissions. Should give you a summary.
    ~~~
2. Upload tournament code:
    ~~~
    git clone https://github.com/chengxiluo/tournament.git # the respository needs to be public or with access token
    ~~~
3. More installations (npm, mup, nodejs, meteor, react, flowrouter):

    Install npm:
    ~~~
    sudo apt install npm
    ~~~
    Install mup:
    ~~~
    sudo npm install -g mup@1.4
    sudo ln -s /usr/bin/nodejs /usr/bin/node
    ~~~
    Install nodejs:
    ~~~
    sudo npm cache clean -f
    sudo npm install -g n
    sudo n stable
    ~~~
    Install meteor:
    ~~~
    sudo curl https://install.meteor.com/ | sh
    ~~~
    Install react, react-dom:
    ~~~
    meteor npm install --save react react-dom
    ~~~
    Install FlowRouter:
    ~~~
    meteor add kadira:flow-router
    ~~~
    Install react-mounter package:
    ~~~
    npm install --save react-mounter
    ~~~
    Install react-beautiful-dnd for drag-and-drop of list items:
    ~~~
    meteor npm install react-beautiful-dnd --save
    ~~~
    
## Edit mup.js file
1. Go to tournament/dev/tournament/.deploy directory, generate mup.js file
    ~~~
    mup init
    ~~~
2. edit mup.js file:

    host => AWS Public IPv4 address
    
    username => ubuntu
    
    unslash pem, pem => pem key address (remember use scp to upload your AWS pem key)
    
    path => '/home/ubuntu/tournament/dev/tournament'
    
    ROOT_URL => AWS Public DNS IPv4 (`http://ec2-....us-east-2.compute.amazonaws.com`)
    
    MONGO_URL => we will edit after we set up MongoDB
    
    slash MONGO_OPLOG_URL
    
    slash mongo part

## Setting up MondoDB on Atlas
1. Register guide: https://www.okgrow.com/posts/mongodb-atlas-setup
2. Connection URI is under "Connect" button -> "Connect your application" -> "Standartd connection string"

    It looks like this: `mongodb://<username>:<PASSWORD>@cluster0-shard....`
3. edit mup.js file again:
  
    MONGO_URL => use above URI

## From ./deploy folder, run
 
    mup setup # every time mup.js changes
    mup deploy # to deploy on remote server, this takes some time

  
Check consent page: `http://<AWSPublicIPAddress>/consent/<ID>`

You are done!


For more detailed instructions, like why we have to do each step, check docs.txt. 