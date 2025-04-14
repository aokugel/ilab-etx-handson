# Red Hat OpenShift AI Self-Managed

# Red Hat OpenShift AI Self-Managed

1. Installing and uninstalling OpenShift AI Self-Managed in a disconnected environment
2. Preface
3. 1. Architecture of OpenShift AI Self-Managed
4. 2. Understanding update channels
5. 3. Deploying OpenShift AI in a disconnected environment
    1. Deploying OpenShift AI in a disconnected environment
    2. 3.1. Requirements for OpenShift AI Self-Managed
    3. 3.2. Adding administrative users in OpenShift
    4. 3.3. Mirroring images to a private registry for a disconnected installation
    5. 3.4. Configuring custom namespaces
    6. 3.5. Installing the Red Hat OpenShift AI Operator
        1. Installing the Red Hat OpenShift AI Operator
        2. 3.5.1. Installing the Red Hat OpenShift AI Operator by using the CLI
        3. 3.5.2. Installing the Red Hat OpenShift AI Operator by using the web console
    7. 3.6. Installing and managing Red Hat OpenShift AI components
        1. Installing and managing Red Hat OpenShift AI components
        2. 3.6.1. Installing Red Hat OpenShift AI components by using the CLI
        3. 3.6.2. Installing Red Hat OpenShift AI components by using the web console
        4. 3.6.3. Updating the installation status of Red Hat OpenShift AI components by using the web console
6. 4. Installing the distributed workloads components
7. 5. Installing the single-model serving platform
    1. Installing the single-model serving platform
    2. 5.1. About the single-model serving platform
    3. 5.2. Configuring automated installation of KServe
    4. 5.3. Manually installing KServe
        1. Manually installing KServe
        2. 5.3.1. Installing KServe dependencies
        3. 5.3.2. Creating an OpenShift Service Mesh instance
        4. 5.3.3. Creating a Knative Serving instance
        5. 5.3.4. Creating secure gateways for Knative Serving
        6. 5.3.5. Installing KServe
        7. 5.3.6. Configuring persistent volume claims (PVC) on KServe
        8. 5.3.7. Disabling KServe dependencies
    5. 5.4. Adding an authorization provider for the single-model serving platform
        1. Adding an authorization provider for the single-model serving platform
        2. 5.4.1. Manually adding an authorization provider
        3. 5.4.2. Installing the Red Hat Authorino Operator
        4. 5.4.3. Creating an Authorino instance
        5. 5.4.4. Configuring an OpenShift Service Mesh instance to use Authorino
        6. 5.4.5. Configuring authorization for KServe
8. 6. Installing the multi-model serving platform
9. 7. Accessing the dashboard
10. 8. Enabling accelerators
11. 9. Working with certificates
    1. Working with certificates
    2. 9.1. Understanding certificates in OpenShift AI
        1. Understanding certificates in OpenShift AI
        2. 9.1.1. How CA bundles are injected
        3. 9.1.2. How the ConfigMap is managed
    3. 9.2. Adding a CA bundle
    4. 9.3. Removing a CA bundle
    5. 9.4. Removing a CA bundle from a namespace
    6. 9.5. Managing certificates
    7. 9.6. Accessing S3-compatible object storage with self-signed certificates
    8. 9.7. Using self-signed certificates with OpenShift AI components
        1. Using self-signed certificates with OpenShift AI components
        2. 9.7.1. Using certificates with data science pipelines
            1. Using certificates with data science pipelines
            2. 9.7.1.1. Providing a CA bundle only for data science pipelines
        3. 9.7.2. Using certificates with workbenches
12. 10. Viewing logs and audit records
    1. Viewing logs and audit records
    2. 10.1. Configuring the OpenShift AI Operator logger
        1. Configuring the OpenShift AI Operator logger
        2. 10.1.1. Viewing the OpenShift AI Operator log
    3. 10.2. Viewing audit records
13. 11. Troubleshooting common installation problems
    1. Troubleshooting common installation problems
    2. 11.1. The Red Hat OpenShift AI Operator cannot be retrieved from the image registry
    3. 11.2. OpenShift AI does not install on unsupported infrastructure
    4. 11.3. The creation of the OpenShift AI Custom Resource (CR) fails
    5. 11.4. The creation of the OpenShift AI Notebooks Custom Resource (CR) fails
    6. 11.5. The OpenShift AI dashboard is not accessible
    7. 11.6. Reinstalling OpenShift AI fails with an error
    8. 11.7. The dedicated-admins Role-based access control (RBAC) policy cannot be created
    9. 11.8. The PagerDuty secret does not get created
    10. 11.9. The SMTP secret does not exist
    11. 11.10. The ODH parameter secret does not get created
    12. 11.11. Data science pipelines are not enabled after installing OpenShift AI 2.9 or later due to existing Argo Workflows resources
14. 12. Uninstalling Red Hat OpenShift AI Self-Managed
    1. Uninstalling Red Hat OpenShift AI Self-Managed
    2. 12.1. Understanding the uninstallation process
    3. 12.2. Uninstalling OpenShift AI Self-Managed by using the CLI
15. Legal Notice

# Installing and uninstalling OpenShift AI Self-Managed in a disconnected environment

#### Install and uninstall OpenShift AI Self-Managed in a disconnected environment

Abstract

Install and uninstall OpenShift AI Self-Managed on your OpenShift cluster in a disconnected environment.

### Preface

Learn how to use both the OpenShift command-line interface and web console to install Red Hat OpenShift AI Self-Managed on your OpenShift cluster in a disconnected environment. To uninstall the product, learn how to use the recommended command-line interface (CLI) method.

Note

Red Hat does not support installing more than one instance of OpenShift AI on your cluster.

Red Hat does not support installing the Red Hat OpenShift AI Operator on the same cluster as the Red Hat OpenShift AI Add-on.

### Chapter 1. Architecture of OpenShift AI Self-Managed

Red Hat OpenShift AI Self-Managed is an Operator that is available in a self-managed environment, such as Red Hat OpenShift Container Platform, or in Red Hat-managed cloud environments such as Red Hat OpenShift Dedicated (with a Customer Cloud Subscription for AWS or GCP), Red Hat OpenShift Service on Amazon Web Services (ROSA Classic or ROSA HCP), or Microsoft Azure Red Hat OpenShift.

OpenShift AI integrates the following components and services:

- At the service layer:
				OpenShift AI dashboard
								A customer-facing dashboard that shows available and installed applications for the OpenShift AI environment as well as learning resources such as tutorials, quick starts, and documentation. Administrative users can access functionality to manage users, clusters, notebook images, accelerator profiles, and model-serving runtimes. Data scientists can use the dashboard to create projects to organize their data science work.
							Model serving
								Data scientists can deploy trained machine-learning models to serve intelligent applications in production. After deployment, applications can send requests to the model using its deployed API endpoint.
							Data science pipelines
								Data scientists can build portable machine learning (ML) workflows with data science pipelines 2.0, using Docker containers. With data science pipelines, data scientists can automate workflows as they develop their data science models.
							Jupyter (self-managed)
								A self-managed application that allows data scientists to configure their own notebook server environment and develop machine learning models in JupyterLab.
							Distributed workloads
								Data scientists can use multiple nodes in parallel to train machine-learning models or process data more quickly. This approach significantly reduces the task completion time, and enables the use of larger datasets and more complex models.
- At the management layer:
				The Red Hat OpenShift AI Operator
								A meta-operator that deploys and maintains all components and sub-operators that are part of OpenShift AI.
							Monitoring services
								Prometheus gathers metrics from OpenShift AI for monitoring purposes.

When you install the Red Hat OpenShift AI Operator in the OpenShift cluster, the following new projects are created:

- The redhat-ods-operator project contains the Red Hat OpenShift AI Operator.
- The redhat-ods-applications project installs the dashboard and other required components of OpenShift AI.
- The redhat-ods-monitoring project contains services for monitoring.
- The rhods-notebooks project is where notebook environments are deployed by default.

You or your data scientists must create additional projects for the applications that will use your machine learning models.

Do not install independent software vendor (ISV) applications in namespaces associated with OpenShift AI.

### Chapter 2. Understanding update channels

You can use update channels to specify which Red Hat OpenShift AI minor version you intend to update your Operator to. Update channels also allow you to choose the timing and level of support your updates have through the fast, stable, stable-x.y eus-x.y, and alpha channel options.

The subscription of an installed Operator specifies the update channel, which is used to track and receive updates for the Operator. You can change the update channel to start tracking and receiving updates from a newer channel. For more information about the release frequency and the lifecycle associated with each of the available update channels, see the Red Hat OpenShift AI Self-Managed Life Cycle Knowledgebase article.

| Channel    | Support                                                                            | Release frequency   | Recommended environment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|------------|------------------------------------------------------------------------------------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fast       | One month of full support                                                          | Every month         | Production environments with access to the latest product features.     							Select this streaming channel with automatic updates to avoid manually upgrading every month.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| stable     | Three months of full support                                                       | Every three months  | Production environments with stability prioritized over new feature availability.     							Select this streaming channel with automatic updates to access the latest stable release and avoid manually upgrading.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| stable-x.y | Seven months of full support                                                       | Every three months  | Production environments with stability prioritized over new feature availability.     							Select numbered stable channels (such as stable-2.10) to plan and upgrade to the next stable release while keeping your deployment under full support.                                                                                                                                                                                                                                                                                                                                                                                  |
| eus-x.y    | Seven months of full support followed by Extended Update Support for eleven months | Every nine months   | Enterprise-grade environments that cannot upgrade within a seven month window.     							Select this streaming channel if you prioritize stability over new feature availability.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| alpha      | One month of full support                                                          | Every month         | Development environments with early-access features that might not be functionally complete.     							Select this channel to use early-access features to test functionality and provide feedback during the development process. Early-access features are not supported with Red Hat production service level agreements (SLAs).     							For more information about the support scope of Red Hat Technology Preview features, see Technology Preview Features Support Scope. 						  							For more information about the support scope of Red Hat Developer Preview features, see Developer Preview Features Support Scope. |

Note

The embedded and beta channels are legacy channels that will be removed in a future release. Do not select the embedded or beta channels for a new Operator installation.

### Chapter 3. Deploying OpenShift AI in a disconnected environment

Read this section to understand how to deploy Red Hat OpenShift AI as a development and testing environment for data scientists in a disconnected environment. Disconnected clusters are on a restricted network, typically behind a firewall. In this case, clusters cannot access the remote registries where Red Hat provided OperatorHub sources reside. Instead, the Red Hat OpenShift AI Operator can be deployed to a disconnected environment using a private registry to mirror the images.

Installing OpenShift AI in a disconnected environment involves the following high-level tasks:

1. Confirm that your OpenShift cluster meets all requirements. See Requirements for OpenShift AI Self-Managed.
2. Add administrative users for OpenShift. See Adding administrative users in OpenShift.
3. Mirror images to a private registry. See Mirroring images to a private registry for a disconnected installation.
4. Install the Red Hat OpenShift AI Operator. See Installing the Red Hat OpenShift AI Operator.
5. Install OpenShift AI components. See Installing and managing Red Hat OpenShift AI components.
6. Configure user and administrator groups to provide user access to OpenShift AI. See Adding users to OpenShift AI user groups.
7. Provide your users with the URL for the OpenShift cluster on which you deployed OpenShift AI. See Accessing the OpenShift AI dashboard.
8. Optionally, configure and enable your accelerators in OpenShift AI to ensure that your data scientists can use compute-heavy workloads in their models. See Enabling accelerators.

#### 3.1. Requirements for OpenShift AI Self-Managed

You must meet the following requirements before you can install Red Hat OpenShift AI on your Red Hat OpenShift cluster in a disconnected environment:

Product subscriptions

- You must have a subscription for Red Hat OpenShift AI Self-Managed.
					
						Contact your Red Hat account manager to purchase new subscriptions. If you do not yet have an account manager, complete the form at https://www.redhat.com/en/contact to request one.

Cluster administrator access to your OpenShift cluster

- You must have an OpenShift cluster with cluster administrator access. Use an existing cluster or create a cluster by following the OpenShift Container Platform documentation: Installing a cluster in a disconnected environment.
- After you install a cluster, configure the Cluster Samples Operator by following the OpenShift Container Platform documentation: Configuring Samples Operator for a restricted cluster.
- Your cluster must have at least 2 worker nodes with at least 8 CPUs and 32 GiB RAM available for OpenShift AI to use when you install the Operator. To ensure that OpenShift AI is usable, additional cluster resources are required beyond the minimum requirements.
- To use OpenShift AI on single node OpenShift, the node has to have at least 32 CPUs and 128 GiB RAM.
- Your cluster is configured with a default storage class that can be dynamically provisioned.
					
						Confirm that a default storage class is configured by running the oc get storageclass command. If no storage classes are noted with (default) beside the name, follow the OpenShift Container Platform documentation to configure a default storage class: Changing the default storage class. For more information about dynamic provisioning, see Dynamic provisioning.
- Open Data Hub must not be installed on the cluster.

For more information about managing the machines that make up an OpenShift cluster, see Overview of machine management.

An identity provider configured for OpenShift

- Red Hat OpenShift AI uses the same authentication systems as Red Hat OpenShift Container Platform. See Understanding identity provider configuration for more information on configuring identity providers.
- Access to the cluster as a user with the cluster-admin role; the kubeadmin user is not allowed.

Internet access on the mirroring machine

- Along with Internet access, the following domains must be accessible to mirror images required for the OpenShift AI Self-Managed installation:
- Along with Internet access, the following domains must be accessible to mirror images required for the OpenShift AI Self-Managed installation:
    - cdn.redhat.com
    - subscription.rhn.redhat.com
    - registry.access.redhat.com
    - registry.redhat.io
    - quay.io
- For CUDA-based images, the following domains must be accessible:
- For CUDA-based images, the following domains must be accessible:
    - ngc.download.nvidia.cn
    - developer.download.nvidia.com

Create custom namespaces

- By default, OpenShift AI uses predefined namespaces, but you can define a custom namespace for the operator and DSCI.applicationNamespace as needed. Namespaces created by OpenShift AI typically include openshift or redhat in their name. Do not rename these system namespaces because they are required for OpenShift AI to function properly. If you are using custom namespaces, before installing the OpenShift AI Operator, you must have created and labeled them as required.

Data science pipelines preparation

- Data science pipelines 2.0 contains an installation of Argo Workflows. If there is an existing installation of Argo Workflows that is not installed by data science pipelines on your cluster, data science pipelines will be disabled after you install OpenShift AI. Before installing OpenShift AI, ensure that your cluster does not have an existing installation of Argo Workflows that is not installed by data science pipelines, or remove the separate installation of Argo Workflows from your cluster.
- Before you can execute a pipeline in a disconnected environment, you must upload the images to your private registry. For more information, see Mirroring images to run pipelines in a restricted environment.
- You can store your pipeline artifacts in an S3-compatible object storage bucket so that you do not consume local storage. To do this, you must first configure write access to your S3 bucket on your storage account.

Install KServe dependencies

- To support the KServe component, which is used by the single-model serving platform to serve large models, you must also install Operators for Red Hat OpenShift Serverless and Red Hat OpenShift Service Mesh and perform additional configuration. For more information, see About the single-model serving platform.
- If you want to add an authorization provider for the single-model serving platform, you must install the Red Hat - Authorino Operator. For information, see Adding an authorization provider for the single-model serving platform.

Install model registry dependencies (Technology Preview feature)

- To use the model registry component, you must also install Operators for Red Hat Authorino, Red Hat OpenShift Serverless, and Red Hat OpenShift Service Mesh. For more information about configuring the model registry component, see Configuring the model registry component.

Access to object storage

- Components of OpenShift AI require or can use S3-compatible object storage such as AWS S3, MinIO, Ceph, or IBM Cloud Storage. An object store is a data storage mechanism that enables users to access their data either as an object or as a file. The S3 API is the recognized standard for HTTP-based access to object storage services.
- The object storage must be accessible to your OpenShift cluster. Deploy the object storage on the same disconnected network as your cluster.
- Object storage is required for the following components:
- Object storage is required for the following components:
    - Single- or multi-model serving platforms, to deploy stored models. See Deploying models on the single-model serving platform or Deploying a model by using the multi-model serving platform.
    - Data science pipelines, to store artifacts, logs, and intermediate results. See Configuring a pipeline server and About pipeline logs.
- Object storage can be used by the following components:
- Object storage can be used by the following components:
    - Workbenches, to access large datasets. See Adding a connection to your data science project.
    - Distributed workloads, to pull input data from and push results to. See Running distributed data science workloads from data science pipelines.
    - Code executed inside a pipeline. For example, to store the resulting model in object storage. See Overview of pipelines in Jupyterlab.

#### 3.2. Adding administrative users in OpenShift

Before you can install and configure OpenShift AI for your data scientist users, you must obtain OpenShift cluster administrator (cluster-admin) privileges.

To assign cluster-admin privileges to a user, follow the steps in the relevant OpenShift documentation:

- OpenShift Container Platform: Creating a cluster admin
- OpenShift Dedicated: Managing OpenShift Dedicated administrators
- ROSA: Creating a cluster administrator user for quick cluster access

#### 3.3. Mirroring images to a private registry for a disconnected installation

You can install the Red Hat OpenShift AI Operator to your OpenShift cluster in a disconnected environment by mirroring the required container images to a private container registry. After mirroring the images to a container registry, you can install Red Hat OpenShift AI Operator by using OperatorHub.

You can use the mirror registry for Red Hat OpenShift, a small-scale container registry, as a target for mirroring the required container images for OpenShift AI in a disconnected environment. Using the mirror registry for Red Hat OpenShift is optional if another container registry is already available in your installation environment.

Prerequisites

- You have cluster administrator access to a running OpenShift Container Platform cluster, version 4.14 or greater.
- You have credentials for Red Hat OpenShift Cluster Manager (https://console.redhat.com/openshift/).
- Your mirroring machine is running Linux, has 100 GB of space available, and has access to the Internet so that it can obtain the images to populate the mirror repository.
- You have installed the OpenShift CLI (oc).
- If you plan to use NVIDIA GPUs, you have mirrored and deployed the NVIDIA GPU Operator. See Configuring the NVIDIA GPU Operator in the OpenShift Container Platform documentation.
- If you plan to use data science pipelines, you have mirrored the OpenShift Pipelines operator.
- If you plan to use the single-model serving platform to serve large models, you have mirrored the Operators for Red Hat OpenShift Serverless and Red Hat OpenShift Service Mesh. For more information, see Serving large models.
- If you plan to use the distributed workloads component, you have mirrored the Ray cluster image.

Note

This procedure uses the oc-mirror plugin v2 (the oc-mirror plugin v1 is now deprecated). For more information, see Changes from oc-mirror plugin v1 to v2 in the OpenShift documentation.

Procedure

1. Create a mirror registry. See Creating a mirror registry with mirror registry for Red Hat OpenShift in the OpenShift Container Platform documentation.
2. To mirror registry images, install the oc-mirror OpenShift CLI plugin v2 on your mirroring machine running Linux. See Installing the oc-mirror OpenShift CLI plugin in the OpenShift Container Platform documentation.
					Important
							The oc-mirror plugin v1 is deprecated. Red Hat recommends that you use the oc-mirror plugin v2 for continued support and improvements.
3. Create a container image registry credentials file that allows mirroring images from Red Hat to your mirror. See Configuring credentials that allow images to be mirrored in the OpenShift Container Platform documentation.
4. Open the example image set configuration file (rhoai-&lt;version&gt;.md) from the disconnected installer helper repository and examine its contents.
5. Using the example image set configuration file, create a file called imageset-config.yaml and populate it with values suitable for the image set configuration in your deployment.
6. Using the example image set configuration file, create a file called imageset-config.yaml and populate it with values suitable for the image set configuration in your deployment.
    - To view a list of the available OpenShift versions, run the following command. This might take several minutes. If the command returns errors, repeat the steps in Configuring credentials that allow images to be mirrored.
							oc-mirror list operators
    - To see the available channels for a package in a specific version of OpenShift Container Platform (for example, 4.18), run the following command:
							oc-mirror list operators --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.18 --package=&lt;package\_name&gt;
    - For information about subscription update channels, see Understanding update channels.
							Important
									The example image set configurations are for demonstration purposes only and might need further alterations depending on your deployment.
								
									To identify the attributes most suitable for your deployment, examine the documentation and use cases in Mirroring images for a disconnected installation by using the oc-mirror plugin v2.
								
								Your imageset-config.yaml should look similar to the following example, where openshift-pipelines-operator-rh is required for data science pipelines, and both serverless-operator and servicemeshoperator are required for the KServe component.
							kind: ImageSetConfiguration
apiVersion: mirror.openshift.io/v2alpha1
mirror:
  operators:
    - catalog: registry.redhat.io/redhat/redhat-operator-index:v4.18
      packages:
        - name: rhods-operator
          defaultChannel: fast
          channels:
            - name: fast
              minVersion: 2.18.0
              maxVersion: 2.18.0
        - name: openshift-pipelines-operator-rh
          channels:
            - name: stable
        - name: serverless-operator
          channels:
            - name: stable
        - name: servicemeshoperator
          channels:
            - name: stable
7. Download the specified image set configuration to a local file on your mirroring machine:
8. Download the specified image set configuration to a local file on your mirroring machine:
    - Replace &lt;mirror\_rhoai&gt; with the target directory where you want to output the image set file.
    - The target directory path must start with file://.
    - The download might take several minutes.
							$ oc mirror -c imageset-config.yaml file://&lt;mirror\_rhoai&gt; --v2Tip
								If the tls: failed to verify certificate: x509: certificate signed by unknown authority error is returned and you want to ignore it, set skipTLS to true in your image set configuration file and run the command again.
9. Verify that the image set .tar files were created:
					$ ls &lt;mirror\_rhoai&gt;Example output
mirror\_000001.tar, mirror\_000002.tar

						If an archiveSize value was specified in the image set configuration file, the image set might be separated into multiple .tar files.
10. Optional: Verify that total size of the image set .tar files is around 75 GB:
					$ du -h --max-depth=1 ./&lt;mirror\_rhoai&gt;/
						If the total size of the image set is significantly less than 75 GB, run the oc mirror command again.
11. Upload the contents of the generated image set to your target mirror registry:
12. Upload the contents of the generated image set to your target mirror registry:
    - Replace &lt;mirror\_rhoai&gt; with the directory that contains your image set .tar files.
    - Replace &lt;registry.example.com:5000&gt; with your mirror registry.
							$ oc mirror -c imageset-config.yaml --from file://&lt;mirror\_rhoai&gt; docker://&lt;registry.example.com:5000&gt; --v2Tip
								If the tls: failed to verify certificate: x509: certificate signed by unknown authority error is returned and you want to ignore it, run the following command:
							$ oc mirror --dest-tls-verify false --from=./&lt;mirror\_rhoai&gt; docker://&lt;registry.example.com:5000&gt; --v2
13. Log in to your target OpenShift cluster using the OpenShift CLI as a user with the cluster-admin role.
14. Verify that the YAML files are present for the ImageDigestMirrorSet and CatalogSource resources:
15. Verify that the YAML files are present for the ImageDigestMirrorSet and CatalogSource resources:
    - Replace &lt;mirror\_rhoai&gt; with the directory that contains your image set .tar files.
							$ ls &lt;mirror\_rhoai&gt;/working-dir/cluster-resources/Example output
cs-redhat-operator-index.yaml
idms-oc-mirror.yaml
16. Install the generated resources into the cluster:
17. Install the generated resources into the cluster:
    - Replace &lt;oc\_mirror\_workspace\_path&gt; with the path to your oc mirror workspace.
							$ oc apply -f &lt;oc\_mirror\_workspace\_path&gt;/working-dir/cluster-resources

Verification

- Verify that the CatalogSource and pod were created successfully:
					$ oc get catalogsource,pod -n openshift-marketplace
						This should return at least one catalog and two pods.
- Check that the Red Hat OpenShift AI Operator exists in the OperatorHub:
- Check that the Red Hat OpenShift AI Operator exists in the OperatorHub:
    1. Log in to the OpenShift web console.
    2. Click Operators → OperatorHub.
							
								The OperatorHub page opens.
    3. Confirm that the Red Hat OpenShift AI Operator is shown.
- If you mirrored additional operators, such as OpenShift Pipelines, Red Hat OpenShift Serverless, or Red Hat OpenShift Service Mesh, check that those operators exist the OperatorHub.

Additional resources

Mirroring images for a disconnected installation by using the oc-mirror plugin v2

#### 3.4. Configuring custom namespaces

By default, OpenShift AI uses predefined namespaces, but you can define a custom namespace for the operator and DSCI.applicationNamespace as needed. Namespaces created by OpenShift AI typically include openshift or redhat in their name. Do not rename these system namespaces because they are required for OpenShift AI to function properly.

Prerequisites

- You have access to a OpenShift AI cluster with cluster administrator privileges.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI as shown in the following example:
					oc login &lt;openshift\_cluster\_url&gt; -u &lt;admin\_username&gt; -p &lt;password&gt;
2. Enter the following command to create the custom namespace:
					oc create namespace &lt;custom\_namespace&gt;
3. If you are creating a namespace for a DSCI.applicationNamespace, enter the following command to add the correct label:
					oc label namespace &lt;application\_namespace&gt; opendatahub.io/application-namespace=true

#### 3.5. Installing the Red Hat OpenShift AI Operator

This section shows how to install the Red Hat OpenShift AI Operator on your OpenShift cluster using the command-line interface (CLI) and the OpenShift web console.

Note

If you want to upgrade from a previous version of OpenShift AI rather than performing a new installation, see Upgrading OpenShift AI in a disconnected environment.

Note

If your OpenShift cluster uses a proxy to access the Internet, you can configure the proxy settings for the Red Hat OpenShift AI Operator. See Overriding proxy settings of an Operator for more information.

##### 3.5.1. Installing the Red Hat OpenShift AI Operator by using the CLI

The following procedure shows how to use the OpenShift command-line interface (CLI) to install the Red Hat OpenShift AI Operator on your OpenShift cluster. You must install the Operator before you can install OpenShift AI components on the cluster.

Prerequisites

- You have a running OpenShift cluster, version 4.14 or greater, configured with a default storage class that can be dynamically provisioned.
- You have cluster administrator privileges for your OpenShift cluster.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have mirrored the required container images to a private registry. See Mirroring images to a private registry for a disconnected installation.

Procedure

1. Open a new terminal window.
2. Follow these steps to log in to your OpenShift cluster as a cluster administrator:
3. Follow these steps to log in to your OpenShift cluster as a cluster administrator:
    1. In the upper-right corner of the OpenShift web console, click your user name and select Copy login command.
    2. After you have logged in, click Display token.
    3. Copy the Log in with this token command and paste it in the OpenShift command-line interface (CLI).
								$ oc login --token=&lt;token&gt; --server=&lt;openshift\_cluster\_url&gt;
4. Create a namespace for installation of the Operator by performing the following actions:
5. Create a namespace for installation of the Operator by performing the following actions:
    1. Create a namespace YAML file named rhods-operator-namespace.yaml.
								apiVersion: v1
kind: Namespace
metadata:
  name: redhat-ods-operator 11 
											Defines the required redhat-ods-operator namespace for installation of the Operator.
    2. Create the namespace in your OpenShift cluster.
								$ oc create -f rhods-operator-namespace.yaml
									You see output similar to the following:
								namespace/redhat-ods-operator created
6. Create an operator group for installation of the Operator by performing the following actions:
7. Create an operator group for installation of the Operator by performing the following actions:
    1. Create an OperatorGroup object custom resource (CR) file, for example, rhods-operator-group.yaml.
								apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: rhods-operator
  namespace: redhat-ods-operator 11 
											Defines the required redhat-ods-operator namespace.
    2. Create the OperatorGroup object in your OpenShift cluster.
								$ oc create -f rhods-operator-group.yaml
									You see output similar to the following:
								operatorgroup.operators.coreos.com/rhods-operator created
8. Create a subscription for installation of the Operator by performing the following actions:
9. Create a subscription for installation of the Operator by performing the following actions:
    1. Create a Subscription object CR file, for example, rhods-operator-subscription.yaml.
								apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: rhods-operator
  namespace: redhat-ods-operator 1
spec:
  name: rhods-operator
  channel: &lt;channel&gt; 2
  source: cs-redhat-operator-index
  sourceNamespace: openshift-marketplace
  startingCSV: rhods-operator.x.y.z 31 
											Defines the required redhat-ods-operator namespace.
										2 
											Sets the update channel. You must specify a value of fast, stable, stable-x.y eus-x.y, or alpha. For more information, see Understanding update channels.
										3 
											Optional: Sets the operator version. If you do not specify a value, the subscription defaults to the latest operator version. For more information, see the Red Hat OpenShift AI Self-Managed Life Cycle Knowledgebase article.
    2. Create the Subscription object in your OpenShift cluster to install the Operator.
								$ oc create -f rhods-operator-subscription.yaml
									You see output similar to the following:
								subscription.operators.coreos.com/rhods-operator created

Verification

- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat OpenShift AI Operator shows one of the following statuses:
- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat OpenShift AI Operator shows one of the following statuses:
    - Installing - installation is in progress; wait for this to change to Succeeded. This might take several minutes.
    - Succeeded - installation is successful.
- In the web console, click Home → Projects and confirm that the following project namespaces are visible and listed as Active :
- In the web console, click Home → Projects and confirm that the following project namespaces are visible and listed as Active:
    - redhat-ods-applications
    - redhat-ods-monitoring
    - redhat-ods-operator

Additional resources

- Installing and managing Red Hat OpenShift AI components
- Adding users to OpenShift AI user groups.
- Adding Operators to a cluster

##### 3.5.2. Installing the Red Hat OpenShift AI Operator by using the web console

The following procedure shows how to use the OpenShift web console to install the Red Hat OpenShift AI Operator on your cluster. You must install the Operator before you can install OpenShift AI components on the cluster.

Prerequisites

- You have a running OpenShift cluster, version 4.14 or greater, configured with a default storage class that can be dynamically provisioned.
- You have cluster administrator privileges for your OpenShift cluster.
- You have mirrored the required container images to a private registry. See Mirroring images to a private registry for a disconnected installation.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → OperatorHub.
3. On the OperatorHub page, locate the Red Hat OpenShift AI Operator by scrolling through the available Operators or by typing Red Hat OpenShift AI into the Filter by keyword box.
4. Click the Red Hat OpenShift AI tile. The Red Hat OpenShift AI information pane opens.
5. Select a Channel. For information about subscription update channels, see Understanding update channels.
6. Select a Version.
7. Click Install. The Install Operator page opens.
8. Review or change the selected channel and version as needed.
9. For Installation mode, note that the only available value is All namespaces on the cluster (default). This installation mode makes the Operator available to all namespaces in the cluster.
10. For Installed Namespace, select Operator recommended Namespace: redhat-ods-operator.
11. For Update approval , select one of the following update strategies:
12. For Update approval, select one of the following update strategies:
    - Automatic: Your environment attempts to install new updates when they are available based on the content of your mirror.
    - Manual: A cluster administrator must approve any new updates before installation begins.
								Important
										By default, the Red Hat OpenShift AI Operator follows a sequential update process. This means that if there are several versions between the current version and the target version, Operator Lifecycle Manager (OLM) upgrades the Operator to each of the intermediate versions before it upgrades it to the final, target version.
									
										If you configure automatic upgrades, OLM automatically upgrades the Operator to the latest available version. If you configure manual upgrades, a cluster administrator must manually approve each sequential update between the current version and the final, target version.
									
										For information about supported versions, see the Red Hat OpenShift AI Life Cycle Knowledgebase article.
13. Click Install.
						
							The Installing Operators pane appears. When the installation finishes, a checkmark appears next to the Operator name.

Verification

- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat OpenShift AI Operator shows one of the following statuses:
- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat OpenShift AI Operator shows one of the following statuses:
    - Installing - installation is in progress; wait for this to change to Succeeded. This might take several minutes.
    - Succeeded - installation is successful.
- In the web console, click Home → Projects and confirm that the following project namespaces are visible and listed as Active :
- In the web console, click Home → Projects and confirm that the following project namespaces are visible and listed as Active:
    - redhat-ods-applications
    - redhat-ods-monitoring
    - redhat-ods-operator

Additional resources

- Installing and managing Red Hat OpenShift AI components
- Adding users to OpenShift AI user groups
- Adding Operators to a cluster

#### 3.6. Installing and managing Red Hat OpenShift AI components

You can use the OpenShift command-line interface (CLI) or OpenShift web console to install and manage components of Red Hat OpenShift AI on your OpenShift cluster.

##### 3.6.1. Installing Red Hat OpenShift AI components by using the CLI

To install Red Hat OpenShift AI components by using the OpenShift command-line interface (CLI), you must create and configure a DataScienceCluster object.

Important

The following procedure describes how to create and configure a DataScienceCluster object to install Red Hat OpenShift AI components as part of a new installation.

- For information about changing the installation status of OpenShift AI components after installation, see Updating the installation status of Red Hat OpenShift AI components by using the web console.
- For information about upgrading OpenShift AI, see Upgrading OpenShift AI Self-Managed in a disconnected environment.

Prerequisites

- The Red Hat OpenShift AI Operator is installed on your OpenShift cluster. See Installing the Red Hat OpenShift AI Operator.
- You have cluster administrator privileges for your OpenShift cluster.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.

Procedure

1. Open a new terminal window.
2. Follow these steps to log in to your OpenShift cluster as a cluster administrator:
3. Follow these steps to log in to your OpenShift cluster as a cluster administrator:
    1. In the upper-right corner of the OpenShift web console, click your user name and select Copy login command.
    2. After you have logged in, click Display token.
    3. Copy the Log in with this token command and paste it in the OpenShift command-line interface (CLI).
								$ oc login --token=&lt;token&gt; --server=&lt;openshift\_cluster\_url&gt;
4. Create a DataScienceCluster object custom resource (CR) file, for example, rhods-operator-dsc.yaml.
						apiVersion: datasciencecluster.opendatahub.io/v1
kind: DataScienceCluster
metadata:
  name: default-dsc
spec:
  components:
    codeflare:
      managementState: Removed
    dashboard:
      managementState: Removed
    datasciencepipelines:
      managementState: Removed
    kserve:
      managementState: Removed 1 2
    kueue:
      managementState: Removed
    modelmeshserving:
      managementState: Removed
    ray:
      managementState: Removed
    trainingoperator:
      managementState: Removed
    trustyai:
      managementState: Removed
    workbenches:
      managementState: Removed1 
									To fully install the KServe component, which is used by the single-model serving platform to serve large models, you must install Operators for Red Hat OpenShift Service Mesh and Red Hat OpenShift Serverless and perform additional configuration. See Installing the single-model serving platform.
								2 
									If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors. See Disabling KServe dependencies.
5. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed . These values are defined as follows: Managed The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so. Removed The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. Important
6. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed. These values are defined as follows:
7. Important
    - To learn how to fully install the KServe component, which is used by the single-model serving platform to serve large models, see Installing the single-model serving platform.
    - If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors. See Disabling KServe dependencies.
    - To learn how to install the distributed workloads components, see Installing the distributed workloads components.
    - To learn how to run distributed workloads in a disconnected environment, see Running distributed data science workloads in a disconnected environment.
8. Create the DataScienceCluster object in your OpenShift cluster to install the specified OpenShift AI components.
						$ oc create -f rhods-operator-dsc.yaml
							You see output similar to the following:
						datasciencecluster.datasciencecluster.opendatahub.io/default created

Verification

- Confirm that there is a running pod for each component:
- Confirm that there is a running pod for each component:
    1. In the OpenShift web console, click Workloads → Pods.
    2. In the Project list at the top of the page, select redhat-ods-applications.
    3. In the applications namespace, confirm that there are running pods for each of the OpenShift AI components that you installed.
- Confirm the status of all installed components:
- Confirm the status of all installed components:
    1. In the OpenShift web console, click Operators → Installed Operators.
    2. Click the Red Hat OpenShift AI Operator.
    3. Click the Data Science Cluster tab and select the DataScienceCluster object called default-dsc.
    4. Select the YAML tab.
    5. In the installedComponents section, confirm that the components you installed have a status value of true.
								Note
										If a component shows with the component-name: {} format in the spec.components section of the CR, the component is not installed.

##### 3.6.2. Installing Red Hat OpenShift AI components by using the web console

To install Red Hat OpenShift AI components by using the OpenShift web console, you must create and configure a DataScienceCluster object.

Important

The following procedure describes how to create and configure a DataScienceCluster object to install Red Hat OpenShift AI components as part of a new installation.

- For information about changing the installation status of OpenShift AI components after installation, see Updating the installation status of Red Hat OpenShift AI components by using the web console.
- For information about upgrading OpenShift AI, see Upgrading OpenShift AI Self-Managed in a disconnected environment.

Prerequisites

- The Red Hat OpenShift AI Operator is installed on your OpenShift cluster. See Installing the Red Hat OpenShift AI Operator.
- You have cluster administrator privileges for your OpenShift cluster.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Click the Data Science Cluster tab.
4. Click Create DataScienceCluster.
5. For Configure via, select YAML view.
						
							An embedded YAML editor opens showing a default custom resource (CR) for the DataScienceCluster object, similar to the following example:
						apiVersion: datasciencecluster.opendatahub.io/v1
kind: DataScienceCluster
metadata:
  name: default-dsc
spec:
  components:
    codeflare:
      managementState: Removed
    dashboard:
      managementState: Removed
    datasciencepipelines:
      managementState: Removed
    kserve:
      managementState: Removed 1 2
    kueue:
      managementState: Removed
    modelmeshserving:
      managementState: Removed
    ray:
      managementState: Removed
    trainingoperator:
      managementState: Removed
    trustyai:
      managementState: Removed
    workbenches:
      managementState: Removed1 
									To fully install the KServe component, which is used by the single-model serving platform to serve large models, you must install Operators for Red Hat OpenShift Service Mesh and Red Hat OpenShift Serverless and perform additional configuration. See Installing the single-model serving platform.
								2 
									If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors. See Disabling KServe dependencies.
6. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed . These values are defined as follows: Managed The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so. Removed The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. Important
7. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed. These values are defined as follows:
8. Important
    - To learn how to fully install the KServe component, which is used by the single-model serving platform to serve large models, see Installing the single-model serving platform.
    - If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors. See Disabling KServe dependencies.
    - To learn how to install the distributed workloads components, see Installing the distributed workloads components.
    - To learn how to run distributed workloads in a disconnected environment, see Running distributed data science workloads in a disconnected environment.
9. Click Create.

Verification

- Confirm that there is a running pod for each component:
- Confirm that there is a running pod for each component:
    1. In the OpenShift web console, click Workloads → Pods.
    2. In the Project list at the top of the page, select redhat-ods-applications.
    3. In the applications namespace, confirm that there are running pods for each of the OpenShift AI components that you installed.
- Confirm the status of all installed components:
- Confirm the status of all installed components:
    1. In the OpenShift web console, click Operators → Installed Operators.
    2. Click the Red Hat OpenShift AI Operator.
    3. Click the Data Science Cluster tab and select the DataScienceCluster object called default-dsc.
    4. Select the YAML tab.
    5. In the installedComponents section, confirm that the components you installed have a status value of true.
								Note
										If a component shows with the component-name: {} format in the spec.components section of the CR, the component is not installed.

##### 3.6.3. Updating the installation status of Red Hat OpenShift AI components by using the web console

You can use the OpenShift web console to update the installation status of components of Red Hat OpenShift AI on your OpenShift cluster.

Important

If you upgraded OpenShift AI, the upgrade process automatically used the values of the previous version’s DataScienceCluster object. New components are not automatically added to the DataScienceCluster object.

After upgrading OpenShift AI:

- Inspect the default DataScienceCluster object to check and optionally update the managementState status of the existing components.
- Add any new components to the DataScienceCluster object.

Prerequisites

- The Red Hat OpenShift AI Operator is installed on your OpenShift cluster.
- You have cluster administrator privileges for your OpenShift cluster.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Click the Data Science Cluster tab.
4. On the DataScienceClusters page, click the default object.
5. Click the YAML tab.
						
							An embedded YAML editor opens showing the default custom resource (CR) for the DataScienceCluster object, similar to the following example:
						apiVersion: datasciencecluster.opendatahub.io/v1
kind: DataScienceCluster
metadata:
  name: default-dsc
spec:
  components:
    codeflare:
      managementState: Removed
    dashboard:
      managementState: Removed
    datasciencepipelines:
      managementState: Removed
    kserve:
      managementState: Removed
    kueue:
      managementState: Removed
    modelmeshserving:
      managementState: Removed
    ray:
      managementState: Removed
    trainingoperator:
      managementState: Removed
    trustyai:
      managementState: Removed
    workbenches:
      managementState: Removed
6. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed . These values are defined as follows: Managed The Operator actively manages the component, installs it, and tries to keep it active. The Operator will upgrade the component only if it is safe to do so. Removed The Operator actively manages the component but does not install it. If the component is already installed, the Operator will try to remove it. Important
7. In the spec.components section of the CR, for each OpenShift AI component shown, set the value of the managementState field to either Managed or Removed. These values are defined as follows:
8. Important
    - To learn how to install the KServe component, which is used by the single-model serving platform to serve large models, see Installing the single-model serving platform.
    - If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors. See Disabling KServe dependencies.
    - To learn how to install the distributed workloads feature, see Installing the distributed workloads components.
    - To learn how to run distributed workloads in a disconnected environment, see Running distributed data science workloads in a disconnected environment.
9. Click Save.
						
							For any components that you updated, OpenShift AI initiates a rollout that affects all pods to use the updated image.

Verification

- Confirm that there is a running pod for each component:
- Confirm that there is a running pod for each component:
    1. In the OpenShift web console, click Workloads → Pods.
    2. In the Project list at the top of the page, select redhat-ods-applications.
    3. In the applications namespace, confirm that there are running pods for each of the OpenShift AI components that you installed.
- Confirm the status of all installed components:
- Confirm the status of all installed components:
    1. In the OpenShift web console, click Operators → Installed Operators.
    2. Click the Red Hat OpenShift AI Operator.
    3. Click the Data Science Cluster tab and select the DataScienceCluster object called default-dsc.
    4. Select the YAML tab.
    5. In the installedComponents section, confirm that the components you installed have a status value of true.
								Note
										If a component shows with the component-name: {} format in the spec.components section of the CR, the component is not installed.

### Chapter 4. Installing the distributed workloads components

To use the distributed workloads feature in OpenShift AI, you must install several components.

Prerequisites

- You have logged in to OpenShift with the cluster-admin role and you can access the data science cluster.
- You have installed Red Hat OpenShift AI.
- You have sufficient resources. In addition to the minimum OpenShift AI resources described in Installing and deploying OpenShift AI (for disconnected environments, see Deploying OpenShift AI in a disconnected environment), you need 1.6 vCPU and 2 GiB memory to deploy the distributed workloads infrastructure.
- You have removed any previously installed instances of the CodeFlare Operator, as described in the Knowledgebase solution How to migrate from a separately installed CodeFlare Operator in your data science cluster.
- If you want to use graphics processing units (GPUs), you have enabled GPU support in OpenShift AI. If you use NVIDIA GPUs, see Enabling NVIDIA GPUs. If you use AMD GPUs, see AMD GPU integration.
				Note
						Red Hat supports the use of accelerators within the same cluster only. Red Hat does not support remote direct memory access (RDMA) between accelerators, or the use of accelerators across a network, for example, by using technology such as NVIDIA GPUDirect or NVLink.
- If you want to use self-signed certificates, you have added them to a central Certificate Authority (CA) bundle as described in Working with certificates (for disconnected environments, see Working with certificates ). No additional configuration is necessary to use those certificates with distributed workloads. The centrally configured self-signed certificates are automatically available in the workload pods at the following mount points:
- If you want to use self-signed certificates, you have added them to a central Certificate Authority (CA) bundle as described in Working with certificates (for disconnected environments, see Working with certificates). No additional configuration is necessary to use those certificates with distributed workloads. The centrally configured self-signed certificates are automatically available in the workload pods at the following mount points:
    - Cluster-wide CA bundle:
						/etc/pki/tls/certs/odh-trusted-ca-bundle.crt
/etc/ssl/certs/odh-trusted-ca-bundle.crt
    - Custom CA bundle:
						/etc/pki/tls/certs/odh-ca-bundle.crt
/etc/ssl/certs/odh-ca-bundle.crt

Procedure

1. In the OpenShift console, click Operators → Installed Operators.
2. Search for the Red Hat OpenShift AI Operator, and then click the Operator name to open the Operator details page.
3. Click the Data Science Cluster tab.
4. Click the default instance name (for example, default-dsc) to open the instance details page.
5. Click the YAML tab to show the instance specifications.
6. Enable the required distributed workloads components. In the spec.components section, set the managementState field correctly for the required components: Table 4.1. Components required for distributed workloads Empty Empty Empty Empty Component Pipelines only Notebooks only Pipelines and notebooks codeflare Managed Managed Managed dashboard Managed Managed Managed datasciencepipelines Managed Removed Managed kueue Managed Managed Managed ray Managed Managed Managed trainingoperator Managed Managed Managed workbenches Removed Managed Managed
7. Enable the required distributed workloads components. In the spec.components section, set the managementState field correctly for the required components:
    - If you want to use the CodeFlare framework to tune models, enable the codeflare, kueue, and ray components.
    - If you want to use the Kubeflow Training Operator to tune models, enable the kueue and trainingoperator components.
    - The list of required components depends on whether the distributed workload is run from a pipeline or notebook or both, as shown in the following table.
8. | Component            | Pipelines only   | Notebooks only   | Pipelines and notebooks   |
|----------------------|------------------|------------------|---------------------------|
| codeflare            | Managed          | Managed          | Managed                   |
| dashboard            | Managed          | Managed          | Managed                   |
| datasciencepipelines | Managed          | Removed          | Managed                   |
| kueue                | Managed          | Managed          | Managed                   |
| ray                  | Managed          | Managed          | Managed                   |
| trainingoperator     | Managed          | Managed          | Managed                   |
| workbenches          | Removed          | Managed          | Managed                   |
9. Click Save. After a short time, the components with a Managed state are ready.

Verification

Check the status of the codeflare-operator-manager, kuberay-operator, and kueue-controller-manager pods, as follows:

1. In the OpenShift console, from the Project list, select redhat-ods-applications.
2. Click Workloads → Deployments.
3. Search for the codeflare-operator-manager , kuberay-operator , and kueue-controller-manager deployments. In each case, check the status as follows:
4. Search for the codeflare-operator-manager, kuberay-operator, and kueue-controller-manager deployments. In each case, check the status as follows:
    1. Click the deployment name to open the deployment details page.
    2. Click the Pods tab.
    3. Check the pod status.
						
							When the status of the codeflare-operator-manager-&lt;pod-id&gt;, kuberay-operator-&lt;pod-id&gt;, and kueue-controller-manager-&lt;pod-id&gt; pods is Running, the pods are ready to use.
    4. To see more information about each pod, click the pod name to open the pod details page, and then click the Logs tab.

Next Step

Configure the distributed workloads feature as described in Managing distributed workloads.

### Chapter 5. Installing the single-model serving platform

#### 5.1. About the single-model serving platform

For deploying large models such as large language models (LLMs), OpenShift AI includes a single-model serving platform that is based on the KServe component. To install the single-model serving platform, the following components are required:

- KServe: A Kubernetes custom resource definition (CRD) that orchestrates model serving for all types of models. KServe includes model-serving runtimes that implement the loading of given types of model servers. KServe also handles the lifecycle of the deployment object, storage access, and networking setup.
- Red Hat OpenShift Serverless: A cloud-native development model that allows for serverless deployments of models. OpenShift Serverless is based on the open source Knative project.
- Red Hat OpenShift Service Mesh: A service mesh networking layer that manages traffic flows and enforces access policies. OpenShift Service Mesh is based on the open source Istio project.
					Note
							Currently, only OpenShift Service Mesh v2 is supported. For more information, see Supported Configurations.

You can install the single-model serving platform manually or in an automated fashion:

#### 5.2. Configuring automated installation of KServe

If you have not already created a ServiceMeshControlPlane or KNativeServing resource on your OpenShift cluster, you can configure the Red Hat OpenShift AI Operator to install KServe and configure its dependencies.

Important

If you have created a ServiceMeshControlPlane or KNativeServing resource on your cluster, the Red Hat OpenShift AI Operator cannot install KServe and configure its dependencies and the installation does not proceed. In this situation, you must follow the manual installation instructions to install KServe.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- Your cluster has a node with 4 CPUs and 16 GB memory.
- You have downloaded and installed the OpenShift command-line interface (CLI). For more information, see Installing the OpenShift CLI.
- You have installed the Red Hat OpenShift Service Mesh Operator and dependent Operators.
					Note
							To enable automated installation of KServe, install only the required Operators for Red Hat OpenShift Service Mesh. Do not perform any additional configuration or create a ServiceMeshControlPlane resource.
- You have installed the Red Hat OpenShift Serverless Operator.
					Note
							To enable automated installation of KServe, install only the Red Hat OpenShift Serverless Operator. Do not perform any additional configuration or create a KNativeServing resource.
- You have installed the Red Hat OpenShift AI Operator and created a DataScienceCluster object.
- To add Authorino as an authorization provider so that you can enable token authentication for deployed models, you have installed the Red Hat - Authorino Operator. See Installing the Authorino Operator.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Install OpenShift Service Mesh as follows:
4. Install OpenShift Service Mesh as follows:
    1. Click the DSC Initialization tab.
    2. Click the default-dsci object.
    3. Click the YAML tab.
    4. In the spec section, validate that the value of the managementState field for the serviceMesh component is set to Managed, as shown:
							spec:
 applicationsNamespace: redhat-ods-applications
 monitoring:
   managementState: Managed
   namespace: redhat-ods-monitoring
 serviceMesh:
   controlPlane:
     metricsCollection: Istio
     name: data-science-smcp
     namespace: istio-system
   managementState: ManagedNote
									Do not change the istio-system namespace that is specified for the serviceMesh component by default. Other namespace values are not supported.
    5. Click Save.
							
								Based on the configuration you added to the DSCInitialization object, the Red Hat OpenShift AI Operator installs OpenShift Service Mesh.
5. Install both KServe and OpenShift Serverless as follows:
6. Install both KServe and OpenShift Serverless as follows:
    1. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
    2. Click the Data Science Cluster tab.
    3. Click the default-dsc DSC object.
    4. Click the YAML tab.
    5. In the spec.components section, configure the kserve component as shown.
							spec:
 components:
   kserve:
     managementState: Managed
     serving:
       ingressGateway:
         certificate:
           secretName: knative-serving-cert
           type: OpenshiftDefaultIngress
       managementState: Managed
       name: knative-serving
    6. Click Save . The preceding configuration creates an ingress gateway for OpenShift Serverless to receive traffic from OpenShift Service Mesh. In this configuration, observe the following details:
    7. Click Save.
    8. The preceding configuration creates an ingress gateway for OpenShift Serverless to receive traffic from OpenShift Service Mesh. In this configuration, observe the following details:
        - The configuration shown uses the default ingress certificate configured for OpenShift to secure incoming traffic to your OpenShift cluster and stores the certificate in the knative-serving-cert secret that is specified in the secretName field.
        - The secretName field can only be set at the time of installation. The default value of the secretName field is knative-serving-cert. Subsequent changes to the certificate secret must be made manually.
        - If you did not use the default secretName value during installation, create a new secret named knative-serving-cert in the istio-system namespace, and then restart the istiod-datascience-smcp-&lt;suffix&gt; pod.
        - You can specify the following certificate types by updating the value of the type field:
        - You can specify the following certificate types by updating the value of the type field:
            - Provided
            - SelfSigned
            - OpenshiftDefaultIngress
        - To use a self-signed certificate or to provide your own, update the value of the secretName field to specify your secret name and change the value of the type field to SelfSigned or Provided.
									Note
											If you provide your own certificate, the certificate must specify the domain name used by the ingress controller of your OpenShift cluster. You can check this value by running the following command:
										
$ oc get ingresses.config.openshift.io cluster -o jsonpath='{.spec.domain}'
        - You must set the value of the managementState field to Managed for both the kserve and serving components. Setting kserve.managementState to Managed triggers automated installation of KServe. Setting serving.managementState to Managed triggers automated installation of OpenShift Serverless. However, installation of OpenShift Serverless will not be triggered if kserve.managementState is not also set to Managed.

Verification

- Verify installation of OpenShift Service Mesh as follows:
- Verify installation of OpenShift Service Mesh as follows:
    - In the web console, click Workloads → Pods.
    - From the project list, select istio-system. This is the project in which OpenShift Service Mesh is installed.
    - Confirm that there are running pods for the service mesh control plane, ingress gateway, and egress gateway. These pods have the naming patterns shown in the following example:
							NAME                                      		  READY     STATUS    RESTARTS   AGE
istio-egressgateway-7c46668687-fzsqj      	 	  1/1       Running   0          22h
istio-ingressgateway-77f94d8f85-fhsp9      		  1/1       Running   0          22h
istiod-data-science-smcp-cc8cfd9b8-2rkg4  		  1/1       Running   0          22h
- Verify installation of OpenShift Serverless as follows:
- Verify installation of OpenShift Serverless as follows:
    - In the web console, click Workloads → Pods.
    - From the project list, select knative-serving. This is the project in which OpenShift Serverless is installed.
    - Confirm that there are numerous running pods in the knative-serving project, including activator, autoscaler, controller, and domain mapping pods, as well as pods for the Knative Istio controller (which controls the integration of OpenShift Serverless and OpenShift Service Mesh). An example is shown.
							NAME                                     	READY     STATUS    RESTARTS  AGE
activator-7586f6f744-nvdlb               	2/2       Running   0         22h
activator-7586f6f744-sd77w               	2/2       Running   0         22h
autoscaler-764fdf5d45-p2v98             	2/2       Running   0         22h
autoscaler-764fdf5d45-x7dc6              	2/2       Running   0         22h
autoscaler-hpa-7c7c4cd96d-2lkzg          	1/1       Running   0         22h
autoscaler-hpa-7c7c4cd96d-gks9j         	1/1       Running   0         22h
controller-5fdfc9567c-6cj9d              	1/1       Running   0         22h
controller-5fdfc9567c-bf5x7              	1/1       Running   0         22h
domain-mapping-56ccd85968-2hjvp          	1/1       Running   0         22h
domain-mapping-56ccd85968-lg6mw          	1/1       Running   0         22h
domainmapping-webhook-769b88695c-gp2hk   	1/1       Running   0         22h
domainmapping-webhook-769b88695c-npn8g   	1/1       Running   0         22h
net-istio-controller-7dfc6f668c-jb4xk    	1/1       Running   0         22h
net-istio-controller-7dfc6f668c-jxs5p    	1/1       Running   0         22h
net-istio-webhook-66d8f75d6f-bgd5r       	1/1       Running   0         22h
net-istio-webhook-66d8f75d6f-hld75      	1/1       Running   0         22h
webhook-7d49878bc4-8xjbr                 	1/1       Running   0         22h
webhook-7d49878bc4-s4xx4                 	1/1       Running   0         22h
- Verify installation of KServe as follows:
- Verify installation of KServe as follows:
    - In the web console, click Workloads → Pods.
    - From the project list, select redhat-ods-applications.This is the project in which OpenShift AI components are installed, including KServe.
    - Confirm that the project includes a running pod for the KServe controller manager, similar to the following example:
							NAME                                          READY   STATUS    RESTARTS   AGE
kserve-controller-manager-7fbb7bccd4-t4c5g    1/1     Running   0          22h
odh-model-controller-6c4759cc9b-cftmk         1/1     Running   0          129m
odh-model-controller-6c4759cc9b-ngj8b         1/1     Running   0          129m
odh-model-controller-6c4759cc9b-vnhq5         1/1     Running   0          129m

#### 5.3. Manually installing KServe

If you have already installed the Red Hat OpenShift Service Mesh Operator and created a ServiceMeshControlPlane resource or if you have installed the Red Hat OpenShift Serverless Operator and created a KNativeServing resource, the Red Hat OpenShift AI Operator cannot install KServe and configure its dependencies. In this situation, you must install KServe manually.

Important

The procedures in this section show how to perform a new installation of KServe and its dependencies and are intended as a complete installation and configuration reference. If you have already installed and configured OpenShift Service Mesh or OpenShift Serverless, you might not need to follow all steps. If you are unsure about what updates to apply to your existing configuration to use KServe, contact Red Hat Support.

##### 5.3.1. Installing KServe dependencies

Before you install KServe, you must install and configure some dependencies. Specifically, you must create Red Hat OpenShift Service Mesh and Knative Serving instances and then configure secure gateways for Knative Serving.

Note

Currently, only OpenShift Service Mesh v2 is supported. For more information, see Supported Configurations.

##### 5.3.2. Creating an OpenShift Service Mesh instance

The following procedure shows how to create a Red Hat OpenShift Service Mesh instance.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- Your cluster has a node with 4 CPUs and 16 GB memory.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have installed the Red Hat OpenShift Service Mesh Operator and dependent Operators.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI as shown in the following example:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;admin\_username&gt; -p &lt;password&gt;
2. Create the required namespace for Red Hat OpenShift Service Mesh.
						$ oc create ns istio-system
							You see the following output:
						namespace/istio-system created
3. Define a ServiceMeshControlPlane object in a YAML file named smcp.yaml with the following contents:
						apiVersion: maistra.io/v2
kind: ServiceMeshControlPlane
metadata:
  name: minimal
  namespace: istio-system
spec:
  tracing:
    type: None
  addons:
    grafana:
      enabled: false
    kiali:
      name: kiali
      enabled: false
    prometheus:
      enabled: false
    jaeger:
      name: jaeger
  security:
    dataPlane:
      mtls: true
    identity:
      type: ThirdParty
  techPreview:
    meshConfig:
      defaultConfig:
        terminationDrainDuration: 35s
  gateways:
    ingress:
      service:
        metadata:
          labels:
            knative: ingressgateway
  proxy:
    networking:
      trafficControl:
        inbound:
          excludedPorts:
            - 8444
            - 8022
							For more information about the values in the YAML file, see the Service Mesh control plane configuration reference.
4. Create the service mesh control plane.
						$ oc apply -f smcp.yaml

Verification

- Verify creation of the service mesh instance as follows:
- Verify creation of the service mesh instance as follows:
    - In the OpenShift CLI, enter the following command:
								$ oc get pods -n istio-system
									The preceding command lists all running pods in the istio-system project. This is the project in which OpenShift Service Mesh is installed.
    - Confirm that there are running pods for the service mesh control plane, ingress gateway, and egress gateway. These pods have the following naming patterns:
								NAME                                          READY   STATUS   	  RESTARTS    AGE
istio-egressgateway-7c46668687-fzsqj          1/1     Running     0           22h
istio-ingressgateway-77f94d8f85-fhsp9         1/1     Running     0           22h
istiod-data-science-smcp-cc8cfd9b8-2rkg4      1/1     Running     0           22h

##### 5.3.3. Creating a Knative Serving instance

The following procedure shows how to install Knative Serving and then create an instance.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- Your cluster has a node with 4 CPUs and 16 GB memory.
- You have downloaded and installed the OpenShift command-line interface (CLI). Installing the OpenShift CLI.
- You have created a Red Hat OpenShift Service Mesh instance.
- You have installed the Red Hat OpenShift Serverless Operator.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI as shown in the following example:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;admin\_username&gt; -p &lt;password&gt;
2. Check whether the required project (that is, namespace) for Knative Serving already exists.
						$ oc get ns knative-serving
							If the project exists, you see output similar to the following example:
						NAME              STATUS   AGE
knative-serving   Active   4d20h
3. If the knative-serving project doesn’t already exist, create it.
						$ oc create ns knative-serving
							You see the following output:
						namespace/knative-serving created
4. Define a ServiceMeshMember object in a YAML file called default-smm.yaml with the following contents:
						apiVersion: maistra.io/v1
kind: ServiceMeshMember
metadata:
  name: default
  namespace: knative-serving
spec:
  controlPlaneRef:
    namespace: istio-system
    name: minimal
5. Create the ServiceMeshMember object in the istio-system namespace.
						$ oc apply -f default-smm.yaml
							You see the following output:
						servicemeshmember.maistra.io/default created
6. Define a KnativeServing object in a YAML file called knativeserving-istio.yaml with the following contents:
						apiVersion: operator.knative.dev/v1beta1
kind: KnativeServing
metadata:
  name: knative-serving
  namespace: knative-serving
  annotations:
    serverless.openshift.io/default-enable-http2: "true"
spec:
  workloads:
    - name: net-istio-controller
      env:
        - container: controller
          envVars:
            - name: ENABLE\_SECRET\_INFORMER\_FILTERING\_BY\_CERT\_UID
              value: 'true'
    - annotations:
        sidecar.istio.io/inject: "true" 1
        sidecar.istio.io/rewriteAppHTTPProbers: "true" 2
      name: activator
    - annotations:
        sidecar.istio.io/inject: "true"
        sidecar.istio.io/rewriteAppHTTPProbers: "true"
      name: autoscaler
  ingress:
    istio:
      enabled: true
  config:
    features:
      kubernetes.podspec-affinity: enabled
      kubernetes.podspec-nodeselector: enabled
      kubernetes.podspec-tolerations: enabled
							The preceding file defines a custom resource (CR) for a KnativeServing object. The CR also adds the following actions to each of the activator and autoscaler pods:
						1 
									Injects an Istio sidecar to the pod. This makes the pod part of the service mesh.
								2 
									Enables the Istio sidecar to rewrite the HTTP liveness and readiness probes for the pod.
								Note
								If you configure a custom domain for a Knative service, you can use a TLS certificate to secure the mapped service. To do this, you must create a TLS secret, and then update the DomainMapping CR to use the TLS secret that you have created. For more information, see Securing a mapped service using a TLS certificate in the Red Hat OpenShift Serverless documentation.
7. Create the KnativeServing object in the specified knative-serving namespace.
						$ oc apply -f knativeserving-istio.yaml
							You see the following output:
						knativeserving.operator.knative.dev/knative-serving created

Verification

- Review the default ServiceMeshMemberRoll object in the istio-system namespace.
						$ oc describe smmr default -n istio-system
							In the description of the ServiceMeshMemberRoll object, locate the Status.Members field and confirm that it includes the knative-serving namespace.
- Verify creation of the Knative Serving instance as follows:
- Verify creation of the Knative Serving instance as follows:
    - In the OpenShift CLI, enter the following command:
								$ oc get pods -n knative-serving
									The preceding command lists all running pods in the knative-serving project. This is the project in which you created the Knative Serving instance.
    - Confirm that there are numerous running pods in the knative-serving project, including activator, autoscaler, controller, and domain mapping pods, as well as pods for the Knative Istio controller, which controls the integration of OpenShift Serverless and OpenShift Service Mesh. An example is shown.
								NAME                                     	READY       STATUS    	RESTARTS   	AGE
activator-7586f6f744-nvdlb               	2/2         Running   	0          	22h
activator-7586f6f744-sd77w               	2/2         Running   	0          	22h
autoscaler-764fdf5d45-p2v98             	2/2         Running   	0          	22h
autoscaler-764fdf5d45-x7dc6              	2/2         Running   	0          	22h
autoscaler-hpa-7c7c4cd96d-2lkzg          	1/1         Running   	0          	22h
autoscaler-hpa-7c7c4cd96d-gks9j         	1/1         Running   	0          	22h
controller-5fdfc9567c-6cj9d              	1/1         Running   	0          	22h
controller-5fdfc9567c-bf5x7              	1/1         Running   	0          	22h
domain-mapping-56ccd85968-2hjvp          	1/1         Running   	0          	22h
domain-mapping-56ccd85968-lg6mw          	1/1         Running   	0          	22h
domainmapping-webhook-769b88695c-gp2hk   	1/1         Running     0          	22h
domainmapping-webhook-769b88695c-npn8g   	1/1         Running   	0          	22h
net-istio-controller-7dfc6f668c-jb4xk    	1/1         Running   	0          	22h
net-istio-controller-7dfc6f668c-jxs5p    	1/1         Running   	0          	22h
net-istio-webhook-66d8f75d6f-bgd5r       	1/1         Running   	0          	22h
net-istio-webhook-66d8f75d6f-hld75      	1/1         Running   	0          	22h
webhook-7d49878bc4-8xjbr                 	1/1         Running   	0          	22h
webhook-7d49878bc4-s4xx4                 	1/1         Running   	0          	22h

##### 5.3.4. Creating secure gateways for Knative Serving

To secure traffic between your Knative Serving instance and the service mesh, you must create secure gateways for your Knative Serving instance.

The following procedure shows how to use OpenSSL version 3 or later to generate a wildcard certificate and key and then use them to create local and ingress gateways for Knative Serving.

Important

If you have your own wildcard certificate and key to specify when configuring the gateways, you can skip to step 11 of this procedure.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have created a Red Hat OpenShift Service Mesh instance.
- You have created a Knative Serving instance.
- If you intend to generate a wildcard certificate and key, you have downloaded and installed OpenSSL version 3 or later.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI as shown in the following example:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;admin\_username&gt; -p &lt;password&gt;Important
								If you have your own wildcard certificate and key to specify when configuring the gateways, skip to step 11 of this procedure.
2. Set environment variables to define base directories for generation of a wildcard certificate and key for the gateways.
						$ export BASE\_DIR=/tmp/kserve
$ export BASE\_CERT\_DIR=${BASE\_DIR}/certs
3. Set an environment variable to define the common name used by the ingress controller of your OpenShift cluster.
						$ export COMMON\_NAME=$(oc get ingresses.config.openshift.io cluster -o jsonpath='{.spec.domain}' | awk -F'.' '{print $(NF-1)"."$NF}')
4. Set an environment variable to define the domain name used by the ingress controller of your OpenShift cluster.
						$ export DOMAIN\_NAME=$(oc get ingresses.config.openshift.io cluster -o jsonpath='{.spec.domain}')
5. Create the required base directories for the certificate generation, based on the environment variables that you previously set.
						$ mkdir ${BASE\_DIR}
$ mkdir ${BASE\_CERT\_DIR}
6. Create the OpenSSL configuration for generation of a wildcard certificate.
						$ cat &lt;&lt;EOF&gt; ${BASE\_DIR}/openssl-san.config
[ req ]
distinguished\_name = req
[ san ]
subjectAltName = DNS:*.${DOMAIN\_NAME}
EOF
7. Generate a root certificate.
						$ openssl req -x509 -sha256 -nodes -days 3650 -newkey rsa:2048 \
-subj "/O=Example Inc./CN=${COMMON\_NAME}" \
-keyout ${BASE\_CERT\_DIR}/root.key \
-out ${BASE\_CERT\_DIR}/root.crt
8. Generate a wildcard certificate signed by the root certificate.
						$ openssl req -x509 -newkey rsa:2048 \
-sha256 -days 3560 -nodes \
-subj "/CN=${COMMON\_NAME}/O=Example Inc." \
-extensions san -config ${BASE\_DIR}/openssl-san.config \
-CA ${BASE\_CERT\_DIR}/root.crt \
-CAkey ${BASE\_CERT\_DIR}/root.key \
-keyout ${BASE\_CERT\_DIR}/wildcard.key  \
-out ${BASE\_CERT\_DIR}/wildcard.crt

$ openssl x509 -in ${BASE\_CERT\_DIR}/wildcard.crt -text
9. Verify the wildcard certificate.
						$ openssl verify -CAfile ${BASE\_CERT\_DIR}/root.crt ${BASE\_CERT\_DIR}/wildcard.crt
10. Export the wildcard key and certificate that were created by the script to new environment variables.
						$ export TARGET\_CUSTOM\_CERT=${BASE\_CERT\_DIR}/wildcard.crt
$ export TARGET\_CUSTOM\_KEY=${BASE\_CERT\_DIR}/wildcard.key
11. Optional: To export your own wildcard key and certificate to new environment variables, enter the following commands:
						$ export TARGET\_CUSTOM\_CERT=&lt;path\_to\_certificate&gt;
$ export TARGET\_CUSTOM\_KEY=&lt;path\_to\_key&gt;Note
								In the certificate that you provide, you must specify the domain name used by the ingress controller of your OpenShift cluster. You can check this value by running the following command:
							
$ oc get ingresses.config.openshift.io cluster -o jsonpath='{.spec.domain}'
12. Create a TLS secret in the istio-system namespace using the environment variables that you set for the wildcard certificate and key.
						$ oc create secret tls wildcard-certs --cert=${TARGET\_CUSTOM\_CERT} --key=${TARGET\_CUSTOM\_KEY} -n istio-system
13. Create a gateways.yaml YAML file with the following contents:
						apiVersion: v1
kind: Service 1
metadata:
  labels:
    experimental.istio.io/disable-gateway-port-translation: "true"
  name: knative-local-gateway
  namespace: istio-system
spec:
  ports:
    - name: http2
      port: 80
      protocol: TCP
      targetPort: 8081
  selector:
    knative: ingressgateway
  type: ClusterIP
---
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
  name: knative-ingress-gateway 2
  namespace: knative-serving
spec:
  selector:
    knative: ingressgateway
  servers:
    - hosts:
        - '*'
      port:
        name: https
        number: 443
        protocol: HTTPS
      tls:
        credentialName: wildcard-certs
        mode: SIMPLE
---
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
 name: knative-local-gateway 3
 namespace: knative-serving
spec:
 selector:
   knative: ingressgateway
 servers:
   - port:
       number: 8081
       name: https
       protocol: HTTPS
     tls:
       mode: ISTIO\_MUTUAL
     hosts:
       - "*"1 
									Defines a service in the istio-system namespace for the Knative local gateway.
								2 
									Defines an ingress gateway in the knative-serving namespace. The gateway uses the TLS secret you created earlier in this procedure. The ingress gateway handles external traffic to Knative.
								3 
									Defines a local gateway for Knative in the knative-serving namespace.
14. Apply the gateways.yaml file to create the defined resources.
						$ oc apply -f gateways.yaml
							You see the following output:
						service/knative-local-gateway created
gateway.networking.istio.io/knative-ingress-gateway created
gateway.networking.istio.io/knative-local-gateway created

Verification

- Review the gateways that you created.
						$ oc get gateway --all-namespaces
							Confirm that you see the local and ingress gateways that you created in the knative-serving namespace, as shown in the following example:
						NAMESPACE         	NAME                      	AGE
knative-serving   	knative-ingress-gateway   	69s
knative-serving     knative-local-gateway     	2m

##### 5.3.5. Installing KServe

To complete manual installation of KServe, you must install the Red Hat OpenShift AI Operator. Then, you can configure the Operator to install KServe.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- Your cluster has a node with 4 CPUs and 16 GB memory.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have created a Red Hat OpenShift Service Mesh instance.
- You have created a Knative Serving instance.
- You have created secure gateways for Knative Serving.
- You have installed the Red Hat OpenShift AI Operator and created a DataScienceCluster object.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. For installation of KServe, configure the OpenShift Service Mesh component as follows:
4. For installation of KServe, configure the OpenShift Service Mesh component as follows:
    1. Click the DSC Initialization tab.
    2. Click the default-dsci object.
    3. Click the YAML tab.
    4. In the spec section, add and configure the serviceMesh component as shown:
								spec:
 serviceMesh:
   managementState: Unmanaged
    5. Click Save.
5. For installation of KServe, configure the KServe and OpenShift Serverless components as follows:
6. For installation of KServe, configure the KServe and OpenShift Serverless components as follows:
    1. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
    2. Click the Data Science Cluster tab.
    3. Click the default-dsc DSC object.
    4. Click the YAML tab.
    5. In the spec.components section, configure the kserve component as shown:
								spec:
 components:
   kserve:
     managementState: Managed
    6. Within the kserve component, add the serving component, and configure it as shown:
								spec:
 components:
   kserve:
     managementState: Managed
     serving:
       managementState: Unmanaged
    7. Click Save.

##### 5.3.6. Configuring persistent volume claims (PVC) on KServe

Enable persistent volume claims (PVC) on your inference service so you can provison persistent storage. For more information about PVC, see Understanding persistent storage.

To enable PVC, from the OpenShift AI dashboard, select the Project drop-down and click knative-serving. Then, follow the steps in Enabling PVC support.

Verification

Verify that the inference service allows PVC as follows:

- In the OpenShift web console, change into the Administrator perspective.
- Click Home → Search.
- In Resources, search for InferenceService.
- Click the name of the inference service.
- Click the YAML tab.
- Confirm that volumeMounts appears, similar to the following output:
						apiVersion: "serving.kserve.io/v1beta1"
kind: "InferenceService"
metadata:
  name: "sklearn-iris"
spec:
  predictor:
    model:
      runtime: kserve-mlserver
      modelFormat:
        name: sklearn
      storageUri: "gs://kfserving-examples/models/sklearn/1.0/model"
      volumeMounts:
        - name: my-dynamic-volume
          mountPath: /tmp/data
    volumes:
      - name: my-dynamic-volume
        persistentVolumeClaim:
          claimName: my-dynamic-pvc

##### 5.3.7. Disabling KServe dependencies

If you have not enabled the KServe component (that is, you set the value of the managementState field to Removed), you must also disable the dependent Service Mesh component to avoid errors.

Prerequisites

- You have used the OpenShift command-line interface (CLI) or web console to disable the KServe component.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Disable the OpenShift Service Mesh component as follows:
4. Disable the OpenShift Service Mesh component as follows:
    1. Click the DSC Initialization tab.
    2. Click the default-dsci object.
    3. Click the YAML tab.
    4. In the spec section, add the serviceMesh component (if it is not already present) and configure the managementState field as shown:
								spec:
 serviceMesh:
   managementState: Removed
    5. Click Save.

Verification

1. In the web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
						
							The Operator details page opens.
2. In the Conditions section, confirm that there is no ReconcileComplete condition with a status value of Unknown.

#### 5.4. Adding an authorization provider for the single-model serving platform

You can add Authorino as an authorization provider for the single-model serving platform. Adding an authorization provider allows you to enable token authentication for models that you deploy on the platform, which ensures that only authorized parties can make inference requests to the models.

The method that you use to add Authorino as an authorization provider depends on how you install the single-model serving platform. The installation options for the platform are described as follows:

If you have not already created a ServiceMeshControlPlane or KNativeServing resource on your OpenShift cluster, you can configure the Red Hat OpenShift AI Operator to install KServe and its dependencies. You can include Authorino as part of the automated installation process.

For more information about automated installation, including Authorino, see Configuring automated installation of KServe.

If you have already created a ServiceMeshControlPlane or KNativeServing resource on your OpenShift cluster, you cannot configure the Red Hat OpenShift AI Operator to install KServe and its dependencies. In this situation, you must install KServe manually. You must also manually configure Authorino.

For more information about manual installation, including Authorino, see Manually installing KServe.

##### 5.4.1. Manually adding an authorization provider

You can add Authorino as an authorization provider for the single-model serving platform. Adding an authorization provider allows you to enable token authentication for models that you deploy on the platform, which ensures that only authorized parties can make inference requests to the models.

To manually add Authorino as an authorization provider, you must install the Red Hat - Authorino Operator, create an Authorino instance, and then configure the OpenShift Service Mesh and KServe components to use the instance.

Important

To manually add an authorization provider, you must make configuration updates to your OpenShift Service Mesh instance. To ensure that your OpenShift Service Mesh instance remains in a supported state, make only the updates shown in this section.

Prerequisites

- You have reviewed the options for adding Authorino as an authorization provider and identified manual installation as the appropriate option. See Adding an authorization provider.
- You have manually installed KServe and its dependencies, including OpenShift Service Mesh. See Manually installing KServe.
- When you manually installed KServe, you set the value of the managementState field for the serviceMesh component to Unmanaged. This setting is required for manually adding Authorino. See Installing KServe.

##### 5.4.2. Installing the Red Hat Authorino Operator

Before you can add Autorino as an authorization provider, you must install the Red Hat - Authorino Operator on your OpenShift cluster.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.

Procedure

1. Log in to the OpenShift web console as a cluster administrator.
2. In the web console, click Operators → OperatorHub.
3. On the OperatorHub page, in the Filter by keyword field, type Red Hat - Authorino.
4. Click the Red Hat - Authorino Operator.
5. On the Red Hat - Authorino Operator page, review the Operator information and then click Install.
6. On the Install Operator page, keep the default values for Update channel, Version, Installation mode, Installed Namespace and Update Approval.
7. Click Install.

Verification

- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat - Authorino Operator shows one of the following statuses:
- In the OpenShift web console, click Operators → Installed Operators and confirm that the Red Hat - Authorino Operator shows one of the following statuses:
    - Installing - installation is in progress; wait for this to change to Succeeded. This might take several minutes.
    - Succeeded - installation is successful.

##### 5.4.3. Creating an Authorino instance

When you have installed the Red Hat - Authorino Operator on your OpenShift cluster, you must create an Authorino instance.

Prerequisites

- You have installed the Red Hat - Authorino Operator.
- You have privileges to add resources to the project in which your OpenShift Service Mesh instance was created. See Creating an OpenShift Service Mesh instance.
						
							For more information about OpenShift permissions, see Using RBAC to define and apply permissions.

Procedure

1. Open a new terminal window.
2. Log in to the OpenShift command-line interface (CLI) as follows:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;username&gt; -p &lt;password&gt;
3. Create a namespace to install the Authorino instance.
						$ oc new-project &lt;namespace\_for\_authorino\_instance&gt;Note
								The automated installation process creates a namespace called redhat-ods-applications-auth-provider for the Authorino instance. Consider using the same namespace name for the manual installation.
4. To enroll the new namespace for the Authorino instance in your existing OpenShift Service Mesh instance, create a new YAML file with the following contents:
						apiVersion: maistra.io/v1
kind: ServiceMeshMember
metadata:
  name: default
  namespace: &lt;namespace\_for\_authorino\_instance&gt;
spec:
  controlPlaneRef:
    namespace: &lt;namespace\_for\_service\_mesh\_instance&gt;
    name: &lt;name\_of\_service\_mesh\_instance&gt;
5. Save the YAML file.
6. Create the ServiceMeshMember resource on your cluster.
						$ oc create -f &lt;file\_name&gt;.yaml
7. To configure an Authorino instance, create a new YAML file as shown in the following example:
						apiVersion: operator.authorino.kuadrant.io/v1beta1
kind: Authorino
metadata:
  name: authorino
  namespace: &lt;namespace\_for\_authorino\_instance&gt;
spec:
  authConfigLabelSelectors: security.opendatahub.io/authorization-group=default
  clusterWide: true
  listener:
    tls:
      enabled: false
  oidcServer:
    tls:
      enabled: false
8. Save the YAML file.
9. Create the Authorino resource on your cluster.
						$ oc create -f &lt;file\_name&gt;.yaml
10. Patch the Authorino deployment to inject an Istio sidecar, which makes the Authorino instance part of your OpenShift Service Mesh instance.
						$ oc patch deployment &lt;name\_of\_authorino\_instance&gt; -n &lt;namespace\_for\_authorino\_instance&gt; -p '{"spec": {"template":{"metadata":{"labels":{"sidecar.istio.io/inject":"true"}}}} }'

Verification

- Confirm that the Authorino instance is running as follows:
- Confirm that the Authorino instance is running as follows:
    1. Check the pods (and containers) that are running in the namespace that you created for the Authorino instance, as shown in the following example:
								$ oc get pods -n redhat-ods-applications-auth-provider -o="custom-columns=NAME:.metadata.name,STATUS:.status.phase,CONTAINERS:.spec.containers[*].name"
    2. Confirm that the output resembles the following example:
								NAME                         STATUS    CONTAINERS
authorino-6bc64bd667-kn28z   Running   authorino,istio-proxy
									As shown in the example, there is a single running pod for the Authorino instance. The pod has containers for Authorino and for the Istio sidecar that you injected.

##### 5.4.4. Configuring an OpenShift Service Mesh instance to use Authorino

When you have created an Authorino instance, you must configure your OpenShift Service Mesh instance to use Authorino as an authorization provider.

Important

To ensure that your OpenShift Service Mesh instance remains in a supported state, make only the configuration updates shown in the following procedure.

Prerequisites

- You have created an Authorino instance and enrolled the namespace for the Authorino instance in your OpenShift Service Mesh instance.
- You have privileges to modify the OpenShift Service Mesh instance. See Creating an OpenShift Service Mesh instance.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a user that has privileges to update the OpenShift Service Mesh instance, log in to the OpenShift CLI as shown in the following example:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;username&gt; -p &lt;password&gt;
2. Create a new YAML file with the following contents:
						spec:
 techPreview:
   meshConfig:
     extensionProviders:
     - name: redhat-ods-applications-auth-provider
       envoyExtAuthzGrpc:
         service: &lt;name\_of\_authorino\_instance&gt;-authorino-authorization.&lt;namespace\_for\_authorino\_instance&gt;.svc.cluster.local
         port: 50051
3. Save the YAML file.
4. Use the oc patch command to apply the YAML file to your OpenShift Service Mesh instance.
						$ oc patch smcp &lt;name\_of\_service\_mesh\_instance&gt; --type merge -n &lt;namespace\_for\_service\_mesh\_instance&gt; --patch-file &lt;file\_name&gt;.yamlImportant
								You can apply the configuration shown as a patch only if you have not already specified other extension providers in your OpenShift Service Mesh instance. If you have already specified other extension providers, you must manually edit your ServiceMeshControlPlane resource to add the configuration.

Verification

- Verify that your Authorino instance has been added as an extension provider in your OpenShift Service Mesh configuration as follows:
- Verify that your Authorino instance has been added as an extension provider in your OpenShift Service Mesh configuration as follows:
    1. Inspect the ConfigMap object for your OpenShift Service Mesh instance:
								$ oc get configmap istio-&lt;name\_of\_service\_mesh\_instance&gt; -n &lt;namespace\_for\_service\_mesh\_instance&gt; --output=jsonpath={.data.mesh}
    2. Confirm that you see output similar to the following example, which shows that the Authorino instance has been successfully added as an extension provider.
								defaultConfig:
  discoveryAddress: istiod-data-science-smcp.istio-system.svc:15012
  proxyMetadata:
    ISTIO\_META\_DNS\_AUTO\_ALLOCATE: "true"
    ISTIO\_META\_DNS\_CAPTURE: "true"
    PROXY\_XDS\_VIA\_AGENT: "true"
  terminationDrainDuration: 35s
  tracing: {}
dnsRefreshRate: 300s
enablePrometheusMerge: true
extensionProviders:
- envoyExtAuthzGrpc:
    port: 50051
    service: authorino-authorino-authorization.opendatahub-auth-provider.svc.cluster.local
  name: opendatahub-auth-provider
ingressControllerMode: "OFF"
rootNamespace: istio-system
trustDomain: null%

##### 5.4.5. Configuring authorization for KServe

To configure the single-model serving platform to use Authorino, you must create a global AuthorizationPolicy resource that is applied to the KServe predictor pods that are created when you deploy a model. In addition, to account for the multiple network hops that occur when you make an inference request to a model, you must create an EnvoyFilter resource that continually resets the HTTP host header to the one initially included in the inference request.

Prerequisites

- You have created an Authorino instance and configured your OpenShift Service Mesh to use it.
- You have privileges to update the KServe deployment on your cluster.
- You have privileges to add resources to the project in which your OpenShift Service Mesh instance was created. See Creating an OpenShift Service Mesh instance.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a user that has privileges to update the KServe deployment, log in to the OpenShift CLI as shown in the following example:
						$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;username&gt; -p &lt;password&gt;
2. Create a new YAML file with the following contents:
						apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: kserve-predictor
spec:
  action: CUSTOM
  provider:
     name: redhat-ods-applications-auth-provider 1
  rules:
     - to:
          - operation:
               notPaths:
                  - /healthz
                  - /debug/pprof/
                  - /metrics
                  - /wait-for-drain
  selector:
     matchLabels:
        component: predictor1 
									The name that you specify must match the name of the extension provider that you added to your OpenShift Service Mesh instance.
3. Save the YAML file.
4. Create the AuthorizationPolicy resource in the namespace for your OpenShift Service Mesh instance.
						$ oc create -n &lt;namespace\_for\_service\_mesh\_instance&gt; -f &lt;file\_name&gt;.yaml
5. Create another new YAML file with the following contents:
						apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: activator-host-header
spec:
  priority: 20
  workloadSelector:
    labels:
      component: predictor
  configPatches:
  - applyTo: HTTP\_FILTER
    match:
      listener:
        filterChain:
          filter:
            name: envoy.filters.network.http\_connection\_manager
    patch:
      operation: INSERT\_BEFORE
      value:
        name: envoy.filters.http.lua
        typed\_config:
          '@type': type.googleapis.com/envoy.extensions.filters.http.lua.v3.Lua
          inlineCode: |
           function envoy\_on\_request(request\_handle)
              local headers = request\_handle:headers()
              if not headers then
                return
              end
              local original\_host = headers:get("k-original-host")
              if original\_host then
                port\_seperator = string.find(original\_host, ":", 7)
                if port\_seperator then
                  original\_host = string.sub(original\_host, 0, port\_seperator-1)
                end
                headers:replace('host', original\_host)
              end
            end
							The EnvoyFilter resource shown continually resets the HTTP host header to the one initially included in any inference request.
6. Create the EnvoyFilter resource in the namespace for your OpenShift Service Mesh instance.
						$ oc create -n &lt;namespace\_for\_service\_mesh\_instance&gt; -f &lt;file\_name&gt;.yaml

Verification

- Check that the AuthorizationPolicy resource was successfully created.
						$ oc get authorizationpolicies -n &lt;namespace\_for\_service\_mesh\_instance&gt;
							Confirm that you see output similar to the following example:
						NAME               AGE
kserve-predictor   28h
- Check that the EnvoyFilter resource was successfully created.
						$ oc get envoyfilter -n &lt;namespace\_for\_service\_mesh\_instance&gt;
							Confirm that you see output similar to the following example:
						NAME                                          AGE
activator-host-header                         28h

### Chapter 6. Installing the multi-model serving platform

For deploying small and medium-sized models, OpenShift AI includes a multi-model serving platform that is based on the ModelMesh component. On the multi-model serving platform, multiple models can be deployed from the same model server and share the server resources.

To install the multi-model serving platform or ModelMesh, follow the steps described in Installing Red Hat OpenShift AI components by using the CLI or Installing Red Hat OpenShift AI components by using the web console.

### Chapter 7. Accessing the dashboard

After you have installed OpenShift AI and added users, you can access the URL for your OpenShift AI console and share the URL with the users to let them log in and work on their models.

Prerequisites

- You have installed OpenShift AI on your OpenShift cluster.
- You have added at least one user to the user group for OpenShift AI.

Procedure

1. Log in to OpenShift web console.
2. Click the application launcher ( 
					
					 ).
3. Right-click on Red Hat OpenShift AI and copy the URL for your OpenShift AI instance.
4. Provide this instance URL to your data scientists to let them log in to OpenShift AI.

Verification

- Confirm that you and your users can log in to OpenShift AI by using the instance URL.

Additional resources

- Logging in to OpenShift AI

Adding users to OpenShift AI user groups.

### Chapter 8. Enabling accelerators

Before you can use an accelerator in OpenShift AI, you must install the relevant software components. The installation process varies based on the accelerator type.

Prerequisites

- You have logged in to your OpenShift cluster.
- You have the cluster-admin role in your OpenShift cluster.
- You have installed an accelerator and confirmed that it is detected in your environment.

Procedure

1. Follow the appropriate documentation to enable your accelerator:
2. Follow the appropriate documentation to enable your accelerator:
    - NVIDIA GPUs: See Enabling NVIDIA GPUs.
    - Intel Gaudi AI accelerators: See Enabling Intel Gaudi AI accelerators.
    - AMD GPUs: See Enabling AMD GPUs.
3. After installing your accelerator, create an accelerator profile as described in: Working with accelerator profiles.

Verification

- From the Administrator perspective, go to the Operators → Installed Operators page. Confirm that the following Operators appear:
- From the Administrator perspective, go to the Operators → Installed Operators page. Confirm that the following Operators appear:
    - The Operator for your accelerator
    - Node Feature Discovery (NFD)
    - Kernel Module Management (KMM)
- The accelerator is correctly detected a few minutes after full installation of the Node Feature Discovery (NFD) and the relevant accelerator Operator. The OpenShift command line interface (CLI) displays the appropriate output for the GPU worker node. For example, here is output confirming that an NVIDIA GPU is detected:
				# Expected output when the accelerator is detected correctly
oc describe node &lt;node name&gt;
...
Capacity:
  cpu:                4
  ephemeral-storage:  313981932Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             16076568Ki
  nvidia.com/gpu:     1
  pods:               250
Allocatable:
  cpu:                3920m
  ephemeral-storage:  288292006229
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             12828440Ki
  nvidia.com/gpu:     1
  pods:               250

### Chapter 9. Working with certificates

Certificates are used by various components in OpenShift to validate access to the cluster. For clusters that rely on self-signed certificates, you can add those self-signed certificates to a cluster-wide Certificate Authority (CA) bundle and use the CA bundle in Red Hat OpenShift AI. You can also use self-signed certificates in a custom CA bundle that is separate from the cluster-wide bundle. Administrators can add a CA bundle, remove a CA bundle from all namespaces, remove a CA bundle from individual namespaces, or manually manage certificate changes instead of the system.

#### 9.1. Understanding certificates in OpenShift AI

For OpenShift clusters that rely on self-signed certificates, you can add those self-signed certificates to a cluster-wide Certificate Authority (CA) bundle (ca-bundle.crt) and use the CA bundle in Red Hat OpenShift AI. You can also use self-signed certificates in a custom CA bundle (odh-ca-bundle.crt) that is separate from the cluster-wide bundle.

##### 9.1.1. How CA bundles are injected

After installing OpenShift AI, the Red Hat OpenShift AI Operator automatically creates an empty odh-trusted-ca-bundle configuration file (ConfigMap), and the Cluster Network Operator (CNO) injects the cluster-wide CA bundle into the odh-trusted-ca-bundle configMap with the label "config.openshift.io/inject-trusted-cabundle". The components deployed in the affected namespaces are responsible for mounting this configMap as a volume in the deployment pods.

```
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/part-of: opendatahub-operator
    config.openshift.io/inject-trusted-cabundle: 'true'
  name: odh-trusted-ca-bundle
```

After the CNO operator injects the bundle, it updates the ConfigMap with the ca-bundle.crt file containing the certificates.

```
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/part-of: opendatahub-operator
    config.openshift.io/inject-trusted-cabundle: 'true'
  name: odh-trusted-ca-bundle
data:
  ca-bundle.crt: |
    <BUNDLE OF CLUSTER-WIDE CERTIFICATES>
```

##### 9.1.2. How the ConfigMap is managed

By default, the Red Hat OpenShift AI Operator manages the odh-trusted-ca-bundle ConfigMap. If you want to manage or remove the odh-trusted-ca-bundle ConfigMap, or add a custom CA bundle (odh-ca-bundle.crt) separate from the cluster-wide CA bundle (ca-bundle.crt), you can use the trustedCABundle property in the Operator’s DSC Initialization (DSCI) object.

```
spec:
  trustedCABundle:
    managementState: Managed
    customCABundle: ""
```

In the Operator’s DSCI object, you can set the spec.trustedCABundle.managementState field to the following values:

- Managed: The Red Hat OpenShift AI Operator manages the odh-trusted-ca-bundle ConfigMap and adds it to all non-reserved existing and new namespaces (the ConfigMap is not added to any reserved or system namespaces, such as default, openshift-\* or kube-*). The ConfigMap is automatically updated to reflect any changes made to the customCABundle field. This is the default value after installing Red Hat OpenShift AI.
- Removed: The Red Hat OpenShift AI Operator removes the odh-trusted-ca-bundle ConfigMap (if present) and disables the creation of the ConfigMap in new namespaces. If you change this field from Managed to Removed, the odh-trusted-ca-bundle ConfigMap is also deleted from namespaces. This is the default value after upgrading Red Hat OpenShift AI from 2.7 or earlier versions to 2.18.
- Unmanaged: The Red Hat OpenShift AI Operator does not manage the odh-trusted-ca-bundle ConfigMap, allowing for an administrator to manage it instead. Changing the managementState from Managed to Unmanaged does not remove the odh-trusted-ca-bundle ConfigMap, but the ConfigMap is not updated if you make changes to the customCABundle field.

In the Operator’s DSCI object, you can add a custom certificate to the spec.trustedCABundle.customCABundle field. This adds the odh-ca-bundle.crt file containing the certificates to the odh-trusted-ca-bundle ConfigMap, as shown in the following example:

```
apiVersion: v1
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/part-of: opendatahub-operator
    config.openshift.io/inject-trusted-cabundle: 'true'
  name: odh-trusted-ca-bundle
data:
  ca-bundle.crt: |
    <BUNDLE OF CLUSTER-WIDE CERTIFICATES>
  odh-ca-bundle.crt: |
    <BUNDLE OF CUSTOM CERTIFICATES>
```

#### 9.2. Adding a CA bundle

There are two ways to add a Certificate Authority (CA) bundle to OpenShift AI. You can use one or both of these methods:

- For OpenShift clusters that rely on self-signed certificates, you can add those self-signed certificates to a cluster-wide Certificate Authority (CA) bundle (ca-bundle.crt) and use the CA bundle in Red Hat OpenShift AI. To use this method, log in to the OpenShift as a cluster administrator and follow the steps as described in Configuring the cluster-wide proxy during installation.
- You can use self-signed certificates in a custom CA bundle (odh-ca-bundle.crt) that is separate from the cluster-wide bundle. To use this method, follow the steps in this section.

Prerequisites

- You have admin access to the DSCInitialization resources in the OpenShift cluster.
- You installed the OpenShift command line interface (oc) as described in Installing the OpenShift CLI.
- You are working in a new installation of Red Hat OpenShift AI. If you upgraded Red Hat OpenShift AI, see Adding a CA bundle after upgrading.

Procedure

1. Log in to the OpenShift.
2. Click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Click the DSC Initialization tab.
4. Click the default-dsci object.
5. Click the YAML tab.
6. In the spec section, add the custom certificate to the customCABundle field for trustedCABundle, as shown in the following example:
					spec:
  trustedCABundle:
    managementState: Managed
    customCABundle: |
      -----BEGIN CERTIFICATE-----
      examplebundle123
      -----END CERTIFICATE-----
7. Click Save.

Verification

- If you are using a cluster-wide CA bundle, run the following command to verify that all non-reserved namespaces contain the odh-trusted-ca-bundle ConfigMap:
					$ oc get configmaps --all-namespaces -l app.kubernetes.io/part-of=opendatahub-operator | grep odh-trusted-ca-bundle
- If you are using a custom CA bundle, run the following command to verify that a non-reserved namespace contains the odh-trusted-ca-bundle ConfigMap and that the ConfigMap contains your customCABundle value. In the following command, example-namespace is the non-reserved namespace and examplebundle123 is the customCABundle value.
					$ oc get configmap odh-trusted-ca-bundle -n example-namespace -o yaml | grep examplebundle123

#### 9.3. Removing a CA bundle

You can remove a Certificate Authority (CA) bundle from all non-reserved namespaces in OpenShift AI. This process changes the default configuration and disables the creation of the odh-trusted-ca-bundle configuration file (ConfigMap), as described in Understanding certificates in OpenShift AI.

Note

The odh-trusted-ca-bundle ConfigMaps are only deleted from namespaces when you set the managementState of trustedCABundle to Removed; deleting the DSC Initialization does not delete the ConfigMaps.

To remove a CA bundle from a single namespace only, see Removing a CA bundle from a namespace.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You installed the OpenShift command line interface (oc) as described in Installing the OpenShift CLI.

Procedure

1. In the OpenShift web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
2. Click the DSC Initialization tab.
3. Click the default-dsci object.
4. Click the YAML tab.
5. In the spec section, change the value of the managementState field for trustedCABundle to Removed:
					spec:
  trustedCABundle:
    managementState: Removed
6. Click Save.

Verification

- Run the following command to verify that the odh-trusted-ca-bundle ConfigMap has been removed from all namespaces:
					$ oc get configmaps --all-namespaces | grep odh-trusted-ca-bundle
						The command should not return any ConfigMaps.

#### 9.4. Removing a CA bundle from a namespace

You can remove a custom Certificate Authority (CA) bundle from individual namespaces in OpenShift AI. This process disables the creation of the odh-trusted-ca-bundle configuration file (ConfigMap) for the specified namespace only.

To remove a certificate bundle from all namespaces, see Removing a CA bundle.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You installed the OpenShift command line interface (oc) as described in Installing the OpenShift CLI.

Procedure

- Run the following command to remove a CA bundle from a namespace. In the following command, example-namespace is the non-reserved namespace.
					$ oc annotate ns example-namespace security.opendatahub.io/inject-trusted-ca-bundle=false

Verification

- Run the following command to verify that the CA bundle has been removed from the namespace. In the following command, example-namespace is the non-reserved namespace.
					$ oc get configmap odh-trusted-ca-bundle -n example-namespace
						The command should return configmaps "odh-trusted-ca-bundle" not found.

#### 9.5. Managing certificates

After installing OpenShift AI, the Red Hat OpenShift AI Operator creates the odh-trusted-ca-bundle configuration file (ConfigMap) that contains the trusted CA bundle and adds it to all new and existing non-reserved namespaces in the cluster. By default, the Red Hat OpenShift AI Operator manages the odh-trusted-ca-bundle ConfigMap and automatically updates it if any changes are made to the CA bundle. You can choose to manage the odh-trusted-ca-bundle ConfigMap instead of allowing the Red Hat OpenShift AI Operator to manage it.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.

Procedure

1. In the OpenShift web console, click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
2. Click the DSC Initialization tab.
3. Click the default-dsci object.
4. Click the YAML tab.
5. In the spec section, change the value of the managementState field for trustedCABundle to Unmanaged, as shown:
					spec:
  trustedCABundle:
    managementState: Unmanaged
6. Click Save.
					
						Note that changing the managementState from Managed to Unmanaged does not remove the odh-trusted-ca-bundle ConfigMap, but the ConfigMap is not updated if you make changes to the customCABundle field.

Verification

1. In the spec section, set or change the value of the customCABundle field for trustedCABundle, for example:
					spec:
  trustedCABundle:
    managementState: Unmanaged
    customCABundle: example123
2. Click Save.
3. Click Workloads → ConfigMaps.
4. Select a project from the project list.
5. Click the odh-trusted-ca-bundle ConfigMap.
6. Click the YAML tab and verify that the value of the customCABundle field did not update.

#### 9.6. Accessing S3-compatible object storage with self-signed certificates

To use object storage solutions or databases that are deployed in an OpenShift cluster that uses self-signed certificates, you must configure OpenShift AI to trust the cluster’s certificate authority (CA).

Each namespace has a ConfigMap called kube-root-ca.crt that contains the CA certificates of the internal API Server. Use the following steps to configure OpenShift AI to trust the certificates issued by kube-root-ca.crt.

Alternatively, you can add a custom CA bundle by using the OpenShift console, as described in Adding a CA bundle.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have an object storage solution or database deployed in your OpenShift cluster.

Procedure

1. In a terminal window, log in to the OpenShift CLI as shown in the following example:
					oc login api.&lt;cluster\_name&gt;.&lt;cluster\_domain&gt;:6443 --web
2. Run the following command to fetch the current OpenShift AI trusted CA configuration and store it in a new file:
					oc get dscinitializations.dscinitialization.opendatahub.io default-dsci -o json | jq -r '.spec.trustedCABundle.customCABundle' &gt; /tmp/my-custom-ca-bundles.crt
3. Add the cluster’s kube-root-ca.crt ConfigMap to the OpenShift AI trusted CA configuration:
					oc get configmap kube-root-ca.crt -o jsonpath="{['data']['ca\.crt']}" &gt;&gt; /tmp/my-custom-ca-bundles.crt
4. Update the OpenShift AI trusted CA configuration to trust certificates issued by the certificate authorities in kube-root-ca.crt:
					oc patch dscinitialization default-dsci --type='json' -p='[{"op":"replace","path":"/spec/trustedCABundle/customCABundle","value":"'"$(awk '{printf "%s\\n", $0}' /tmp/my-custom-ca-bundles.crt)"'"}]'

Verification

- You can successfully deploy components that are configured to use object storage solutions or databases that are deployed in the OpenShift cluster. For example, a pipeline server that is configured to use a database deployed in the cluster starts successfully.

Note

You can verify your new certificate configuration by following the steps in the OpenShift AI fraud detection tutorial. Run the script to install local object storage buckets and create connections, and then enable data science pipelines.

For more information about running the script to install local object storage buckets, see Running a script to install local object storage buckets and create connections.

For more information about enabling data science pipelines, see Enabling data science pipelines.

#### 9.7. Using self-signed certificates with OpenShift AI components

Some OpenShift AI components have additional options or required configuration for self-signed certificates.

##### 9.7.1. Using certificates with data science pipelines

If you want to use self-signed certificates, you have added them to a central Certificate Authority (CA) bundle as described in Working with certificates (for disconnected environments, see Working with certificates).

No additional configuration is necessary to use those certificates with data science pipelines.

###### 9.7.1.1. Providing a CA bundle only for data science pipelines

Perform the following steps to provide a Certificate Authority (CA) bundle just for data science pipelines.

Procedure

1. Log in to OpenShift.
2. From Workloads → ConfigMaps, create a ConfigMap with the required bundle in the same data science project or namespace as the target data science pipeline:
							kind: ConfigMap
apiVersion: v1
metadata:
    name: custom-ca-bundle
data:
    ca-bundle.crt: |
    # contents of ca-bundle.crt
3. Add the following snippet to the .spec.apiserver.caBundle field of the underlying Data Science Pipelines Application (DSPA):
							apiVersion: datasciencepipelinesapplications.opendatahub.io/v1
kind: DataSciencePipelinesApplication
metadata:
    name: data-science-dspa
spec:
    ...
    apiServer:
    ...
    cABundle:
        configMapName: custom-ca-bundle
        configMapKey: ca-bundle.crt

The pipeline server pod redeploys with the updated bundle and uses it in the newly created pipeline pods.

Verification

Perform the following steps to confirm that your CA bundle was successfully mounted.

1. Log in to the OpenShift console.
2. Go to the OpenShift project that corresponds to the data science project.
3. Click the Pods tab.
4. Click the pipeline server pod with the ds-pipeline-dspa-&lt;hash&gt; prefix.
5. Click Terminal.
6. Enter cat /dsp-custom-certs/dsp-ca.crt.
7. Verify that your CA bundle is present within this file.

You can also confirm that your CA bundle was successfully mounted by using the CLI:

1. In a terminal window, log in to the OpenShift cluster where OpenShift AI is deployed.
							oc login
2. Set the dspa value:
							dspa=dspa
3. Set the dsProject value, replacing $YOUR\_DS\_PROJECT with the name of your data science project:
							dsProject=$YOUR\_DS\_PROJECT
4. Set the pod value:
							pod=$(oc get pod -n ${dsProject} -l app=ds-pipeline-${dspa} --no-headers | awk '{print $1}')
5. Display the contents of the /dsp-custom-certs/dsp-ca.crt file:
							oc -n ${dsProject} exec $pod -- cat /dsp-custom-certs/dsp-ca.crt
6. Verify that your CA bundle is present within this file.

##### 9.7.2. Using certificates with workbenches

Important

Self-signed certificates apply by default to workbenches that you create after configuring the certificates centrally as described in Working with certificates (for disconnected environments, see Working with certificates). To apply centrally configured certificates to an existing workbench, stop and then restart the workbench.

Self-signed certificates are stored in /etc/pki/tls/custom-certs/ca-bundle.crt. Workbenches are preset with an environment variable that points packages to this path, and that covers many popular HTTP client packages. For packages that are not included by default, you can provide this certificate path. For example, for the kfp package to connect to the data science pipeline server:

```
from kfp.client import Client

with open(sa_token_file_path, 'r') as token_file:
    bearer_token = token_file.read()

    client = Client(
        host='https://<GO_TO_ROUTER_OF_DS_PROJECT>/',
        existing_token=bearer_token,
        ssl_ca_cert='/etc/pki/tls/custom-certs/ca-bundle.crt'
    )
    print(client.list_experiments())
```

### Chapter 10. Viewing logs and audit records

As a cluster administrator, you can use the OpenShift AI Operator logger to monitor and troubleshoot issues. You can also use OpenShift audit records to review a history of changes made to the OpenShift AI Operator configuration.

#### 10.1. Configuring the OpenShift AI Operator logger

You can change the log level for OpenShift AI Operator components by setting the .spec.devFlags.logmode flag for the DSC Initialization/DSCI custom resource during runtime. If you do not set a logmode value, the logger uses the INFO log level by default.

The log level that you set with .spec.devFlags.logmode applies to all components, not just those in a Managed state.

The following table shows the available log levels:

| Log level                    | Stacktrace level   | Verbosity   | Output   | Timestamp type            |
|------------------------------|--------------------|-------------|----------|---------------------------|
| devel or development         | WARN               | INFO        | Console  | Epoch timestamps          |
| "" (or no logmode value set) | ERROR              | INFO        | JSON     | Human-readable timestamps |
| prod or production           | ERROR              | INFO        | JSON     | Human-readable timestamps |

Logs that are set to devel or development generate in a plain text console format. Logs that are set to prod, production, or which do not have a level set generate in a JSON format.

Prerequisites

- You have admin access to the DSCInitialization resources in the OpenShift cluster.
- You installed the OpenShift command line interface (oc) as described in Installing the OpenShift CLI.

Procedure

1. Log in to the OpenShift as a cluster administrator.
2. Click Operators → Installed Operators and then click the Red Hat OpenShift AI Operator.
3. Click the DSC Initialization tab.
4. Click the default-dsci object.
5. Click the YAML tab.
6. In the spec section, update the .spec.devFlags.logmode flag with the log level that you want to set.
					apiVersion: dscinitialization.opendatahub.io/v1
kind: DSCInitialization
metadata:
  name: default-dsci
spec:
  devFlags:
    logmode: development
7. Click Save.

You can also configure the log level from the OpenShift CLI by using the following command with the logmode value set to the log level that you want.

```
oc patch dsci default-dsci -p '{"spec":{"devFlags":{"logmode":"development"}}}' --type=merge
```

Verification

- If you set the component log level to devel or development, logs generate more frequently and include logs at WARN level and above.
- If you set the component log level to prod or production, or do not set a log level, logs generate less frequently and include logs at ERROR level or above.

##### 10.1.1. Viewing the OpenShift AI Operator log

1. Log in to the OpenShift CLI.
2. Run the following command:
						oc get pods -l name=rhods-operator -o name -n redhat-ods-operator |  xargs -I {} oc logs -f {} -n redhat-ods-operator
							The operator pod log opens.

You can also view the operator pod log in the OpenShift Console, under Workloads &gt; Deployments &gt; Pods &gt; redhat-ods-operator &gt; Logs.

#### 10.2. Viewing audit records

Cluster administrators can use OpenShift auditing to see changes made to the OpenShift AI Operator configuration by reviewing modifications to the DataScienceCluster (DSC) and DSCInitialization (DSCI) custom resources. Audit logging is enabled by default in standard OpenShift cluster configurations. For more information, see Viewing audit logs in the OpenShift documentation.

Note

In Red Hat OpenShift Service on Amazon Web Services with hosted control planes (ROSA HCP), audit logging is disabled by default because the Elasticsearch log store does not provide secure storage for audit logs. To send the audit logs to Amazon CloudWatch, see Forwarding logs to Amazon CloudWatch.

The following example shows how to use the OpenShift audit logs to see the history of changes made (by users) to the DSC and DSCI custom resources.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You installed the OpenShift command line interface (oc) as described in Installing the OpenShift CLI.

Procedure

1. In a terminal window, if you are not already logged in to your OpenShift cluster as a cluster administrator, log in to the OpenShift CLI as shown in the following example:
					$ oc login &lt;openshift\_cluster\_url&gt; -u &lt;admin\_username&gt; -p &lt;password&gt;
2. To access the full content of the changed custom resources, set the OpenShift audit log policy to WriteRequestBodies or a more comprehensive profile. For more information, see About audit log policy profiles.
3. Fetch the audit log files that are available for the relevant control plane nodes. For example:
					oc adm node-logs --role=master --path=kube-apiserver/ \
  | awk '{ print $1 }' | sort -u \
  | while read node ; do
      oc adm node-logs $node --path=kube-apiserver/audit.log &lt; /dev/null
    done \
  | grep opendatahub &gt; /tmp/kube-apiserver-audit-opendatahub.log
4. Search the files for the DSC and DSCI custom resources. For example:
					jq 'select((.objectRef.apiGroup == "dscinitialization.opendatahub.io"
                or .objectRef.apiGroup == "datasciencecluster.opendatahub.io")
              and .user.username != "system:serviceaccount:redhat-ods-operator:redhat-ods-operator-controller-manager"
              and .verb != "get" and .verb != "watch" and .verb != "list")' &lt; /tmp/kube-apiserver-audit-opendatahub.log

Verification

- The commands return relevant log entries.

Tip

To configure the log retention time, see the Logging section in the OpenShift documentation.

Additional resources

- Viewing audit logs
- About audit log policy profiles

### Chapter 11. Troubleshooting common installation problems

If you are experiencing difficulties installing the Red Hat OpenShift AI Operator, read this section to understand what could be causing the problem and how to resolve it.

If the problem is not included here or in the release notes, contact Red Hat Support. When opening a support case, it is helpful to include debugging information about your cluster. You can collect this information by using the must-gather tool as described in Must-Gather for Red Hat OpenShift AI and Gathering data about your cluster.

You can also adjust the log level of OpenShift AI Operator components to increase or reduce log verbosity to suit your use case. For more information, see Configuring the OpenShift AI Operator logger.

#### 11.1. The Red Hat OpenShift AI Operator cannot be retrieved from the image registry

Problem

When attempting to retrieve the Red Hat OpenShift AI Operator from the image registry, an Failure to pull from quay error message appears. The Red Hat OpenShift AI Operator might be unavailable for retrieval in the following circumstances:

- The image registry is unavailable.
- There is a problem with your network connection.
- Your cluster is not operational and is therefore unable to retrieve the image registry.

Diagnosis

Check the logs in the Events section in OpenShift for further information about the Failure to pull from quay error message.

Resolution

- Contact Red Hat support.

#### 11.2. OpenShift AI does not install on unsupported infrastructure

Problem

You are deploying on an environment that is not documented as supported by the Red Hat OpenShift AI Operator.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Deploying on $infrastructure, which is not supported. Failing Installation error message.

Resolution

- Before proceeding with a new installation, ensure that you have a fully supported environment on which to install OpenShift AI. For more information, see Red Hat OpenShift AI: Supported Configurations.

#### 11.3. The creation of the OpenShift AI Custom Resource (CR) fails

Problem

During the installation process, the OpenShift AI Custom Resource (CR) does not get created. This issue occurs in unknown circumstances.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Attempt to create the ODH CR failed. error message.

Resolution

- Contact Red Hat support.

#### 11.4. The creation of the OpenShift AI Notebooks Custom Resource (CR) fails

Problem

During the installation process, the OpenShift AI Notebooks Custom Resource (CR) does not get created. This issue occurs in unknown circumstances.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Attempt to create the RHODS Notebooks CR failed. error message.

Resolution

- Contact Red Hat support.

#### 11.5. The OpenShift AI dashboard is not accessible

Problem

After installing OpenShift AI, the redhat-ods-applications, redhat-ods-monitoring, and redhat-ods-operator project namespaces are Active but you cannot access the dashboard due to an error in the pod.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects.
4. Click Filter and select the checkbox for every status except Running and Completed.
					
						The page displays the pods that have an error.

Resolution

- To see more information and troubleshooting steps for a pod, on the Pods page, click the link in the Status column for the pod.
- If the Status column does not display a link, click the pod name to open the pod details page and then click the Logs tab.

#### 11.6. Reinstalling OpenShift AI fails with an error

Problem

After uninstalling the OpenShift AI Operator and reinstalling it by using the CLI, the reinstallation fails with an unable to find DSCInitialization error in the OpenShift AI Operator pod log. This issue can occur if the Auth custom resource from the previous installation was not deleted after uninstalling the OpenShift AI Operator and before reinstalling it. For more information, see Understanding the uninstallation process.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for an error message similar to the following:
					{"name":"auth"},"namespace":"","name":"auth","reconcileID":"7bff53ae-1252-46fe-831a-fdc824078a1b","error":"unable to find DSCInitialization","stacktrace":"sigs.k8s.io/controller-runtime/pkg/internal/controller.

Resolution

1. Uninstall the OpenShift AI Operator.
2. Delete the Auth custom resource:
3. Delete the Auth custom resource:
    1. In the OpenShift web console, switch to the Administrator perspective.
    2. Click API Explorer.
    3. From the All groups drop-down list, select or enter services.platform.opendatahub.io.
    4. Click the Auth kind.
    5. Click the Instances tab.
    6. Click the action menu (⋮) and select Delete Auth.
							
								The Delete Auth dialog appears.
    7. Click Delete.
4. Install the OpenShift AI Operator again.

#### 11.7. The dedicated-admins Role-based access control (RBAC) policy cannot be created

Problem

The Role-based access control (RBAC) policy for the dedicated-admins group in the target project cannot be created. This issue occurs in unknown circumstances.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Attempt to create the RBAC policy for dedicated admins group in $target\_project failed. error message.

Resolution

- Contact Red Hat support.

#### 11.8. The PagerDuty secret does not get created

Problem

An issue with Managed Tenants SRE automation process causes the PagerDuty’s secret to not get created.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Pagerduty secret does not exist error message.

Resolution

- Contact Red Hat support.

#### 11.9. The SMTP secret does not exist

Problem

An issue with Managed Tenants SRE automation process causes the SMTP secret to not get created.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: SMTP secret does not exist error message.

Resolution

- Contact Red Hat support.

#### 11.10. The ODH parameter secret does not get created

Problem

An issue with the OpenShift AI Operator’s flow could result in failure to create the ODH parameter.

Diagnosis

1. In the OpenShift web console, switch to the Administrator perspective.
2. Click Workloads → Pods.
3. Set the Project to All Projects or redhat-ods-operator.
4. Click the rhods-operator-&lt;random string&gt; pod.
					
						The Pod details page appears.
5. Click Logs.
6. Select rhods-operator from the drop-down list.
7. Check the log for the ERROR: Addon managed odh parameter secret does not exist. error message.

Resolution

- Contact Red Hat support.

#### 11.11. Data science pipelines are not enabled after installing OpenShift AI 2.9 or later due to existing Argo Workflows resources

Problem

After installing OpenShift AI 2.9 or later with an Argo Workflows installation that is not installed by OpenShift AI on your cluster, data science pipelines are not enabled despite the datasciencepipelines component being enabled in the DataScienceCluster object.

Diagnosis

After you install OpenShift AI 2.9 or later, the Data Science Pipelines tab is not visible on the OpenShift AI dashboard navigation menu.

Resolution

- Delete the separate installation of Argo workflows on your cluster. After you have removed any Argo Workflows resources that are not created by OpenShift AI from your cluster, data science pipelines are enabled automatically.

### Chapter 12. Uninstalling Red Hat OpenShift AI Self-Managed

This section shows how to use the OpenShift command-line interface (CLI) to uninstall the Red Hat OpenShift AI Operator and any OpenShift AI components installed and managed by the Operator.

Note

Using the CLI is the recommended way to uninstall the Operator. Depending on your version of OpenShift, using the web console to perform the uninstallation might not prompt you to uninstall all associated components. This could leave you unclear about the final state of your cluster.

#### 12.1. Understanding the uninstallation process

Installing Red Hat OpenShift AI created several custom resource instances on your OpenShift cluster for various components of OpenShift AI. After installation, users likely created several additional resources while using OpenShift AI. Uninstalling OpenShift AI removes the resources that were created by the Operator, but retains the resources created by users to prevent inadvertently deleting information you might want.

What is deleted

Uninstalling OpenShift AI removes the following resources from your OpenShift cluster:

- DataScienceCluster custom resource instance and the custom resource instances it created for each component
- DSCInitialization custom resource instance
- Auth custom resource instance created during or after installation
- FeatureTracker custom resource instances created during or after installation
- ServiceMesh custom resource instance created by the Operator during or after installation
- KNativeServing custom resource instance created by the Operator during or after installation
- redhat-ods-applications, redhat-ods-monitoring, and rhods-notebooks namespaces created by the Operator
- Workloads in the rhods-notebooks namespace
- Subscription, ClusterServiceVersion, and InstallPlan objects
- KfDef object (version 1 Operator only)

What might remain

Uninstalling OpenShift AI retains the following resources in your OpenShift cluster:

- Data science projects created by users
- Custom resource instances created by users
- Custom resource definitions (CRDs) created by users or by the Operator

While these resources might still remain in your OpenShift cluster, they are not functional. After uninstalling, Red Hat recommends that you review the data science projects and custom resources in your OpenShift cluster and delete anything no longer in use to prevent potential issues, such as pipelines that cannot run, notebooks that cannot be undeployed, or models that cannot be undeployed.

Additional resources

Operator Lifecycle Manager (OLM) uninstall documentation

#### 12.2. Uninstalling OpenShift AI Self-Managed by using the CLI

The following procedure shows how to use the OpenShift command-line interface (CLI) to uninstall the Red Hat OpenShift AI Operator and any OpenShift AI components installed and managed by the Operator.

Prerequisites

- You have cluster administrator privileges for your OpenShift cluster.
- You have downloaded and installed the OpenShift command-line interface (CLI). See Installing the OpenShift CLI.
- You have backed up the persistent disks or volumes used by your persistent volume claims (PVCs).

Procedure

1. Open a new terminal window.
2. In the OpenShift command-line interface (CLI), log in to your OpenShift cluster as a cluster administrator, as shown in the following example:
					$ oc login &lt;openshift\_cluster\_url&gt; -u system:admin
3. Create a ConfigMap object for deletion of the Red Hat OpenShift AI Operator.
					$ oc create configmap delete-self-managed-odh -n redhat-ods-operator
4. To delete the rhods-operator, set the addon-managed-odh-delete label to true.
					$ oc label configmap/delete-self-managed-odh api.openshift.com/addon-managed-odh-delete=true -n redhat-ods-operator
5. When all objects associated with the Operator are removed, delete the redhat-ods-operator project.
6. When all objects associated with the Operator are removed, delete the redhat-ods-operator project.
    1. Set an environment variable for the redhat-ods-applications project.
							$ PROJECT\_NAME=redhat-ods-applications
    2. Wait until the redhat-ods-applications project has been deleted.
							$ while oc get project $PROJECT\_NAME &amp;&gt; /dev/null; do
echo "The $PROJECT\_NAME project still exists"
sleep 1
done
echo "The $PROJECT\_NAME project no longer exists"
								When the redhat-ods-applications project has been deleted, you see the following output.
							The redhat-ods-applications project no longer exists
    3. When the redhat-ods-applications project has been deleted, delete the redhat-ods-operator project.
							$ oc delete namespace redhat-ods-operator

Verification

- Confirm that the rhods-operator subscription no longer exists.
					$ oc get subscriptions --all-namespaces | grep rhods-operator
- Confirm that the following projects no longer exist.
- Confirm that the following projects no longer exist.
    - redhat-ods-applications
    - redhat-ods-monitoring
    - redhat-ods-operator
    - rhods-notebooks
$ oc get namespaces | grep -e redhat-ods* -e rhods*Note
									The rhods-notebooks project was created only if you installed the workbenches component of OpenShift AI. See Installing and managing Red Hat OpenShift AI components.

### Legal Notice

Copyright © 2025 Red Hat, Inc.

The text of and illustrations in this document are licensed by Red Hat under a Creative Commons Attribution–Share Alike 3.0 Unported license ("CC-BY-SA"). An explanation of CC-BY-SA is available at . In accordance with CC-BY-SA, if you distribute this document or an adaptation of it, you must provide the URL for the original version.

Red Hat, as the licensor of this document, waives the right to enforce, and agrees not to assert, Section 4d of CC-BY-SA to the fullest extent permitted by applicable law.

Red Hat, Red Hat Enterprise Linux, the Shadowman logo, the Red Hat logo, JBoss, OpenShift, Fedora, the Infinity logo, and RHCE are trademarks of Red Hat, Inc., registered in the United States and other countries.

® is the registered trademark of Linus Torvalds in the United States and other countries.

® is a registered trademark of Oracle and/or its affiliates.

® is a trademark of Silicon Graphics International Corp. or its subsidiaries in the United States and/or other countries.

® is a registered trademark of MySQL AB in the United States, the European Union and other countries.

® is an official trademark of Joyent. Red Hat is not formally related to or endorsed by the official Joyent Node.js open source or commercial project.

The ® Word Mark and OpenStack logo are either registered trademarks/service marks or trademarks/service marks of the OpenStack Foundation, in the United States and other countries and are used with the OpenStack Foundation's permission. We are not affiliated with, endorsed or sponsored by the OpenStack Foundation, or the OpenStack community.

All other trademarks are the property of their respective owners.

<!-- 🖼️❌ Image not available. Please use `PdfPipelineOptions(generate_picture_images=True)` -->

#### Learn

- Developer resources
- Cloud learning hub
- Interactive labs
- Training and certification
- Customer support
- See all documentation

#### Try, buy, &amp; sell

- Product trial center
- Red Hat Ecosystem Catalog
- Red Hat Store
- Buy online (Japan)

#### Communities

- Customer Portal Community
- Events
- How we contribute

#### About Red Hat Documentation

We help Red Hat users innovate and achieve their goals with our products and services with content they can trust. Explore our recent updates.

#### Making open source more inclusive

Red Hat is committed to replacing problematic language in our code, documentation, and web properties.

#### About Red Hat

We deliver hardened solutions that make it easier for enterprises to work across platforms and environments, from the core datacenter to the network edge.

#### Red Hat legal and privacy links

- About Red Hat
- Jobs
- Events
- Locations
- Contact Red Hat
- Red Hat Blog
- Inclusion at Red Hat
- Cool Stuff Store
- Red Hat Summit

#### Red Hat legal and privacy links

- Privacy statement
- Terms of use
- All policies and guidelines
- Digital accessibility