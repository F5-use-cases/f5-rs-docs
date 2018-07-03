Running the container on your docker host
------------------------------------------

.. NOTE:: The following instructions will create a volume on your docker host and will instruct you 
          to store private information in the host volume. the information in the volume will persist 
		  on the host even after the container is terminated. 


1.0 Copy ssh key, aws credentials and global parameters file
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

the SSH key will be used when creating EC2 instances.  
we will store them in the Jenkins SSH folder so that Jenkins can use them to access instances.

Copy credentials and parameters files from the host folder using the following script: 

.. code-block:: terminal

   /home/snops/host_volume/udf_startup.sh
       
1.1 Configure jenkins and reload it
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Run the following command to configure jenkins with your personal information and reload it: 

.. code-block:: terminal

   ansible-playbook --vault-password-file /var/jenkins_home/.vault_pass.txt /home/snops/f5-rs-jenkins/playbooks/jenkins_config.yaml


   
- Start: :ref:`module1`

