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

.. code-block:: terminal

   ssh-keygen -f $HOME/.ssh/id_rsa -t rsa -N ''
   cp /home/snops/host_volume/f5-rs-global-vars-vault.yaml /home/snops/f5-rs-global-vars-vault.yaml
   mkdir ~/.aws && cp /home/snops/host_volume/credentials ~/.aws/credentials
   

configure your personal information in the global parameters file. 
for the username use your student#, put your actual 

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



Open Jenkins:
------------------------------------------------------------------------------------

go to UDF, on the 'jumphost' click on 'access' and 'jenkins'  

usernmae: snops , password: default


when you open jenkins you should see two jobs that have started running automaticlly, 'Push a WAF policy',
this happens because jenkins monitors the repo and start the jobs. you can cancel the jobs or let them fail. 


start the dev environment:
------------------------------------------------------------------------------------

in jenkins open the 'DevSecOps - Lab - App2' folder', the lab files are all in this folder 
we will start by deploying a dev environment, you will start a pipeline that creates a full environment in AWS. 

.. image:: /img/devsecops_lab01/jenkins010.PNG
   :width: 800 px
   :align: center
   
click on the 'f5-rs-app2-dev' folder.
here you can see all of the relevant jenkins jobs for the dev environment.

.. image:: /img/devsecops_lab01/jenkins020.PNG
   :width: 800 px
   :align: center

click on 'Full stack deployment' , that's the pipeline view for the same folder. 

.. image:: /img/devsecops_lab01/jenkins030.PNG
   :width: 800 px
   :align: center
   
click on 'run' to start the dev environment pipeline. 

.. image:: /img/devsecops_lab01/jenkins040.PNG
   :width: 800 px
   :align: center


you can review the output of each job while its running, click on the small 'console output' icon as shown in the screenshot:

.. image:: /img/devsecops_lab01/jenkins050.PNG
   :width: 800 px
   :align: center
   
   
wait until all of the jobs have finished (turned green). 

.. image:: /img/devsecops_lab01/jenkins060.PNG
   :width: 800 px
   :align: center

open slack - https://f5-rs.slack.com/messages/C9WLUB89F/
go to the 'builds' channel. 
use the search box on the upper right corner and filter by your username (student#). 
jenkins will send to this channel the bigip and the application address. 

.. image:: /img/devsecops_lab01/Slack-040.PNG
   :width: 800 px
   :align: center

open the bigip and login using the provided credentials. 
explore the objects that were created: 

Cloud formation template:
~~~~~~~~~~~~~~~~~~~~~~~~~
this is the base deployment of the bigip, we start with the F5 supported 2nic CFT. 
it deploys bigip with the latest cloud version, installs the necessary cloudlibs and cloud related scripts.

bigip rs onboard:
~~~~~~~~~~~~~~~~~
deploys the 'enterprise' default profiles, for example: 
HTTP, analytics, AVR, DOSL7, iapps etc. 

push a waf policy:
~~~~~~~~~~~~~~~~~
pushes a waf policy from the repo to the bigip, updates DOSL7 and FPS profiles. 

rs-iapp service:
~~~~~~~~~~~~~~~~~
deploys a service on the bigip using either AS2 or AS3 

rs-attacks:
~~~~~~~~~~~~~~~~~
good and bad traffic generation to the app.


try to access the app using the ip provided in the slack channel - that's the Elastic ip address that's tied to the VIP on the bigip. 
after ignoring the ssl error (because the certificate isn't valid for the domain) you should get to the Hackazone mainpage


.. image:: /img/devsecops_lab01/hackazone010.PNG
   :width: 800 px
   :align: center


SecOps role:
----------------------------------------
in this example the app owner deployed a new service to their dev environemnt, the tests show that some of the valid requests are blocked. you should log in to the bigip as the secops engineer and fix the false-positive. 

go to 'traffic learning', make sure you are editing the 'linux-high' policy. 
you should see a suggestion on 'High ASCII characters in headers' , examine the request. this is a flase positive. the app uses a different language in the header and it is legitimate traffic. 
accept the suggestion.


.. image:: /img/devsecops_lab01/Bigip-040.PNG
   :width: 800 px
   :align: center

check the other suggestions, you'll see some signatures that were triggered. those are actual threats that are part of the autometed security testing and we can ignore the suggestions. 

apply the policy. we will now export the policy to the git repo and start the autometed build again to check that we are ready to promote it to production. 

go back to jenkins, under the 'f5-rs-app2-dev' there is a job that will export the policy and save it to the git repo - 'SEC export waf policy'

.. image:: /img/devsecops_lab01/jenkins075.PNG
   :width: 800 px
   :align: center
   
click on this job and choose 'Build with Parameters' from the left menu. 

.. image:: /img/devsecops_lab01/jenkins080.PNG
   :width: 800 px
   :align: center

you can leave the defaults, it asks for two parameters. the first parameter is the name of the policy on the bigip and the other is the new policy name in the git repo.  

click on 'build' 

check the slack channel - you should see a message about the new security policy that's ready. 
this illustrates how chatops can help between different teams. 

.. image:: /img/devsecops_lab01/Slack-030.PNG
   :width: 800 px
   :align: center

the security admin role ends here. it's now up to the appowner to run the pipeline again. 

deploy to dev again:
~~~~~~~~~~~~~~~~~~~

ssh into the contianer, make sure you are connected as user 'jenkins' 
go to the application git folder. check which branches are there and what is the active branch. (git branch) 
you should be on the 'dev' branch. the files you see belong to the dev branch. 

.. code-block:: terminal

   cd /home/snops/f5-rs-app2
   git branch


Configure your information in git, this information is used by git (in this lab it we use local git so it only has local meaning) 

.. code-block:: terminal

   git config --global user.email "you@example.com"
   git config --global user.name "Your Name"
   
 
edit the iac_parameters.yaml file to point the deployment to the new ASM policy (linux-high-v01). then add the file to git and commit 

.. code-block:: terminal

   vi iac_parameters.yaml 
   git add iac_parameters.yaml
   git commit -m "changed asm policy"


go back to jenkins and open the 'f5-rs-app2-dev ' folder. choose the 'waf policy' tab , jenkins is set up to monitor the application repo. when a 'commit' is identified jenkins will start an automatic pipeline to deploy the service. it takes up to a minute for jenkins to start the pipeline. 

jenkins takes the parametes from the git repo and uses them to deploy/update the service. 

log on to the bigip again, check which ASM policies are there and which policy is attached to the 'App2 VIP. 
check the 'traffic learning' for the security policy and verify you no longer see the 'high ascii charachters' 

this concludes the tests in the 'dev' environment. we are now ready to push the changes to production. 
we will 'merge' the app1 dev branch with the master branch so that the production deployment will use the correct policy. 
on the /home/snops/f5-rs-app2 folder:

.. code-block:: terminal
 
   git checkout master
   git merge -m "changed asm policy"

* the merge will trigger a job in jenkins that's configured to monitor this repo - 'Push waf policy', since the environment isn't deployed yet it will fail, either cancel the job or let it fail. 

deploy to PROD:
~~~~~~~~~~~~~~~~~~

we will deploy the environemnt. go to the 'f5-rs-app2-prod' folder, choose the 'Full stack deployment' view and run the pipeline. 
go to slack to get the ip's for the bigip and the app. 








