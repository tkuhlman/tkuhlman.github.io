---
layout: post
title:  "Ansible Utilities"
tags:
  - Ansible
  - configuration management
  - scripts
---
In the last few weeks I have been immersed in [Ansible](http://www.ansible.com). I have been using Ansible with [Vagrant](https://github.com/stackforge/monasca-vagrant),
with [Docker](https://github.com/hpcloud-mon/monasca-docker) and in a test environment on bare metal. Those are all topics I may explore in more depth later but
what really is conspicuous today is the way I can build simple utilities with Ansible.

My collection of Ansible [scripts](https://github.com/tkuhlman/ansible-utils) which aren't part of a larger set of playbooks is still quite small but I feel it
is but the tip of the iceberg. In the last few years I have built a fair collection of [Fabric](http://www.fabfile.org/en/latest/) scripts for
various common tasks. These have been highly useful and I am will continue to use Fabric especially for tasks needing the more complicated logic and control structures
possible using the full power of Python. Ansible also has a future in my tool set, in particular for tasks where modules exist.

With Ansible there are a couple of advantages which make it useful for the small orchestration tasks. It uses the same inventory already setup for configuration
management, making it quite easy to target to the right boxes. The organization structures within Ansible make it is easy to mix and match tasks. Additionally it is a tool
already being used so there is no new learning curve for my colleagues. The biggest advantage is the functionality encapsulated in pre-built modules,
in particular the idempotency these bring.

Take as an example my first little [utility](https://github.com/tkuhlman/ansible-utils/blob/master/setup_user.yml), I was already immersed in Ansible so no
mental context switch was needed and I was able to write it quickly. It is simple and quickly written but immediately saved me some time. I could have fairly
easily written this is Fabric also but it would have taken me much longer if I wanted to make an equivalent Fabric script idempotent. In my first run idempotency
isn't needed but it gives peace of mind because there is no danger if I accidentally run it twice. This will be especially helpful a month or two from now when
I have forgotten where I ran it.
