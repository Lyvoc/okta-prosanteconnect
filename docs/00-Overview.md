# Vue d’ensemble

Ce guide présente une intégration **Okta** (mono‑tenant) pour **ProSanté Connect (PSC)**.

## Architecture

<img width="2136" height="883" alt="PSC_Okta_MonoTenant" src="https://github.com/user-attachments/assets/b667887c-5a18-458f-83e2-819f922eece2" />


## Principes
- **Un établissement = un tenant Okta** (org).
- `KC_IDP_HINT` = `OKTA_<tenantAlias>` (identifiant de référence côté PSC).
- Claims jetons OIDC :
  - `tid` (tenant id) : constante par tenant (`OKTA_<tenantAlias>` ou UUID figé).
  - `oid` (object id) : `user.id` Okta (immuable au sein du tenant).
  - `amr` : références de méthodes d’auth; possibilité d’enrichissement via **Token Inline Hook**.

## Licences Okta
- **Minimum** : UD, SSO, API Access Management.
- **Optionnels** : Okta Device Access, Lifecycle Management.
