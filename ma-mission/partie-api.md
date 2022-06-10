# Partie API

Comme le développement Flutter de l'application avançait bien, il m'a été proposé de m'occuper également de la partie API afin que je découvre l'envers du décor.

L'objectif était de définir de nouvelles routes qui seront utilisées par l'application, pour s'authentifier d'une part, mais également pour récupérer les informations nécessaires au fonctionnement de l'application.

#### L'authentification

Pour l'authentification, il nous fallait définir 3 routes distinct pour représenter les 3 états possibles:

* customer\_mobile\_app/set : l'utilisateur n'est pas encore authentifié. L'API reçoit donc un numéro de téléphone et une pendingKey.
* customer\_mobile\_app/check : l'utilisateur doit confirmer son "identité". L'API reçoit donc un numéro de téléphone, une pendingKey et un code de validation.
* customermobile\_app/login : L'utilisateur c'est déjà authentifié et doit maintenant vérifier l'authenticité de sa clé d'authentification. L'API reçoit alors une X-AUTH-MOBILE-KEY dans ses headers.

S'ajoute à cela les routes nécessaires à la récupération des informations de l'utilisateur :

* Récupérer l'abonnement
* Récupérer les options de l'abonnement
* Récupérer les metrics de l'abonnement (similaire à un système de tags)
* Récupérer la consommation de données mensuelle de l'utilisateur
* Récupérer le produit
* Récupérer les métrics du produit

### API Public

Dans notre cas, le but de l'API Public est tout simplement de passer le relais à l'API CRM (interne et non accessible depuis l'exterieur) tout en vérifiant que toutes les informations nécessaires ont été fournie. En l'occurence des paramètres tels que le numéro de téléphone, la clé ou le code de validation dans le cas de l'authentification ou des headers comme la clé d'authentification ou la clé API, clé API qui est nécessaire pour toutes les communications avec celle-ci (la clé API permet à l'API de savoir d'où viens la personne qui lui demande des ressources. Ici la personne viendra de l'application mobile VA Telecom).

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

C'est ici que nos requêtes de Base de Données seront faites et que les informations voulus seront récupérées et renvoyées.

Puisque API Public ne fait que passer les requêtes à API CRM, celle-ci possède les mêmes routes qu'API Public.

```
/**
 * Vérifie que la X-AUTH-MOBILE-KEY passée dans les headers 
 * correspond bien à un abonnement
 * 
 * @Route("api/customer_mobile_app/abonnement", methods={"GET"}, )
 */
public function getAbonnemenetMobileDevice(
    Request $request, 
    HttpClientInterface $crmAdminClient
){
    
    if($request->headers->has('X-AUTH-MOBILE-KEY')){

        // Récupere l'abonnement
        $abonnement = $this->em->getRepository(Abonnement::class)
                        ->getAbonnementByAuthKey(
                            $request->headers->get('X-AUTH-MOBILE-KEY')
                        );

        if($abonnement){
            // Appel du normalizer pour récuperer la structure à exposer
            $abonnementData = $this->serializer->normalize(
                $abonnement, 'json', 
                ['groups' => 'mobileDeviceAbonnement:get']
            );
            return new JsonResponse($abonnementData);
        }else{
            return new JsonResponse('No Abonnement for received Key','404');
        }
    }else{
        return new JsonResponse('Authentication error ', '500');
    }
}
```

Symfony fonctionne avec un système d'annotation. Ici, dans la première partie commenté, se trouve la route qui nous permet de communiquer, "@Route". C'est cette route que nous devons utiliser pour obtenir notre abonnement.

Dans cette application, la clé généré lors de notre authentification nous identifie, et doit être passée dans touts les appels. On peut d'ailleurs le constater dans la fonction, l'abonnement est récupéré avec la fonction getAbonnementByAuthKey() avec la clé d'authentification (X-AUTH-MOBILE-KEY) en paramètre.

Le processus est pratiquement le même pour toutes les fonctions de cette partie de l'API, à quelque exceptions près.&#x20;
