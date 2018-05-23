.. _f5_rs_aws_net:


f5_rs_aws_net - Deploys vpc and network objects to an AWS region
+++++++++++++++++++++++++++++++++++++++++++++

.. versionadded:: 0.9


.. contents::
   :local:
   :depth: 2


Synopsis
--------

* Deploys vpc and network objects to an AWS region.


Requirements (on host that executes module)
-------------------------------------------

  * f5-sdk >= 3.0.9
  * ansible >= 2.4
  * boto3 >= 1.6.4


Options
-------

.. raw:: html

    <table border=1 cellpadding=4>
    <tr>
    <th class="head">parameter</th>
    <th class="head">required</th>
    <th class="head">default</th>
    <th class="head">choices</th>
    <th class="head">comments</th>
    </tr>
                <tr><td>aws_region<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td>us-west-2</td>
        <td><ul><li>any aws region </li><li>any aws region</li></ul></td>
        <td><div> aws region in which the vpc will be created</div>        </td></tr>
                <tr><td>deploymentName<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td></td>
        <td></td>
        <td><div>unique name for deployment, must be firstname-first letter of last name and a 3 digit number - example  yossir100</div></td></tr>
        </table>
    </br>



Examples
--------

    
Deploy:

ansible-playbook --vault-password-file ~/.vault_pass.txt \
-i inventory/hosts playbooks/aws_net_deploy.yaml -e "\
deploymentName=yossir100 \
aws_region=us-west-2"

Destroy:

ansible-playbook --vault-password-file ~/.vault_pass.txt \
-i inventory/hosts playbooks/aws_net_deploy.yaml -e "\
deploymentName=yossir100 \
aws_region=us-west-2 \
cft_state=absent"


Return Values
-------------

Return values are stored in the following etcd path: 

f5_rs_aws_net/<deploymentName>/

.. raw:: html

    <table border=1 cellpadding=4>
    <tr>
    <th class="head">name</th>
    <th class="head">description</th>
    <th class="head">sample</th>
    </tr>

        <tr>
        <td> applicationSubnets </td>
        <td> application subnets </td>
        <td align=center> "subnet-bebc4cc7,subnet-b9d571e3" </td>
    </tr>
            <tr>
        <td> availabilityZone1 </td>
        <td> availabilityZone1 </td>
        <td align=center> us-west-2b </td>
    </tr>
            <tr>
        <td> availabilityZone2 </td>
        <td> availabilityZone2 </td>
        <td align=center> us-west-2c </td>
    </tr>
            <tr>
        <td> vpc </td>
        <td> vpc id </td>
        <td align=center> "vpc-676cd31e" </td>
    </tr>
            <tr>
        <td> subnets </td>
        <td> subnets </td>
        <td align=center> "subnet-21bc4c58,subnet-72ca6e28" </td>
    </tr>
        
    </table>
    </br></br>

Notes
-----

.. note::
    - For more information on using Ansible to manage F5 Networks devices see https://www.ansible.com/integrations/networks/f5.



Status
~~~~~~

This module is flagged as **preview** which means that it is not guaranteed to have a backwards compatible interface.


Support
~~~~~~~

This module is community maintained without core committer oversight.

For more information on what this means please read :doc:`/usage/support`


For help developing modules, should you be so inclined, please read :doc:`Getting Involved </development/getting-involved>`, :doc:`Writing a Module </development/writing-a-module>` and :doc:`Guidelines </development/guidelines>`.