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

  - SSH to RS-CONTAINER 
  - SSH to the linux  host 
  - HTTP Access to Jenkins (only available after you start the lab) 


1.1 Connect using SSH to the RS-CONTAINER
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In UDF navigate to the  :guilabel:`Deployments` 

#. Click the :guilabel:`Details` button for your DevSecOps Deployment

#. Click the :guilabel:`Components` tab

#. Find the ``Linux Jumphost`` Component and click the the :guilabel:`ACCESS`
   button.
   
#. use your favorite SSH client to connect to :guilabel:`RS-CONTAINER` using your UDF private key. username is :guilabel:`root`


1.2 Configure the rs-container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The entire lab is built from code hosted in this repo, the container that you are connecting to runs on the linux host
and is publicly available. to run the deployments you need to configure it with personal information and credentials. 


       
1.3 Configure credentials and personal information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

   
1.3.1 Copy ssh key, aws credentials and global parameters file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

the SSH key will be used when creating EC2 instances.  
we will store them in the Jenkins SSH folder so that Jenkins can use them to access instances.

Copy credentials and parameters files from the host folder using the following script: 

.. code-block:: terminal

   /home/snops/host_volume/udf_startup.sh
   

1.3.2 Edit the global parameters file with your personal information 
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

   vault_dac_user: "student01" // username IS case sensitive
   vault_dac_email: "yossi@f5.com"
   vault_dac_password: "Sup3rsecur3Passw0rd1"

- Press the ``ESC`` key and save the file by typing: ``:wq``  

* After you save the ``f5-rs-global-vars-vault.yaml`` file for the first time you get an error message, ignore it it's a bug
  ERROR! Unexpected Exception, this is probably a bug: [Errno 1] Operation not permitted: '/home/snops/f5-rs-global-vars-vault.yaml'

1.3.3 Configure jenkins and reload it
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to configure jenkins with your personal information and reload it: 

.. code-block:: terminal

   ansible-playbook --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml


   
- Start: :ref:`module1`

