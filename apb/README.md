Road-to-Cloud-Native Workshop Installer
=========

The provided Ansible Playbook Bundle automates preparing an OpenShift cluster for the Road-to-Cloud-Native Labs 
by deploying required services (lab instructions, Gogs, Nexus, etc) which are used during the labs.

## Use an installed APB on OpenShift
Select the item from the catalog.

## Development
When you develop the APB you can test on a platform in the following ways.

### Install the apb tool
The new `apb` tool will help you develop your APB in an easy way. Download the latest apb tool 
from here: [https://github.com/automationbroker/apb/releases](https://github.com/automationbroker/apb/releases)

The [README.md](https://github.com/automationbroker/apb/blob/master/README.md) in that repository will give you a short getting started very insightful.

If you're starrting a new APB, you should look into the workflow in the [Getting started doc](https://github.com/automationbroker/apb/blob/master/docs/getting_started.md)

### Create a namespace to test
Once logged into your OCP cluster, you must first create a project:

```bash
oc new-project roadtocloudnative
```

### Update the com.redhat.apb.spec LABEL
If you've changed the apb.yml you will need to update the `com.redhat.apb.spec` LABEL with a base64 encoded version of apb.yml. To do this we need to run `apb bundle prepare`

```bash
apb bundle prepare
```

### Building and publishing your APB for testing

At this point we have a fully formed APB that we can build. In this example, we'll build an APB using the OpenShift build system. Let's first create a `buildconfig` for the APB. Perform this step when you're building a new APB that you haven't previously built on OpenShift:

```bash
oc new-build --binary=true --name workshop -n roadtocloudnative
```

To start the build from the current directory and follow the logs:

```bash
oc start-build --follow --from-dir . workshop -n roadtocloudnative
```

If the build completes successfully, you'll see some newly available imagestreams:

```bash
oc get is
NAME       DOCKER REPO                                                  TAGS     UPDATED
workshop   docker-registry.default.svc:5000/roadtocloudnative/workshop   latest   About a minute ago
```

We can now add the internal OpenShift registry to our list of configured registries:

```bash
apb registry add my-registry --type local_openshift --namespaces roadtocloudnative
Getting specs for registry: [my-registry]
INFO Empty AuthType. Assuming credentials are defined in the config...
INFO == REGISTRY CX ==
INFO Name: my-registry
INFO Type: local_openshift
INFO Url:
INFO Validating specs...
INFO All specs passed validation!
INFO Registry my-registry has 1 valid APBs available from 1 images scanned
 APB                           IMAGE                                                          REGISTRY
 ------------------------- -+- ---------------------------------------------------------- -+- -----------
 cloud-native-workshop-apb  |  docker-registry.default.svc:5000/roadtocloudnative/workshop  |  my-registry
```

### Running the APB
Now that `apb` is aware of the `road-to-cloud-native-workshop-apb` APB in the registry, we can perform any supported action against the APB. The `apb bundle` subcommand gives you some actions you can perform on a specific bundle in a registry.

We can `provision` the `road-to-cloud-native-workshop-apb` that now exists in the registry by running:

```bash
apb bundle provision road-to-cloud-native-workshop-apb -n roadtocloudnative --follow
```

The provision command will prompt you for all required parameters to your APB.

__NOTE__: The `--follow` flag used above will show logs produced by the running APB.

You can `deprovision` the `road-to-cloud-native-workshop-apb` that now exists in the registry by running:

```bash
apb bundle deprovision road-to-cloud-native-workshop-apb -n roadtocloudnative --follow
```

The deprovision command will prompt you for the specific instance to delete and all required parameters to your APB.

## Advanced options

|Variable                   | Default Value            | Description   |
|---------------------------|--------------------------|---------------|
|`openshift_user`      |  | Username of the admin user |
|`openshift_password`      |  | Password of the admin user |
|`user_count`      | 5 | Number of Participants  |
|`openshift_master_url`      |  | The address to OpenShift master URL to be displayed in the lab guide to participants  |
|`infrasvcs_adm_user`      | adminuser | Admin user for infrastructure services (Gogs, Nexus, ...) |
|`infrasvcs_adm_pwd`      | adminpwd | Admin password for infrastructure services (Gogs, Nexus, ...) |
