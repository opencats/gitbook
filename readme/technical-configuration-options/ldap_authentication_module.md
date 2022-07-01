# LDAP\_Authentication\_module

\###LDAP Authentication module

\*\*\* Note that LDP integration has been updated and included by default - see INSTALL.MD for info, or some of the LDPA commit info e.g. [https://github.com/opencats/OpenCATS/commit/525f110dd0652d53958f6adf2c7f57a48b6f229e](https://github.com/opencats/OpenCATS/commit/525f110dd0652d53958f6adf2c7f57a48b6f229e)&#x20;



Please read the README before using the plugin.

**Active Directory**

I've been working to try and get this plugin to auth against active directory. I use this model but it does not seem to work.

&#x20;  `define ('AUTH_MODE', 'ldap'); // Currently supports ldap, sql` `define ('LDAP_HOST', 'ldap_server');` `define ('LDAP_PORT', '389');` `define ('LDAP_BASEDN', 'ou=users,ou=company,dc=local,dc=domain');` `define ('LDAP_UID', 'sAMAccountName');` `define ('LDAP_CONNECT_DN', 'uid=test,ou=users,ou=company,dc=local,dc=domain');` `define ('LDAP_PASSWORD', 'test123');`

Not sure if this is an issue with the code right now or not. Like the user in the thread this came from it does not seem to work with sAMAccountName or uid.



\[**ADD - needs to be updated to reflect current codebase**]
