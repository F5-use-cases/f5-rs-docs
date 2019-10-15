Lab 2 (BIGIP):
----------------------------
 
Task 1.2 - Explore the app repo 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.2.1 explore the infrastructure as code parameters file:
*****************************************************************

1.2.2 view git branches in the application repo:
****************************************************

on the container CLI type the following command to view git branches:

.. code-block:: terminal

   cd /home/snops/f5-rs-app10
   git branch -a 

1.2.3 explore files in the app repo:
****************************************************

.. code-block:: terminal

   more iac_parameters.yaml
   
the infrastructure of the environments is deployed using ansible playbooks that were built by devops/netops. 
those playbooks are being controlled by jenkins which takes the iac_parameters.yaml file and uses it as parameters for the playbooks. 

- You can choose the AWS region you want to deploy in 
- You can also control the WAF blocking state using this file 

Task 1.3 - Update the AWS region for the DEV environment (Optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.3.1 Update git with your information:
**************************
Configure your information in git, this information is used by git (in this lab we use local git so it only has local meaning) 
- on the RS-CONTAINER CLI 

.. code-block:: terminal

   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   
1.3.2 verify you edit the dev branch:
************************** 
- go to the container CLI
- go to the application git folder (command below) 
- check which branches are there and what is the active branch. (command below) 

.. code-block:: terminal

   cd /home/snops/f5-rs-app10
   git branch
   
1.3.3 Update the infrastructure as code parameters file:
************************** 
 
edit the iac_parameters.yaml file to the desired AWS region. then add the file to git and commit.

 - change line: aws_region: "us-west-2"
 - to: aws_region: "your_region" 

.. code-block:: terminal

   vi iac_parameters.yaml 
   git add iac_parameters.yaml
   git commit -m "changed aws region"
   




   
