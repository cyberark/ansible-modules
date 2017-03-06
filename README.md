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

**Note**: As the role will include the galaxy user, you can create a symbolic link as follows:
```
ln -s /etc/ansible/roles/enunez-cyberark.cyberark_modules cyberark_modules
```

1) Example playbook showing the use of cyberark_authentication module for logon and logoff without using shared logon authentication.

```
---
- hosts: localhost

  roles:

    - role: cyberark_modules

  tasks:

    - name: Logon to CyberArk Vault using PAS Web Services SDK
      cyberark_authentication:
        api_base_url: "https://components.cyberark.local"
        validate_certs: false
        username: "testuser"
        password: "Cyberark1"


    - name: Debug message
      debug:
        var: cyberark_session


    - name: Logoff from CyberArk Vault
      cyberark_authentication:
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Debug message
      debug: var=cyberark_session
```


2) Example playbook showing the use of cyberark_user module to get user details.
```
---
- hosts: localhost

  roles:

    - role: cyberark_modules

  tasks:

    - name: Logon to CyberArk Vault using PAS Web Services SDK
      cyberark_authentication:
        api_base_url: "https://components.cyberark.local"
        validate_certs: false
        use_shared_logon_authentication: true

    - name: Debug message
      debug:
        var: cyberark_session

    - name: Get Users Details
      cyberark_user:
        user_name: "testuser"
        state: details
        cyberark_session: "{{ cyberark_session }}"
      register: cyberarkaction

    - debug: msg="{{cyberarkaction.cyberarkUser.result}}"
      when: cyberarkaction.status_code == 200

    - name: Logoff from CyberArk Vault
      cyberark_authentication:
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Debug message
      debug: var=cyberark_session
```


3) Example playbook showing the use of cyberark_user module to create a user.
```
---
- hosts: localhost

  roles:

    - role: cyberark_modules

  tasks:

    - name: Logon to CyberArk Vault using PAS Web Services SDK
      cyberark_authentication:
        api_base_url: "https://components.cyberark.local"
        validate_certs: false
        use_shared_logon_authentication: true

    - name: Debug message
      debug:
        var: cyberark_session

    - name: Create User
      cyberark_user:
        user_name: "testuser"
        initial_password: "Cyberark1"
        user_type_name: "EPVUser"
        change_password_on_the_next_logon: false
        state: present
        cyberark_session: "{{ cyberark_session }}"
      register: cyberarkaction

    - debug: msg="{{cyberarkaction.cyberarkUser.result}}"
      when: cyberarkaction.status_code == 201

    - name: Logoff from CyberArk Vault
      cyberark_authentication:
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Debug message
      debug: var=cyberark_session
```


4) Example playbook showing the use of cyberark_user module to reset's a user credential.
```
---
- hosts: localhost

  roles:

    - role: cyberark_modules

  tasks:

    - name: Logon to CyberArk Vault using PAS Web Services SDK
      cyberark_authentication:
        api_base_url: "https://components.cyberark.local"
        validate_certs: false
        use_shared_logon_authentication: true

    - name: Debug message
      debug:
        var: cyberark_session

    - name: Get Users Details
      cyberark_user:
        user_name: "testuser"
        state: details
        cyberark_session: "{{ cyberark_session }}"
      register: cyberarkaction

    - debug: msg="{{cyberarkaction.cyberarkUser.result}}"
      when: cyberarkaction.status_code == 200

    - name: Reset user credential
      cyberark_user:
        user_name: "testuser"
        new_password: "Cyberark1"
        disabled: false
        state: update
        cyberark_session: "{{ cyberark_session }}"
      register: cyberarkaction

    - debug: msg="{{cyberarkaction.cyberarkUser.result}}"
      when: cyberarkaction.status_code == 200

    - name: Logoff from CyberArk Vault
      cyberark_authentication:
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Debug message
      debug: var=cyberark_session
```


5) Example playbook showing the use of cyberark_user module to delete a user.
```
---
- hosts: localhost

  roles:

    - role: cyberark_modules

  tasks:

    - name: Logon to CyberArk Vault using PAS Web Services SDK
      cyberark_authentication:
        api_base_url: "https://components.cyberark.local"
        validate_certs: false
        use_shared_logon_authentication: true

    - name: Debug message
      debug:
        var: cyberark_session

    - name: Remove  User
      cyberark_user:
        username: "testuser"
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Logoff from CyberArk Vault
      cyberark_authentication:
        state: absent
        cyberark_session: "{{ cyberark_session }}"

    - name: Debug message
      debug: var=cyberark_session
```

License
-------

BSD

Author Information
------------------

- Edward Nunez (edward.nunez@cyberark.com)
