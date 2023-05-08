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

Modifier la valeur de `CONTAINER_IMAGE` dans le workflow `build-and-push-container-image` pour matcher avec 

### Déclenchement du build

Pour déclencher le build, nous allons créer une nouvelle version de l'application en modifiant la version de l'application dans le fichier `package.json`.

![app versio](package-json.jpg)

Puis il s'agit de faire un commit et un push de la modification.

### Le pipeline

![pipeline](pipeline.jpg)

### Résultat dans Dockerhub




## Déploiement et synchronisation de l'application depuis ArgoCD

### Connexion à ArgoCD

L'interface d'ArgoCD est disponible ici : [ArgoCD](https://52.188.42.67)

Pour se connecter, utiliser votre username GitHub et le mot de passe initial `mewomewo`.

![argo login](argologin.jpg)


### Création de l'application


