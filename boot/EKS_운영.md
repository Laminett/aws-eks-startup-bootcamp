- 마스터 노드
  - 컴포넌트
- 워커노드
  - 오브젝트
    - kubelet: 에이전트 형식으로 api와 통신
    - pod: kubelet을 통해 생성됨
    - service: kube-proxy(iptables)를 통해 생성

age간 트래픽
- ec2 노드간 트래픽

alb 타켓을 선택할 수 있음
- 인스턴스 선택이 아닌 ip를 선택할 수 있음
- ip대상으로 팟을 지정할 수 있음 -> alb 팟과 직접 통신
  - iptables룰을 타지않음
  - 전송 비용이 무료 
  - aws loadbalance addon 추가 설정

- topology aware hints
  - 같은 가용영역에 해당 팟이 있으면 가용역역으로 트래픽을 전송하게 되어 가용영역간 트래픽이 발생하지 않게 설정할 수 있음
  - 팟의 평균치를 통해 오토 스캐일링이 발생하는데 특정 팟만 임계치가 높아지면 발생하지 않음
  - 사용하기 위해서 모든 팟에 골고루 리소스가 분산되게 설정하는 것이 중요

- coreDNS
  - 각 컨테이너는 kubelet에 의해 /etc/resolv.conf를 상속
  - 설정된 ndot의 개수가 아니면 search에 설정된 도메인들을 추가적으로 붙여서 coreDNS에 계속해서 질의를 함
  - fqdn, ndot
  - 기본적으로 eks클러스터에 2개의 복제본으로 배포됨
  - 확장
    - CPA: 클러스터 규모에 따른 확장
    - HPA: 팟 임계치에 따른 확장
  - node local dns cache
  - node에는 처리할 수 있는 트래픽 임계치가 있음 해당 임계치가 넘으면 패킷이 드랍되어 처리가 안될 수 있음

- node 선정 기준
  - affinity 선호 팟
  - request/limit linux soft limit / hard limit
  - podAntiAffinity설정으로
    - 팟이 각 노드에 균등하게 배포될 수 있게 할 수 있음
    - 모든 노드에 팟이 전부 있다면 스케줄러에 의해 배포됨
  - topologySpreadConstratints
    - 최대 n개의 pod 차이수 차이를 허용함
  - PodDisruptionBudget
    - 최소한 유지하는 팟을 설절

- 노드확장
  - 수집된 지표를 통해 HPA Controller가 ReplicaSet을 수정하여 새로운 팟을 노드에 배포
  - pending pod이 생겼을 때 cluster auto scaler가 감지하고 aws auto scaling group에서 노드가 생성되고 생성된 노드에 팟을 배포
  - 오버프로비저닝 패턴
    - pause팟(아무 일도 하지 않는) 노드에 배포된 상태로
    - 팟 우선순위 설정으로 펜딩 팟을 에빅트하고 해당 공간으로 팟이 배포
    - 노드를 사용하지 않으면 노드가 제거되기 때문에 노드를 계속 유지하는 방식
    - 