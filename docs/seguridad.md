# 🛡️ Análisis de Seguridad - TechStore

## Riesgos identificados en las transacciones

### 1. SQL Injection 💉
**Descripción:** Un atacante inserta código SQL malicioso a través de formularios o parámetros de URL para manipular la base de datos.

**Ejemplo de ataque:**
```sql
-- Input malicioso en login:
email: ' OR '1'='1
-- Resultado: bypass de autenticación
```

**Mitigación:** Consultas parametrizadas (prepared statements):
```javascript
db.query('SELECT * FROM users WHERE email = ?', [email]);
```

---

### 2. Fraude en pagos 💸
**Descripción:** Robo o uso indebido de datos de tarjetas de crédito durante la compra.

**Mitigación:**
- Pasarela de pagos certificada **PCI-DSS**
- **Tokenización** de datos sensibles
- Verificación **3D Secure (3DS)**
- Nunca almacenar datos de tarjetas en la BD propia

---

### 3. Robo de credenciales 🔓
**Descripción:** Acceso a contraseñas almacenadas en texto plano o con hashes débiles.

**Mitigación:**
- Hash con **bcrypt** o **argon2** (no MD5/SHA1)
- Uso de **salt** único por usuario
- Política de contraseñas robustas (mínimo 8 caracteres, mezcla)

---

### 4. Man-in-the-Middle (MITM) 🕵️
**Descripción:** Interceptación de la comunicación entre cliente y servidor.

**Mitigación:**
- **HTTPS** obligatorio en toda la aplicación
- Certificados SSL/TLS válidos
- HSTS (HTTP Strict Transport Security)

---

### 5. XSS (Cross-Site Scripting) ⚡
**Descripción:** Inyección de scripts maliciosos en el frontend que se ejecutan en el navegador de otros usuarios.

**Mitigación:**
- Sanitización y escape de inputs del usuario
- **Content Security Policy (CSP)**
- Validación tanto en frontend como backend

---

### 6. CSRF (Cross-Site Request Forgery) 🎭
**Descripción:** Un atacante engaña al usuario autenticado para que ejecute acciones no deseadas.

**Mitigación:**
- **Tokens CSRF** en formularios
- Validación del header `Origin` y `Referer`
- Cookies con flag `SameSite`

---

### 7. Acceso no autorizado 🚫
**Descripción:** Usuarios sin permisos acceden a recursos privilegiados.

**Mitigación:**
- Autenticación con **JWT**
- Control de roles a nivel BD vía **DCL**:
```sql
GRANT SELECT, INSERT ON products TO rol_cliente;
REVOKE DELETE ON users FROM rol_cliente;
```

---

## ✅ Validación de transacciones (ejemplo: compra de smartphone)

```sql
BEGIN TRANSACTION;
  -- 1. Verificar stock (consulta parametrizada)
  SELECT stock FROM products WHERE id = ?;

  -- 2. Descontar stock
  UPDATE products SET stock = stock - 1
  WHERE id = ? AND stock > 0;

  -- 3. Registrar la orden
  INSERT INTO orders (user_id, total, fecha)
  VALUES (?, ?, NOW());

  -- 4. Procesar pago
  IF pago_exitoso THEN
      COMMIT;     -- ✅ Confirma compra
  ELSE
      ROLLBACK;   -- ❌ Revierte todo
  END IF;
```

Este patrón garantiza las propiedades **ACID**:
- **Atomicidad:** todas las operaciones se ejecutan o ninguna
- **Consistencia:** el stock nunca queda negativo
- **Aislamiento:** dos compras simultáneas no interfieren
- **Durabilidad:** los cambios confirmados son permanentes
