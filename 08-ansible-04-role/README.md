# Домашнее задание к занятию "8.4 Работа с Roles"

## Подготовка к выполнению
1. Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
2. Добавьте публичную часть своего ключа к своему профилю в github.

## Основная часть

Наша основная цель - разбить наш playbook на отдельные roles. Задача: сделать roles для clickhouse, vector и lighthouse и написать playbook для использования этих ролей. Ожидаемый результат: существуют три ваших репозитория: два с roles и один с playbook.

1. Создать в старой версии playbook файл `requirements.yml` и заполнить его следующим содержимым:

   ```yaml
   ---
     - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
       scm: git
       version: "1.11.0"
       name: clickhouse 
   ```

2. При помощи `ansible-galaxy` скачать себе эту роль.
3. Создать новый каталог с ролью при помощи `ansible-galaxy role init vector-role`.
4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`. 
5. Перенести нужные шаблоны конфигов в `templates`.
6. Описать в `README.md` обе роли и их параметры.
7. Повторите шаги 3-6 для lighthouse. Помните, что одна роль должна настраивать один продукт.
8. Выложите все roles в репозитории. Проставьте тэги, используя семантическую нумерацию Добавьте roles в `requirements.yml` в playbook.
9. Переработайте playbook на использование roles. Не забудьте про зависимости lighthouse и возможности совмещения `roles` с `tasks`.
10. Выложите playbook в репозиторий.
11. В ответ приведите ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

Ответ: Результат загрузки ролей и далее ссылки на них и playbook
```bash
user@user:~/PycharmProjects/mnt-homeworks-2/08-ansible-04-role/playbook$ ansible-galaxy role install -r requirements.yml -p roles
Starting galaxy role install process
- extracting clickhouse to /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-04-role/playbook/roles/clickhouse
- clickhouse (1.11.0) was installed successfully
- extracting lighthouse-role to /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-04-role/playbook/roles/lighthouse-role
- lighthouse-role (1.0.0) was installed successfully
- extracting vector-role to /home/user/PycharmProjects/mnt-homeworks-2/08-ansible-04-role/playbook/roles/vector-role
- vector-role (1.0.0) was installed successfully
```
https://github.com/alexeiemelin/lighthouse-role

https://github.com/alexeiemelin/vector-role

https://github.com/alexeiemelin/mnt-homeworks/tree/MNT-13/08-ansible-04-role/playbook

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
