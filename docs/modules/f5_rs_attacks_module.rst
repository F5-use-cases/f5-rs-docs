.. _f5_rs_attacks:


f5_rs_attacks - Run attacks on an HTTP/S target
+++++++++++++++++++++++++++++++++++++++++++++++++++

.. versionadded:: 2.4


.. contents::
   :local:
   :depth: 2


Synopsis
--------

* Run attacks on an HTTP/S target


Requirements (on host that executes module)
-------------------------------------------

  * curl


Options
-------

.. raw:: html

	<table border="1" cellpadding="4">
	<tbody>
	<tr>
	<th class="head">parameter</th>
	<th class="head">required</th>
	<th class="head">default</th>
	<th class="head">choices</th>
	<th class="head">comments</th>
	</tr>
	<tr>
	<td>https_target<br />
	<div style="font-size: small;">&nbsp;</div>
	</td>
	<td>yes</td>
	<td>none</td>
	<td>
	<ul>
	<li>any aws region</li>
	</ul>
	</td>
	<td>
	<div>aws region in which the vpc will be created</div>
	</td>
	</tr>
	</tbody>
	</table>



Examples
--------

 ::

    
	Run:
	ansible-playbook -vv --vault-password-file ~/.vault_pass.txt \
	playbooks/http_attacks/cmd_attack.yaml -e "\
	https_target="$(etcdctl get f5-rs-aws-external-lb/yossir100/bigipELBDnsName)""

Return Values
-------------

	Returns the HTTP response from the server 


Notes
-----

.. note::
    - For more information on using Ansible to manage F5 Networks devices see https://www.ansible.com/integrations/networks/f5.
    - Requires the f5-sdk Python package on the host. This is as easy as ``pip install f5-sdk``.



Status
~~~~~~

This module is flagged as **preview** which means that it is not guaranteed to have a backwards compatible interface.


Support
~~~~~~~

This module is community maintained without core committer oversight.

For more information on what this means please read :doc:`/usage/support`


For help developing modules, should you be so inclined, please read :doc:`Getting Involved </development/getting-involved>`, :doc:`Writing a Module </development/writing-a-module>` and :doc:`Guidelines </development/guidelines>`.