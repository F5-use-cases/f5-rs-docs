F5 Unified Demo Framework (UDF)
-------------------------------

.. NOTE:: This environment is currently available for F5 employees only

Determine how to start your deployment:

- **Official Events (ISC, SSE Summits):**  Please follow the
  instructions given by your instructor to join the UDF Course.

- **Self-Paced/On Your Own:** Login to UDF,
  :guilabel:`Deploy` the 
  :guilabel:`Security Lab: DevSecOps`
  Blueprint and :guilabel:`Start` it.

1.  Connecting to the Environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To connect to the lab environment we will use SSH to the jumphost. 

SSH key has to be configured in UDF in order to access the jumphost. 


  The lab environment provides several access methods to the Jumphost:

  - SSH to the linux  host 
  - HTTP Access to Jenkins (only available after you start the lab) 


1.1 Connect using SSH to the Linux Host 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In UDF navigate to your :guilabel:`Deployments`

#. Click the :guilabel:`Details` button for your Deployment

#. Click the :guilabel:`Components` tab

#. Find the ``Linux Jumphost`` Component and click the the :guilabel:`Details`
   button.

#. use your favorite SSH client to connect using your private key.


#. Select how you would like to continue:


1.2 Run the rs-container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The entire lab is built from code hosted in this repo, in order to launch the lab environment you will download and run a container that has the tools we are using (ansible and jenkins) as well as the depndencies and requirements to interact with the differnet services (F5, AWS, github.. ) 
on the linux jumphost in UDF, run the following command to start the container,
the will attach a volume from the linux host to the container


.. code-block:: terminal

    sudo docker run --name rs-container -v config:/home/snops/host_volume -p 2222:22 -p 10000:8080 -it --rm f5usecases/f5-rs-container


.. Note:: after running the docker run command you are immediatly 'attached' to the container.

   if you open another window to the linux host you need to attach to the container again. 
   
   to attach use the command: sudo docker attach rs-container
       

1.3 Configure credentials and personal information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1.3.1 log in as jenkins (root password is 'default')
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
jenkins user is used so that the config changes we do are available to jenkins

.. code-block:: terminal

   su root -c "su jenkins"
   
   
1.3.2 Copy ssh key, aws credentials and global parameters file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

the SSH key will be used when creating EC2 instances.  
we will store them in the Jenkins SSH folder so that Jenkins can use them to access instances.

Copy credentials and parameters files from the host folder using the following commands: 

.. code-block:: terminal

   ssh-keygen -f /var/jenkins_home/.ssh/id_rsa -t rsa -N ''
   cp /home/snops/host_volume/f5-rs-global-vars-vault.yaml /home/snops/f5-rs-global-vars-vault.yaml
   mkdir /var/jenkins_home/.aws && cp /home/snops/host_volume/credentials /var/jenkins_home/.aws/credentials
   echo password > /var/jenkins_home/.vault_pass.txt
   

1.3.3 Edit the global parameters file with your personal information 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   
- Edit the encrypted global parameters file ``/home/snops/f5-rs-global-vars-vault.yaml`` by typing:

.. code-block:: terminal

   ansible-vault edit --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/f5-rs-global-vars-vault.yaml

- Once in edit mode - type ``i`` to activate INSERT mode and configure your personal information by changing the following variables: ``vault_dac_user``, ``vault_dac_email`` and ``vault_dac_password``
- Use your student# from Teams for ``vault_dac_user`` - used as a Tenant ID to differentiate between multiple deployments
- Choose your own (secure) value for ``vault_dac_password`` - ** this is the password for the ``admin`` user of the BIG-IP **
- There are a number of special characters that you should avoid using in passwords for F5 products. See https://support.f5.com/csp/article/K2873 for details

For example:

.. code-block:: terminal

   vault_dac_user: "student01"
   vault_dac_email: "yossi@f5.com"
   vault_dac_password: "Sup3rsecur3Passw0rd1"

- Press the ``ESC`` key and save the file by typing: ``:wq``  

* After you save the ``f5-rs-global-vars-vault.yaml`` file for the first time you get an error message, ignore it it's a bug
  ERROR! Unexpected Exception, this is probably a bug: [Errno 1] Operation not permitted: '/home/snops/f5-rs-global-vars-vault.yaml'

1.3.4 Configure jenkins and reload it
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to configure jenkins with your personal information and reload it: 

.. code-block:: terminal

   ansible-playbook --vault-password-file ~/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml


   
- Start: :ref:`module1`

