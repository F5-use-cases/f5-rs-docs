Running the container for the first time on a host
------------------------------------------

1.0 Configure the rs-container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The entire lab is built from code hosted in this repo.
To run the deployments you need to configure your personal information and credentials. 

.. NOTE:: You will be asked to configure sensitive parameters like AWS credentials.
          those are used to deploy resources on your account. those cloud resources will appear on your cloud account 
		  it is your responsibility to use it responsibly and shut down the instances when done. 
       
1.1 Configure credentials and personal information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following steps are required only in the first time you run the container on a host, 
this information persists on the host and will be available for you on any subsequent runs. 

1.1.1 Create an AWS credentials file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- create an AWS credentials file by typing:

.. code-block:: terminal

   vi /home/snops/host_volume/credentials

- Copy and paste the following (and change to your keys):   
   
.. code-block:: terminal

   [default]
   aws_access_key_id = CHANGE_TO_ACCESS_KEY
   aws_secret_access_key = CHANGE_TO_SECRET

   
1.1.2 Create a personal SSH key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The SSH key will be used when creating EC2 instances.  
we will store them in the host-volume so they will persist any container restart:

.. code-block:: terminal

   mkdir -p /home/snops/host_volume/sshkeys
   ssh-keygen -f /home/snops/host_volume/sshkeys/id_rsa -t rsa -N ''  

1.1.3 Edit the global parameters file with your personal information 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^   
   
- Edit the encrypted global parameters file `/home/snops/f5-rs-global-vars-vault.yaml` by typing:

.. code-block:: terminal

   ansible-vault edit --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/f5-rs-global-vars-vault.yaml

- Once in edit mode - type ``i`` to activate INSERT mode and configure your personal information by changing the following variables: ``vault_dac_user``, ``vault_dac_email`` and ``vault_dac_password``
- use your f5 username for ``vault_dac_user`` - used as a Tenant ID to differentiate between multiple deployments
- Choose your own (secure) value for ``vault_dac_password`` - ** this is the password for the ``admin`` user of the BIG-IP **
- There are a number of special characters that you should avoid using in passwords for F5 products. See https://support.f5.com/csp/article/K2873 for details

For example:

.. code-block:: terminal

   vault_dac_user: "rosenboim"
   vault_dac_email: "yossi@f5.com"
   vault_dac_password: "Sup3rsecur3Passw0rd1"

- Press the ``ESC`` key and save the file by typing: ``:wq``  

1.1.4 copy the parameters file to your host volume so it will persist after container restart 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to copy the parameters file: 

.. code-block:: terminal

   cp /home/snops/f5-rs-global-vars-vault.yaml /home/snops/host_volume/f5-rs-global-vars-vault.yaml


1.1.4 copy the parameters file to your host volume so it will persist after container restart 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to copy the parameters file: 

.. code-block:: terminal

   cp /home/snops/f5-rs-global-vars-vault.yaml /home/snops/host_volume/f5-rs-global-vars-vault.yaml

   
1.2 Run the normal startup script 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After the initial setup your personal information is now on the host volume and will persist so you won't need to repeat those steps. 
you will now run the script to copy the files you created on the host volume to the relevant libraries. 

Copy credentials and parameters files from the host folder using the following script: 

.. code-block:: terminal

   /home/snops/startup.sh
   
   
1.3 Configure jenkins and reload it
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to configure jenkins with your personal information and reload it: 

.. code-block:: terminal

   ansible-playbook --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml


   

