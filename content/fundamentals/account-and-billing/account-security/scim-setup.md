---
pcx_content_type: how-to
title: Provision with SCIM
weight: 4
updated: 2023-12-03
---

# Provision Cloudflare with SCIM

By connecting a System for Cross-domain Identity Management (SCIM) provider, you can provision access to the Cloudflare dashboard on a per-user basis, mastered from your IDP.

Currently, we only provide SCIM support for Enterprise customers, and only from Microsoft Entra and Okta.
In order to get this set up, reach out to your account team, and ask for 

## Limitations

- If a user is the only Super Administrator on an Enterprise account, they will not be deprovisioned.
- We do not currently support Domain Scoped Roles provisioning via SCIM, only account-scoped roles. We are working on this limitation
- We do not currently allow custom group names, in order to leave space for future development

## Prerequisites

- Cloudflare provisioning with SCIM is only available to Enterprise customers and requires a Cloudflare-specific feature flag. Contact your account team for more information.
- In Cloudflare, [Super Administrator](/fundamentals/setup/manage-members/roles/) access on the account 
- In your IDP, the ability to create applications, as well as groups

---

## Create an API token

1. [Create an API token](/fundamentals/api/get-started/create-token/) with the following permissions:

   | Type    | Item             | Permission |
   | ------- | ---------------- | ---------- |
   | Account | Account Settings | Read       |
   | Account | Account Settings | Edit       |
   | User    | Memberships      | Read       |
   | User    | Memberships      | Edit       |


2. Under **Account Resources**, select the specific account to include or exclude from the dropdown menu.
3. Select **Continue to summary**.
4. Validate the permissions and select **Create Token**.
5. Copy the token value.

---

### Set up your Okta SCIM application.

1. In the Okta dashboard, go to **Applications** > **Applications**.
2. Select **Browse App Catalog**.
3. Locate and select **SCIM 2.0 Test App (OAuth Bearer Token)**.
4. Select **Add Integration** and name your integration.
5. Enable the following options:

   - **Do not display application icon to users**
   - **Do not display application icon in the Okta Mobile App**

6. Disable **Automatically log in when user lands on login page**.
7. Select **Next**, then select **Done**.

### Integrate the Cloudflare API.

1. In your integration page, go to **Provisioning** > **Configure API Integration**.
2. Enable **Enable API Integration**.
3. In SCIM 2.0 Base Url, enter: `https://api.cloudflare.com/client/v4/accounts/<your_account_ID>/scim/v2`.
4. In OAuth Bearer Token, enter your API token value.
5. Disable **Import Groups**.
6. Select **Save**.

### Set up your SCIM users.

   1. In **Provisioning to App**, select **Edit**.
   2. Enable **Create Users** and **Deactivate Users**. Select **Save**.
   3. In the integration page, go to **Assignments** > **Assign** > **Assign to Groups**.
   4. Assign users to your Cloudflare SCIM group.
   5. Select **Done**.
   6. This will provision all of the users affected to your Cloudflare account with "minimal account access"

### Configure user permissions on Okta

1. Select **Directory**, **Groups**, **Add group** and add groups with the following names:
   `CF-<your_account_ID> - <Role_Name>`

The list of available roles are the following:

Administrator
Administrator Read Only
Super Administrator (All Privileges)
Analytics
API Gateway
API Gateway Read
Audit Logs Viewer
Billing
Bot Management
Cache Purge
SSL/TLS, Caching, Performance, Page Rules, and Customization
DNS
Magic Network Monitoring Admin
Magic Network Monitoring
Magic Network Monitoring Read-Only
Cloudflare Gateway
HTTP Applications
HTTP Applications Read
Cloudflare Images
Load Balancer
Log Share
Network Services Write (Magic)
Network Services Read (Magic)
Minimal Account Access
Page Shield
PAge Shield Read
Hyperdrive Admin
Hyperdrive Readonly
Cloudflare R2 Admin
Cloudflare R2 Read 
Log Share Reader
Cloudflare Stream
Cloudflare Zero Trust
Cloudflare DEX
Cloudflare Zero Trust PII
Cloudflare Zero Trust Read Only
Cloudflare Zero Trust Reporting
Transform Rules
Trust and Safety
Turnstile
Turnstile Read
Vectorize Admin
Vectorize Readonly
Firewall
Waiting Room Admin
Waiting Room Read
Cloudflare Workers Admin
Zaraz Admin
Zaraz Edit
Zaraz Readonly
Zone Versioning (Account-Wide)
Zone Versioning Read (Account-Wide)
   


2. In the Application object, go to **Provisioning**. Select **Edit**.
3. Enable **Create Users** and **Deactivate Users**. Select **Save**.
5. Go to **Push Groups** and make sure the appropriate group matches the existing group of the same name on Cloudflare
6. Disable **Rename groups**. Select **Save**.
7. Within the **Push Groups** tab, select **Push Groups**.
8. Add the groups you created.
9. Select **Save**.

Adding any users to these groups will grant them the role. Removing the users from the identity provider will remove them from the associated role.

Refer to [Roles](/fundamentals/setup/manage-members/roles/) more details.

---

## Provision using Microsoft Entra

### Set up the Microsoft Entra Enterprise application.

   1. Go to your Microsoft Entra instance > Enterprise Applications.
   2. Select **Create your own application** and name your application.
   3. Select **Integrate any other application you don’t find in the gallery (Non-gallery)**.
   4. Select **Create**.

### Provision the Microsoft Entra Enterprise application.

   1. Under **Manage** on the sidebar menu, select **Provisioning**.
   2. Select **Automatic** on the dropdown menu for the Provisioning Mode.
   3. Enter your API token value and the tenant URL: `https://api.cloudflare.com/client/v4/accounts/<your_account_ID>/scim/v2`.
   4. Select **Test Connection**, then select **Save**.

### Configure user permissions in Microsoft Entra 

Currently, groups need to match a specific format to provision specific Cloudflare account-level roles. Cloudflare is in the process of adding Cloudflare Groups, which can take in freeform group names in the future.

These permissions work on an exact string match with the form

   `CF-<your_account_ID> - <Role_Name>`

The list of available roles are the following:

Administrator
Administrator Read Only
Super Administrator (All Privileges)
Analytics
API Gateway
API Gateway Read
Audit Logs Viewer
Billing
Bot Management
Cache Purge
SSL/TLS, Caching, Performance, Page Rules, and Customization
DNS
Magic Network Monitoring Admin
Magic Network Monitoring
Magic Network Monitoring Read-Only
Cloudflare Gateway
HTTP Applications
HTTP Applications Read
Cloudflare Images
Load Balancer
Log Share
Network Services Write (Magic)
Network Services Read (Magic)
Minimal Account Access
Page Shield
PAge Shield Read
Hyperdrive Admin
Hyperdrive Readonly
Cloudflare R2 Admin
Cloudflare R2 Read 
Log Share Reader
Cloudflare Stream
Cloudflare Zero Trust
Cloudflare DEX
Cloudflare Zero Trust PII
Cloudflare Zero Trust Read Only
Cloudflare Zero Trust Reporting
Transform Rules
Trust and Safety
Turnstile
Turnstile Read
Vectorize Admin
Vectorize Readonly
Firewall
Waiting Room Admin
Waiting Room Read
Cloudflare Workers Admin
Zaraz Admin
Zaraz Edit
Zaraz Readonly
Zone Versioning (Account-Wide)
Zone Versioning Read (Account-Wide)
   

Refer to [Roles](/fundamentals/setup/manage-members/roles/) more details.

1. To ensure that only required groups are provisioned, go to your Microsoft Entra instance.
2. Under Manage on the sidebar menu, select **Provisioning**.
3. Select **Provision Entra Groups** in Mappings.
4. Select **All records** under Source Object Scope.
5. Select **Add scoping filter** and create the appropriate filtering criteria to capture only the necessary groups.
6. Save the Attribute Mapping by selecting **OK** and return to the Enterprise Application Provisioning overview page.
7. Select **Start provisioning** to view the new users and groups populated on the Cloudflare dashboard. 
