# rhbk-lab

#Create a rhbk project on OpenShift

oc create project rhbk-lab

#Install Operator subscription on OpenShift

oc create -f operator/rhbk-subscription.yaml

#Install Database instance

oc create -f database/example-postgres.yaml

#Create Database secrets used By Keycloak instance

oc create secret generic keycloak-db-secret --from-literal=username=keycloak --from-literal=password=keycloak

#Create self Signed certificate

openssl req -subj '/CN=sso.inforemington.local/O=Inforemington./C=BR' -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem


#Import generated certificates in OpenShift secrets

oc create secret tls sso-tls-secret --cert certificate.pem --key key.pem

#Install Keycloak Instance

oc create -f keycloak/kc-example.yaml

