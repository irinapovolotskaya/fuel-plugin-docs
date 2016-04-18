System testing
==============

* This section provides information on the test cases that are obligatory for your plugin.

  * These cases serve to make sure that your plugin does not break the Mirantis OpenStack environments.

* Please note that Mirantis recommends that you used the HA environment configuration for testing purposes:

  * At least three controllers must be configured to have a reliable HA environment;
    this is the minimum requirement for quorum-based clusters, such as Pacemaker and Galera.
    The controller cluster can include more than three servers to increase the level of reliability.
    For more information about how Fuel deploys HA, see
    `Multi-node with HA Deployment <https://docs.mirantis.com/openstack/fuel/fuel-7.0/reference-architecture.html#multi-node-ha>`_.
    
.. note:: System testing includes OSTF as the tool recommended by Mirantis.

Install plugin and deploy environment
-------------------------------------

Test Case ID: install_plugin_deploy_env

Steps to reproduce:

#. Copy the plugin to the Fuel Master node (please refer to the User Guide for more details).

#. Install the plugin.

#. Ensure that plugin is installed successfully with running `fuel plugins --list` command in the Fuel CLI.

#. Create an environment with enabled plugin in the Fuel Web UI.

#. Add 3 nodes with Controller role and 1 node with Compute and  another role.

#. Finalize environment configuration (e.g. networking, nodes interfaces).

#. Run network verification check.

#. Deploy the cluster.

#. Run OSTF.

Expected result:

* Plugin is installed successfully at the Fuel Master node and the corresponding output appears in the CLI.

*  Cluster is created and network verification check is passed.

* OSTF tests (Health Checks) are passed.

* Environment is deployed successfully.

Modifying env with enabled plugin (removing/adding controller nodes)
--------------------------------------------------------------------

Test Case ID: modify_env_with_plugin_controller

Steps to reproduce:

#. Copy the plugin to the Fuel Master node (please refer to the User Guide for more details).

#. Install the plugin.

#. Ensure that the plugin is installed successfully using CLI with running `fuel plugins --list` command in the Fuel CLI.

#. Create an environment with enabled plugin in the Fuel Web UI.

#. Add 3 nodes with Controller role and 1 node with Compute and  another role.

#. Finalize environment configuration (e.g. networking, nodes interfaces).

#. Enable the plugin and configure it following the instructions from the Plugin Guide.

#  Run network verification check.

#. Deploy the cluster.

#. Run OSTF.

#. Remove 1 node with Controller role (i.e. remove the primary Controller node which should have the lowest ID,
   where plugin’s services are running to ensure that all plugins resources are migrated to another Controller node). 
   
#. Re-deploy the cluster.

#. Run OSTF.

#. Add  1 new node with Controller role.

#. Re-deploy the cluster.

#. Run OSTF.

Expected result:

* Plugin is installed successfully at the Fuel Master node and the corresponding output appears in the CLI.

* Cluster is created and network verification check is passed.

* Plugin is enabled and configured in the Fuel Web UI.

* OSTF tests (Health Checks) are passed.

* Environment is deployed successfully.

* When adding/removing Controller node  (where plugin-related services are run):

  * all plugins resources are migrated to another Controller node
  
  * the environment is redeployed successfully when adding/removing Controller node.
  
 Modifying environment with enabled plugin (removing/adding Compute node)
 ------------------------------------------------------------------------
 
 Test Case ID: modify_env_with_plugin_compute
 
 Steps to reproduce:
 
#. Copy the plugin to the Fuel Master node (please refer to the User Guide for more details).

#. Install the plugin.

#. Ensure that the plugin is installed successfully using CLI with running `fuel plugins --list` command in the Fuel CLI.

#. Create an environment with enabled plugin in the Fuel Web UI.

#. Add 3 nodes with Controller role and 1 node with Compute and  another role.

#. Finalize environment configuration (e.g. networking, nodes interfaces).

#. Enable the plugin and configure it following the instructions from the Plugin Guide.

#. Run network verification check.

#. Deploy the cluster.

#. Run OSTF.

#. Remove 1 node with Compute role
   (i.e. remove the node, where plugin’s services are running to ensure that
   all plugins resources are migrated to another Compute node).
   
#. Re-deploy the cluster.

#. Run OSTF.

#. Add  1 new node with Compute role.

#. Re-deploy the cluster.

#. Run OSTF.

Expected result:

* Plugin is installed successfully at the Fuel Master node and the corresponding output appears in the CLI.

* Cluster is created and network verification check is passed.

* Plugin is enabled and configured in the Fuel Web UI.

* OSTF tests (Health Checks) are passed.

* Environment is deployed successfully.

* When adding/removing Compute node  (where plugin-related services are run):

  * all plugins resources are migrated to another Controller node
  
  * the environment is redeployed successfully when adding/removing Compute node.
  
  
Fuel create mirror and update (setup) of core repos
---------------------------------------------------

Test Case ID: fuel-create-mirror

Steps to reproduce:

#. Copy the plugin to the Fuel Master node (please refer to the User Guide for more details).

#. Install the plugin.

#. Ensure that the plugin is installed successfully using CLI with running `fuel plugins --list` command in the Fuel CLI.

#. Create an environment with enabled plugin in the Fuel Web UI.

#. Add 3 nodes with Controller role and 1 node with Compute and  another role.

#. Finalize environment configuration (e.g. networking, nodes interfaces).

#. Enable the plugin and configure it following the instructions from the Plugin Guide.

#. Run network verification check.

#. Deploy the cluster.

#. Run OSTF.

#. Log into controller/compute/storage/etc nodes via Fuel CLI and get PID of services which were
   launched by plugin and store them.
   
#. Launch the following command on the Fuel Master node::

     fuel-createmirror -M
     
#.  Run the command below on the Fuel Master node:

    * For MOS < 8.0::
    
       fuel --env <ENV_ID> node --node-id <NODE_ID1> <NODE_ID2> <NODE_ID_N> --tasks upload_core_repos
       
    * For MOS 8.0::
    
       fuel --env <ENV_ID> node --node-id <NODE_ID1> <NODE_ID2> <NODE_ID_N> --tasks setup_repositories
       
#. Log into controller/compute/storage/etc nodes and check if plugin's services are alive and if their
   PID are changed.
   
#. Check if all nodes remain in ready status.

#. Rerun OSTF.

Expected result:

* Plugin is installed successfully at the Fuel Master node and the corresponding output appears in the CLI.

* Cluster is created and network verification check is passed.

* Plugin is enabled and configured in the Fuel Web UI.

* OSTF tests (Health Checks) are passed.

* Environment is deployed successfully.

* When adding/removing Compute node (where plugin-related services are run):

  * all plugins resources are migrated to another Compute node
  
  * the environment is re-deployed successfully when adding/removing Compute node.
  
* Plugin's services shouldn't be restarted after corresponding task was executed:

  * if they are restarted as some exception, this information should be added to plugin's User Guide. 
  
 #.Cluster (nodes) should remain in ready state.
 
 #. OSTF test should be passed on rerun.

