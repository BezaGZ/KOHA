
# 🐋 Koha Community en Docker (Versión Local con Traducción)

Este proyecto permite construir y ejecutar una instancia **local de Koha Community** usando Docker. Utiliza una imagen basada en `debian:buster` e instala `koha-common`, conectándose a un contenedor MariaDB y Memcached.

---

## 📦 Estructura del proyecto

```
.
├── Dockerfile                  # Imagen personalizada de Koha
├── docker-compose.yml.local   # Definición de servicios (Koha, DB, Memcached)
├── entrypoint.sh              # Script de inicialización
└── templates/                 # Archivos de configuración dinámicos
    ├── koha-common.cnf
    ├── koha-no-domain.conf
    ├── koha-sites.conf
    └── koha.conf
```

---

## ⚙️ ¿Qué hace este setup?

1. Construye una imagen Docker personalizada con Koha instalado.
2. Usa `entrypoint.sh` para:
   - Crear la base de datos (`koha-create`)
   - Configurar Apache
   - Activar Plack y Zebra
   - Instalar los idiomas seleccionados
3. Arranca la interfaz STAFF y el OPAC en puertos accesibles desde localhost.

---

## 🛠️ Requisitos previos

- Docker
- Docker Compose

---

## 🚀 Cómo iniciar

### 1. Construcción e inicio

```bash
docker-compose -f docker-compose.yml.local up -d --build
```

Esto creará y levantará 3 servicios:
- `koha` → servicio principal
- `koha-db` → MariaDB 10.3
- `memcached` → caché para rendimiento

### 2. Obtener credenciales

```bash
docker exec -ti koha koha-passwd defaultlibraryname
```

El usuario será:
```bash
koha_defaultlibraryname
```

---

## 🌐 Acceso

- STAFF (administración): [http://localhost:8080](http://localhost:8080)
- OPAC (público): [http://localhost](http://localhost)

---

## 🌍 Idiomas incluidos

Este setup incluye soporte multilenguaje:
```yaml
KOHA_TRANSLATE_LANGUAGES: "es-ES,en"
```

Puedes modificar este valor para agregar más idiomas en el archivo `docker-compose.yml.local`.

---

## 📌 Notas

- Este entorno es solo para pruebas/desarrollo local.
- La instancia se llama `defaultlibraryname` por defecto. Puedes cambiarla con la variable `LIBRARY_NAME`.
- Si el contenedor ya fue levantado una vez, se detectará y no repetirá la configuración.

---

## ✨ Créditos

Basado en la imagen de [Kedu SCCL](https://github.com/Kedu-SCCL/docker-koha-community), adaptado para entorno local.
