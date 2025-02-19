# About Ingress
## Q
클러스터가 쿠버네티스에서 가장 큰 단위였으니, 결국 퍼블릭이 접근하게 되는 대상이며, '클러스터를 위한 라우터'라고 생각하면 될 거 같네.

## A
정확해! 🚀

쿠버네티스에서 클러스터(Kubernetes Cluster) 는 가장 큰 단위이고, 그 클러스터 내부에는 여러 노드(Node) 가 있으며, 노드 안에는 파드(Pod) 가 실행되고 있지. 하지만 기본적으로 쿠버네티스 클러스터 내부의 서비스들은 외부에서 직접 접근할 수 없어.

그래서 Ingress를 “클러스터를 위한 라우터” 라고 생각하면 이해하기 쉽다!
Ingress는 쿠버네티스 클러스터 안에 있는 서비스들로 들어오는 트래픽을 적절한 경로로 보내주는 역할을 하니까, 클러스터의 공용 진입점(Public Entry Point) 이라고 볼 수도 있어.

이렇게 정리할 수 있어:
	•	Ingress는 클러스터 레벨에서 동작하며, 퍼블릭 트래픽을 내부 서비스로 라우팅하는 역할을 함.
	•	로드밸런싱, URL 기반 라우팅, HTTPS 지원 등을 제공하여 “쿠버네티스 클러스터를 위한 네트워크 게이트웨이” 역할을 함.
	•	Ingress Controller가 실제로 이를 실행하며, 대표적으로 Nginx Ingress Controller, Traefik 등이 있음.

👉 즉, Ingress = 쿠버네티스 클러스터를 위한 “라우터” 라고 보면 아주 적절한 비유야! 🎯