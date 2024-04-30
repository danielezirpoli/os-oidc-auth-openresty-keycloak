# Architecture
```mermaid
graph LR;
    u[User]-->|1|ap["Auth Proxy - OpenResty (Nginx)"];
    subgraph OpenShift
    ap<-->|2|iam[IdP - Keycloak];
    ap-->|3|m[Microservice]
    end
```

# Setup Keycloak
[Get started with Keycloak on OpenShift (keycloak.org)](https://www.keycloak.org/getting-started/getting-started-openshift)

# URLs
Keycloak: https://keycloak-keycloak.apps-crc.testing
Keycloak Admin Console: https://keycloak-keycloak.apps-crc.testing/admin
Keycloak Account Console: https://keycloak-keycloak.apps-crc.testing/realms/<myrealm>/account
