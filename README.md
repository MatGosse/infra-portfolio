  Stack portfolio
===========================



Instances du projet portfolio.

Cette stack est déployée sur le cluster k8s ovh.

Le déploiement est géré par helm pour simplifier (templater)
les différentes instances (dev/preprod/...)

Je ne reviens pas ici sur l'installation des prérequis
* kubectl
* helm
* un accès au cluster (export KUBECONFIG...)

## Préparer les fichiers pour une instance

## Présrequis installer
* installer la stack traefik
* installer les dependances lier a la generation des secrets
```bash
  helm repo add mittwald https://helm.mittwald.de &&
  helm repo update &&
  helm upgrade --install kubernetes-secret-generator mittwald/kubernetes-secret-generator
```

S'inspirer du fichier `chart/values.yaml` pour créer le fichier spécifique de l'instance et l'éditer pour adapter les valeurs.

## Dépanage

### Si postgres "record is corrupted"

```shell
command: [ "/bin/bash", "-c", "sleep 600" ] -> un comment this line
helm upgrade --namespace portfolio  portfolio-develop ./chart -f dev.yaml
kubectl exec -it <id-pod-api> -n portfolio -c postgres -- /bin/bash
su postgres
pg_resetwal /var/lib/postgresql/data
```

## Déployer 

### La première fois
```shell
helm install --namespace portfolio --create-namespace  portfolio-develop ./chart -f dev.yaml
```

### Mettre à jour (pour les fois d'après)

```shell
helm upgrade --namespace portfolio portfolio-develop ./chart -f dev.yaml
```
