.. f5_rs_aws_external_lb:


f5_rs_aws_external_lb - Creates an ELB on a given AWS vpc
+++++++++++++++++++++++++++++++++++++++++++++++++++

.. versionadded:: 2.4


.. contents::
   :local:
   :depth: 2


Synopsis
--------

* Creates an ELB on a given AWS vpc


Requirements (on host that executes module)
-------------------------------------------

  * f5-sdk >= 3.0.9


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
    </tr>
                <tr><td>subnets<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td>none</td>
        <td><ul><li>aws subnet in azA </li><li>aws subnet in azB</li></ul></td>
        <td><div> aws subnets in which ELB will be available </div>        </td></tr>
    </tr>
                <tr><td>vs_port<br/><div style="font-size: small;"></div></td>
    <td>no</td>
    <td>443</td>
        <td><ul><li>444 </li><li> * </li></ul></td>
        <td><div> the listener on the ELB, default is 443. default health check is HTTPS </div>        </td></tr>
        </table>
    </br>



Examples
--------

	Deploy:
	
	ansible-playbook --vault-password-file ~/.vault_pass.txt \
	playbooks/aws_external_elb_deploy.yaml -e "\
	deploymentName=yossir100 \
	service_name=App1 \
	aws_region="$(etcdctl get f5-rs-aws-net/yossir100/aws_region)" \
	vpc="$(etcdctl get f5-rs-aws-net/yossir100/vpc)" \
	subnets="$(etcdctl get f5-rs-aws-net/yossir100/subnets)""
	
	Destroy:
	
	ansible-playbook --vault-password-file ~/.vault_pass.txt \
	playbooks/aws_external_elb_deploy.yaml -e "\
	deploymentName=yossir100 \
	service_name=App1 \
	aws_region="$(etcdctl get f5-rs-aws-net/yossir100/aws_region)" \
	vpc="$(etcdctl get f5-rs-aws-net/yossir100/vpc)" \
	subnets="$(etcdctl get f5-rs-aws-net/yossir100/subnets)"
	state=absent"


Return Values
-------------
	Return values are stored in the following etcd path: 
	
	f5_rs_aws_external_lb/<deploymentName>/


.. raw:: html

	<table style="width: 828px;" border="1" cellpadding="4">
	<tbody>
	<tr>
	<th class="head" style="width: 128px;">name</th>
	<th class="head" style="width: 187px;">description</th>
	<th class="head" style="width: 475px;">sample</th>
	</tr>
	<tr>
	<td style="width: 128px;">bigipELBDnsName</td>
	<td style="width: 187px;">dns name of the ELB</td>
	<td style="width: 475px;" align="center">"username-yossir100-App1-BigipElb-1130768938.us-west-2.elb.amazonaws.com"</td>
	</tr>
	<tr>
	<td style="width: 128px;">bigipElasticLoadBalancer</td>
	<td style="width: 187px;">Id of ELB</td>
	<td style="width: 475px;" align="center">username-yossir100-App1-BigipElb</td>
	</tr>
	<tr>
	<td style="width: 128px;">externalLBSecurityGroup</td>
	<td style="width: 187px;">Security Group for external LB of BIG-IP</td>
	<td style="width: 475px;" align="center">sg-df867ca1</td>
	</tr>
	</tbody>
	</table>

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