# ElasticBeanstalk example

Deployment of a simple node.js app, playing with various EB deployments methods:
All at Once, Immutable and Blue/Green.

All the exercise has been performed on Cloud9 (https://aws.amazon.com/cloud9/), using the EB CLI (https://github.com/aws/aws-elastic-beanstalk-cli-setup).

Some of the useful commands are:

* `npm i c9 -g` , to have the c9 command available in Cloud9;
* `curl -s http://169.254.169.254/latest/meta-data`, to have infos about the local EC2 instance;
* `eb init`, to initialize a folder for EB; it will create as well a repo on CodeCommit if you want;
* `eb create --single`, to create the EB env, with a single EC2 instance (Free Tier compatible);
* `eb deploy <environment_name_>`, to deploy changes to the EB environment;
* `eb clone`, to clone the environment (useful for playing with blue/green deployment);
* `eb swap <source> --destination_name <clone>`, to swap the URLs and perform the blue/green deployment;
* `eb terminate <environment_name>`, to terminate the EB environment.

## Docker image

Deployment of a containerized app.

* create the `docker/Dockerfile`;
* you need to tell EB that you want to deploy Docker images, with the command `eb platform select` and selecting docker;
* build the image with `docker build --tag study-sync:1.0 -f docker/Dockerfile .` (from the parent directory);
* you can run locally the image with `docker run --env PORT=8080 --publish 8080:8080 study-sync:1.0`
* to access it, you need to get the ip address of the EC2 instance `curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

## Docker image on Elastic Container Registry (ECR)

You can as well push a docker image to ECR, with the code inside, and pull it when deploying.
You will need to have a Dockerrun.aws.json file.

Here the steps to follow:

* `docker build -t study-sync .`
* `aws ecr get-login-password | docker login --username AWS --password-stdin <account_number>.dkr.ecr.us-east-1.amazonaws.com`
* `docker tag <docker image> <account_number>.dkr.ecr.us-east-1.amazonaws.com/study-sync`
* create a `study-sync` repo in ECR
* `docker push <account_number>.dkr.ecr.us-east-1.amazonaws.com/study-sync`
* create a file `Dockerrun.aws.json` and add the `AmazonEC2ContainerRegistryReadOnly` policy to the `aws-elasticbeanstalk-ec2-role`
* deploy the app with the EB commands
