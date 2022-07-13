# Question 1: An ansible script that restarts a _docker-compose_ service.

### Assumptions

- Work directory of the application locates at _/work/a_
- Work directory contains _Dockerfile_ and _docker-compose.yml_
- Scenarios
    - _q1_answer_01.yml_: An ansible script that restarts a _docker-compose_
    - _q1_answer_02.yml_: An ansible script that restarts a service called _docker-compose_

#### _q1_answer_01.yml_ scenario description

The ansible script uses the _community.docker.docker_compose_ module to restart the application. the docker community collection is required.
`ansible-galaxy collection install community.docker`

#### _q1_answer_02.yml_ scenario description

In this case, a _docker-compose_ service is installed and managed by _systemd_ so the ansible script uses the _ansible.builtin.systemd_ module to restart a service named _docker-compose_.

#### Prerequisite

_docker.service_ and _docker-compose.service_ are enabled on the startup by
`systemctl enable docker && systemctl enable docker-compose`


```
# /etc/systemd/system/docker-compose.service

[Unit]
Description=docker-compose application service
Requires=docker.service
After=docker.service

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/work/a
ExecStart=/usr/local/bin/docker-compose up -d
ExecStop=/usr/local/bin/docker-compose down
TimeoutStartSec=0

[Install]
WantedBy=multi-user.target
```
