# Lab Mobile Security - Analyse de InsecureBankv2

> **Analyste**: Lahrouf Hiba | **Date**: 22 Mars 2026 | **Cadre**: Pédagogique - Analyse défensive

---

## Table des matières
1. [Introduction](#introduction)
2. [Prérequis et installation](#prerequis)
3. [Préparation du workspace](#workspace)
4. [Analyse BeVigil](#bevigil)
5. [Analyse Yaazhini](#yaazhini)
6. [Triage et normalisation](#triage)
7. [Mapping OWASP](#owasp)
8. [Rapport final](#rapport)
9. [Clôture](#cloture)
10. [Résultats](#resultats)

---

## Introduction

Ce lab consiste en un audit défensif complet d'une application Android volontairement vulnérable : **InsecureBankv2**. L'objectif est d'identifier les vulnérabilités, mauvaises configurations et expositions potentielles en utilisant deux outils d'analyse statique : **BeVigil** et **Yaazhini**.

> ⚠️ **Cadre légal** : Analyse réalisée uniquement sur une application pédagogique open source. Aucune exploitation réelle. Aucun test intrusif.

### Cible

| Champ | Valeur |
|---|---|
| Application | InsecureBankv2 |
| Package | com.android.insecurebankv2 |
| Version | 1.0 |
| Source | https://github.com/dineshshetty/Android-InsecureBankv2 |
| Hash SHA-256 | B18AF2A0E44D7634BBCDF93664D9C78A2695E050393FCFBB5E8B91F902D194A4 |
| Min SDK | 15 (Android 4.0.3) |
| Target SDK | 22 (Android 5.1) |
| Taille | 3.3 MB |

---

## Prérequis et installation

### Outils nécessaires

| Outil | Usage | Lien |
|---|---|---|
| BeVigil (CloudSEK) | Analyse externe en ligne | bevigil.com |
| Yaazhini (VegaBird) | Analyse statique APK Windows | vegabird.com/yaazhini |
| PowerShell | Commandes et traçabilité | Inclus dans Windows |

### Téléchargement de l'APK

1. Aller sur https://github.com/dineshshetty/Android-InsecureBankv2
2. Cliquer sur InsecureBankv2.apk
3. Cliquer sur Download raw file

![GitHub repo InsecureBankv2](screenshots/1.png)
*Page GitHub du repo - Application volontairement vulnérable avec 1.4k stars*

![Téléchargement APK](screenshots/2.png)
*Page de téléchargement du fichier APK (3.3 MB)*

![Download terminé](screenshots/3.png)
*Confirmation du téléchargement de l'APK*

### Installation de Yaazhini

1. Aller sur vegabird.com/yaazhini
2. Télécharger la version gratuite (entrer son email)
3. Recevoir le lien par email et télécharger le .exe
4. Double-cliquer sur le .exe et suivre l'installation

![Email Yaazhini](screenshots/24.png)
*Email reçu - Yaazhini Free Version 2.0.2 - Téléchargement gratuit*

![Installation Yaazhini](screenshots/25.png)
*Installation de Yaazhini en cours*

---

## Workspace

### Étape 1 - Créer le dossier principal
```powershell
cd C:\Users\pc\OneDrive\Desktop
mkdir lab-mobile-security
cd lab-mobile-security
```

![Création dossier principal](screenshots/4.png)
*Création du dossier lab-mobile-security sur le Bureau*

### Étape 2 - Créer l'arborescence
```powershell
mkdir 00-scope, 01-bevigil, 02-yaazhini, 03-triage, 04-report, screenshots
```

![Création arborescence](screenshots/5.png)
*Création de tous les sous-dossiers du lab*

### Étape 3 - Vérifier la structure
```powershell
tree
```

![Structure tree](screenshots/6.png)
*Vérification de la structure avec la commande tree*

### Étape 4 - Copier l'APK et calculer le hash SHA-256
```powershell
Copy-Item -Path "C:\Users\pc\Downloads\InsecureBankv2.apk" -Destination "00-scope\InsecureBankv2.apk"
$hash = Get-FileHash -Path "00-scope\InsecureBankv2.apk" -Algorithm SHA256
Write-Host "Hash SHA-256 : $($hash.Hash)"
```

![Copie APK et hash](screenshots/7.png)
*Hash SHA-256 : B18AF2A0E44D7634BBCDF93664D9C78A2695E050393FCFBB5E8B91F902D194A4*

### Étape 5 - Créer les fichiers de traçabilité

![Création scope.md](screenshots/8.png)
*Création du fichier scope.md définissant le périmètre d'analyse*

![Contenu analyse_info.txt](screenshots/9.png)
*Fichier analyse_info.txt avec toutes les informations de traçabilité*

![Création commands.log](screenshots/10.png)
*Initialisation du fichier commands.log*

### Étape 6 - Vérification finale
```powershell
tree /F
```

![Tree /F final](screenshots/11.png)
*Structure complète avec tous les fichiers créés*
---

## BeVigil

**BeVigil** est un outil en ligne de CloudSEK qui analyse les applications mobiles pour identifier les assets exposés, endpoints, vulnérabilités et technologies.

### Étape 1 - Accéder à BeVigil

1. Aller sur bevigil.com
2. Créer un compte avec son email
3. Se connecter

![Page d'accueil BeVigil](screenshots/12.png)
*Interface de recherche BeVigil - Analyse de plus d'1 million d'applications*

### Étape 2 - Scanner l'APK

1. Cliquer sur Scan App dans le menu
2. Choisir Option 2 : Scan .apk file
3. Uploader InsecureBankv2.apk
4. Attendre la fin du scan

![Upload APK BeVigil](screenshots/20.png)
*Upload de l'APK en cours - 30% - 112KB/s*

![Scan initié](screenshots/21.png)
*Profil Hiba Lahrouf - Scan initié et meta-data extraction complète*

### Étape 3 - Explorer les résultats

#### Dashboard - Vue d'ensemble

![BeVigil Dashboard](screenshots/13.png)
*Security Rating 7.4/10 - 5 issues détectées - Répartition High/Med/Low*

#### Vulnerabilities - Page 1

![BeVigil Vulnerabilities 1](screenshots/14.png)
*10 vulnérabilités : Weak Crypto CWE-327, SQL Injection CWE-89, HTTP Client CWE-757, CBC Padding, Object Deserialization*

#### Vulnerabilities - Page 2

![BeVigil Vulnerabilities 2](screenshots/22.png)
*Suite : Insecure Random, SafetyNet API, Sensitive Logs CWE-532, Shared Preferences CWE-312, Root Detection*

#### Strings - Secrets détectés

![BeVigil Strings](screenshots/15.png)
*Possible Secret Detected (LOW) dans resources/res/values/strings.xml*

#### Manifest Scanner

![BeVigil Manifest](screenshots/16.png)
*Exported Activity CWE-926 (LOW) - 4 fichiers concernés dans AndroidManifest.xml*

#### Assets

![BeVigil Assets](screenshots/17.png)
*URLs (61 fichiers), File paths (52), Hostnames (23), REST API (2), Emails (6) - tous CWE-200*

#### APKiD

![BeVigil APKiD](screenshots/18.png)
*Anti-VM checks : Build.MODEL check + 2 autres - Compiler dx/dexmerge*

#### Malware

![BeVigil Malware](screenshots/19.png)
*Aucun malware détecté - Application propre ✅*

### Étape 4 - Documenter les résultats

![Création bevigil_notes.md](screenshots/23.png)
*Création du fichier bevigil_notes.md avec tous les résultats documentés*

---

## Yaazhini

**Yaazhini** est un outil Windows de VegaBird Technologies qui décompile et analyse les APK Android pour identifier les vulnérabilités internes, permissions, activités et configurations.

### Étape 1 - Lancer Yaazhini

![Interface Yaazhini vide](screenshots/26.png)
*Interface principale - APK Scanner à gauche, API Scanner à droite*

### Étape 2 - Configurer et lancer le scan

1. Entrer InsecureBankv2 dans Project / App Name
2. Cliquer sur Choose file et sélectionner l'APK dans 00-scope
3. Cliquer sur Upload & Scan

![Yaazhini configuré](screenshots/27.png)
*Yaazhini configuré avec le nom du projet et l'APK sélectionné*

![Yaazhini Progress Bar](screenshots/28.png)
*Scan en cours : APK vérifié, décompilation démarrée*

### Étape 3 - Résultats du scan

#### Home - App Summary

![Yaazhini Home](screenshots/29.png)
*Résumé : Version 1.0, MinSDK 15, TargetSDK 22, 3.3 MB - Scan le 22-MAR-2026*

#### Vulnerabilities - Vue d'ensemble

![Yaazhini Vulnerabilities](screenshots/30.png)
*84 vulnérabilités : High (36), Medium (13), Low (3), Warning (13), Information (19)*

#### Vulnerabilities - Détail High

![Yaazhini High Vulns 1](screenshots/38.png)
*High (36) : Insecure Communication CVSS 8.1 - ChangePassword.java, DoLogin.java, DoTransfer.java*

![Yaazhini High Vulns 2](screenshots/39.png)
*Suite High : Tracker.java, Action.java(16) - HTTP non chiffré dans tout le code*

#### View Code - AndroidManifest

![Yaazhini AndroidManifest](screenshots/31.png)
*AndroidManifest.xml : ViewStatement exported=true, TrackUserContentProvider exported=true, MyBroadCastReceiver exported=true*

#### Linked URLs

![Yaazhini URLs](screenshots/32.png)
*URLs détectées : schemas.android.com, goo.gl, google-analytics.com, schema.org*

#### Libraries

![Yaazhini Libraries](screenshots/33.png)
*5 librairies : android.support.v4, v7 (obsolètes), com.google.ads, com.google.android*

#### Permissions

![Yaazhini Permissions](screenshots/34.png)
*12 permissions dont SEND_SMS, READ_CONTACTS, READ_CALL_LOG, USE_CREDENTIALS, ACCESS_COARSE_LOCATION*

#### Activities

![Yaazhini Activities](screenshots/35.png)
*10 activités : LoginActivity, DoLogin, DoTransfer, ChangePassword, ViewStatement, WrongLogin*

#### Receivers

![Yaazhini Receivers](screenshots/36.png)
*MyBroadCastReceiver exported=TRUE ⚠️ - EnableWalletOptimizationReceiver exported=false*

#### Services

![Yaazhini Services](screenshots/37.png)
*Aucun service détecté*

### Étape 4 - Documenter les résultats

![Création yaazhini_notes.md](screenshots/40.png)
*Création du fichier yaazhini_notes.md avec tous les éléments documentés*
---

## Triage

La normalisation consiste à consolider les résultats des deux outils, éliminer les doublons et classer chaque constat par sévérité.

### Résultats consolidés - 15 constats

| ID | Source | Élément | Sévérité | OWASP |
|---|---|---|---|---|
| FIND-001 | Yaazhini | Insecure Communication HTTP | **HIGH** | MASVS-NETWORK-1 |
| FIND-002 | BeVigil+Yaazhini | Insecure HTTP Client | **HIGH** | MASVS-NETWORK-1 |
| FIND-003 | Yaazhini | MyBroadCastReceiver exporté | **HIGH** | MASVS-PLATFORM-1 |
| FIND-004 | Yaazhini | TrackUserContentProvider exporté | **HIGH** | MASVS-PLATFORM-2 |
| FIND-005 | BeVigil+Yaazhini | Storage Shared Preferences | MEDIUM | MASVS-STORAGE-1 |
| FIND-006 | BeVigil+Yaazhini | Sensitive Info in Logs | MEDIUM | MASVS-STORAGE-3 |
| FIND-007 | Yaazhini | Permissions excessives (10/12) | MEDIUM | MASVS-PLATFORM-1 |
| FIND-008 | BeVigil | Possible Secret Detected | MEDIUM | MASVS-STORAGE-2 |
| FIND-009 | Yaazhini | Activities sensibles exposées | MEDIUM | MASVS-PLATFORM-1 |
| FIND-010 | BeVigil | Weak Crypto Algorithms | MEDIUM | MASVS-CRYPTO-1 |
| FIND-011 | Yaazhini | SDK version obsolète | MEDIUM | MASVS-CODE-1 |
| FIND-012 | Yaazhini | Librairies obsolètes | LOW | MASVS-CODE-3 |
| FIND-013 | BeVigil | CBC Padding Oracle Attack | MEDIUM | MASVS-CRYPTO-1 |
| FIND-014 | BeVigil | Non-parameterized SQL Query | MEDIUM | MASVS-CODE-4 |
| FIND-015 | Yaazhini | URLs HTTP en clair | LOW | MASVS-NETWORK-2 |

![Création triage.csv](screenshots/41.png)
*Création du fichier triage.csv avec les 15 constats*

![triage.csv dans Excel](screenshots/42.png)
*Vue du triage.csv dans Excel - 15 findings avec source, sévérité et référence OWASP*

---

## OWASP

Corrélation de chaque constat avec le standard **OWASP MASVS** (Mobile Application Security Verification Standard).

### Répartition par catégorie

| Catégorie | Findings | Sévérité max |
|---|---|---|
| MASVS-NETWORK | FIND-001, 002, 015 | **HIGH** |
| MASVS-PLATFORM | FIND-003, 004, 007, 009 | **HIGH** |
| MASVS-STORAGE | FIND-005, 006, 008 | MEDIUM |
| MASVS-CRYPTO | FIND-010, 013 | MEDIUM |
| MASVS-CODE | FIND-011, 012, 014 | MEDIUM |

![Création owasp_mapping.md](screenshots/43.png)
*Création du fichier owasp_mapping.md avec les justifications pour chaque finding*

---

## Rapport

### Top 5 Constats Critiques

#### 1. Insecure Communication HTTP - FIND-001 🔴 HIGH
| Champ | Détail |
|---|---|
| CVSS | 8.1 |
| Preuve | 36 occurrences dans DoLogin.java, DoTransfer.java, ChangePassword.java |
| Impact | Interception des credentials et transactions bancaires via MITM |
| Remédiation | Implémenter SSL/TLS pour toutes les connexions réseau |
| OWASP | MASVS-NETWORK-1 |

#### 2. MyBroadCastReceiver Exporté - FIND-003 🔴 HIGH
| Champ | Détail |
|---|---|
| Preuve | AndroidManifest.xml - exported=true |
| Impact | App tierce peut déclencher des actions non autorisées |
| Remédiation | Définir exported=false ou ajouter une permission custom |
| OWASP | MASVS-PLATFORM-1 |

#### 3. TrackUserContentProvider Exporté - FIND-004 🔴 HIGH
| Champ | Détail |
|---|---|
| Preuve | AndroidManifest.xml - exported=true |
| Impact | Accès aux données utilisateur depuis apps tierces sans permission |
| Remédiation | Définir exported=false sur le ContentProvider |
| OWASP | MASVS-PLATFORM-2 |

#### 4. Stockage Données Sensibles en Clair - FIND-005 🟡 MEDIUM
| Champ | Détail |
|---|---|
| Preuve | BeVigil CWE-312 + Yaazhini SharedPreferences non chiffrées |
| Impact | Credentials lisibles sur appareil rooté ou via ADB |
| Remédiation | Utiliser EncryptedSharedPreferences ou Android Keystore |
| OWASP | MASVS-STORAGE-1 |

#### 5. Permissions Excessives - FIND-007 🟡 MEDIUM
| Champ | Détail |
|---|---|
| Preuve | 10 permissions sensibles sur 12 dont SEND_SMS, READ_CONTACTS |
| Impact | Surface d'attaque élargie - données personnelles accessibles |
| Remédiation | Supprimer toutes les permissions non justifiées |
| OWASP | MASVS-PLATFORM-1 |

![Création rapport_final.md](screenshots/44.png)
*Création du rapport final avec résumé exécutif, top 5 constats et recommandations*

### Recommandations Prioritaires

1. 🚨 **URGENT** : Remplacer HTTP par HTTPS dans DoLogin.java, DoTransfer.java, ChangePassword.java
2. 🚨 **URGENT** : Définir exported=false sur MyBroadCastReceiver et TrackUserContentProvider
3. ⚠️ **IMPORTANT** : Migrer vers EncryptedSharedPreferences + supprimer les 10 permissions non justifiées

---

## Clôture

![Création checklist_fin.md](screenshots/45.png)
*Checklist de clôture complétée et signée - Lahrouf Hiba - 22 Mars 2026*

### Structure finale du projet
lab-mobile-security/
├── 00-scope/
│   ├── scope.md
│   └── InsecureBankv2.apk
├── 01-bevigil/
│   └── bevigil_notes.md
├── 02-yaazhini/
│   └── yaazhini_notes.md
├── 03-triage/
│   ├── triage.csv
│   └── owasp_mapping.md
├── 04-report/
│   └── rapport_final.md
├── screenshots/
│   └── [46 screenshots]
├── analyse_info.txt
├── commands.log
├── checklist_fin.md
└── README.md
![Tree final](screenshots/46.png)
*Structure finale complète - tous les fichiers créés et organisés*

---

## Résultats

### Résumé BeVigil
- Security Rating : **7.4/10 (Average)**
- Total issues : **10**
- Aucun malware détecté ✅

### Résumé Yaazhini
- Total vulnérabilités : **84**
- High : **36** | Medium : **13** | Low : **3** | Warning : **13** | Info : **19**

### Résumé global
- Total constats normalisés : **15**
- High : **4** | Medium : **8** | Low : **3**
- Niveau de risque global : 🔴 **ÉLEVÉ**

### Faux Positifs
- URLs schemas.android.com : namespace XML standard Android ✅
- EnableWalletOptimizationReceiver : composant Google exported=false ✅
- Chaîne password dans strings.xml : label UI uniquement ✅

---

**Lahrouf Hiba - 22 Mars 2026**
*Lab réalisé dans un cadre pédagogique - Analyse défensive uniquement*