# 🛒 Arquitectura TechStore

**Actividad Formativa - Semana 7**
Módulo: Taller de Plataformas Web (CIB302)
Unidad 3: Introducción a los sistemas de bases de datos

---

## 👥 Integrantes

- Joaquin Rojas
- Boris Fernández

---

## 📌 Caso

**TechStore** es una tienda online que vende productos electrónicos (smartphones, laptops, tabletas y accesorios tecnológicos). La empresa requiere implementar una nueva plataforma para gestionar:

- Inventario de productos
- Transacciones de clientes
- Datos de usuarios

---

## ⚖️ Decisión técnica: SQL vs NoSQL

Se eligió una **base de datos SQL (PostgreSQL / MySQL)** por las siguientes razones:

| Requerimiento de TechStore | Justificación SQL |
|---|---|
| Pagos y transacciones | Garantiza propiedades **ACID** |
| Relaciones User-Order-Product | Modelo **relacional** nativo |
| Stock e inventario | Evita **anomalías de concurrencia** |
| Reportes (ventas, top productos) | **JOINs** nativos optimizados |
| Datos estructurados | **Esquema fijo** y consistente |

NoSQL podría ser útil para logs o búsquedas, pero no es apropiado para transacciones financieras que requieren consistencia fuerte.

---

## 🛡️ Seguridad

**Riesgos identificados:** SQL Injection, Fraude en pagos, Robo de credenciales, MITM, XSS, CSRF, Acceso no autorizado.

**Medidas implementadas:** Consultas parametrizadas, HTTPS/SSL, Hash bcrypt, JWT, PCI-DSS, Tokens CSRF, DCL (GRANT/REVOKE).

---

## 📝 Instrucciones SQL utilizadas

| Tipo | Comandos | Uso en TechStore |
|---|---|---|
| **DDL** | CREATE, ALTER, DROP | Crear tablas (users, products, orders) |
| **DML** | INSERT, UPDATE, DELETE, SELECT | Registrar compras, actualizar stock |
| **DCL** | GRANT, REVOKE | Control de roles (admin, cliente) |
| **TCL** | BEGIN, COMMIT, ROLLBACK | Transacciones atómicas |

---

## 📂 Estructura del repositorio

- README.md
- diagrama/ → Diagrama de arquitectura (.drawio y .pdf)
- docs/ → Análisis de seguridad detallado

---

## 🛠️ Herramienta utilizada

Diagrama creado con [Draw.io](https://app.diagrams.net)
