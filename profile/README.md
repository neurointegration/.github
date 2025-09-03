# Начало работы с проектом "Спринты Нейроинтеграции"

Это руководство поможет вам настроить рабочее окружение для разработки и развертывания приложения.

## 1. Получение доступов

Прежде всего, вам необходимо получить доступы к следующим ресурсам:
*   **Yandex Cloud:** Обратитесь к Алине, чтобы получить доступ к облачной инфраструктуре проекта.
*   **GitHub:** Также запросите у Алины приглашение в организацию `neurointegration` на GitHub.

## 2. Инструкция для Frontend-разработчиков

### 2.1. Предварительные требования

Убедитесь, что на вашем компьютере установлены:
*   **Docker и Docker Compose:** Самый простой способ — установить [Docker Desktop](https://www.docker.com/products/docker-desktop/).

### 2.2. Локальный запуск Backend (с помощью Docker)

Фронтенд-разработчикам для локальной работы потребуется запущенный бэкенд. Следуйте этим шагам, чтобы поднять его в Docker.

1.  **Склонируйте репозиторий с бэкендом:**
    ```bash
    git clone git@github.com:neurointegration/backend-neurointegration-sprints.git
    cd backend-neurointegration-sprints
    ```

2.  **Настройте переменные окружения:**
    *   Перейдите в Yandex Cloud по [этой ссылке](https://console.yandex.cloud/folders/b1gfn47iid5moudv2umd/lockbox/secret/e6q0pu6q9mrhtoldt9pk/overview), чтобы получить доступ к секретам проекта в сервисе Lockbox.
    *   Найдите и скопируйте значение секрета с ключом `.env-for-local-backend-in-docker`.
    *   В корневой папке репозитория `backend-neurointegration-sprints` создайте файл `.env` и вставьте в него скопированное значение.
    *   Вернитесь в Lockbox, найдите и скопируйте значение секрета с ключом `neurosprints-developer.sa-ydb-key-for-local-use`.
    *   В той же папке создайте файл `neurosprints-developer.sa` и вставьте в него это значение.

3.  **Запустите приложение:**
    *   Для запуска всех сервисов выполните команду:
        ```bash
        docker-compose up -d
        ```
    *   Swagger-документация API будет доступна по адресу: [http://localhost/swagger/index.html](http://localhost/swagger/index.html)

4.  **Управление контейнерами:**
    *   **Остановка:** Для остановки всех контейнеров выполните команду:
        ```bash
        docker-compose down
        ```
    *   **Пересборка:** Если вы внесли изменения в код бэкенда и хотите их применить, пересоберите образ:
        ```bash
        docker-compose build
        ```
        А затем снова запустите:
        ```bash
        docker-compose up -d
        ```

### 2.3. Локальный запуск Frontend

1.  **Склонируйте репозиторий с фронтендом:**
    ```bash
    git clone git@github.com:neurointegration/frontend-neurointegration-sprints.git
    cd frontend-neurointegration-sprints
    ```

2.  **Установите зависимости и запустите приложение:**
    ```bash
    npm install
    npm run dev
    ```

### 2.4. Развертывание (Deploy) Frontend

Развертывание фронтенда в облачное хранилище (Object Storage) происходит через GitHub Actions.

1.  Перейдите в репозиторий `frontend-neurointegration-sprints` на GitHub.
2.  Откройте вкладку **Actions**.
3.  Выберите workflow **Deploy React App to Object Storage**.
4.  Нажмите кнопку **Run workflow**.
5.  Выберите окружение (`test` или `prod`), в которое хотите задеплоить изменения.
6.  Нажмите **Run workflow** для запуска.

## 3. Инструкция для Backend-разработчиков

Эта инструкция предназначена для локальной разработки и отладки бэкенда без использования Docker.

### 3.1. Предварительные требования

*   На вашем компьютере должен быть установлен **.NET 9 SDK**.

### 3.2. Локальный запуск Backend

1.  **Склонируйте репозиторий с бэкендом:**
    ```bash
    git clone git@github.com:neurointegration/backend-neurointegration-sprints.git
    cd backend-neurointegration-sprints
    ```

2.  **Настройте секреты для локальной разработки:**
    *   Перейдите в Yandex Cloud по [этой ссылке](https://console.yandex.cloud/folders/b1gfn47iid5moudv2umd/lockbox/secret/e6q0pu6q9mrhtoldt9pk/overview) в сервис Lockbox.
    *   Найдите и скопируйте значение секрета с ключом `neurosprints-developer.sa-ydb-key-for-local-use`.
    *   В корневой папке репозитория `backend-neurointegration-sprints` создайте файл `neurosprints-developer.sa` и вставьте в него это значение.
    *   Вернитесь в Lockbox, найдите и скопируйте значение секрета с ключом `secrets.json-for-local-backend`.
    *   Сохраните это значение в файл `secrets.json` для проекта `Api`. Проще всего это сделать так:
        *   **Visual Studio:** В обозревателе решений (Solution Explorer) кликните правой кнопкой мыши по проекту `Api`, выберите "Manage User Secrets". В открывшийся файл `secrets.json` вставьте скопированное значение.
        *   **Другие IDE:** Найдите способ управления секретами пользователя для .NET-проектов в вашей среде разработки.

3.  **Запустите приложение:**
    *   Вы можете запустить проект `Api` напрямую из вашей IDE (Visual Studio, Rider) или с помощью команды в терминале из папки `src/Api`:
        ```bash
        dotnet run
        ```

### 3.3. Развертывание (Deploy) Backend

1.  **Сборка и публикация образа:**
    *   Запушьте ваш код в ветку `main` репозитория `backend-neurointegration-sprints`.
    *   После этого автоматически запустится GitHub Actions, который соберет Docker-образ приложения и загрузит его в Yandex Container Registry.

2.  **Обновление контейнера в Yandex Cloud:**
    *   В консоли Yandex Cloud перейдите в нужный каталог (`neurosprints-stage` или `neurosprints-prod`).
    *   Откройте сервис **Serverless Containers**.
    *   Выберите контейнер `neurosprints-<название_окружения>`.
    *   Перейдите на вкладку **Редактор**.
    *   Нажмите кнопку **Создать ревизию**, чтобы развернуть новую версию приложения из опубликованного Docker-образа.
