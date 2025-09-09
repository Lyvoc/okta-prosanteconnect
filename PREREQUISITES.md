# Socle technique minimal pour la mise en œuvre de PSC avec Okta

Un socle technique minimal est nécessaire à l’intégration de **Pro Santé Connect (PSC)** avec **Okta** en termes de prérequis de raccordement.  
Il s’agit pour l’établissement de santé (ES) de s’assurer que les éléments suivants sont en place avant de procéder aux étapes de configuration.  

La mise en place de ce socle est optionnelle selon l’existant de l’ES, mais reste indispensable pour garantir la conformité avec le **référentiel PSC sans couture** et les bonnes pratiques de sécurité.  

---

## Locataire Okta et licences requises

Un **tenant Okta** (org) fonctionnel est nécessaire pour supporter la délégation d’authentification.

| Type de licence | Modules requis | Modules optionnels |
|-----------------|----------------|--------------------|
| **Okta Identity Cloud** | - Universal Directory (UD) <br> - Single Sign-On (SSO) <br> - API Access Management (AM) <br> - Workflows basic (5 worklfows)  | - Okta Device Access (ODA) <br> - Lifecycle Management (LCM) |

📖 Documentation : [Okta Editions & Features](https://www.okta.com/pricing/)

---

## Accès administrateur et environnements PSC

- Un **compte administrateur Okta** est nécessaire pour la configuration initiale.  
- Accès aux environnements **PSC Bac à sable** et **Production** selon la procédure Datapass.  
- Une nomenclature doit être définie pour le **KC_IDP_HINT**, par exemple :  
  OKTA\_<"tenantAlias">


📖 Documentation : [Okta Admin Console Overview](https://help.okta.com/en-us/content/topics/getting-started/admin-console/admin-console.htm)

---

## Déploiement des Moyens d’Identification Électronique (MIE)

L’ES doit avoir mis en œuvre au moins un MIE conforme aux exigences de l’ANS :  

- Authentification par **carte CPx (smartcard)** avec code PIN.  
- Authentification **Okta Verify** (Push ou TOTP).  
- Clés de sécurité **FIDO2 / Passkeys**.  

📖 Documentation :  
- [Okta Verify](https://help.okta.com/en-us/content/topics/end-user/okta-verify/okta-verify-overview.htm)  
- [FIDO2/WebAuthn with Okta](https://help.okta.com/en-us/content/topics/security/webauthn/fido2-webauthn.htm)  
- [Smart Card Authentication with Okta](https://help.okta.com/en-us/content/topics/security/smart-card-authentication.htm)  

---

## Déclaration des comptes utilisateurs

Pour les phases de test et d’expérimentation, l’ES doit prévoir :  
- Des **utilisateurs non-administrateurs** (professionnels de santé) pour valider les scénarios.  
- Des **groupes** Okta permettant de simuler les populations cibles et appliquer des règles d’accès conditionnel.  

📖 Documentation :  
- [Manage Users](https://help.okta.com/en-us/content/topics/users-groups-profiles/usgp-manage-users.htm)  
- [Manage Groups](https://help.okta.com/en-us/content/topics/users-groups-profiles/usgp-manage-groups.htm)  

---

## Intégration des appareils (optionnel)

Avec **Okta Device Access**, il est possible d’étendre la sécurité PSC directement à l’ouverture de session Windows/macOS :  

- Authentification MFA dès le login.  
- Support de la **smartcard (CPx)**, **FIDO2**, ou **Okta Verify**.  
- Gestion centralisée via les politiques d’Okta.  

📖 Documentation : [Okta Device Access Overview](https://help.okta.com/oie/en-us/content/topics/device-access/device-access.htm)  

---

## Gouvernance et bonnes pratiques

Comme pour toute fédération d’identité, l’ES reste responsable de :  

- La **sécurisation** et la **maintenance** de son tenant Okta.  
- La **bonne configuration des méthodes d’authentification** conformes aux exigences de l’ANS.  
- L’application des **bonnes pratiques de gestion des accès administratifs**.  

📖 Documentation : [Security Best Practices for Okta](https://help.okta.com/en-us/content/topics/security/security-best-practices.htm)
