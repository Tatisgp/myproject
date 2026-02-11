# Docker Setup - Laravel 12

Proyecto completamente dockerizado y funcionando ✅

## 🚀 Acceso rápido

- **Aplicación web**: http://localhost
- **Base de datos**: localhost:3306 (usuario: laravel / password: password)
- **PHP-FPM**: localhost:9000

## 📋 Estructura Docker

```
myproject/
├── Dockerfile              # Imagen PHP 8.2-FPM
├── docker-compose.yml      # Orquestación de servicios
├── .dockerignore           # Archivos a excluir
├── docker/nginx/
│   └── conf.d/app.conf    # Configuración Nginx
├── app/                   # Código Laravel
├── storage/               # Almacenamiento
├── bootstrap/             # Bootstrap de Laravel
└── [otros archivos...]
```

## 🔧 Servicios ejecutándose

1. **App** (PHP-FPM 8.2) → Puerto 9000
2. **Nginx** (Servidor web) → Puertos 80/443
3. **MySQL** (Base de datos) → Puerto 3306

## 💻 Comandos útiles desde `/myproject/`

### Ver estado de contenedores
```bash
docker-compose ps
```

### Logs en tiempo real
```bash
docker-compose logs -f app
# o todos los servicios
docker-compose logs -f
```

### Entrar a bash en PHP
```bash
docker-compose exec app bash
```

### Comandos Artisan
```bash
# Crear controlador
docker-compose exec app php artisan make:controller NombreController

# Crear modelo con migración
docker-compose exec app php artisan make:model Nombre -m

# Ver rutas
docker-compose exec app php artisan route:list

# Tinker (REPL)
docker-compose exec app php artisan tinker
```

### Migraciones
```bash
docker-compose exec app php artisan migrate
docker-compose exec app php artisan migrate:rollback
```

### Composer
```bash
docker-compose exec app composer install
docker-compose exec app composer require nombre/paquete
docker-compose exec app composer update
```

### Tests
```bash
docker-compose exec app php artisan test
```

## 🛑 Gestión de contenedores

### Detener
```bash
docker-compose down
```

### Reiniciar
```bash
docker-compose restart
```

### Reconstruir imagen
```bash
docker-compose build --no-cache
docker-compose up -d
```

### Eliminar todo (⚠️ Borra la BD)
```bash
docker-compose down -v
```

## 🔐 Credenciales

### Base de Datos
- **Host**: db
- **Puerto**: 3306
- **Database**: laravel
- **Usuario**: laravel
- **Contraseña**: password
- **Root**: root

### Variables de entorno
Edita el `docker-compose.yml` para cambiar variables. Las principales están bajo `environment:` del servicio `app`.

## 🐛 Troubleshooting

### "Connection refused" en MySQL
Espera 10-15 segundos, MySQL necesita tiempo para iniciar.

### Permisos en storage
```bash
docker-compose exec app chown -R www-data:www-data /app/storage
docker-compose exec app chmod -R 775 /app/storage
```

### Limpiar caché
```bash
docker-compose exec app php artisan cache:clear
docker-compose exec app php artisan config:clear
docker-compose exec app php artisan view:clear
```

## 📝 Notas

- Los cambios en archivos PHP se reflejan automáticamente
- Las dependencias están en volumen para persistencia
- La base de datos se almacena en `dbdata` (volumen persistente)
- Edita archivos normalmente en tu editor, el contenedor los detecta
