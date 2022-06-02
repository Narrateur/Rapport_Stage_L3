# Play Store

Google Play Console permet de tester et déployer les applications Android.

Pour tester et déployer une application sur le Play Store, il suffit de suivre la documentation fournie par Flutter sur ce sujet : [https://docs.flutter.dev/deployment/android](https://docs.flutter.dev/deployment/android)

#### Les APK et les AAB

Historiquement, les **APK (Android Packages)** étaient le format universel pour les applications Android. Ils sont aujourd'hui remplacés par des **AAB (Android App Bundle)**. La grande différence entre ces deux formats réside lors du téléchargement depuis le Play Store. Télécharger un APK signifie télécharger toutes les ressources disponibles (les ressources sont tout ce qui n'est pas du code, comme les images, les fichiers de langue ou le son), alors qu'un AAB construira un APK qui ne contiendra que les ressources dont vous avez besoin. Ainsi, si votre smartphone n'a qu'une résolution HD, vous obtiendrez alors un APK sans ressources 4K. Si vous avez choisi le français et l'anglais comme langue sur votre appareil,  vous ne recevrez pas les ressources pour de l'espagnol.



L'obfuscation et la minification du code sont par défaut activés lors de la construction de l'AAB et sa mise en tests sur le Google Play Console. L'obfuscation permet de rendre le code difficilement compréhensible pour l'humain tout en étant parfaitement compilable pour l'ordinateur. La minification quand à elle réduit la taille du code, en supprimant les retours à la ligne et l'indentation. La minification peut être considéré comme de l'obfuscation.&#x20;

L'application se trouve d'abord en tests internes. Ainsi seul les personnes ajoutées au groupe de testeurs pourront télécharger l'application via le Play Store.

Pour que les testeurs puissent avoir accès à l'application, il faut créer des releases. Ces releases représentent une version de l'application à l'instant T. Lorsque l'on souhaite faire tester notre application, il faut produire un App Bundle. C'est ce fichier en .aab que l'on mettra dans notre release et qui sera ensuite téléchargé par les testeurs via le Play Store. Il est généré via la commande :&#x20;

```
flutter build appbundle
```

Il suffit ensuite de créer une release et d'y déposer le fichier en .aab

![Interface du Google Play Console](<../../.gitbook/assets/Capture d’écran 2022-06-01 à 13.12.54.png>)

A noter que le nombre à coté de la version (le 11 dans 11(1.0.0) par exemple) représente le numéro de build et doit être incrémenté manuellement depuis le pubspec.yaml du projet.
