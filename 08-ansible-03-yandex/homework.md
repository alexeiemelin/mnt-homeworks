# Домашнее задание к занятию "08.03 Использование Yandex Cloud"

## Подготовка к выполнению

1. Подготовьте в Yandex Cloud три хоста: для `clickhouse`, для `vector` и для `lighthouse`.

## Основная часть

1. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает lighthouse.
2. При создании tasks рекомендую использовать модули: `get_url`, `template`, `yum`, `apt`.
3. Tasks должны: скачать статику lighthouse, установить nginx или любой другой webserver, настроить его конфиг для открытия lighthouse, запустить webserver.
4. Приготовьте свой собственный inventory файл `prod.yml`.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
^C [ERROR]: User interrupted execution
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
The authenticity of host '51.250.76.88 (51.250.76.88)' can't be established.
ECDSA key fingerprint is SHA256:StuMUZ6sAvjtkYnGjoRUfVxuxayf5WtzqXGILqnCKBA.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
ok: [clickhouse-1]

TASK [Get clickhouse distrib] ********************************************************************************************************************
changed: [clickhouse-1] => (item=clickhouse-client)
changed: [clickhouse-1] => (item=clickhouse-server)
failed: [clickhouse-1] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "item": "clickhouse-common-static", "msg": "Request failed", "response": "HTTP Error 404: Not Found", "status_code": 404, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
changed: [clickhouse-1]

TASK [Install clickhouse packages] ***************************************************************************************************************
changed: [clickhouse-1]

TASK [clickhouse | Create clickhouse config] *****************************************************************************************************
changed: [clickhouse-1]

TASK [Flush handlers] ****************************************************************************************************************************

RUNNING HANDLER [Start clickhouse service] *******************************************************************************************************
changed: [clickhouse-1]

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
The authenticity of host '51.250.10.89 (51.250.10.89)' can't be established.
ECDSA key fingerprint is SHA256:cPc/zWoGXTDqwIUhd5zM7YpAuj0dQcaCFhokMmxrcmQ.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
ok: [vector-1]

TASK [Get Vector distrib] ************************************************************************************************************************
changed: [vector-1]

TASK [Install Vector] ****************************************************************************************************************************
changed: [vector-1]

TASK [Vector | Template config] ******************************************************************************************************************
changed: [vector-1]

TASK [Vector | Create systed unit] ***************************************************************************************************************
changed: [vector-1]

TASK [Vector | Start Service] ********************************************************************************************************************
changed: [vector-1]

PLAY [Install nginx] *****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
The authenticity of host '51.250.95.136 (51.250.95.136)' can't be established.
ECDSA key fingerprint is SHA256:3iglVEtrL58/2VEu42ZOW8wyMS94IxYn+ZG2F3JNwlU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
ok: [lighthouse-1]

TASK [NGINX | Install epel-release] **************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Install nginx] *********************************************************************************************************************
changed: [lighthouse-1]

TASK [nginx | Create general config] *************************************************************************************************************
changed: [lighthouse-1]

RUNNING HANDLER [start-nginx] ********************************************************************************************************************
changed: [lighthouse-1]

RUNNING HANDLER [reload-nginx] *******************************************************************************************************************
changed: [lighthouse-1]

PLAY [Install Lighthouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [Lighthouse | Install dependencies] *********************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Copy from git] ****************************************************************************************************************
changed: [lighthouse-1]

TASK [lighthouse | Create lighthouse config] *****************************************************************************************************
changed: [lighthouse-1]

RUNNING HANDLER [reload-nginx] *******************************************************************************************************************
changed: [lighthouse-1]

PLAY RECAP ***************************************************************************************************************************************
clickhouse-1               : ok=5    changed=4    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-1               : ok=11   changed=7    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-1                   : ok=6    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
5. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-lint site.yml
```
6. Попробуйте запустить playbook на этом окружении с флагом `--check`.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook -i inventory/prod.yml site.yml --check

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [clickhouse-1]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1] => (item=clickhouse-client)
ok: [clickhouse-1] => (item=clickhouse-server)
failed: [clickhouse-1] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "centos", "item": "clickhouse-common-static", "mode": "0664", "msg": "Request failed", "owner": "centos", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [clickhouse-1]

TASK [clickhouse | Create clickhouse config] *****************************************************************************************************
ok: [clickhouse-1]

TASK [Flush handlers] ****************************************************************************************************************************

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [vector-1]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [vector-1]

TASK [Install Vector] ****************************************************************************************************************************
ok: [vector-1]

TASK [Vector | Template config] ******************************************************************************************************************
ok: [vector-1]

TASK [Vector | Create systed unit] ***************************************************************************************************************
ok: [vector-1]

TASK [Vector | Start Service] ********************************************************************************************************************
changed: [vector-1]

PLAY [Install nginx] *****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [NGINX | Install epel-release] **************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Install nginx] *********************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Create general config] *************************************************************************************************************
ok: [lighthouse-1]

PLAY [Install Lighthouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [Lighthouse | Install dependencies] *********************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Copy from git] ****************************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Create lighthouse config] *****************************************************************************************************
ok: [lighthouse-1]

PLAY RECAP ***************************************************************************************************************************************
clickhouse-1               : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-1               : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-1                   : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
7. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [clickhouse-1]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1] => (item=clickhouse-client)
ok: [clickhouse-1] => (item=clickhouse-server)
failed: [clickhouse-1] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "centos", "item": "clickhouse-common-static", "mode": "0664", "msg": "Request failed", "owner": "centos", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [clickhouse-1]

TASK [clickhouse | Create clickhouse config] *****************************************************************************************************
ok: [clickhouse-1]

TASK [Flush handlers] ****************************************************************************************************************************

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [vector-1]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [vector-1]

TASK [Install Vector] ****************************************************************************************************************************
ok: [vector-1]

TASK [Vector | Template config] ******************************************************************************************************************
ok: [vector-1]

TASK [Vector | Create systed unit] ***************************************************************************************************************
ok: [vector-1]

TASK [Vector | Start Service] ********************************************************************************************************************
changed: [vector-1]

PLAY [Install nginx] *****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [NGINX | Install epel-release] **************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Install nginx] *********************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Create general config] *************************************************************************************************************
ok: [lighthouse-1]

PLAY [Install Lighthouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [Lighthouse | Install dependencies] *********************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Copy from git] ****************************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Create lighthouse config] *****************************************************************************************************
ok: [lighthouse-1]

PLAY RECAP ***************************************************************************************************************************************
clickhouse-1               : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-1               : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-1                   : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
8. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook -i inventory/prod.yml site.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [clickhouse-1]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1] => (item=clickhouse-client)
ok: [clickhouse-1] => (item=clickhouse-server)
failed: [clickhouse-1] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 1000, "group": "centos", "item": "clickhouse-common-static", "mode": "0664", "msg": "Request failed", "owner": "centos", "response": "HTTP Error 404: Not Found", "secontext": "unconfined_u:object_r:user_home_t:s0", "size": 246310036, "state": "file", "status_code": 404, "uid": 1000, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [clickhouse-1]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [clickhouse-1]

TASK [clickhouse | Create clickhouse config] *****************************************************************************************************
ok: [clickhouse-1]

TASK [Flush handlers] ****************************************************************************************************************************

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [vector-1]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [vector-1]

TASK [Install Vector] ****************************************************************************************************************************
ok: [vector-1]

TASK [Vector | Template config] ******************************************************************************************************************
ok: [vector-1]

TASK [Vector | Create systed unit] ***************************************************************************************************************
ok: [vector-1]

TASK [Vector | Start Service] ********************************************************************************************************************
changed: [vector-1]

PLAY [Install nginx] *****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [NGINX | Install epel-release] **************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Install nginx] *********************************************************************************************************************
ok: [lighthouse-1]

TASK [nginx | Create general config] *************************************************************************************************************
ok: [lighthouse-1]

PLAY [Install Lighthouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [lighthouse-1]

TASK [Lighthouse | Install dependencies] *********************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Copy from git] ****************************************************************************************************************
ok: [lighthouse-1]

TASK [lighthouse | Create lighthouse config] *****************************************************************************************************
ok: [lighthouse-1]

PLAY RECAP ***************************************************************************************************************************************
clickhouse-1               : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
lighthouse-1               : ok=8    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
vector-1                   : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```
9. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
10. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-03-yandex` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
