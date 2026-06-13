# Configuration unique de l'éditeur de Maman (à faire une seule fois)

Le site est statique (hébergé sur Vercel) et n'a pas de base de données.
Pour que Maman puisse **publier en direct** ses photos sans toucher au code,
on utilise **Sveltia CMS** : une page `/admin/` où elle se connecte, téléverse
une photo, écrit un titre + une description et clique sur **Publish**.
Ça enregistre tout dans le dépôt GitHub → Vercel republie le site tout seul.

Pour des raisons de sécurité, la connexion passe par un petit « serveur de
connexion » gratuit. Voici les **3 étapes** à faire **une seule fois**.

---

## Étape 1 — Créer une « GitHub OAuth App »

1. Va sur https://github.com/settings/developers → **OAuth Apps** → **New OAuth App**.
2. Remplis :
   - **Application name** : `Kreation-KF Admin` (ce que tu veux)
   - **Homepage URL** : `https://kreation-kf.vercel.app`
   - **Authorization callback URL** : `https://VOTRE-WORKER.workers.dev/callback`
     (tu mettras la vraie adresse après l'étape 2 — reviens la corriger)
3. Clique **Register application**.
4. Note le **Client ID**. Clique **Generate a new client secret** et note le **Client secret**
   (il ne s'affiche qu'une fois).

---

## Étape 2 — Déployer le serveur de connexion (Cloudflare Workers, gratuit)

Le plus simple, sans rien installer :

1. Crée un compte gratuit sur https://dash.cloudflare.com (si pas déjà fait).
2. Va sur le projet officiel : https://github.com/sveltia/sveltia-cms-auth
   et suis son bouton **« Deploy to Cloudflare Workers »** (dans le README).
   - Sinon : Cloudflare → **Workers & Pages** → **Create** → colle le code du fichier
     `src/index.js` de ce dépôt.
3. Une fois déployé, tu obtiens une adresse du type :
   `https://sveltia-cms-auth.TON-COMPTE.workers.dev`
4. Dans les **Settings → Variables** du Worker, ajoute ces variables
   (type « Secret » pour les deux premières) :
   - `GITHUB_CLIENT_ID`     = le Client ID de l'étape 1
   - `GITHUB_CLIENT_SECRET` = le Client secret de l'étape 1
   - `ALLOWED_DOMAINS`      = `kreation-kf.vercel.app`
5. Retourne à l'étape 1 (point 2) et mets la vraie **Authorization callback URL** :
   `https://sveltia-cms-auth.TON-COMPTE.workers.dev/callback`

---

## Étape 3 — Brancher l'adresse dans le site

1. Ouvre `admin/config.yml`.
2. Remplace la ligne :
   ```yaml
   base_url: https://REMPLACER-PAR-VOTRE-AUTH.workers.dev
   ```
   par ton adresse réelle, par ex :
   ```yaml
   base_url: https://sveltia-cms-auth.TON-COMPTE.workers.dev
   ```
3. Sauvegarde, `git commit` et `git push`. Vercel republie.

---

## C'est prêt ✅

Donne à Maman ce lien : **https://kreation-kf.vercel.app/admin/**

Elle clique **Login with GitHub** une fois (avec un compte que tu autorises
comme collaborateur du dépôt `MrKillerVortex/mom-website`), puis :

1. Ouvre **🎨 Galerie de Maman → Mes œuvres**
2. Clique **Add Mes œuvres**
3. Choisit une **Photo**, écrit un **Titre** et une **Description**, choisit une **Catégorie**
4. Clique **Publish**

Quelques secondes plus tard, sa création apparaît **en haut de la galerie**
du vrai site, visible par tout le monde.

> 💡 Astuce : pour qu'elle puisse publier, ajoute son compte GitHub comme
> **collaborateur** du dépôt (GitHub → repo → Settings → Collaborators).
> Si tu préfères qu'elle utilise TON compte, elle se connectera simplement
> avec tes identifiants GitHub.
