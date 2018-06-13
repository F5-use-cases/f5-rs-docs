DevSecOps - Advanced WAF in a CI/CD Workflow
===========================================

This lab covers the following topics:

 - Shifting WAF policies `left <https://en.wikipedia.org/wiki/Shift_left_testing>`, closer to Dev 
 - Declarative Advanced WAF

Lab Goals: 
~~~~~~~~~~

 + Describe devsecops main concepts and how they translate into an actual environemnt
 + Describe the different roles in a devsecops workflow (secops, dev, devops)
 + Describe workflow with F5 application security integrated into the pipeline 

Roles in the lab: 
~~~~~~~~~~~~~~~~ 
 + secops - represents an app security engineer
 + dave - represents an application dev team member that is responsible for the app and pushing it in the pipeline. 
 + devops/automation/SRE - aren't represented in the lab. their role builds the tools we operate in this lab (the automation pipeline of the security and infrastructure) 

OUT OF SCOPE: 
~~~~~~~~~~~~~ 
 + the "How to" of the automation pieces. 
 + for the "how to" please refer to the F5 super netops training. 
 
Expected time to complete: **2 hours**

To continue please review the information about the Lab Environment.

.. toctree::
   :maxdepth: 1
   :glob:

   labinfo/labinfo
   module*/module*
   conclusion
