
Installing <plugin name> plugin
===============================

To install the <plugin name> plugin, follow these steps:

#. Download it from the `Fuel Plugins Catalog`_.

#. Copy the plugin's RPM to the Fuel Master node (if you don't
   have the Fuel Master node, please see `the official
   Mirantis OpenStack documentation <https://docs.mirantis.com/openstack/fuel/fuel-8.0/quickstart-guide.html#installing-mirantis-openstack-manually>`_)::

      [root@home ~]# scp <plugin filename> root@fuel-master:/tmp

#. Log into Fuel Master node and install the plugin using the
   `Fuel CLI <https://docs.mirantis.com/openstack/fuel/fuel-8.0/user-guide.html#using-fuel-cli>`_::

      [root@fuel-master ~]# fuel plugins --install <plugin filename>

#. Verify that the plugin is installed correctly::


     [root@fuel-master ~]# fuel plugins
     id | name          | version | package_version
     ---|---------------|---------|----------------
     1  | <name>        |<version>| <version>

.. _Fuel Plugins Catalog: https://www.mirantis.com/products/openstack-drivers-and-plugins/fuel-plugins/
