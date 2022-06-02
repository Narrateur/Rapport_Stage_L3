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

### API Public

Dans notre cas, le but de l'API Public est tout simplement de passer le relais à l'API CRM tout en vérifiant que toutes les informations nécessaire ont été fournie. En l'occurence des paramètres tels que le numéro de téléphone, la clé ou le code de validation dans le cas de l'authentification ou des headers comme la clé d'authentification ou la clé API, clé API qui est nécessaire pour toute les communications avec celle-ci (la clé API permet à l'API de savoir d'où viens la personne qui lui demande des ressources. Ici la personne viendra de l'application mobile VA Telecom).

```
api_mobile_device_set:
    pattern:   ^/api/customer_mobile_app/set
    provider:  mobile_device_user_provider
    security: false
    guard:
        authenticators:
            - App\Security\MobileDeviceAuthenticator

api_mobile_device_check:
    pattern:   ^/api/customer_mobile_app/check
    provider:  mobile_device_user_provider
    security: false
    guard:
        authenticators:
            - App\Security\MobileDeviceAuthenticator

api_mobile_device_login:
    pattern:   ^/api/customer_mobile_app/login
    provider:  mobile_device_user_provider
    stateless: true
    guard:
        authenticators:
            - App\Security\MobileDeviceAuthenticator
            
api_mobile_device_api:
    pattern:   ^/api/customer_mobile_app/
    provider:  mobile_device_user_provider
    stateless: true
    guard:
        authenticators:
            - App\Security\MobileDeviceAuthenticator
```

### API CRM

C'est ici que nos appels de Base de Données seront fait et que les informations voulus seront récupéré et renvoyé.

Puisque API Public ne fait que passer les requêtes à API CRM, celle-ci possède les même routes qu'API Public.
