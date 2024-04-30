# Access Application
Nginx available at http://nginx-route-test-login.apps-crc.testing:8080/

/portal -> sample web page
/api -> route to microservice

# Docker build setup
```
oc new-build --name=reverse-proxy --namespace=test-login --binary --strategy=docker
```

# Docker build run
```
oc start-build reverse-proxy --namespace=test-login --from-dir=application/nginx --follow
```

# Deploy
1. Update the image in the deployment with one resulting from the build
2. Delete old deployement
```
oc delete deployment nginx-deployment -n test-login
```
3. Deploy
```
oc apply -f application/nginx/nginx-deployment.yaml --namespace=test-login
oc set sa deploy nginx-deployment nginx-sa
```
4. Apply Service Account
   SA created by following the guide [How to fix permission errors in pods using service accounts | Enable Sysadmin (redhat.com)](https://www.redhat.com/sysadmin/security-context-constraint-permissions)
```
oc set sa deploy nginx-deployment nginx-sa
```

# Pod error with default Service Account (Permissions)
To solve this issue read documents in [References - Security Contexts Constraints](#security-contexts-constraints)
```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: can not modify /etc/nginx/conf.d/default.conf (read-only file system?)
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/04/16 17:25:24 [warn] 1#1: the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
nginx: [warn] the "user" directive makes sense only if the master process runs with super-user privileges, ignored in /etc/nginx/nginx.conf:2
2024/04/16 17:25:24 [emerg] 1#1: mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
nginx: [emerg] mkdir() "/var/cache/nginx/client_temp" failed (13: Permission denied)
```

# References
## Security Contexts Constraints
- [Introduction to Security Contexts and SCCs (redhat.com)](https://www.redhat.com/en/blog/introduction-to-security-contexts-and-sccs)
- [Managing SCCs in OpenShift (redhat.com)](https://www.redhat.com/en/blog/managing-sccs-in-openshift)
- [How to fix permission errors in pods using service accounts | Enable Sysadmin (redhat.com)](https://www.redhat.com/sysadmin/security-context-constraint-permissions)
## OpenResty
- [OpenResty official webpage](https://openresty.org/)
- [docker-openresty (github.com)](https://github.com/openresty/docker-openresty/tree/master)
- [lua-resty-openidc (github.com)](https://github.com/zmartzone/lua-resty-openidc)
- [lua-nginx-module (github.com)](https://github.com/openresty/lua-nginx-module)
- [Authentication and authorization proxy with OpenResty(daenney.github.io)](https://daenney.github.io/2019/10/05/beyondcorp-at-home-authn-authz-openresty/#proxying-to-a-backend)