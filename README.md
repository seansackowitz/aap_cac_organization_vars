# How to use as-code to get Ansible Playbooks into AAP as Job Templates
## Background

You’ve created some playbooks and you need to get them into the Ansible Automation Platform (AAP).  In this document we’ll attempt to explain the step-by-step process.
All of your content will be added to AAP using a Job Template in AAP.  You’ll add definitions in vars files for all of the projects, credentials, job templates, workflow templates, etc.  Once your definitions are added to the vars files, a playbook / Job Template will execute which will create in AAP all of your objects you defined in the vars files.
## Prerequisites

1. You’ve written your playbooks and committed them to a GitHub repository.
2. Hopefully you’ve also run one or more of the following checks.  In the future some or all of these checks may be performed by GitHub actions or some other pipeline tool.
    - ansible-lint and/or yaml-lint

    - ansible-playbook <playbook name> --syntax-check

    - ansible-playbook <playbook name> --check
3. You will need to know your Organization name in AAP before you can start creating your object definitions.
## Add Definitions to vars files
**NOTE:**  You do not need to add your playbooks to the playbooks directory of this repository.  Your playbooks should remain in their existing repository.  The playbooks in this repository are for configuration purposes.
To get started, clone the repository to your local development system.
    git clone https://github.com/seansackowitz/aap_cac_organization_vars
Once you’ve cloned the repository, create a unique feature branch
    git checkout -b <feature branch name>
Now you're ready to start creating your object definitions.  You can define Projects, Credentials, Inventories, Inventory Sources, Notifications, Schedules, Job Templates, Workflow Templates, Labels, and more.
You will see at the root of the repository several yaml files. Each of these yaml files are resource definition files that can be used to define any resources required by your organization. Each of the files contains a single top level variable that is a list. Add your definitions to these files.

At a very minimum you will need to define:
1. Credentials
2. Projects
3. Inventories / Inventory Sources
4. Job Templates

There is much more that you can add but these are the bare minimum to get up and running in AAP.
To define a credential you would edit the credentials.yml file.  You will see variable called `controller_credentials`.  This is where you would define your credentials.
**NOTE:** If you define credentials in credentials.yml, know that it is considered bad practice to include passwords in plain text.
You can reference documentation for the credential definition [here](https://github.com/redhat-cop/controller_configuration/tree/devel/roles/credentials).
A basic (minimal) definition for a credential might look like:
```yaml
- name: 'gitlab'
  organization: 'Default'
  credential_type: 'Source Control'
  inputs:
    username: 'person'
    password: 'password'
```
However looking at the **Data Structure** table in the documentation you will see that many more options are available.  Check the `Required` column to determine which parameters are necessary and decide on any additional parameters you wish to include.
Add as many credential definitions as you need and then save your file changes.
You can repeat this process for all of the object types you need to define.
[Projects](https://github.com/redhat-cop/controller_configuration/tree/devel/roles/projects)
[Inventories](https://github.com/redhat-cop/controller_configuration/tree/devel/roles/inventories)
[Job Templates](https://github.com/redhat-cop/controller_configuration/tree/devel/roles/job_templates)
A complete list of roles and their documentation is [here](https://github.com/redhat-cop/controller_configuration/tree/devel/roles).
Once you've added all of your object definitions, you may want to check the vars files with YAML lint to make sure the data structures are properly formatted.  If you skip this step, then the objects will fail to import if any of the object definitions are improperly formatted (bad indentiation, etc).
When you're ready, commit your changes:
    git commit -am "add a unique commit message"
And then push your changes to GitHub:
    git push -u origin <feature branch name>
After you've successfully pushed your changes to GitHub you will need to login to GitHub and open a pull request to merge your changes to the main branch.  You will need to assign some reviewers to review and approve your changes before you can merge them to the main branch.