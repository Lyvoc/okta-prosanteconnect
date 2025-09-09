
# Okta × ProSanté Connect (PSC) – (Mono‑tenant)

Ce projet en cours vise à enrichir la documentation officielle de l’[Agence du Numérique en Santé](https://esante.gouv.fr/lagence) (ANS) concernant l’intégration de Pro Santé Connect (PSC) en mode sans couture et la délégation à un [fournisseur d'identité local](https://industriels.esante.gouv.fr/produits-et-services/pro-sante-connect/delegation-un-fournisseur-d-identite-local). Ces évolutions permettent aux professionnels de santé d’accéder à PSC en s’appuyant directement sur l’identité numérique délivrée par leur établissement de santé (ES). Le suivi de l’avancement est disponible [ici](https://industriels.esante.gouv.fr/produits-et-services/pro-sante-connect/travaux-en-cours).

Les travaux portent spécifiquement sur la mise en œuvre de cette délégation avec Okta. La solution est d’ores et déjà fonctionnelle dans l’environnement Bac à sable PSC, et sa généralisation à l’environnement de production est en cours de planification. Des guides pratiques et des ressources sont mis à disposition pour faciliter l’activation de la délégation avec Okta, désormais reconnu parmi les [fournisseurs d’identité supportés par PSC](https://industriels.esante.gouv.fr/produits-et-services/pro-sante-connect/documentation-technique-idp-externe).

Ce dépôt fournit **une implémentation de référence avec Okta comme solution de fournisseur d'identité** pour l’intégration avec **ProSanté Connect (PSC)**.  

## A propos de Pro Santé Connect (PSC) et du projet "sans couture"

[**Pro Santé Connect**](https://esante.gouv.fr/produits-services/pro-sante-connect) (PSC) est le fédérateur d’identités des professionnels des secteurs sanitaire, médico-social et social inscrits au **Répertoire Partagé des Professionnels de Santé (RPPS)**. Ce service socle, proposé par l’Agence du Numérique en Santé (ANS) en tant qu’autorité compétente, constitue la brique centrale d’authentification pour l’écosystème numérique en santé.

Il offre aux professionnels une manière **simple, sécurisée et unifiée** de se connecter à l’ensemble de leurs services numériques, avec une navigation fluide d’une application à l’autre. Les éditeurs de services peuvent intégrer gratuitement ce socle, fondé sur des technologies standardisées et interopérables.

PSC apporte plusieurs bénéfices clés :

* **Sécurisation de l’identification électronique** : il protège les données de santé traitées et garantit la conformité réglementaire en matière de niveau de garantie de l’identification électronique des usagers professionnels. À ce jour, et en dehors des dispositifs de délégation, seuls deux moyens d’identification électronique (MIE) sont supportés : les [cartes de Professionnels de Santé (CPx)](https://esante.gouv.fr/produits-services/cartes-de-professionnels-de-sante) et l’[application mobile e-CPS](https://esante.gouv.fr/produits-services/e-cps).
* **Un mode de connexion familier** : largement adopté, il favorise l’engagement des utilisateurs grâce à une expérience homogène entre de nombreux services.
* **Exploitation des attributs sectoriels** : PSC transmet des informations fiables (profession, savoir-faire, situation d’exercice, etc.) permettant leur vérification et, le cas échéant, l’automatisation du contrôle d’accès.

Enfin, PSC bénéficie d’une homologation au [**Référentiel Général de Sécurité (RGS)**](https://cyber.gouv.fr/le-referentiel-general-de-securite-rgs), gage de robustesse et de conformité aux exigences de cybersécurité de l’État.

Le projet **PSC sans couture** vise à simplifier et fluidifier l’accès aux services numériques des établissements de santé.
PSC permet désormais la délégation d’authentification à des fournisseurs d’identités tiers, dont **Okta**.
Grâce à cette évolution, une authentification réalisée via un tenant Okta autorisé peut être reconnue et acceptée par PSC, sous réserve des configurations techniques requises.

## À propos d’Okta

[**Okta**](https://www.okta.com/fr/) est un fournisseur de services d’identité reconnu mondialement, spécialisé dans la gestion des accès et la fédération d’identités. Sa plateforme **Identity Cloud** permet aux organisations de sécuriser et de simplifier l’authentification de leurs utilisateurs, collaborateurs, partenaires et clients.

Okta offre une approche **cloud-first**, indépendante des environnements technologiques, et s’appuie sur des **standards ouverts** (SAML, OpenID Connect, OAuth 2.0, SCIM, etc.) pour garantir l’interopérabilité et la flexibilité.

Avec Okta, les organisations bénéficient :

* d’une **sécurisation avancée des accès** grâce à l’authentification multifacteur (MFA), la détection adaptative des menaces et la gestion centralisée des politiques ;
* d’une **expérience utilisateur fluide** avec des fonctionnalités telles que le Single Sign-On (SSO) et l’authentification sans mot de passe ;
* d’une **gouvernance renforcée** avec la gestion du cycle de vie des identités, l’automatisation des habilitations et la traçabilité complète des accès ;
* d’une **intégration rapide** grâce à un large catalogue d’applications préconfigurées et à des APIs robustes.

Okta accompagne ainsi les établissements de santé et les organisations publiques ou privées dans la **mise en conformité réglementaire** et la **sécurisation de leurs écosystèmes numériques**.

### Liens utiles Okta :

* Site officiel : [https://www.okta.com/fr/](https://www.okta.com/fr/)
* Documentation développeurs : [https://developer.okta.com/docs/](https://developer.okta.com/docs/)
* Catalogue d’intégrations : [https://www.okta.com/integrations/](https://www.okta.com/integrations/)
* Blog sécurité et identité : [https://www.okta.com/blog/](https://www.okta.com/blog/)
* Support et centre d’aide : [https://support.okta.com/help](https://support.okta.com/help)

## Objectifs

- Modèle **mono‑locataire Okta** : *un établissement = un tenant Okta*.
- Normalisation du **KC_IDP_HINT** au format `OKTA_<tenantAlias>` (ex. `OKTA_chu-nantes`) ou numéro Datapass.
- Claims standardisés dans les jetons **OIDC**:
  - `tid` = identifiant de tenant attendu par PSC (valeur constante `OKTA_<tenantAlias>` ou UUID figé si requis).
  **Attention!** le claim **tid** n'est pas standard mais en lien avec les spécifications Microsoft. PSC utilise Entra ID pour router les authentifications vers Keycloak. 
  - `oid` = identifiant immuable de l’utilisateur (`user.id` Okta).
  - `amr` = méthodes d’authentification; possibilité d’**enrichissement** via Token Inline Hook

## Licences

> **Licences Okta minimales** : Universal Directory (UD), Single Sign‑On (SSO), API Access Management (AM), Okta Workflows.  
> **Optionnels** : Okta Device Access, Lifecycle Management.
Plus de détail [ici](./LICENSE.md)

## Arborescence

- `docs/` : sources des guides 
- `imgs/` : schémas  et rendus PNG 
- `scripts/` : scripts de build, Postman (optionnel)
- `.github/` : templates pour issues/PR

---
