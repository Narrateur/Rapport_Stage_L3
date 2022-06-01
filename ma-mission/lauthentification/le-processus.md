---
description: Comment l'utilisateur s'authentifie-t-il ?
---

# Le processus

Un schéma à donc été réalisé afin de détailler tous les états possible lors de l'authentification :

![Schéma du processus d'authentification](<../../.gitbook/assets/App\_Authentication (1).jpg>)

Lors du lancement de l'application, celle-ci recherche dans le Secure Storage les informations qui y sont stockés : la AuthKey (clé définitive), la pendingKey (clé qui signifie que l'authentification est toujours en cours), le numéro de téléphone, le authCode (code de validation) et le authCodeTime (date et heure à laquelle le sms contenant le code de validation a été envoyé). A partir des informations récupéré, l'utilisateur est redirigé vers l'étape d'authentification la plus adapté.

* S'il y a une authKey (X-AUTH-MOBILE-KEY), alors un appel API est effectué avec cette clé en paramètre afin de vérifier son authenticité. Si un abonnement est trouvé avec cette clé, alors l'utilisateur est bien authentifié et peut accéder à l'application. Dans le cas contraire, il est renvoyé sur la page de saisie de numéro afin de l'authentifier de nouveau. L'utilisateur a à ce moment là toujours la possibilité de ressayer (refaire un appel API).
* S'il y a un numéro de téléphone et une pendingKey, alors l'utilisateur est redirigé vers la page de saisie de code de validation. L'utilisateur a toujours la possibilité d'accéder à la page de saisie de numéro de téléphone, afin de recevoir un nouveau code de validation.
* Si aucune de ces informations ne sont présentes, l'utilisateur est renvoyé sur la page de saisie du numéro de téléphone.

Le processus d'authentification se déroule comme suit :

*   Saisie du numéro de téléphone :&#x20;

    Une fois celui-ci renseigné, s'il est bien renseigné, redirige l'utilisateur sur la page de saisie du code. Ensuite une clé est généré, puis haché en SHA-256, et envoyé avec le numéro dans une requête API. Une fois reçu, l'API génère un code de validation à 6 chiffres, envoie un sms avec ce code et stocke dans la base de données le numéro et la pendingKey reçu, et le code généré.
*   Saisie du code de validation :

    L'utilisateur reçu le SMS et saisie le code de validation. Un appel API est fait avec le numéro et la pendingKey stocké localement, et le code de validation saisie. L'API vérifie alors les informations reçu avec celles stockés. Si tous correspond, la pendingKey devient authKey et la authKey est renvoyé, rien sinon. Côté application, si une clé est renvoyé, elle est stocké dans le Secure Storage du téléphone, la pendingKey est supprimé et l'utilisateur est renvoyé sur la « page d'accueil ».
* Lorsque l'utilisateur se trouve sur la page de saisie du code de validation, il a toujours la possibilité de revenir sur la page de saisie du numéro de téléphone. Afin d'éviter que l'utilisateur entre son numéro à plusieurs reprise et reçoive plusieurs messages, au bout du troisième essai, un délai de 5 minutes a été ajouté entre l'envoie de 2 SMS. De plus, le code à 6 chiffres n'est valide que 10 minutes.

![Page de saisie du numéro de téléphone](../../.gitbook/assets/Screenshot\_V3\_1.jpg) ![Erreur si l'utilisateur tente de s'envoyer de nouveau un SMS de validation](../../.gitbook/assets/Screenshot\_V3\_2.jpg) ![Page de saisie du code de validation](../../.gitbook/assets/Screenshot\_V3\_3.jpg)
