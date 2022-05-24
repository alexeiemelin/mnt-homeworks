# Домашнее задание к занятию "08.02 Работа с Playbook"

## Подготовка к выполнению

1. Создайте свой собственный (или используйте старый) публичный репозиторий на github с произвольным именем.
2. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.
3. Подготовьте хосты в соответствии с группами из предподготовленного playbook.

## Основная часть

1. Приготовьте свой собственный inventory файл `prod.yml`.
2. Допишите playbook: нужно сделать ещё один play, который устанавливает и настраивает [vector](https://vector.dev).
3. При создании tasks рекомендую использовать модули: `get_url`, `template`, `unarchive`, `file`.
4. Tasks должны: скачать нужной версии дистрибутив, выполнить распаковку в выбранную директорию, установить vector.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook site.yml -i inventory/prod.yml

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7] => (item=clickhouse-client)
ok: [centos7] => (item=clickhouse-server)
failed: [centos7] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 0, "group": "root", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "root", "response": "HTTP Error 404: Not Found", "size": 246310036, "state": "file", "status_code": 404, "uid": 0, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [centos7]

TASK [Flush handlers] ****************************************************************************************************************************

TASK [Create database] ***************************************************************************************************************************
ok: [centos7]

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get Vector distrib] ************************************************************************************************************************
changed: [centos7]

TASK [Install Vector] ****************************************************************************************************************************
changed: [centos7]

PLAY RECAP ***************************************************************************************************************************************
centos7                    : ok=7    changed=2    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
```

6. Запустите `ansible-lint site.yml` и исправьте ошибки, если они есть.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-lint site.yml
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ 
```
7. Попробуйте запустить playbook на этом окружении с флагом `--check`.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook site.yml -i inventory/prod.yml --check

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7] => (item=clickhouse-client)
ok: [centos7] => (item=clickhouse-server)
failed: [centos7] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 0, "group": "root", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "root", "response": "HTTP Error 404: Not Found", "size": 246310036, "state": "file", "status_code": 404, "uid": 0, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [centos7]

TASK [Flush handlers] ****************************************************************************************************************************

TASK [Create database] ***************************************************************************************************************************
skipping: [centos7]

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [centos7]

TASK [Install Vector] ****************************************************************************************************************************
ok: [centos7]

PLAY RECAP ***************************************************************************************************************************************
centos7                    : ok=6    changed=0    unreachable=0    failed=0    skipped=1    rescued=1    ignored=0  
```
8. Запустите playbook на `prod.yml` окружении с флагом `--diff`. Убедитесь, что изменения на системе произведены.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7] => (item=clickhouse-client)
ok: [centos7] => (item=clickhouse-server)
failed: [centos7] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 0, "group": "root", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "root", "response": "HTTP Error 404: Not Found", "size": 246310036, "state": "file", "status_code": 404, "uid": 0, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [centos7]

TASK [Flush handlers] ****************************************************************************************************************************

TASK [Create database] ***************************************************************************************************************************
ok: [centos7]

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [centos7]

TASK [Install Vector] ****************************************************************************************************************************
ok: [centos7]

PLAY RECAP ***************************************************************************************************************************************
centos7                    : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0   
```
9. Повторно запустите playbook с флагом `--diff` и убедитесь, что playbook идемпотентен.
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-02-playbook/playbook$ ansible-playbook site.yml -i inventory/prod.yml --diff

PLAY [Install Clickhouse] ************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7] => (item=clickhouse-client)
ok: [centos7] => (item=clickhouse-server)
failed: [centos7] (item=clickhouse-common-static) => {"ansible_loop_var": "item", "changed": false, "dest": "./clickhouse-common-static-22.3.3.44.rpm", "elapsed": 0, "gid": 0, "group": "root", "item": "clickhouse-common-static", "mode": "0644", "msg": "Request failed", "owner": "root", "response": "HTTP Error 404: Not Found", "size": 246310036, "state": "file", "status_code": 404, "uid": 0, "url": "https://packages.clickhouse.com/rpm/stable/clickhouse-common-static-22.3.3.44.noarch.rpm"}

TASK [Get clickhouse distrib] ********************************************************************************************************************
ok: [centos7]

TASK [Install clickhouse packages] ***************************************************************************************************************
ok: [centos7]

TASK [Flush handlers] ****************************************************************************************************************************

TASK [Create database] ***************************************************************************************************************************
ok: [centos7]

PLAY [Install Vector] ****************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [centos7]

TASK [Get Vector distrib] ************************************************************************************************************************
ok: [centos7]

TASK [Install Vector] ****************************************************************************************************************************
ok: [centos7]

PLAY RECAP ***************************************************************************************************************************************
centos7                    : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=1    ignored=0  
```
10. Подготовьте README.md файл по своему playbook. В нём должно быть описано: что делает playbook, какие у него есть параметры и теги.
11. Готовый playbook выложите в свой репозиторий, поставьте тег `08-ansible-02-playbook` на фиксирующий коммит, в ответ предоставьте ссылку на него.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
