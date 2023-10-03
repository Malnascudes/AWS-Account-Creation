# A guide to AWS accounts creation

- [Create a standalone AWS account](#create-a-standalone-aws-account)
  - [Payment](#payment)
  - [Verify Identity](#verify-identity)
  - [Support Plan](#support-plan)
- [Access AWS Account with root user](#access-aws-account-with-root-user)
- [Additional Accounts](#additional-accounts)
  - [Development Account](#development-account)
  - [Staging Account](#staging-account)
- [Enabling IAM Identity Center](#enabling-iam-identity-center)
- [Administrator Users](#administrator-users)
  - [Create an Administrative Permission Set](#create-an-administrative-permission-set)
  - [Create Administrator User Group](#create-administrator-user-group)
  - [Create Administrator User](#create-administrator-user)
  - [Give Administraive permissions to created User](#give-administraive-permissions-to-created-user)
- [AWS access portal URL](#aws-access-portal-url)
- [Crypsis Delizziosa User](#crypsis-delizziosa-user)
  - [Create a System Administrator Permission Set](#create-a-system-administrator-permission-set)
  - [System Administrator User Group](#system-administrator-user-group)
  - [System Administrator User](#system-administrator-user)
  - [Give System Administor permissions to Crypsis Users](#give-system-administor-permissions-to-crypsis-users)
- [Cost Managing](#cost-managing)
  - [Set up Cost Explorer](#set-up-cost-explorer)
  - [Creating a Budget](#creating-a-budget)


## Create a standalone AWS account
[Reference](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html)

* Open the [Amazon Web Services home page](https://aws.amazon.com/).

* Choose *"Create an AWS account"*.

<img src="assets/account_creation_1_export.png" alt="description" width="500" style="display: block; margin: auto;">


* Enter your account information, and then choose Verify email address. This will send a verification code to your specified email address.

* Enter the verification code, and then choose **Verify**.

* Choose **Business** or **Personal**. Personal accounts and business accounts have the same features and functions.

* Enter your company or personal information.

### Payment

* Enter the information about payment method, and then choose Verify and Continue.

### Verify Identity

* To Verify the Identity add the phone number so a SMS can be sent with a verification code.
* Enter the verification code and press Continue

### Support Plan
* If desired select a support plan to have access to AWS support (not needed)

* Complete Sign Up

## Access AWS Account with root user

<img src="assets/login_1_export.png" alt="description" width="400" style="display: block; margin: auto;">

* Click on "Go to AWS Console" or go to [this link](https://console.aws.amazon.com/console/home)

* **Enter root user email adress**

* **Enter passowrd for root user**

<img src="assets/aws_homescreen_export.png" alt="description" width="600" style="display: block; margin: auto;">

## Additional Accounts
In order to **isolate the development, staging and production environments** we will be creating a set of **additional accounts to totally separate the resources of each environment**. This will be done by creating **2 new accounts under the same email as the root user**, a development and a staging one, that will be accessed by the administrator users and the system administrator users.

To create this accounts we will be using the **same email as the root user** but with small changes. We will be using a new tab of the AWS console, the **"*Organization*"** tab. To open it click on the user name to unfold a menu and select `Organization`

<img src="assets/iam_identity_center_24_export.png" alt="description" width="400" style="display: block; margin: auto;">

### Development Account
* To create the development account click on ***"Add an AWS account"*** and fill in the following information:
  * AWS account name: `Development`
  * Email address of the account's owner: Here we will use the **same email as the root user but add `+dev` right before the `@`**. For example if the root user email was `root-user-email@domain.com` we will now use `root-user-email+dev@domain.com`. This is an AWS feature that lets us use the same email for different accounts to have the resources separated as we want now.

<img src="assets/iam_identity_center_25_export.png" alt="description" width="600" style="display: block; margin: auto;">
  
  * Leave IAM role name as it is and click on **"*Create AWS Account*"**

<img src="assets/iam_identity_center_26_export.png" alt="description" width="600" style="display: block; margin: auto;">

### Staging Account
* To create the staging account click on ***"Add an AWS account"*** and fill in the following information:
  * AWS account name: `Staging`
  * Email address of the account's owner: Here we will use the **same email as the root user but add `+stage` right before the `@`**. For example if the root user email was `root-user-email@domain.com` we will now use `root-user-email+stage@domain.com`. This is an AWS feature that lets us use the same email for different accounts to have the resources separated as we want now.

<img src="assets/iam_identity_center_27_export.png" alt="description" width="600" style="display: block; margin: auto;">
  
  * Leave IAM role name as it is and click on **"*Create AWS Account*"**

<img src="assets/iam_identity_center_28_export.png" alt="description" width="600" style="display: block; margin: auto;">


## Enabling IAM Identity Center

[Reference](https://docs.aws.amazon.com/streams/latest/dev/setting-up.html)

Instead of working with the root user **we will create different sets of users with different premissions and use the root user only when needed**. [This are the actions that require root access](https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-tasks.html). We will create 2 users: an Administrator one used by you to manage the account, and a `crypsis-delizziosa` one used by us to manage the infrastructure.

The tool for managing of this users is the **"IAM Identity Center"**. To use it we will first have to enable it.

[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-enable-identity-center.html)

**Search for the "*IAM Identity Center*" in the resource finder.**

<img src="assets/iam_identity_center_0_export.png" alt="description" width="600" style="display: block; margin: auto;">

Proceed and click on "*Enable*" and "*Create AWS organization*"

<img src="assets/iam_identity_center_1_export.png" alt="description" height="200">
<img src="assets/iam_identity_center_2_export.png" alt="description" height="200">
<img src="assets/iam_identity_center_3_export.png" alt="description" width="600">


## Administrator Users

We will now create and **Aministrator user** that will be used by you to **manage all the accounts without using the root user.**
To do so we will start by creating an Admistrative Permission set, then an Admisitrator Users group and create a user in that group, finaly we will give the Administrative Permission set to the Administrator Users group so all users in that group have the administrative permissions.

### Create an Administrative Permission Set

[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html)
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-create-an-administrative-permission-set.html)

In order to give users certain permisions we will be using "Permission sets". Permission sets are stored in IAM Identity Center and define the level of access that users and groups have to an AWS account. Perform the following steps to create a permission set that grants administrative permissions.

* In the "IAM Identity Center" console, under **"*Multi-account permissions*"** choose **"*Permission sets*"**

<img src="assets/iam_identity_center_4_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Click **"*Create permission set*"**

* In the "*Permission set type*" leave it as **"*Predefined permission set*"** and **select `AdministratorAccess`** and click *"Next"*

<img src="assets/iam_identity_center_19_export.png" alt="description" width="400" style="display: block; margin: auto;">

* In the "*Specify permission set details*" page, keep the default settings and choose "*Next*"

* Review and Create permission set

<img src="assets/iam_identity_center_5_export.png" alt="description" width="600" style="display: block; margin: auto;">

### Create Administrator User Group
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/addgroups.html)

User Groups allow to manage permissions on several users. We will now create a user group for administor users.

* Select **"*Groups*"** from the left menu of the *"IAM Identity Center console"*

<img src="assets/iam_identity_center_6_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Set **"*Group name*" to `Administrators`**

* Add description if desired

* Click *"Create Group"*

<img src="assets/iam_identity_center_7_export.png" alt="description" width="600" style="display: block; margin: auto;">

### Create Administrator User
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/addusers.html)

To create an administrator user and add it to the created group follow this steps:

* Select **"*Users*"** from the left menu of the *"IAM Identity Center console"* and *"Add User"* 

<img src="assets/iam_identity_center_8_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Fill in the following information:
  * Username: `Administrator`
  * email adress: Input the **email adress of the administrator user (one of your choice)**, this email will be **used by you** to access the AWS account as an administrator
  * First name: Input the Administrator First Name
  * Last name: Input the Administrator First Name
  * Display name: Choose a display name for the Administrator User

<img src="assets/iam_identity_center_18_export.png" alt="description" width="300" style="display: block; margin: auto;">

* Leave the optional fields unset and click on "*Next*"
* Add the user to previously created `Administrators` group

<img src="assets/iam_identity_center_32_export.png" alt="description" width="300" style="display: block; margin: auto;">

* Review and click *"Add user"*
  
<img src="assets/iam_identity_center_10_export.png" alt="description" width="600" style="display: block; margin: auto;">

This will send a verification link to the email adress to verify the account and set a password.

### Give Administraive permissions to created User
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-assign-account-access-admin-user.html)

We will now give the `AdministratorAccess` permission set to all the users in the `Administrators` group so they can access all the created accounts (main, development and staging) with administrative permissions.

* Under "*Multi-account permissions*" choose "*AWS accounts*"

* Select all the accounts (the root user created in [first section](#create-a-standalone-aws-account) and the `Development` and `Staging` ones created in the [Additional Accounts section](#additional-accounts)) and click on "*Assign users or groups*"

<img src="assets/iam_identity_center_13_export.png" alt="description" width="400" style="display: block; margin: auto;">

* Select the `Administrators` group previously created and click on *"Next"*

<img src="assets/iam_identity_center_14_export.png" alt="description" width="400" style="display: block; margin: auto;">

* On *"Permission sets"* select the `AdministratorAccess` created.

<img src="assets/iam_identity_center_15_export.png" alt="description" width="400" style="display: block; margin: auto;">

* Review and click on *"Submit"*

## AWS access portal URL
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-sign-in-access-portal.html)

In order **to access the account without using the root user we need an "*AWS access portal URL*"**. Using this link all the users can loggin using the User name and their chosen password.

To get the "*AWS access portal URL*" first go to the *"IAM Identity Center console"*.

<img src="assets/iam_identity_center_0_export.png" alt="description" width="600" style="display: block; margin: auto;">

In the left menu of *"IAM Identity Center console"* go to **Settings** and copy the `AWS access portal URL` from the *"Identity source"* section

<img src="assets/iam_identity_center_11_export.png" alt="description" width="600" style="display: block; margin: auto;">

This link is the one that will be used for the users to access the account by entering the user name and the password set by the user in the verification email.

This is what it looks like when you open the link:

<img src="assets/iam_identity_center_29_export.png" alt="description" width="250">
<img src="assets/iam_identity_center_30_export.png" alt="description" height="250">

## Crypsis Delizziosa User

For security reasons we will create a **user without administrator permissions that is able to manage servers and databases**. This kind of users are also known as System Administrator users.

This users will have access to the Main Account, the Development and Staging one just like the Administrator User but with administrative permissions.
To create them we will follow similar steps as [Administrator Users section](#administrator-users)

### Create a System Administrator Permission Set
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-create-permission-set-to-grant-least-privilege-permissions.html)

To create System Administrator for the new users will follow the same steps as in the [Create an administrative permission set](#create-an-administrative-permission-set) section but select `SystemAdministrator` as the "Policy for predefined permission set"
[Here you can find a detailed description of the permissions given to the permission sets](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)

* Go to the *"IAM Identity Center console"*.

<img src="assets/iam_identity_center_0_export.png" alt="description" width="600" style="display: block; margin: auto;">

* Under "*Multi-account permissions*" choose "*Permission sets*"

<img src="assets/iam_identity_center_4_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Click "*Create permission set*"

* In the "*Permission set type*" leave it as "*Predefined permission set*", **select `SystemAdministrator` and click *"Next"***

<img src="assets/iam_identity_center_12_export.png" alt="description" width="400" style="display: block; margin: auto;">

* In the "*Specify permission set details*" page, keep the default settings and choose "*Next*"

* Review and Create permission set

<img src="assets/iam_identity_center_16_export.png" alt="description" width="600" style="display: block; margin: auto;">

### System Administrator User Group
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/addgroups.html)

We will now create a User group to manage all the System Administrator users. To do so we will follow the same steps as in the [Create Administrator User Group section](#create-administrator-user-group)

* Select **"*Groups*"** from the left menu of the *"IAM Identity Center console"*

<img src="assets/iam_identity_center_6_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Set **"*Group name*" to `System Administrators`**

* Add description if desired

* Click *"Create Group"*

<img src="assets/iam_identity_center_17_export.png" alt="description" width="600" style="display: block; margin: auto;">


### System Administrator User

[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/addusers.html)

Now we will create the user that will be used by us to manage the infrastructure on the main, development and staging account, this user will be in the `System Administrators`. To create an system administrator user and add it to the created group follow this steps:

* Select **"*Users*"** from the left menu of the *"IAM Identity Center console"* and *"Add User"* 

<img src="assets/iam_identity_center_8_export.png" alt="description" width="125" style="display: block; margin: auto;">

* Fill in the following information
  * Username: `crypsis-delizziosa`
  * **email adress: `crypsisdelizziosa@gmail.com`**
  * First name: Crypsis
  * Last name: Delizziosa
  * Display name: Crypsis Delizziosa

<img src="assets/iam_identity_center_9_export.png" alt="description" width="300" style="display: block; margin: auto;">

* Leave the optional fields unset and click on "*Next*"
* Add the user to previously created `System Administrators` group

<img src="assets/iam_identity_center_31_export.png" alt="description" width="300" style="display: block; margin: auto;">

* Review and click *"Add user"*
  
<img src="assets/iam_identity_center_33_export.png" alt="description" width="600" style="display: block; margin: auto;">

This will send a verification link to the email adress to verify the account and set a password.


### Give System Administor permissions to Crypsis Users
[Reference](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-started-assign-account-access-admin-user.html)

We will now give the `SystemAdministrator` permission set to all the users in the `System Administrators` group so they can access all the created accounts (main, development and staging) with system administrative permissions.

* Under "*Multi-account permissions*" choose "*AWS accounts*"

* Select all the accounts (the root user created in [first section](#create-a-standalone-aws-account) and the `Development` and `Staging` ones created in the [Additional Accounts section](#additional-accounts)) and click on "*Assign users or groups*"

<img src="assets/iam_identity_center_13_export.png" alt="description" width="400" style="display: block; margin: auto;">

* Select the `System Administrators` group previously created and click on *"Next"*

<img src="assets/iam_identity_center_34_export.png" alt="description" width="400" style="display: block; margin: auto;">

* On *"Permission sets"* select the `SystemAdministrator` created.

<img src="assets/iam_identity_center_35_export.png" alt="description" width="400" style="display: block; margin: auto;">

* Review and click on *"Submit"*


## Cost Managing
[Reference](https://saturncloud.io/blog/setting-amazon-aws-billing-limits-what-you-should-know/)
[Reference](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

Currently, AWS does not provide a built-in feature to set hard billing limits, however AWS provides several tools and strategies for managing your expenditure and avoiding the dreaded bill shock.

We will first set up a [Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) to visualize, understand, and manage the AWS infrastructure costs and usage over time. Then we will create a [Budget](https://aws.amazon.com/aws-cost-management/aws-budgets/) to set up alerts when we exceed certain thresholds.

**To perform this actions we need to log with the root user created in** [the first section](#create-a-standalone-aws-account)

### Set up Cost Explorer
[Reference](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-enable.html)

### Creating a Budget
We will now create a Budget that notifies us if we exceed, or are forecasted to exceed, the budget amount.

* With the root user, click on the user name to unfold a menu and select `Billing Dashboard`

<img src="assets/billing_1_export.png" alt="description" width="300" style="display: block; margin: auto;">

* Under *"Cost Management"* on the left menu select *"Budget"* Monthly cost budgets and then *"Create a budget"*

* Use a "User Template" and select ***"Monthly cost budget"***

<img src="assets/billing_2_export.png" alt="description" width="400" style="display: block; margin: auto;">

* **Set the budget name, monthly budget and email adresses to send the alert to.** And click on *"Create budget"*

This has created a budget plan that will notify us when 
* The spend reaches 85% of the plan
* The actual spend reaches 100%
* The forecasted spend is expected to reach 100%.

For more information about budgets visit [the official user guide](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)
