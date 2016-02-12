# PHP 5 Nginx (`php5-nginx`)

Master:
[![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/php5-nginx/branches/master/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/php5-nginx)

Develop:
[![Build Status](https://semaphoreci.com/api/v1/bas-ansible-roles-collection/php5-nginx/branches/develop/badge.svg)](https://semaphoreci.com/bas-ansible-roles-collection/php5-nginx)

Bridging role to use PHP 5 with Nginx using PHP-FPM

**Part of the BAS Ansible Role Collection (BARC)**

## Overview

* Installs the latest stable version of PHP-FPM from non-system, or optionally, from system only sources
* Configures the PHP configuration file for the CLI SAPI with safe and secure defaults

## Quality Assurance

This role uses manual and automated testing to ensure the features offered by this role work as advertised. 
See `tests/README.md` for more information.

## Dependencies

* [**bas-ansible-roles-collection.php5**](https://galaxy.ansible.com/bas-ansible-roles-collection/php5/)
  * Minimum version: *0.2.0*
* [**bas-ansible-roles-collection.nginx**](https://galaxy.ansible.com/bas-ansible-roles-collection/nginx/)
  * Minimum version: *2.1.0*

Note: It is possible to 'pin' these role dependencies, and this is recommended for BAS projects. It is also possible to 
avoid these dependencies by skipping the *BARC_ROLE_DEPENDENCY* tag.

More information on role dependencies, dependency pinning and avoid dependencies is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-Manifest)

## Requirements

* None

More information on role requirements is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-requirements)

## Limitations

* None

More information on role requirements is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-limitations)

## Usage

### BARC Manifest

By default, BARC roles will record that they have been applied to a system, this is termed the BAS Manifest.
The specific local facts set for this role are:

* `ansible_local.barc_php5_nginx.general.role_applied` - (boolean) records whether a role has been applied
* `ansible_local.barc_php5_nginx.general.role_version` - (string) records the version of the role that was applied

Note: You **SHOULD** use this feature to determine whether this role has been applied to a system.
If you do not want these facts to be set by this role, you **MUST** skip the **BARC_SET_MANIFEST** tag.

More information is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Role-Manifest)

### What are bridging-roles?

Bridging-roles are a concept with the BARC for roles which enable two or more roles to be used together.

They are used to keep roles more generic and prevent roles needing to be aware of other roles. They also act as a
convenience as specifying the bridging role in a playbook will, through role dependencies, include the various roles it
'bridges'.

More information is available in the 
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Bridging-roles)

### Non-system packages

It is a convention of BARC roles to use the latest version of packages. Where a suitable non-system package source is 
available it will be used. Suitable non-system packages require a reputable, maintainer, typically a company or well 
respected individual. If this is for some reason unsuitable, it is possible to only use system packages by setting the 
*BARC_use_non_system_package_sources* variable to `false`.

Note: As the package policy varies between system and non-system package sources, and between operating systems, the 
version of installed packages is variable.

### PHP configuration files

This role, will only configure options relevant to the `fpm` SAPI specifically.

See the [PHP5](https://galaxy.ansible.com/bas-ansible-roles-collection/php5/) role, on which this role depends, 
for more information on PHP configuration files.

Settings for other SAPIs, such as the CLI, **SHOULD** be set in other roles, or project/purpose specific playbooks.
Providing the Ansible INI module is used, setting additional INI options will not cause conflicts with this role and 
so preserve idempotency.

The configuration options set by this role are considered suitably generic and applicable to all situations such that 
they are safe to be used as defaults - however it is accepted there are situations they would not be suitable. They 
include options for:

* Processing time and memory limits
* Default timezone - localised to *Europe/London*
* Error reporting and logging
* Hiding the PHP version for security purposes

See the *php5_nginx_sapi_fpm_options* variable in the role defaults file (`defaults/main.yml`) for the specific options 
set. Where any of these options are unsuitable, override this variable with a copy of these defaults, omitting the 
unsuitable options.

### PHP extensions

See the [PHP5](https://galaxy.ansible.com/bas-ansible-roles-collection/php5/) role, on which this role depends, 
for more information on which extensions are enabled and how to control them.

### Typical playbook

```yaml
---

- name: install php 5, nginx and php-fpm
  hosts: all
  become: yes
  vars: []
  roles:
    - bas-ansible-roles-collection.php5-nginx
```

### Tags

BARC roles use standardised tags to control which aspects of an environment are changed by roles.
Tasks in this role will 'tag' themselves with these tags as appropriate, typically in `tasks/main.yml`.

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Appendix-B---BARC-Standardised)

### Variables

#### *php5_nginx_barc_role_name*

* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: `php5_nginx`

#### *php5_nginx_barc_role_version*

* **MUST NOT** be specified
* Specifies the name of this role within the BAS Ansible Roles Collection (BARC) used for setting local facts
* See the *BARC roles manifest* section for more information
* Example: `2.0.0`

### *php5_nginx_sapi_fpm_options*

**MAY** be specified.

Specifies configuration options for the FPM (PHP-FPM) SAPI.

Structured as a list of items, with each item having the following properties:

* *section*
    * **MUST** be specified
    * Specifies the section of the INI configuration file options for this item belong to
    * Values **MUST** be valid section names as determined by the INI configuration format
* *options*
    * **MAY** be specified
    * Specifies the list of options, and values, to be set within the section set by the *section* item
    * Structured as a list of sub-items, with each sub-item having the following properties
        * *option*
            * **MUST** be specified if any part of the sub-item is specified
            * Specifies the *option* of the INI option/value pair
            * Values **MUST** be valid option names as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP
        * *value*
            * *MUST** be specified if any part of the sub-item is specified
            * Specifies the *value* of the INI option/value pair
            * Values **MUST** be valid values as determined by the INI configuration format and **MUST** be valid
            option names as determined by PHP
            * Boolean values **MUST** be quoted to prevent Ansible coercing values to True/False which is invalid for 
            PHP configurations

Example:

```yml
php5_nginx_sapi_fpm_options:
  - section: "Resource Limits"
    options:
      - option: max_execution_time  # (seconds)
        value: 30
```

Default: *See role defaults*

## Developing

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the 
[BAS Ansible Roles Collection](https://jira.ceh.ac.uk/projects/BARC) (BARC) project on Jira.

This service is currently only available to BAS or NERC staff, although external collaborators can be added on request.
See our contributing policy for more information.

More information is also available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Issue-Tracking)

### Source code

All changes should be committed, via pull request, to the canonical repository:

`ssh://git@stash.ceh.ac.uk:7999/barc/php5-nginx.git`

A read-only mirror of this repository is maintained on GitHub:

`git@github.com:bas-ansible-roles-collection/php5-nginx.git`

More information is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Source-Code)

### Contributing policy

This project welcomes contributions, see `CONTRIBUTING.md` for our general policy.

### Release procedure

The general release procedure for BARC roles is available in the
[BARC General Documentation](https://antarctica.hackpad.com/BARC-Overview-and-Policies-SzcHzHvitkt#:h=Release-procedures)

## Licence

Copyright 2015 NERC BAS.

Unless stated otherwise, all documentation is licensed under the Open Government License - version 3. All code is
licensed under the MIT license.

Copies of these licenses are included within this role.
