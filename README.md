[![Main Taski workflow](https://github.com/MrFR0D0/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/MrFR0D0/kittygram_final/actions/workflows/main.yml)

#  Проект «Kittygram»

## Описание проекта:

- #### Проект Kittygram позволяет пользователям поделиться котами и рассказать об их достижениях!

## Возможности сайта:
- #### Зарегистрироваться.
- #### Добавить кота!
- #### Убрать (своего) кота!
- #### Подобрать цвет, на который его окрас больше всего походит!
- #### Не забудьте счастливую фотографию любимца!
- #### Рассказть всем чем прославился ваш кот!
- #### Указать возраст любимца.
---

## В проекте были использованы технологии:
* #### Django REST
* #### Python 3.9
* #### Gunicorn
* #### Nginx
* #### JS
* #### Node.js
* #### PostgreSQL
* #### Docker


## Как запустить проект:

``` Настраиваем Docker
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin;
# Переходим в директорию проекта.
cd kittygram_final/
# Создаем и редактируем файл .env, в котором нужно указать данные
sudo nano .env
# 
POSTGRES_DB=<Желаемое_имя_базы_данных>
POSTGRES_USER=<Желаемое_имя_пользователя_базы_данных>
POSTGRES_PASSWORD=<Желаемый_пароль_пользователя_базы_данных>
DB_HOST=db
DB_PORT=5432
SECRET_KEY=<Секретный ключ джанго>
DEBUG=<Режим дебага>
ALLOWED_HOSTS=<Разрешенные хосты>
# Сохраняем файл.
# Далее выполняем последовательно
sudo docker compose -f docker-compose.production.yml down
sudo docker compose -f docker-compose.production.yml pull
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collect_static/. /static_backend/static/
# Создаем суперпользователся. Следуем инструкциям при выполнении.
sudo docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser

Устанавливаем и настраиваем NGINX

```bash
# Устанавливаем NGINX
sudo apt install nginx -y
# Запускаем
sudo systemctl start nginx
# Настраиваем firewall
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
# Включаем firewall
sudo ufw enable
# Открываем конфигурационный файл NGINX
sudo nano /etc/nginx/sites-enabled/default
# Полностью удаляем из него все и пишем новые настройки

server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_set_header HOST $host;
        proxy_pass http://127.0.0.1:9000;

    }
}

# Сохраняем изменения и выходим из редактора
# Проверяем корректность настроек
sudo nginx -t
# Запускаем NGINX
sudo systemctl start nginx
```
Настраиваем HTTPS

```bash
# Установка пакетного менеджера snap.
# У этого пакетного менеджера есть нужный вам пакет — certbot.
sudo apt install snapd
# Установка и обновление зависимостей для пакетного менеджера snap.
sudo snap install core; sudo snap refresh core
# При успешной установке зависимостей в терминале выведется:
# core 16-2.58.2 from Canonical✓ installed 

# Установка пакета certbot.
sudo snap install --classic certbot
# При успешной установке пакета в терминале выведется:
# certbot 2.3.0 from Certbot Project (certbot-eff✓) installed

# Создание ссылки на certbot в системной директории,
# чтобы у пользователя с правами администратора был доступ к этому пакету.
sudo ln -s /snap/bin/certbot /usr/bin/certbot

# Получаем сертификат и настраиваем NGINX следуя инструкциям
sudo certbot --nginx
# Перезапускаем NGINX
sudo systemctl reload nginx
```

---
### Автор:
* [Dmitry Nikolaev](https://github.com/MrFR0D0)