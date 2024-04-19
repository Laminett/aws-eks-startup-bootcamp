- HPA
- VPA
  - safety margin

- 오토스케일링 그룹
  - AZ단위

노드그룹단위

- karpenter
  - 노드 프로비저닝
  - 노드 크기 조정
  - pending pod -> karpenter -> ec2 fleet
    - 기존 pending pod -> ca -> asg -> ec2 api
  - pending pod -> scheduler -> existing capacity -> pod binding
  - 딱 맞는 ec2를 생성할 수 있음
  - 쿠버네티스 네이티브
  - 쿠버네티스 스케줄러 역할은 하지 않음
  - batch window
  - spring actuator request count -> custom metric으로 설정할 수 있음
  - KEDA

- 쿠버네티스 스케줄러
  - filter
  - preScore
  - score


