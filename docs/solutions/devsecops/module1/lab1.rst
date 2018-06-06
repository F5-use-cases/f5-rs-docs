Lab 1: Enable dev to deploy their app with a waf policy 
----------------------------

Background: 
~~~~~~~~~~~~~

Security team has created some security policies templates, those were built based on the F5 templates with some modifications to the specific enterprise. 
in this lab we don't cover the 'how to' of the security templates. we focus on the operational side and the workflows. 

The Tasks are split between the two roles:
 - secops
 - dev (the person who's responsible of changing code for the app and the infrastructure of the app) - dave 
 
Lab scenario:
~~~~~~~~~~~~~

New app - App2 is being developed. the app is an e-commerce site. 
code is ready and ready to go into 'DEV' environemnt. for lab simplicity there are only two environments - DEV and PROD. 
dave should deploy their new code into a DEV environment that is exactly the same as the production environment. 
run their tests and security testing and after all of the tests finish successfully deploy the code to production.

the infrastructure of the environments is built from code which exposes some parameters to dave. 
that enables dave to choose the aws region in which to deploy and the name of the app. 
 
dave can control the deployment of the security policies from his repo. 
 
Task 1 - Deploy dev environment 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Open Jenkins:
------------------------------------------------------------------------------------

go to UDF, on the 'jumphost' click on 'access' and 'jenkins'  

usernmae: snops , password: default

when you open jenkins you should see two jobs that have started running automaticlly, 'Push a WAF policy',
this happens because jenkins monitors the repo and start the jobs. you can cancel the jobs or let them fail. 


in jenkins open the 'DevSecOps - Lab - App2' folder', the lab files are all in this folder 
we will start by deploying a dev environment, you will start a pipeline that creates a full environment in AWS. 

.. image:: /docs/solutions/devsecops/images/jenkins010.PNG
   :width: 800 px
   :align: center
   
click on the 'f5-rs-app2-dev' folder.
here you can see all of the relevant jenkins jobs for the dev environment.

.. image:: /docs/solutions/devsecops/images/jenkins020.PNG
   :width: 800 px
   :align: center

click on 'Full stack deployment' , that's the pipeline view for the same folder. 

.. image:: /docs/solutions/devsecops/images/jenkins030.PNG
   :width: 800 px
   :align: center
   
click on 'run' to start the dev environment pipeline. 

.. image:: /docs/solutions/devsecops/images/jenkins040.PNG
   :width: 800 px
   :align: center


you can review the output of each job while its running, click on the small 'console output' icon as shown in the screenshot:

.. image:: /docs/solutions/devsecops/images/jenkins050.PNG
   :width: 800 px
   :align: center
   
   
wait until all of the jobs have finished (turned green). 

.. image:: /docs/solutions/devsecops/images/jenkins060.PNG
   :width: 800 px
   :align: center

open slack - https://f5-rs.slack.com/messages/C9WLUB89F/
go to the 'builds' channel. 
use the search box on the upper right corner and filter by your username (student#). 
jenkins will send to this channel the bigip and the application address. 

.. image:: /docs/solutions/devsecops/images/Slack-040.PNG
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


.. image:: /docs/solutions/devsecops/images/hackazone010.PNG
   :width: 800 px
   :align: center

