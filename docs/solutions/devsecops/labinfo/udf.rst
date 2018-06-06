F5 Unified Demo Framework (UDF)
-------------------------------

.. NOTE:: This environment is currently available for F5 employees only

Determine how to start your deployment:

- **Official Events (ISC, SSE Summits):**  Please follow the
  instructions given by your instructor to join the UDF Course.

- **Self-Paced/On Your Own:** Login to UDF,
  :guilabel:`Deploy` the
  ``Security Lab: devsecops``
  Blueprint and :guilabel:`Start` it.

Connecting to the Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To connect to the lab environment we will use SSH to the jumphost. 

SSH key has to be configured in UDF in order to access the jumphost. 


  The lab environment provides several access methods to the Jumphost:

  - SSH to the linux  host 
  - SSH to the SNOPS container (only available after you start the lab)
  - HTTP Access to Jenkins (only available after you start the lab) 


Connect using SSH to the Linux Host 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the UDF navigate to your :guilabel:`Deployments`

#. Click the :guilabel:`Details` button for your Deployment

#. Click the :guilabel:`Components` tab

#. Find the ``Linux Jumphost`` Component and click the the :guilabel:`Details`
   button.

#. use your favorite SSH client to connect using your private key.


#. Select how you would like to continue:


Run the rs-container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The entire lab is built from code hosted in this repo, in order to launch the lab environment you will download and run a container that has the tools we are using (ansible and jenkins) as well as the depndencies and requirements to interact with the differnet services (F5, AWS, github.. ) 
on the linux jumphost in UDF, run the following command to start the container,
the will attach a volume from the linux host to the container


.. code-block:: terminal

    sudo docker run -v config:/home/snops/host_volume -p 2222:22 -p 10000:8080 -it --rm f5usecases/f5-rs-container



Configure credentials and personal information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

the following script will configure jenkins with your information and reload it. 

.. code-block:: terminal

   ansible-playbook --vault-password-file ~/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml


   - Start: :ref:`module1`

