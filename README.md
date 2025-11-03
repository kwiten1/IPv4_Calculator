# IPv4 Calculator - Калькулятор подсетей

Веб-приложение для расчета IPv4-подсетей с интуитивно понятным интерфейсом. Работает в Docker-контейнере с Nginx.

<p align="center"> <img width="727" height="893" src="https://github.com/user-attachments/assets/c66c3f4a-1bd5-43c2-a992-f5c41787bb89" alt="Интерфейс приложения" /> </p>

## Возможности

- Расчет сетевых параметров по IP-адресу и маске подсети
- Разделение сети на подсети с детальной информацией
- Двоичное представление IP-адресов и масок
- Адаптивный дизайн для всех устройств
- Быстрое развертывание с помощью Docker

---

## Быстрое развертывание
## Структура проекта для развёртывания без nginx.conf

```
ipv4-calculator/
├── docker-compose.yml      # Конфигурация Docker Compos
├── html/                   # Статические файлы приложения
│   └── index.html          # Основное веб-приложение
└── README.md               # Документация проекта
```

Запустите контейнер 

docker-compose up -d

---

### Предварительные требования

- Docker
- Docker Compose

---

### Шаг 1: Создайте директорию проекта

```
mkdir ipv4-calculator
cd ipv4-calculator
```

### Шаг 2: Создайте необходимые файлы

#### docker-compose.yml

```
version: '3.8'
services:
  nginx:
    image: nginx:alpine
    container_name: ipv4-calculator
    ports:
      - "3333:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
    restart: unless-stopped
    networks:
      - ipv4-network

networks:
  ipv4-network:
    driver: bridge
```

#### nginx.conf


```
server {
    listen 80;
    server_name localhost;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location ~* \.(css|js|jpg|jpeg|png|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;

    gzip on;
    gzip_types text/plain text/css application/javascript application/json;
}
```

#### Создайте папку для HTML-файлов

```
mkdir html
```

Поместите ваш файл index.html в папку html/.

---

### Шаг 3: Запустите контейнер


```
docker-compose up -d
```

### Шаг 4: Проверьте работу

Откройте браузер и перейдите по адресу:

[http://localhost:3333](http://localhost:3333)

---

## Управление контейнером

### Просмотр логов

```
docker-compose logs -f
```

### Остановка приложения

```
docker-compose down
```

### Перезапуск

```
docker-compose restart
```

---

## Структура проекта

```
ipv4-calculator/
├── docker-compose.yml      # Конфигурация Docker Compose
├── nginx.conf              # Конфигурация Nginx
├── html/                   # Статические файлы приложения
│   └── index.html          # Основное веб-приложение
└── README.md               # Документация проекта
```

---

## Использование приложения

### Вкладка "Калькулятор"

1. Введите IP-адрес (например, 192.168.1.100)
2. Выберите маску подсети в формате CIDR (например, /24)
3. Получите полную информацию о сети:
    - Адрес сети
    - Широковещательный адрес
    - Диапазон хостов
    - Количество хостов
    - Двоичное представление

### Вкладка "Разделение на подсети"

1. Укажите основную сеть (IP + маска)
2. Выберите количество подсетей или необходимое число хостов в каждой
3. Получите детальную информацию по каждой подсети:
    - Новая маска
    - Диапазон адресов
    - Количество хостов
    - Двоичные данные
