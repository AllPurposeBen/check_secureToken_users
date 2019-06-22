# A Watchman Monitoring Plugin for reporting on user account secure token status
## Purpose

This plugin will list user accounts, on applicable systems, that are missing a secure token. This is helpful to find users that may have been created programmatically by various methods and wouldn't have received a secure token, thus preventing them from be able to unlock FileVault 2 encrypted disks.

We look for any user account with a UID over 500. If users without tokens are found, you get a warning for the plugin results. You can optionally specify a localadmin user to check explicitly by shortname (in case your localadmin has a < 500 UID). If the specified localadmin account is missing a token, you'll get a WM alert instead of a warning.

Applicable systems are any install of macOS on an APFS filesystem. Since secure token only applies to APFS and FileVault 2, we don't even run the plugin on HFS+ systems. In the event of a system getting upgraded and thus conversion to APFS, the plugin will start running.

## Parts
3 files make up this plugin's components:

* `check_secureToken_users.plugin` The actual plugin code. Written in bash. This lives in `.../Plugins/` folder.
* `check_secureToken_users.plist` This is the required plist that tells the WM agent how to run the plugin. Also lives in `.../Plugins/` folder.
* `check_secureToken_users_settings.plist` This plist has some extra data that allows you to set a localadmin user account name. PluginSupport/check_secureToken_users_settings.plist specifiedUser 'shortname'` This file lives in `.../PluginSupport/`

##Settings
You can set the localadmin user shortname in the WM Pref Pane under `Settings -> Secure Token Users` 

Or via CLI with:
```defaults write /Library/MonitoringClient/PluginSupport/check_secureToken_users_settings.plist specifiedUser 'shortname'```

##Future Improvements 

* Add ability to choose to receive an alert instead of a warning when a non-token bearing user is found.
* Ability to list user shortnames to exclude from checking.
* Likely having this plugin's functionality Sherlock'd into Watchman's built in plugins. 

## Deployment
This may be avaialble for automatic deployment for your wM subdomain in the (someday) WM Plugin Market Place but could be had by requesting from WM Support in the mean time.

If you'd like to deploy it yourself, you can package up the above components for whatever method you like (I use munki-pkg) or use this pre-built installer pkg.

If you package your own, you could set your localadmin user setting very easily at deployment time.

### Thanks
Please feel free to submit issues as you find them and PR's for improvments, if you're so inclined. 


