---
layout: post
title:  "Advanced Ansible"
tags:
  - Ansible
  - configuration management
---

It would be an mistake to call me an Ansible expert but I am now an experienced Ansible user and it is time to expand on my earlier [Ansible posts](/tags/#Ansible).
I have now been using Ansible on a regular basis for awhile and have used it with vagrant based vms, docker containers, as well as across small clusters of machines.
I have also written a couple of Ansible modules and will likely write another one or two soon. Writing modules is a topic that I will address in
another post.

As I use Ansible in various environments I continue to enjoy the simplicity of its model. I find it easy to run against a wide variety of deployment
models by taking existing playbooks and tweaking them slightly or combining them in new ways. This flexibility has been key for my use cases lately as I can
work on making my roles and modules robust and then easily use them for small installs, clusters, one or os or another and anything else needed.

I have found that rolling deploys with deployment verification is much easier in Ansible then with many other configuration management tools I have previously used.
Targeting a subset of hosts is as simple as setting the correct serial value in the playbook or even targeting groups of hosts over multiple subsequent runs. Next
you just need to add in deployment verification. For most of the services I have setup that is as simple as a [wait_for](http://docs.ansible.com/wait_for_module.html)
task, though for some the [assert](http://docs.ansible.com/assert_module.html) module is a better option. Either option is easy to implement.

One aspect of Ansible that needs some work is its ability to finishing running triggered handlers on failure. I have encountered many situations where we are running
a play with multiple roles and earlier roles register handlers which never run because a later role fails and handlers have not yet been flushed. In the case of a
restarted service this is quite annoying especially as the next run won't trigger handlers and if I put both the handler and a service start task in some instances
the service will restart twice. There is a work around, basically first flush handlers then define the service task with a state of started. This works well but
it is a bit annoying that I have to remember to explicitly flush handlers for every role.

I have hope that the running of queued handlers is something that will be fixed because I am happy to see improvements in Ansible with 1.8 that directly affect my
work. The most notable changes for me were actually in the Ansible Galaxy command. The ability to specify a role requirements file in yaml and point at git repos directly
as well as optionally specify tags is essential. Though I like what is happening with the Ansible Galaxy public site it just isn't always possible to push things up there and
often while waiting for a pull request to be approved it is nice to point to my updated version without making a new entry in galaxy.
I am also tired of seeing tkuhlman prepended to the beginning of my roles in the various playbooks. Though account based differentiation is a good feature for
Galaxy that encourages sharing of tweaks to existing roles I really only want to be aware of it on download, the new format accomplishes exactly that.

Besides continued work with writing modules there are some other aspects of Ansible I still would like to explore. Wider use of assert and lookups as well as
pipelining come to mind. Also, I have not yet had the opportunity to use Ansible across a large cluster of nodes. I have worked with large clusters before and likely
will again fairly soon, I look forward to the learning that will come from running Ansible in such an environment.
