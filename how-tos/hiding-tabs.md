# Hiding tabs

### Hiding some tabs all the time

If you only want to use a subset of the tabs available, and want to hide the ones you don't use, then add the names of the tabs you want hidden to the printTabs method in the `TemplateUtility.php` class on line 602 like so

`if (empty($tabText) || $tabText === 'Companies' || $tabText === 'Job Orders' || $tabText === 'Activities') { continue; }`

Originally the line was:

`if (empty($tabText)) { continue; }`

and only add the modules you want to hide.

### Hiding tabs for users based on privilege

turn off / on tabs for different permissions. This is a way for tabs not to appear (but users may still access them)

At any time - you can get the permission of the logged in user with `$loggedInAccessLevel = $_SESSION['CATS']->getRealAccessLevel(); Real Access level returns the logged in user access:, Read Only - 100, Add / Edit - 200, Add / Edit / Delete (Default) - 300, Site Administrator - 400, Root - 500`

Adding this code to printTabs in TemplateUtility.php in the f`oreach ($modules as $moduleName => $parameters)` loop will hide certain tabs if a user does not meet the appropriate permissions. Module names are: home, activity, joborders, candidates, companies, contacts, lists, calendar, reports, settings

`$loggedInAccessLevel = $_SESSION['CATS']->getRealAccessLevel(); $minimumAccessLevel = array ("lists" => 400, "companies" => 400); if (array_key_exists($moduleName, $minimumAccessLevel)) { if ($loggedInAccessLevel < $minimumAccessLevel[$moduleName]) { continue; //Disabling module for the user by not showing it - if they do not have the minium access level } }`

This is partially supported in the [ACL implementatio](../technical-configuration-options/access-control-lists.md)n - there is explained ACL.\
In some pages, there is a check for 'calculated''access level and required access level, if added into all pages (modules), then it shall be easy to hide menu an also to protect backend functionality. (hiding menu just don't show page to user but it is easy to construct get request to change values).\


There was this change for some menu:\
[https://github.com/opencats/OpenCATS/pull/91/files#diff-1b811f65c6b10c3dc1cd71932e2f911dL46](https://github.com/opencats/OpenCATS/pull/91/files#diff-1b811f65c6b10c3dc1cd71932e2f911dL46)

\
and also implementation in Template utility [https://github.com/opencats/OpenCATS/pull/91/files#diff-da82a11a9e0e4b5b31ac602622b777c5L647](https://github.com/opencats/OpenCATS/pull/91/files#diff-da82a11a9e0e4b5b31ac602622b777c5L647)

Usage is documented at: [https://github.com/AnritsuSolutionsSK/OpenCATS/blob/develop/lib/TemplateUtility.php#L576](https://github.com/AnritsuSolutionsSK/OpenCATS/blob/develop/lib/TemplateUtility.php#L576)
