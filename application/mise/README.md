oc apply -f application/mise/mise-deployment.yaml --namespace=test-login

oc apply -f application/mise/mise-route.yaml --namespace=test-login

http://mise-route-test-login.apps-crc.testing:8080/calculator/check
