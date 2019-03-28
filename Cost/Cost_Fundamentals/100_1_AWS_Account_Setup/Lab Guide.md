# Level 100: AWS Account Setup: Lab Guide

## Authors
- Nathan Besh, Cost Lead Well-Architected
- Spencer Marley, Commercial Architect

## Feedback
If you wish to provide feedback on this lab, there is an error, or you want to make a suggestion, please email: costoptimization@amazon.com


# Table of Contents
1. [Configure IAM access](#IAM_access)
2. [Create an account structure](#account_structure)
3. [Configure account settings](#account_settings)
4. [Configure Cost and Usage reports](#CUR)
5. [Enable AWS Cost Explorer](#cost_explorer)
6. [Enable AWS-Generated Cost Allocation Tags](#cost_tags)
7. [Tear down](#tear_down)
8. [Feedback survey](#survey)


## 1. Configure IAM access to your billing<a name="IAM_access"></a>
**NOTE**: You will need to sign into the account with root account credentials to perform this action. You need to enter in the account email and password for root access.

You need to enable IAM access to your billing so the correct IAM users can access the information. This allows other users (non-root) to access billing information in the master account. It is also required if you wish for member accounts to see their usage and billing information. This step will not provide access to the information, that is configured through IAM policies.


1. Log in to your Master account as the root user, Click on the account name in the top right, and click on **My Account** from the menu:
![Images/AWSAcct4.png](Images/AWSAcct4.png)

2. Scroll down to **IAM User and Role Access to Billing Information**, and click **Edit**:
![Images/AWSAcct5.png](Images/AWSAcct5.png)

3. Select **Activeate IAM Access** and click on **Update**:
![Images/AWSAcct6.png](Images/AWSAcct6.png)

4. Confirm that **IAM user/role access to billing information is activated**:
![Images/AWSAcct7.png](Images/AWSAcct7.png) 

You will now be able to provide access to non-root users to billing information via IAM policies.

**NOTE:** Logout as the root user before continuing.


## 2. Create an account structure<a name="account_structure"></a>
**NOTE**: Do NOT do this step if you already have an orgnization and consolidated billing setup.

You will create an AWS Organization, and join one or more accounts to te master account. An organization will allow you to centrally manage multilpe AWS accounts efficiently and consistently. It is recommended to have a master account that is primarily used for billing and does not contain any resources, all resources and workloads will reside in the member accounts. You will need organizations:CreateOrganization access, and 2 or more AWS accounts. When you create a new master account, it will contain all billing information for member accounts, member accounts will no longer have any billing information, including historical billing information.  Ensure you backup or export any reports or data.  

### 2.1 Create an AWS Organization
You will create an AWS Organization with the master account. 

1. Login to the AWS console as an IAM user with the required permissions, start typing *AWS Organizations* into the **Find Services** box and click on **AWS Organizations**:
![Images/AWSOrg1.png](Images/AWSOrg1.png)

2. Click on **Create organization**:
![Images/AWSOrg2.png](Images/AWSOrg2.png)

3. To create a fully featured organization, Click on **Create organization**
![Images/AWSOrg3.png](Images/AWSOrg3.png)

4. You will receive a verification email, click on **Verify your email address** to verify your account:
![Images/AWSOrg4.png](Images/AWSOrg4.png)

5. You will then see a verification message in the console for your organization:
![Images/AWSOrg5.png](Images/AWSOrg5.png)

You now have an organization that you can join other accounts to.

### 2.2 Join member accounts
You will now join other accounts to your organization.

1. From the AWS Organizations console click on **Add account**:
![Images/AWSOrg6.png](Images/AWSOrg6.png)

2. Click on **Invite account**:
![Images/AWSOrg7.png](Images/AWSOrg7.png)

3. Enter in the **Email or account ID**, enter in any relevant **Notes** and click **Invite**:
![Images/AWSOrg8.png](Images/AWSOrg8.png)

4. You will then have an open request:
![Images/AWSOrg9.png](Images/AWSOrg9.png)

5. Log in to your **member account**, and go to **AWS Organizations**:
![Images/AWSOrg1.png](Images/AWSOrg1.png)

6. You will see an invitation in the menu, click on **Invitations**:
![Images/AWSOrg10.png](Images/AWSOrg10.png)

7. Verify the details in the request (they are blacked out here), and click on **Accept**: 
![Images/AWSOrg11.png](Images/AWSOrg11.png)

8. Verify the Organization ID (blacked out here), and click **Confirm**:
![Images/AWSOrg12.png](Images/AWSOrg12.png)

9. You are shown that the account is now part of your organization:
![Images/AWSOrg13.png](Images/AWSOrg13.png)

10. The member account will receive an email showing success:
![Images/AWSOrg14.png](Images/AWSOrg14.png)

11. The master account will also receive email notification of success:
![Images/AWSOrg15.png](Images/AWSOrg15.png) 

Repeat the steps above (exercise 1.2) for each additional account in your organization. 


## 3. Configure billing account settings<a name="account_settings"></a>
It is important to ensure your account contacts are up to date and correct. This allows AWS to be able to contact the correct people in your organization if required. It is recommended to use a mailing list or shared email that is accessible by muptile team members for redundancy. Ensure the email accounts are actively monitored.

1. Log in to your Master account as an IAM user with the required permissions, Click on the account name in the top right, and click on **My Account** from the menu:
![Images/AWSAcct11.png](Images/AWSAcct1.png)

2. Scroll down to **Alternate Contacts** and click on **Edit**:
![Images/AWSAcct2.png](Images/AWSAcct2.png)

3. Enter information into each of the fields for **Billing**, **Operations** and **Security**, and click **Update**:
![Images/AWSAcct3.png](Images/AWSAcct3.png)


## 4. Configure Cost and Usage Reports<a name="CUR"></a>
Cost and Usage Reports provide the most detailed information on your usage and bills. They can be configured to deliver 1 line per resource, for every hour of the day. They must be configured to enable you to access and analyze your usage and billing information. This will allow you to make modifications to your usage, and make your applications more efficient.


### 4.1 Configure a Cost and Usage Report
If you configure multilpe Cost and Usage Reports (CURs), then it is recommended to have 1 CUR per bucket. If you **must** have multilpe CURs in a single bucket, ensure you use a different **report path prefix** so it is clear they are different reports.

1. Log in to your Master account as an IAM user with the required permissions, and go to the **Billing** console:
![Images/AWSCUR1.png](Images/AWSCUR1.png) 

2. Select **Cost & Usage Reports** from the left menu:
![Images/AWSCUR2.png](Images/AWSCUR2.png) 

3. Click on **Create report**:
![Images/AWSCUR3.png](Images/AWSCUR3.png)

4. Enter a **Report name** (it can be any name), ensure you have selected **Include resource IDs** and **Data refresh settings**, then click on **Next**:
![Images/AWSCUR4.png](Images/AWSCUR4.png)

5. Click on **Configure**:
![Images/AWSCURDelivery0.png](Images/AWSCURDelivery0.png)

6. Enter a unique bucket name, and ensure the region is correct, click **Next**:
![Images/AWSCURDelivery1.png](Images/AWSCURDelivery1.png)

7. Read and verify the policy, this will allow AWS to deliver billing reports to the bucket. Click on **I have confirmed that this policy is correct**, then click **Save**:
![Images/AWSCURDelivery3.png](Images/AWSCURDelivery3.png)

8. Esure your bucket is a **Valid Bucket** (if not, verify the bucket policy). Enter a **Report path prefix** (it can be any word) without any '/' characters, ensure the **Time Granularity** is **Hourly**, **Report Versioning** is set to **Overwrite existing report**, under **Enable report data integration for** select **Amazon Athena**, and click **Next**:
![Images/AWSCURDelivery4.png](Images/AWSCURDelivery4.png)

9. Review the configuration, scroll to the bottom and click on **Review and Complete**:
![Images/AWSCUR6.png](Images/AWSCUR6.png)

You have successfully configured a Cost and Usage Report to be delivered.  It may take up to 24hrs for the first report to be delivered.

### 4.2 Enable monthly billing report
The monthly billing report contains estimated AWS charges for the month. It contains line items for each unique combination of AWS product, usage type, and operation that the account uses.

1. Go to the billng console:
![Images/AWSMonthlyUsage0.png](Images/AWSMonthlyUsage0.png)

2. Click on **Billing preferences** from the left menu:
![Images/AWSMonthlyUsage1.png](Images/AWSMonthlyUsage1.png)

3. Scroll down, and click on **Receive Billing Reports**, then click on **Configure**:
![Images/AWSMonthlyUsage2.png](Images/AWSMonthlyUsage2.png)

4. From the left dropdown, select your S3 billing bucket configured above:
![Images/AWSMonthlyUsage3.png](Images/AWSMonthlyUsage3.png)

5. Click on **Next**:
![Images/AWSMonthlyUsage4.png](Images/AWSMonthlyUsage4.png)

6. Read and verify the policy, this will allow AWS to deliver billing reports to the bucket. Click on **I have confirmed that this policy is correct**, then click **Save**:
![Images/AWSMonthlyUsage5.png](Images/AWSMonthlyUsage5.png)

7. Ensure only **Monthly report** is selected, and uncheck all other boxes.  Click on **Save preferences**:
![Images/AWSMonthlyUsage6.png](Images/AWSMonthlyUsage6.png)



## 5. Enable AWS Cost Explorer<a name="cost_explorer"></a>
AWS Cost Explorer has an easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time. You must enable it before you can use it within your accounts.
 
1. Log in to your Master account as an IAM user with the required permissions, and go to the **Billing** console:
![Images/AWSExplorer0.png](Images/AWSExplorer0.png)

2. Select **Cost Explorer** from the left menu:
![Images/AWSExplorer1.png](Images/AWSExplorer1.png)

3. Click on **Enable Cost Explorer**:
![Images/AWSExplorer2.png](Images/AWSExplorer2.png)

4. You will receive notification that Cost Explorer has been enabled, and data will be populated:
![Images/AWSExplorer3.png](Images/AWSExplorer3.png)


## 6. Enable AWS-Generated Cost Allocation Tags<a name="cost_tags"></a>
Enabling AWS-Generated Cost Allocation Tags, generates a cost allocation tag containing resource creator information that is automatically applied to resources that are created within your account. This allows you to view and allocate costs based on who created a resource. 

1. Log in to your Master account as an IAM user with the required permissions, and go to the **Billing** console:
![Images/AWSBillTag0.png](Images/AWSBillTag0.png)

2. Select **Cost Allocation Tags** from the left menu:
![Images/AWSBillTag1.png](Images/AWSBillTag1.png)

3. Click on **Activate** to enable the tags:
![Images/AWSBillTag2.png](Images/AWSBillTag2.png)

4. You will see that it is activated: 
![Images/AWSBillTag3.png](Images/AWSBillTag3.png)



## 7. Tear down<a name="tear_down"></a>  
This exercise covered fundamental steps that are recommended for all AWS accounts to enable Cost Optimization. There is no tear down for exercises in this lab.
Ensure you remove the IAM policies from the users/groups if they were used. 

## 8. Survey <a name="survey"></a>
Thanks for taking the lab, We hope that you can take this short survey (<2 minutes), to share your insights and help us improve our content.

[![Survey](Images/survey.png)](https://amazonmr.au1.qualtrics.com/jfe/form/SV_cvavNi7IbbzCyfX)


This survey is hosted by an external company (Qualtrics) , so the link above does not lead to our website.  Please note that AWS will own the data gathered via this survey and will not share the information/results collected with survey respondents.  Your responses to this survey will be subject to Amazons Privacy Policy.
 
