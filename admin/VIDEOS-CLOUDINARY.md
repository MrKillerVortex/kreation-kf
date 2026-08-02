# 🎬 Grosses vidéos — configuration Cloudinary (à faire une seule fois)

## Le problème

Quand Maman téléverse une vidéo depuis `/admin/`, le fichier part **dans le
dépôt GitHub**, via l'API de GitHub. Cette API bloque autour de **20 Mo** —
d'où l'échec sur toute vidéo un peu longue. Et même en dessous, chaque vidéo
resterait **pour toujours** dans l'historique du dépôt, qui gonflerait vite.

## La solution

Les **vidéos** partent chez **Cloudinary** (hébergeur de médias, offre gratuite),
les **photos** continuent d'aller dans le dépôt GitHub comme avant.

Ce que ça change :

- **~100 Mo par vidéo** au lieu de ~20 Mo
- Cloudinary **compresse automatiquement** la vidéo selon le téléphone ou
  l'ordinateur qui la regarde → chargement bien plus rapide
- **Image de couverture automatique** (prise à la 1ʳᵉ seconde) si Maman n'en
  met pas
- Le dépôt GitHub reste léger
- Le lecteur vidéo du site reste **le même** (aucun logo YouTube)

Offre gratuite : **25 crédits/mois** (1 crédit ≈ 1 Go de stockage ou 1 Go de
bande passante), **3 utilisateurs**, 1 compte. Largement suffisant ici.

---

## Étape 1 — Créer le compte Cloudinary

1. Va sur **https://cloudinary.com/users/register_free** et crée un compte
   gratuit (aucune carte bancaire demandée).
2. Une fois dans le tableau de bord (**Dashboard**), note les deux valeurs
   affichées en haut :
   - **Cloud name** (ex. `dxxxxxxxx`)
   - **API Key** (une suite de chiffres, ex. `123456789012345`)

> 🔒 **Ne copie JAMAIS l'`API Secret`.** Le *Cloud name* et l'*API Key* sont
> publics par nature (ils finissent dans le code du site, qui est public).
> L'`API Secret`, lui, donnerait le contrôle total du compte.

---

## Étape 2 — Brancher les valeurs dans le site

1. Ouvre `admin/config.yml`.
2. Dans le bloc `media_libraries:`, remplace les deux valeurs :

   ```yaml
   cloudinary:
     config:
       cloud_name: "REMPLACER_PAR_CLOUD_NAME"   # ← ton Cloud name
       api_key: "REMPLACER_PAR_API_KEY"         # ← ton API Key
   ```

3. Sauvegarde, puis `git commit` et `git push`. Vercel republie tout seul.

---

## Étape 3 — Donner l'accès Cloudinary à Maman

Pour téléverser, Maman doit être **connectée au compte Cloudinary** qui
correspond à l'`API Key` ci-dessus. Deux façons :

- **Le plus simple** : lui donner les identifiants du compte Cloudinary
  (elle se connecte une fois, son téléphone s'en souvient ensuite).
- **Plus propre** : Cloudinary → **Settings → Users → Invite user** (l'offre
  gratuite en autorise 3) → elle reçoit une invitation et se crée son propre
  mot de passe.

---

## Comment Maman ajoute une vidéo, ensuite

1. `/admin/` → **🎬 Mes vidéos (galerie)** ou **🎥 Coulisses / Atelier**
2. **Add Vidéos** → clique sur le champ **Vidéo**
3. Dans la fenêtre qui s'ouvre, choisir l'onglet **Cloudinary**
   (⚠️ pas l'onglet du dépôt — c'est lui qui est limité à ~20 Mo)
4. La première fois : se connecter à Cloudinary
5. **Upload** → choisir la vidéo dans le téléphone → **Insert**
6. Titre, description, puis **Publish**

---

## En cas de problème

- **L'onglet Cloudinary reste sur l'écran de connexion** : Maman n'est pas
  connectée au bon compte Cloudinary. Vérifie que le compte correspond bien à
  l'`API Key` mise dans `config.yml`.
- **Le téléversement échoue sur une très longue vidéo** : au-delà de **100 Mo**,
  Cloudinary refuse le fichier sur l'offre gratuite. Utilise alors le champ
  **« … ou lien YouTube / Vimeo »** (voir plus bas).
- **Les vidéos déjà en ligne** (dans `videos/`) continuent de fonctionner : rien
  à refaire, le site gère les deux cas.

---

## Solution de secours : un lien YouTube / Vimeo

Chaque vidéo a un second champ, **« … ou lien YouTube / Vimeo »**. Aucune limite
de taille ni de durée, et aucune configuration à faire — mais le lecteur affiché
est celui de YouTube (avec sa marque), pas celui du site.

Pour Maman :

1. Depuis l'application YouTube du téléphone, publier la vidéo en
   **« Non répertoriée »** (visible seulement par ceux qui ont le lien, elle
   n'apparaît pas dans les recherches).
2. **Partager → Copier le lien**.
3. Dans `/admin/`, coller le lien dans le champ **« … ou lien YouTube / Vimeo »**
   et **laisser le champ « Vidéo » vide**.
4. Titre, description, **Publish**.

Si les deux champs sont remplis, c'est le **lien** qui est affiché.

Toutes les formes d'adresses habituelles sont acceptées : `youtu.be/…`,
`youtube.com/watch?v=…`, `youtube.com/shorts/…`, `vimeo.com/…`, y compris les
liens privés Vimeo avec code (`vimeo.com/123456789/abcdef`). Un lien qui n'est
ni YouTube ni Vimeo est simplement ignoré (la vidéo n'apparaît pas) — c'est
volontaire, pour éviter d'intégrer n'importe quel site dans la page.

Côté vie privée, l'intégration passe par **youtube-nocookie.com** : YouTube ne
dépose pas ses cookies publicitaires tant que le visiteur n'a pas lancé la
lecture.
