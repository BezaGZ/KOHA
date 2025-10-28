# Koha Community en Docker para CUNORI

Este repositorio contiene la configuración de Docker para desplegar una instancia local de **Koha Community**, adaptada específicamente para el proyecto de la **Biblioteca Digital del Centro Universitario de Oriente (CUNORI)**.

El objetivo es proporcionar un entorno de desarrollo y pruebas que sea fácil de instalar y gestionar, ahora con soporte para dos volúmenes de datos separados: `koha-datos` y `koha-data`.

---

## ⚙️ Requisitos Previos

Antes de comenzar, asegúrate de tener instalados los siguientes componentes:

1. **Git** (para clonar el repositorio)  
2. **Docker** y **Docker Compose** (para gestionar los contenedores y volúmenes)  

---

## 🚀 Instalación Limpia (Desde Cero)

Sigue estos pasos para realizar una instalación limpia que crea automáticamente los dos volúmenes necesarios.

### 1. Clonar el repositorio

```bash
git clone https://github.com/BezaGZ/KOHA.git
```

### 2. Acceder a la carpeta de instalación limpia

```bash
cd KOHA/KOHACUNORI
```

### 3. Construir y levantar los servicios con Docker Compose

```bash
docker-compose up -d --build
```

> _Este proceso creará automáticamente los volúmenes `koha-datos` y `koha-data` y puede tardar varios minutos la primera vez._

### 4. Obtener la contraseña de administrador

```bash
docker exec -ti koha koha-passwd biblioteca
```

Copia la contraseña que aparece para usarla en el acceso.

### 5. Acceder a Koha

- **URL de Administración:** [http://localhost:80](http://localhost:8080)  
- **Usuario:** `koha_biblioteca`  
- **Contraseña:** _(la que obtuviste en el paso anterior)_  

---

## 🔄 Migración (Backup y Restauración Completa)

Para migrar una instancia existente de Koha que utiliza los dos volúmenes `koha-datos` y `koha-data`, sigue las instrucciones a continuación.

### 📦 Parte A: Crear la Copia de Seguridad (Máquina de Origen)

1. Detén los servicios de Koha:

    ```bash
    docker-compose down
    ```

2. Crea los archivos de respaldo para cada volumen:

    ```bash
    # Backup de koha-datos
    docker run --rm -v koha-datos:/data -v "$(pwd)":/backup alpine tar -czf /backup/koha-db-backup_datos_FECHA.tar.gz -C /data .

    # Backup de koha-data
    docker run --rm -v koha-data:/data -v "$(pwd)":/backup alpine tar -czf /backup/koha-db-backup_data_FECHA.tar.gz -C /data .
    ```

    > _Reemplaza `FECHA` con la fecha actual, por ejemplo: `2025-10-10`._

3. Transfiere los archivos `.tar.gz` y la carpeta `KOHACUNORIBACKUP` a la nueva máquina.

---

### 📥 Parte B: Restaurar la Copia de Seguridad (Nueva Máquina)

1. Navega a la carpeta de restauración:

    ```bash
    cd KOHA/KOHACUNORIBACKUP
    ```

2. Crea los volúmenes de Docker:

    ```bash
    docker volume create koha-datos
    docker volume create koha-data
    ```

3. Restaura los datos desde los backups:

    ```bash
    # Restaurar koha-datos
    docker run --rm -v koha-datos:/data -v "$(pwd)":/backup alpine tar xzf /backup/koha-db-backup_datos_FECHA.tar.gz -C /data

    # Restaurar koha-data
    docker run --rm -v koha-data:/data -v "$(pwd)":/backup alpine tar xzf /backup/koha-db-backup_data_FECHA.tar.gz -C /data
    ```

4. Construye y levanta el entorno:

    ```bash
    docker-compose up -d --build
    ```

5. Verifica que los servicios estén activos:

    ```bash
    docker-compose ps
    ```

    Si los contenedores aparecen como `Up` o `running`, la migración fue exitosa.

---

## 🛠️ Scripts Recomendados para Backup y Restauración

Para facilitar el proceso de backup y restauración, aquí tienes dos scripts recomendados:

### Backup en Windows (`backup_koha.bat`)

```batch
@echo off
set FECHA=%DATE:~6,4%-%DATE:~3,2%-%DATE:~0,2%
docker-compose down
docker run --rm -v koha-datos:/data -v "%cd%":/backup alpine tar -czf /backup/koha-db-backup_datos_%FECHA%.tar.gz -C /data .
docker run --rm -v koha-data:/data -v "%cd%":/backup alpine tar -czf /backup/koha-db-backup_data_%FECHA%.tar.gz -C /data .
docker-compose up -d --build
echo Backup completado con fecha %FECHA%.
pause
```

### Restauración en Linux/Mac (`restore_koha.sh`)

```bash
#!/bin/bash
FECHA=$1

if [ -z "$FECHA" ]; then
  echo "Uso: ./restore_koha.sh YYYY-MM-DD"
  exit 1
fi

docker-compose down
docker volume create koha-datos
docker volume create koha-data

docker run --rm -v koha-datos:/data -v "$(pwd)":/backup alpine tar xzf /backup/koha-db-backup_datos_${FECHA}.tar.gz -C /data
docker run --rm -v koha-data:/data -v "$(pwd)":/backup alpine tar xzf /backup/koha-db-backup_data_${FECHA}.tar.gz -C /data

docker-compose up -d --build
echo "Restauración completada para la fecha ${FECHA}."
```

> _Asegúrate de dar permisos de ejecución al script de Linux/Mac con:_  
> `chmod +x restore_koha.sh`

---

## 🔑 Acceso Final

- **URL:** [http://localhost:80](http://localhost:8080)  
- **Usuario:** `koha_biblioteca`  
- **Contraseña:** _(la establecida o la restaurada desde backup)_

---

## 📝 Notas Adicionales

- Revisa los archivos `docker-compose.yml` para entender la configuración de volúmenes y servicios.  
- Puedes personalizar los nombres de los volúmenes y archivos de backup según tu entorno, pero asegúrate de actualizar los comandos y scripts en consecuencia.  
- Mantén los backups en un lugar seguro para evitar pérdida de datos.  
- Si tienes dudas o problemas, consulta la documentación oficial de Koha y Docker.

---

## 🙌 Créditos

Este proyecto está basado en la imagen y configuración de **Kedu SCCL**, con adaptaciones para un entorno local y los requerimientos específicos de **CUNORI**.
