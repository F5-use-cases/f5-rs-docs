Getting started
===================

This document explains how to use the F5 Reference solutions.

Slack channel:

https://f5-rs.slack.com

email me: yossi@f5.com

Run the rs-container - First time run on a new docker host 
----------------------------

.. code-block:: terminal

    docker run -t -d --name rs-container -v config:/home/snops/host_volume -p 2222:22 -p 10000:8080 --rm f5usecases/f5-rs-container

Configure credentials and personal information
------------------------------------------------------------

Taken out for now
   

SSH key configuration (if you don't have an existing SSH key you want to use):
------------------------------------------------------------------------------------

if this is the first time use key_gen to generate a new private and public key (accept all defaults)

.. code-block:: terminal

   ssh-keygen
   
store both private ad public key in a secure place on your laptop for future use!

.. code-block:: terminal

   cp ~/.ssh/* /home/snops && chmod a+rw /home/snops/id_rsa*

   
copy from /home/snops/id_rsa* to your laptop

SSH key configuration (if you do have an existing SSH key you want to use):
------------------------------------------------------------------------------------

copy files to /home/snops using your favorite tool

.. code-block:: terminal

   su root -c "chmod 400 /home/snops/id_rsa*  && mkdir /var/jenkins_home/.ssh && cp /home/snops/id_rsa* /var/jenkins_home/.ssh && chown jenkins:snops /var/jenkins_home/.ssh/id_rsa*"


Open Jenkins and relod jobs:
------------------------------------------------------------------------------------

Open Jenkins http://container-ip:10000 

usernmae: snops , password: default

reload jenkins configuration from disk http://container-ip:10000/reload


Run your first solution:
------------------------------------------------------------------------------------

start   aws waf with splunk /   A - start the use case automatic pipeline

this will start a pipeline that build aws-net, aws-app, aws autoscale bigip using bigiq license manager , aws-ELB

to verify start the "local-attacks" job and check if requests are getting blocked. 

app is accessible on the https://ELB-address 

a link to the app is in the 'console output' of the last job - rs-attacks yossir100

statistics are sent to splunk.


it takes about 20 minutes for the deploymnet to be ready, when done click on the 'rs-attacks yossir100' 
open the 'console output' and check if the malicious request was blocked (you should see a response that looks like: "The requested URL was rejected. Please consult with your administrator")

the hyperlink in the same page will take you to the app that was created 


.. image:: img/aws_waf_sp.png
   :align: center
   
   

.. |run_rs_container| raw:: html

   <a href="https://hub.docker.com/r/yossiros/f5-rs-container/" target="_blank">Docker hub page</a>

.. |install_ansible| raw:: html

   <a href="http://docs.ansible.com/ansible/latest/intro_installation.html" target="_blank">http://docs.ansible.com/ansible/latest/intro_installation.html</a>

.. |install_ansible_pip| raw:: html

   <a href="http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-pip" target="_blank">http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-pip</a>



