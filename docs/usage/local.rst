Running the container on your docker host
------------------------------------------

.. NOTE:: The following instructions will create a volume on your docker host and will instruct you 
          to store private information in the host volume. the information in the volume will persist 
		  on the host even after the container is terminated. 

1.  run the rs-container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: terminal

   docker run -t -d --name rs-container -v config:/home/snops/host_volume -p 2222:22 -p 10000:8080 --rm f5usecases/f5-rs-container
 
 The container exposes the following access methods:

  - SSH to RS-CONTAINER ssh://localhsot:2222
  - HTTP Access to Jenkins http://localhost:10000 (only available after you start the lab) 

1.1 Connect using SSH to the RS-CONTAINER
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  - SSH to dockerhost:2222 
  - username: :guilabel:`root`
  - password: :guilabel:`default`



1.2 Configure the container
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   
- Move on to configure the container:

.. toctree::
   :maxdepth: 1
   :glob:

   first_time_run
   regular_run
