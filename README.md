# Проект связанный с внесением изменений в код с сохранением функциональности, добавлением нового функционала, настройкой инфраструктуры

## Аналоги

- https://java-source.net/open-source/issue-trackers

  ## Выполнены поставленные задачи:

- Разобраться со структурой проекта (onboarding).
- Удалить социальные сети: vk, yandex. 
- Вынести чувствительную информацию в отдельный проперти файл:
логин
пароль БД
идентификаторы для OAuth регистрации/авторизации
настройки почты
Значения этих проперти должны считываться при старте сервера из переменных окружения машины. 
- Переделать тесты так, чтоб во время тестов использовалась in memory БД, а не PostgreSQL. Для этого нужно определить 2 бина, и выборка какой из них использовать должно определяться активным профилем Spring. H2 не поддерживает все фичи, которые есть у PostgreSQL, поэтому тебе прийдется немного упростить скрипты с тестовыми данными.
- Сделать рефакторинг метода com.javarush.jira.bugtracking.attachment.FileUtil#upload чтоб он использовал современный подход для работы с файловой системмой.
- Написать Dockerfile для основного сервера
- Написать docker-compose файл для запуска контейнера сервера вместе с БД и nginx. Для nginx используй конфиг-файл config/nginx.conf. При необходимости файл конфига можно редактировать.

## Важно для проверки:

## Как запустить приложение и тесты из среды разработки:
  - Требуется закомментировать строку `spring.datasource.url=jdbc:postgresql://postgres-db:5432/jira`
    и раскомментировать строку `spring.datasource.url=jdbc:postgresql://localhost:5432/jira`
    в файле src/main/resources/application.properties
  - Удалить контейнеры, образы , тома (volumes) созданные при запуске docker compose
  - Создать контейнер с бд jira в терминале/консоли:
    `docker run -p 5432:5432 --name postgres-db -e POSTGRES_USER=jira -e POSTGRES_PASSWORD=JiraRush -e POSTGRES_DB=jira -e 
    PGDATA=/var/lib/postgresql/data/pgdata -v ./pgdata:/var/lib/postgresql/data -d postgres`
  - Создать и заполнить таблицы бд jira c помощью скриптов `db/changelog.sql`, `data4dev/data.sql`
  - ВЫполнить метод main() класса com.javarush.jira.JiraRushApplication (для запуска приложения и отслеживания
    его работы по адресу http://localhost:8080/)
  - Запустить тесты из контекстного меню командой Run 'All Tests' директории src/test/java

## Как запустить приложение с помощью docker-compose:
  - Требуется закомментировать строку `spring.datasource.url=jdbc:postgresql://localhost:5432/jira`
    и раскомментировать строку `spring.datasource.url=jdbc:postgresql://postgres-db:5432/jira`
    в файле src/main/resources/application.properties
  - Удалить docker контейнер с бд jira `postgres-db` и другие контейнеры, образы, тома ,
    которые могли появиться при предыдущем выполнении команды `docker-compose up --build`
  - Выполнить команду maven `mvn clean install package -Dmaven.test.skip=true`
  - Выполнить команду в терминале/консоли `docker-compose up --build` 
    При выполнении команды на линукс иногда появлялось сообщение `failed to solve: error from sender: open 
    /home/miux/Java/javarush/project-final/pgdata-test/pgdata: permission denied`. Для устранения проблемы приходилось
    переназначать права директории `./pgdata` на чтение и запись. Повторно вызывать команду `docker-compose up --build`
  - Для полноценной проверки приложения требуется заполнить созданные таблицы бд jira, сxема public (сейчас она находится в контейнере созданном docker compose)
    с помощью скрипта `data4dev/data.sql`.


