#### -- First install -- SSH Agent plugin --allows us to use ssh credentials of ec2 server to ssh into the instance and execute commands on them

create multi branch pipeline in jenkins 

multi branch pipeline has own credentials scope.. (only for the pipeline not for other builds)

create new credentials with SSH Username with private key
#### -- username = ec2-user
#### -- private_key = contents of your .pem file you got it from aws

Now use this credentials in the Jenkinsfile for the pipeline. and we can write commands we want to execute in ec2-instance once we connect with ssh. 

We have pipeline syntax on jenkins UI... select sshagent: SSH Agent in the dropdown and we get all the ssh credentials we have... 
...once you select the ssh credentials.. we can click on generate pipeline script and the script will be generated which we can use on Jenkinsfile

When fetching images from private repository from docker hub...
First you have to do docker login inside the ec2-instance and have the authentication key locally for the private repository

Now allow access from jenkins server IP address permission to connect to ec2-instance
#### -- ssh        &nbsp;   &nbsp;   &nbsp;   &nbsp;      22    &nbsp;   &nbsp;    &nbsp;  &nbsp;       jenkins-IP (in aws)

To build the app and build the image.. we are using jenkins-shared-library which is another repo inside which we have implemented dockerlogin, docker build and docker push on private repository...
We are importing the jenkins-shared-library in our project inside Jenkinsfile so that we can use it.

------

To deploy the application with docker-compose
How do we execute docker compose from remote server like ec2-instance from jenkins
#### -- install docker compose on ec2-instance
#### -- create docker-compose.yaml file on your application
#### -- adjust Jenkinsfile to execute docker-compose command on ec2 instance

### Replace docker image with newly built version
#### -- From jenkinsfile to shell script via parameter
Access via $1 in Shell Script + export environment variable (on EC2)

#### -- To set the docker image name dynamically, usually when we are building pipeline for new version of application release, the version of application should be set dynamically
#### -- To create a complete pipeline, we have to version increment the app and with dynamic image versioning and using that increment image in docker compose
