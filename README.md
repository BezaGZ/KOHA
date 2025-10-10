# 🐋 Koha Community en Docker para CUNORI

Este repositorio contiene la configuración de Docker para desplegar una instancia local de **Koha Community**, adaptada específicamente para el proyecto de la **Biblioteca Digital del Centro Universitario de Oriente (CUNORI)**.

El objetivo es proporcionar un entorno de desarrollo y pruebas que sea fácil de instalar y gestionar.

---

## 🚀 Inicio Rápido (Para Usuarios Avanzados)

Si ya tienes experiencia y solo quieres levantar el proyecto:

```bash
# 1. Clona el repositorio
git clone https://github.com/BezaGZ/KOHA.git

# 2. Entra al directorio
cd KOHA

# 3. Levanta los servicios
docker-compose  up -d --build
```

# 📚 **KOHA – Guía de Implementación con Docker**

Este repositorio contiene todo lo necesario para desplegar una instancia de Koha utilizando Docker. Puedes elegir entre una instalación limpia o restaurar una copia de seguridad.

---

## ⚙️ Opción 1: Instalación Limpia (Desde Cero)

### **1. Requisitos Previos**
1. **Git** (para descargar el repositorio)
2. **Docker** y **Docker Compose** (para gestionar los contenedores)

---

### **2. Clona el Repositorio**
```bash
git clone https://github.com/BezaGZ/KOHA.git
```

---

### **3. Accede a la Carpeta de Instalación Limpia**
```bash
cd KOHA/KOHACUNORI
```

---

### **4. Construye y Levanta los Servicios**
```bash
docker-compose up -d --build
```
> _La primera vez puede tardar varios minutos._

---

### **5. Obtén la Contraseña de Administrador**
```bash
docker exec -ti koha koha-passwd biblioteca
```
Copia la contraseña que aparece.

---

### **6. Accede a Koha**
- **URL de Administración:** [http://localhost:8080](http://localhost:8080)
- **Usuario:** `koha_biblioteca`
- **Contraseña:** _(la que obtuviste en el paso anterior)_

---

## 🚚 Opción 2: Migración (Backup y Restauración)

Puedes migrar una instancia existente de Koha mediante backup y restauración.

### **Importante: ¿Qué copia de seguridad usar?**
- **Crear tu propia copia:** Sigue la Parte A.
- **Usar la copia incluida:** Hay un archivo `koha-db-backup_2025-10-09_16-18-11.tar.gz` en la carpeta `KOHACUNORIBACKUP`. Si quieres restaurar esa versión, ve directo a la Parte B.

---

### **🅰️ Parte A: Crear la Copia de Seguridad (Máquina de Origen)**

1. **Detén los servicios de Koha:**
    ```bash
    docker-compose down
    ```
2. **Crea el archivo de respaldo:**
    ```bash
    # Reemplaza 'koha-datos' si tu volumen tiene otro nombre
    # Cambia el nombre del backup si lo deseas
    docker run --rm -v koha-datos:/data -v "$(pwd)":/backup alpine tar -czf /backup/koha-db-backup_FECHA.tar.gz -C /data .
    ```
    > _Ponle un nombre descriptivo, por ejemplo: `koha-db-backup_2025-10-10.tar.gz`_
3. **Transfiere los archivos:**
    - Copia la carpeta `KOHACUNORIBACKUP` y tu archivo `.tar.gz` a la nueva máquina.
    - Coloca el archivo `.tar.gz` dentro de `KOHACUNORIBACKUP`.

---

### **🅱️ Parte B: Restaurar la Copia de Seguridad (Nueva Máquina)**

1. **Navega a la carpeta de restauración:**
    ```bash
    cd KOHA/KOHACUNORIBACKUP
    ```
2. **Crea el volumen de Docker:**
    ```bash
    docker volume create koha-datos
    ```
3. **Restaura los datos desde el backup:**
    ```bash
    # Reemplaza el nombre del archivo con el de tu backup
    docker run --rm -v koha-datos:/data -v "$(pwd)":/backup alpine tar xzf /backup/koha-db-backup_FECHA.tar.gz -C /data
    ```
4. **Construye y levanta el entorno:**
    ```bash
    docker-compose up -d --build
    ```
5. **Verifica que todo esté funcionando:**
    ```bash
    docker-compose ps
    ```
    Si todo aparece como `Up` o `running`, ¡la migración fue exitosa!

---

### **Acceso Final**
- **URL:** [http://localhost:8080](http://localhost:8080)
- **Usuarios y contraseñas:** Los mismos que tenía tu instancia original.

---

## 📝 **Notas Adicionales**
- Si tienes dudas, revisa los archivos `docker-compose.yml` de cada carpeta.
- Puedes personalizar los nombres de volúmenes y archivos de backup según tu entorno.

---

## ✨ Créditos

Este proyecto está basado en la imagen y configuración de **Kedu SCCL**, con adaptaciones para un entorno local y los requerimientos específicos de **CUNORI**.