---
layout: default
title: CDP Data Services Control Plane on Openshift
parent: "Openshift Deployment"
grand_parent: CDP Private Cloud
nav_order: 3
---

# CDP Data Services Control Plane on Openshift
{: .no_toc }

This article describes the steps to deploy the CDP Data Services Control Plane on the Openshift platform.

---

1. In Cloudera Manager portal, navigate to `Data Services` on the left panel and click `Add CDP Private Cloud Containerized Cluster`.

    ![](../../assets/images/ocp4/addocp1.png)
    
2. Click `here` link.

    ![](../../assets/images/ocp4/addocp2.png)   

3. Select the 1.3.4 version and click `Next`.

    ![](../../assets/images/ocp4/addocp3.png)  
    
4. Select `Use a custom Docker Repository (Recommended for production)` option. Enter the fields based on the setup of the [Docker registry in the Nexus server]({{ site.baseurl }}{% link docs/cdppvc/nexus.md %}).

    ![](../../assets/images/ocp4/addocp4.png)     

5. Select `Use existing databases (Recommended for production)` option. Enter the fields based on the prerequisites highlighted in the [Installation Prerequisites]({{ site.baseurl }}{% link docs/cdppvc/prerequisites.md %}) page.

    ![](../../assets/images/ocp4/addocp5.png)  
    
6. Select the `kubeconfig` file as highlighted in the [Installation Prerequisites](% link docs/cdppvc/prerequisites.md %}) page. 
For Vault configuration, enter the fields based on the setup of the [Hashicorp Vault]({{ site.baseurl }}{% link docs/cdppvc/vault.md %}). 
For Storage, enter the default block storageClass of the deployed [OCS]({{ site.baseurl }}{% link docs/cdppvc/prerequisites.md %}).

    ![](../../assets/images/ocp4/addocp6.png)  
    
7. Click `Next`.

    ![](../../assets/images/ocp4/addocp7.png)  
    
8. Click `Next`.   
    ![](../../assets/images/ocp4/addocp8.png)  
    
9. Click `Finish`.    
    ![](../../assets/images/ocp4/addocp9.png)  
    
10. The successful installation creates CDP PvC Control Plane on the Openshift platform as shown below.
    ![](../../assets/images/ocp4/addocp10.png)  
     
    
