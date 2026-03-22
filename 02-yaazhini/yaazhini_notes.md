# Notes d'analyse Yaazhini - InsecureBankv2

## Informations generales
- Outil: Yaazhini by VegaBird Technologies
- Date de scan: 22-MAR-2026, 9:27 PM
- App Name: InsecureBankv2
- Package: com.android.insecurebankv2
- Version: 1.0
- Android Min SDK: 15 (tres ancien - vulnerable)
- Android Target SDK: 22 (obsolete)
- App Size: 3.3 MB

## Elements identifies

### Element 1: Insecure Communication (HTTP)
- **Localisation**: High (36 occurrences) - ChangePassword.java, DoLogin.java, DoTransfer.java, AnalyticsReceiver.java, CampaignTrackingReceiver.java, Action.java(16), et autres
- **Description**: URLs HTTP detectees en clair dans le code source. Le trafic reseau nest pas chiffre.
- **CVSS**: 8.1
- **Impact potentiel**: Interception des donnees sensibles (credentials, transactions bancaires) via attaque MITM
- **Remediation**: Utiliser SSL/TLS pour toutes les connexions transmettant des donnees sensibles

### Element 2: Receiver exporte sans protection
- **Localisation**: AndroidManifest.xml - com.android.insecurebankv2.MyBroadCastReceiver (exported=true)
- **Description**: Un BroadcastReceiver est accessible depuis des applications tierces sans permission requise
- **Impact potentiel**: Une application malveillante peut envoyer des broadcasts et declencher des actions non autorisees
- **Remediation**: Definir exported=false ou ajouter une permission custom pour proteger le receiver

### Element 3: Permissions excessives
- **Localisation**: AndroidManifest.xml - 12 permissions declarees
- **Description**: Permissions sensibles non justifiees pour une app bancaire:
  * SEND_SMS - envoi de SMS sans confirmation
  * READ_CONTACTS - acces aux contacts
  * READ_CALL_LOG - acces aux journaux dappels
  * USE_CREDENTIALS - acces aux tokens dauthentification
  * GET_ACCOUNTS - acces a la liste des comptes
  * READ_PROFILE - acces au profil personnel
  * ACCESS_COARSE_LOCATION - acces a la localisation
  * WRITE_EXTERNAL_STORAGE - ecriture sur SD card
  * READ_EXTERNAL_STORAGE - lecture sur SD card
  * READ_PHONE_STATE - acces a letat du telephone
- **Impact potentiel**: Surface dattaque elargie, acces a des donnees personnelles non necessaires
- **Remediation**: Supprimer les permissions non necessaires au fonctionnement de lapp

### Element 4: Activities sensibles exposees
- **Localisation**: Activities tab - 10 activites detectees
- **Description**: Activites critiques identifiees dans le code:
  * LoginActivity - ecran de connexion
  * DoLogin - traitement de lauthentification
  * DoTransfer - traitement des transferts bancaires
  * ChangePassword - modification du mot de passe
  * FilePrefActivity - gestion des preferences fichiers
  * ViewStatement - consultation des releves
  * WrongLogin - gestion des echecs de connexion
- **Impact potentiel**: Architecture interne de lapp completement visible, facilitant le reverse engineering
- **Remediation**: Minimiser les activites exportees, ajouter des controles dauthentification

### Element 5: Librairies tierces obsoletes
- **Localisation**: Libraries tab - 5 librairies detectees
- **Description**: Librairies utilisees:
  * android.support.annotation
  * android.support.v4 (obsolete - remplace par AndroidX)
  * android.support.v7 (obsolete - remplace par AndroidX)
  * com.google.ads
  * com.google.android
- **Impact potentiel**: Librairies support obsoletes peuvent contenir des vulnerabilites non patchees
- **Remediation**: Migrer vers AndroidX, mettre a jour toutes les dependances

### Element 6: URLs HTTP en clair (Linked URLs)
- **Localisation**: Linked URLs tab
- **Description**: URLs detectees dans le code:
  * http://schemas.android.com (3 URLs)
  * http://goo.gl (2 URLs raccourcies - serveur ESF)
  * http://www.google-analytics.com (tracking non chiffre)
  * https://ssl.google-analytics.com
  * http://schema.org (13+ URLs)
- **Impact potentiel**: URLs raccourcies (goo.gl) peuvent masquer des redirections malveillantes
- **Remediation**: Remplacer toutes les URLs HTTP par HTTPS

### Element 7: AndroidManifest - Composants exportes
- **Localisation**: View Code - AndroidManifest.xml
- **Description**: Plusieurs composants exportes sans protection:
  * ViewStatement activity: exported=true
  * TrackUserContentProvider: exported=true
  * MyBroadCastReceiver: exported=true
  * ChangePassword activity: exported=true
- **Impact potentiel**: Composants accessibles depuis apps tierces, bypass possible de lauthentification
- **Remediation**: Revoir tous les composants exported=true et ajouter des permissions

## Ce qui est certain
- 36 vulnerabilites High liees aux communications HTTP non chiffrees
- MyBroadCastReceiver expose publiquement (exported=true)
- 10 permissions sensibles sur 12 non justifiees pour une app bancaire
- SDK cible version 22 (Android 5.1) - tres obsolete

## Ce qui est hypothese
- Les credentials pourraient etre stockes en clair dans les SharedPreferences
- DoLogin et DoTransfer pourraient etre accessibles sans authentification prealable
- Les URLs goo.gl raccourcies pourraient pointer vers des serveurs de test non securises

## Points d interet
- CVSS 8.1 pour les communications non chiffrees - impact critique
- TrackUserContentProvider expose - possible fuite de donnees utilisateur
- Action.java contient 16 occurrences de communications non chiffrees
- Min SDK 15 = Android 4.0.3 - supporte des appareils extremement vulnerables
