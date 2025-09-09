# Okta Device Access (optionnel)

- Authentification de session sur endpoints Windows/macOS.
- À utiliser lorsque l’établissement souhaite étendre la gouvernance jusqu’au poste.

Okta Device Access (ODA) est un ensemble de fonctionnalités permettant d'optimiser la sécurité des appareils avec Okta. Il est actuellement axé sur les appareils Windows (ordinateurs portables, ordinateurs de bureau, machines virtuelles et serveurs) et macOS.

La figure suivante présente les principaux composants et intégrations avec la plateforme Okta Workforce Identity Cloud et les systèmes externes.

<img width="1024" height="517" alt="image" src="https://github.com/user-attachments/assets/e88c42a2-9828-4dc4-b582-89144081d056" />


La solution exploite Okta Verify sur les postes de travail Microsoft Windows et Apple macOS pour implémenter des fonctionnalités telles que l'authentification multifacteur (MFA) pour postes de travail, la synchronisation et la réinitialisation de mot de passe en libre-service. Les postes de travail macOS et Windows utilisent Okta Verify et la configuration locale pour implémenter ces fonctionnalités (elles peuvent être déployées par un produit de gestion d'appareils externe). Okta Verify fonctionnera avec Okta et Verify sur les appareils mobiles pour mettre en œuvre les différents cas d'usage. Okta Verify Windows Desktop MFA

Pour plus d'information: [lien](https://iamse.blog/access-management/okta-device-access/)

Bien qu'Okta puisse agir en tant qu'autorité de certification (AC), de nombreuses entreprises préfèrent exploiter leur infrastructure à clés publiques (PKI) existante, à savoir les services de certificats Microsoft Active Directory (ADCS).

Ce guide technique propose une approche étape par étape pour utiliser votre propre AC ADCS avec Okta Device Access.
Nous explorerons le processus de création d'une propriété de certificat personnalisée dans un modèle ADCS, une étape essentielle pour intégrer harmonieusement votre PKI sur site à la plateforme cloud native d'Okta.

Voici le lien du guide de configuration d'Okta avec une PKI externe [lien](https://iamse.blog/2025/09/02/unifying-your-corporate-pki-with-okta-device-access/)
