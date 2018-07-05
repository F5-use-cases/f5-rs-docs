Initial setup 
---------------

1. Configure the rs-container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
   
1.1.3-(F5 employees ONLY) - Create parameters file on the host volume 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- copy paste the text from https://hive.f5.com/people/rosenboim/blog/2018/06/28/rs-global-parameters-file
- If using UDF you should already have the file, skip to 1.1.4

.. code-block:: terminal

   vi /home/snops/host_volume/f5-rs-global-vars-vault.yaml
   
1.1.3-(External users) - Create parameters file on the host volume 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

copy paste the example file to the host volume

.. code-block:: terminal

   cp  /home/snops/f5-rs-global-vars-vault.yaml /home/snops/host_volume/f5-rs-global-vars-vault.yaml

   
1.1.4 Edit the global parameters file with your personal information 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^   
   
- Edit the encrypted global parameters file `/home/snops/host_volume/f5-rs-global-vars-vault.yaml` by typing:

.. code-block:: terminal

   ansible-vault edit --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/host_volume/f5-rs-global-vars-vault.yaml

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

1.2 Run the container startup script 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Run the container startup script with the following command:
- The script will download the repos again and copy files from the host volume you just populated to the relevant directories 

.. code-block:: terminal

   /snopsboot/start


2. Start a solution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List of available solutions:
 
.. toctree::
   :maxdepth: 1
   :caption: Solutions
   :glob:

   /solutions/*/*_index

   


   

