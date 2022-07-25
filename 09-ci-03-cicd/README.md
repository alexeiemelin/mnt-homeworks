# Домашнее задание к занятию "09.03 CI\CD"

## Подготовка к выполнению

1. Создаём 2 VM в yandex cloud со следующими параметрами: 2CPU 4RAM Centos7(остальное по минимальным требованиям)
2. Прописываем в [inventory](./infrastructure/inventory/cicd/hosts.yml) [playbook'a](./infrastructure/site.yml) созданные хосты
3. Добавляем в [files](./infrastructure/files/) файл со своим публичным ключом (id_rsa.pub). Если ключ называется иначе - найдите таску в плейбуке, которая использует id_rsa.pub имя и исправьте на своё
4. Запускаем playbook, ожидаем успешного завершения
5. Проверяем готовность Sonarqube через [браузер](http://localhost:9000)
6. Заходим под admin\admin, меняем пароль на свой
7.  Проверяем готовность Nexus через [бразуер](http://localhost:8081)
8. Подключаемся под admin\admin123, меняем пароль, сохраняем анонимный доступ

## Знакомоство с SonarQube

### Основная часть

1. Создаём новый проект, название произвольное
2. Скачиваем пакет sonar-scanner, который нам предлагает скачать сам sonarqube
3. Делаем так, чтобы binary был доступен через вызов в shell (или меняем переменную PATH или любой другой удобный вам способ)

Ответ: 
```bash 
export PATH=$PATH:~/Downloads/sonar-scanner-cli-4.7.0.2747-linux/sonar-scanner-4.7.0.2747-linux/bin
```
4. Проверяем `sonar-scanner --version`

Ответ: 
```bash
sonar-scanner --version
INFO: Scanner configuration file: /home/user/Downloads/sonar-scanner-cli-4.7.0.2747-linux/sonar-scanner-4.7.0.2747-linux/conf/sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarScanner 4.7.0.2747
INFO: Java 11.0.14.1 Eclipse Adoptium (64-bit)
INFO: Linux 5.13.0-48-generic amd64
```
5. Запускаем анализатор против кода из директории [example](./example) с дополнительным ключом `-Dsonar.coverage.exclusions=fail.py`

Ответ: запускаем следующим образом (весь вывод в консоль не стал копировать):
```bash
sonar-scanner \
>   -Dsonar.projectKey=netology \
>   -Dsonar.sources=. \
>   -Dsonar.host.url=http://51.250.4.38:9000 \
>   -Dsonar.login=a695b3e36178667896a5e2da174d05a430a264d2 \
> -Dsonar.coverage.exclusions=fail.py
```
6. Смотрим результат в интерфейсе

Ответ:
```
2 бага
```

8. Исправляем ошибки, которые он выявил(включая warnings)
9. Запускаем анализатор повторно - проверяем, что QG пройдены успешно
10. Делаем скриншот успешного прохождения анализа, прикладываем к решению ДЗ


## Знакомство с Nexus

### Основная часть

1. В репозиторий `maven-public` загружаем артефакт с GAV параметрами:
   1. groupId: netology
   2. artifactId: java
   3. version: 8_282
   4. classifier: distrib
   5. type: tar.gz
2. В него же загружаем такой же артефакт, но с version: 8_102
3. Проверяем, что все файлы загрузились успешно
4. В ответе присылаем файл `maven-metadata.xml` для этого артефекта

Ответ: maven-metadata.xml лежит в корне репозитория

### Знакомство с Maven

### Подготовка к выполнению

1. Скачиваем дистрибутив с [maven](https://maven.apache.org/download.cgi)
2. Разархивируем, делаем так, чтобы binary был доступен через вызов в shell (или меняем переменную PATH или любой другой удобный вам способ)

Ответ: 
```bash
sudo tar xvf ~/Downloads/apache-maven-*.tar.gz -C /opt
export PATH=/opt/apache-maven-3.8.6/bin:$PATH
```

3. Удаляем из `apache-maven-<version>/conf/settings.xml` упоминание о правиле, отвергающем http соединение( раздел mirrors->id: my-repository-http-unblocker)
4. Проверяем `mvn --version`

Ответ:
```bash
mvn -v
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /opt/apache-maven-3.8.6
Java version: 11.0.15, vendor: Private Build, runtime: /usr/lib/jvm/java-11-openjdk-amd64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.13.0-48-generic", arch: "amd64", family: "unix"
```

5. Забираем директорию [mvn](./mvn) с pom

### Основная часть

1. Меняем в `pom.xml` блок с зависимостями под наш артефакт из первого пункта задания для Nexus (java с версией 8_282)
2. Запускаем команду `mvn package` в директории с `pom.xml`, ожидаем успешного окончания

Ответ:
```bash
[WARNING] JAR will be empty - no content was marked for inclusion!
[INFO] Building jar: /home/user/PycharmProjects/mnt-homeworks-2/09-ci-03-cicd/mvn/target/simple-app-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  20.350 s
[INFO] Finished at: 2022-07-25T22:28:33+03:00
[INFO] ------------------------------------------------------------------------
```

3. Проверяем директорию `~/.m2/repository/`, находим наш артефакт

Ответ:
```bash
user@user:~/.m2/repository/netology/java/8_282$ ls -la
total 160
drwxrwxr-x 2 user user   4096 Jul 25 22:28 .
drwxrwxr-x 3 user user   4096 Jul 24 20:58 ..
-rw-rw-r-- 1 user user 139662 Jul 25 22:28 java-8_282-distrib.tar.gz
-rw-rw-r-- 1 user user     40 Jul 25 22:28 java-8_282-distrib.tar.gz.sha1
-rw-rw-r-- 1 user user   1119 Jul 25 22:28 java-8_282.pom.lastUpdated
-rw-rw-r-- 1 user user    175 Jul 25 22:28 _remote.repositories
```

4. В ответе присылаем исправленный файл `pom.xml`

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
