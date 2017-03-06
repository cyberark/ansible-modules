cyberark_modules
================

Role to add CyberArk modules -- If not available from ansible core, or to get the latest.

Requirements
------------

- CyberArk Privileged Account Security Web Services SDK.

Role Variables
--------------

None.

Provided Modules
----------------

- **cyberark_authentication**: Module for CyberArk Vault Authentication using Privileged Account Security Web Services SDK
- **cyberark_user**: Module for CyberArk User Management using Privileged Account Security Web Services SDK


Example Playbook
----------------

Example playbook showing how to include CyberArk modules in your playbook. 

```
---
- hosts: all 

  roles:
    - role: {enunez-cyberark.cyberark_modules}
```

License
-------

BSD

Author Information
------------------

- Edward Nunez (edward.nunez@cyberark.com)
