# Bloc, qu'est ce que c'est ?

Contrairement à la première fois, l'application sera développé en Bloc.&#x20;

#### LE BLOC:

La programmation par Bloc permet de délocaliser la logique métier dans les Blocs. Bloc fonctionne avec des Events, des Blocs et des States.

* Les Events permettent de lancer un Bloc précis, un peu comme un appel de fonction.
* Les Blocs exécutent des actions, des appels API, récupèrent des informations ou encore construisent des objets, puis émettent un State.
* Les States contiennent les résultats du travail effectué par le Bloc. Si le State change, les BlocProviders associé dans toute l'application seront reconstruits. Si le State ne change pas, rien ne bouge.

Grâce aux Blocs, nous sommes en mesure de produire une application qui réagira différemment selon les States renvoyé par les Blocs, nous permettant ainsi d'afficher des éléments selon un status particulier. Ici, nos States contiennent plusieurs informations: un status (status de l'action en cours), un message (pour les erreur par exemple) et un objet (souvent la résultat de l'Event demandé). De plus, déclarer nos Bloc tôt dans l'application nous permet d'y accéder dans toutes les pages qui en hérite.

![Le lieu de déclaration du Bloc agit comme une cascade.](<../../.gitbook/assets/Shéma Bloc.jpg>)

Dans cet exemple, n'importe quelle page peut accéder aux informations des Bloc Client et Consommations. Par contre, la page 2 et les autres pages qui en découle ne peuvent récupérer les informations du Bloc Options. Grâce à Bloc, modifier une information dans le Bloc Client dans la page 5 la modifiera dans toutes les autres.&#x20;

#### Un exemple devrait être plus parlant.&#x20;

Prenons une page, dans laquelle nous avons un Bloc Client et un bouton "Afficher info client". Lorsqu'on appuis sur ce bouton, nous ajouterons un Event "GetInfoClient" à notre Bloc. Puisqu'il à un nouvel Event, notre bloc se met au travail. A peine eut-il le temps de commencer qu'il émet un State avec un status "inProgress". De cette manière, nous pouvons prévoir un affichage sur notre page pour un status "inProgress": un cercle de chargement par exemple. Le Bloc fait ses appels API nécessaire à la réalisation de sa mission, construit son objet Client à partir de ces informations et émet un nouveau State avec un status "success" et son objet Client. Il nous reste plus qu'a prévoir le cas d'un "success" sur notre page et ainsi afficher les informations contenu dans notre objet Client, accessible depuis notre State.&#x20;
