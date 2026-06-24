Sauvegarde les changements du workspace via Git.

Étapes à suivre dans l'ordre :

1. Si le dossier n'est pas encore un dépôt Git (pas de `.git/`), exécuter `git init` d'abord.

2. Vérifier l'état avec `git status` pour voir ce qui a changé.

3. Ajouter tous les fichiers modifiés avec `git add -A`, SAUF ceux exclus par `.gitignore` (notamment `.env`).

4. Créer un commit avec un message qui résume ce qui a été fait. Format du message : une ligne courte en français, au présent. Exemple : "Ajoute la structure livrables et la gestion des secrets".

5. Confirmer à l'utilisateur que le commit a été créé avec succès, en indiquant le nombre de fichiers sauvegardés et le message du commit utilisé.

Règles importantes :
- Ne jamais commiter `.env` ou tout fichier contenant des secrets.
- Ne jamais faire de `git push` sauf si l'utilisateur le demande explicitement.
- Si l'argument `$ARGUMENTS` est fourni, l'utiliser comme message de commit à la place du message généré automatiquement.
