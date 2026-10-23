[README.txt](https://github.com/user-attachments/files/33892837/README.txt)
# OpenStore — fichiers préparés

## Fichiers
- `index.html` : boutique, connexion et affichage des APK GitHub. Le bouton de suppression n'apparaît qu'au compte dont le rôle lu dans `store_users/<email normalisé>` est `admin`/`administrateur`.
- `admin.html` : tableau de bord avec Utilisateurs, Modération, Notifications et Logs. Les signalements sont lus depuis `reports`, les messages depuis `messages`, les notifications depuis `notifications` et le journal depuis `moderation_logs`.
- `firebase.database.rules.json` : exemple de règles strictes basé sur la revendication personnalisée `auth.token.admin`.

## IMPORTANT — avant de déployer
1. Copie `index.html` et `admin.html` à la racine du dépôt GitHub Pages.
2. Le panneau utilise des chemins de données indicatifs (`reports`, `messages`, `notifications`, `moderation_logs`). Il faudra faire correspondre la création de signalements/messages à ces formats.
3. Le contrôle de rôle dans le navigateur ne sécurise pas la base. Les règles fournies exigent un custom claim Firebase `admin: true`, à attribuer aux administrateurs depuis un environnement de confiance avec Firebase Admin SDK (jamais depuis le HTML). Tant que ce claim n'est pas configuré, les écritures protégées seront refusées, ce qui est préférable à une sécurité fictive.
4. Les rôles visibles dans `store_users` ne suffisent pas à eux seuls à prouver un droit d'administration. Ne rends jamais ce nœud modifiable par les utilisateurs ordinaires.
5. La suppression d'un enregistrement dans `store_users` ne supprime pas le compte Firebase Authentication. La suppression réelle de comptes Auth et la création sécurisée d'utilisateurs nécessitent un backend de confiance / Admin SDK.
6. Sauvegarde tes règles actuelles avant de remplacer quoi que ce soit. Les règles de ce dossier sont volontairement strictes et peuvent nécessiter des adaptations selon les règles actuelles et les chemins de données existants.

## Structure de données attendue
- `store_users/<clé-email>` : `{ "email": "...", "role": "admin|ami|invite|testeur|codeur|programmeur|tuteur|famille", "status": "active|suspended" }`
- `reports/<id>` : `{ "reason": "...", "reporter": "...", "reportedUser": "...", "messageId": "...", "messageText": "...", "context": "...", "status": "open" }`
- `messages/<id>` : message à modérer
- `notifications/<id>` : `{ "title": "...", "message": "...", "read": false, "createdAt": 0 }`
- `moderation_logs/<id>` : actions du tableau de bord
