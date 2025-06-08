---
pagination_next: null
pagination_prev: null
---

# Authorization

Access to resources in Cognite Data Fusion (CDF) is governed by role-based access control. Principals are members of groups ([IAM](index.md)), which confer capabilities. These capabilities are used to determine if an action should succeed or fail.

## Group membership

A principal is associated with, or determined to be a member of, one or more groups are managed in Azure AD:

1. Administrators manage users, groups, and users’ group membership in their Azure AD instance.
2. In Cognite Data Fusion, administrators have created a group with a sourceId matching the Azure AD GUID of each Azure AD group intended to give a user access to Cognite Data Fusion.
3. A user makes a request against Cognite Data Fusion, the user is authenticated against Azure AD, and then:

   - Azure AD is queried for which groups the user is currently member of.
   - The groups are resolved against groups in Cognite Data Fusion. Usually not all groups the user is member of will be in Cognite Data Fusion, only those relevant for Cognite Data Fusion access control.
   - The user will be recognized as a member of any Cognite Data Fusion group whose sourceId matches the a GUID of a group they're in Azure AD.

## Capabilities

Through capabilities, users and services are granted access to perform a particular operation on some data, for example, to read a time series or delete an asset. A resource type, actions, and scope define a capability. The resource type and scope define **what data** the capability is for, while the action defines **which operations** are allowed.

The capabilities of a user allow for zero or more operations in Cognite Data Fusion. A user with zero capabilities will only be able to call those API endpoints that don't require particular access.

1. **Resource Type** - Access is granted to a particular resource type, such as events, assets, or groups.

2. **Action** - Access is granted to perform a particular operation, such as READ, LIST, or WRITE. The meaning of the action is specific to the resource type.

3. **Scope** -Access is granted to a specific set of resources of the given type. The means of specifying the subset vary by resource type, but include:
   - All - All resources within the resource-type.
   - Scoping by asset subtree - Access is granted to resources associated with a particular subtree of the asset hierarchy. For example, the data points of a time series are only available when linked to an asset in the relevant asset subtree the user has access to.

### Security Categories

Some resource types allow requiring an additional access level above and beyond standard capabilities on the resource by resource basis. These resources are tagged with Security Categories. A user must have the capability of accessing the particular security category a resource is tagged with, AND have the correct resource type capability providing access to that resource otherwise, in order to access that resource. A user that doesn't have a specific security category in the set of capabilities won't be able to perform any operations on resources that are tagged with that specific security category.

### Example

**Data setup:**

1. Some files (such as file 44), assets, etc exist
2. Time series 123 is associated with asset 555 and is labeled with security category 36.
3. Time series 456 is associated with asset 555 and not labeled with a security category.

**Group setup:**

1. Group A: _capabilities_: [read on time series associated with asset subtrees 555 or 55,...]
2. Hypothetical Group A.2: _capabilities_: [write on time series 123,...]
3. Group B: _capabilities_: [member of security category 36,...]
4. Group C: _capabilities_: [read on time series 456,...]

**User setup:**

1. Johnny is a member of groups A and B only.
2. Bobby is a member of A only.
3. Carl is a member of group B only.
4. No one is a member of group C.

**Request and response consequences:**

1. Johnny tries to read time series 123, and succeeds.
    - Johnny has a capability designating him a member of security category 36 AND read on the time series (it's associated with asset 555, which he has read access to).
2. Johnny tries to read time series 456, and succeeds.
    - Johnny does have a capability with read access covering the time series
3. Johnny tries to read File 44, and fails.
    - Johnny has no capability providing access to files, whether associated with asset sub-trees 55, 555, or not; or whether the file has a security category or not.
4. Bobby is a member of group A, and can't read time series 123.
    - Bobby has read access that would normally cover the time series via group A, but it also carries a security category designation which he doesn't have. Denied!
5. Carl is a member of group B, and can't read time series 123.
    - Carl does carry the security category designation Bobby lacks in (III), but doesn’t have read access to the time   series normally. Denied!
    - If Carl was a member of group A.2 that offered write on that time series, he would be able to write to it, without being able to read from it, without it affecting the permissions of any of the other users heretofore described.
