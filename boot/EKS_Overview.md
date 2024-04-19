## EKS Overview
- ECS, EKS 차이
- EKS 특징
  - k8s소스를 수정하지 않고 구동
  - 4개의 k8s 마이너 버전을 지원
  - 데이터 플레인: 팟이 배포되는 노드
  - 컨트롤 플레인: api 서버
  - etcd클러스터도 분리해서 lb하여 동작하게 할 수 있다.
  - api서버는 aws에서 관리하는 vpc에 배포됨

- eksctl, CloudFormation & AWS CDK & Terraform을 사용해 배포할 수 있음
- 온프로미스에서 eks 클러스터를 실행할 수 있음.
  
- Kubernetes API 서버 엔드포인트 설정
- kubectl, data plane에서 접속, control plane에서 data plane으로 요청을 보내는 방법으로 managed vpc에 접근할 수 있음
- private한 통식을 위해선 private링크로 생성해야 클러스터와 노드를 정상적으로 연결할 수 있음
- private한 클라우드는 오직 bastion host를 통해서만 접속할 수 있게 설정하여 사용할 수 있음
- eks api를 사용해서 클러스터에 접근할 수 있음
  - 기존에 configmap에 선언된 롤만 클러스터에 접근 가능

- control plane은 로깅 설정을 해야 로그를 확인할 수 있음.

- data plane
  - self-managed node group
    - custom ami
  - managed node group
    - aws에서 처리
  - node type
    - ec2
    - fargate

- networking
  - overlay network
  - vpc cni
    - vpc서브넷 대역을 pool로 관리하고 pod에게 ip를 할당함
    - 동일한 대역을 설정하여 overlay 가 아닌 일반 vpc 네트워킹이 가능하게 함

- storage
  - csi driver
    - eks에 영구 볼륨을 사용할 수 있음
      - 동적 생성 방식 드라이버를 지정하는 storageclass 사전 정의
      - 요청에 의해 볼륨이 할당되는 방식
    - ebs는 region별로 
    - efs

- security
  - authentication
    - 쿠버네티스는 외부 인증을 사용함
  - authorization
  - admission control
    - 관리자가 임의의 롤을 설정할 수 있음
  - service account
    - irsa를 사용하면 ec에 롤이 부여되어 불필요한 팟에게 권한을 설정하게 하지 않고 각 팟마다 권한을 부여해서 aws자원에 접근할 수 있게 할 수 있음.
    - 각 팟마다 service account를 생성하고 서비스 어카운트에 iam롤과 정책을 설정해서 자원에 접근하도록 하는게 권장사항

