---
title: "Microsoft Entra ID"
---

# Microsoft Entra ID

Implements OpenID Connect. By default, `nonce` is set to `_`.

For usage, see [OAuth 2.0 provider with PKCE](/guides/oauth2-pkce).

```ts
import { MicrosoftEntraID } from "arctic";

const entraId = new MicrosoftEntraID(clientId, clientSecret, redirectURI);
```

```ts
const url: URL = await entraId.createAuthorizationURL(state, codeVerifier, {
	// optional
	scopes // "openid" always included
});
const tokens: MicrosoftEntraIdTokens = await entraId.validateAuthorizationCode(code, codeVerifier);
const tokens: MicrosoftEntraIdTokens = await entraId.refreshAccessToken(refreshToken);
```

## Get user profile

Add the `profile` scopes. Optionally add the `email` scopes to get user email.

```ts
const entraId = new MicrosoftEntraID(clientId, clientSecret, redirectURI, {
	scopes: ["profile", "email"]
});
```

Parse the ID token or use the `userinfo` endpoint. See [ID token claims](https://learn.microsoft.com/en-us/entra/identity-platform/id-token-claims-reference).

```ts
const response = await fetch("https://graph.microsoft.com/oidc/userinfo", {
	headers: {
		Authorization: `Bearer ${accessToken}`
	}
});
const user = await response.json();
```
