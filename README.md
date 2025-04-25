# commerce Platform

Este documento explica cómo **construir, ejecutar y verificar** el entorno completo del proyecto **commerce** usando Docker.

Incluye backend (Rust 1.75), frontend (Vue + Vite) y base de datos MySQL, conforme a las guías oficiales del proyecto.

---

## ⚙️ Requisitos previos

Antes de ejecutar el entorno asegurate de tener instalados:

* **Docker** y **Docker Compose**
* **Rust 1.75+** (para desarrollo local, opcional)
* **Node.js 20+** (solo si querés correr el frontend fuera del contenedor)

---

## Ejecución paso a paso

### Backend (API Rust)

```bash
cd core-service/
./build-container.sh && ./start-service.sh
```

Esto:

* Construye la imagen **multi-stage** `cargo-build → debian-slim`).
* Levanta la API en `http://localhost:9090`.
* Conecta automáticamente con el contenedor MySQL.

---

### Frontend (Vue + Vite)

```bash
cd ../ui-portal/
docker compose up -d
```

Esto inicia:

* El contenedor del frontend servido con **Nginx**.
* Accesible en `http://localhost:5000`.

---

## Verificación de datos

### Verificar productos cargados

```bash
docker exec -it stellar-mysql mysql -u root -p stellar_commerce \
  -e "SELECT COUNT(*) FROM catalog.products;"
```

### Verificar órdenes cargadas

```bash
docker exec -it stellar-mysql mysql -u root -p stellar_commerce \
  -e "SELECT COUNT(*) FROM orders.history;"
```

---

## Probar el backend (API)

### Llamada básica

```bash
curl -i http://localhost:9090/v2/listings
```

Deberías obtener una respuesta `200 OK` con un listado de productos y un token **JWT-RS256** generado automáticamente.

---

## Accesos rápidos

| Servicio     | URL                                                            | Descripción                     |
| ------------ | -------------------------------------------------------------- | ------------------------------- |
| **Frontend** | [http://localhost:5000](http://localhost:5000)                 | UI Vue (Vite + WindiCSS)        |
| **Backend**  | [http://localhost:9090](http://localhost:9090)                 | API REST Rust                   |
| **Docs**     | [http://localhost:9090/docs](http://localhost:9090/docs)       | Documentación Swagger           |
| **MySQL**    | localhost:3306                                                 | Base de datos `stellar_commerce` |


# PR Merge: 2026-07-26 06:14:12
