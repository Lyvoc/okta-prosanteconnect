# Lifecycle Management avec Okta (optionnel)

- Provisioning / déprovisioning vers applications métiers.
- Connecteurs OIN, HR-as-a-Source, workflows.

Le **Workforce Lifecycle Management ** est la fonctionnalité d’Okta qui permet de gérer l’ensemble du cycle de vie des identités et des accès des collaborateurs, depuis leur arrivée dans l’organisation jusqu’à leur départ.

## Objectifs principaux
- **Automatiser** la création, la modification et la suppression des comptes utilisateurs.  
- **Assurer la conformité** aux politiques de sécurité et réglementaires (PGSSI-S, ANS, ISO 27001…).  
- **Réduire les risques** liés aux comptes orphelins ou aux droits excessifs.  
- **Simplifier l’expérience utilisateur** en offrant un accès rapide et cohérent aux applications.

## Phases du cycle de vie

<img width="2748" height="1230" alt="image" src="https://github.com/user-attachments/assets/122c024e-7b66-44a3-affe-6f114bc2fa90" />


1. **Onboarding - Joiner (arrivée)**  
   - Création automatique du compte utilisateur dans Okta depuis une source autoritaire (ex : SIRH, Active Directory, annuaire local).  
   - Attribution des rôles, groupes et applications en fonction du profil.  
   - Envoi des informations de connexion et activation des méthodes MFA.

2. **Mouvement interne - Mover (mobilité)**  
   - Gestion des changements de poste, d’unité ou de site.  
   - Mise à jour automatique des droits d’accès en fonction des nouveaux besoins.  
   - Retrait des accès devenus inutiles.

3. **Offboarding - Leaver (départ)**  
   - Désactivation immédiate du compte lors de la sortie du collaborateur.  
   - Révocation des sessions en cours et retrait des accès aux applications.  
   - Conservation des journaux et traçabilité pour audit.

## Fonctionnalités clés d’Okta
- **Connecteurs standards (SCIM, API, AD/LDAP)** : intégration avec les systèmes RH, IAM ou ITSM.  
- **Règles d’automatisation** : affectation d’applications et de groupes selon les attributs du profil.  
- **Workflows graphiques** : scénarios d’approbation, notifications, mises à jour spécifiques.  
- **Rapports et audits** : suivi de l’historique des changements, conformité aux exigences ANS.  

## Cas d’usage typiques dans un Établissement de Santé
- Création automatique d’un compte Okta lorsqu’un professionnel de santé est enregistré dans le SIRH.  
- Attribution automatique des accès aux applications (dossier patient, messagerie sécurisée, ProSanté Connect…) en fonction du service d’affectation.  
- Révocation immédiate des accès en cas de départ ou de fin de mission (ex. praticien intérimaire).  
- Suivi des comptes inactifs et génération de rapports pour les audits ANS.  


## Bénéfices pour un ES
- **Sécurité renforcée** : aucun compte actif sans supervision.  
- **Conformité** : respect des exigences d’authentification forte et de traçabilité.  
- **Efficacité opérationnelle** : réduction du temps de provisioning / deprovisioning.  
- **Expérience utilisateur fluide** : accès rapide aux ressources nécessaires, sans délais.  



