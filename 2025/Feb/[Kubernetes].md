# [Kubernetes]

> 개념과 이슈 트래킹을 조합해서 “복기 가능한” 자료를 만들도록 합니다.
> 

> 이슈 트래킹 작성 원칙: Phase1(환경, 로그, 최근 변경사항), Phase2(확인, 시도, 결과분석) 형식으로 정리하세요. (Phase2는 최대 5회까지 반복하고 해결 안 되면 도움 요청.)
> 

> 실습은 천천히, 하지만 robust하게 하세요.
> 

# Q&A

- 사이드카가 뭐야?
    - 하나의 Pod 내부에서 메인 컨테이너와 함께 실행되는 보조 컨테이너.
- 왜 사이드카를 사용해?
    - 기능분리: 어플리케이션 코드와 무관하게 새로운 기능을 추가할 수 있음.
    - 유지보수 용이: 애플리케이션을 수정하지 않고 새로운 기능을 추가할 수 있음
- 리플리카셋이 뭐야?
    - 쿠버네티스에서 동일한 Pod를 일정 개수만큼 유지하는 컨트롤러.
    - 리플리카셋만 따로 쓰는 경우는 잘 없고, 디플로이먼트 컨트롤러 안에 포함시켜서 쓴다.
- 디플로이먼트도 컨트롤러인데, 같은 컨트롤러인 리플리카셋도 디플로이먼트 안에 있다고? 컨트롤러 안에 컨트롤러가 있는 셈인가?
    - 맞다.
- 드레이닝이 뭐야?
    - 쿠버네티스에서 특정 노드를 비우는 과정. 해당 노드에서 실행중인 모든 Pod를 종료하고 다른 노드로 이동시키는 작업
    - 사용자가 결제를 하던 중 파드가 내려가버리면 파드(컨테이너들)의 데이터가 날아가며 문제가 생길 수 있음. 안전을 위해 트래픽을 비우는 과정이 드레이닝.
- 디플로이먼트 전략이 뭐야?
    - 애플리케이션의 새로운 버전을 배포할 때, 기존 버전과 새로운 버전을 어떻게 교체할지 결정하는 방식.
- 롤링 업데이트 전략에서 maxSurge와 maxUnavailable 전략 설명
    - maxSurge: 새롭게 생성할 수 있는 추가 Pod 개수 (최대 몇 개 더 만들 수 있는지)
    - maxUnavailable: 동시에 비활성화될 수 있는 기존 Pod 개수(한 번에 몇 개 삭제할 수 있는지)
- 쿠버네티스에서 서비스의 정의
    - 동적으로 변경되는 Pod들의 네트워크 접근을 안정적으로 제공하는 네트워크 오브젝트.
    - 변하는 Pod들의 IP 대신, 클러스터 내부 또는 외부에서 항상 동일한 방식으로 접근할 수 있도록 해주는 역할.
- 서비스 타입의 정의와 종류
    - 서비스 타입은 말 그대로 서비스의 타입.
    - 종류로는, ClusterIP, NodePort, LoadBalancer, ExternalName이 있다.
- EKS2048이 뭐야?
    - Amazon Elastic Kubernetes Service(EKS)에서 2048 게임을 배포하여 EKS 기능을 학습하고 실습하는데 사용도는 샘플 애플리케이션
- Job vs Cron Job
    
    [Crone Job과 Message Broker를 활용한 데이터 배치 처리 실습](https://www.notion.so/Crone-Job-Message-Broker-19f4047c5d6d810e9dbbc44585fce6c1?pvs=21)
    
    - 둘 다 특정 작업을 실행하고 관리하는 컨트롤러. 그러나 목적과 실행 방식이 다름.
    - Job: 한 번 실행되고 완료되는 작업을 관리하는 쿠버네티스 오브젝트
    - Cron Job: 일정한 주기로 반복실행하는 쿠버네티스 오브젝트.
    - 데이터 배치 처리에 적합함.
- 잉그레스의 정의
    - 쿠버네티스에서 클러스터 외부의 HTTP(S) 트래픽을 내부 서비스로 라우팅하는 역할을 하는 리소스
- 잉그레스가 있으면 서비스가 필요 없나?
    - 아니, 잉그레스는 트래픽을 직접 Pod로 보내지 못하고 반드시 서비스를 통해야 한다. 따라서 잉그레스는 외부 트래픽을 쿠버네티스 내부의 서비스로 전달하는 역할만 수행할 뿐, 서비스 없이 단독으로 존재할 수 없음.
- 왜 디플로이먼트 상태를 확인하는 코드가 kubectl get deploy,po야?
    - deploy: deployment의 단축 이름. 배포 리소스를 의미.
        - 그렇다면 여기서 deployment는 배포 리소스이지 컨트롤러가 아니란 말인가? 그렇다. deployment 배포 리소스는 애플리케이션의 원하는 상태를 선언적으로 정의하는 객체이지, deployment 컨트롤러와는 다르다.
    - po: pods의 약어. pods는 워낙 자주 쓰는 자원이기 때문에 네 글자 밖에 안 됨에도 이렇게 단축했다고 한다. 그런데 사실 이해가 안 된다. pods나 po나. 언어적 사회성을 고려하지 않은 형편없는 단축어다.
- Manifest의 정의
    - 클러스터 리소스의 구성과 상태를 선언적으로 기술한 **설정 파일**
    - 그렇다면 리소스를 생성할 때 가장 먼저 manifest를 작성해야겠군? 맞다.

# 테이블: 서비스 타입

| 서비스 타입 | 네트워크 범위 | 외부접근 가능 여부 |
| --- | --- | --- |
| **ClusterIP** | 클러스터 내부에서만 접근 가능 (기본값) | ❌ 불가능 |
| **NodePort** | 노드의 특정 포트를 통해 외부에서 접근 가능 | ✅ 가능 |
| **LoadBalancer** | 클라우드 제공자의 로드밸런서를 사용하여 외부에 서비스 노출 | ✅ 가능 (클라우드 환경) |
| **ExternalName** | 외부 DNS로 서비스 요청을 리디렉션 | ✅ 가능 (DNS 기반) |

# 그림들

![IMG_0411.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0411.png)

![IMG_0412.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0412.png)

![IMG_0413.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0413.png)

![IMG_0414.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0414.png)

![IMG_0415.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0415.png)

![IMG_0416.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0416.png)

![IMG_0417.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0417.png)

![IMG_0418.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0418.png)

![IMG_0419.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0419.png)

![IMG_0420.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0420.png)

![IMG_0421.PNG](%5BKubernetes%5D%2019f4047c5d6d8017b736fc8fcd4c49eb/IMG_0421.png)

# 이름들

- 쿠버네티스 API: 쿠버네티스 조작은 모두 API로 동작한다.CLI kubectl을 통해 마스터 노드 kube-apiserver에게 YAML 혹은 JSON 형식으로 전송한다.
- 오브젝트: K8s 클러스터 내부의 엔티티로, 파드, 컨트롤러, 서비스 등의 인스턴스를 의미. 쿠버네티스 API의 리소스 종류에 맞게 설정 및 생성된다. (그럼 모든 파드, 컨트롤러, 서비스는 공통 조상을 가지는가? 아니, 객체지향 개념은 아니다. 오브젝트들이 동일한 API에서 kind만 달리 설정된다는 의미다. 파드와 컨트롤러는 같은 메뉴판에 있는 파스타와 스테이크의 관계 같은 거지.)
- kube-system 네임스페이스: K8s 시스템 관련 기능을 하는 오브젝트들은 이 네임 스페이스에서 만들어짐.
- 쿠버네티스에서의 네임 스페이스: 오브젝트들은 kind 안에서 유일한 이름을 가져야 함.
- 워크로드: 컨테이너, 파드, 컨트롤러의 그룹. 컨테이너 실행을 관리하기 위해 사용.
- 컨테이너: 쿠버네티스에서는 컨테이너만 독자적으로 실행하는 것이 불가능하며, 파드 내에 실행해야 한다. 컨테이너 가동 시 이미지의 이름, 실행 명령어, 실행 인자, 환경 변수, 리소스 요청/상한 값이 설정된 대로 생성되겠지
- 파드: 컨테이너를 실행하기 위한 한 개 또는 여러 개의 컨테이너. 매우 중요한 오브젝트임
- 컨트롤러: 파드를 제어하는 오브젝트

# 탑다운1

목표: Minikube 클러스터를 로컬에서 실행하고, 간단한 Nginx 웹 서버를 배포한 후, 외부에서 접근할 수 있도록 서비스로 노출합니다.

## 1.  클러스터 생성

```bash
# 클러스터 1개 생성
minikube start
```

## 2. 메니페스트 작성

nginx-deployment.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        
```

## 3. Nginx Service Manifest 작성

nginx-service.yaml

```bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```

## 4. 메니페스트를 클러스터에 적용

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
```

## 5. 서비스 접근 확인

```bash
minikube service nginx-service
```

# 이슈: 지금 클러스터에 할당된 Elastic IP와 다른 주소로 서비스 url이 생성

## Phase1

환경: ubuntu 22, amd, EC2, minikube

로그: 없음

변경사항: nginx-serivce.yaml, nginx-deployment.yaml 생성 후 클러스터 적용

## Phase2-1

확인: 서비스 타입이 NodePort로 지정됨. NodePort 타입의 서비스는 클러스터 노드의 아이피와 지정된 포트를 통해 외부에 노출. 이 과정에서 Elastic IP가 아닌 기본 네트워크 인터페이스 아이피가 사용된 것으로 보임. 

externalIPs 필드를 활용하면 서비스로 외부 접근 가능하다고 함

시도: 

```bash
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
  externalIPs:
    - 43.200.202.183
```

결과 분석: 실패

클러스터 적용과 서비스 확인을 진행하였음

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml
minikube service nginx-service
```

```bash
ubuntu@ip-172-31-27-11:~/manifest$ minikube service nginx-service
|-----------|---------------|-------------|---------------------------|
| NAMESPACE |     NAME      | TARGET PORT |            URL            |
|-----------|---------------|-------------|---------------------------|
| default   | nginx-service |          80 | http://192.168.49.2:32594 |
|-----------|---------------|-------------|---------------------------|
🎉  Opening service default/nginx-service in default browser...
👉  http://192.168.49.2:32594
```

결과로 생성된 url에 externalURL은 반영되지 않음. 그냥 elastic IP를 바로 접속하면? 이것도 안 됨

## Phase2-2: 다른 이슈 발견

확인: nginx-service.yaml의 externalURL 설정은 서비스 url과 무관함. 이와 관련한 GPT 문답

> externalIPs 필드는 Kubernetes 클러스터 내에서 노드가 특정 IP(예: 탄력적 IP)로 들어오는 트래픽을 서비스로 전달하도록 **추가적인 라우팅 규칙**을 설정하는 역할을 합니다. 하지만 이 필드가 있다고 해서 Kubernetes API나 관련 도구가 자동으로 서비스 URL을 그 외부 IP로 변경해주지는 않습니다.
> 

노드 포트는 말 그대로 노드의 포트를 지정하는 서비스 타입. 노드는 여전히 호스트의 아이피로 설정될 수밖에 없다. 그래서 로드 밸런서를 쓰는 것이 일반적. 그러나 로드 밸런서는 추가 비용을 유발할 수 있다고 한다. 왜? 로드 밸런서 리소스는 따로 인스턴스처럼 생성이 된다. 그래서 직접 비활성화 시켜주는 조치가 필요하다. 일단 로드 밸런서로 서비스 타입을 변환해보자.

시도

nginx-service.yaml을 다음과 같이 변경

```bash
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

이를 클러스터에 적용하면 AWS 에서 제공하는 로드 밸런서가 생성된다. 이는 AWS 콘솔에서 확인 가능

결과분석: 솔루션이 제대로 적용안 됨.(솔루션 자체의 문제가 아님)

현재 externalIP가 pending 상태. AWS 콘솔에서도 아무것도 확인 안 됨. kubectl에서 매니페스트 적용한 것과 별개로 AWS 콘솔에서 별도의 조작을 해줘야 하는가? 그렇지는 않지만, AWS와 클러스터 통합 여부에 따라 안 될 수도 있다. 그런데 해결이 안 되고 있으니, 별도의 이슈를 생성하도록 한다.

# 이슈: 로드 밸런서가 생성되지 않음

## Phase1

환경: unbuntu, EC2, 

로그: 없음

변경사항: nginx-service.yaml을 통해 로드 밸런서 타입을 지정하였음.

## Phase2-1: 다른 이슈 발견

확인: AWS와 쿠버네티스 클러스터 연동 확인

```bash
# 각 노드 정보 표기. 연동이 제대로 됐으면 각 노드에 AWS 관련 정보 표기
kubectl get nodes -o wide
#  failure-domain.beta.kubernetes.io/zone 또는 
# topology.kubernetes.io/region 표기가 있는지 확인
kubectl describe node minikube | grep topology
```

관련 표기가 없음. AWS와 쿠버네티스 클러스터(minikube)가 연동되지 않았다. 

> Minikube는 로컬 개발 및 테스트 용도로 만들어진 Kubernetes 클러스터이기 때문에, AWS와의 통합(예: AWS 리소스 자동 프로비저닝, IAM 역할 연동, AWS 관련 노드 라벨 등)이 자동으로 이루어지지 않습니다.
> 

시도: 

그렇다면 minikube로는 EC2에서 로드 밸런서를 생성할 수 없군. 

결과분석: 원천 불가

그냥 노드 포트 타입으로 다시 바꾸고, 잠깐 인스턴스 아이피 보안 설정을 열어서 서비스 접속 테스트를 하고 다음 실습으로 넘어가도록 하자. → 이조차도 안 된다.

# 이슈: 노드포트 서비스로 접속이 안 됨

## Phase1

환경: ubuntu 22, amd, EC2, minikube, elastic IP 사용 중

로그: 없음

변경사항: nginx-serivce.yaml에 노드 포트 설정하여 클러스터에 적용, http://192.168.49.2:32594로 서비스 링크 생성 됨. AWS 인스턴스의 보안 그룹에서 해당 포트를 모든 TCP 요청에 대해 허가하도록 인바운드 규칙을 설정함. 그럼에도 [http://<할당](http://할당딘)된 탄력적IP>:노드포트 로 접속이 불가능함. 

## Phase2-2: 대안 발견(curl)

확인: Minikube의 특성을 생각해야 함.

> Minikube를 EC2 인스턴스에서 실행할 경우, Minikube 클러스터는 기본적으로 독자적인 가상 네트워크 내에서 동작합니다.  **서비스 링크**로 보이는 http://192.168.49.2:32594는 Minikube 내부에서 할당된 IP이며, EC2 인스턴스에 할당된 탄력적 IP와는 별개의 네트워크 인터페이스에 매핑되어 있습니다.
> 

→ 즉, 아무리 AWS 보안 설정을 지지고 볶아도 외부에서 Minikube 클러스터 서비스를 이용할 수는 없다는 소리. 대신 미니쿠베의 가상 네트워크로  [http://192.168.49.2:32594](http://192.168.49.2:32594/) 접속해야 한다. 그런데 이건 당연히 외부 접속이 불가능한 네트워크다. 

> 외부 접근을 원한다면 **minikube tunnel**을 사용하는 방법이 가장 일반적입니다.
> 

시도

1. 포트 설정: 그대로 놔 두면 됨
2. LoadBalancer로 서비스 타입 다시 변경
3. 터널은 계속 터미널을 점유하고 있어야 함

결과 분석:

이것도 안 된다. 미니쿠베로는 외부 접속이 안 되는 거 같다.

대신, curl로 EC2 호스트 인스턴스에서 접속하여 html을 확인할 수  있다. 일단은 이렇게 사용할 수 있다. 다음 단계 실습은, 미니쿠베로 외부 클라이언트 접속을 우선적으로 해보자. → 하긴 그래서 잉그레스가 있는 거지. 서비스의 아이피가 외부 호스팅이 가능해야 한다는 생각이 이상한 것이다.

# 탑다운2

목표: 다음은 실습 난이도를 한 단계 높여서 **Ingress 컨트롤러**와 **TLS 종료**, 그리고 **경로 기반 라우팅**을 사용하는 프로젝트 예시입니다.

## 1. 잉그레스 컨트롤러 설치

ingress 컨트롤러를 이렇게 설치할 수 있다.

```bash
minikube addons enable ingress
```

실행 확인

```bash
kubectl get pods -n ingress-nginx
```

## 2. 두 개의 애플리케이션 배포

### 1) Nginx Deployment and service

nginx-deployment.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

### 2) node.js 간단한 웹 배포

node-deployment.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-deployment
  labels:
    app: node-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        # 예시용 간단한 Node.js 이미지 (실제 테스트를 위해 간단한 이미지 필요)
        image: hashicorp/http-echo
        args:
          - "-text=Hello from Node.js app"
        ports:
        - containerPort: 5678
---
apiVersion: v1
kind: Service
metadata:
  name: node-service
spec:
  selector:
    app: node-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5678
  type: ClusterIP
```

## 3. Ingress 리소스 생성

### a) TLS 인증서 생성

> Minikube는 테스트 용도로 Self-Signed 인증서를 사용할 수 있습니다. 아래 명령어를 통해 인증서와 키를 생성합니다.
> 

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -out tls.crt -keyout tls.key \
  -subj "/CN=example.com/O=example.com"
```

인증서를 쿠버네티스 secret으로 생성

```bash
kubectl create secret tls tls-secret --key tls.key --cert tls.crt
```

### b) Ingress 리소스 생성

ingress.yaml

```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: advanced-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - example.com
    secretName: tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /nginx
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
      - path: /node
        pathType: Prefix
        backend:
          service:
            name: node-service
            port:
              number: 80
```

> 이 Ingress 설정은:
•	[example.com](http://example.com/) 도메인에 대해 TLS를 적용
•	/nginx 경로는 Nginx 서비스로 라우팅
•	/node 경로는 Node.js (http-echo) 서비스로 라우팅
> 

> 실제 테스트에서는 /etc/hosts 파일에 example.com을 Minikube IP(예: minikube ip)로 매핑하거나, 브라우저에서 해당 도메인으로 접속해 테스트할 수 있습니다.
> 

### 4. 리소스 적용 및 테스트

각 야멜 파일을 클러스터에 적용

```bash
kubectl apply -f nginx-deployment.yaml
kubectl apply -f node-deployment.yaml
kubectl apply -f ingress.yaml
```

리소스가 잘 배포됐는지 확인

```bash
kubectl get all
kubectl get ingress
```

Ingress에 할당된 아이피 확인

```bash
minikube ip
```

192.168.49.2

중요: 로컬 호스트 파일 수정

/etc/hosts 파일에 아래 항목 추가

```bash
<minikube_ip>   example.com
```

로컬 브라우저에서 접속 테스트→ EC2에서는 어떻게 하지?

[https://example.com/nginx](https://example.com/nginx) → curl [https://example.com/nginx](https://example.com/nginx) -k
[https://example.com/node](https://example.com/node)

## 의문점들

### Q1.

잉그레스 쓰는게 외부 클라이언트에서도 서비스 접속할 수 있게 해주는 거 아닌가? 미니쿠베라는 이유로 잉그레스 써도 외부 클라이언트 접속이 안 되면, 미니쿠베로는 대체 뭘 할 수 있는가? 터널 안 쓰면 아무것도 못 하는 거 아닌가?

A.

Minikube는 **로컬 개발 및 테스트용**으로 만들어진 도구이므로,

실제 **퍼블릭 클라우드 환경**(예: AWS, GCP)에서의 Ingress처럼 외부 IP나 로드 밸런서를 자동으로 생성해 주지 않습니다.

결과적으로 “Ingress를 정의하면 외부에서도 접근 가능”이라는 Kubernetes 일반론이, **Minikube 환경**에서는 제한적으로만 적용됩니다.

**1. 왜 Minikube에서 Ingress를 써도 외부 접속이 어려운가?**

1.	**Minikube의 네트워크 구조**

•	Minikube는 내부적으로 VM이나 컨테이너 네트워크를 사용해 로컬 환경에서 Kubernetes를 실행합니다.

•	“외부 IP”가 없는 상태이므로, Ingress를 만들었다 해도 외부(인터넷)에서 직접 접속할 수 없습니다.

2.	**LoadBalancer 연동 부재**

•	클라우드 환경에서는 Ingress 컨트롤러가 클라우드의 로드 밸런서를 만들고 퍼블릭 IP를 할당합니다.

•	Minikube에는 그런 기능이 없어서, Ingress를 정의해도 “로컬 전용”으로만 동작합니다.

**2. 그렇다면 Minikube로 Ingress를 쓰는 이유는?**

1.	**개발/테스트 환경 시뮬레이션**

•	실제 클라우드 환경에서 Ingress를 사용하기 전, 로컬 환경에서 Ingress 리소스(YAML) 구성을 미리 시험해 볼 수 있습니다.

•	예: 경로 기반 라우팅, TLS 설정 등을 로컬에서 빠르게 검증할 수 있습니다.

2.	**여러 서비스 라우팅 테스트**

•	한 도메인(또는 IP) 아래 여러 서비스(/app1, /app2 등)로 트래픽을 분기하는 로직을 로컬에서 확인할 수 있습니다.

**3. Minikube에서 외부 접근하려면 어떻게 해야 하나?**

**A. minikube tunnel 사용 (LoadBalancer 타입 서비스 테스트)**

•	**LoadBalancer 타입**의 서비스를 생성하고,

•	minikube tunnel 명령어를 실행하면,

Minikube가 로컬 머신(또는 EC2 인스턴스)의 네트워크 인터페이스를 통해 외부에서 접근 가능한 포트를 열어줍니다.

**장점:** 로컬 개발 환경에서 LoadBalancer 테스트 가능

**단점:** tunnel 프로세스를 계속 실행해 두어야 하며, 도메인 기반 접근은 /etc/hosts 등 추가 설정이 필요

**B. NodePort + SSH 포트 포워딩 (또는 NAT)**

•	NodePort로 서비스를 노출한 뒤, SSH 포트 포워딩을 통해 로컬 브라우저에서 테스트할 수 있습니다.

•	혹은 Minikube가 동작하는 호스트(EC2 등)에서 OS 레벨 NAT를 설정해 외부 IP/포트를 NodePort로 매핑할 수도 있습니다.

**C. /etc/hosts로 도메인 매핑 (로컬 테스트)**

•	로컬 PC나 Minikube 실행 호스트의 /etc/hosts를 수정해 example.com을 Minikube IP로 매핑하면, 로컬에서 해당 도메인으로 접근 가능합니다.

•	여전히 **인터넷 전체**에는 노출되지 않고, 해당 호스트(혹은 로컬 PC) 내부에서만 유효합니다.

**4. 정리**

•	**Minikube**는 프로덕션 환경에서 외부 트래픽을 받아들일 용도로 설계된 것이 아니라, **로컬에서 K8s를 학습하고 테스트**하기 위한 도구입니다.

•	Ingress를 사용해도 **“로컬(또는 Minikube 호스트) 내부”**에서만 동작하며, 외부 접근이 필요하면 **추가적인 터널링**이나 **포트 포워딩** 설정을 해야 합니다.

•	실제로 외부(인터넷)에서 접근 가능한 Kubernetes 환경을 만들려면, **EKS, GKE, AKS** 같은 클라우드-managed Kubernetes나, 자체 클러스터에 **cloud-controller**(AWS, OpenStack 등)를 설정해야 합니다.

결국, **Minikube에서 Ingress는 “기능적 학습”을 위한 것이지, 실제 퍼블릭 접근을 위한 솔루션이 아니다**라고 이해하시면 됩니다.

따라서 터널링이나 포트 포워딩 같은 추가 작업 없이 “외부에서 바로 접근”하기는 어렵고,

이는 Minikube의 근본적인 설계(로컬 테스트용) 때문입니다

### Q2

1. 개발/테스트 환경 시뮬레이션
•	실제 클라우드 환경에서 Ingress를 사용하기 전, 로컬 환경에서 Ingress 리소스(YAML) 구성을 미리 시험해 볼 수 있습니다.
•	예: 경로 기반 라우팅, TLS 설정 등을 로컬에서 빠르게 검증할 수 있습니다.
2. 여러 서비스 라우팅 테스트
•	한 도메인(또는 IP) 아래 여러 서비스(/app1, /app2 등)로 트래픽을 분기하는 로직을 로컬에서 확인할 수 있습니다.

이런 기능들은 잉그레스를 써야만 할 수 있는 것들인가보지?

A

네, 맞습니다.
Ingress는 여러 서비스에 대한 경로 기반 라우팅, TLS 종료, 도메인 기반 라우팅 등을 통합적으로 관리할 수 있게 해 주므로,
이런 기능들을 효율적으로 구현하고 테스트하려면 Ingress를 사용하는 것이 일반적입니다.

물론 NodePort나 LoadBalancer 서비스를 각각 따로 설정하여 일부 기능을 구현할 수는 있지만,
Ingress는 중앙 집중식으로 여러 라우팅 규칙과 TLS 설정을 한 곳에서 관리할 수 있는 장점이 있습니다.

# 이슈: nginx-deployment.yaml 클러스터 적용 오류

## Phase1

환경: EC2, AMD, Ubuntu, minikube

로그: error: error validating "nginx-deployment.yaml": error validating data: failed to download openapi: Get "[https://192.168.49.2:8443/openapi/v2?timeout=32s](https://192.168.49.2:8443/openapi/v2?timeout=32s)": dial tcp 192.168.49.2:8443: connect: no route to host; if you choose to ignore these errors, turn validation off with --validate=false

변경사항: nginx-deployment.yaml 작성 후 kubectl apply -f nginx-deployment.yaml 실행

## Phase2-1: 해결

확인: 192.168.49.2:8443이 무슨 아이피인지 모르겠다. 클러스터 아이피인가? 일단 로그 자체의 의미는 야멜 파일을 클러스터에 적용하는 과정에 validation에서 문제가 생겼다는 것으로 보인다. 

즉, 내부에서 라우팅이 안 됐던 것이다. minikube status로 확인 결과 컨트롤 플레인이 stop 상태인 것을 확인.

시도: minikube start

결과 분석: 성공. 

# 탑다운3

다음은 쿠버네티스의 다양한 고급 기능(다중 서비스, Ingress, TLS, 네트워크 정책, Persistent Volume, Horizontal Pod Autoscaler 등)을 활용하는 고난이도 실습 예시입니다.

## 1. 네임스페이스 생성

모든 리소스를 격리할 네임스페이스를 생성합니다.

```markdown
apiVersion: v1
kind: Namespace
metadata:
  name: advanced-demo
```

```markdown
kubectl apply -f namespace.yaml
```

## 2. ConfigMap, Secret 생성

백엔드 서비스에서 사용할 설정 값과 데이터베이스 접속 정보를 관리

```markdown
apiVersion: v1
kind: ConfigMap
metadata:
  name: backend-config
  namespace: advanced-demo
data:
  LOG_LEVEL: "info"
  FEATURE_FLAG: "true"
```

```markdown
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: advanced-demo
type: Opaque
data:
  # base64 인코딩된 값으로 입력 (예: echo -n "mypassword" | base64)
  DB_PASSWORD: bXlwYXNzd29yZA==
```

```markdown
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
```

## 3. DB deploy(including persistent storage)

```markdown
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: db-pvc
  namespace: advanced-demo
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
  namespace: advanced-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASSWORD
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-data
        persistentVolumeClaim:
          claimName: db-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: mysql-service
  namespace: advanced-demo
spec:
  selector:
    app: mysql
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306
  clusterIP: None  # Headless service (DB 클러스터 내 연결 테스트용)
```

```markdown
kubectl apply -f db-deployment.yaml
```

## 4. 백엔드 서비스 배포

backend-deployment.yaml

```markdown
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  namespace: advanced-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: nginxdemos/hello # 데모용 이미지
        ports:
        - containerPort: 8080
        env:
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef:
              name: backend-config
              key: LOG_LEVEL
        - name: FEATURE_FLAG
          valueFrom:
            configMapKeyRef:
              name: backend-config
              key: FEATURE_FLAG
        - name: DB_HOST
          value: mysql-service.advanced-demo.svc.cluster.local
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASSWORD
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
  namespace: advanced-demo
spec:
  selector:
    app: backend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: ClusterIP
```

클러스터 적용하기

## 5. 프론트엔드 서비스 배포

```markdown
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: advanced-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
  namespace: advanced-demo
spec:
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: ClusterIP
```

```markdown
kubectl apply -f frontend-deployment.yaml
```

## 6. 네트워크 정책

```markdown
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-backend-to-db
  namespace: advanced-demo
spec:
  podSelector:
    matchLabels:
      app: mysql
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
    ports:
    - protocol: TCP
      port: 3306
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: advanced-demo
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 8080
```

```markdown
kubectl apply -f networkpolicy.yaml
```

## 7. Ingress와 TLS 구성

### **a. TLS 인증서 생성 (Self-Signed)**

```markdown
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -out tls.crt -keyout tls.key \
  -subj "/CN=advanced-demo.local/O=advanced-demo"
```

쿠버네티스 시크릿 생성

```markdown
kubectl create secret tls tls-secret --key tls.key --cert tls.crt -n advanced-demo
```

### **b. Ingress 리소스 작성**

```markdown
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: advanced-ingress
  namespace: advanced-demo
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
    - advanced-demo.local
    secretName: tls-secret
  rules:
  - host: advanced-demo.local
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: backend-service
            port:
              number: 80
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-service
            port:
              number: 80
```

이것도 클러스터에 적용.

## 8. HPA 설정

예를 들어, 백엔드 서비스에 대해 CPU 사용량이 50%를 초과하면 자동으로 Pod 수를 증가시키도록 설정합니다.

```markdown
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: backend-hpa
  namespace: advanced-demo
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: backend
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

```markdown
kubectl apply -f hpa.yaml
```

## 9. 테스트 및 검증

### 모든 리소스 상태 확인

```markdown
kubectl get all -n advanced-demo
kubectl get ingress -n advanced-demo
```

### 잉그레스 접속 테스트

/etc/hosts 파일에 minikube ip와 advanced-demo.local 매핑 추가

```markdown
<minikube ip> advanced-demo.local # 192.168.49.2
```

[https://advanced-demo.local](https://advanced-demo.localfh)로 접속 시 기본 경로는 프론트엔드로, /api 경로는 백엔드 api 서비스로 라우팅.

### HPA 테스트

백엔드 서비스에 부하를 발생시키거나, kubectl top pod -n advanced-demo 명령어로 CPU 사용량을 모니터링 하여 HPA 동작 확인

# 이슈: 클러스터 잉그레스 connection refused

## Phase1

환경: AMD, Ubuntu, Ec2, minikube
로그: curl: (7) Failed to connect to advanced-demo.local port 443 after 0 ms: Connection refused

변경사항: 

1. backend-deployment, conifgmap, db-deployment, frontend-deployment, hpa, ingress, namespace, networkpolicy, secret 야멜 파일들을 통해 리소스를 설정하고 클러스터에 적용.
2. /etc/hosts 파일에 minikube ip를 advanced-demo.local에 할당. `192.168.49.2 advanced-demo.local`

## Phase2-1

확인:	 

1. 네임스페이스 advanced-demo에 Ingress 리소스가 올바르게 생성되었는지 확인: 정상
    1. kubectl get ingress -n advanced-demo
        1. advanced-ingress   <none>   advanced-demo.local             80, 443   22m
2. Minikube에서 Ingress 애드온이 활성화되어 있고, Ingress 컨트롤러(NGINX Ingress)가 실행 중인지 확인: 비정상(ingress
    1. kubectl get pods -n ingress-nginx
        1. No resources found in ingress-nginx namespace.
        2. 잉그레스 애드온을 클러스터에 따로 활성화시켜야 하는데 아까 클러스터를 삭제하고 다시 시작한 후 활성화를 안 했다.
    2. 의문: 잉그레스는 ingress-nginx라는 별도의 네임스페이스를 쓰는가?
        1. 네, 보통 **ingress-nginx** 컨트롤러는 별도의 네임스페이스(대개 ingress-nginx)에 배포됩니다. 예를 들어, Minikube에서 Ingress 애드온을 활성화하면, 기본적으로 ingress-nginx 네임스페이스에 컨트롤러 Pod들이 생성됩니다. 물론, 설치 방법이나 환경에 따라 네임스페이스 명은 달라질 수 있습니다

시도

```markdown
minikube addons enable ingress
kubectl get pods -n ingress-nginx
```

정상적으로 추가됨. 

결과 분석: 성공

- 결과 보기
    
    curl [https://advanced-demo.local](https://advanced-demo.local/) -k
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    <style>
    html { color-scheme: light dark; }
    body { width: 35em; margin: 0 auto;
    font-family: Tahoma, Verdana, Arial, sans-serif; }
    </style>
    </head>
    <body>
    <h1>Welcome to nginx!</h1>
    <p>If you see this page, the nginx web server is successfully installed and
    working. Further configuration is required.</p>
    
    <p>For online documentation and support please refer to
    <a href="[http://nginx.org/](http://nginx.org/)">nginx.org</a>.<br/>
    Commercial support is available at
    <a href="[http://nginx.com/](http://nginx.com/)">nginx.com</a>.</p>
    
    <p><em>Thank you for using nginx.</em></p>
    </body>
    </html>