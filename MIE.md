## Moyens d'identification électronique (MIE) et méthodes d'authentification Okta

Le projet **Pro Santé Connect (PSC) sans couture** permet d’envisager l’utilisation de différents **Moyens d’Identification Électronique (MIE)** pour offrir une expérience de navigation fluide et sécurisée.

### Prise en charge des MIE conformes aux exigences de l’ANS dans Okta

Les sections suivantes détaillent la prise en charge des MIE conformes aux exigences de l’**Agence du Numérique en Santé (ANS)** dans **Okta**.

---

### Utilisation des certificats X.509 de la carte CPx physique

Un professionnel de santé (PS) peut utiliser le certificat **X.509** de sa carte de professionnel de santé (**CPx**) pour s’authentifier sur son tenant Okta et accéder à des services numériques connectés à PSC, sans avoir à se réauthentifier.

Les éléments de configuration nécessaires pour les cartes **CPx physiques** sont précisés dans le [**Guide de configuration des cartes CPx**](https://industriels.esante.gouv.fr/sites/default/files/media/document/ANS_PUSC_PSCE_Guide_installation_et_utilisation_du_Minidriver_CPS_20181203_v2.1.0.pdf) publié par l’ANS.

---

### Utilisation d’une clé de sécurité FIDO2

Okta prend en charge l’utilisation d’une **clé de sécurité FIDO2** comme MIE conforme.
Cette méthode, basée sur le standard ouvert **Fast IDentity Online v2.0**, permet une authentification sans mot de passe à l’aide d’une clé physique ou de biométrie (empreinte digitale, reconnaissance faciale, etc.).
La fourniture des clés FIDO2 est à la charge de l’établissement de santé.

Les éléments de configuration sont décrits dans le [**Guide de configuration des clés de sécurité FIDO2**](https://help.okta.com/oie/en-us/content/topics/identity-engine/authenticators/configure-webauthn.htm).

---

### Utilisation d’Okta Verify (Push / TOTP)

L’application **Okta Verify** peut être utilisée dans ce contexte comme MIE 2FA conforme. Elle permet soit une authentification par **notification push**, soit par **code TOTP** généré localement sur l’appareil.
Cette approche combine sécurité forte et expérience utilisateur simplifiée, sans dépendance au mot de passe seul.

Les éléments de configuration sont couverts dans la documentation [**Okta Verify**](https://help.okta.com/eu/en-us/content/topics/end-user/ov-overview.htm.)

---

### Utilisation de Okta Device Access

**Okta Device Access** est une fonctionnalité d’Okta qui étend la gestion des identités jusqu’au **poste de travail**.
Concrètement, elle permet de remplacer ou compléter l’authentification native des systèmes (Windows, macOS) par les mécanismes d’authentification Okta.

Okta Device Access permet :

* de contrôler l’ouverture de session des **postes Windows et macOS** via les politiques de sécurité Okta ;
* d’appliquer de l’**authentification forte (MFA)** dès le login au poste de travail (ex. Okta Verify, FIDO2, biométrie) ;
* d’offrir une **expérience sans couture** : une seule authentification au poste peut donner accès à toutes les applications fédérées dans Okta ;
* d’assurer une **gouvernance centralisée** : règles, accès et logs sont gérés dans Okta plutôt que localement.

C’est donc une brique qui rapproche la sécurité des postes de travail du modèle **Zero Trust**, en alignant l’accès aux terminaux avec la même logique que celle appliquée aux applications et aux services.

Attention! L’intégration d’**Okta Device Access** ne permet d’utiliser **Windows Hello** (PIN ou biométrie) comme facteur d’authentification FIDO2.
La raison est que Windows Hello est un fournisseur d’informations d’identification supplémentaire et il est prévu qu’il ne soit pas invité à utiliser Okta Desktop MFA si le code PIN Windows Hello est utilisé. [Source](https://support.okta.com/help/s/article/okta-desktopmfa-is-not-compatible-with-windows-hello?language=en_US)
Cette approche renforce la sécurité des ouvertures de session Windows tout en simplifiant l’expérience des PS, notamment dans les environnements partagés ou mobiles.

[Lien démo Okta device Access](https://www.youtube.com/watch?v=tmbjFlI7rx4)

### Prise en charge de la e-CPS en mode découplé (CIBA)

L’application **e-CPS** reste supportée par **Pro Santé Connect (PSC)** dans le cadre de l’appairage.
Contrairement à d’autres solutions, **Okta supporte le flux découplé CIBA (Client-Initiated Backchannel Authentication)** grâce à sa fonctionnalité **Transactional Verification**.

Ce mécanisme permet à un professionnel de santé d’initier une connexion sur un poste (device de consommation) et de la valider à distance depuis un autre appareil (ex. smartphone avec e-CPS), sans interaction directe entre les deux.
La mise en œuvre nécessite la configuration d’un **authenticator mobile compatible** dans Okta.

Pour plus d’informations techniques, se référer à la documentation Okta sur [CIBA et Transactional Verification](https://developer.okta.com/docs/guides/configure-ciba/main/).

### Valeurs AMR spécifiques pour Okta

Dans le cadre du projet PSC sans couture, des valeurs **AMR (Authentication Methods References)** spécifiques ont été ajoutées pour tout **KC\_IDP\_HINT** lié à Okta :

| Valeurs AMR                          | Méthode d’authentification                                                 | Cas d’usage typique                                                                  | Niveau de garantie RIE |
|--------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------|------------------------|
| `"swk", "mfa", "pwd", "okta_verify"` | Authentification par mot de passe + Okta Verify (notification push ou TOTP) | Connexion classique depuis un poste individuel ou en mobilité (PC portable, smartphone) | Substantiel            |
| `"sc", "pin", "mfa", "hwk"`          | Authentification par carte **CPx** (smartcard) avec code PIN                | Utilisation d’un poste de travail fixe ou partagé dans un établissement de santé       | Élevé                  |
.

Ces valeurs permettent de tracer précisément la méthode d’authentification utilisée et d’assurer la conformité avec les exigences de l’ANS.
