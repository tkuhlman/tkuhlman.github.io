---
layout: post
title:  "Ansible Modules"
tags:
  - Ansible
  - Python
---

Ansible is relatively simple in relation to other configuration management frameworks. This makes it easier to approach and accomplish real work with, especially
for those who don't work in it full time, however there are times when more functionality is needed. Ansible's answer for most of these situations is to
write a module.

I have written a couple of [modules for Monasca](https://github.com/hpcloud-mon/ansible-module-monasca) and doing so was easy, particularly if you know Python.

## Common Python Functions

Though you can write modules in any language there are various functions available for Python that simplify the process.

The documentation for [writing Ansible modules](http://docs.ansible.com/developing_modules.html) is a bit light on some details particularly on using the
Python common functions. The docs mostly encourage you to check out code examples, my initial reaction to this was dread that things were going to get difficult.
Happily I found most of the examples were straight forward and so it was simple enough for someone already familiar with Python.

The common functions make the writing of the modules in Python a simple coding task. Looking back at the modules I have written more lines are dedicated to the documentation
of the module than to the code itself. Among the code a large chunk is dealing with defining the various optional values that can be passed in. I point
this out only to highlight that the common libraries make the code and the logic itself quite simple and even naturally steer toward documentation
driven development.

## Modules for Idempotency

As I have [written previously](infrastructure/2014/11/02/ansible-config-management-simplified.html) loops and conditionals are cumbersome in Ansible. In my
usage I particularly felt this at times I tried to accomplish a task lacking a module and retain idempotency. Looping through a list to check the status and
then conditionally performing operations based on the result is possible in raw Ansible but is more straight forward, flexible and cleaner to implement in a
module.

In Ansible a module is the fundamental mechanism used to accomplish idempotent operations. The ease of implementing idempotency in a module
verses in Ansible directly has more than anything else motivated me to add to my todo list a few modules I would like to write.

## Shared code among modules

The one major complaint I have with the modules I have written is that it is difficult to have code shared between modules. Importing python libraries is straight forward
as well as including code from the Ansible base but code shared between Ansible modules is not possible.

The reason it doesn't work is because Ansible does not execute the module locally but rather copies it to
a remote host. Additionally Ansible is trying to do this with as few operations as possible to keep it performant. Ansible would have to either parse the module
or copy the entire library directory on each run for a shared file to be available.

Though I understand the reasons behind this
behaviour since Ansible handles including libraries from the Ansible core and any included python libraries in the system path are available I found myself expecting
to be able to define my own shared python files. After spending a bit of time looking for a way to have a shared python file shipped with my Ansible modules, I
choose to just copy the shared code to both Ansible modules. That is far from ideal but the amount of shared code was relatively small so the alternative of packing
it into a library to be installed on the remote machine would be more trouble.

Ultimately I have no solution for this so will live with it as a minor annoyance for now.
