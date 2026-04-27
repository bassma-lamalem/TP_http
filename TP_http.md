# TP HTTP — Rapport Complet

---

## TP 1 : Exploration avec les DevTools

### Code de statut
- **200 

### Headers de requête (Request Headers)

| Header | Valeur |
|--------|--------|
| `:authority` | `httpbin.org` |
| `:method` | `GET` |
| `:path` | `/get` |
| `:scheme` | `https` |
| `accept` | `text/html,application/xhtml+xml,application/xml,*/*` |
| `accept-encoding` | `gzip, deflate, br, zstd` |
| `accept-language` | `fr-FR,fr,en-US,en,ar,ja` |
| `user-agent` | `Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/147` |
| `sec-ch-ua` | `"Google Chrome";v="147", "Chromium";v="147"` |
| `sec-ch-ua-mobile` | `?0` |
| `sec-ch-ua-platform` | `"Windows"` |
| `sec-fetch-site` | `none` |
| `sec-fetch-mode` | `navigate` |
| `sec-fetch-dest` | `document` |
| `sec-fetch-user` | `?1` |
| `upgrade-insecure-requests` | `1` |
| `priority` | `u=0, i` |

### Content-Type de la réponse
- `application/json`

### 1.3 Tester différentes méthodes

- **GET** : permet de récupérer les données envoyées par le serveur.
- **POST** : envoie des données au serveur et récupère une réponse en JSON.

### 1.4 Codes de statut

| URL | Code | Signification |
|-----|------|---------------|
| `https://httpbin.org/status/200` | 200 OK | Requête réussie |
| `https://httpbin.org/status/404` | 404 Not Found | Ressource introuvable |
| `https://httpbin.org/status/500` | 500 Internal Server Error | Erreur serveur |
| `https://httpbin.org/redirect/3` | 302 Found | Redirections multiples |

### Exercice : Tableau des requêtes HTTP

| URL | Méthode | Code | Content-Type |
|-----|---------|------|--------------|
| `httpbin.org/get` | GET | 200 | `application/json` |
| `httpbin.org/post` | POST | 200 | `application/json` |
| `httpbin.org/status/201` | GET | 201 | `text/html; charset=utf-8` |

---

## TP 2 : Maîtrise de cURL

### Différence entre `-i` et `-v`

| Option | Description |
|--------|-------------|
| `-i` (include) | Affiche les headers + le body |
| `-v` (verbose) | Affiche le détail complet de la requête (debug réseau) |

### Exercice avancé : commande cURL

```bash
curl -i -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -H "X-Custom-Header: MonHeader" \
  -d '{"action": "test", "value": 42}'
```

---

## TP 3 : API REST avec JavaScript

### Exercice pratique : `fetchWithRetry`

```javascript
async function fetchWithRetry(url, options, maxRetries) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      const response = await fetch(url, options);
      if (response.status >= 500) {
        throw new Error("Erreur serveur");
      }
      return response;
    } catch (error) {
      if (i === maxRetries - 1) {
        throw error;
      }
      await new Promise(resolve => setTimeout(resolve, 1000));
    }
  }
}
```

---

## TP 4 : Analyse des Headers de Sécurité

### Résultats

| Site | HSTS | X-Frame | CSP | Note |
|------|------|---------|-----|------|
| `github.com` | Oui | `deny` | Oui | A |
| `google.com` | Oui | `SAMEORIGIN` | Oui | B |
| `stackoverflow.com` | Oui | `SAMEORIGIN` | Oui | C |

---

## TP 5 : Cache HTTP

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>TP HTTP</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
            text-align: center;
            margin: 0;
            padding: 0;
        }
        header {
            background: #32042e;
            color: white;
            padding: 20px;
        }
        img {
            width: 300px;
            margin-top: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            border: none;
            background: #ff6976;
            color: white;
            cursor: pointer;
            border-radius: 5px;
            font-size: 16px;
        }
        button:hover {
            background: #2980b9;
        }
        footer {
            margin-top: 40px;
            padding: 10px;
            background: #ddd;
        }
    </style>
</head>
<body>

<header>
    <h1>TP Cache HTTP</h1>
    <p>Test du cache navigateur + headers HTTP</p>
</header>

<img src="https://httpbin.org/image/png" alt="image test cache">

<div>
    <button onclick="showMessage()">Tester JS</button>
</div>

<footer>
    <p>TP5 HTTP</p>
</footer>

<script>
    function showMessage() {
        alert("Cache HTTP fonctionne ");
    }
    console.log("Page chargée avec cache possible");
</script>

</body>
</html>
```

---

## Exercices Récapitulatifs

### Exercice 1 : Client HTTP minimaliste

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Mini HTTP Client</title>

    <style>
        body { font-family: Arial; background: #f5f5f5; padding: 20px; }
        .row { display: flex; gap: 10px; margin-bottom: 10px; }
        input, select, button, textarea {
            padding: 10px;
            border-radius: 10px;
            border: 1px solid #ccc;
        }
        input, textarea { flex: 1; }
        button { cursor: pointer; }
        .box { background: white; padding: 15px; border-radius: 15px; margin-top: 15px; }
        textarea { width: 100%; height: 150px; }
    </style>
</head>
<body>

<div class="row">
    <select id="method">
        <option>GET</option>
        <option>POST</option>
    </select>
    <input type="text" id="url" placeholder="Entrer URL">
    <button onclick="sendRequest()">Envoyer</button>
</div>

<div id="headers-container">
    <div class="row">
        <input type="text" placeholder="Header key" class="key">
        <input type="text" placeholder="Header value" class="value">
        <button onclick="addHeader()">+</button>
    </div>
</div>

<div class="box">
    <h3>Body</h3>
    <textarea id="body"></textarea>
</div>

<div class="box">
    <h3>Response body</h3>
    <pre id="response"></pre>
</div>

<script>
    function addHeader() {
        const container = document.getElementById("headers-container");
        const div = document.createElement("div");
        div.className = "row";
        div.innerHTML = `
            <input type="text" placeholder="Header key" class="key">
            <input type="text" placeholder="Header value" class="value">
        `;
        container.appendChild(div);
    }

    async function sendRequest() {
        const url = document.getElementById("url").value;
        const method = document.getElementById("method").value;
        const body = document.getElementById("body").value;
        const keys = document.querySelectorAll(".key");
        const values = document.querySelectorAll(".value");

        let headers = {};
        for (let i = 0; i < keys.length; i++) {
            if (keys[i].value !== "") {
                headers[keys[i].value] = values[i].value;
            }
        }

        try {
            const response = await fetch(url, {
                method: method,
                headers: headers,
                body: method === "POST" ? body : null
            });
            const text = await response.text();
            document.getElementById("response").textContent =
                "Status: " + response.status + "\n\n" + text;
        } catch (error) {
            document.getElementById("response").textContent = "Erreur: " + error;
        }
    }
</script>

</body>
</html>
```

---

### Exercice 2 : Questions théoriques

#### 1. Différence entre `no-cache` et `no-store`

| Directive | Comportement |
|-----------|-------------|
| `no-cache` | Le navigateur peut stocker la réponse, mais doit toujours vérifier avec le serveur avant de l'utiliser. |
| `no-store` | Le navigateur ne stocke rien (ni requête ni réponse). |

#### 2. Pourquoi POST n'est-il pas idempotent ?

Parce que chaque requête POST peut créer une nouvelle ressource.

#### 3. Que se passe-t-il si le serveur renvoie un code 301 ?

Le serveur indique une **redirection permanente** :
- Le navigateur est redirigé vers une nouvelle URL.
- La nouvelle URL doit être utilisée à la place de l'ancienne.

#### 4. À quoi sert le header `Origin` ?

Il indique l'origine de la requête (protocole, domaine, port). Utilisé pour la sécurité **(CORS)** afin de contrôler les accès entre sites.

#### 5. Pourquoi utiliser `HttpOnly` sur les cookies de session ?

Pour renforcer la sécurité :
- Empêche l'accès au cookie via JavaScript.
- Protège contre les attaques **XSS**.
- Le cookie est uniquement envoyé au serveur.
