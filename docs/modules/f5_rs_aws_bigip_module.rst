.. f5_rs_aws_bigip:


f5_rs_aws_bigip - deploys bigip in AWS using CFT
+++++++++++++++++++++++++++++++++++++++++++++++++++

.. versionadded:: 2.4


.. contents::
   :local:
   :depth: 2


Synopsis
--------

* deploys bigip in AWS using CFT, currently supports only the WAF-autoscale CFT


Requirements (on host that executes module)
-------------------------------------------

  * aws


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
        <td><div> expecting two subnets in the format of {subnet1, subnet2} </div>        </td></tr>
    </tr>
                <tr><td>service_name<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td>app1</td>
        <td><ul><li>app1 </li><li> up to 6 letters </li></ul></td>
        <td><div> service name for identification  </div>        </td></tr>
    </tr>
                <tr><td>vpc<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td>none</td>
        <td><ul><li>vpc-isdfr </li><li>  </li></ul></td>
        <td><div> vpc in which bigip's will be deployed  </div>        </td></tr>
    </tr>
                <tr><td>bigipElasticLoadBalancer<br/><div style="font-size: small;"></div></td>
    <td>yes</td>
    <td>none</td>
        <td><ul><li>elb-name-dsfsd </li><li> </li></ul></td>
        <td><div> ELB for scaling out the bigip's  </div>        </td></tr>
        </table>
    </br>



Examples
--------

	Deploy:
	
	ansible-playbook --vault-password-file ~/.vault_pass.txt \
	playbooks/aws_bigip_deploy.yaml -e " \
	deploymentName=yossir100 \
	service_name=App1 \
	aws_region="$(etcdctl get f5-rs-aws-net/yossir100/aws_region)" \
	vpc="$(etcdctl get f5-rs-aws-net/yossir100/vpc)" \
	subnets="$(etcdctl get f5-rs-aws-net/yossir100/subnets)" \
	bigipElasticLoadBalancer="$(etcdctl get f5-rs-aws-external-lb/yossir100/bigipElasticLoadBalancer)" \
	applicationPoolTagValue="$(etcdctl get f5-rs-aws-app/yossir100/appAutoscaleGroupName)""
	
	Destroy:
	
	ansible-playbook --vault-password-file ~/.vault_pass.txt \
	playbooks/aws_bigip_deploy.yaml -e " \
	deploymentName=yossir100 \
	service_name=App1 \
	aws_region="$(etcdctl get f5-rs-aws-net/yossir100/aws_region)" \
	vpc="$(etcdctl get f5-rs-aws-net/yossir100/vpc)" \
	subnets="$(etcdctl get f5-rs-aws-net/yossir100/subnets)" \
	bigipElasticLoadBalancer="$(etcdctl get f5-rs-aws-external-lb/yossir100/bigipElasticLoadBalancer)" \
	applicationPoolTagValue="$(etcdctl get f5-rs-aws-app/yossir100/appAutoscaleGroupName)"
	state=absent"

Return Values
-------------
	Return values are stored in the following etcd path: 
	
	f5_rs_aws_app/<deploymentName>/


.. raw:: html

	<table style="width: 828px;" border="1" cellpadding="4">
	<tbody>
	<tr>
	<th class="head" style="width: 128px;">name</th>
	<th class="head" style="width: 187px;">description</th>
	<th class="head" style="width: 475px;">sample</th>
	</tr>
	<tr>
	<td style="width: 128px;">appAutoscaleGroupName</td>
	<td style="width: 187px;">auto scale group name of the app in EC2</td>
	<td style="width: 475px;" align="center">"yossir-demo1-App1-application-appAutoscaleGroup-SXQKA5PH9TI"</td>
	</tr>
	<tr>
	<td style="width: 128px;">appInternalDnsName</td>
	<td style="width: 187px;">internal DNS name of the ELB fronting the app</td>
	<td style="width: 475px;" align="center">internal-demo1-App1-AppElb-1123840165.us-west-2.elb.amazonaws.comb</td>
	</tr>
	<tr>
	<td style="width: 128px;">appInternalElasticLoadBalancer</td>
	<td style="width: 187px;">Id of ELB for App Pool</td>
	<td style="width: 475px;" align="center">demo1-App1-AppElb</td>
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