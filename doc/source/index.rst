====================================
Designate role for OpenStack-Ansible
====================================

This Ansible role installs and configures OpenStack Designate.

This role will install the following services:
    * designate-api
    * designate-central
    * designate-worker
    * designate-producer
    * designate-mdns
    * designate-sink

The DNS servers Designate will interface with can be defined in the
``designate_pools_yaml`` variable. This is eventually written to the Designate
`pools.yaml <https://docs.openstack.org/designate/latest/admin/pools.html#managing-pools>`_
file.

To clone or view the source code for this repository, visit the role repository
for `os_designate <https://opendev.org/openstack/openstack-ansible-os_designate>`_.

Default variables
~~~~~~~~~~~~~~~~~

.. literalinclude:: ../../defaults/main.yml
   :language: yaml
   :start-after: under the License.

Adding The Service to Your OpenStack-Ansible Deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add a new service to your OpenStack-Ansible (OSA) deployment:

* Define ``designate_pools_yaml`` variable as described above.

* Define ``dnsaas_hosts`` in your ``conf.d`` or ``openstack_user_config.yml``.
  For example:

  .. code-block:: yaml

      dnsaas_hosts:
        infra1:
          ip: 172.20.236.111
        infra2:
          ip: 172.20.236.112
        infra3:
          ip: 172.20.236.113

* Create respective LXC containers (skip this step for metal deployments):

  .. code-block:: console

     openstack-ansible openstack.osa.containers_lxc_create --limit designate_all,dnsaas_hosts

* Run service deployment playbook:

  .. code-block:: console

     openstack-ansible openstack.osa.designate

For more information, please refer to the `OpenStack-Ansible project documentation <https://docs.openstack.org/project-deploy-guide/openstack-ansible/latest/>`_.

Always verify that the integration is successful and that the service behaves
correctly before using it in a production environment.

Dependencies
~~~~~~~~~~~~

This role needs the following variables defined:

.. code-block:: yaml

    designate_galera_address
    designate_galera_password
    designate_service_password
    designate_oslomsg_rpc_password
    designate_oslomsg_notify_password

Example playbook
~~~~~~~~~~~~~~~~

.. literalinclude:: ../../examples/playbook.yml
   :language: yaml

Tags
~~~~

This role supports two tags: ``designate-install`` and ``designate-config``.
The ``designate-install`` tag can be used to install and upgrade. The
``designate-config`` tag can be used to maintain configuration of the service.
