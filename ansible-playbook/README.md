# Ansible Template
An attempt of getting some handy template for just another standard Ansible project

## First, credits:

Special thanks to:
- Roberto, Daniel, Marcos and all the crew collaborating in [AAG Ansible Content](https://github.dxc.com/Hosting2020/aag_ansible_content)
- This wonderful project about [playbooks to fix commons false positives alerts](https://github.dxc.com/Hosting2020/ansible_unix_alert_reduction)
- [Ansible Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

## Best Practices

Below is the best practice for the directory structure.
The Mandatory fields must be follow otherwhise the content won't be pushed.

### Directory Structure

    project_name                  # directory where the project resides (MANDATORY)
        README.md                 # instructions on how to run the playbook and general information (MANDATORY)
        inventory                 # folder for inventory collections
            development           # inventory file for development servers
            hosts                 # common inventory file (MANDATORY)
            production            # inventory file for production servers
            staging               # inventory file for staging environment

        group_vars/
           group1.yml             # here we assign variables to particular groups
           group2.yml
        host_vars/
           hostname1.yml          # here we assign variables to particular systems
           hostname2.yml

        library/                  # if any custom modules, put them here (optional)
        module_utils/             # if any custom module_utils to support modules, put them here (optional)
        filter_plugins/           # if any custom filter plugins, put them here (optional)

        site.yml                  # master playbook (MANDATORY)
        webservers.yml            # playbook for webserver tier
        dbservers.yml             # playbook for dbserver tier

        roles/
            common/               # this hierarchy represents a "role"
                tasks/            #
                    main.yml      #  <-- tasks file can include smaller files if warranted
                handlers/         #
                    main.yml      #  <-- handlers file
                templates/        #  <-- files for use with the template resource
                    ntp.conf.j2   #  <------- templates end in .j2
                files/            #
                    bar.txt       #  <-- files for use with the copy resource
                    foo.sh        #  <-- script files for use with the script resource
                vars/             #
                    main.yml      #  <-- variables associated with this role
                defaults/         #
                    main.yml      #  <-- default lower priority variables for this role
                meta/             #
                    main.yml      #  <-- role dependencies
                library/          # roles can also include custom modules
                module_utils/     # roles can also include custom module_utils
                lookup_plugins/   # or other types of plugins, like lookup in this case

            webtier/              # same kind of structure as "common" was above, done for the webtier role
            monitoring/           # ""
            fooapp/               # ""


## Playbooks Information

All playbooks requires the following extra var:

* `server`: the playbook uses this variable to identify the host where is going to be applied, acording to inventory file content. Groups and "all" can also be used.

Here is the information for each of the playbooks.

### Meta playbook for remediations

1. `site.yml`

* Main playbook. This apply all steps for some server(s)

### Playbooks for specific remediations

1. `concrete_playbook.yml`

   * This playbook perform some discrete actions


## How to run the playbooks manually

All the playbooks are run in a similar fashion

```console
$ ansible-playbook -u edXXXXXX -kK --extra-vars="server=testVm" -i ./inventory_list remove_scopeux_alert.yml
 ...
```

* `edXXXXXX`: user to log on the remote server.

* `testVm`: server or group where this playbook is going to be execute. It should exist in the inventory referenced below.

* `inventory_list`: inventory file or folder, so as to include multiple inventories, which contains server(s) or group(s) where this playbook needs to be executed.


### Related Content

* Here is a quick guide for the use GitHub Enterprise withing our Automation Development context: https://github.dxc.com/Hosting2020/use_ghe_on_compartment  
* Internal Best Practices for repositories setup and maintenance: https://github.dxc.com/Hosting2020/ghe_best_practices
* Ansible Best Practices: https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html
