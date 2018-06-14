Lab 2: (Dave) Account takeover protection (app layer encryption) - OPTIONAL 
----------------------------------

Background: 
~~~~~~~~~~~~~

Application is up and running, sales on the site have seen a big growth. our support center started getting complaints from customers 
that their account is abused and they are charged with purcheses they never did. 
after further investigation it turns out that the user's credentials were stolen by a malware on the client side. 

secops engineer suggests to turn on f5's application encryption on the login page, he configured a template profile with some settings that make sense for the enterprise. exposing the login page paramters (URI), and a choice to enable/disable. 


Task 4 - Enable application layer encryption 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

it is up to Dave now to deploy the new feature in DEV and promote to PROD when it makes sense for him. 

- Open the container CLI 
- make sure you are connected as user 'jenkins' 
- go to the application git folder. check which branches are there and what is the active branch. (git branch) 
- you should be on the 'dev' branch. the files you see belong to the dev branch. 

.. code-block:: terminal

   cd /home/snops/f5-rs-app2
   git checkout dev
   git branch
   
 
- edit the iac_parameters.yaml file to enable login password encryption, 
- change the setting from:

  + login_password_encryption: "disabled"

  + to:

  + login_password_encryption: "enabled"

- add the file to git and commit 

.. code-block:: terminal

   vi iac_parameters.yaml 
   git add iac_parameters.yaml
   git commit -m "enabled login password encryption"
   
   
- go back to jenkins and open the 'f5-rs-app2-dev ' folder. choose the 'waf policy' tab , jenkins is set up to monitor the application repo. when a 'commit' is identified jenkins will start an automatic pipeline to deploy the service. it takes up to a minute for jenkins to start the pipeline. 

- jenkins takes the parametes from the git repo and uses them to deploy/update the service. 

- log on to the dev bigip again, check the setting on the FPS profile.

	|ale-bigip-010|
   

this concludes the tests in the 'dev' environment. we are now ready to push the changes to production. 
we will 'merge' the app2 dev branch with the master branch so that the production deployment will use the correct policy. 
on the /home/snops/f5-rs-app2 folder:

.. code-block:: terminal
 
   git checkout master
   git merge dev -m "enabled login password encryption"

the merge will trigger a job in jenkins that's configured to monitor this repo - 'Push waf policy', open the f5-rs-app2-prd folder and navigate to the 'service deployment pipeline' , you should see the jobs running in up to a minute.  

open the PRODUCTION bigip, check that the FPS profile named rs_fps has the 'login_password_encryption' enabled. 
   
   
.. |pbd-bigip-010| image:: images/pbd-bigip-010.PNG 
   
.. ||pbd-bigip-020|| image:: images/|pbd-bigip-020|.PNG 
   
.. |ale-bigip-010| image:: images/ale-bigip-010.PNG
   
.. |jenkins040| image:: images/jenkins040.PNG
   
.. |jenkins050| image:: images/jenkins050.PNG
   
.. |jenkins060| image:: images/jenkins060.PNG
   
.. |jenkins070| image:: images/jenkins070.PNG
   
.. |hackazone010| image:: images/hackazone010.PNG
