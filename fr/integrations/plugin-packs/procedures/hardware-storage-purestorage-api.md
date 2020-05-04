---
id: hardware-storage-purestorage-restapi
title: Pure Storage RestAPI
---

## Vue d'ensemble

Pure Storage est un fabricant de matériel de stockage flash pour les datacenters. Il fournit également un logiciel propriétaire de déduplication et de compression des données afin d'optimiser la quantité stockée sur chaque disque. 

## Contenu du Plugin-Pack

### Objets supervisés

* Baies de stockage

## Métriques collectées                                                                                             

Pour plus d'informations sur les métriques collectées ainsi que le fonctionnement de l'API, vous pouvez vous référer à la documentation officielle Pure Storage: https://blog.purestorage.com/introducing-the-pure1-rest-api/

<!--DOCUSAURUS_CODE_TABS-->
<!--Alarms-Global-->

| Metric name        | Description                                              |
| :----------------- | :------------------------------------------------------- |
| Status             | Status of alarms. Threshold/Unit: String                 |

<!--Hardware-Global-->

| Metric name        | Description                                             |
| :----------------- | :------------------------------------------------------ |
| Status             | Status of components. Threshold/Unit: String            |

<!--Volume-Usage-Global-->

| Metric name        | Description                                              |
| :----------------- | :------------------------------------------------------- |
| Volume-Usage       | The usage of volume. Unit: Bytes or %                    |
| Data-Reduction     | The data-reduction ratio on the volume. Unit: ratio      |
| Total-Reduction    | The total-reduction on the volume. Unit: count           |
<!--END_DOCUSAURUS_CODE_TABS-->

## Prérequis

* ce Plugin nécessite une version de l'API Pure Storage >= 1.11
(https://static.pure1.purestorage.com/api-swagger/index.html).

* Un compte de service autorisé à se connecter à l'équipement doit être créé
Ce compte doit bénéficier d'un accès en lecture seule à la baie de stockage.
 
## Installation

<!--DOCUSAURUS_CODE_TABS-->

<!--Online IMP Licence & IT-100 Editions-->

1. Installer le Plugin sur l'ensemble des collecteurs supervisant des baies Pure Storage:

```bash
yum install centreon-plugin-Hardware-Storage-Purestorage-Restapi
```

2. Installer le Plugin-Pack depuis la page "Configuration > Plugin packs > Manager"


<!--Offline IMP License-->

1. Installer le Plugin sur l'ensemble des collecteurs supervisant des baies Pure Storage:

```bash
yum install centreon-plugin-Hardware-Storage-Purestorage-Restapi
```

2. Installer le RPM du Plugin-Pack contenant les modèles de supervision :

```bash
yum install centreon-pack-hardware-storage-purestorage-restapi
```

3. Installer le Plugin-Pack depuis la page "Configuration > Plugin packs > Manager"

<!--END_DOCUSAURUS_CODE_TABS-->

## Configuration

Appliquez le modèle "HW-Storage-Purestorage-Restapi-custom" à votre hôte nouvellement créé. 
Renseignez ensuite les macros d'hôtes marquées comme obligatoires ci-dessous: 

| Obligatoire | Name            | Description                                |
| :---------- | :-------------- | :----------------------------------------- |
| X           | APIURLPATH      | URL de l'API de Pure Storage               |
| X           | APIURLUSERNAME  | Nom d'utilisateur de l'API de Pure Storage |
| X           | APIURLPASSWORD  | Mot de passe de l'API de Pure Storage      |
|             | APIEXTRAOPTIONS | Extra options de l'API de Pure Storage     |

## FAQ

#### Comment puis-je tester le Plugin et quelles sont les options disponibles ?

Une fois le Plugin installé, vous pouvez tester celui-ci directement en ligne de commande depuis le collecteur Centreon
en vous connectant avec l'utilisateur *centreon-engine* puis en lançant la commande suivante:

```bash
/usr/lib/centreon/plugins//centreon_purestorage_restapi.pl
	--plugin=storage::purestorage::restapi::plugin
	--mode=volume-usage
	--hostname=192.168.1.1
	--api-path='/api/1.11'
	--username='centreon'
	--password='centreon' 
	--filter-name='.*'
	--units='%'
	--warning-usage=''
	--critical-usage=''
	--warning-data-reduction=''
	--critical-data-reduction=''
	--warning-total-reduction=''
	--critical-total-reduction=''
	--verbose
```

```
OK: Volume 'PROD::CENTREON' Usage Total: 6.00 TB Used: 1.13 TB (18.85%) Free: 4.87 TB (81.15%), Data Reduction : 2.917, Total Reduction : 5.193, Snapshots : 0.00 B
```

Cette commande vérifie l'utilisation du volume de stockage Pure Storage (```--mode=volume-usage```) en utilisant l'url api (```--api-path='/api/1.11'```). Elle fournit également les informations concernant l'optimisation de l'espace disque. 
