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
