
oc create serviceaccount jenkins
oc project education 

JENKINS_SECRETS=$(oc describe sa jenkins | grep Token|tr -d ' ' | cut -d ":" -f2-)
oc describe secrets $JENKINS_SECRETS | grep ^token | tr -d ' ' | cut -d ':' -f2
JENKINS_TOKEN=$( oc describe secrets $JENKINS_SECRETS | grep ^token | tr -d ' ' | cut -d ':' -f2 )

oc policy add-role-to-user edit  -z jenkins
oc adm policy add-scc-to-user privileged -z jenkin
