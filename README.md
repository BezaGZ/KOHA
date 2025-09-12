# 🐋 Koha Community en Docker para CUNORI

Este repositorio contiene la configuración de Docker para desplegar una instancia local de **Koha Community**, adaptada específicamente para el proyecto de la **Biblioteca Digital del Centro Universitario de Oriente (CUNORI)**.

El objetivo es proporcionar un entorno de desarrollo y pruebas que sea fácil de instalar y gestionar.

---

## 📖 Documentación Completa y Manual de Instalación

Toda la guía de instalación, configuración, uso y detalles técnicos del proyecto se encuentran en nuestro sitio de documentación oficial.

### **[➡️ Acceder a la Documentación Completa](https://ingenieria-usac-edu.gitbook.io/documentacion-koha-community/)**

La documentación incluye:

* **Manual de Instalación** paso a paso.
* **Manual de Configuración Inicial** para adaptar Koha.
* **Manual de Operación y Gestión** para el uso diario.
* **Manual de Respaldo y Recuperación** de datos.

---

## 🚀 Inicio Rápido (Para Usuarios Avanzados)

Si ya tienes experiencia y solo quieres levantar el proyecto:

```bash
# 1. Clona el repositorio
git clone [https://github.com/BezaGZ/KOHA.git](https://github.com/BezaGZ/KOHA.git)

# 2. Entra al directorio
cd KOHA

# 3. Levanta los servicios
docker-compose -f docker-compose.yml.local up -d --build

## Creditos
Créditos
Este proyecto está basado en la imagen y configuración de Kedu SCCL, con adaptaciones para un entorno local y los requerimientos específicos de CUNORI.
