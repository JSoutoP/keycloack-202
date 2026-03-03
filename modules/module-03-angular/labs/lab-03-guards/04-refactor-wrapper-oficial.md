# Lab 03 — Guards

## Paso 04 — Refactorizar usando el wrapper oficial

---

## 🎯 Objetivo

Sustituir la integración manual basada en:

* `keycloak-js`
* Bootstrap manual en `main.ts`
* Servicio personalizado
* Guard manual

Por la integración oficial con:

```
keycloak-angular
```

Y comparar la diferencia de complejidad.

---

# 🧠 ¿Por qué hacemos este refactor ahora?

En los pasos anteriores hemos:

* Bootstrappeado autenticación manualmente.
* Gestionado `init()`.
* Creado un guard manual.
* Controlado redirecciones.

Ahora que entendemos el mecanismo interno, podemos simplificarlo usando el wrapper oficial.

---

# 🛠 Paso 1 — Instalar wrapper oficial

Desde:

```bash
apps/angular-app/angular-app
```

Ejecutar:

```bash
npm install keycloak-angular keycloak-js
```

---

# 🛠 Paso 2 — Eliminar bootstrap manual

Editar:

```
src/main.ts
```

Eliminar este bloque:

```typescript
initializeKeycloak().then(() => {
  bootstrapApplication(AppComponent);
});
```

Y reemplazar por:

```typescript
import { bootstrapApplication } from '@angular/platform-browser';
import { provideKeycloak } from 'keycloak-angular';
import { AppComponent } from './app/app.component';

bootstrapApplication(AppComponent, {
  providers: [
    provideKeycloak({
      config: {
        url: 'http://localhost:8080',
        realm: 'training',
        clientId: 'spa-client'
      },
      initOptions: {
        onLoad: 'login-required',
        pkceMethod: 'S256'
      }
    })
  ]
}).catch(err => console.error(err));
```

---

## 🧠 Qué cambia aquí

El wrapper ahora:

* Registra internamente un `APP_INITIALIZER`.
* Ejecuta `keycloak.init()` automáticamente.
* Bloquea el bootstrap hasta resolver autenticación.
* Gestiona el ciclo completo.

Ya no necesitamos:

```
src/app/keycloak.service.ts
```

---

# 🛠 Paso 3 — Reemplazar el guard manual

Abrir:

```
src/app/auth.guard.ts
```

Reemplazar contenido por:

```typescript
import { inject } from '@angular/core';
import { CanActivateFn } from '@angular/router';
import Keycloak from 'keycloak-js';

export const authGuard: CanActivateFn = async (route, state) => {
  const keycloak = inject(Keycloak);

  if (!keycloak.authenticated) {
    await keycloak.login({
      redirectUri: window.location.origin + state.url
    });
  }

  return true;
};
```

---

# 📊 Comparativa final

| Integración Manual              | Wrapper Oficial    |
| ------------------------------- | ------------------ |
| Servicio propio                 | ❌                  |
| init() manual                   | ❌                  |
| Bootstrap bloqueado manualmente | ❌                  |
| Guard custom                    | Simplificado       |
| Más código                      | Mucho menos código |

---

# 🧪 Validar funcionamiento

Reiniciar aplicación:

```bash
npm start
```

Probar:

```
http://localhost:4200/protected
```

El comportamiento debe ser idéntico al paso anterior.

---

# 🧠 Qué hemos aprendido

1️⃣ Cómo funciona realmente `keycloak-js`.
2️⃣ Qué significa bootstrapear autenticación.
3️⃣ Cómo proteger rutas manualmente.
4️⃣ Cómo el wrapper simplifica toda la integración.

---

# 📌 Estado final del Lab 03

✔ Integración profesional
✔ Código simplificado
✔ Guard limpio
✔ Arquitectura lista para producción
