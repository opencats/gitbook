# Hiding tabs

If you only want to use a subset of the tabs available, and want to hide the ones you don't use, then add the names of the tabs you want hidden to the printTabs method in the `TemplateUtility.php` class on line 602 like so

`if (empty($tabText) || $tabText === 'Companies' || $tabText === 'Job Orders' || $tabText === 'Activities') { continue; }`

Originally the line was:

`if (empty($tabText)) { continue; }`

and only add the modules you want to hide.
