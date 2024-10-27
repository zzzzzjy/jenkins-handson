
#### 1.1. Install Jenkins on Linux(Centos) 
```bash
sudo dnf upgrade -y
sudo dnf install -y java-11-openjdk
```

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
tar xvzf apache-maven-3.8.8-bin.tar.gz -C /usr/local
```

vi editor로 열어MAVEN_HOME 환경정보를 추가
```bash
export MAVEN_HOME=/usr/local/apache-maven-3.8.8 
PATH=$PATH:$HOME/bin:$MAVEN_HOME/bin 
```

maven test
```bash
mvn
```

sudo dnf install -y git 
wget https://pkg.jenkins.io/redhat-stable/jenkins.repo -O /etc/yum.repos.d/jenkins.repo 
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

```bash
sudo dnf install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
Podman을 설치
```bash
yum install -y podman 
sysctl user.max_user_namespaces=15000
```

#### 1.2 install jenkins on docker
sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
docker pull hello-world 
docker images


docker-compose 설치 
```bash
DOCKER_CONFIG=/usr/local/lib/docker 
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.14.2/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
```

```bash
vi ~/.bash_profile 
```

```bash
DOCKER_CONFIG=/usr/local/lib/docker
PATH=$PATH:$HOME/bin:$MAVEN_HOME/bin:$DOCKER_CONFIG/cli-plugins
```

```bash
source ~/.bash_profile
```

docker-compose

#### 1.3.	Start Jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword

#### 1.4. Install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
tar xvzf 
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
chmod +x /usr/local/bin/minikube
```

minikube 시작 
```bash
minikube start  --force
```

```bash
curl -L "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl

chmod +x /usr/local/bin/kubectl

kubectl get po -A
```

```bash
less ~/.kube/config
```


kube config변경
```bash
ls -al /root/.minikube/ca.crt

cat /root/.minikube/ca.crt|base64 -w0;echo
cat /root/.minikube/profiles/minikube/client.crt|base64 -w0;echo
cat /root/.minikube/profiles/minikube/client.key|base64 -w0;echo

cd ~/.kube
vi config
```

certificate-authority => certificate-authority-data로 변경
client-certificate => client-certificate-data 로 변경
client-key => client-key-data 로 변경

```bash
kubectl get pods 

# 결과 
No resources found in default namespace
```

#### 1.5. Install Kubernetes plugin

Name : jnlp-slave
Docker image : jenkinsci/jnlp-slave:latest
Working directory : /home/jenkins/agent
Command to run : /bin/sh -c

```bash
kubectl create ns jenkins
kubectl create clusterrolebinding jenkins-admin  --clusterrole cluster-admin --serviceaccount jenkins:default
```

#### 1.6. Install Agent Node

Name : linux-one
Number of executors : 2
Remote root directory : /home/jenkins/agent


java -jar agent.jar -jnlpUrl http://119.81.97.216:8080/manage/computer/linux%2Done/jenkins-agent.jnlp -secret 772e400714d944fe23c5137b0bc0d7344f4007eb05551032f15bd5b960e92f06 -workDir "/home/jenkins/agent" 


#### 1.7. Configure Global Build of Jenkins