# 1. microk8s 설치
   ## EPEL 추가
   - sudo yum install -y epel-release
   - service firewalld stop (방화벽 정지)
   ## 스냅설치
   - sudo yum install -y snapd (스냅설치)
   - sudo systemctl enable --now snapd.socket
   - sudo ln -s /var/lib/snapd/snap /snap
   ## microk8s 설치
   - snap install microk8s --classic --channel=1.18/stable
   ## 그룹에 가입
   - sudo usermod -a -G microk8s $USER
   - sudo chown -f -R  $USER ~/.kube
   - su - $USER
   ## 사용방법
   - microk8s status --wait-ready (addon 상태확인)
   - microk8s kubectl get nodes, microk8s kubectl get services
   - alias kubectl='micrik8s kubectl' (알리아스 등록)
  ## 에드온 사용
   - microk8s enable dns dashboard storage
   - microk8s enable kubeflow -> failure can be occured due to disk space shortage
  ## microk8s 시작 및 정지
   - microk8s stop/start
  ## microk8s 설정 방법
   - /var/snap/microk8s/current/args/ 밑에 설정관련 파일들 다수 있음
     -> 여기에서 적당한 파일 열고 수정함
   - microk8s stop 후에 microk8s start
# 2. helm 설치
  ## helm3 addon anable
   - microk8s enable helm3
  ## microk8s가 아닌 경우에 (kubernetes인 경우 helm3 직접설치)
   - curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
   - chmod 700 get_helm.sh
   - ./get_helm.sh
   - helm vesion
   - helm repo add stable https://kubernetes-charts.storage.googleapis.com/
   - helm repo update
   - helm search repo stable
# 3. hkube 설치
   - alias helm='microk8s heml3'
   - helm repo add hkube http://hkube.io/helm/
   - helm repo update
   - helm install hkube hkube/hkube
     (helm install hkube --set build_secret.docker_username=docker_username --set build_secret.docker_password=docker_password hkube/hkube)
   - 다 뜰때가지 시간이 제법 걸림 (5분 정도)
# 4. 접속
   - http://GCP 에서 할당받은 IP/hkube/simulator
   - 알고리즘/파이프라인 실행 시에 에러 발생하는 것은
     kube-apiserver 파일에서 --allow-privileged=true를 추가한 후에 microk8s stop -> start 
