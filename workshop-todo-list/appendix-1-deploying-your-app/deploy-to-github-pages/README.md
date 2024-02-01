# Déployer sur GitHub Pages

Pour déployer nos changements sur GitHub pages, nous allons utiliser le package `angular-cli-ghpages` [https://github.com/angular-schule/angular-cli-ghpages](https://github.com/angular-schule/angular-cli-ghpages)


## Installer `angular-cli-ghpages`

Nous allons utiliser une commande Angular CLI pour ajouter cette librairie à notre application et configurer automatiquement l'action de déploiement. Exécutez la commande suivante :

```
ng add angular-cli-ghpages
```

## Committer les changements

Committer vos changements en exécutant cette commande dans votre répertoire de projet.

```
git add -A && git commit -m "Your Message"
```

Puis exécutez la commande suivante pour pousser les changements

```
git push
```

## Déployer sur GitHub Pages


Utilier la commande suivante pour déployer votre application sur GitHub Pages:

```
ng deploy --base-href="/[nom-du-projet-github]/"
```

`/[your-repo-name]/` est un espace réservé pour le nom de votre dépôt github. Donc si votre projet a le nom `https://github.com/myname/ng-girls` la valeur doit être : `--base-href="/ng-girls/"`.

Votre application sera disponible à l'adresse https://\[votre-nom-d'utilisateur-GH].github.io/\[nom-du-repo]/

Pour plus d'informations, voir [https://github.com/angular-schule/angular-cli-ghpages](https://github.com/angular-schule/angular-cli-ghpages).

## Proboèmes connus

### Ecran blanc (et erreur 404 dans DevTools dans le navigateur)

Si le déploiement a réussi mais que vous voyez une page blanche dans le navigateur, vous avez probablement utilisé des lettres majuscules dans le nom de votre dépôt. Essayez d'ajouter un nouveau dépôt avec uniquement des lettres minuscules depuis le site GitHub. Plus tard, supprimez la connexion à l'ancien à partir de vos fichiers locaux en tapant :

```
git remote rm
```

Dans le terminal. Ajoutez une connexion au nouveau dépôt :

```
git remote add origin https://github.com/{YOUR_USERNAME}/{YOUR_REPO}.git
git push -u origin main
```

Puis buildez à nouveau le site web :

```
ng deploy --base-href="/[your-repo-name]/"
```

### Problème sur Windows

Sur les machines (windows), vous pouvez rencontrer un problème comme celui-ci :

```
An error occurred!
 Error: Unspecified error (run without silent option for detail)
    at C:\Users\<myuser>\AppData\Roaming\nvm\v8.9.1\node_modules\angular-cli-ghpages\node_modules\gh-pages\lib\index.js:232:19
    at _rejected (C:\Users\<myuser>\AppData\Roaming\nvm\v8.9.1\node_modules\angular-cli-ghpages\node_modules\q\q.js:844:24)
    ...
```

Essayer de le déboguer avec `angular-cli-ghpages -S` . Si vous obtenez l'erreur suivante :

```
fatal: could not read Username for \'https://github.com\': No error\n',
```

Vous pouvez faire ce qui suit :

1. Créer un nouvel Access Token personnel ici : [https://github.com/settings/tokens](https://github.com/settings/tokens)
3. Lancer la commande suivante et remplacer votre token, organisation (votre utilisateur), dépôt, nom d'utilisateur et email :

    ```
    angular-cli-ghpages --repo=https://<personal-access-token>@github.com/organization/your-repo.git --name="Displayed Username" --email=mail@example.org
    ```
