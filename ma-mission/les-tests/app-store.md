# App Store

L'App Store Connect permet de tester et déployer les applications Apple.

Pour tester et déployer une application sur le Play Store, il suffit de suivre la documentation fournie par Flutter sur ce sujet : [https://docs.flutter.dev/deployment/ios#upload-the-app-bundle-to-app-store-connect](https://docs.flutter.dev/deployment/ios#upload-the-app-bundle-to-app-store-connect)

L'application se trouve d'abord en tests internes. Ainsi seul les personnes ajouté au groupe de testeurs pourront télécharger l'application.

Pour que le groupe de testeurs puisse avoir accès à l'application de test, il faut leurs fournir les build de celle-ci. Contrairement à Android, l'obfuscation et la minification du code doivent être précisés lors de l'exécution de la commande de création de l'App Bundle :

```
flutter build ipa --obfuscate --split-debug-info=build/app/outputs/symbols
```

Cette commande créera un fichier en .xcarchive, qu'il faudra ouvrir via Xcode.&#x20;

![](../../.gitbook/assets/interface\_xcode.png)

Une fois validé, on peut distribuer le build pour qu'il soit disponible sur l'App Store Connect.

![](../../.gitbook/assets/interface\_app\_store\_connect.png)

Après avoir ajouté le groupe de testeurs au build, ils pourront installer ou mettre à jour l'application via TestFlight sur leurs iPhone respectifs.

![](../../.gitbook/assets/interface\_app\_testflight.PNG) ![](../../.gitbook/assets/iPhone\_app.PNG)

A noter que le nombre entre parenthèse (le 10 dans 1.0.0 (10) par exemple) représente le numéro de build et doit être incrémenté manuellement depuis le pubspec.yaml du projet.
