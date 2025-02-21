# Các bước thực hiện

## Bối cảnh
Hoàn toàn là môi trường local, vì một số lý do nên chúng em chưa thể làm trên môi trường cloud.

1 server jenkins được cài đặt docker, minikube; 1 server sonarQube

Mỗi 1 branch là một service và main là nơi để deploy ứng dụng. 

## Cài đặt
Cài đặt, cấu hình cho Jenkins và các agent ở theo hướng dẫn các link dưới đây:

[Jenkins](https://www.jenkins.io/doc/book/installing/linux/)


[Cấu hình webhook Github và Jenkins cho tự động CI/CD](https://medium.com/@sangeetv09/how-to-configure-webhook-in-github-and-jenkins-for-automatic-trigger-with-cicd-pipeline-34133e9de0ea). 

[Cài đặt các credential cho github repo và docker registry](https://www.jenkins.io/doc/book/using/using-credentials/)

[Docker](https://docs.docker.com/engine/install/ubuntu/)

[SonarQube](https://gist.github.com/dmancloud/0abf6ad0cb16e1bce2e907f457c8fce9)

[Tích hợp SonarQube vào Jenkins](https://www.geeksforgeeks.org/how-to-integrate-sonarqube-with-jenkins/)

[Tích hợp Synk vào Jenkins](https://docs.snyk.io/scm-ide-and-ci-cd-integrations/snyk-ci-cd-integrations/jenkins-plugin-integration-with-snyk)

[Minikube](https://kubernetes.io/vi/docs/tasks/tools/install-minikube/)

Cài đặt cert minikube vào jenkins (chung server):

```bash
sudo mkdir -p /var/lib/jenkins/.kube
sudo cp ~/.kube/config /var/lib/jenkins/.kube/config
sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube
sudo chmod 600 /var/lib/jenkins/.kube/config
sudo mkdir -p  /var/lib/jenkins/.minikube
sudo cp -r ~/.minikube/* /var/lib/jenkins/.minikube
sudo vi /var/lib/jenkins/.kube/config
```

Thay thế `/home/<USER>/.minikube/` bằng `/var/lib/jenkins/.minikube`

Kiểm tra bằng `sudo -u jenkins kubectl get nodes`
