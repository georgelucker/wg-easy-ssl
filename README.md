# WireGuard VPN + Nginx Proxy + Let's Encrypt

Этот `docker-compose`-проект разворачивает веб-интерфейс для управления WireGuard VPN с помощью [wg-easy](https://github.com/WeeJeWel/wg-easy), с доступом через доменное имя и HTTPS, автоматически выдаваемым Let's Encrypt.

## 📦 Сервисы

- **wg-easy** — удобный веб-интерфейс для управления пользователями WireGuard.
- **nginx-proxy** — автоматически проксирует контейнеры на основе переменных окружения.
- **acme-companion** — автоматически выпускает и обновляет TLS-сертификаты от Let's Encrypt.

## 📁 Структура volumes

| Том             | Назначение                                      |
|------------------|--------------------------------------------------|
| etc_wireguard    | Конфигурации WireGuard                           |
| nginx_data       | Данные Nginx                                     |
| nginx_conf       | Конфигурации Nginx                              |
| certbot_data     | TLS-сертификаты Let's Encrypt                   |
| vhost_data       | Конфигурации виртуальных хостов Nginx           |
| html_data        | Статические файлы, доступные через Nginx        |

⚙️ Использование

Клонируйте репозиторий:

git clone https://github.com/georgelucker/wg-easy-ssl.git
cd wg-easy-ssl

1. Укажите домен и email

Откройте docker-compose.yml и замените следующие значения на свои:

      - WG_HOST=vpn.your-domain.ru              # ← ваш реальный домен
      - VIRTUAL_HOST=vpn.your-domain.ru         # ← тот же или другой домен
      - LETSENCRYPT_HOST=vpn.your-domain.ru     # ← домен для HTTPS
      - LETSENCRYPT_EMAIL=yourmail@gmail.com    # ← ваша почта

⚠️ Убедитесь, что ваш домен указывает на IP-адрес сервера, где запускается этот compose.

2. Сгенерируйте хеш пароля

Чтобы задать пароль администратора для интерфейса wg-easy, необходимо использовать bcrypt-хеш. Вы можете сгенерировать его так:

docker run --rm -it ghcr.io/wg-easy/wg-easy:14 sh -c "node -e \"console.log(require('bcrypt').hashSync('ВашПароль', 10))\""

Скопируйте полученную строку и вставьте в переменную PASSWORD_HASH без изменений:

- PASSWORD_HASH=$2b$10$3kK...

3. Запуск

После настройки переменных, запустите контейнеры:

docker compose up -d

4. Откройте интерфейс

Через несколько секунд после запуска откройте в браузере:

https://vpn.your-domain.ru

Войдите с паролем, который вы зашифровали ранее.

5. Остановка

Чтобы остановить и удалить контейнеры:

docker compose down

✅ Проверка портов

Убедитесь, что на сервере открыты:

51820/udp — WireGuard

80/tcp и 443/tcp — для получения и работы HTTPS-сертификатов

🌍 Требования

Docker и Docker Compose установлены

Домен указывает на IP-адрес сервера

Сервер имеет выход в интернет

🔄 Автообновление сертификатов

nginx-letsencrypt автоматически продлевает сертификаты и перезагружает прокси при необходимости. Никаких ручных действий не требуется.
