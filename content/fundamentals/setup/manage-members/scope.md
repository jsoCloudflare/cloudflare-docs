---
pcx_content_type: reference
title: Role scopes
weight: 4
---

# Role scopes

At Cloudflare, we are extending our role model to be significantly more flexible with scopes. As of today, users can apply roles to a whole account, or specific domains within that account, but we are looking to expand these into other resources in the future.

A scope is one of 3 pieces of information needed to create a membership (a User, a role, and a scope)

When you assign domain specific roles to account members, they grant specific access to a given set of zones.

In order to assign your users a set of appropriate permissions, multiple policies can be assigned in order to mix and match appropriate account and domain level access.

E.g. A user should be able to read everything across an account, but only have the permission to purge the cache for 1 production zone, and the ability to fully administer a staging zone.
3 Policies can be assigned to this user:
1) An account-wide "Administrator Read Only" role
2) A Domain Scoped role to the production zone with "Cache Domain Purge"
3) A Domain Scoped role to the staging zone with "Domain Administrator"


---

## Choose role scopes

We have different sets of roles for each type of scope.
You choose the scope of a role when you [add a member](/fundamentals/setup/manage-members/manage/) to your account.

{{<Aside type="note">}}
You need to choose to assign either an Account Scoped role or Domain Scoped role in a given policy. You cannot combine both types of roles in the same policy, but can assign multiple policies to the same member as a separate step. 
{{</Aside>}}
### All domains

If you want the member to have a role that applies across your account, use the following combination of fields.

| Field | Value |
| --- | --- |
| Operator | *Include* |
| Type | *All domains* |

### Individual Domains

If you want the member to have a role that applies only to a given domain within your account, use the following combination of fields.

| Field | Value |
| --- | --- |
| Operator | *Include* |
| Type | *A specific domain* |
| Name | The domain you want included |


### Domain groups

If you want a member to have access to a group of predefined domains, you can also create a **Domain Group**.

#### Create group

To create a domain group: 

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/login) and select your account (you must be logged in as a **Super Administrator** and have a [verified email address](/fundamentals/setup/account/verify-email-address/)).
2. Go to **Manage Account** > **Configurations** > **Lists**.
3. For **Domain Group Manager**, select **Create**.
4. Create your domain group:

    1. Select the domains to include.
    2. Add a **Name**.
    3. Select **Create**.

You can also edit and delete these groups as needed.

#### Use group

To assign a member permissions to a domain group, use the following combination of fields:

| Field | Value |
| --- | --- |
| Operator | *Include* |
| Type | *Domain Group* |
| Name | *Example Group* |

{{<Aside type="note">}}

With Domain Groups, you can only assign [domain-scoped roles](/fundamentals/setup/manage-members/roles/#domain-scoped-roles) to these members.

{{</Aside>}}
