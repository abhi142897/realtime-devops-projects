AWS Cloud resource tracking

Problem:
If we want to track the resources created in the AWS cloud, we need to go to the UI and check for resources manually every time. This is a tedious task that requires a lot of manual effort.

Solution:
In this project, we will automate the process of tracking resources in the AWS cloud using AWS CLI.

Approach:
Write a shell script that will run AWS CLI commands to list the required resources.

How to use:
1) Ensure aws is configured on a machine.
2) Simply run the shell script and output will show list of S3 buckets and EC2 instances.
