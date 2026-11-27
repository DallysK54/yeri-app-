# YERI — installation et sécurité

YERI est une application web installable (PWA). Les opérations restent sur l’appareil et sont chiffrées dans le navigateur avec AES-GCM 256 bits ; la clé est dérivée de la phrase secrète par PBKDF2/SHA-256. Le code n’appelle aucun service bancaire ou IA externe. Le service worker ne met en cache que les fichiers de l’application, jamais les données du budget.

## Déployer l’application

L’installation sur téléphone ne fonctionne pas directement depuis `file://`. Il faut publier le contenu de ce dossier sur un hébergement HTTPS. Le fichier `_headers` ajoute des en-têtes de sécurité sur Netlify et Cloudflare Pages ; GitHub Pages ignore ce fichier mais sert les sites en HTTPS et YERI garde une politique CSP dans sa page.

### Avec GitHub Pages

1. Crée un dépôt GitHub personnel, par exemple `yeri-app`. Le dépôt peut être public ; il ne doit contenir que les fichiers de l’application, jamais une sauvegarde de tes données.
2. Téléverse le contenu de ce dossier `outputs` à la racine du dépôt : `index.html`, `app.js`, `styles.css`, `manifest.webmanifest`, `service-worker.js`, `_headers` et le dossier `icons`.
3. Dans le dépôt, ouvre **Settings → Pages**. Choisis le déploiement depuis la branche principale et le dossier racine, puis enregistre.
4. Quand GitHub affiche l’adresse de YERI, ouvre **Settings → Pages** et vérifie que **Enforce HTTPS** est activé. Ouvre ensuite cette adresse commençant par `https://` sur ton téléphone.

GitHub Pages publie le code du site sur Internet. Les données du budget ne sont pas dans le dépôt : elles restent dans le stockage local du navigateur sous forme chiffrée. Ne téléverse jamais `YERI-sauvegarde-chiffree.json` dans le dépôt.

## Installer sur Android

1. Ouvre l’adresse HTTPS de YERI dans Chrome.
2. Appuie sur **⋮ → Installer l’application** (ou **Ajouter à l’écran d’accueil** selon la version).
3. Ouvre YERI depuis son icône et crée une phrase secrète d’au moins 10 caractères. Une phrase de plusieurs mots est plus difficile à deviner.

## Installer sur iPhone

1. Ouvre l’adresse HTTPS dans Safari.
2. Appuie sur **Partager → Ajouter à l’écran d’accueil**.
3. Active **Ouvrir comme app web** si cette option apparaît, puis appuie sur **Ajouter**.
4. Ouvre YERI depuis l’icône et crée ta phrase secrète.

## Sauvegardes et limites

Dans **⋮**, utilise **Sauvegarder mes données** pour télécharger une sauvegarde chiffrée et **Restaurer une sauvegarde** pour la réimporter. Garde la sauvegarde dans un endroit privé et mémorise la même phrase secrète : sans elle, les données ne peuvent pas être déchiffrées. Le déverrouillage automatique se déclenche après cinq minutes d’inactivité, ou une minute quand l’application reste en arrière-plan.

Le chiffrement protège les données enregistrées au repos dans le navigateur. Il ne protège pas un téléphone compromis ou déjà déverrouillé, ni les données visibles à l’écran pendant l’utilisation. Une suppression du stockage du navigateur efface le coffre ; conserve donc une sauvegarde chiffrée. Cette version ne synchronise pas les données entre appareils et ne se connecte pas à Wave.

## À propos de SWE-agent

Le dépôt [SWE-agent](https://github.com/SWE-agent/SWE-agent) décrit un agent de développement qui peut résoudre des problèmes de code et aider à chercher des vulnérabilités. Ce n’est pas une bibliothèque de sécurité à intégrer dans YERI. YERI est livré sans agent ni dépendance externe côté navigateur.

## Mettre à jour une version déjà publiée

1. Télécharge la nouvelle archive `YERI-installation.zip` et décompresse-la.
2. Dans le dépôt GitHub déjà utilisé, ouvre **Code → Add file → Upload files**.
3. Téléverse le contenu décompressé à la racine, en remplaçant les anciens fichiers portant les mêmes noms, puis valide **Commit changes** sur `main`. Ne téléverse pas les sauvegardes chiffrées de tes finances.
4. GitHub Pages republie après le commit ; tu n’as pas besoin de refaire le réglage **Settings → Pages**.
5. Ferme puis rouvre YERI sur ton téléphone, ou actualise la page. Le service worker mis à jour remplacera le cache précédent.

La mise à jour garde le coffre chiffré déjà enregistré. Pour effacer les anciens exemples et établir ton propre budget, appuie sur la flèche ↻ en haut, confirme la remise à zéro, puis saisis ton revenu et ajoute tes enveloppes (montant libre, même à zéro). Cette remise à zéro efface les opérations et objectifs présents sur cet appareil ; exporte une sauvegarde avant si tu veux les conserver.
