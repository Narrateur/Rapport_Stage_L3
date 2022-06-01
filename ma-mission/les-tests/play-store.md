# Play Store

Google Play Console permet de tester et déployer les applications Android.

Pour sortir une application sur le Play Store, il suffit de suivre la documentation fournie par Flutter sur ce sujet : [https://docs.flutter.dev/deployment/android](https://docs.flutter.dev/deployment/android)

L'obfuscation et la minification du code sont par défaut activé lors de la construction de l'AAB et sa mise en tests sur le Google Play Console. L'obfuscation permet de rendre le code difficilement compréhensible pour l'humain tout en étant parfaitement compilable pour l'ordinateur. La minification quand à elle réduit la taille du code, en supprimant les retours à la ligne et l'indentation par exemple. La minification peut être considéré comme de l'obfuscation.&#x20;

L'application se trouve d'abord en tests interne. Ainsi seul les personnes ajouté au groupe de testeurs pourront télécharger l'application via le Play Store.

Pour que les testeurs puissent avoir accès à l'application, il faut leurs fournir des release. Ces releases représentent une version de l'application. Lorsque l'on souhaite faire tester notre application, il faut produire un App Bundle. C'est ce fichier en .aab que l'on mettra dan notre release et qui sera ensuite téléchargé par les testeurs via le Play Store.

![Interface du Google Play Console](<../../.gitbook/assets/Capture d’écran 2022-06-01 à 13.12.54.png>)

A noter que le nombre à coté de la version ( le 11 dans 11(1.0.0) par exemple) représente le numéro de build et doit être incrémenté manuellement depuis le pubspec.yaml du projet.
