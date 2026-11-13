# Research: Ansible

## 1. Introduction

Ansible is an open source tool for automation and configuration management. Allows you to configure and implements systems also coordinate task in multiple nodes at same time.
Some caracteristic are agentless system, you dont have to install any agent in the node you want to manage, ansible use yamel syntax doing it simple to read and understand.
Ansible works with idempotency, it means, you can execute the ansible instructions how many times you want using inventory, playbooks and config files and the final result will be the same.
You can automate task and even include CI/CD

## 2. Core Concepts

Ansible has an straightforward architecture relies on control and managed nodes

##### 1. Control Node: Its the device where you run ansible resources like playbooks.

##### 2. Managed Nodes: These are the devices you want to managed like servers, switches, cloud clusters, etc. It works via SSH, WinRM, PSRP, etc

##### 3. Inventory: File where you list all managed nodes, could be static one (simple text file with a list of ip addresesss and hostnames) or dynamic one (Script for fetch the list of currently running instances)

##### 4. Modules: Small programs that ansible pushes to managed nodes to perform specific taks. Can be Core modules and custom ones

##### 5. Playbooks: Playbooks are the blueprint of automation, its based on yaml syntax, you define list of tasks to be excecuted, you can also group devices and set variables for them

##### 6. Plugins: Plugins extend Ansible’s core functionality. Like connection plugins or callback ones

##### 7. Idempotency: An idempotent task produces the same result whether you run it once or fifty times. If a system is already in a desired state ansible will not doing anything. If a system isn't in a desire state it will apply only the neccesary changes to reach a desire state

## 3. How It Works

Basic workflow is:

1. Create inventory: Define IP Adresses of the host you want
2. Write Playbook: Define the desired state of those hosts in a YAML file.
3. Execute: Run the playbook using the ansible-playbook command.
4. Connect & Push: Ansible connects to the target nodes (via SSH/WinRM).
5. Fact Gathering: Ansible gathers system details (IP, OS version, CPU) from the target; these are called "Facts."
6. Execute Modules: Ansible executes the modules defined in the playbook on the remote node.
7. Clean Up: Ansible removes the modules after execution and reports the status (Changed, Failed, or OK)

## 4. Practical Applications

#### Cloud infrastructure provisioning

Organizations use ansible for setup cloud resources in an automated way in platforms like aws, azure and other providers.

#### Network Device Automation

It uses for automate the configuration of network devices like switches, routers and firewalls.

#### Security and Compliance Enforcements

You can automate the security configuration using playbooks to enforce security policies

#### Disaster Recovery Automation

Restoration of services by executing predifined playbooks could restore configuration and reinstall software minimizing downtime

#### CI/CD

With ansible you can automate task of CI/CD pipelines

## 5. Advantages and Limitations

Advantages are clear:

    - Simple to learn
    - No dependency on agents
    - Playbooks using yml syntax
    - Ansible Galaxy -> Its a portal that acts as central repository for locating, reusing and sharing ansible-related content

But disadvantaed are clear too:

    - Learning Curve: Mastering advanced features and best practices may require some learning. Understanding concepts such as Jinja templating, roles, and playbooks’ intricacies might take time for beginners.
    
    - Statelessness: Ansible doesn’t inherently track the state of managed systems beyond the execution of tasks. This can be a disadvantage for scenarios requiring detailed state tracking and management, which are better addressed by tools like Terraform.

    - Scalability: Ansible is designed to scale, managing very large infrastructures with thousands of nodes can become challenging. The lack of built-in state tracking and the inherent overhead of SSH connections might lead to performance issues in large-scale deployments.

    - Limited Parallelism: Ansible allows parallel execution of tasks, it may not fully leverage the available resources on very large infrastructures. This limitation can impact performance in scenarios where rapid execution is critical.

    - Complexity of Tasks: Ansible simplifies automation, complex tasks might require writing intricate playbooks or using external tools and scripts. Managing such complexity can become challenging, especially for users with limited programming experience.

    - Community Modules Quality: Ansible’s community provides a vast array of modules, the quality and reliability of these modules can vary. Users might encounter issues with community-contributed modules, necessitating careful evaluation and testing before production use.

    - Performance Overhead: Ansible’s agentless architecture relies on SSH connections for communication, which can introduce overhead, especially in environments with strict security policies or high-latency networks.

## 6. Comparison and Alternatives

| Name | Pros | Cons | Scenarios |
| - | - | - |
| Puppet | Strong for complex enviroments, Idempotency, Scalability, Reporting | High Learning Curve, Agent-Based, Resource Intensive, Slower Executions | You can use it in complex infrastructures with well-defined states |
| Chef | Flexibility and power, Test-Driven approach, Strong Community, Mature and Feature-Rich | Complexity, High Learning curve, Agent-Based, Resource Intensive | Use it when the infrastructure is masive or more complex |


## 7. Application in ForgeOps

ForgeOps will use ansible for set an automated configuration and instructions for common and normalized infrastructure and security.
Deps and packet installation, creation and configure of users, Manage of resources, SSH connections

## 8. Personal Conclusions

Ansible is a powerful tool for managing and configure multiple devices at same time, reduce the time for set up enviroments and downtime task. Easier to understand and learn, i understood how you can reduce human error in procceses and make operations reproducible

## 9. References

- [Inside Tech - Ansible](https://www.insidetech.com.ar/redhat?gad_source=1&gad_campaignid=23183138110&gbraid=0AAAAAD963JejciTRM4Yc7q1rRmayPDb7C&gclid=Cj0KCQiAh9vXBhDKARIsAMooYhecqvfnH1C2MqvOXoRV3I2166FtjVilCXSajRh_izuWseJRarO5NjcaAvpqEALw_wcB)
- [Geeks for Geeks](https://www.geeksforgeeks.org/devops/introduction-to-ansible-and-its-architecture-components/)
- [WHIZLABS](https://www.whizlabs.com/blog/ansible-advantages-and-disadvantages/)
- [IDEMPOTENCY](https://oneuptime.com/blog/post/2026-02-21-how-to-write-idempotent-ansible-tasks/view)
- [COMPARISIONS](https://www.automq.com/blog/ansible-alternatives-2025-terraform-chef-salt-puppet-cfengine)
- [UpGuard - Pros and Cons](https://www.upguard.com/blog/top-5-best-and-worst-attributes-of-ansible)