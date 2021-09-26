## Why I wrote this page
This is a reference for my future self, but also for anyone I work with or who may find it useful that covers my workflow for getting my Python programmes to users or servers.

In general, I have two target use-cases when I'm writing code:

- I'm providing a utility for users to use on their workstations - generally Windows PCs
- I'm proving software to be run as a service on a server, often Windows but sometimes Linux

These two use-cases share some similarities:

- I can't guarantee the version of Python (or eventhe existence of Python) on the target system
- I don't want the installer or user to need to know any Python or to do lots of setup work, it needs to just install and work
- I don't want the deployed code to be scattered around non-obvious places, ideally everything should be in one directory so it can be uninstalled simply by deleting that directory

In the case of deployment to a server there is an important addition to the third point above:

- The installed software must not have any dependence on the continued existence of the user account by which it was installed.

[My dev environment](dev.md)