# Access control lists

OpenCATS uses access-control checks around modules, actions, settings pages, imports, and AJAX behavior. Recent security work tightened authorization checks for module actions and AJAX endpoints, so custom code should not assume that a reachable URL is permission-free.

## OpenCATS 0.12.0 changes

CSV exports through the export module require Site Administrator access, including candidate exports. Recruiters with ordinary add/edit/delete access should not expect the export action to be available.

Additional checks cover tag administration, attachment deletion, hidden candidates in job-order pipelines, email/phone lookup endpoints, and activity/pipeline updates. Access to a parent record must also be respected when working with its related records or attachments.

## Practical guidance

* Give users the lowest access level that supports their role.
* Review access after adding users, changing roles, or enabling optional workflows such as import.
* Test custom modules or custom AJAX endpoints with low-privilege users.
* Do not expose administrative actions through custom links unless the action is protected by OpenCATS permission checks.
* Preserve CSRF token handling when customizing forms or JavaScript.

## Customization note

If you add custom modules, actions, or settings pages, review the OpenCATS configuration and permission mappings in the application code so your new entry points are covered by the same authorization model as built-in pages.
