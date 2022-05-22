---
layout: default
title: Data Services Management Console
parent: Data Services
grand_parent: CDP Private Cloud
nav_order: 1
---

# Data Services Management Console
{: .no_toc }

This article explains the steps to deploy and configure the CDP Data Services environment after the successful installation of [ECS]({{ site.baseurl }}{% link docs/cdppvc/ecs.md %}) platform. This sets the stage for hosting CML, CDE and CDW services in the subsequent subtopics.

- TOC
{:toc}

---

# LDAP User Setting

1. In CM, navigate to `Data Services`. Click `Open CDP Private Cloud Data Services`. 

    ![](../../assets/images/dsconsole/cmds.png)
    
2. The system will redirect to the following page.  

    ![](../../assets/images/dsconsole/dsmenu.png)

3. Log in as the Local Administrator.

    ![](../../assets/images/dsconsole/dslogin.png)
    
4. Navigate to `Administration` > `Authentication`. The external LDAP server is the centralized user authentication database that stores the user credentials with the associated group. This demo is connected to Red Hat IPA. Fill up the necessary external LDAP server fields as shown in the following example. Click `Test Connection` and check that the connection is successful. Click `Save`.


    ![](../../assets/images/dsconsole/cdpauthentication.png)
    
5. Log out and log in using the ldap user credential.    

    ![](../../assets/images/dsconsole/cdpldaplogin.png)
    
6. The system prompts "You don't have the access rights".    
    
    ![](../../assets/images/dsconsole/cdpldapnorole.png)

7. Log out and log in as the Local Administrator. Navigate to `User Management`. Click `Update Roles` next to the ldap user. 

    ![](../../assets/images/dsconsole/cdpldapupdaterole.png)
    

8. Select the roles for this ldap user accordingly. Click `Update Roles`.

    ![](../../assets/images/dsconsole/cdpselectrole.png)
    

9. Log out and log in as the ldap user to get full access rights as the local administrator.


# CDP Data Lake Environment

1. Navigate to `Environments`. There is only one environment which is the default environment. Click `Register Environment`. Fill up the fields to create data lake environment for the CDP Data Services to use.
    
    ![](../../assets/images/dsconsole/dsregistration.png)
        
        
    ![](../../assets/images/dsconsole/dsenv.png)
    
    ![](../../assets/images/dsconsole/dsldapconfig.png)
    

---    
   Next Step
   {: .label .label-blue } 
   
- Proceed to create the CML service in this [subtopic]({{ site.baseurl }}{% link docs/cdppvc/cml.md %}).
- Proceed to create the CDW service in this [subtopic]({{ site.baseurl }}{% link docs/cdppvc/cdw.md %}).
- Proceed to create the CDE service in this [subtopic]({{ site.baseurl }}{% link docs/cdppvc/cde.md %}).
    

    
    

    
    
    ![](../../assets/images/dsconsole/dsroles.png)