# Authorization Server & Claims (API Access Management)

## Étapes
1. Créez (ou dédiez) un **Authorization Server**.
2. Définissez les **scopes** nécessaires (`openid profile email` + scopes applicatifs).
3. Ajoutez les **custom claims** :
   - **`tid`** (String) → `OKTA_<tenantAlias>` (valeur constante) → inclus dans ID & Access Token.
   - **`oid`** (String) → `user.id` → inclus dans ID & Access Token.
   - (Optionnel) **`kc_idp_hint`** (String) → `OKTA_<tenantAlias>`.
4. **`amr`** : par défaut renseigné par Okta. Si l’ANS exige des libellés spécifiques, utilisez un **Token Inline Hook** pour enrichir la liste.

## AMR attendu (cas OKTA)
- Mot de passe + Okta Verify → `["swk","mfa","pwd","okta_verify"]`
- Carte CPx (possession + PIN) → `["sc","pin","mfa","hwk"]`

