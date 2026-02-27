<div align="center">
  <h1>MyBB Managing With Ansible</h1>
  <p>
    <strong>Автоматизация управления форумами MyBB с помощью Ansible</strong>
  </p>
  <p>
    <a href="https://www.mybb.com/">
        <img src="https://img.shields.io/badge/MyBB-1.8.39-blue?style=for-the-badge&logo=mybb" alt="MyBB Version">
    </a>
    <a href="https://www.ansible.com/">
        <img src="https://img.shields.io/badge/Ansible-10.x-green?style=for-the-badge&logo=ansible" alt="Ansible Compatible">
    </a>
    <a href="https://github.com/kmitrakov/MyBB-Managing-With-Ansible/releases">
        <img src="https://img.shields.io/github/v/release/kmitrakov/MyBB-Managing-With-Ansible?style=for-the-badge&logo=github" alt="Release">
    </a>
    <a href="https://github.com/kmitrakov/MyBB-Managing-With-Ansible/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/kmitrakov/MyBB-Managing-With-Ansible?style=for-the-badge" alt="License">
    </a>
  </p>
</div>

---

<p>
    <div>
        <img src="" width="100%" alt="" />
    </div>
</p>

## <a id="title0">Содержание</a>
- [Описание](#title1)
    - [Возможности](#title1.1)
    - [Предварительные требования](#title1.2)
    - [Установка и настройка](#title1.3)
- [Использование](#title2)
    - [Развёртывание нового форума](#title2.1)
    - [Создание резервной копии существующего форума](#title2.2)
    - [Удаление существующего форума](#title2.3)
- [Рекомендации](#title3)
- [Версии и совместимость](#title4)
- [Часто задаваемые вопросы](#title5)
- [Разработка и внесение правок](#title6)
- [Команда проекта](#title7)
- [Источники](#title8)

## <a id="title1">Описание</a>
Этот репозиторий предоставляет набор готовых плейбуков Ansible для автоматизации рутинных операций по управлению форумами на движке MyBB. Проект предназначен для системных администраторов, DevOps-инженеров и владельцев форумов, которые хотят стандартизировать и упростить процессы развертывания, резервного копирования и удаления своих сообществ.

### <a id="title1.1">Возможности</a>
- **Полностью автоматизированное развертывание.** Установка MyBB заданной версии с подготовкой базы данных и веб-сервера.
- **Резервное копирование.** Создание дампа базы данных и архива файлов форума с временной меткой.
- **Чистое удаление.** Полное удаление файлов форума и его базы данных с возможностью создания финального бекапа.
- **Безопасность.** Поддержка ```ansible-vault``` для шифрования паролей и другой чувствительной информации.
- **Гибкая конфигурация.** Все параметры (пути, версии, учетные данные) вынесены в отдельные файлы переменных.

### <a id="title1.2">Предварительные требования</a>

- **Управляющая машина.** Linux/macOS с установленными Python и Git.
- **Целевой сервер.** Linux-сервер (рекомендуется Ubuntu/Debian/CentOS) с доступом по SSH и настроенным Python.
- **Права sudo.** Пользователь, от имени которого запускается Ansible, должен иметь права ```sudo``` без запроса пароля (или настроенный пароль в Ansible) для установки пакетов.

### <a id="title1.3">Установка и настройка</a>
1. Получите код данного пакета.
```shell
git clone https://github.com/kmitrakov/MyBB-Managing-With-Ansible.git
```
2. Установите пакет для управления виртуальными окружениями.
```shell
pip3 install --upgrade virtualenv
```
3. Создайте виртуальное окружение.
```shell
python3 -m virtualenv venv
```
4. Запустите виртуальное окружение.
```shell
source venv/bin/activate
```
5. Установите Ansible.
```shell
pip3 install ansible
```
6. Отредактируйте файл ```inventory/default/group_vars/hosts```.
    - ansible_host - Целевой сервер.
    - ansible_user - Пользователь для подключения по SSH.
    - ansible_ssh_private_key_file - Сертификат для подключения по SSH.

Пример файла:
```text
[service_mybb]
host1 ansible_host=host-01.hosting.com ansible_user=host-01-admin ansible_ssh_private_key_file=~/.ssh/host-01-admin-key-1762179269032

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```
> Убедитесь, что сертификат добавлен в агент или указан верный путь, а хост сервера присутствует в `~/.ssh/known_hosts`.

7. Отредактируйте файл ```inventory/default/group_vars/all/vars.yml```.
    - tmp_dir - Временный каталог.

Пример файла:
```yaml
---
tmp_dir: "/tmp"
```
8. Отредактируйте файл ```inventory/default/group_vars/all/vault.yml```.
    - mysql_root_password - Пароль root от MySql.

Пример файла:
```yaml
---
mysql_root_password: "ykqfmb4Hke2ol8wO"
```
9. Отредактируйте файл ```inventory/default/group_vars/service_mybb/vars.yml```.
    - mybb_version - Версия MyBB.
    - mybb_install_dir - Каталог установки MyBB.
    - mybb_backup_dir - Каталог резервных копий MyBB.

Пример файла:
```yaml
---
service_mybb_state: "deploy"

# MyBB variables
mybb_version: "1.8.39"
mybb_install_dir: "/var/www/html/forum"
mybb_backup_dir: "/var/backups/forum"

timestamp: "{{ ansible_date_time.year }}-{{ ansible_date_time.month }}-{{ ansible_date_time.day }}_{{ ansible_date_time.hour }}-{{ ansible_date_time.minute }}-{{ ansible_date_time.second }}"
mybb_backup_filename: "mybb_backup_{{ timestamp }}.tar.gz"
```
10. Отредактируйте файл ```inventory/default/group_vars/service_mybb/vault.yml```.
    - mybb_db_name - Имя базы данных MyBB.
    - mybb_db_user - Пользователь базы данных MyBB.
    - mybb_db_password - Пароль пользователя базы данных MyBB.

Пример файла:
```yaml
---
# MyBB database variables
mybb_db_name: "forum"
mybb_db_user: "forum_user"
mybb_db_password: "bzEq8el06ozoJU16"
mybb_db_host: "localhost"
```
## <a id="title2">Использование</a>
Управление осуществляется путём изменения значения переменной ```service_mybb_state``` в файле ```inventory/default/group_vars/service_mybb/vars.yml``` и последующего запуска основного плейбука.

### <a id="title2.1">Развёртывание нового форума</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```deploy```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```
Плейбук скачает указанную версию MyBB, настроит базу данных, скопирует файлы и выполнит начальную конфигурацию.

### <a id="title2.2">Создание резервной копии существующего форума</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```backup```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```
Архив с файлами форума и дамп базы данных будут сохранены в директории ```mybb_backup_dir```.

### <a id="title2.3">Удаление существующего форума</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```delete```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```
Внимание, эта операция необратима.

## <a id="title3">Рекомендации</a>
### <a id="title3.1">Шифрование чувствительных данных</a>
Файлы ```vault.yml``` содержат чувствительные данные. Рекомендуется использовать утилиту [ansible-vault](https://docs.ansible.com/projects/ansible/latest/cli/ansible-vault.html) для защиты данных.

Для этого необходимо:
1. Создать файл, содержащий пароль для шифрования, например ```default_password_file.yml```, и записать в него пароль.
2. Добавить файл в ```.gitignore```.

Пример:
```text
# Ansible vault password files
default_password_file.yml
```
3. Команды для шифрования файлов:
```shell
ansible-vault encrypt inventory/default/group_vars/all/vault.yml --vault-password-file default_password_file.yml
ansible-vault encrypt inventory/default/group_vars/service_mybb/vault.yml --vault-password-file default_password_file.yml
```
4. Команды для дешифровки файлов:
```shell
ansible-vault decrypt inventory/default/group_vars/all/vault.yml --vault-password-file default_password_file.yml
ansible-vault decrypt inventory/default/group_vars/service_mybb/vault.yml --vault-password-file default_password_file.yml
```
5. Запуск плейбука с использованием шифрования:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml --vault-password-file default_password_file.yml
```

### <a id="title3.2">Система контроля версий</a>
Храните все неизменяемые файлы (например, зашифрованные ```vault.yml``` и обычные ```vars.yml```) в Git. Это позволит отслеживать историю изменений конфигурации вашего форума.

### <a id="title3.3">Именование</a>
При использовании проекта для нескольких форумов, скопируйте каталог ```inventory/default``` в ```inventory/forum_name``` и настраивайте переменные под каждый форум индивидуально.

### <a id="title3.4">Тестирование</a>
Перед выполнением плейбуков в реальной среде используйте флаг ```--check``` для "сухого прогона" и ```--diff```, чтобы увидеть, какие изменения будут внесены в файлы.
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml --check --diff
```

## <a id="title4">Версии и совместимость</a>
| Версия пакета | Совместимость с MyBB | Статус                                     |
|:--------------|:---------------------|:-------------------------------------------|
| 1.0.x         | 1.8.39               | ✅ Поддерживается                          |

## <a id="title5">Часто задаваемые вопросы</a>
- **Я нашел ошибку. Куда сообщить?**
- Пожалуйста, создайте [Issue](https://github.com/kmitrakov/MyBB-Managing-With-Ansible/issues) в этом репозитории, подробно описав проблему, указав путь к файлу, на котором обнаружена ошибка.
- **Можно ли использовать этот проект для обновления MyBB?**
- Нет, в текущей версии автоматическое обновление не поддерживается. Изменение версии в `vars.yml` приведет к попытке развернуть новую версию, что может нарушить работу существующего форума.

## <a id="title6">Разработка и внесение правок</a>
Если вы хотите помочь с улучшением или исправить ошибку:
1.  Сделайте форк (Fork) этого репозитория.
2.  Создайте новую ветку (Branch) для ваших изменений (`git checkout -b fix-deploy`).
3.  Внесите правки.
4.  Сделайте коммит (Commit) ваших изменений (`git commit -am 'Исправлена ошибка с правами на файлы при разворачивании MyBB'`).
5.  Запуште (Push) изменения в ваш форк (`git push origin fix-deploy`).
6.  Создайте новый Pull Request в этом репозитории.

Ваша помощь приветствуется!

## <a id="title7">Команда проекта</a>
- [Kirill Mitrakov](https://github.com/kmitrakov/) [(https://mitrakov.tech)](https://mitrakov.tech).

## <a id="title8">Источники</a>
- [Документация Ansible](https://docs.ansible.com/)
- [Официальная документация по установке MyBB](https://docs.mybb.com/1.8/install/)