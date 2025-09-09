# Paramétrage de la délégation PSC → Okta

1. Côté Okta, créez l’application OIDC (type Web) représentant PSC.
2. Renseignez les **Redirect URIs** PSC (sandbox/prod).
3. Configurez l’Authorization Server et les claims (`tid`, `oid`, `amr`).
4. Testez avec la collection Postman fournie (`scripts/postman/psc-okta.postman.json`).

Les schémas de séquence dans `imgs/` illustrent le flux :
- `seq-psc-okta-login.png`
- `seq-token-content.png`
