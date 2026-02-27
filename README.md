# MyBB Managing With Ansible

![MyBB Version](https://img.shields.io/badge/MyBB-1.8.39-blue?style=flat-square)
![Release](https://img.shields.io/badge/Release-1.0.0-orange?style=flat-square)
![Status](https://img.shields.io/badge/Status-Actively%20Maintained-brightgreen?style=flat-square)

Управление MyBB с помощью Ansible.

<p>
    <div>
        <img src="" width="100%" alt="" />
    </div>
</p>

- [Описание](#title1)
    - [Предварительная настройка](#title1.1)
    - [Развёртывание нового форума MyBB](#title1.2)
    - [Создание резервной копии существующего форума MyBB](#title1.3)
    - [Удаление существующего форума MyBB](#title1.4)
- [Рекомендации](#title2)
- [Версии и совместимость](#title3)
- [Часто задаваемые вопросы](#title4)
- [Разработка и внесение правок](#title5)
- [Команда проекта](#title6)
- [Источники](#title7)

## <a id="title1">Описание</a>
Репозиторий содержит плейбуки Ansible для управления MyBB. Предназначен для администраторов, разработчиков и владельцев форумов на базе MyBB.

Выполняемые задачи:
- Развёртывание нового форума MyBB.
- Создание резервной копии существующего форума MyBB.
- Удаление существующего форума MyBB

### <a id="title1.1">Предварительная настройка</a>
#### <a id="title1.1.1">Общая настройка</a>
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
#### <a id="title1.1.2">Настройка проекта</a>
1. Отредактируйте файл ```inventory/default/group_vars/hosts```.
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
Примечания:
- Для успешного подключения по SSH информация о целевом сервере должна быть добавлена в файл ```known_hosts```.

2. Отредактируйте файл ```inventory/default/group_vars/all/vars.yml```.
    - tmp_dir - Временный каталог.

Пример файла:
```yaml
---
tmp_dir: "/tmp"
```
3. Отредактируйте файл ```inventory/default/group_vars/all/vault.yml```.
    - mysql_root_password - Пароль root от MySql.

Пример файла:
```yaml
---
mysql_root_password: "ykqfmb4Hke2ol8wO"
```
4. Отредактируйте файл ```inventory/default/group_vars/service_mybb/vars.yml```.
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
5. Отредактируйте файл ```inventory/default/group_vars/service_mybb/vault.yml```.
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

### <a id="title1.2">Развёртывание нового форума MyBB</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```deploy```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```

### <a id="title1.3">Создание резервной копии существующего форума MyBB</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```backup```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```

### <a id="title1.4">Создание резервной копии существующего форума MyBB</a>
1. В файле ```inventory/default/group_vars/service_mybb/vars.yml``` переменной ```service_mybb_state``` задайте значение ```delete```.
2. Запустите выполнение плейбука:
```shell
ansible-playbook -i inventory/default/hosts service_mybb.yml
```

## <a id="title2">Рекомендации</a>
### <a id="title2.1">Шифрование чувствительных данных</a>
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

## <a id="title3">Версии и совместимость</a>
| Версия пакета | Совместимость с MyBB | Статус                                     |
|:--------------|:---------------------|:-------------------------------------------|
| 1.0.x         | 1.8.39               | ✅ Поддерживается                          |

## <a id="title4">Часто задаваемые вопросы</a>
- **Я нашел ошибку. Куда сообщить?**
- Пожалуйста, создайте [Issue](https://github.com/kmitrakov/MyBB-Managing-With-Ansible/issues) в этом репозитории, подробно описав проблему, указав путь к файлу, на котором обнаружена ошибка.

## <a id="title5">Разработка и внесение правок</a>
Если вы хотите помочь с улучшением или исправить ошибку:
1.  Сделайте форк (Fork) этого репозитория.
2.  Создайте новую ветку (Branch) для ваших изменений (`git checkout -b fix-deploy`).
3.  Внесите правки.
4.  Сделайте коммит (Commit) ваших изменений (`git commit -am 'Исправлена ошибка с правами на файлы при разворачивании MyBB'`).
5.  Запуште (Push) изменения в ваш форк (`git push origin fix-deploy`).
6.  Создайте новый Pull Request в этом репозитории.

Ваша помощь приветствуется!

## <a id="title6">Команда проекта</a>
- [Kirill Mitrakov](https://github.com/kmitrakov/) [(https://mitrakov.tech)](https://mitrakov.tech).

## <a id="title7">Источники</a>
- [Документация Ansible](https://docs.ansible.com/)
- [Официальная документация по установке MyBB](https://docs.mybb.com/1.8/install/)