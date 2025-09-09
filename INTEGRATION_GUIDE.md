# Guide d'intégration d'Okta à Pro Santé Connect

## Définition de Pro Santé Connect

**Pro Santé Connect (PSC)** est le fédérateur d’identités des professionnels des secteurs sanitaire, médico-social et social inscrits au Répertoire Partagé des Professionnels de Santé (RPPS). Mis en œuvre par l’Agence du Numérique en Santé (ANS), en sa qualité d’autorité compétente, il constitue un **service socle national** pour l’accès aux services numériques en santé.

PSC offre aux professionnels une expérience de connexion **simple, sécurisée et unifiée**, leur permettant de passer d’un service à l’autre de manière fluide. Basé sur des standards ouverts, il peut être implémenté gratuitement par les services numériques en santé.

Concrètement, PSC permet :

* **Une identification électronique sécurisée**, garantissant la conformité réglementaire et la protection des données de santé traitées. À ce jour, seuls la carte CPS et l’application e-CPS sont reconnus comme moyens d’identification électronique (MIE) ;
* **Un parcours utilisateur cohérent**, les professionnels étant déjà familiers de ces mécanismes d’authentification ;
* **L’exploitation des attributs sectoriels** (profession, savoir-faire, situation d’exercice, etc.), utilisables pour la vérification et l’automatisation des règles de contrôle d’accès.

Le fédérateur PSC évolue désormais pour permettre la **délégation d’authentification à des fournisseurs d’identité (FI) tiers**, constituant la première étape du projet *Pro Santé Connect sans couture*.

Pour approfondir :

* [Pro Santé Connect – site de l’ANS](https://esante.gouv.fr/produits-services/pro-sante-connect)
* [Délégation à un fournisseur d’identité local](https://industriels.esante.gouv.fr/produits-et-services/pro-sante-connect/delegation-un-fournisseur-d-identite-local)
* [Travaux en cours – Portail Industriels](https://industriels.esante.gouv.fr/)


## Objectif du document

L’objectif de ce document est d’expliquer la mise en œuvre d’une **délégation de l’authentification Pro Santé Connect (PSC)** auprès du fournisseur d’identité (FI) tiers **Okta**.

Ce document détaille les éléments de configuration nécessaires pour établir une fédération d’identité entre PSC et un tenant Okta déployé au sein d’un établissement de santé (ES), en conformité avec les exigences de sécurité définies dans le référentiel *PSC sans couture*.

Il couvre notamment l’intégration de l’application Pro Santé Connect (PSC) configurée dans Okta, en mettant en avant les prérequis, les concepts clés et les étapes essentielles. Les spécificités liées au FI Okta sont mises en lumière, celui-ci jouant le rôle de **premier cas d’usage pilote** pour une fédération d’identité entre PSC et un FI tiers.

Okta offre une expérience d’authentification centralisée et **Single Sign-On (SSO)**, permettant aux professionnels de santé d’accéder de manière fluide à toutes les ressources intégrées au sein du tenant de l’ES. Outre l’application PSC, ces ressources peuvent inclure des applications SaaS métiers, des portails hospitaliers, des services cloud ou toute application intégrée via le catalogue d’Okta Integration Network (OIN).

## Etape 1 - Déclaration de l'application OIDC pour la liaison avec PSC 

- L'objectif de cette étape va être de déclarer une application de type SPA pour la liaison avec Pro Santé Connect.
  Voici les étapes à suivre:
  Depuis l'interface d'administration d'Okta,cliquez sur **"Application"**.

<img width="313" height="759" alt="Screenshot 2025-09-05 at 09 51 54" src="https://github.com/user-attachments/assets/90f67acf-803d-4bb1-a9ac-278628f74add" />

 Cliquez ensuite sur **"Create App Integration"**, 
 
<img width="832" height="164" alt="Screenshot 2025-09-05 at 09 52 18" src="https://github.com/user-attachments/assets/c212bf09-7c52-4b04-aa04-262d80e8e69b" />

Puis sur **"Sign-in method" = "OIDC- OpenID Connect**.
Ensuite veuillez selectionner le type d'application = **"Single-Page Application"** et cliquez sur sur **"Next"**

<img width="957" height="836" alt="Screenshot 2025-09-05 at 09 52 36" src="https://github.com/user-attachments/assets/b076d0c9-54bd-4075-9292-1b7aae16d26a" />

Veuillez copier le **client ID** généré. Il servira à créer la liason avec PSC. Vous devrez le fournir à l'ANS lors du raccordement.

<img width="836" height="563" alt="Screenshot 2025-09-05 at 09 53 29" src="https://github.com/user-attachments/assets/bd5098d0-6ef0-4b61-831b-dabc2cdb5ece" />

Ensuite, veuillez renseigner les **URIs de redirection d'authentification**.

<img width="730" height="549" alt="Screenshot 2025-09-05 at 09 58 35" src="https://github.com/user-attachments/assets/2096033b-c7e3-4e83-8e74-2da4633e3168" />

Dans la rubrique **"Assignments">"Controlled Access"**, veuillez selctionner **"Skip group assignment for now"** et cliquer sur **"Save"**. L'assignation se fera après l'activation complete de cette intégration.  

<img width="904" height="295" alt="Screenshot 2025-09-05 at 10 00 17" src="https://github.com/user-attachments/assets/258634f8-b512-4911-9737-a2c0597d42aa" />

## Etape 2 - Chargement du workflows pour alimenter le claim OID en se base sur le claim SUB dans Okta

- L'objectif de cette étape sera d'utiliser un Okta Workflows pour venir aliment la valeur du claim OID utilisé par Microsoft en se basant sur le Sub d'Okta
Okta Workflow servira à faire une translation à la volée en invokant un webhook à chaque demande d'authentification.
Voici les étapes à suivre:
Depuis la console d'administration d'Okta, veuillez cliquer sur **"Workflows"** puis sur **"Workflows console"**

<img width="302" height="950" alt="Screenshot 2025-09-05 at 10 06 51" src="https://github.com/user-attachments/assets/f21eb885-4093-471e-8d5b-43566a184d94" />

Depuis la console d'administration d'Okta, veuillez naviguer sur l'onglet **"Flows"**, puis cliquer le petit **"+"** bleu en face de **"Folders"** pour créer un nouveau répertoire.
Veuillez choisir un nom, par exemple **"token inline"**, puis cliquer sur **"Import"**
A cette étape vous allez devoir importer un modèle de flow fonctionnel pour réaliser cette transormation.
Le script se trouve [ici](./scripts/workflows/tokenInlineHook.folder)

<img width="448" height="399" alt="Screenshot 2025-09-05 at 10 07 59" src="https://github.com/user-attachments/assets/641aa1ad-b7f6-4f52-a28c-296778500999" />

Dans l'encadré rouge vous verrez la commande de transformation qui sera invoké via le webhook qui sera créer à l'étape 3.
Veuilez enregistrer et activer ce workflow en cliquant sur le bouton **"Save"** en haut, à droite. 

<img width="1857" height="857" alt="Screenshot 2025-09-05 at 10 08 37" src="https://github.com/user-attachments/assets/b2b65960-65ce-43b0-b14a-deb2eb51481f" />

Ensuite cliquez sur l'icône **"</>"** en pied de page de la première carte du workflow à l'écran.

<img width="364" height="863" alt="Screenshot 2025-09-05 at 10 08 51" src="https://github.com/user-attachments/assets/d0f0d9ee-1382-4c75-b4f1-28d73b9bd6d5" 

Copier et garder l'url du champ **"Invoke URL"** de la fenêtre **"API endpoint settings"** et cliquer sur **"close"** 
Vous pouvez à présent fermer cette fenêtre et revenir sur l'interface d'administration principale d'Okta.

<img width="570" height="831" alt="Screenshot 2025-09-05 at 10 09 13" src="https://github.com/user-attachments/assets/7aafcf4a-cc80-484d-bb36-5533a45ae1f1" />

## Etape 3 - Création du Web hook (Inline Hook Okta) pour alimentation de l'AMR en invoquant le workflows en étape 2 

L'objective de cette étape sera de créer le web hook d'invoquation pour alimenter le claim AMR qui déclanchera le workflow créé à l'étpae précédente.
Voici les étapes à suivre:
Depuis la console d'administration d'Okta,veuillez cliquer sur **"Workflow"** puis sur **"Inline Hooks"**

<img width="318" height="806" alt="Screenshot 2025-09-05 at 10 01 11" src="https://github.com/user-attachments/assets/8eeeb537-2264-4946-9fa4-fb635e6c93c3" />

Ensuite cliquez sur **"Add Inline hook"**, puis semectionnez **"Token"** 
 
<img width="1056" height="638" alt="Screenshot 2025-09-05 at 10 01 36" src="https://github.com/user-attachments/assets/43c46c07-b483-4675-8662-cb1a0a4403e2" />

Depuis l'onglet **"Overview"** dans le champ **"Name"**, saisissez **"amr"** , puis coller l'url récupérée à l'étape 2.
Pour finir cliquez sur **"Save"**

<img width="1042" height="863" alt="Screenshot 2025-09-05 at 10 02 19" src="https://github.com/user-attachments/assets/28974794-428a-40ea-95d3-565265e3133e" />

## Etape 3 - Création du claim TID et consolidation du claim OID pour alimenter PSC

- L'objectif de cette dernière étape sera de créer le claim TID depuis le serveur d'Autorisation d'Okta, et d'utiliser le cliam OID invoké via le webhook créé précédemment.
Voici les dernières étapes à suivre:
Depuis l'interface d'administration d'Okta, veuillez cliquer sur l'onglet **"Security"** puis sur **"API"**.

<img width="321" height="859" alt="Screenshot 2025-09-05 at 10 03 12" src="https://github.com/user-attachments/assets/c71e2a96-913c-41d7-8099-cae563c87575" />

Ici vous aurez la posibilité d'utiliser soit le serveur d'autorisation par défaut d'Okta, soit de créer un serveur custom. Dans ce guide, nous avons fait le choix de prendre le serveur par défaut à fin de faciliter la lecture.
En face du serveur par defaut, veuillez cliquer sur le petit crayon.

<img width="1064" height="379" alt="Screenshot 2025-09-05 at 10 03 35" src="https://github.com/user-attachments/assets/652ee23d-0218-4bb0-9ec7-0a8819774b11" />

Ensuite, pour des soucis de compatibilité avec l'infrastructure de PSC, nous allons à la fois surcharger l'ID token en plus de l'access token du claim TID. 
## Note:
Le claim OID ne peut pas être utiliser ici car c'est un mot réservé, non utilisé par Okta mais qui est toutefois nécessaire à la communication avec PSC en lien avec le choix d'architecture dépendant de Entra ID. Cela est la raison à l'utilisation du webhook et du workflows décrit aux étapes 2 et 3. 
Le TID fait référence surtout au tenant ID de Microsoft qui ne peut être au même format dans Okta, car cette nommenclature est propre à Entra ID. Nous allons donc surcharger ce champs avec l'Org ID du tenant Okta.

## Comment récupérer l’ID de votre tenant Okta

En pratique, chez **Okta**, il n’existe pas d’“ID de tenant” au sens strict comme chez Microsoft Entra ID.  
Votre organisation (org) Okta est principalement identifiée par son **domaine unique** (par ex. `https://dev-123456.okta.com`).  
Selon le contexte, voici ce que cela peut désigner et comment vous pouvez le récupérer :

---

## 1. Domaine Okta (Org URL)
C’est le plus courant :

- Dans la console d’administration Okta, regardez l’URL :  
  `https://dev-123456.okta.com` → ici, `dev-123456` est l’**identifiant de votre org/tenant**.  
- Même logique si vous utilisez un domaine personnalisé (`https://sso.mones.fr`), l’ID d’org reste lié au domaine d’origine `okta.com` ou `okta-preview.com`.

📖 Documentation : [Find your Okta domain](https://developer.okta.com/docs/guides/find-your-domain/main/)

---

## 2. Org ID (UUID interne)
Okta attribue également un **identifiant interne (UUID)** à votre org.  
Vous pouvez le récupérer via l’API Management :

**Exemple:**

GET https://{yourOktaDomain}/api/v1/org
Authorization: SSWS ${api_token}

**Réponse en JSON**

{
  "id": "00o123abcdXYZ7890h7",
  "companyName": "Votre établissement de santé",
  "subdomain": "dev-123456",
  ...
}

Le champ "id" est l’orgId interne.


<img width="1063" height="550" alt="Screenshot 2025-09-05 at 10 05 19" src="https://github.com/user-attachments/assets/2cbdde3b-a5f9-4cf8-8ba6-f7705f3a9972" />  

Ensuite, veuillez cliquer sur l'onglet **"access policy"** 
Ici, vous aurez le choix de créer une nouvelle règle d'accès en cliquant sur **"Add New Access Policy"**, ce qui est recommandé en production, ou de cliquer sur la règle par défaut.
Puis de cliquer sur **"Edite"** pour lié votre application OIDC créée à l'étape 1.
 
<img width="1083" height="774" alt="Screenshot 2025-09-05 at 10 05 54" src="https://github.com/user-attachments/assets/4d15bef5-3acd-4a8e-91f1-248dfca2a2c1" />

Maintenant, veuillez cliquer sur le petit crayon en face de la règle par défaut pour éditer la règle.

<img width="751" height="235" alt="Screenshot 2025-09-05 at 10 06 12" src="https://github.com/user-attachments/assets/16fc58dc-c93f-4e32-b1b0-da3a35eb5c6f" />

A l'étape de la clause **"THEN"** veuillez selectionner le web hook **"amr"** précédemment créé et cliquer sur **"Update rule"**

 <img width="712" height="946" alt="Screenshot 2025-09-05 at 10 06 28" src="https://github.com/user-attachments/assets/188e438d-8809-4480-8f78-37f833c8b7df" />

Pour tester veuillez consulter la documentation [ici](./docs)

