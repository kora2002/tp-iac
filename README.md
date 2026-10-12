# TP Infrastructure as Code - Terraform et Docker

Déploiement d'un serveur Nginx dans un conteneur Docker avec Terraform.

## Prérequis

- Terraform (version 1.5.0 ou plus)
- Docker (le service doit être démarré : `docker info` doit fonctionner)
- Git

## Installation

Cloner le dépôt puis initialiser Terraform :

```bash
git clone https://github.com/kora2002/tp-iac.git
cd tp-iac
terraform init
```

## Déploiement

Vérifier la configuration, regarder le plan, puis appliquer :

```bash
terraform fmt -recursive
terraform validate
terraform plan
terraform apply
```

Taper `yes` pour confirmer. Les commandes sont aussi disponibles avec le Makefile :
`make init`, `make fmt`, `make validate`, `make plan`, `make apply`.

Pour utiliser d'autres valeurs (nom du conteneur, port, réseau), on peut
créer un fichier `.tfvars` et l'utiliser avec `-var-file`.

## Tests

Afficher les informations du déploiement :

```bash
terraform output
```

Ouvrir l'URL affichée par `website_url` dans un navigateur, ou tester avec :

```bash
curl http://localhost:8082
docker ps
docker network ls
```

À chaque push, GitHub Actions vérifie le formatage (`terraform fmt -check`)
et la validité de la configuration (`terraform validate`).

## Destruction

Regarder ce qui va être supprimé, puis détruire :

```bash
terraform plan -destroy
terraform destroy
```

Taper `yes` pour confirmer. Le conteneur et le réseau sont supprimés.
L'image Nginx reste sur la machine (`keep_locally = true`).

## Structure du projet

- `main.tf` : description de l'image, du réseau et du conteneur
- `variables.tf` : variables (nom du conteneur, port, nom du réseau)
- `outputs.tf` : informations affichées après le déploiement
- `html/index.html` : page servie par Nginx
- `Makefile` : raccourcis pour les commandes Terraform
- `.github/workflows/terraform.yml` : contrôles CI
- `reponses.txt` : réponses aux questions du TP