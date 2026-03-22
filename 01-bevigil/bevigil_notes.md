# Notes d'analyse BeVigil - InsecureBankv2

## Outil
- Source: bevigil.com (CloudSEK)
- Application: InsecureBankv2 (com.android.insecurebankv2)
- Version: 1.0
- Security Rating: 7.4/10 (Average)
- Total issues detectees: 10

## Ce qui est certain

### VULNERABILITES (10 detectees)
- Weak Crypto Algorithms (CWE-327) - LOW - 4 fichiers
- Non-parameterized SQL Query (CWE-89) - LOW - 1 fichier
- Possible Object Deserialization (CWE-502) - LOW - 1 fichier
- Insecure HTTP Client Used (CWE-757) - LOW - 4 fichiers
- CBC Padding Oracle Attack Possible (NA) - LOW - 3 fichiers
- Insecure Random Used (NA) - LOW - 4 fichiers
- Use of SafetyNet API for device integrity check (NA) - LOW - 1 fichier
- Sensitive Information in Logs (CWE-532) - LOW - 2 fichiers
- Storage of sensitive info in Shared Preferences (CWE-312) - LOW - 2 fichiers
- Check for rooted device by app (NA) - LOW - 1 fichier

### STRINGS
- Possible Secret Detected - LOW
- Localisation: resources/res/values/strings.xml - 1 fichier

### MANIFEST SCANNER
- Exported Activity (CWE-926) - LOW
- Localisation: resources/AndroidManifest.xml - 4 fichiers concernes

### ASSETS
- URLs exposees (CWE-200) - LOW - 61 fichiers concernes
- File paths exposes (CWE-200) - LOW - 52 fichiers concernes
- Hostnames exposes (CWE-200) - LOW - 23 fichiers concernes
- REST API endpoints exposes (CWE-200) - LOW - 2 fichiers concernes
- Email addresses detectees (CWE-200) - LOW - 6 fichiers concernes

### APKID
- Fichier analyse: classes.dex
- Anti-VM checks: Build.MODEL check + 2 autres techniques
- Compiler: dx (possible dexmerge)
- Manipulator: dexmerge

### MALWARE
- Aucun malware detecte - application propre

## Ce qui est hypothese
- Le secret detecte dans strings.xml pourrait etre une cle API active encore valide
- Les REST API endpoints exposes pourraient ne pas avoir d authentification
- Les activites exportees pourraient etre accessibles sans permission depuis apps tierces
- Les checks Anti-VM suggerent une tentative de detection d environnement d analyse
- Les donnees dans Shared Preferences pourraient contenir credentials utilisateur en clair

## Points d interet
- Storage in Shared Preferences (CWE-312) - donnees sensibles stockees sans chiffrement
- Sensitive Information in Logs (CWE-532) - credentials potentiellement visibles dans logs
- Secret detecte dans strings.xml - a investiguer en priorite avec Yaazhini
- 61 URLs exposees - volume important revelant architecture interne
- Insecure HTTP Client - communications non chiffrees possibles
- CBC Padding Oracle - algorithme de chiffrement vulnerable

## Domaines et sous-domaines
- Hostnames detectes dans 23 fichiers - details a confirmer via Yaazhini
- Probablement serveur local de test (app bancaire pedagogique)

## Endpoints et APIs
- REST API endpoints detectes dans 2 fichiers
- Pattern probable: /api/ ou /rest/ - a confirmer via Yaazhini

## Emails et identifiants
- Adresses email detectees dans 6 fichiers
- Probablement emails de developpeurs ou comptes de test

## Technologies detectees
- Compilateur: dx avec dexmerge
- HTTP Client: DefaultHTTPClient (obsolete et non securise)
- Algorithmes crypto faibles (CWE-327) - probablement MD5 ou SHA1
- SafetyNet API utilisee pour verification integrite appareil
- Shared Preferences pour stockage (non chiffre)
