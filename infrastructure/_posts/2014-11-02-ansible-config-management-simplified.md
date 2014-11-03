---
layout: post
title:  "Ansible - Configuration Management Simplifed"
tags:
  - Ansible
  - configuration management
---
In the last couple of weeks I have been converting [Monasca](https://wiki.openstack.org/wiki/Monasca) from [Chef](https://www.getchef.com/)
to [Ansible](http://www.ansible.com/).
This has begun with the [monasca-vagrant](https://github.com/stackforge/monasca-vagrant) development environment. Having worked with
Chef for the last 3 years as I learn Ansible I inevitably evaluate it in comparision to Chef.  

The core quality of Ansible compared to Chef is Simplicity. Ansible attempts to solve many of the same problems as Chef but with an
approach that seems born of real world experience and eliminates many theoretically superior design paradigms for practical alternatives.

The most readily apparent simplification between Chef and Ansible is the lack of both an agent and a server in the Ansible model.
Many simplifications flow from this fundamental design decision, including:

- Extremely simple startup. There is no need to install a server or agents or even learn the depths of the tools paradigm before starting. In
practice this is fairly important as few start with overhauling the entire configuration management of a service as I did but rather
start with a single aspect and incrementally grow their coverage.
- Encryption of data becomes much simpler as there is no need to take a shared secret and distribute to
the agent on each box. Additionally unlike Chef there is no need to modify the recipes to use encrypted data, the decrypted data
is just treated as normal variables during an Ansible run
- With all runs potentially starting on a workstation Ansible is easily enabled for orchestration tasks as well as configuration management.
Fundamentally there is little difference between the work done by Ansible to enable each use case. This allows tool simplification, no need for an additional
orchestration layer.
- Related to the enabling of orchestration tasks the simplicity of running plays via Ansible means it is easy to activate Ansible
within larger scripts. I for example use a deployment script based on [Fabric](http://www.fabfile.org/en/latest/) which interacts with
git as well as some in house tools as it deploys. The script already relies on an ssh key for the git aspects and so adding in the
triggering of an Ansible run is extremely easy. When using chef with the script the proper triggering of a run is much more
complicated as I not only have to upload to the server I also have to navigate an entirely separate authentication system. Ansible covers
many of the same use case as fabric so I could probably rewrite much if not all of the script in Ansible but I don't need to,
and can choose to do so or not later. This ability to be a self contained layer that is easy to interact with a key feature of a truly
useful model that should not be understated.
- SSH authentication management is required for Ansible and a at first glance seems to be a disadvantage but in all environments
I have worked with it was already happening. Given that the end result of avoiding the need for another authentication/authorization
system is an advantage for this simplified model.

Ansible's lack of an agent or server is perhaps a factor in why regular unattended runs of Ansible are not built into the system.
If you want these to happen you must add additional
scripting around Ansible, via a cron job or a more sophisticated tool such as [Ansible Tower](http://www.ansible.com/tower). This
is in contrast to Chef where unattended runs are trivial to enable and take advantage of. It is also a
great example of a theoretical deficiency on the side of Ansible which in practical terms isn't. In all the infrastructures I have
worked with the professionalism of administrators has been sufficient that I can't recall an instance where the unattended chef runs were
actually useful. Perhaps I am fortunate regarding the people I have worked with but regardless requiring unattended Ansible runs to use
another layer of abstraction is not much of a negative. Layers of abstraction are fundamental to computing and when used well they simplify
the solution rather than complicate it, I believe this is such a case.

As one who has used Chef for years to deploy various software one of the biggest areas where Ansible's simplicity shines is the variables.
In Ansible there are variables with a just
[5 layers of precedence](http://docs.ansible.com/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)
there is no merging of dictionaries or lists across those layers. In contrast in Chef there are attributes as well as data bags, two distinct types.
For each type the scope can vary with data bags being across the entire chef server and attributes either having an environment, node or run scope.
Additionally there are multiple types of attributes and during a run there is a list of
[15 levels of precedence](https://docs.getchef.com/chef_overview_attributes.html#attribute-precedence). Lastly as the attributes for a run are built
they merge in certain ways for the final product. The Ansible variables have in my experience been quite sufficient to get the job done and are
a welcome relief from the headache of Chef attributes, data bags and encrypted data bags.

Ansible is simpler in that it uses a yaml configuration for plays rather than Chef's extensions to ruby with a fully capable ruby interpreter
running. Though this perhaps makes it easier to approach Ansible for a non-programmer this is one instance where perhaps simplicity isn't an advantage.
Chef has far more capability in its cookbooks, a large advantage in some situations, however there is no need to use the ruby aspects of Chef if they are not needed,
so many situations are kept simpler. In Ansible loops and conditionals are a bit cumbersome and more complicated combinations are either downright atrocious
or best done utilizing multiple runs. In short the lack of code like control structures makes the implementation more complicated. On
the plus side multiple runs in Ansible are much easier to accomplish and even multiple plays can sometimes be sufficient. There is also a big catch in my argument,
I have yet to investigate creating modules in Ansible, if they are sufficiently quick to implement and powerful then perhaps yaml plus modules will just be
a way of enforcing clear code structure.

My conclusion after converting [monasca-vagrant](https://github.com/stackforge/monasca-vagrant) is that the simplicity of Ansible reflects the type of
trade offs that favor practical usage rather than theoretically superior design. In general Ansible is easier to start a configuration management project
with and quicker to finish, with the result being easier for others to understand. This will make it more accessible for others on my team with
limited time to dedicate to configuration management tasks as well as allow my work to progress more quickly. Further we should be able to consolidate
our orchestration scripts around Ansible, simplifying the knowledge needed to run our toolset.
I have yet to use Ansible for more complicated fully HA deploys, orchestration, nor have I created a Ansible module to extend the capabilities. If in my exploration
of these more advanced topics I am able to retain the simplicity for common installations and continue to find a practical usefulness in the design
decisions, then Ansible will truly be a joy to use and the most useful tool in the field.
