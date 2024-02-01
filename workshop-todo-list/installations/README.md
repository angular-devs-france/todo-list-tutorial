# #1: ⌛ Installations

Bien qu'il soit possible de développer des applications Web avec un simple éditeur de texte, les outils disponibles rendent le développement plus facile et plus agréable. Nous aurons besoin d'un navigateur pour voir le résultat, de NodeJS pour exécuter des scripts sur notre ordinateur, et de NPM pour récupérer facilement des bibliothèques sur le Web. Avec NPM, nous installerons *Angular CLI*, qui exécutera un script avec NodeJS pour créer un projet de base pour nous, et utiliser NPM pour récupérer les bibliothèques dont nous aurons besoin pour le projet (comme Angular). Un IDE nous aidera à écrire le code et à gérer le projet.

En complément, nous recommandons Git pour gérer les versions de votre code, et GitHub pour le publier et le partager.

Jetez un coup d'œil au [tutoriel vidéo](https://www.facebook.com/719166003/videos/1048549972848310/) dans le groupe Facebook ngGirls où Shmuela montre comment vérifier et mettre à jour l'environnement de développement. (Vous devrez peut-être rejoindre le groupe pour y accéder.) Notez que cette vidéo a été enregistrée avec la version 16 d'Angular, et que les versions ultérieures ont des différences dans le contenu des fichiers créés et de l'application de base. De plus, Shmuela indique d'installer *Angular CLI* globalement sur l'ordinateur. Cependant, nous utiliserons `npx` pour créer le projet.

{% embed url="https://www.facebook.com/719166003/videos/1048549972848310/" %}

## Navigateur

Notre premier outil est le **navigateur**. Nous l'utiliserons pour voir le résultat de notre travail et le déboguer. Nous recommandons [Google Chrome](https://www.google.com/chrome/browser/desktop/) - il a de superbes outils de développement. [Microsoft Edge](https://www.microsoft.com/edge?WT.mc\_id=javascript-38439-shjacobs) est construit sur Chromium (le moteur open-source derrière Chrome) et a toutes les grandes fonctionnalités et les outils de développement. [Firefox](https://www.mozilla.org/en-US/firefox/new/) est également génial. Si vous n'en avez pas déjà un, cliquez simplement sur le lien correspondant et suivez les instructions pour télécharger et installer le navigateur de votre choix.

## IDE

Notre prochain outil est l'**IDE** , ou environnement de développement intégré. C'est un logiciel qui vous aide à écrire le code. Les IDE peuvent faire beaucoup de choses étonnantes, comme :

* colorer le code pour faciliter l'identification des expressions
* proposer de l'autocompletion
* aide a naviguer facilement dans les fichiers de votre projet
* et bien plus ...

Microsoft [Visual Studio Code](https://code.visualstudio.com/?WT.mc\_id=javascript-38439-shjacobs) (VS Code) est un IDE très populaire. Il est léger, extensible, open-source et **gratuit**. Vous pouvez trouver de nombreux plugins pour améliorer votre productivité ou simplement pour vous amuser, ou même écrire les vôtres.

JetBrains [Webstorm](https://www.jetbrains.com/webstorm/download/) est l'un des IDE les plus puissants du marché. Il est payant mais vous obtiendrez le premier mois gratuitement, et une licence totalement gratuite si vous êtes étudiante.

Choisissez l'IDE avec lequel vous souhaitez travailler et suivez les instructions d'installation sur son site Web.

### **Plugins**

Les plugins aident l'IDE à comprendre le code. Webstorm est livré avec les plugins nécessaires. Si vous choisissez d'utiliser VS Code, nous vous recommandons d'installer le [pack d'extension Angular Essentials](https://marketplace.visualstudio.com/items?itemName=johnpapa.angular-essentials\&WT.mc\_id=javascript-38439-shjacobs).

## NodeJS and NPM

**Veillez à vérifier les** [**prérequis d'*Angular CLI***](https://angular.io/guide/setup-local#prerequisites) **pour les versions de NodeJS et NPM à jour !**

Un autre outil que la plupart des développeurs Web utilisent est **NodeJS**. Une fois installé, il est livré avec **NPM** (Node Package Manager).

NodeJS vous permet d'exécuter du code JavaScript sur votre ordinateur. Il est utilisé pour exécuter un serveur local qui sert les fichiers du projet au navigateur et simule un site Web réel.

NPM vous permet de télécharger et d'installer facilement différentes bibliothèques depuis Internet et de gérer leurs versions.

**Téléchargez NodeJS** [**ici**](https://nodejs.org/)**.**

Si vous avez déjà NodeJS installé, assurez-vous que la version correspond aux prérequis en exécutant ceci dans votre ligne de commande / terminal :

{% code title="command-line" %}
```
node -v
```
{% endcode %}

('-v' pour 'version')

Si la version est inférieure à celle requise, vous devez faire attention lors de l'installation d'une nouvelle version, car vous pourriez avoir des projets qui dépendent de la version que vous avez. Utilisez Node Version Manager (NVM) pour installer la version requise. Consultez cette [question Stack Overflow](https://stackoverflow.com/questions/8191459/how-do-i-update-node-js) pour en savoir plus.

Une fois installé, vous devriez également avoir NPM installé. Vérifiez sa version en exécutant :

{% code title="command-line" %}
```
npm -v
```
{% endcode %}

## Git

Git est un outil qui vous aide à gérer les versions de votre code et à travailler en collaboration avec les membres de l'équipe. Il y a beaucoup de choses à savoir à ce sujet, mais dans ce tutoriel, nous ne couvrirons que l'utilisation de base.

Vous pouvez le télécharger et suivre les instructions d'installation [ici](https://git-scm.com/) .
Quand on vous demande si vous souhaitez installer **git bash**, répondez oui.

{% hint style="info" %}
Nous vous recommandons d'installer ou de mettre à jour vers la dernière version de Git pour profiter de leurs mises à jour de sécurité.
{% endhint %}

## GitHub

[GitHub](https://github.com/) est une application, qui s'intègre à Git. Elle vous permet de publier votre projet (code) sur le Web, de copier (fork et clone) d'autres projets open source et de collaborer. Vous pouvez créer des référentiels publics et privés et inviter des collaborateurs. Certaines méthodes de déploiement utilisent GitHub pour publier votre application. Pour pouvoir utiliser GitHub pendant l'atelier, assurez-vous de créer un utilisateur sur GitHub (gratuitement, bien sûr).

Nous vous recommandons vivement d'utiliser GitHub pour partager votre projet avec votre mentor. Vous et votre mentor pourrez examiner le code, y apporter des commentaires, ouvrir des discussions, etc.

Accédez à GitHub : [https://github.com/](https://github.com/). Cliquez sur **Sign up**. Remplissez le formulaire d'inscription et assurez-vous de valider votre adresse e-mail. Il est recommandé d'utiliser l'authentification à deux facteurs pour une sécurité accrue.

## Créer un projet avec *Angular CLI*

[*Angular CLI*](https://cli.angular.io) est un outil puissant qui simplifie beaucoup le processus de développement. Il installe également des bibliothèques que vous utiliserez dans vos projets actuels et futurs.

Avec une fonctionnalité relativement nouvelle de NPM, vous n'avez pas besoin d'installer *Angular CLI* sur votre ordinateur pour créer un projet. La commande `npx` sait où trouver le package Angular-CLI par son nom `@angular/cli` . Il téléchargera le package (si vous ne l'avez pas déjà installé) et exécutera sa commande `new` .

> Même si vous avez *Angular CLI* installé à partir d'un projet précédent sur lequel vous travailliez, nous dirons à npx d'utiliser la dernière version. Donc, si votre version installée est ancienne, vous obtiendrez toujours un projet créé avec la dernière version.

Tout d'abord, créez un dossier pour stocker tous vos projets, par exemple _myProjects_, puis allez dans le dossier, en utilisant la ligne de commande :

{% code title="command-line" %}
```
cd the-path-to-your-folder/myProjects
```
{% endcode %}

Maintenant, créez un projet Angular en exécutant :

```
npx @angular/cli@latest new todo-list
```

*Angular CLI* posera quelques questions pour vous aider à créer une nouvelle application. Répondez aux questions comme indiqué ci-dessous :

1. Which stylesheet format would you like to use? (Use arrow keys): Select **SCSS**
2. Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)?  (y/N): **N**

Cela peut prendre un certain temps, car de nombreux packages sont téléchargés depuis le Web et installés.

_**Si, pour une raison quelconque, la commande**_ _**`npx`**_ _**ne fonctionne pas, suivez les**_ [_**instructions en bas de cette page**_](./#if-npx-doesnt-work)_**,**_ _**puis revenez ici pour exécuter votre projet.**_

Apprenez-en plus sur *Angular CLI* dans la section suivante.

### Exécution de votre projet

Entrez dans le nouveau dossier que *Angular CLI* a créé pour ce projet :

{% code title="command-line" %}
```
cd todo-list
```
{% endcode %}

Une fois dans le dossier de l'application, exécutez l'application en utilisant la commande suivante :

{% code title="command-line" %}
```bash
ng serve -o
```
{% endcode %}

`ng` est la commande qui exécute la version locale *Angular CLI* installée dans votre projet. Nous utiliserons cette commande pour créer de nouveaux composants, construire le projet, et plus encore.

Le flag `-o` est un raccourci pour `--open`, qui ouvrira votre navigateur à la bonne URL : [`localhost:4200`](http://localhost:4200)

Vous devriez voir la page comme ceci :

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption><p>start screen. welcome message depends on project name</p></figcaption></figure>


## Félicitations !

Vous avez une application Angular en cours d'exécution ! **Gardez le terminal où vous avez exécuté la commande `ng serve` ouvert pendant que vous travaillez sur l'application.** Les modifications que vous apportez au code du projet sont immédiatement reflétées dans le navigateur Web.
Vous pouvez ouvrir un autre terminal pour effectuer des tâches en parallèle.

Pour arrêter l'application, appuyez sur `Ctrl+C` dans le terminal, ou fermez le terminal.

Maintenant, nous sommes prêts à commencer à développer !



## 💾 Enregistrez votre code sur GitHub

*Angular CLI* est déjà configuré avec git, a inclus tous les fichiers du projet et a effectué le premier commit. Vous pouvez déjà commencer à utiliser GitHub pour enregistrer votre projet en ligne.

Vous vous demandez peut-être pourquoi nous enregistrons sur GitHub maintenant. Enregistrer le travail de codage en cours dans un référentiel de code accessible est une bonne pratique de développement de logiciels. Cela permet également aux mentors de mieux vous aider à cette étape et au fur et à mesure que nous travaillons sur le tutoriel. Nous voulons que vous puissiez continuer à travailler sur le tutoriel si vous manquez de temps pendant l'atelier.

Allez à [Appendice 1: Git et GitHub](../appendix-1-git-and-github.md) pour obtenir des instructions sur la publication de votre code.

## Déployer votre application

A ce stade, vous pouvez déjà déployer votre application. Cela signifie qu'elle sera disponible en ligne pour tout le monde ! Il existe plusieurs entreprises et méthodes qui peuvent héberger votre application Web. Choisissez une méthode et obtenez les instructions dans [Appendice 2: Déployer votre application.](../appendix-1-deploying-your-app/)
