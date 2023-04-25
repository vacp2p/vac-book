We have a cloud server with 256 Gb of ram that can be used to run simulations, using tools like [wakurtosis](https://github.com/logos-co/wakurtosis). The goal of this machine is to be able to quickly run some specific tests, experiments or do some prototyping. Your key can be added by opening a PR [here](https://github.com/status-im/infra-misc/blob/master/ansible/group_vars/vacdev.yml) so that you can ssh to the machine. Note that the machine sits behind a firewall so if you need to expose a given port, it must be opened manually, see repo.

Some notes:
* If you are going to use the machine, make sure no one else is using it.
* No formal booking system, just ask in discord.
* When you are done with it, leave it as clean as possible: no stale images running, try to organize folders, etc.

## Running simulations

We have a simple tool [nwaku-simulator](https://github.com/alrevuelta/nwaku-simulator/) that allows running some simple simulations with up to 100/200 nwaku nodes, configured with discv5 and using a bootstrap node to connect to each other. This tool also scraps metrics and exposes them in a grafana dashboard. It can be run with just two commands with a configurable amount of nodes and a given release version. See instructions in repo.