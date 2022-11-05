# NACL vs Security Group

> reference link
>
> 1. [javaTpoint: aws nacl vs security group](https://www.javatpoint.com/aws-nacl-vs-security-group)
> 2. [AWS Docs: default VPC](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/default-vpc.html)
> 3. [AWS Docs: VPC Network ACLs](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/vpc-network-acls.html)
> 4. [medium blog: NACL vs Security Groups](https://medium.com/awesome-cloud/aws-difference-between-security-groups-and-network-acls-adc632ea29ae)



* 처음 AWS 계정을 생성하면, 각 AWS 리전이 기본 VPC가 존재한다. 기본 VPC는 각 가용 영역의 퍼블릭 서브넷, 인터넷 게이트웨이 및 DNS 확인 활성화 설정과 함께 제공된다. 

* 라우팅 테이블에 인터넷 게이트웨이가 기본적으로 추가되어 있기 때문에, EC2 인스턴스를 생성하면 바로 외부에서 접속이 가능하다. 
* 또한, `NACL(Network Access Control List)` 역시 기본으로 생성되어 있는 것을 확인할 수 있었다. 아래 사진은 내 계정의 default NACl의 모습이다.

<img width="1149" alt="nacl" src="https://user-images.githubusercontent.com/80478750/199626836-e3a1c20d-712a-4ec6-b704-cd609b97fb87.png">



* 아래 사진은 default NACl에 설정되어 있는 Inbound rules 이다. 
  * '*'로 되어있는 규칙은, 패킷이 번호가 매겨진 다른 어떤 규칙과도 일치하지 않을 경우에는 거부하도록 되어 있다. 이 규칙을 수정하거나 제거할 수 없다. 

<img width="1100" alt="nacl2" src="https://user-images.githubusercontent.com/80478750/199626832-092687bd-dc4e-4192-ae7c-fc5fd54cb619.png">





<br>



### Difference b/w Security Group and NACL

<img width="465" alt="nacl img" src="https://user-images.githubusercontent.com/80478750/199628900-ace5600a-0c30-40ab-b962-769e465fed1b.png">




||Security Group|Network ACL|
|:---|:---|:----|
|**Rules: Allow or Deny**|Security group supports **allow rules only** (by default all rules are denied). e.g. You cannot deny a certain IP address from establishing a connection.|Network ACL supports **allow and deny** rules. By deny rules, you could explicitly deny a certain IP address to establish a connection. example: Block IP address 123.201.57.39 from establishing a connection to an EC2 Instance.|
|**State**|Security groups are **stateful**. This means any changes applied to an incoming rule will be automatically applied to the outgoing rule. e.g. If you allow an incoming port 80, the outgoing port 80 will be automatically opened.|Network ACLs are **stateless**. This means any changes applied to an incoming rule will not be applied to the outgoing rule. e.g. If you allow an incoming port 80, you would also need to apply the rule for outgoing traffic.|
|**Scope**|It is associated with an EC2 instance.|It is associated with a subnet.|
|**Rule Process Order**|All the rules are evaluated before deciding whether to allow the traffic.|Rules are evaluated in order, starting from the **lowest number**.|
|**Occurrence**|Instance can have multiple Security groups.|Subnet can have only one NACL.|



#### 그 외 특징들

##### 1. NACL

* 1개의 VPC에 최대 200개까지 생성 가능하다.
* 1개의 NACL에 인바운드, 아웃바운드 각각 20개까지 등록 가능하다.
* NACL은 여러 개의 Subnet에 적용이 가능하다. 





