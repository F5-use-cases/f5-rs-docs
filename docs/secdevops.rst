SecDevOps lab 
===================

this lab covers how to protect an app using F5's application security solutions and 'bake' the security policy into the application lifecycle. 

Getting started
===================

if you run into issues / problems please contact me email with some information, yossi@f5.com


Run the rs-container
----------------------------

The entire lab is built from code hosted in this repo, in order to launch the lab environment you will download and run a container that has the tools we are using (ansible and jenkins) as well as the depndencies and requirements to interact with the differnet services (F5, AWS, github.. ) 
on the linux jumphost in UDF, run the following command to start the container,
the will attach a volume from the linux host to the container


.. code-block:: terminal

    sudo docker run -v config:/home/snops/host_volume -p 2222:22 -p 10000:8080 -it --rm f5usecases/f5-rs-container



Configure credentials and personal information
------------------------------------------------------------

log in as jenkins (root password is 'default')

jenkins user is used so that the config changes we do are available to jenkins

.. code-block:: terminal

   su root -c "su jenkins"
   
   
Create the SSH keys, the SSH key will be used when creating EC2 instances.  we will strore them in the jenkins SSH folder so that jenkins can use them to access instances.
Copy credentilas and paramaters files from the host folder. 
copy the udf SSH key to the authorized ssh keys folder for direct ssh access using UDF. 

.. code-block:: terminal

   mkdir /var/jenkins_home/.ssh
   ssh-keygen -f /var/jenkins_home/.ssh/id_rsa -t rsa -N ''
   cp /home/snops/host_volume/f5-rs-global-vars-vault.yaml /home/snops/f5-rs-global-vars-vault.yaml
   cp /home/snops/host_volume/sshkeys/udf.pub /home/snops/.ssh/authorized_keys
   
   


copy the global paramaters file to /home/snops (for now get it from me yossi@f5.com) 

.. code-block:: terminal

   vi /home/snops/f5-rs-global-vars-vault.yaml
   

configure your personal information in the global parameters file. 

.. code-block:: terminal

   echo password > ~/.vault_pass.txt
   ansible-vault edit --vault-password-file ~/.vault_pass.txt /home/snops/f5-rs-global-vars-vault.yaml

* after you save the f5-rs-global-vars-vault.yaml file for the first time you get an error message, ignore it it's a bug
  ERROR! Unexpected Exception, this is probably a bug: [Errno 1] Operation not permitted: '/home/snops/f5-rs-global-vars-vault.yaml'

Configure jenkins and reload it
------------------------------------------------------------

the following script will configure jenkins with your information and reload it. 

.. code-block:: terminal

   ansible-playbook --vault-password-file ~/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml

   
SSH key configuration (if you don't have an existing SSH key you want to use):
------------------------------------------------------------------------------------

use key_gen to generate a new private and public key (accept all defaults - DON'T use passphrase)

.. code-block:: terminal

   ssh-keygen
   
command will create a private/public key pair in the jenkins home directory: /var/jenkins_home/.ssh/id_rsa /var/jenkins_home/.ssh/id_rsa.pub

store both private and public key in a secure place on your laptop for future use!


Open Jenkins:
------------------------------------------------------------------------------------

on your laptop (the container host) Open Jenkins http://localhost:10000

usernmae: snops , password: default



start the dev environment:
------------------------------------------------------------------------------------

in jenkins open the 'DevSecOps - Lab - App2' folder', the lab files are all in this folder 
we will start by deploying a dev environment, you will start a pipeline that creates a full environment in AWS. 

click on the 'f5-rs-app2-dev' folder.
here you can see all of the relevant jenkins jobs for the dev environment.

click on 'Full stack deployment' , that's the pipeline view for the same folder. 
click on 'run' to start the dev environment pipeline. 

wait until all of the jobs have finished (turned green). 

open slack - https://f5-rs.slack.com/messages/C9WLUB89F/
go to the 'builds' channel. 
use the search box on the upper right corner and filter by your username (student#). 
jenkins will send to this channel the bigip and the application address. 

open the bigip and login using the provided credentials. 
try to access the app using the ip provided in the slack channel - that's the Elastic ip address that's tied to the VIP on the bigip. 

check the bigip configuration under the 'rs_app1' partition, 
AS3 is used to push the service configuration to the bigip. the AS3 decleration deploys all of the objects into a partition. 
check which ASM policy is attached to the 'service_main' VIP. 

go to 'traffic learning', make sure you are editing the 'linux-high' policy. 
you should see a suggestion on 'High ASCII characters in headers' , examine the request. this is a flase positive. the app uses a different language in the header and it is legitimate traffic. 
accept the suggestion.

check the other suggestions, you'll see some signatures that were triggered. those are actual threats that are part of the autometed security testing and we can ignore the suggestions. 

apply the policy. we will now export the policy to the git repo and start the autometed build again to check that we are ready to promote it to production. 

go back to jenkins, under the 'f5-rs-app1-dev' there is a job that will export the policy and save it to the git repo - 'SEC export waf policy'
click on this job and choose 'Build with Parameters' from the left menu. 

you can leave the defaults, it asks for two parameters. one is the name of the policy on the bigip and the other is the new policy name in the git repo. 

click on 'build' 

check the slack channel - you should see a message about the new security policy that's ready. 
this illustrates how chatops can help between different teams. 

the security admin role ends here. it's now up to the developer to update the iac_parameters.yaml in their repo to point to the new policy and run the pipeline again. 

change the policy used for the app:
~~~~~~~~~~~~~~~~~~~

ssh into the contianer, make sure you are connected as user 'jenkins' 
go to the application git folder. check which branches are there and what is the active branch. (git branch) 
you should be on the 'dev' branch. the files you see belong to the dev branch. 

.. code-block:: terminal

   cd /home/snops/f5-rs-app1
   git branch


Configure your information in git, this information is used by git (in this lab it we use local git so it only has local meaning) 

.. code-block:: terminal

   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   
 
edit the iac_parameters.yaml file to point the deployment to the new ASM policy. then add the file to git and commit 

.. code-block:: terminal

   vi iac_parameters.yaml 
   git add iac_parameters.yaml
   git commit -m "changed asm policy"
   
go back to jenkins and open the 'f5-rs-app1-dev ' folder. choose the 'waf policy' tab , jenkins is set up to monitor the application repo. when a 'commit' is identified jenkins will start an automatic pipeline to deploy the service. it takes up to a minute for jenkins to start the pipeline. 

jenkins takes the parametes from the git repo and uses them to deploy/update the service. 


log on to the bigip again, check which ASM policies are there and which policy is attached to the 'service_main' VIP. 
check the 'traffic learning' for the security policy and verify you no longer see the 'high ascii charachters' 

this concludes the tests in the 'dev' environment. we are now ready to push the changes to production. 
we will 'merge' the app1 dev branch with the master branch so that the production deployment will use the correct policy. 

.. code-block:: terminal
 
   git checkout master
   git merge -m "changed asm policy"


we will deploy the environemnt. go to the 'f5-rs-app1-prod' folder, choose the 'aws stack waf 01' view and run the pipeline. 
go to slack to get the ip's for the bigip and the app. 













