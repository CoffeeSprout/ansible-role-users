coffeesprout.users
==================

Setup local OS level users. Mainly meant for admin accounts, rsync users etc.
Mostly just a wrapper around [ansible user](https://docs.ansible.com/ansible/latest/modules/user_module.html)

Requirements
------------

None

Role Variables
--------------

    users: []

A list of users to be created on the system. For example:

    users:
    - name: admin
      key: "{{lookup('file', 'admin.pub')}}"
    - name: barry
      key: "https://github.com/barry.keys"
      key_options: "environment=\"SUPER=man\""
    - name: systemuser
      system: True
    - name: differentkeyeduser
      key: "{{lookup('file', 'differentkeyeduser.pub')}}"
      pubkey_location: "/etc/ssh/user_keys/differentkeyeduser.pub"
    - name: offboardeduser
      state: absent

It's recommended you use the first format (lookup the pub file) and check the files in together with your playbook.

    sudo_users: []
    
A simple list of strings which users should be root on the system. This manages an included file (see below) with the added advantage that we don't have to add or remove from the wheel group. It's generally recommended you keep at least one user in the wheel group, for example the user used to provision the server with.
  

    users_sudoersd_file: "{{ sudoersd_location }}/managed"
    
Location of the sudoers.d file that contains our list of sudoers; This is overriden dynamically based on OS

    users_pubkey_location: " ~/.ssh/authorized_keys"

The location where the key should be uploaded to; Change this if you have setup SSHD to look elsewhere.
This change is something you need to manage outside this role, as well as the folder.
You might want to do this because of the following reasons:
* To prevent the user from changing the keys or options you set on them
* Because not every user has a home folder

Changing the overall users\_pubkey\_location or the specific user pubkey\_location will make the key readonly to the user.
This can also be triggered by adding lock\_key to the user.

You can add further control to users by defining key\_options; They map to the default options in SSH: https://man.openbsd.org/sshd#AUTHORIZED_KEYS_FILE_FORMAT

It's also possible to set:

    users_default_key_option

This option will be set for all user keys (for example on a jumphost you would not allow logins, but you will alow proxyjump); If you define an option for the user it will override this default (For example for granting the admin full access)

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
      - role: coffeesprout.users
        users:
        - name: admin
          key: "{{lookup('file', 'admin.pub')}}"
        - name: barry
          key: "https://github.com/barry.keys"
        - name: systemuser
          system: True
        - name: differentkeyeduser
          key: "{{lookup('file', 'differentkeyeduser.pub')}}"
          pubkey_location: "/etc/ssh/user_keys/differentkeyeduser.pub"

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
