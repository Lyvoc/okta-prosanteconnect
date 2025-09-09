# Modèle mono‑locataire Okta

- Isolation forte (sécurité, conformité, audit).
- Alignement sur l’attente ANS : **configuration figée répliquée par établissement** (format du `tid`, présence et sémantique `amr`, etc.).

## Schéma d'architecture multi-locataire vs mono-locataire

**Multi-locataire**
<img width="1446" height="595" alt="image" src="https://github.com/user-attachments/assets/d9fedad6-8832-498e-b980-a18c63381450" />

**Mono-locataire (commun à tous les fournisseurs tiers)**

<img width="1318" height="591" alt="image" src="https://github.com/user-attachments/assets/066a735e-4242-4f74-9444-6611c12d3d7f" />


## `KC_IDP_HINT`
- Format recommandé : `OKTA_<tenantAlias>`
- Cette valeur est la **référence "source"** côté PSC (Datapass associé).
