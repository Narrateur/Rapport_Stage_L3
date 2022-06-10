# Comment s'authentifier ?

Définir la méthode d'authentification a été un point important lors du développement. Il s'agit du premier processus lancé par l'application, celui qui permettra de récupérer toutes les informations de l'utilisateur et vérifier que celui-ci peut avoir accès aux données de l'abonnement.

Le numéro de téléphone rattaché à la sim est lié à l'abonnement de l'utilisateur. Connaitre le numéro de téléphone nous permet donc de savoir à qui il appartient. Mais avant ça, comment est organisé le système informatique de VA Solutions ?

![Schéma des API de VA Solutions](<../../.gitbook/assets/Schema API.jpg>)

Tous ce qui vient de l'extérieur n'a accès qu'à l'API Public. Elle vérifie quel type d'utilisateur nous sommes et passe le relais à l'API interne concernée. Puisque l'utilisateur ne s'authentifiera pas avec un couple identifiant/mot de passe, il faudra donc effectuer quelques ajouts aux API concernées.

Sachant cela, 2 solutions d'authentification ont été proposés :

1. Lors du démarrage de l'application, une clé unique est générée par l'application et l'envoie à l'API par SMS, l'API récupère le numéro de téléphone via le SMS, et lie la clé à l'abonnement (trouvé grâce au numéro). La clé est récupérée et stockée sur le téléphone et servira pour toutes les communications avec l'API. A noter qu'à la place d'envoyer un SMS, nous aurions pu faire un appel API. Le numéro de téléphone aurai alors été récupéré par le téléphone et passé en paramètre lors de l'appel, avec la clé.
2. Au lancement de l'application, l'utilisateur saisie son numéro de téléphone dans l'interface, puis clique sur un bouton « valider ». Là, une requête est envoyé à l'API, contenant le numéro de téléphone saisie ainsi qu'une clé générée aléatoirement. L'API récupère le numéro de téléphone et la clé, les stocke dans la base, et un code de validation à 6 chiffres qui sera envoyé à l'utilisateur par SMS au numéro saisie. L'utilisateur rentre alors le code de validation qu'il a reçu, puis lorsqu'il appuiera sur « confirmer », une nouvelle requête, contenant le numéro de téléphone précédemment saisi, la clé générée et le code de validation entrée, sera faite à l'API, qui se chargera de comparer la combinaison numéro/clé/code avec la combinaison numéro/clé/code stockée dans la base. Si tout correspond, la clé "abonnement.pending.key" dans la base devient "abonnement.auth.key" et servira pour toutes les futures communications de l'application vers l'API.

Sur le papier, la première solution semble meilleure. L'utilisateur ne fait rien, il attend juste les autorisations pour accéder à l'application.

Cependant elle soulève quelques problèmes :

1. Afin d'envoyer automatiquement un SMS ou d'effectuer l'appel API, il faut récupérer le numéro de téléphone (pour le SMS ou pour le paramètre de l'appel API). Il existe des packages Flutter qui permettent de récupérer les informations de la carte SIM, cependant, iOS devient de plus en plus restrictif concernant les informations liées aux terminaux et risque d'émécher l'application de fonctionner. De manière générale, il n'est pas ou difficilement possible de récupérer ces informations sur iOS.
2. L'envoie automatique de SMS n'est plus possible. Flutter a évolué et ne permet plus ce genre d'automatisme. L'option la plus proche serait d'ouvrir l'application d'envoie de SMS par défaut du téléphone, avec un message pré-remplie, mais cela nuirait à l'expérience utilisateur.

Puisque l'application doit être disponible pour Android **et** iOS, c'est donc la 2ème solution qui à été choisie. Le processus peut être un peu plus long, mais il est simple et réalisable et n'émèchera pas son fonctionnement malgré les possibles évolutions des OS.
