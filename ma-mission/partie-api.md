# Partie API

Comme le développement Flutter de l'application avançait bien, il m'a été proposé de m'occuper également de la partie authentification sur les API afin que je découvre l'envers du décor.

L'objectif était de définir de nouvelles routes qui seront utilisé par l'application, pour s'authentifier, mais également pour récupérer les informations nécessaire au fonctionnement de l'application.

#### L'authentification

Pour l'authentification, il fallait définir 3 routes distinct pour représenter les 3 états possible :&#x20;

* Saisie du numéro : l'utilisateur n'est pas encore authentifié
* Saisie du code de validation : l'utilisateur doit confirmer son "identité"
* L'utilisateur c'est déjà authentifié et doit maintenant vérifier l'authenticité de sa clé d'authentification.

S'ajoute à cela les routes nécessaire à la récupération des information de l'utilisateur :&#x20;

* Récupérer l'abonnement
* Récupérer les options de l'abonnement
* Récupérer les métrics de l'abonnement
* Récupérer la consommation de données mensuelle de l'utilisateur

