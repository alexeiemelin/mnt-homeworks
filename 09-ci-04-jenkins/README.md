# Домашнее задание к занятию "09.04 Jenkins"

## Подготовка к выполнению

1. Создать 2 VM: для jenkins-master и jenkins-agent.
2. Установить jenkins при помощи playbook'a.
3. Запустить и проверить работоспособность.
4. Сделать первоначальную настройку.

## Основная часть

Ответ: Правильно ли я понимаю, что в jenkins нужно обновить python с 3.6 до 3.8 чтобы тест роли запустился?
Если да, то подскажите как это сделать корректно.

```bash
23:12:16 Started by user Alexei Emelin
23:12:16 Running as SYSTEM
23:12:16 Building remotely on netology (centos7) in workspace /opt/jenkins_agent/workspace/netology-freestyle
23:12:16 The recommended git tool is: NONE
23:12:16 using credential c2ffa8f5-96a5-4bb9-9b7a-49cd3590c78e
23:12:16  > git rev-parse --resolve-git-dir /opt/jenkins_agent/workspace/netology-freestyle/.git # timeout=10
23:12:16 Fetching changes from the remote Git repository
23:12:16  > git config remote.origin.url https://github.com/alexeiemelin/vector-molecule.git # timeout=10
23:12:17 Fetching upstream changes from https://github.com/alexeiemelin/vector-molecule.git
23:12:17  > git --version # timeout=10
23:12:17  > git --version # 'git version 1.8.3.1'
23:12:17 using GIT_SSH to set credentials 
23:12:17 [INFO] Currently running in a labeled security context
23:12:17 [INFO] Currently SELinux is 'enforcing' on the host
23:12:17  > /usr/bin/chcon --type=ssh_home_t /opt/jenkins_agent/workspace/netology-freestyle@tmp/jenkins-gitclient-ssh899990640709105240.key
23:12:17 Verifying host key using known hosts file, will automatically accept unseen keys
23:12:17  > git fetch --tags --progress https://github.com/alexeiemelin/vector-molecule.git +refs/heads/*:refs/remotes/origin/* # timeout=10
23:12:17  > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
23:12:17 Checking out Revision b529a659d54d5b6b65456a8c12964589c0d505b8 (refs/remotes/origin/main)
23:12:17  > git config core.sparsecheckout # timeout=10
23:12:17  > git checkout -f b529a659d54d5b6b65456a8c12964589c0d505b8 # timeout=10
23:12:17 Commit message: "first commit"
23:12:17  > git rev-list --no-walk b529a659d54d5b6b65456a8c12964589c0d505b8 # timeout=10
23:12:17 [netology-freestyle] $ /bin/sh -xe /tmp/jenkins856936385491965638.sh
23:12:17 + cd vector-role
23:12:17 + molecule --version
23:12:18 /home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.11) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!
23:12:18   RequestsDependencyWarning)
23:12:18 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:18   from cryptography.exceptions import InvalidSignature
23:12:19 molecule 3.5.2 using python 3.6 
23:12:19     ansible:2.10.17
23:12:19     delegated:3.5.2 from molecule
23:12:19     docker:1.1.0 from molecule_docker requiring collections: community.docker>=1.9.1
23:12:19 + molecule test -s default
23:12:20 /home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.11) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!
23:12:20   RequestsDependencyWarning)
23:12:21 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:21   from cryptography.exceptions import InvalidSignature
23:12:21 INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
23:12:21 INFO     Performing prerun...
23:12:21 INFO     Set ANSIBLE_LIBRARY=/home/jenkins/.cache/ansible-compat/5036a8/modules:/home/jenkins/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
23:12:22 INFO     Set ANSIBLE_COLLECTIONS_PATH=/home/jenkins/.cache/ansible-compat/5036a8/collections:/home/jenkins/.ansible/collections:/usr/share/ansible/collections
23:12:22 INFO     Set ANSIBLE_ROLES_PATH=/home/jenkins/.cache/ansible-compat/5036a8/roles:/home/jenkins/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
23:12:22 INFO     Running default > dependency
23:12:22 WARNING  Skipping, missing the requirements file.
23:12:22 WARNING  Skipping, missing the requirements file.
23:12:22 INFO     Running default > lint
23:12:22 INFO     Lint is disabled.
23:12:22 INFO     Running default > cleanup
23:12:22 WARNING  Skipping, cleanup playbook not configured.
23:12:22 INFO     Running default > destroy
23:12:22 INFO     Sanity checks: 'docker'
23:12:23 
23:12:23 PLAY [Destroy] *****************************************************************
23:12:23 
23:12:23 TASK [Destroy molecule instance(s)] ********************************************
23:12:23 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:23   from cryptography.exceptions import InvalidSignature
23:12:24 changed: [localhost] => (item=centos7)
23:12:24 
23:12:24 TASK [Wait for instance(s) deletion to complete] *******************************
23:12:25 FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
23:12:30 ok: [localhost] => (item=centos7)
23:12:30 
23:12:30 TASK [Delete docker networks(s)] ***********************************************
23:12:30 
23:12:30 PLAY RECAP *********************************************************************
23:12:30 localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
23:12:30 
23:12:30 INFO     Running default > syntax
23:12:31 
23:12:31 playbook: /opt/jenkins_agent/workspace/netology-freestyle/vector-role/molecule/default/converge.yml
23:12:31 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:31   from cryptography.exceptions import InvalidSignature
23:12:31 INFO     Running default > create
23:12:32 
23:12:32 PLAY [Create] ******************************************************************
23:12:32 
23:12:32 TASK [Log into a Docker registry] **********************************************
23:12:32 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:32   from cryptography.exceptions import InvalidSignature
23:12:32 skipping: [localhost] => (item=None)
23:12:32 skipping: [localhost]
23:12:32 
23:12:32 TASK [Check presence of custom Dockerfiles] ************************************
23:12:33 ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
23:12:33 
23:12:33 TASK [Create Dockerfiles from image names] *************************************
23:12:33 skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
23:12:33 
23:12:33 TASK [Discover local Docker images] ********************************************
23:12:34 ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
23:12:34 
23:12:34 TASK [Build an Ansible compatible image (new)] *********************************
23:12:34 skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
23:12:34 
23:12:34 TASK [Create docker network(s)] ************************************************
23:12:34 
23:12:34 TASK [Determine the CMD directives] ********************************************
23:12:34 ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
23:12:34 
23:12:34 TASK [Create molecule instance(s)] *********************************************
23:12:35 changed: [localhost] => (item=centos7)
23:12:35 
23:12:35 TASK [Wait for instance(s) creation to complete] *******************************
23:12:36 FAILED - RETRYING: Wait for instance(s) creation to complete (300 retries left).
23:12:42 failed: [localhost] (item={'started': 1, 'finished': 0, 'ansible_job_id': '568395291009.5092', 'results_file': '/home/jenkins/.ansible_async/568395291009.5092', 'changed': True, 'failed': False, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "568395291009.5092", "ansible_loop_var": "item", "attempts": 2, "changed": false, "finished": 1, "item": {"ansible_job_id": "568395291009.5092", "ansible_loop_var": "item", "changed": true, "failed": false, "finished": 0, "item": {"image": "docker.io/pycontribs/centos:7", "name": "centos7", "pre_build_image": true}, "results_file": "/home/jenkins/.ansible_async/568395291009.5092", "started": 1}, "msg": "Unsupported parameters for (community.docker.docker_container) module: command_handling Supported parameters include: api_version, auto_remove, blkio_weight, ca_cert, cap_drop, capabilities, cgroup_parent, cleanup, client_cert, client_key, command, comparisons, container_default_behavior, cpu_period, cpu_quota, cpu_shares, cpus, cpuset_cpus, cpuset_mems, debug, default_host_ip, detach, device_read_bps, device_read_iops, device_requests, device_write_bps, device_write_iops, devices, dns_opts, dns_search_domains, dns_servers, docker_host, domainname, entrypoint, env, env_file, etc_hosts, exposed_ports, force_kill, groups, healthcheck, hostname, ignore_image, image, init, interactive, ipc_mode, keep_volumes, kernel_memory, kill_signal, labels, links, log_driver, log_options, mac_address, memory, memory_reservation, memory_swap, memory_swappiness, mounts, name, network_mode, networks, networks_cli_compatible, oom_killer, oom_score_adj, output_logs, paused, pid_mode, pids_limit, privileged, published_ports, pull, purge_networks, read_only, recreate, removal_wait_timeout, restart, restart_policy, restart_retries, runtime, security_opts, shm_size, ssl_version, state, stop_signal, stop_timeout, sysctls, timeout, tls, tls_hostname, tmpfs, tty, ulimits, user, userns_mode, uts, validate_certs, volume_driver, volumes, volumes_from, working_dir", "stderr": "/home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.11) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!\n  RequestsDependencyWarning)\n/home/jenkins/.local/lib/python3.6/site-packages/paramiko/transport.py:33: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.\n  from cryptography.hazmat.backends import default_backend\n", "stderr_lines": ["/home/jenkins/.local/lib/python3.6/site-packages/requests/__init__.py:104: RequestsDependencyWarning: urllib3 (1.26.11) or chardet (5.0.0)/charset_normalizer (2.0.12) doesn't match a supported version!", "  RequestsDependencyWarning)", "/home/jenkins/.local/lib/python3.6/site-packages/paramiko/transport.py:33: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.", "  from cryptography.hazmat.backends import default_backend"]}
23:12:42 
23:12:42 PLAY RECAP *********************************************************************
23:12:42 localhost                  : ok=4    changed=1    unreachable=0    failed=1    skipped=4    rescued=0    ignored=0
23:12:42 
23:12:42 CRITICAL Ansible return code was 2, command was: ['ansible-playbook', '--inventory', '/home/jenkins/.cache/molecule/vector-role/default/inventory', '--skip-tags', 'molecule-notest,notest', '/home/jenkins/.local/lib/python3.6/site-packages/molecule_docker/playbooks/create.yml']
23:12:42 WARNING  An error occurred during the test sequence action: 'create'. Cleaning up.
23:12:42 INFO     Running default > cleanup
23:12:42 WARNING  Skipping, cleanup playbook not configured.
23:12:42 INFO     Running default > destroy
23:12:43 
23:12:43 PLAY [Destroy] *****************************************************************
23:12:43 
23:12:43 TASK [Destroy molecule instance(s)] ********************************************
23:12:43 /usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography and will be removed in a future release.
23:12:43   from cryptography.exceptions import InvalidSignature
23:12:44 changed: [localhost] => (item=centos7)
23:12:44 
23:12:44 TASK [Wait for instance(s) deletion to complete] *******************************
23:12:45 FAILED - RETRYING: Wait for instance(s) deletion to complete (300 retries left).
23:12:50 ok: [localhost] => (item=centos7)
23:12:50 
23:12:50 TASK [Delete docker networks(s)] ***********************************************
23:12:50 
23:12:50 PLAY RECAP *********************************************************************
23:12:50 localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
23:12:50 
23:12:50 INFO     Pruning extra files from scenario ephemeral directory
23:12:50 Build step 'Execute shell' marked build as failure
23:12:50 Finished: FAILURE
```

1. Сделать Freestyle Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.
2. Сделать Declarative Pipeline Job, который будет запускать `molecule test` из любого вашего репозитория с ролью.
3. Перенести Declarative Pipeline в репозиторий в файл `Jenkinsfile`.
4. Создать Multibranch Pipeline на запуск `Jenkinsfile` из репозитория.
5. Создать Scripted Pipeline, наполнить его скриптом из [pipeline](./pipeline).
6. Внести необходимые изменения, чтобы Pipeline запускал `ansible-playbook` без флагов `--check --diff`, если не установлен параметр при запуске джобы (prod_run = True), по умолчанию параметр имеет значение False и запускает прогон с флагами `--check --diff`.
7. Проверить работоспособность, исправить ошибки, исправленный Pipeline вложить в репозиторий в файл `ScriptedJenkinsfile`. Цель: получить собранный стек ELK в Ya.Cloud.
8. Отправить две ссылки на репозитории в ответе: с ролью и Declarative Pipeline и c плейбукой и Scripted Pipeline.

## Необязательная часть

1. Создать скрипт на groovy, который будет собирать все Job, которые завершились хотя бы раз неуспешно. Добавить скрипт в репозиторий с решеним с названием `AllJobFailure.groovy`.
2. Дополнить Scripted Pipeline таким образом, чтобы он мог сначала запустить через Ya.Cloud CLI необходимое количество инстансов, прописать их в инвентори плейбука и после этого запускать плейбук. Тем самым, мы должны по нажатию кнопки получить готовую к использованию систему.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
