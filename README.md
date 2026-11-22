ДЗ3 Docker (8 баллов)

Приложение с счетчиком запущенное через Docker Compose. Использовал: app, redis

Запуск:

docker compose up -d --build

Проверка работы:

curl http://localhost:8080/

curl -X POST http://localhost:8080/increment

docker compose ps

Настройки в файле .env.example:

HOST_PORT=8080

GREETING=52

Чтобы изменить порт и приветствие можно создать файл .env с нужными значениями 

Хранение данных

Счётчик хранится в Redis. Данные сохраняются в именованном volume redis_data. Запись на диск включена через --appendonly yes

Проверка сохранениф данных:

docker compose down

docker compose up -d

curl http://localhost:8080/

Проверка Redis

Остановка хранилища:

docker compose stop redis

curl -i http://localhost:8080/healthz

curl -i http://localhost:8080/readyz

curl -i -X POST http://localhost:8080/increment

При остановленном Redis первый запрос возвращает 200, остальные 503

Восстановление:

docker compose start redis

docker compose ps

curl http://localhost:8080/

curl -X POST http://localhost:8080/increment

После восстановаления Redis приложение продолжает работу без перезапуска



Доп.(4 и 6 баллов):
Для проверки на 6 баллов с sqlite  используется compose.sqlite.yaml:
docker compose -f compose.sqlite.yaml up -d --build
логи запуска и запросов сохранены в файле logs_4.txt


4 балла: счетчик увеличился с 0 до 2. Логи в `logs_4.txt`

6 баллов: после пересоздания контейнера sqlite сохранил значение 3

8 баллов: после `docker compose down` и повторного `up -d` Redis сохранил значение 3 при остановке Redis `/healthz` вернул 200, а `/readyz` и `/increment` - 503. После запуска Redis счётчик увеличился с 3 до 4 без перезапуска приложения
