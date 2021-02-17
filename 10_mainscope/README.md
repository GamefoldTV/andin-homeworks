# Домашнее задание к занятию «3.3 Coroutines в Android»

В качестве результата пришлите ссылки на ваш GitHub-проект в личном кабинете студента на сайте [netology.ru](https://netology.ru).

**Важно**: ознакомьтесь со ссылками, представленными на главной странице [репозитория с домашними заданиями](../README.md).

**Важно**: если у вас что-то не получилось, то оформляйте Issue [по установленным правилам](../report-requirements.md).

## Как сдавать задачи

1. Откройте ваш проект Android приложения с предыдущего ДЗ (можете брать код с лекции)
1. Сделайте необходимые коммиты
1. Сделайте пуш (удостоверьтесь, что ваш код появился на GitHub)
1. Ссылку на ваш проект отправьте в личном кабинете на сайте [netology.ru](https://netology.ru)
1. Задачи, отмеченные, как необязательные, можно не сдавать, это не повлияет на получение зачета (в этом ДЗ все задачи являются обязательными)

## Задача remove & likes

### Легенда

[Используя код с лекции](https://github.com/netology-code/andin-code/tree/master/10_mainscope) (сервер также берите с лекции), реализуйте в проекте функциональность удаления и проставления лайков (для этого нужно отредактировать `PostViewModel` и `PostRepositoryImpl`):

```kotlin
// PostViewModel
fun likeById(id: Long) {
    TODO()
}

fun removeById(id: Long) {
    TODO()
}

// PostRepositoryImpl
override suspend fun removeById(id: Long) {
    TODO("Not yet implemented")
}

override suspend fun likeById(id: Long) {
    TODO("Not yet implemented")
}
```

Логика работы:
1. Сначала модифицируете запись в локальной БД (либо удаляете)
1. Затем отправляете соответствующий запрос в API (HTTP)

Не забудьте про обработку ошибок и кнопку `Retry`, в случае, если запрос в API завершился с ошибкой (в том числе в случае отсутствия сетевого соединения*).

Примечание*: не обязательно для этого перезапускать сервер, достаточно отключить сеть в шторке телефона/эмулятора.

### Результат

Опубликуйте изменения в виде Pull Request'а в вашем проекте на GitHub.

В качестве результата пришлите: ссылку на PR GitHub-проект в личном кабинете студента на сайте [netology.ru](https://netology.ru)

## Задача save*

**Важно**: это необязательная задача. Её (не)выполнение не влияет на получение зачёта по ДЗ.

В текущей реализации сохранения мы сначала отправляем запрос в API и только в случае получения успешного ответа добавляем его в локальную БД (и пост появляется в `RecyclerView`):
```kotlin
override suspend fun save(post: Post) {
    try {
        val response = PostsApi.service.save(post)
        if (!response.isSuccessful) {
            throw ApiError(response.code(), response.message())
        }

        val body = response.body() ?: throw ApiError(response.code(), response.message())
        dao.insert(PostEntity.fromDto(body))
    } catch (e: IOException) {
        throw NetworkError
    } catch (e: Exception) {
        throw UnknownError
    }
}
```

Попробуйте сделать наоборот: сначала сохранить в локальную БД (чтобы он сразу появился в `RecyclerView`) а затем уже запрос в API.

<details>
<summary>Подсказка</summary>

Для этого вам скорее всего придётся подумать над двумя вещами:
1. Какой `id` должен быть у поста (ведь `id` присваивается на сервере)?
1. Надо как-то отделять несохранённые на сервере посты от сохранённых (например, в мессенджерах Telegram/WhatsApp рисуется специальная иконка, уведомляющая о том, что сообщение ещё не сохранено на сервере)

Для реализации второго пункта стоит подумать о том, чтобы внести в `PostEntity` дополнительные поля, отвечающие за данный статус, и, соответственно, на базе них рисовать иконку статуса в карточке поста + продумывать механику взаимодействия (например, несохранённый пост нельзя залайкать).
</details>

Не забудьте про `Retry`, позволяющий осуществить повторную попытку вызова API сохранения в случае возникновения проблем (например, отсутствия сетевого подключения).

### Результат

Опубликуйте изменения в виде Pull Request'а в вашем проекте на GitHub.

В качестве результата пришлите ссылку на PR GitHub-проект в личном кабинете студента на сайте [netology.ru](https://netology.ru)