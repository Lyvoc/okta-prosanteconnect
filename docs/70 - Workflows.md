
# Okta Workflows

**Okta Workflows** est une plateforme d’automatisation *no-code/low-code* intégrée à Okta.  
Elle permet de créer et d’orchestrer des processus d’identité et d’accès sans écrire de code, en connectant facilement Okta à d’autres applications ou systèmes.

## Objectifs principaux
- **Automatiser** les processus liés aux identités (provisioning, MFA, gouvernance, alertes).  
- **Éliminer le code spécifique** : réduire la dépendance aux scripts maison difficiles à maintenir.  
- **Simplifier l’intégration** avec des applications tierces via des connecteurs prêts à l’emploi.  
- **Renforcer la conformité** grâce à des scénarios documentés et traçables.

## Fonctionnement
- Les Workflows sont constitués de **connecteurs** (Okta, ServiceNow, Slack, Salesforce, etc.), de **fonctions** et de **conditions logiques**.  
- Chaque automatisation est déclenchée par un **événement** (ex. création d’utilisateur, changement d’attribut, authentification échouée).  
- Les actions se succèdent sous forme de **flux visuels** (drag-and-drop).

<img width="700" height="414" alt="image" src="https://github.com/user-attachments/assets/42dfeafa-1fac-401c-8e80-63710a490692" />


## Exemples de cas d’usage
1. **Gestion des utilisateurs**
   - Lorsqu’un nouvel utilisateur est créé dans Okta → créer un compte dans l’application métier via SCIM.  
   - Si un utilisateur change de service → mettre à jour automatiquement ses groupes et ses droits.  

2. **Sécurité et MFA**
   - Détecter des tentatives de connexion depuis un pays non autorisé → notifier le RSSI par Slack et bloquer le compte.  
   - Forcer la réinitialisation MFA d’un utilisateur qui n’a pas validé ses facteurs dans les 24h.  

3. **Conformité et audits**
   - Générer un rapport mensuel des comptes inactifs et l’envoyer par email en CSV.  
   - Créer automatiquement une tâche ServiceNow si un utilisateur dispose de droits élevés sans justification.  

## Avantages clés pour les Établissements de Santé
- **Réduction du risque opérationnel** : suppression des tâches manuelles sujettes à erreurs.  
- **Agilité** : création rapide de flux adaptés à des besoins spécifiques (ex. onboarding praticiens temporaires).  
- **Interopérabilité** : intégration avec le SIH via API, SCIM ou connecteurs standards.  
- **Auditabilité** : chaque flux est versionné et peut être documenté pour répondre aux exigences de l’ANS.  

---

## Bénéfices
- **Productivité** : automatisation des tâches répétitives (gain de temps pour les équipes IT).  
- **Sécurité** : réaction immédiate à des événements suspects (alerte, blocage, escalade).  
- **Conformité** : support des contrôles périodiques exigés par l’ANS (rapports, traçabilité).  
- **Expérience utilisateur** : moins de délais dans l’attribution ou le retrait des accès.

Pour plus d'information sur Okta Workflows [lien](https://help.okta.com/wf/en-us/content/topics/workflows/learn/learn-workflows.htm)

---

