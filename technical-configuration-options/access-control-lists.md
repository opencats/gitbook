# Access control lists

OpenCATS contains feature that allows to define which access level has role (user) for an object. User is created with for example access level `ACCESS_LEVEL_READ`. Without definition of access level map, user has access only to objects (functionalities) where is required access level ACCESS\_LEVEL\_READ; when higher access level is required (for example ACCESS\_LEVEL\_EDIT), user is not allowed to access such object / functionality.

Without definition of `ACCESS_LEVEL_MAP`, it works as before, that is based on access level defined for user. When configuration userCategory -> securedObject -> accessLevel is defined, then accessLevel is calculated based on category (role) that user has assigned.

If we need user to have `ACCESS_LEVEL_EDIT` for example for candidates, without ACL, we have to define `ACCESS_LEVEL_EDIT` for user. Unfortunately, user has then also edit access to job orders for example.

To restrict access level edit only for candidates, we need to define ACL map and then keep user in `ACCESS_LEVEL_READ`.

Access level map is configured in `const ACCESS_LEVEL_MAP` in form:

```
<role> -> <secured object> -> <access level>
```

example of such map:

```
const ACCESS_LEVEL_MAP = array(
...
    'recruiter' => array(
        'calendar' => ACCESS_LEVEL_EDIT,
        'candidates'=> ACCESS_LEVEL_EDIT,
        'candidates.add'=> ACCESS_LEVEL_DISABLED
    ),
...
);
```

this map allows user in 'recruiter' role to manage calendar functionalities with minimum level `ACCESS_LEVEL_EDIT`; allows to manage candidates functionalities with minimum level `ACCESS_LEVEL_EDIT` but has disabled functionality for adding candidates (`'candidates.add'=> ACCESS_LEVEL_DISABLED`)

Another example is for role candidate, that has access only to joborders. User can have access level `ACCESS_LEVEL_READ` and we can restrict defining following map:

```
const ACCESS_LEVEL_MAP = array(
...
    'candidate' => array(
        ACL::SECOBJ_ROOT => ACCESS_LEVEL_DISABLED,
        'joborders' => ACCESS_LEVEL_READ,
    ),
...
);
```

`ACL::SECOBJ_ROOT` means that user with such role has `ACCESS_LEVEL_DISABLED` to all objects / functionalities; then user has `ACCESS_LEVEL_READ` to 'joborders' and also it's sub-objects ('joborders.\*')

Configuration is defined in config.php (for version 0.9.4). For later releases it is proposed to be moved to extra ACL.php file, so for forward compatibility, it is advised to add `include_once("./configACL.php");` into config.php file instead of direct definition in that file.

Role is defined in (existing) userCategory field, that was used for 'careerportal' functionality for similar purpose to role. So if user shall use access level of role, user shall have assigned proper category. When more categories are assigned to user, only first one is checked.

***

Changes made:

changed implementation of Session::getAccessLevel Open question: shall we treat default access level for user to be maximum access level? checks of access level in modules moved to handle request some public modules functions made private (not used out of class) menu (tabs/subtabs) access level checks also for secured object name (documented in function). What is not finished / needs further development

access level check not added into all places (only where it was used) secured object names corrections, simplification reading permissions / access levels (ACL) from a configuration file tabs/subtabs to be displayed based on permissions also for read only access automatic tests for different configuration of permissions
