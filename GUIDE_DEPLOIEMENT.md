# Guide de déploiement — Comparateur Châssis Véhicules

## Ce que vous avez

Un **fichier unique `index.html`** qui contient toute l'application (HTML + CSS + JavaScript). C'est une application 100% côté client — aucun serveur, aucune base de données, aucun PHP nécessaire. Les fichiers Excel sont traités directement dans le navigateur de l'utilisateur.

---

## Option 1 — Hébergement mutualisé classique (OVH, LWS, o2switch, Hostinger...)

### Étapes

1. **Connectez-vous à votre panneau d'hébergement** (cPanel, Plesk, ou autre)

2. **Accédez au Gestionnaire de fichiers** (File Manager)

3. **Naviguez vers le dossier `public_html`** (ou `www` selon l'hébergeur)

4. **Créez un sous-dossier** (optionnel mais recommandé) :
   ```
   public_html/comparateur/
   ```

5. **Uploadez le fichier `index.html`** dans ce dossier

6. **C'est prêt !** L'application est accessible à :
   ```
   https://votre-domaine.com/comparateur/
   ```

### Sécuriser avec un vrai mot de passe (htaccess)

L'application a un code d'accès intégré (`chassis2024` par défaut), mais pour plus de sécurité, ajoutez une protection serveur :

**Créez un fichier `.htaccess`** dans le dossier `comparateur/` :
```apache
AuthType Basic
AuthName "Accès restreint"
AuthUserFile /home/votre-compte/.htpasswd
Require valid-user
```

**Créez le fichier `.htpasswd`** (via cPanel > "Protection de répertoire" ou en ligne de commande) :
```bash
htpasswd -c /home/votre-compte/.htpasswd utilisateur1
```

### Changer le code d'accès de l'application

Ouvrez `index.html` dans un éditeur de texte et cherchez cette ligne (vers la fin) :
```javascript
const ACCESS_CODE = "chassis2024";
```
Remplacez `chassis2024` par le code de votre choix.

---

## Option 2 — Hébergement gratuit (GitHub Pages)

Idéal pour tester ou si vous n'avez pas d'hébergement.

### Étapes

1. **Créez un compte GitHub** sur [github.com](https://github.com) (gratuit)

2. **Créez un nouveau dépôt** (repository) :
   - Cliquez "New repository"
   - Nom : `comparateur-chassis`
   - Cochez "Public"
   - Cliquez "Create repository"

3. **Uploadez `index.html`** :
   - Cliquez "uploading an existing file"
   - Glissez `index.html`
   - Cliquez "Commit changes"

4. **Activez GitHub Pages** :
   - Allez dans Settings > Pages
   - Source : "Deploy from a branch"
   - Branch : `main` / `/ (root)`
   - Cliquez Save

5. **Attendez 2 minutes**, votre application sera en ligne à :
   ```
   https://votre-pseudo.github.io/comparateur-chassis/
   ```

---

## Option 3 — Netlify (gratuit, très simple)

1. Allez sur [netlify.com](https://www.netlify.com) et créez un compte gratuit

2. Sur le tableau de bord, **glissez-déposez le dossier** contenant `index.html`

3. En 30 secondes, vous obtenez une URL comme :
   ```
   https://joyful-swan-abc123.netlify.app
   ```

4. Vous pouvez personnaliser le nom dans Site settings > Domain management

---

## Option 4 — Avec un nom de domaine personnalisé

Si vous avez déjà un domaine (ex: `monentreprise.ci`) :

1. Hébergez le fichier selon l'option 1, 2 ou 3

2. Dans votre panneau DNS, créez un enregistrement :
   - **Type** : CNAME ou A
   - **Nom** : `chassis` (pour `chassis.monentreprise.ci`)
   - **Valeur** : l'adresse de votre hébergement

---

## Structure des fichiers

Votre dossier ne contient qu'un seul fichier :

```
comparateur/
└── index.html      ← Toute l'application est ici
```

C'est tout. Pas de PHP, pas de Node.js, pas de base de données.

---

## Fonctionnement technique

| Aspect | Détail |
|--------|--------|
| **Traitement** | 100% dans le navigateur (JavaScript) |
| **Données** | Jamais envoyées à un serveur — tout reste en local |
| **Dépendance externe** | SheetJS (xlsx.js) chargé depuis CDN cloudflare |
| **Compatibilité** | Chrome, Firefox, Edge, Safari (récents) |
| **Mobile** | Interface responsive, utilisable sur téléphone |
| **Taille fichiers** | Peut traiter des fichiers de plusieurs milliers de lignes |

---

## Personnalisation

### Changer le code d'accès
Ligne à modifier dans `index.html` :
```javascript
const ACCESS_CODE = "chassis2024";
```

### Supprimer l'écran de connexion
Supprimez tout le bloc `<div id="loginOverlay">...</div>` et changez :
```html
<div id="app" class="hidden">
```
en :
```html
<div id="app">
```

### Changer les couleurs
Les couleurs sont définies dans les variables CSS `:root` en haut du fichier.

### Ajouter des colonnes au tableau de résultats
Modifiez cette ligne dans le JavaScript :
```javascript
const DISPLAY_COLS_PRIORITY = ["MARQUE2","VALFOB","OPERATEUR","MARQUE1","NUMENR","ORIGINE","PROVENANCE"];
```

---

## FAQ

**Q : Les données de mes fichiers Excel sont-elles envoyées quelque part ?**
Non. Tout le traitement se fait dans le navigateur de l'utilisateur. Aucune donnée ne quitte son ordinateur.

**Q : Combien de lignes peut-on traiter ?**
Jusqu'à environ 50 000 lignes sans problème. Au-delà, le traitement peut prendre quelques secondes.

**Q : Faut-il installer quelque chose sur le serveur ?**
Non. Un simple hébergement web statique suffit (pas besoin de PHP, Python, Node.js, ni de base de données).

**Q : Plusieurs personnes peuvent utiliser l'application en même temps ?**
Oui, sans limite. Chaque utilisateur traite ses fichiers localement.
