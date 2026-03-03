# Lab 02 — Integrar Keycloak (Bootstrap Manual)

## Paso 01 — Instalar keycloak-js

---

## 🎯 Objetivo

Instalar el **adapter oficial de Keycloak para aplicaciones JavaScript** y preparar la aplicación Angular para inicializar autenticación antes del arranque.

En este laboratorio trabajaremos:

* Sin wrapper
* Sin magia
* Sin automatismos

Queremos entender cómo funciona realmente la integración.

---

## 📍 Ubicación

Desde:

```
apps/angular-app/angular-app
```

---

## 🛠 Instalar dependencia oficial

Ejecuta:

```bash
npm install keycloak-js
```

---

## 📦 ¿Qué es keycloak-js?

`keycloak-js` es el adapter oficial de Keycloak para aplicaciones SPA.

Nos permite:

* Iniciar sesión (Authorization Code + PKCE)
* Gestionar tokens (access + refresh)
* Detectar expiración
* Hacer logout
* Consultar claims del usuario

Importante:

👉 No es un wrapper Angular
👉 No gestiona el ciclo de vida de Angular
👉 No bloquea el bootstrap automáticamente

Eso lo haremos nosotros manualmente.

---

## 🧠 Concepto clave — ¿Qué significa bootstrapear autenticación?

En una SPA autenticada:

1. El usuario puede venir redirigido desde Keycloak.
2. Hay que procesar el `code` que viene en la URL.
3. Hay que intercambiarlo por tokens.
4. Hay que validar la sesión.
5. Solo entonces Angular debería arrancar completamente.

Si no hacemos esto correctamente:

* Los guards pueden ejecutarse antes de que Keycloak esté listo.
* Puede haber loops de login.
* Puede fallar la validación del token.

Por eso el siguiente paso será:

👉 Inicializar Keycloak antes de arrancar Angular.

---

## ✅ Resultado esperado

Tras este paso:

* La dependencia `keycloak-js` estará instalada.
* La aplicación aún no está autenticada.
* Estamos listos para crear el servicio de inicialización.

---

## ➡️ Siguiente paso

En el siguiente archivo:

```
02-configurar-init.md
```

Crearemos el servicio que:

* Instancia Keycloak
* Ejecuta `init()`
* Bloquea el bootstrap de Angular hasta resolver autenticación.
