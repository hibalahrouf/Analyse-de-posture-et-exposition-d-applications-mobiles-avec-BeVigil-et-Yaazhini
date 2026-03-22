# Mapping OWASP - InsecureBankv2

## Reference: OWASP MASVS (Mobile Application Security Verification Standard)
## Source: https://github.com/OWASP/owasp-masvs

---

## FIND-001 et FIND-002: Insecure Communication HTTP
- **Categorie OWASP**: MASVS-NETWORK
- **Reference specifique**: MASVS-NETWORK-1
- **Justification**: Toutes les communications reseau doivent etre chiffrees avec TLS.
  LInsecureBankv2 utilise HTTP en clair dans 36 fichiers dont ChangePassword.java
  et DoTransfer.java - donnees bancaires transitent sans chiffrement.

## FIND-003 et FIND-004: Composants exportes sans protection
- **Categorie OWASP**: MASVS-PLATFORM
- **Reference specifique**: MASVS-PLATFORM-1 et MASVS-PLATFORM-2
- **Justification**: Les composants Android exportes (Activities, Receivers, Providers)
  doivent etre proteges par des permissions. MyBroadCastReceiver et
  TrackUserContentProvider sont accessibles depuis nimporte quelle app tierce.

## FIND-005: Storage sensitive info Shared Preferences
- **Categorie OWASP**: MASVS-STORAGE
- **Reference specifique**: MASVS-STORAGE-1
- **Justification**: Les donnees sensibles ne doivent jamais etre stockees en clair.
  Les SharedPreferences non chiffrees peuvent etre lues sur appareil roote
  ou via backup ADB.

## FIND-006: Sensitive Information in Logs
- **Categorie OWASP**: MASVS-STORAGE
- **Reference specifique**: MASVS-STORAGE-3
- **Justification**: Les logs applicatifs ne doivent pas contenir dinformations sensibles.
  Les credentials visibles dans logcat sont accessibles via ADB sans root
  sur Android < 4.1.

## FIND-007: Permissions excessives
- **Categorie OWASP**: MASVS-PLATFORM
- **Reference specifique**: MASVS-PLATFORM-1
- **Justification**: Une application doit demander uniquement les permissions
  strictement necessaires. SEND_SMS, READ_CONTACTS et READ_CALL_LOG
  nont aucune justification pour une app bancaire.

## FIND-008: Possible Secret Detected
- **Categorie OWASP**: MASVS-STORAGE
- **Reference specifique**: MASVS-STORAGE-2
- **Justification**: Les secrets (cles API, tokens, passwords) ne doivent jamais
  etre hardcodes dans le code source ou les ressources. Ils doivent etre
  stockes dans Android Keystore.

## FIND-009: Activities sensibles exposees
- **Categorie OWASP**: MASVS-PLATFORM
- **Reference specifique**: MASVS-PLATFORM-1
- **Justification**: Les activites gerant lauthentification et les transactions
  (DoLogin, DoTransfer, ChangePassword) doivent verifier lidentite
  de lappelant avant execution.

## FIND-010: Weak Crypto Algorithms
- **Categorie OWASP**: MASVS-CRYPTO
- **Reference specifique**: MASVS-CRYPTO-1
- **Justification**: Les algorithmes cryptographiques faibles (MD5, SHA1, DES)
  ne doivent pas etre utilises pour proteger des donnees sensibles.
  Ils sont susceptibles aux attaques par collision et brute force.

## FIND-011: SDK version obsolete
- **Categorie OWASP**: MASVS-CODE
- **Reference specifique**: MASVS-CODE-1
- **Justification**: Cibler des versions Android obsoletes (SDK 22 = Android 5.1)
  expose lapp a des vulnerabilites systeme non corrigees et empeche
  lutilisation des APIs de securite modernes.

## FIND-012: Librairies tierces obsoletes
- **Categorie OWASP**: MASVS-CODE
- **Reference specifique**: MASVS-CODE-3
- **Justification**: Les librairies tierces doivent etre maintenues a jour.
  android.support.v4 et v7 sont deprecated depuis 2018 et ne recoivent
  plus de patches de securite.

## FIND-013: CBC Padding Oracle Attack
- **Categorie OWASP**: MASVS-CRYPTO
- **Reference specifique**: MASVS-CRYPTO-1
- **Justification**: Le mode CBC sans verification integrite permet des attaques
  par oracle de padding. Migrer vers GCM qui inclut authentification
  et chiffrement simultanement.

## FIND-014: Non-parameterized SQL Query
- **Categorie OWASP**: MASVS-CODE
- **Reference specifique**: MASVS-CODE-4
- **Justification**: Les requetes SQL construites par concatenation de chaines
  sont vulnerables a linjection SQL. Utiliser des requetes parametrees
  ou un ORM pour prevenir ce risque.

## FIND-015: URLs HTTP en clair
- **Categorie OWASP**: MASVS-NETWORK
- **Reference specifique**: MASVS-NETWORK-2
- **Justification**: Toutes les URLs codees en dur doivent utiliser HTTPS.
  Les URLs HTTP exposent les communications au risque dinterception
  et de manipulation par un attaquant en position MITM.

---

## Resume par categorie OWASP

| Categorie | Nombre de findings | Severite max |
|---|---|---|
| MASVS-NETWORK | 3 (FIND-001, 002, 015) | High |
| MASVS-PLATFORM | 3 (FIND-003, 004, 007, 009) | High |
| MASVS-STORAGE | 3 (FIND-005, 006, 008) | Medium |
| MASVS-CRYPTO | 2 (FIND-010, 013) | Medium |
| MASVS-CODE | 3 (FIND-011, 012, 014) | Medium |
