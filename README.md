# TP Infrastructure as Code avec Terraform et Docker

Ce projet déploie un serveur Nginx dans un conteneur Docker avec Terraform. Il crée aussi un réseau Docker dédié et monte le dossier `html/` dans le conteneur.

## Installation

Il faut installer Terraform, Docker et Git, puis vérifier que tout fonctionne :

```bash
terraform -version
docker info
git --version
```

Ensuite, on initialise le projet :

```bash
terraform init
terraform fmt -recursive
terraform validate
```

## Déploiement

```bash
terraform plan
terraform apply
terraform output
```

On confirme avec `yes` après avoir relu le plan. Le fichier `Makefile` propose aussi des raccourcis comme `make plan` et `make apply`.

## Tests

Le site est accessible sur http://localhost:8080. On peut aussi tester avec :

```bash
curl http://localhost:8080
docker ps
docker network ls
```

Pour changer de port ou de nom de conteneur, on peut modifier les variables `external_port` et `container_name` dans `variables.tf`.

## Destruction

```bash
terraform plan -destroy
terraform destroy
```

Après la destruction, `docker ps -a`, `docker network ls` et `terraform state list` ne doivent plus montrer le conteneur ni le réseau. L'image Nginx reste en local à cause de `keep_locally = true`.

## Intégration continue

Le workflow GitHub Actions dans `.github/workflows/terraform.yml` vérifie le formatage et la validation du code à chaque push et pull request. Il ne déploie rien.
