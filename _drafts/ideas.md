# Potential Ideas
- Documentation driven development
- Balancing collaboration and engineering. Steering toward synergy without going to far either direction.
- Vagrant love.
- Building packages. Various ways of doing debs, pythong packages, etc.
- git
  - workflow, lots of repos versus few, git subtree versus git submodules
- Docker
  - Ansible + docker how I both build and deploy with Ansible.
    - Annoyances with docker, include that the service startups fail with the newer init replacements so my roles have to be modified.
    - The building of the container is split from the running so I find myself having to change roles so some things only happen when on startup. Not that
      this is unique to containers but has annoyed me.
  - Image deployment, why it works for Docker and why the industry moved away from it years ago for servers.
  - The ~/bin/docker-tips and ~/bin/docker-cleanup should be mentioned. Really just the mess it can become to manage many containers. Ansible
    really helps out here so mention that.
  - the docker orchestration toolkit, machine, swarm and compose.
- The next big challenge is the management of large amounts of containers.
  - Lots of contenders are entering this space perhaps do a review and comparison of them.
  - kubernetes, flocker, even things like apache mesos and to some extent juju fit into this.
- Something golang related.
- Markdown - use with github and readthedocs

## Non-technical ideas
- Architects who don't know details are impediments to progress rather than enablers.
- The importance of running the service you work on.
  - It proves the product.
  - It bring fast feedback loops that help you to improve the product, as you get immediate clarity on bugs, needed features and annoyances.
  - Reveals scale and operational issues that are difficult to discover any other way.
  - It helps developers truly understand the use cases.
  - Coupled with ci/cd you improve development speed and quality through smaller more automated deploys
  - Increases developer satisfaction through allowing a developer to work hard on something, deploy it and immediately see it used in
    a real installation.
  - Leverages the motivation that naturally comes from being on call and responsible.
- Teams and tasks need to be vertical slices not component/tech based.
  - A team is best with one set of goals but multiple sets of expertise. Ie make product X is the goal, the skills include back end code, ops, ui,
    testing, etc.
  - When breaking down team tasks epics should be vertical slices crossing across components not focused on a component. A focus on a component
    tends to me tunnel vision, missing the big picture, etc.
