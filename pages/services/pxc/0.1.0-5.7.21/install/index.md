---
layout: layout.pug
navigationTitle: Install and Customize
excerpt: Installing and customizing the DC/OS Percona XtraDB Cluster service
title: Install and Customize
menuWeight: 15
model: /services/pxc/data.yml
render: mustache
---


 DC/OS {{ model.techName}} is available in the Universe and can be installed by using either the web interface or the DC/OS CLI.

The default DC/OS {{ model.techName}} Service installation provides reasonable defaults for trying out the service, but that may not be sufficient for production use. You may require different configurations depending on the context of the deployment.


# Configuration Best Practices for Production 

- Increase the number of TCP socket ports available. This is particularly important if the flow will be setting up and tearing down a large number of sockets in small period of time.
    
    ```
    sudo sysctl -w net.ipv4.ip_local_port_range="10000 65000"
    ```
    
- Tell Linux you never want {{ model.techName}} to swap. Swapping is fantastic for some applications. It isn’t good for something like {{ model.techName}} that always wants to be running. To tell Linux to turn swapping off, you can edit `/etc/sysctl.conf` to add the following line
        
    ```
    vm.swappiness = 0
    ```
        
For the partitions handling the various {{ model.techName}} repos, turn off things like `atime`. Doing so can cause a surprising bump in throughput. Edit the `/etc/fstab` file and for the partition(s) of interest add the `noatime` option.

# Prerequisites
   
- If you are using Enterprise DC/OS, you may [need to provision a service account](/latest/security/ent/service-auth/custom-service-auth/) before installing DC/OS {{ model.techName}} Service. Only someone with `superuser` permission can create the service account.
  - `strict` [security mode](/latest/security/ent/service-auth/custom-service-auth/) requires a service account.
  - In `permissive` security mode a service account is optional.
  - `disabled` security mode does not require a service account.
- Your cluster must have at least {{ model.install.nodePrerequisite }}.

# Installing from the DC/OS CLI

To start a basic test cluster of {{ model.techName}}, run the following command on the DC/OS CLI. Enterprise DC/OS users must follow additional instructions.

   ```shell
   dcos package install {{ model.serviceName}} 
   ```

This command creates a new instance with the default name `{{ model.serviceName}}`. Two instances cannot share the same name, so installing additional instances beyond the default instance requires customizing the name at install time for each additional instance. However, the application can be installed using the same name in case of foldered installation, wherein we can install the same application in different folders.

All DC/OS {{ model.techName}} CLI commands have a `--name`  argument allowing the user to specify which instance to query. If you do not specify a service name, the CLI assumes a default value matching the package name, for example, `{{ model.serviceName}}`. The default value for `--name` can be customized via the DC/OS CLI configuration:

   ```shell
   dcos {{ model.serviceName}} --name={{ model.serviceName}} <cmd>
   ```

You can specify a custom configuration in an `options.json` file and pass it to `dcos package install` using the `--options` parameter.

   ```shell
   dcos package install {{ model.serviceName}} --options=options.json
   ```

For more information on building the `options.json` file, see [DC/OS documentation](https://docs.mesosphere.com/latest/usage/managing-services/config-universe-service/) for service configuration access.

## Installing from the DC/OS web interface

Alternatively, you can install {{ model.techName}} from the DC/OS web interface by clicking on **Deploy** after selecting the app from the Catalog. If you install {{ model.techName}} from the DC/OS web interface, the `dcos {{ model.serviceName}}` CLI commands are not automatically installed to your workstation. You can install them manually, using the DC/OS CLI:


   ```shell
   dcos package install {{ model.serviceName}} --cli
   ```

## Installing multiple instances

By default, the {{ model.techName}} service is installed with a service name of {{ model.serviceName}}. You may specify a different name using a custom service configuration as follows:

   ```shell
   {
       "service": {
           "name": "pxc-other"
       }
   }
   ```

When the above JSON configuration is passed to the `package install {{ model.serviceName}}`  command via the      argument, the new service will use the name specified in that JSON configuration:

   ```shell
   dcos package install {{ model.serviceName}} --options={{ model.serviceName}}-other.json
   ```
   
Multiple instances of {{ model.techName}} may be installed into your DC/OS cluster by customizing the name of each instance. For example, you might have one instance of {{ model.techName}} named `{{ model.serviceName}}-staging` and another named `{{ model.serviceName}}-prod`, each with its own custom  configuration.

After specifying a custom name for your instance, it can be reached using `dcos {{ model.serviceName}}` CLI commands or directly over HTTP as described below.

<p class="message--note"><strong>NOTE: </strong>The service name <strong>cannot</strong> be changed after initial install. Changing the service name would require installing a new instance of the service against the new name, then copying over any data as necessary to the new instance.</p>

## Installing into folders

In DC/OS 1.10 and later, services may be installed into folders by specifying a slash-delimited service name. For example:

```shell
{
    "service": {
        "name": "/foldered/path/to/{{ model.serviceName}}"
    }
}
```
The above example will install the service under a path of `foldered => path => to => {{ model.serviceName}}`. It can then be reached using `dcos {{ model.serviceName}}` CLI commands, or directly over HTTP as described below.

<p class="message--note"><strong>NOTE: </strong>The service folder location <strong>cannot</strong> be changed after initial install. Changing the service folder location would require installing a new instance of the service against the new location, then copying over any data as necessary to the new instance.</p>

## Addressing named instances

After you have installed the service under a custom name or under a folder, it may be accessed from all `dcos {{ model.serviceName}}` CLI commands using the `--name` argument. By default, the `--name` value defaults to the name of the package, or `{{ model.serviceName}}`.

For example, if you had an instance named `{{ model.serviceName}}-dev`, the following command would invoke a `pod list` command against it:

   ```shell
   dcos {{ model.serviceName}} --name={{ model.serviceName}}-dev pod list
   ```
The same query would be over HTTP as follows:

   ```shell
   curl -H "Authorization:token=$auth_token" <dcos_url>/service/{{ model.serviceName}}-dev/v1/pod
   ```
Likewise, if you had an instance in a folder like `/foldered/path/to/{{ model.serviceName}}`, the following command would invoke a `pod list` command against it:

   ```shell
   dcos {{ model.serviceName}} --name=/foldered/path/to/{{ model.serviceName}} pod list
   ```
   
Similarly, it could be queried directly over HTTP as follows:

   ```shell
   curl -H "Authorization:token=$auth_token" <dcos_url>/service/foldered/path/to/{{ model.serviceName}}-dev/v1/pod
   ```

<p class="message--note"><strong>NOTE: </strong>You may add a <code>-v</code> (verbose) argument to any <code>dcos percona-pxc-mysql</code> command to see the underlying HTTP queries that are being made. This can be a useful tool to see where the CLI is getting its information. In practice, <code>dcos percona-pxc-mysql</code> commands are a thin wrapper around an HTTP interface provided by the DC/OS Percona XtraDB Cluster Service itself.</p>

## Virtual Networks

DC/OS {{ model.techName}} supports deployment on virtual networks on DC/OS, allowing each container (task) to have its own IP address and not use port resources on the agent machines. This can be specified by passing the following configuration during installation:

   ```shell
   {
       "service": {
           "virtual_network_enabled": true
       }
   }
   ```
<p class="message--note"><strong>NOTE: </strong>Once the service is deployed on a virtual network, it <strong>cannot</strong> be updated to use the host network.</p>


## Minimal Installation

For development purposes, you may wish to install {{ model.techName}} on a local DC/OS cluster. For this, you can use `dcos-docker` or `dcos-vagrant`. To start a minimal cluster with a single broker, create a JSON options file named `sample-pxc-minimal.json`:

   ```shell
   {
       "node": {
       "count": 3,
       "mem": 512,
       "cpu": 0.5
       }
   }
   ```
The command below creates a cluster using `sample-{{ model.serviceName}}-minimal.json`:


   ```shell
   dcos package install {{ model.serviceName}} --options=sample-{{ model.serviceName}}-minimal.json
   ```

## Interacting with your foldered service

1. Interact with your foldered service via the DC/OS CLI with this flag: `--name=/path/to/myservice`.
2. To interact with your foldered service over the web directly, use <a href="http://<dcos-url>/service/path/to/myservice">http://<dcos-url>/service/path/to/myservice</a>. For example: <a href="http://<dcos-url>/service/testing/percona-pxc-mysql/v1/endpoints">http://<dcos-url>/service/testing/percona-pxc-mysql/v1/endpoints</a>.

## Placement Constraints

Placement constraints allow you to customize where a service is deployed in the DC/OS cluster. Depending on the service, some or all components may be configurable using Marathon operators. For example, [["hostname", "UNIQUE"]] ensures that at most one pod instance is deployed per agent.

A common task is to specify a list of whitelisted systems to deploy to. To achieve this, use the following syntax for the placement constraint:
   ```shell
   [["hostname", "LIKE", "10.0.0.159|10.0.1.202|10.0.3.3"]]
   ```
You must include spare capacity in this list, so that if one of the whitelisted systems goes down, there is still enough room to repair your service (via `pod replace`) without requiring that system.

**Example**

In order to define placement constraints as part of an install or update of a service, they should be provided as a JSON encoded string. For example, one can define a placement constraint in an options file as follows:

   ```shell
   cat options.json
   {
       "hello": {
       "placement": "[[\"hostname\", \"UNIQUE\"]]"
       }
   }
   ```
This file can be referenced to install a {{ model.serviceName}} service.

   ```shell
   dcos package install hello-world --options=options.json
   ```
Likewise this file can be referenced to update a {{ model.serviceName}} service.

   ```shell
   dcos {{ model.serviceName}} update start --options=options.json
   ```

## Regions and Zones

Placement constraints can be applied to zones by referring to the @zone key. For example, one could spread pods across a minimum of three different zones by specifying the constraint:

When the region awareness feature is enabled (currently in beta), the @region key can also be referenced for defining placement constraints. Any placement constraints that do not reference the @region key are constrained to the local region.

**Example**

   ```shell
   [["@zone", "GROUP_BY", "3"]]
   ```
Suppose we have a Mesos cluster with three zones. For balanced placement across those three zones, we would have a configuration like this:

   ```shell
   {
   "count": 6,
   "placement": "[[\"@zone\", \"GROUP_BY\", \"3\"]]"
   }
   ```
Instances will all be evenly divided between those three zones.

## Secured Installation
  Please refer to the Security Guide for secured installation of [pxc](/security).
