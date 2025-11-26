1. What is Ansible, and how does it differ from other configuration management tools like Chef or Puppet?
   - Ansible is an open-source automation tool used for configuration management, application deployment, and task automation.
   - It helps us to manage multiple systems from a single control node using simple YAML playbooks.
   - Ansible is agentless works over SSH, idempotent (makes changes only when needed), and follows the Infrastructure as Code approach to ensure consistency and reliability across 
     environments.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2. Is ansible use Iac.?
   - Ansible follows the Infrastructure as Code approach, where infrastructure configurations are written as code using YAML playbooks.
   - This means server setups, software installations, and configurations are defined in code form — version-controlled, reusable, and consistent across environments.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

3. Explain the architecture of Ansible — what are control nodes and managed nodes?
    - Ansible follows a Control Node and Managed Node architecture.
    - The Control Node is where Ansible is installed and from where all playbooks and automation tasks are executed.
    - The Managed Nodes are the target machines that Ansible manages using SSH for Linux or WinRM for Windows.
    - The actual task execution happens on the Managed Nodes and the Control Node only sends instructions and modules, which are executed locally on the managed nodes to perform the 
      required actions.
    - Since Ansible is agentless, no additional software is needed on the managed nodes.
    - playbooks are written and stored on the Control Node.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

4. What is an inventory file in Ansible, and how is it structured?
   - In Ansible, the Inventory file is a configuration file that contains the list of managed nodes (hosts) that Ansible controls.
   - It defines which servers Ansible will connect to and how they are grouped like dev, test, or production environments.
   - The default inventory file is located at /etc/ansible/hosts but we can also create custom inventory files.
   - The inventory can be written in INI, YAML, or dynamic format.

   * Ansible supports three types of inventories:
      1. Static Inventory where hosts are manually defined.
      2. Dynamic Inventory where hosts are fetched automatically from external sources like AWS or Azure.
      3. Hybrid Inventory which combines both static and dynamic sources.
         - This flexibility allows Ansible to work efficiently in both traditional and cloud-based environments.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

5. What are Playbooks in Ansible?
   - Playbooks are YAML files in Ansible that define a set of tasks to automate configuration, deployment, and management of systems.
   - They describe the desired state of servers in a human-readable, declarative format.
   - Each playbook can contain one or more plays, and each play has tasks, vars, handlers that execute Ansible modules on the target hosts.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

6.  What is the difference between a play and a task in Ansible?
    * Play is a higher level in playbooks.
      - Play define hosts, tasks, vars and handlers.
      - One play contains multiple tasks executed sequentially.

    * task is a low level in playbooks.
      - task defines modules like apt, service, copy.
      - Each task is executed individually in all targeted hosts.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   

7.  What are Ansible modules Give examples of commonly used ones?  
    - Ansible modules are predefined and reusable units of code that perform specific tasks on managed nodes.
    - They are used inside playbooks to install packages, manage services, copy files, and more.	

  How Modules Work:
   - A task in a playbook calls a module.
   - The Control Node pushes the module to the Managed Node.
   - The module executes the required action on the node.
   - The module returns a JSON result back to the Control Node.
   - Modules are idempotent — they only make changes if necessary. 	

  Types:
   * Core Modules – shipped with Ansible (always available).
   * Extra Modules – additional modules installed separately.
   * Custom Modules – written by users if needed.

  Common Module Categories:
   - Package Management – install/uninstall software
   - Service Management – start/stop services
   - File Management – copy files, manage directories, set permissions
   - User Management – add/remove users or groups
   - Command/Script Execution – run shell commands
   - Cloud Modules – create instances in AWS, Azure, GCP

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

8. What is an Ansible role, and how is it structured?  
   - Ansible Roles are a structured way to organize playbooks and related files into reusable components.
   - A role contains directories like tasks, handlers, templates, files, and vars that define specific automation logic.
   - Roles help us to keep playbooks modular, reusable, and easier to maintain especially in large environments.

  project/
├── ansible.cfg
├── inventory/
│   ├── dev
│   ├── prod
├── group_vars/
│   ├── all.yml
│   ├── webservers.yml
├── roles/
│   ├── webserver/
│   ├── database/
├── playbooks/
│   ├── site.yml
│   ├── deploy.yml
└── templates/


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

9. How do you reuse and organize Playbooks efficiently?
   - In Ansible, playbooks are reused and organized efficiently by breaking them into smaller, modular components using roles, includes, and imports.
   - Roles help structure automation into reusable parts like tasks, handlers, variables, and templates.
   - We also use variable files, templates, and handlers to avoid repetition, and organize inventory and group variables for different environments.
   - This modular approach makes playbooks cleaner, reusable, and easier to maintain in large automation projects.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

10. What is the use of ansible.cfg?
    - The ansible.cfg file is Ansible’s main configuration file used to define default settings like inventory path, SSH connection options, privilege escalation, and roles path.
    - It customizes how Ansible behaves and is read automatically when running playbooks, helping simplify and standardize Ansible operations.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

11. How do you secure sensitive data in Ansible?
    - In Ansible sensitive data such as passwords, API keys, or secret tokens are secured using Ansible Vault.
    - Ansible Vault allows you to encrypt files, variables, or even entire playbooks so that confidential information isn’t stored in plain text.
    - Only users with the vault password or key file can decrypt and use that data during playbook execution.

     Commands: 
     * ansible-vault create secret.yml - Create and encrypt a new file.
     * ansible-vault encrypt vars.yml  - Encrypt an existing file.
     * ansible-vault decrypt vars.yml  - Decrypt a file
     * ansible-vault edit secret.yml   - Edit an encrypted file securely.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

12. What are Ansible Vaults and how do you use them?
    - Ansible Vault is a security feature that allows you to encrypt sensitive data such as passwords or API keys in Ansible files.
    - It ensures that secrets are not stored in plain text and can only be accessed with a vault password.
    - You can create, edit, encrypt, or decrypt files using commands like ansible-vault create, encrypt, and decrypt, and use them in playbooks with the --ask-vault-pass option.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

13. How can you handle dynamic inventories in Ansible?
    - Dynamic Inventory in Ansible allows automatic fetching of host information from external sources like cloud providers (AWS, Azure, GCP) instead of maintaining static files.
    - It uses inventory plugins or scripts to generate host lists in real-time, making it ideal for dynamic, cloud-based, or auto-scaling environments.

    Example: AWS EC2 Dynamic Inventory

            Step 1: Enable the AWS EC2 inventory plugin
                    - Create a file aws_ec2.yml:

                         plugin: amazon.aws.ec2
                         regions:
                           - us-east-1
                         keyed_groups:
                           - key: tags.Name
                             prefix: tag


             Step 2: Run Ansible commands using this inventory
                    - ansible-inventory -i aws_ec2.yml --list


             Step 3: Use it in a playbook
                    - ansible-playbook -i aws_ec2.yml site.yml
   Ansible will automatically pull all EC2 instances from your AWS account and target them by tags or other attributes.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

14. What is register in Ansible, and how do you use its output?
    - In Ansible the register keyword is used to capture the output of a task and store it in a variable.
    - This allows you to use that output later in the playbook for example, to make decisions, perform conditional checks, or display results.
    - The registered variable stores details like the command output (stdout), error message (stderr), and return code (rc).
    - You can then use these values in conditional statements (when:) or debug tasks.-

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

15. What are Ansible facts, and how can you use them in Playbooks?
    - Ansible facts are system information variables that Ansible automatically collects from managed nodes when a playbook runs.
    - These facts contain detailed data about the target machine such as its hostname, IP address, operating system, network interfaces, CPU, memory, and more.
    - Facts are gathered by a special module called setup, and you can use these values dynamically in your playbooks for conditional logic, configuration, or templating.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

16. How do you implement conditional statements (when) in Ansible?
    - In Ansible, the when statement is used to implement conditional logic.
    - It ensures a task runs only when a specific condition is true, based on facts, variables, or registered results.
    - The when clause uses Jinja2 expressions and is commonly used for OS-based tasks, conditional installations, or handling command results.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

17. How can you use loops (with_items, loop) in Ansible tasks?
    - In Ansible, loops are used to repeat a task multiple times with different values.
    - The modern way to implement loops is using the loop keyword, while the older syntax uses with_items.
    - Loops help avoid duplication and make playbooks cleaner and more efficient.
    - For example, you can install multiple packages or create multiple users in a single task using a loop.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 
18. What is idempotency in Ansible?
    - Idempotency in Ansible means that running the same playbook multiple times will not make any unnecessary changes if the system is already in the desired state.
    - It ensures automation is repeatable, reliable, and safe, and most Ansible modules are designed to be idempotent by default.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

19. How do handlers and notify work in Ansible?
    - In Ansible, handlers are special tasks that are triggered only when notified by another task.
    - They are typically used to perform actions like restarting a service only if a change occurs not every time the playbook runs.
    - The notify directive is used inside a task to tell Ansible which handler to run when that task reports a change.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

19. How do you debug Playbooks when something fails?
    - To debug playbooks in Ansible, I use verbosity options (-vvv) for detailed logs, the debug module to print variable values and ignore_errors or failed_when to control failures.
    - I also use --step or --start-at-task to run specific tasks and enable logging in ansible.cfg to trace issues easily.
    - These methods help identify and fix problems quickly and safely.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

20. What are Ansible tags, and how do they help in execution control?
    - Ansible tags are labels that you assign to tasks, plays, or roles in a playbook to control which parts of the playbook are executed.
    - Tags are very useful when you have a large playbook and you only want to run or skip specific tasks instead of executing everything.
    - They don’t change what a module does (like installing or restarting), but they help decide when a particular task should run.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

21. How can you use Ansible for AWS provisioning?
    - You can use Ansible for AWS provisioning by using AWS modules like amazon.aws.ec2, ec2_vpc_net, and ec2_security_group to create and manage AWS resources directly from playbooks.
    - Ansible connects to AWS using access keys or IAM roles and automates tasks such as launching EC2 instances, configuring networking, or creating S3 buckets, making it an effective 
      Infrastructure as Code tool for cloud automation.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

22. How can Ansible integrate with Jenkins for CI/CD automation?
    - Ansible integrates with Jenkins by being triggered as part of a CI/CD pipeline to handle deployment and configuration tasks.
    - Jenkins runs Ansible playbooks (via plugin or command line) after build or test stages, automating tasks like code deployment, server configuration, or service restarts.
    - This integration ensures continuous delivery, consistency, and zero manual intervention across environments.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

23. What are some ways to optimize Ansible Playbook performance?
    - You can optimize Ansible Playbook performance by increasing parallelism using forks, disabling unnecessary fact gathering, using SSH pipelining, and reusing connections.
    - Also, use tags, async tasks, and fact caching to avoid redundant operations and speed up execution.
    - Breaking playbooks into roles and limiting target hosts also helps improve performance and manageability.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

24. How do you test Ansible Playbooks before production deployment?
    - I test Ansible playbooks using syntax check (--syntax-check), dry-run mode (--check), and Ansible Lint to catch issues early.
    - Then I run them in a staging environment or use Molecule for role testing.
    - In CI/CD, I integrate these checks with Jenkins so that playbooks are automatically validated before production deployment.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

25. How do you perform rolling updates with Ansible?
    - Rolling updates in Ansible are done using the serial keyword, which controls how many hosts are updated at a time.
    - For example, setting serial: 2 updates two servers at once, ensuring high availability.
    - Often, we combine it with pre_tasks (to remove servers from the load balancer) and post_tasks (to add them back), achieving zero-downtime deployments safely.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

26. What is the difference between Ansible Galaxy and Ansible Tower?
    - Ansible Galaxy is a community hub where users share and download Ansible roles and collections to reuse automation content.
    - Ansible Tower is an enterprise tool that provides a web interface, access control, scheduling, and monitoring for running and managing Ansible playbooks.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

27. What are Ansible Collections?
    - Ansible Collections are a packaging format used to distribute and organize Ansible content — such as roles, modules, plugins, and playbooks — in a single, reusable bundle.
    - Collections make it easier to share, version, and manage Ansible content, especially for large projects or when working with automation content from different vendors (like AWS,
      Cisco, or Red Hat).

   collection/
├── ansible_collections/
│   └── my_namespace/
│       └── my_collection/
│           ├── roles/
│           ├── playbooks/
│           ├── plugins/
│           ├── docs/
│           └── README.md


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

28. Explain a real-world use case where you automated infrastructure or application deployment using Ansible.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

29. How do you use Ansible Tower (AWX) for enterprise automation?