AWS auto-scale WAF with splunk 
================================


This solution deploys the following components to your AWS account:

- AWS VPC with 6 subnets 
- Containerized Hackazon app in an autoscale group of EC2 instances (t2.micro) 
- F5 WAF+ADC in an auto scale group (m3.xlarge) 
- External ELB to distribute the load across F5'S 


Solution diagram:
------------------------------------------------------------------------------------

	|lab-diag-010|
	
   
on jenkins main page, click on the folder - "aws waf with splunk"

	|jenkins10.PNG|


Click on the "aws waf stack 01" tab

	|jenkins102.PNG|
	
.. image:: img/jenkins102.PNG
   :align: center

click on "run" to start the solution pipeline:

.. image:: img/jenkins11.PNG
   :align: center

choose the region in which you want to deploy the stack and click "build":

Wait until the stack is ready (takes about 10-15 minutes). you should see all of the jobs in green. 

if one of the jobs failed, try to run in again, if it still deosn't work send me a note: yossi@f5.com

.. image:: img/jenkins12.PNG
   :align: center
   
   


   
  
.. |jenkins10| image:: images/jenkins10.PNG
.. |lab-diag-010| image:: images/aws_waf_sp.png
.. |jenkins102| image:: images/jenkins102.PNG









