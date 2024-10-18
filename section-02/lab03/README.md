

kubectl create secret docker-registry regcred \
--docker-username=$REGISTRY_USER \
--docker-password=$REGISTRY_PASS \
--docker-server=$REGISTRY_SERVER \
--docker-email=$REGISTRY_EMAIL


