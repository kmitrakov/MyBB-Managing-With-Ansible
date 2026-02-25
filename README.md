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
    - [Развёртывание нового форума MyBB (deploy)](#title1.2)
    - [Создание резервной копии существующего форума MyBB (backup)](#title1.3)
    - [Удаление существующего форума MyBB (delete)](#title1.4)
- [Версии и совместимость](#title2)
- [Часто задаваемые вопросы](#title3)
- [Разработка и внесение правок](#title4)
- [Команда проекта](#title5)
- [Источники](#title6)

## <a id="title1">Описание</a>
Репозиторий содержит плейбуки Ansible для управления MyBB. Предназначен для администраторов, разработчиков и владельцев форумов на базе MyBB.

Выполняемые задачи:
- Развёртывание нового форума MyBB.
- Создание резервной копии существующего форума MyBB.
- Удаление существующего форума MyBB

### <a id="title1.1">Предварительная настройка</a>
- Получите код данного пакета.
```
git clone https://github.com/kmitrakov/MyBB-Managing-With-Ansible.git
```
- Установите пакет для управления виртуальными окружениями.
```
pip3 install --upgrade virtualenv
```
- Создайте виртуальное окружение.
```
python3 -m virtualenv venv
```
- Запустите виртуальное окружение.
```
source venv/bin/activate
```
- Установите Ansible.
```
pip3 install ansible
```

### <a id="title1.2">Развёртывание нового форума MyBB (deploy)</a>

### <a id="title1.3">Создание резервной копии существующего форума MyBB (backup)</a>

### <a id="title1.4">Создание резервной копии существующего форума MyBB (delete)</a>

## <a id="title2">Версии и совместимость</a>
| Версия пакета | Совместимость с MyBB | Статус                                     |
|:--------------|:---------------------|:-------------------------------------------|
| 1.0.x         | 1.8.39               | ✅ Поддерживается                          |

## <a id="title3">Часто задаваемые вопросы</a>
- **Я нашел ошибку. Куда сообщить?**
- Пожалуйста, создайте [Issue](https://github.com/kmitrakov/MyBB-Managing-With-Ansible/issues) в этом репозитории, подробно описав проблему, указав путь к файлу, на котором обнаружена ошибка.

## <a id="title4">Разработка и внесение правок</a>
Если вы хотите помочь с улучшением или исправить ошибку:
1.  Сделайте форк (Fork) этого репозитория.
2.  Создайте новую ветку (Branch) для ваших изменений (`git checkout -b fix-deploy`).
3.  Внесите правки.
4.  Сделайте коммит (Commit) ваших изменений (`git commit -am 'Исправлена ошибка с правами на файлы при разворачивании MyBB'`).
5.  Запуште (Push) изменения в ваш форк (`git push origin fix-deploy`).
6.  Создайте новый Pull Request в этом репозитории.

Ваша помощь приветствуется!

## <a id="title5">Команда проекта</a>
- [Kirill Mitrakov](https://github.com/kmitrakov/) [(https://mitrakov.tech)](https://mitrakov.tech).

## <a id="title6">Источники</a>
- [Документация Ansible](https://docs.ansible.com/)
- [Официальная документация по установке MyBB](https://docs.mybb.com/1.8/install/)