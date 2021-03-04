# Домашнее задание к занятию «4.3 Рассылка и приём Push-уведомлений»

В качестве результата пришлите ссылки на ваш GitHub-проект в личном кабинете студента на сайте [netology.ru](https://netology.ru).

**Важно**: ознакомьтесь со ссылками, представленными на главной странице [репозитория с домашними заданиями](../README.md).

**Важно**: если у вас что-то не получилось, то оформляйте Issue [по установленным правилам](../report-requirements.md).

## Как сдавать задачи

1. Откройте ваш проект Android приложения с предыдущего ДЗ (можете брать код с лекции).
1. Сделайте необходимые коммиты.
1. Сделайте пуш (удостоверьтесь, что ваш код появился на GitHub).
1. Ссылку на ваш проект отправьте в личном кабинете на сайте [netology.ru](https://netology.ru).
1. Задачи, отмеченные, как необязательные, можно не сдавать, это не повлияет на получение зачета (в этом ДЗ все задачи являются обязательными).

## Задача RecipientId

**Важно**: не забудьте добавить в сервер и клиента нужные файлы (см. лекцию). А также добавьте их в `.gitignore`.

### Описание

Реализуйте на клиентской стороне при получении push-сообщения проверку `recipientId` (сервер будет присылать вам его в Push'е).

Для этого сравнивайте полученный `recipientId` с тем, что хранится у вас в `AppAuth`, и выполняйте одно из следующих действий:
* если `recipientId` = тому, что в `AppAuth`, то всё ok, показываете `Notification`;
* если `recipientId` = 0 (и не равен вашему), значит сервер считает, что у вас анонимная аутентификация и вам нужно переотправить свой push token;
* если `recipientId` != 0 (и не равен вашему), значит сервер считает, что на вашем устройстве другая аутентификация и вам нужно переотправить свой push token;
* если `recipientId` = null, то это массовая рассылка, показываете `Notification`.

Для тестирования отправляйте запрос вида:

```http request
POST http://localhost:9999/api/pushes?token=<put your token here>
Content-Type: application/json

{
  "recipientId": null,
  "content": "Wow!"
}
```

Пользуйтесь для этого любым средством: Postman, cURL или любое другое, включая OkHttp, которые мы  рассматривали в рамках лекций.

### Результат

Опубликуйте изменения в виде Pull Request'а в вашем проекте на GitHub.

В качестве результата пришлите: ссылку на PR GitHub-проект в личном кабинете студента на сайте [netology.ru](https://netology.ru)