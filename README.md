# LAB-6-Analyse-statique-d-un-APK-avec-MobSF-dans-la-VM-Mobexler

## 1-Informations Générales

Application : DivaApplication.apk

Taille de l'apk : 1.5M

Analyste : saad makkouch

Date : 21 avril 2026

Hash SHA-256 : 5cefc51fce9bd760b92ab2340477f4dda84b4ae0c5d04a8c9493e4fe34fab7c5

Outil utilisé : MobSF (Mobile Security Framework) v4.0.6

## 2-Résumé exécutif

L’analyse de l’application a révélé plusieurs vulnérabilités de sécurité, notamment des failles liées à la validation des entrées et à la protection des données sensibles. Ces faiblesses peuvent être exploitées pour compromettre la confidentialité et l’intégrité de l’application. Le niveau de risque global est considéré comme élevé. Des mesures correctives sont nécessaires pour renforcer la sécurité et limiter les risques d’exploitation.

## Vulnérabilités critiques
1. problèmes de configuration ou de conception des permissions
   - **Sévérité :** Critique
   - **Description :** met en évidence des permissions sensibles pouvant entraîner des vulnérabilités liées au stockage des données. L’accès en lecture et écriture au stockage externe expose l’application à des risques de fuite de données sensibles et de manipulation non autorisée des fichiers. Ces faiblesses peuvent être exploitées par des applications malveillantes présentes sur le même appareil.
   - **Preuve :** analyse mobsf, l'image ci-dessous
   - **Impact :** Exposition des données sensibles à des accès non autorisés et risque de modification ou suppression des fichiers par des applications malveillantes.
   - **Remédiation :** Limiter ou supprimer les permissions de stockage externe, utiliser le stockage interne privé de l’application et appliquer le principe du moindre privilège pour n’accorder que les accès strictement nécessaires.
  
<img width="1622" height="527" alt="image" src="https://github.com/user-attachments/assets/60c9714d-bb6a-4a9f-a2fc-77ddf10c1a94" />

2. Mauvaise configuration du certificat de signature
    - **Sévérité :** Critique
    - **Description :** L’analyse révèle que l’application est signée avec un certificat de debug et utilise un schéma de signature v1 uniquement, la rendant vulnérable à la faille Janus. Cela permet potentiellement la modification du code de l’application sans invalider sa signature.
    - **Preuve :** Analyse MobSF (Certificate Analysis), voir image ci-dessus
    - **Impact :** Risque de modification malveillante de l’application (injection de code) sans détection, compromettant l’intégrité et la sécurité de l’application.
    - **Remédiation :** Signer l’application avec un certificat de production sécurisé, utiliser les schémas de signature modernes (v2/v3), et éviter l’utilisation de certificats de debug en environnement de production.

<img width="1824" height="522" alt="image" src="https://github.com/user-attachments/assets/d2221274-65ff-4ba4-9b34-5919d870fd61" />

3. Mauvaise configuration du fichier AndroidManifest
    - **Sévérité :** Critique
    - **Description :** L’analyse révèle plusieurs mauvaises configurations dans le fichier AndroidManifest, notamment l’activation du mode debug, l’installation sur des versions Android vulnérables, l’activation de la sauvegarde des données et l’exposition de composants (Activities, Content Provider) sans protection. Ces configurations augmentent la surface d’attaque de l’application.
    - **Preuve :** Analyse MobSF (Manifest Analysis), voir image ci-dessus
    - **Impact :** Accès non autorisé aux composants de l’application, fuite de données sensibles, exploitation facilitée par des attaquants via le débogage ou des applications malveillantes.
    - **Remédiation :** Désactiver le mode debug en production, restreindre les versions Android supportées, désactiver la sauvegarde des données si non nécessaire, et protéger les composants exposés via des permissions ou en les rendant non exportés.

<img width="1810" height="723" alt="image" src="https://github.com/user-attachments/assets/4e30bf07-f8a1-4aad-b0ae-08e919313e61" />

4. Vulnérabilités dans le code source de l’application
    - **Sévérité :** Critique
    - **Description :** L’analyse du code révèle plusieurs mauvaises pratiques de sécurité, notamment l’activation du mode debug, l’utilisation de requêtes SQL non sécurisées exposant à des injections SQL, le stockage de données sensibles dans des fichiers temporaires ou en stockage externe, ainsi que la journalisation d’informations sensibles. Ces failles augmentent significativement les risques de compromission de l’application.
    - **Preuve :** Analyse MobSF (Code Analysis), voir image ci-dessus
    - **Impact :** Fuite de données sensibles, exécution de requêtes malveillantes (SQL Injection), accès non autorisé aux informations et compromission globale de l’application.
    - **Remédiation :** Désactiver le mode debug en production, utiliser des requêtes SQL paramétrées, éviter de stocker des données sensibles en clair, chiffrer les données sensibles et supprimer toute journalisation d’informations critiques.

<img width="1823" height="620" alt="image" src="https://github.com/user-attachments/assets/e94b77a4-4286-406a-9629-c01d22857ecb" />



