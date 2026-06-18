# 📱 Publier depuis le téléphone (connexion par jeton)

Sur **mobile (iPhone et Android)**, le bouton « Login with GitHub » laisse un
**écran blanc** après la connexion : c'est une limite connue de Sveltia CMS (le
popup de connexion ne réussit pas à renvoyer l'info à la page sur téléphone).

La solution fiable sur mobile est **« Sign in with Token »** : Maman colle **une
seule fois** un jeton GitHub sur son téléphone, et ça reste mémorisé ensuite.

> 🔒 **Sécurité — important**
> Le jeton donne le droit de modifier le site. Il ne doit **jamais** être écrit
> dans le code du site (qui est public). On le colle **uniquement** dans l'écran
> de connexion `/admin/`, directement sur le téléphone de Maman.

---

## Partie A — Shawn crée le jeton (une fois)

1. Va sur **https://github.com/settings/tokens?type=beta**
   (GitHub → Settings → Developer settings → **Fine-grained tokens**).
2. Clique **Generate new token**.
3. Remplis :
   - **Token name** : `Telephone Maman - Kreation-KF`
   - **Expiration** : `Custom` → mets une date dans ~**1 an**
     (note-la : le jeton devra être refait à cette date).
   - **Resource owner** : `MrKillerVortex`
4. **Repository access** → coche **Only select repositories** → choisis
   **`kreation-kf`**.
5. **Permissions** → **Repository permissions** → trouve **Contents** →
   passe-le à **Read and write**.
   (« Metadata : Read-only » se coche tout seul, c'est normal — laisse-le.)
6. Clique **Generate token** en bas, puis **copie** le jeton
   (il commence par `github_pat_…`). ⚠️ Il ne s'affiche **qu'une seule fois**.

> Si « Sign in with Token » refuse le jeton fine-grained, refais un jeton
> **classique** ici : https://github.com/settings/tokens/new — coche la case
> **`repo`**, et utilise ce jeton à la place.

---

## Partie B — Donner le jeton à Maman, sur son téléphone

Le plus simple : envoie-lui le jeton par un message privé, ou ouvre toi-même
sa session une fois sur son téléphone.

1. Sur le téléphone, ouvre le site et descends tout en bas → bouton
   **« ✏️ Espace Maman »** (ou va sur `kreation-kf.vercel.app/admin/`).
2. Sur l'écran de connexion, choisis **« Sign in with Token »**
   (PAS « Login with GitHub »).
3. **Colle le jeton** `github_pat_…`, puis valide.
4. C'est connecté ✅ — et ça reste mémorisé sur ce téléphone.

Ensuite, pour publier : **🎨 Galerie de Maman → Mes œuvres → Add** →
photo + titre + description + catégorie → **Publish**.

---

## En cas de problème

- **Toujours blanc / ça tourne sans fin** : vérifie que tu as bien cliqué
  « Sign in with Token » et non « Login with GitHub ».
- **« Bad credentials » / jeton refusé** : le jeton est expiré ou mal copié
  (un espace en trop). Refais la Partie A, ou essaie le jeton **classique**.
- **Depuis un ordinateur**, « Login with GitHub » fonctionne normalement : c'est
  une bonne solution de secours.
