# Vue.js Todo List

[Déjà déployé ici](https://todo.jlsquare.fr/)

- Jean-Loup Mellion
- Gatien Da Rocha

## Installation et Utilisation

### Prérequis

- [Node.js](https://nodejs.org/) (version 14 ou supérieure)
- [npm](https://www.npmjs.com/)

### Installation

1. **Téléchargez et décompressez** l'archive du projet.
2. **Naviguez** dans le répertoire du projet :

```bash
cd chemin/vers/le/dossier/todo-app
```

3. **Installez les dépendances** :

```bash
npm install
```

### Développement

1. **Lancez l'application** :

```bash
npm run dev
```

2. **Ouvrez** [http://localhost:5173/](http://localhost:5173/) dans votre navigateur.

### Production

1. **Construisez l'application** :

   ```bash
   npm run build
   ```

2. **Déployez** le contenu du dossier `/dist` sur votre serveur web. [Déjà déployé ici](https://todo.jlsquare.fr/) avec nginx + ssl.

### Utilisation

- **Ajouter une tâche** : Cliquez sur "Ajouter une tâche", remplissez le formulaire, et cliquez sur "Ajouter".
- **Modifier une tâche** : Cliquez sur "Modifier" à côté de la tâche, mettez à jour le formulaire, et cliquez sur "Mettre à jour".
- **Supprimer une tâche** : Cliquez sur "Supprimer" à côté de la tâche.
- **Filtrer les tâches** : Utilisez les filtres pour trier les tâches par état, priorité, date de début ou date de fin.
- **Mode sombre** : Activez/désactivez le mode sombre avec le bouton dédié.
- **Exporter/Importer des tâches** : Utilisez les boutons d'exportation et d'importation pour gérer les tâches au format JSON. (Pour changer de navigateur ou de périphérique par exemple, sinon les données sont toujours stockées en local dans le navigateur).
