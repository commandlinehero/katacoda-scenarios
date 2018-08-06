A playbook based on a SCAP Security Guide (SSG) profile contains fixes for all rules, and the system is remediated according to the profile regardless of the state of the machine. On the other hand, playbooks based on scan results contain only fixes for rules that failed during an evaluation.

In Red Hat Enterprise Linux 7.5, Red Hat provides pre-built Ansible playbooks for many compliance profiles. The playbooks are stored in the /usr/share/scap-security-guide/ansible/ directory. You can apply the pre-generated Ansible playbooks provided by the scap-security-guide in this directory on your host.

Alternatively, to generate an Ansible playbook based on a profile (for example, the DISA STIG profile for Red Hat Enterprise Linux 7), enter the following command:

`oscap xccdf generate fix --fix-type ansible \
--profile xccdf_org.ssgproject.content_profile_stig-rhel7-disa \
--output stig-rhel7-role.yml \
/usr/share/xml/scap/ssg/content/ssg-rhel7-ds.xml`{{execute}}

To generate an Ansible playbook based on the results of a scan, enter the following command:

$ oscap xccdf generate fix --fix-type ansible \
--result-id "" \
--output stig-playbook-result.yml \
results.xml

where the results.xml file contains results of the scan obtained when scanning with the –results option and the result-id option contains an ID of the TestResult component in the file with results. In the example, above, we are using empty result-id. This is a trick to avoid specifying the full result ID.

To apply the Ansible playbook, enter the following command:

$ ansible-playbook -i inventory.ini stig-playbook-result.yml

Note that the ansible-playbook command is provided by the ansible package. See the ansible-playbook(1) man page and the Ansible Tower User Guide for more information.

The atomic scan command enables users to use OpenSCAP scanning capabilities to scan docker-formatted container images and containers on the system. It is possible to scan for known CVE vulnerabilities or for configuration compliance. Additionally, users can remediate docker-formatted container images to the specified policy.

The OpenSCAP scanner and SCAP content are provided in a container image that allows for easier updating and and deployment of the scanning tools.  The `atomic scan` command enables the evaluation of Red Hat Enterprise Linux based container images and running containers against any provided SCAP profile.

For example, here is how to scan the container for configuration compliance to the RHEL 7 DISA STIG profile.

$ sudo atomic scan --scan_type configuration_compliance \
 --scaner_args profile=stig-rhel7-disa, report registry.access.redhat.com/rhel7:latest

To remediate docker-formatted container images to the specified policy, you need to add the –remediate option to the atomic scan command when scanning for configuration compliance. The following command builds a new remediated container image compliant with the DISA STIG policy from the Red Hat Enterprise Linux 7 container image:

$ sudo atomic scan --remediate --scan_type configuration_compliance \
--scanner_args profile=xccdf_org.ssgproject.content_profile_stig-rhel7-disa,report \
registry.access.redhat.com/rhel7:latest

This is your first step.

## Task

This is an _example_ of creating a scenario and running a **command**

`echo 'Hello World'`{{execute}}
