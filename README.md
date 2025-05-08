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

## ⚙️ Использование

1. Клонируйте репозиторий:
   ```bash
   git clone https://github.com/georgelucker/wg-easy-ssl.git
   cd wg-easy-ssl
