# Chapitre 6 : Le Protocole HTTP - Rapport de Travaux Pratiques

## TP 1 : Exploration avec les DevTools

### 1.1 Analyse d'une requête GET (`/get`)
• **Code de statut** : `200 OK`
• **Headers de requête (Request Headers)** :
    • `:authority` → `httpbin.org`
    • `:method` → `GET`
    • `accept` → `text/html, application/json, */*`
    • `user-agent` → `Mozilla/5.0... Chrome/147`

### 1.2 Codes de statut observés
• `status/200` → **200 OK** (Succès)
• `status/404` → **404 Not Found** (Ressource inexistante)
• `status/500` → **500 Internal Server Error** (Problème serveur)
• `redirect/3` → **302 Found** (Redirection temporaire)

| URL | Méthode | Code | Content-Type |
| :--- | :--- | :--- | :--- |
| `httpbin.org/get` | GET | 200 | `application/json` |
| `httpbin.org/post` | POST | 200 | `application/json` |
| `httpbin.org/status/201` | GET | 201 | `text/html; charset=utf-8` |

---

## TP 2 : Maîtrise de cURL

### Différence entre `-i` et `-v`
• **`-i` (include)** : Affiche les en-têtes (headers) de réponse suivis du corps (body).
• **`-v` (verbose)** : Affiche tout le tunnel de communication (Handshake TLS, headers envoyés et reçus).

### Commande personnalisée
```bash
curl -i -X POST [https://httpbin.org/post](https://httpbin.org/post) \
  -H "Content-Type: application/json" \
  -H "X-Custom-Header: MonHeader" \
  -d '{"action": "test", "value": 42}'
