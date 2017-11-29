## x.y.z (pending)

## 2.14.2

* Support versions 7.2.8 till 7.4.2
* Standalone supports chef-client 13+
* Oracle DB Support
* Ubuntu 16.04 Xenial Support
* Fix wrong owner and directory path


## 2.14.1

* Enable a switch to activate/disable apache2
* Update gitignore
* fix download url for jira 7.2+


## 2.14.0

* Add missing settings to dbconfig.xml

## 2.13.0

* Add configurable DB connection pool sizes
* Add JIRA 7.1.9 checksums
* Add JIRA 7.1.7 checksums
* Bump default version to 7.1.9
* Downgrade checksum failure to warn and inform user how to bypass.

## 2.12.2

* Add JIRA 7.1.2 checksums
* Bump default version to 7.1.2

## 2.12.1

* Fix standalone install's home_path directory ownership
* Fix use of deprecated listen_address from apache2 cookbook
* Fix style issues in cookbook
* Move foodcritic rules to .foodcritic

## 2.12.0

* Add JIRA version 7.1.0 support
* Add simple logging message if JIRA does not need upgrading.

## 2.11.1

* Update JIRA Software checksums for 7.0.10
* Drop support for 5.2.x and 6.0.x releases except for latest

## 2.11.0

* Bump default version to 7.0.10

## 2.10.0

* Add option to override Apache webapp template using `node['jira']['apache2']['template_cookbook']` attribute
* Update README.md with proper recognition of contributions

## 2.9.0

* Add autotune feature for JIRA
  (currently autotunes min and max mem settings only)

## 2.8.0

* Add JIRA 7.0.4
* Bumped default version to 7.0.4
* Switched default DB to Postgres
* Add basic Postgres tuning
* Add default pwd for MySQL installs on localhost
* Set collation to 'C' for PostgreSQL database
* Added cloud testing config for `test-kitchen` via DigitalOcean.
  [[GH-55]](https://github.com/afklm/jira/issues/55)
* Force apache restart to circumvent `mod_proxy` bug on Ubuntu 12.04.
  [[GH-52]](https://github.com/afklm/jira/issues/52)
* Use versioned ark install paths.
  [[GH-56]](https://github.com/afklm/jira/issues/56)
* Small fixups to make all test suites succeed on cloud platforms.
  [[GH-57]](https://github.com/afklm/jira/issues/57)
* Fix standalone directory perms and accompanying CentOS bug.
  [[GH-57]](https://github.com/afklm/jira/issues/57)

## 2.7.3

* Added JIRA 7.0.2
* Bumped default version to 7.0.2

## 2.7.2

* Added sensitive true for crowd_sso template
* Set Postgres DB owner to JIRA user
* Explicitly set `home_path` perms.
  [[#48]](https://github.com/afklm/jira/issues/48)

## 2.7.0

* Added support for JIRA 7.0
* Added `node['jira']['flavor']` attribute with default value of 'software'
* Bump default JIRA version to 7.0.0

## 2.6.3

* Add JIRA 6.4.12 and bump default version

## 2.6.2

* Added a `node['jira']['group']` attribute for clarity and override ability
* Issue warn in library for unsupported DB types to make override possible

## 2.6.1

* Removed unused config entries from cookbook metadata
* Replaced file cookbook with FileEdit#search_file_replace

## 2.6.0

* Redirect http-based requests to https
* Auto upgrade if `node['jira']['version']` is higher than installed version
* Configures Crowd SSO if `node['jira']['crowd_sso']['enabled']` is true
* Improve file restrictions for standalone installs

## 2.5.1

* MIGRATED: renamed cookbook chef_jira -> jira after getting the supermarket
            namespace
* Bump default version to JIRA 6.4.11
* Use https for Jira downloads.
  [[GH-18]](https://github.com/afklm/jira/issues/18)
* Added service restart when ark resource changes.
  [[GH-16]](https://github.com/afklm/jira/issues/16)
* Added support for PostgreSQL 9.2+ in `dbconfig.xml`.
  [[GH-14]](https://github.com/afklm/jira/issues/14)
* Fixed LWRPs after cookbook name change.
  [[GH-13]](https://github.com/afklm/jira/pull/13)
* Set Tomcat `proxyName`/`proxyPort` even without SSL.
  [[GH-11]](https://github.com/afklm/jira/issues/11)
* Removed unnecessary non-dynamic `web.xml` template.
  [[GH-10]](https://github.com/afklm/jira/issues/10)
* Fixed setting of `jira.home` for all install types.
  [[GH-15]](https://github.com/afklm/jira/issues/15)

Thanks go to @elijah @gsreynolds @legal90 and @patcon for helping out in this
release.

## 2.1.0

* MIGRATED: renamed cookbook jira -> chef_jira

## 2.0.1

* Bugfix: #8: Remove include_attribute from default attributes to prevent hard dependency on tomcat cookbook

## 2.0.0

If you were using the default recipe, there are no changes you need to make for your environment to upgrade this cookbook.

Major features are full support for standalone deployments and war building/deployments. Using ark where possible (its worked well for those using my Stash cookbook).

I've removed the upgrade recipe in favor of using ark. If you'd like to keep old JIRA installations around (in case of upgrade issues, etc.), I would recommend switching to the standalone install_type since ark will automatically create versioned symlinks and you can easily revert `node['jira']['version']`. To convert install_type if install_path is the default (X.Y.Z being currently installed `node['jira']['version']`):
* `service jira stop`
* `mv /opt/atlassian/jira /opt/atlassian/jira-X.Y.Z`
* `ln -s /opt/atlassian/jira-X.Y.Z /opt/atlassian/jira`
* Set `node['jira']['install_type']` to standalone
* Run Chef Client

Other than that, migrated some recipes/templates and split out some recipes from the old linux_installer recipe, so ensure that if you're using a custom run list or template override for any nodes, that they include the new recipes/template location as necessary.

Full details:
* REMOVED: upgrade recipe and associated `backup_*` and `*_backup` attributes
* MIGRATED: linux_installer -> installer recipe
* MIGRATED: tomcat_configuration -> container_server_configuration recipe
* MIGRATED: Tomcat templates into tomcat folder:
  * permgen.sh.erb -> tomcat/permgen.sh.erb
  * server.xml.erb -> tomcat/server.xml.erb
  * setenv.sh.erb -> tomcat/setenv.sh.erb
  * web.xml.erb -> tomcat/web.xml.erb
* SPLIT: SysV init service configuration into sysv recipe and add init_type attribute
* SPLIT: database jar deployment (mysql_connector_j, etc.) into container_server_jars recipe which also installs JIRA jars for war install_type
* Bugfix: Use :create action instead of :create_if_missing for installer remote_file
* Enhancement: Add standalone and war recipes and quite a few attributes for supporting those install_type's
* Enhancement: Add build_war recipe
* Enhancement: LWRPs for handling multiple instances of install, etc.
* Enhancement: Bump default JIRA version to 6.1.5

## 1.7.0

* Bump default JIRA version to 6.1

## 1.6.0

* Bump default JIRA version to 6.0.7

## 1.5.0

* Bump default JIRA version to 6.0.6

## 1.4.0

* Initial Microsoft SQL Server support

## 1.3.0

* Bump default JIRA version to 6.0.5

## 1.2.0

* Bump default JIRA version to 6.0.2

## 1.1.0

* Bump default JIRA version to 6.0.1

## 1.0.0

* Split default recipe into individual recipes
* apache2 recipe does not include default recipe
* Load database/tomcat settings via Jira.settings library (bonus: help support Chef Solo)
* Moved apache2 attributes into default attributes
* Bump default JIRA version to 5.2.11
* Added url_base attribute
* Auto-detect checksum attribute for some versions
* Added Vagrantfile and Test Kitchen for testing
* minitest fixes
* Added COMPATIBILITY.md
* Refactored README documentation

## 0.1.3

* Chef 11 fixes in apache2 recipe

## v0.1.2

* Hopefully removed hard dependency on java_ark from java cookbook

## v0.1.1

* Added permgen.sh template for custom JAVA_HOME, otherwise always defaults to
  JIRA installed JRE

## v0.1.0

* Initial release
