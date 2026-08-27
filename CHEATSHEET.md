# CHEATSHEET

## YouTube API

### requests — appel HTTP GET

```python
import requests

response = requests.get(url)       # envoie la requête
response.raise_for_status()        # lève une exception si HTTP 4xx/5xx
data = response.json()             # corps JSON -> dict Python
```

- `raise_for_status()` : sans lui, une réponse en erreur (clé invalide, quota) passe silencieusement et le `.json()` plante plus loin de façon obscure.
- Erreurs réseau à attraper : `requests.exceptions.RequestException` (classe mère de toutes).

### json — inspecter une réponse

```python
import json
print(json.dumps(data, indent=4))   # affiche le dict indenté, lisible
```

- `json.dumps(obj, indent=4)` : objet Python -> chaîne JSON formatée. Sert juste à lire à la main.
- Ne pas confondre : `json.dumps` (dump String) écrit une string ; `json.dump` écrit dans un fichier.

### Naviguer dans la réponse channels

```python
url = f"https://youtube.googleapis.com/youtube/v3/channels?part=contentDetails&forHandle={HANDLE}&key={API_KEY}"
playlist_id = data["items"][0]["contentDetails"]["relatedPlaylists"]["uploads"]
```

- `part=` : on ne reçoit que les blocs demandés (impacte le quota).
- `forHandle=` : le nom après le `@` dans l'URL de la chaîne.
- `items` est toujours une **liste**, même pour une seule chaîne -> `[0]`.
- `relatedPlaylists["uploads"]` : playlist contenant toutes les vidéos publiées de la chaîne.
