# Chapitre 6 : Le Protocole HTTP - Rapport de Travaux Pratiques

## TP 1 : Exploration avec les DevTools

### 1.1 Code de statut
- **200 OK** (Requête réussie)

### 1.2 Headers de requête (Request Headers)

- :authority → httpbin.org  
- :method → GET  
- :path → /get  
- :scheme → https  

- accept → text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7  
- accept-encoding → gzip, deflate, br, zstd  
- accept-language → fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7  

- user-agent → Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.0.0 Safari/537.36  

- sec-ch-ua → "Google Chrome";v="147", "Not.A/Brand";v="8", "Chromium";v="147"  
- sec-ch-ua-mobile → ?0  
- sec-ch-ua-platform → "macOS"  

- sec-fetch-site → none  
- sec-fetch-mode → navigate  
- sec-fetch-dest → document  
- sec-fetch-user → ?1  

- upgrade-insecure-requests → 1  
- priority → u=0, i  

### 1.3 Content-Type de la réponse
- application/json  

### 1.4 Tester différentes méthodes
- **GET** : Permet de récupérer les données envoyées par le serveur.  
- **POST** : Envoie des données au serveur et récupère une réponse structurée (souvent en JSON).  

### 1.5 Codes de statut observés
- https://httpbin.org/status/200 → **200 OK** (Succès)  
- https://httpbin.org/status/404 → **404 Not Found** (Ressource introuvable)  
- https://httpbin.org/status/500 → **500 Internal Server Error** (Erreur serveur)  
- https://httpbin.org/redirect/3 → **302 Found** (Redirections multiples)  

## Exercice : Tableau des requêtes HTTP

| URL | Méthode | Code | Content-Type |
| :--- | :--- | :--- | :--- |
| httpbin.org/get | GET | 200 | application/json |
| httpbin.org/post | POST | 200 | application/json |
| httpbin.org/status/201 | GET | 201 | text/html; charset=utf-8 |

---

# TP 2 : Maîtrise de cURL

- **-i (include)** : Affiche les headers de la réponse suivis du corps (body).  
- **-v (verbose)** : Affiche tout le dialogue réseau (Handshake, headers envoyés et reçus), idéal pour le debug.

### Exercice avancé : Commande cURL
```bash
curl -i -X POST [https://httpbin.org/post](https://httpbin.org/post) \
  -H "Content-Type: application/json" \
  -H "X-Custom-Header: MonHeader" \
  -d '{"action": "test", "value": 42}'
  ### TP 3 : API REST avec JavaScript

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
      console.log(`Échec tentative ${i+1}, nouvel essai dans 1s...`);
      await new Promise(resolve => setTimeout(resolve, 1000));
    }
  }
}


