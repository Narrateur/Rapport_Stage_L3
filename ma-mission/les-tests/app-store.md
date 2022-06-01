# App Store

L'App Store Connect permet de tester et déployer les applications Apple.

Pour sortir une application sur le Play Store, il suffit de suivre la documentation fournie par Flutter sur ce sujet : [https://docs.flutter.dev/deployment/ios#upload-the-app-bundle-to-app-store-connect](https://docs.flutter.dev/deployment/ios#upload-the-app-bundle-to-app-store-connect)

Contrairement au build pour Android, l'obfuscation et la minification du code doivent être précisé lors de l'exécution de la commande de création de l'App Bundle :

```
flutter build ipa --obfuscate --split-debug-info=build/app/outputs/symbols
```

L'application se trouve d'abord en tests interne. Ainsi seul les personnes ajouté au groupe de testeurs pourront télécharger l'application.
