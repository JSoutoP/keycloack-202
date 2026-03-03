# Lab 02 — Paso 04

## Consumir endpoint protegido desde Angular enviando el token

---

# 🎯 Objetivo

Desde el componente `Protected` actual:

* Añadir botón
* Enviar el `access_token`
* Mostrar respuesta del backend
* Sin romper el refresh automático existente

---

# 🧩 BLOQUE 1 — Añadir HttpClient

### 🔹 Añadir import

En la parte superior del archivo:

```ts
import { HttpClient, HttpClientModule } from '@angular/common/http';
```

---

### 🔹 Añadir HttpClientModule en el decorador

Dentro de `@Component` → `imports:`

Añadir:

```ts
HttpClientModule
```

Ejemplo:

```ts
imports: [CommonModule, AsyncPipe, HttpClientModule],
```

---

### 🔹 Inyectar HttpClient

Dentro de la clase:

```ts
private http = inject(HttpClient);
```

---

# 🧩 BLOQUE 2 — Variables de estado

Dentro de la clase `Protected`, añadir:

```ts
apiResponse: any = null;
apiError: any = null;
```

---

# 🧩 BLOQUE 3 — Método callBackend()

Añadir dentro de la clase:

```ts
callBackend() {

  const token = this.keycloak.token;

  if (!token) {
    this.apiError = 'No token available';
    return;
  }

  this.http.get('http://localhost:3000/protected', {
    headers: {
      Authorization: `Bearer ${token}`
    }
  }).subscribe({
    next: (res) => {
      this.apiResponse = res;
      this.apiError = null;
    },
    error: (err) => {
      this.apiError = err.error;
      this.apiResponse = null;
    }
  });
}
```

---

# 🧩 BLOQUE 4 — Añadir botón en `protected.html`

Añadir en el template:

```html
<hr>

<button (click)="callBackend()">
  Call Protected API
</button>
```

---

# 🧩 BLOQUE 5 — Mostrar resultado

Añadir debajo del botón:

```html
<div *ngIf="apiResponse">
  <h3>API Response</h3>
  <pre>{{ apiResponse | json }}</pre>
</div>

<div *ngIf="apiError">
  <h3 style="color:red;">Error</h3>
  <pre>{{ apiError | json }}</pre>
</div>
```

---

# ⚠️ Posible error CORS

Si aparece error en consola:

En backend Express:

```js
const cors = require('cors');
app.use(cors());
```

Reiniciar backend.

---

# ✅ Resultado esperado

Al pulsar el botón:

* Se envía el token actual
* El backend valida firma RS256 vía JWKS
* Devuelve los claims del usuario

---

# 🧠 Qué acabamos de integrar

SPA autenticada
→ Token OIDC
→ Header Authorization
→ API protegida
→ Validación criptográfica real
