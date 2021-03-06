---
layout: layout.pug
title: 'Tutorial &#8211; Restricting Access to DC/OS Service Groups'
menuWeight: 6
excerpt: >
  Demonstrates how to use the DC/OS web
  interface to achieve multi-tenancy in
  permissive mode.
preview: true
enterprise: true
---
This tutorial demonstrates how to implement user permissions for DC/OS services in the permissive [security mode](/1.8/administration/installing/ent/custom/configuration-parameters/#security). When you are done you will have multi-tenancy by using DC/OS permissions.  

**Prerequisites:**

- DC/OS Enterprise is [installed](/1.8/administration/installing/ent/) in permissive [mode](/1.8/administration/installing/ent/custom/configuration-parameters/#security).

## Create users and groups

1.  Create user groups from the **Services > Create Group** tab of the DC/OS web interface.

    ![Services Create Group](../img/service-group1.png)
    
    In this example a group called `prod-a` and a group called `prod-b` are created. After the groups are created you should see two folders. This is where you will deploy services for the respective groups and set the permissions for each unit.
    
    ![Group folders](../img/service-group2.png)
    
1.  Create your users and groups and define the required permissions for each group. 

    1.  Select the **System** tab and then select **Organization > Users**. 
     
    1.  Select **New User**.  In this example, two users are created (`Cory` and `Nick`).     
        
        ![Create user Cory](../img/service-group3.png)
        
        When you're done you should see the two users. 
        
        ![All users](../img/service-group4.png)
        
        Next we will create the groups and assign permissions to the DC/OS services.
    
    1.  Select the **System** tab and then select **Organization > Groups**.
    
    1.  Select **New Group**. In this example, two groups are created:
        
        - `prod-a group` for managing the DC/OS services for user Cory.
        - `prod-b group` for managing the DC/OS services for user Nick.
        
        ![prod-a group](../img/service-group5.png)
        
## Define the permissions
    
1.  Select the **System** tab and then select **Organization > Groups**. 

1.  Select the prod-a group and select **Add Permission**.  In this example, permissions are assigned to prod-a to allow users to create their own services!  

1.  Select the **Insert Permission String** toggle to enter using the string format. Strings are case sensitive.
    
    ![prod-a group](../img/service-group6.png)
    
    All of the required permissions for each group are added here. These permission will allow users to have access to the DC/OS cluster and deploy their own services from the Universe.  These permission will also restrict each group so that they can only see their own DC/OS services.
      
1.  Add each of these permissions for the prod-a group.   
    
    -  `dcos:adminrouter:service:marathon` = `full`
    -  `dcos:service:marathon:marathon:services:/prod-a` = `full`  
    -  `dcos:adminrouter:ops:slave` = `full`
    -  `dcos:adminrouter:ops:mesos` = `full`
    -  `dcos:adminrouter:package` = `full`
    
    ![prod-a group](../img/service-group7.png)
    
    Here's what the permissions view should look like after adding:
    
    ![prod-a group](../img/service-group8.png)
    
1.  Add each of these permissions for the prod-b group.   
            
    -  `dcos:adminrouter:service:marathon` = `full`
    -  `dcos:service:marathon:marathon:services:/prod-b` = `full`  
    -  `dcos:adminrouter:ops:slave` = `full`
    -  `dcos:adminrouter:ops:mesos` = `full`
    -  `dcos:adminrouter:package` = `full`
    
    Now that the permissions are assigned, you can assign them to users!   
    
1.  Select the **System** tab and then select **Organization > Users** and select **Cory**.  

1.  Select **Group Membership** then place your cursor in the search box and select `prod-agroup`. 

    ![prod-a group](../img/service-group9.png)
    
1.  Select the **System** tab and then select **Organization > Users** and select **Nick**.  
    
1.  Select **Group Membership** then place your cursor in the search box and select `prod-bgroup`. 
    
    
## Log in to the DC/OS web interface as user

1.  Log in as Cory to the DC/OS web interface. You can see that user Cory only has access to the **Services** and **Universe** tabs. Also, Cory can only see the **prod-a** services.
 
    **Tip:** To log out of as the current user, choose **Sign Out** from the lower-left dropup menu.
    
    ![prod-a group](../img/service-group10.png)
    
    Let’s push an Nginx service from the Universe into prod-a group.  
    
1.  Select **Universe** and start typing `nginx` in the search box, then click to select.

    1.  Choose the **Advanced Installation** option. 
        
        ![prod-a group](../img/service-group11.png)
        
    1.  In the **name** field, specify `/prod-a/nginx` and then click **Review and Install** and **Install**.
    
        ![prod-a group](../img/service-group12.png)
        
        Now let’s go back to the **Services** tab and you’ll see that Cory was able to deploy Nginx into the `prod-a` group.
        
        ![prod-a group](../img/service-group13.png)
        
1.  Repeat the previous steps for Nick. Be sure to specify `/prod-b/nginx` for **name**. 

1.  Repeat the previous steps for Nick. Be sure to specify `/prod-b/nginx` for **name**. 

1.  While logged in as Cory or Nick, click on the nginx launch icon to view the success message.
    
    ![NGINX](../img/service-group-nginx.png)
    
    
Now let’s look at the **Services** tab from the superuser view.  


## Monitor user accounts in the DC/OS web interface as superuser

1.  Log out of the current user and then back in as a user with [superuser](/1.8/administration/id-and-access-mgt/ent/permissions/superuser-perm/) permission. You will see that both services are running in the prod-a and prod-b groups.  

    ![prod-a group](../img/service-group14.png)




        


        
        


