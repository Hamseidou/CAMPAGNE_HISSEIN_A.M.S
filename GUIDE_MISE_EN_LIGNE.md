# Mettre le site en ligne avec le vote en direct

Ce qu'il te faut : un compte Google (pour Firebase) et un compte GitHub. Compte environ 20 minutes. Tout est gratuit.

Les fichiers à publier :
- `index.html` (le site, photo comprise)
- `Programme_action_Hissein_Ahmat_ENASTIC.pdf`
- `CV_Hissein_Ahmat_Mahamat_Seidou.pdf`

Garde ces noms exactement : les boutons de téléchargement les utilisent.

---

## Partie 1. Créer la base du vote (Firebase)

GitHub peut afficher le site, mais il ne peut pas enregistrer les votes. Firebase s'en charge : c'est lui qui garde le compteur et le met à jour pour tout le monde.

1. Va sur **console.firebase.google.com** et connecte-toi avec ton compte Google.
2. **Créer un projet**. Nom au choix, par exemple `campagne-hissein`. Désactive Google Analytics, puis valide.
3. Dans le menu de gauche, ouvre **Build > Realtime Database**, puis **Créer une base de données**. Choisis la région Europe (Belgique) et le mode **verrouillé**.
4. Ouvre l'onglet **Règles**, efface tout et colle ceci, puis **Publier** :

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "votes": {
      ".read": true,
      "$uid": {
        ".write": "auth != null && auth.uid === $uid",
        ".validate": "newData.val() === true"
      }
    }
  }
}
```

Ces règles disent : tout le monde peut lire le compteur, et chaque visiteur peut seulement ajouter ou retirer son propre vote.

5. Ouvre **Build > Authentication > Commencer > Sign-in method**, choisis **Anonyme**, active-le et enregistre. Cela donne à chaque visiteur un identifiant discret, sans inscription, pour qu'il ne vote qu'une fois par appareil.
6. Récupère tes deux valeurs :
   - **databaseURL** : sur la page Realtime Database, l'adresse affichée en haut, de la forme `https://campagne-hissein-default-rtdb.europe-west1.firebasedatabase.app`.
   - **apiKey** : roue dentée **Paramètres du projet > Général**, puis **Vos applications > Ajouter une application Web (</>)**. Donne un nom, valide, et copie la valeur `apiKey`.

## Partie 2. Brancher le site

1. Ouvre `index.html` dans un éditeur de texte.
2. Cherche `RÉGLAGES À REMPLIR`. Tu trouveras :

```js
var FIREBASE = {
  apiKey: "",
  databaseURL: ""
};
```

3. Colle tes deux valeurs entre les guillemets, puis enregistre.

Tant que ces deux champs sont vides, le vote fonctionne en mode démonstration et compte seulement sur ton appareil. Un message le précise sur la page.

## Partie 3. Publier sur GitHub Pages

1. Sur **github.com**, crée un dépôt **public**, par exemple `campagne-hissein`.
2. **Add file > Upload files**, envoie les 3 fichiers, puis **Commit changes**.
3. Va dans **Settings > Pages**. Sous **Build and deployment**, choisis **Deploy from a branch**, la branche **main**, le dossier **/ (root)**, puis **Save**.
4. Après 1 à 2 minutes, ton site est à l'adresse `https://TON-PSEUDO.github.io/campagne-hissein/`. Envoie-la par WhatsApp.

## Partie 4. Vérifier

Ouvre le lien sur deux téléphones. Vote sur l'un : le chiffre doit monter sur l'autre en quelques secondes, sans recharger.

---

## Bon à savoir

- **Un soutien par appareil.** Quelqu'un qui vide les données de son navigateur peut revoter. C'est un sondage de soutien entre amis, pas un vote officiel, et la page le dit.
- **Remettre le compteur à zéro** (par exemple après tes tests) : dans Firebase, **Realtime Database > Données**, supprime le nœud `votes`.
- **Protéger ta clé** (conseillé) : dans **console.cloud.google.com > API et services > Identifiants**, ouvre la clé de navigateur du projet, choisis **Restrictions relatives aux sites web** et ajoute `https://TON-PSEUDO.github.io/*`. Ainsi, la clé ne marche que depuis ton site.
- **Ton CV sera public.** Le dépôt est public, donc quiconque a le lien peut télécharger le CV. Le tien contient ton e-mail, ton téléphone et les numéros de tes références. Si tu préfères, retire la partie « Références » du PDF avant de l'envoyer, ou ne publie pas le CV.
- **Modifier le site plus tard** : dans GitHub, ouvre `index.html`, clique sur le crayon, modifie, puis **Commit changes**. La mise à jour est en ligne en 1 à 2 minutes.
