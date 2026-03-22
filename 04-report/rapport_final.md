# Rapport d'analyse de securite mobile

## A. Informations generales
- **Date**: 2026-03-22
- **Analyste**: Lahrouf Hiba
- **Cible**: InsecureBankv2 (com.android.insecurebankv2)
- **Version**: 1.0
- **Hash SHA-256**: B18AF2A0E44D7634BBCDF93664D9C78A2695E050393FCFBB5E8B91F902D194A4
- **Outils utilises**: BeVigil (CloudSEK), Yaazhini (VegaBird Technologies)
- **Source APK**: https://github.com/dineshshetty/Android-InsecureBankv2

## B. Resume executif
L'analyse statique de l'application InsecureBankv2 a revele 15 constats de securite
dont 4 de severite High, 8 Medium et 3 Low. Les problemes les plus critiques concernent
36 occurrences de communications HTTP non chiffrees dans des fichiers sensibles comme
DoLogin.java et DoTransfer.java, ainsi que plusieurs composants Android exportes sans
protection permettant un acces non autorise depuis des applications tierces. Le niveau
de risque global est considere ELEVE en raison de l'impact direct sur la confidentialite
des donnees bancaires des utilisateurs.

## C. Top 5 constats

### 1. Insecure Communication HTTP - FIND-001
- **Severite**: High
- **CVSS**: 8.1
- **Preuve**: Yaazhini - 36 occurrences dans ChangePassword.java, DoLogin.java, DoTransfer.java
- **Impact**: Interception des credentials et transactions bancaires via attaque MITM
- **Remediation**: Implementer SSL/TLS pour toutes les connexions reseau
- **Reference OWASP**: MASVS-NETWORK-1

### 2. MyBroadCastReceiver exporte - FIND-003
- **Severite**: High
- **Preuve**: Yaazhini - AndroidManifest.xml exported=true sur MyBroadCastReceiver
- **Impact**: Application tierce peut declencher des actions non autorisees
- **Remediation**: Definir exported=false ou ajouter une permission custom
- **Reference OWASP**: MASVS-PLATFORM-1

### 3. TrackUserContentProvider exporte - FIND-004
- **Severite**: High
- **Preuve**: Yaazhini - AndroidManifest.xml exported=true sur TrackUserContentProvider
- **Impact**: Acces aux donnees utilisateur depuis apps tierces sans permission
- **Remediation**: Definir exported=false sur le ContentProvider
- **Reference OWASP**: MASVS-PLATFORM-2

### 4. Stockage donnees sensibles en clair - FIND-005
- **Severite**: Medium
- **Preuve**: BeVigil CWE-312 + Yaazhini - SharedPreferences non chiffrees
- **Impact**: Credentials et donnees bancaires lisibles sur appareil roote ou via ADB
- **Remediation**: Utiliser EncryptedSharedPreferences ou Android Keystore
- **Reference OWASP**: MASVS-STORAGE-1

### 5. Permissions excessives - FIND-007
- **Severite**: Medium
- **Preuve**: Yaazhini - 10 permissions sensibles dont SEND_SMS, READ_CONTACTS, READ_CALL_LOG
- **Impact**: Surface attaque elargie - acces a des donnees personnelles non necessaires
- **Remediation**: Supprimer toutes les permissions non justifiees par les fonctionnalites
- **Reference OWASP**: MASVS-PLATFORM-1

## D. Faux positifs notables
- URLs schemas.android.com: URLs de namespace XML Android standard - pas une vraie exposition
- com.google.android.gms.wallet.EnableWalletOptimizationReceiver: Composant Google Play Services - exported=false, pas un risque
- Libraries android.support: Obsoletes mais pas directement exploitables dans ce contexte
- URLs schema.org: Schemas de donnees semantiques standards - pas de risque direct

## E. Recommandations prioritaires
1. **URGENT** - Remplacer toutes les communications HTTP par HTTPS dans DoLogin.java,
   DoTransfer.java et ChangePassword.java pour proteger les donnees bancaires en transit
2. **URGENT** - Definir exported=false sur MyBroadCastReceiver et TrackUserContentProvider
   dans AndroidManifest.xml pour empecher tout acces non autorise depuis apps tierces
3. **IMPORTANT** - Migrer le stockage des donnees sensibles vers EncryptedSharedPreferences
   et supprimer les 10 permissions non justifiees pour reduire la surface d'attaque

## F. Annexes
- Notes BeVigil: 01-bevigil/bevigil_notes.md
- Notes Yaazhini: 02-yaazhini/yaazhini_notes.md
- Triage complet: 03-triage/triage.csv
- Mapping OWASP: 03-triage/owasp_mapping.md
- Screenshots: screenshots/
