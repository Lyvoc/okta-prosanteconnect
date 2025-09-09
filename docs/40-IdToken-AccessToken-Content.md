# Contenu des jetons

Exemple d’ID Token comprenant `tid`, `oid`, et `amr` (extrait JSON).  
Vérifiez la présence et les valeurs lors des tests d’interopérabilité avec PSC.


# Exemple d’ID Token Okta (contenant `tid`, `oid`, `amr`)

```json
{
  "iss": "https://es-monotenant.okta.com/oauth2/default",
  "aud": "0oa1exampleClientId12345",
  "iat": 1694179200,
  "exp": 1694182800,
  "auth_time": 1694179180,
  "sub": "00u1abcd2EFGH3ijk4l5",
  "amr": [
    "okta_verify"
  ],
  "tid": "TID-CHU-NANCY",
  "oid": "OID-12345678",
  "acr": "loa3",
  "name": "Jean Dupont",
  "preferred_username": "jean.dupont@chu-nancy.fr",
  "given_name": "Jean",
  "family_name": "Dupont",
  "email": "jean.dupont@chu-nancy.fr"
}
```
## Dans cet exemple, voici le détail des champs utilisés:

**iss** : URL de ton tenant Okta (mono-tenant ES).

**aud** : identifiant du client OIDC configuré côté Okta.

**sub** : identifiant unique de l’utilisateur Okta.

**amr** : ici avec la valeur demandée "okta_verify".

**tid** : code identifiant l’établissement de santé (ici TID-CHU-NANCY).

**oid** : identifiant immuable de l’utilisateur dans l’ES (pivot d’appariement).

**acr** : niveau d’authentification (ici loa3).


**Champs additionnels usuels** : name, email, preferred_username.
