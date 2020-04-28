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
- name: systemuser
  system: True


    users_sudoersd_file: "{{ sudoersd_location }}/managed"
    
Location of the sudoers.d file that contains our list of sudoers; This is overriden dynamically based on OS

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

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
