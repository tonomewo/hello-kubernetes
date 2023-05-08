# Exercice GitOps


Cet exercice vous permet de déployer une application `hello-kubernetes` en 2 étapes : 

1. Création de l'image Docker et upload dans DockerHub
2. Création de l'application dans ArgoCD et synchronisation


## Pré-requis

Les pré-requis suivants sont nécessaires :

- Docker Engine ou Docker Desktop installé sur votre poste
- Création d'un compte [DockerHub](https://hub.docker.com/) (c'est gratuit)
- Outil de VCS installé (VSCode ou autre)
- Fork du [repo](https://github.com/smontri-mewo/hello-kubernetes.git)

```
Attention : DockerHub n'accepte pas les - dans le nom de repository
```

## Pipeline de build de l'image

Le pipeline `GitHub Actions` est déjà crée dans le repo.
Il s'agit du du workflow `build-and-push-container-image`.

### Secrets

Ces secrets doivent être crée via Settings | Secrets and variables | Actions 


| Name | Description | 
| ---- | ----------- |
| DOCKERHUB_USERNAME | Utilisateur pour se connecter à DockerHub |
| DOCKERHUB_PASSWORD | Mot de passe pour se connecter à DockerHub |


![Hello world! from the hello-kubernetes image](secrets.jpg)

### Variables

| Name | Description |
| ---- | ----------- |
| IMAGE_VERSION | version de l'image dans src/app/package.json |

### Configuration du build

Modifier la valeur de `CONTAINER_IMAGE` dans le workflow `build-and-push-container-image` pour correspondre à votre registre et repository docker !

### Déclenchement du build

Pour déclencher le build, nous allons créer une nouvelle version de l'application en modifiant la version de l'application dans le fichier `package.json`.

![app versio](package-json.jpg)

Puis il s'agit de faire un commit et un push de la modification.

### Le pipeline

![pipeline](pipeline.jpg)

### Résultat dans Dockerhub

![dockerhub](dockerhub.jpg)

## Déploiement et synchronisation de l'application depuis ArgoCD

### Connexion à ArgoCD

L'interface d'ArgoCD est disponible ici : [ArgoCD](https://52.188.42.67)

Pour se connecter, utiliser votre username GitHub et le mot de passe initial `mewomewo`.

![argo login](argologin.jpg)


### Création de l'application

Depuis l'interface d'ArgoCD, créer une nouvelle application en suivant les consignes ci-dessous :

![app1](app1.jpg)

![app2](app2.jpg)

![app3](app3.jpg)

Pour finaliser la création de l'application, un petit clic sur `Create`.

L'application est visible depuis la console dans la liste des applications :

![app4](app4.jpg)

Et en cliquant dessus on accès au détail :

![app5](app5.jpg)

On constate que celle-ci n'est pas synchronisée, Git à jour mais aucune application déployée dans le cluster.

### Première synchronisation

Pour synchroniser l'application, un clic sur `Sync` et puis `Synchronize`.

![sync](sync.jpg)

Une fois réconciliée, l'application apparaît dans un état synchronisé.

![synced](synced.jpg)

L'application est accessible via l'url (LoadBalancer). Une fois l'application déployée, demandez l'adresse pour accéder à l'application.

![app_initiale](app_initiale.jpg)

### Modification de l'application

Nous allons modifier le message affiché dans la page web de l'app, en mettant à jour la valeur de `message` dans le fichier `values.yaml`.

![modification](modification.jpg)

Dans l'interface d'ArgoCD, l'application apparaît comme désynchronisée, le repo Git et l'application déployée sont différentes.

![desync](desync.jpg)

Pour réconcilier, cliquer sur `Sync` puis `Synchronize`.

![desync2](desync2.jpg)

Le statut est mis à jour :

![resync](resync.jpg)

Et l'application égalemernt :

![new_app](new_app.jpg)

### Fin et destruction

Fin de l'exercice, lancez un `delete` de l'application pour supprimer les objets Kubernetes crées dans le cluster.

![delete](delete.jpg)