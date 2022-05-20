---
layout: default
title: CDP Base Configuration
parent: Installation
grand_parent: CDP Private Cloud
nav_order: 4
---

# CDP Base Configuration
{: .no_toc }

Check and make necessary configurations to ensure that the CDP Base cluster has been set up correctly prior to installing the ECS platform. 

- TOC
{:toc}

---

## Check Dependencies

1. After the CDP Base has successfully been installed, the dependencies should be reflected in the each service's configurations as shown below. If not, please amend the configurations accordingly.

    ![](../../assets/images/cdpbase/hiveconfig.png)
    
    ![](../../assets/images/cdpbase/atlasconfig.png)
    
    ![](../../assets/images/cdpbase/solrconfig.png)
    
    ![](../../assets/images/cdpbase/hdfsconfig.png)
    
    ![](../../assets/images/cdpbase/yarnconfig.png)
    
    ![](../../assets/images/cdpbase/kafkaconfig.png)

    ![](../../assets/images/cdpbase/hbaseconfig.png)

    ![](../../assets/images/cdpbase/yarnqueueconfig.png)
    
    ![](../../assets/images/cdpbase/ozone.png)
    

## Ranger Configuration

1. After the CDP Base has successfully been installed, the dependencies should be reflected in the each service's configurations     

## Ranger Configuration

1. The external LDAP server is the centralized user authentication database that stores the user credentials with the associated group. This demo is connected to Red Hat IPA. Configure Ranger with the necessary external LDAP server settings. Navigate to `base 1` > `Ranger` > `Configurations`.

| Parameter       | Value         |
|:----------------|:------------------|
| Admin LDAP Auth URL          |   | 
| Admin LDAP Auth Bind User   |   | 
| Admin LDAP Auth Bind User Password  |   | 
| Admin LDAP Auth User DN Pattern |   | 
| Admin LDAP Auth User Search Filter |   | 
| Admin LDAP Auth Group Search Base |   | 
| Admin LDAP Auth Group Search Filter |   | 
| Admin LDAP Auth Base DN  |   | 
| Admin LDAP Auth Bind User Password  |   | 
| Admin LDAP Auth Bind User Password  |   | 

2. Configure the following settings in Ranger. Click `Save Changes`.

    ![](../../assets/images/cdpbase/rangersetting1.png)