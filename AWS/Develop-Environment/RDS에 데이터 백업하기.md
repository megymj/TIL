# RDS에 데이터 백업하기

> 참고한 교재: AWS Certified Solutions Architect Associate



### 설계

* 보통 하나의 Region에 두 개의 Subnet(Public Subnet, Private Subnet)을 만든 뒤, EC2는 Public Subnet, RDS는 Private Subnet으로 하는 것이 원칙이지만, 지금은 RDS에 데이터를 백업하는 용도로만 사용할 것이므로 Public Subnet에 두기로 함
* AWS 공식 문서: "Amazon RDS DB 인스턴스를 시작하려면 RDS DB 서브넷 그룹에 두 개 이상의 서브넷이 포함되어야 합니다. 이러한 서브넷은 동일한 AWS 리전의 다른 가용 영역에 있어야 합니다." -> 따라서 가용 영역 2개에 각각 Public Subnet을 구성함

<img width="987" alt="design" src="https://user-images.githubusercontent.com/80478750/198871272-e0767e2c-a6e8-4523-9fea-9919cdd5de85.png">



### 1. VPC(Virtual Private Cloud)

> reference link: [VPC란?](https://blog.kico.co.kr/2022/03/08/aws-vpc/)

* **VPC**는 **독립적인 가상의 네트워크 공간**으로 사용자의 설정에 따라 자유롭게 구성할 수 있는 공간을 의미한다. 따라서 사용자는 서브넷 생성, 라우팅 테이블, 네트워크 게이트웨이 구성 등 네트워킹 환경을 사용자가 원하는 대로 완벽하게 제어할 수 있다.
* 또한 클라우드를 public / private 영역으로 논리적으로 분리할 수 있게 해준다. 즉, VPC 내에 인터넷 접근을 위한 Public Subnet을 생성하고 격리된 접근을 위한 Private Subnet을 생성할 수 있다. 
* 한 번 생성된 VPC는 주소의 크기를 변경할 수 없으며, 만약 더 큰 주소가 필요한 경우 새로운 VPC를 생성하고 이에 따라 애플레케이션도 migration해야 하는 불편함이 발생할 수 있다. 
* VPC는 리전에 제한되고, 다른 리전으로 확장할 수 없으며, VPC 내에서 동일 리전의 AZ를 포함시킬 수 있다. 



### 2. Route table 존재 이유?

* 모든 Subnet을 Route table을 지닌다. 예를 들어, 특정 VPC의 Subnet이 Route table에 IG(Internet Gateway)를 포함하고 있다면, 해당 Subnet은 인터넷 액세스 권한 및 정보를 지닌다. 반대로 Subnet에 IG 정보가 없다면, 해당 서브넷과 연결된 서버로는 인터넷에 접근할 수 없다. 
  * 아래 사진은 Route table에 IG를 추가하는 과정으로, 만약 IG를 추가하지 않고 3번에서 RDS를 접속하려고 하니 접속이 되지 않는 것을 확인할 수 있었다. 

<img width="1382" alt="route-table" src="https://user-images.githubusercontent.com/80478750/198816166-a317b3c6-4c46-442f-9a3d-9343704818fc.png">



* 각각의 Subnet은 항상 Route table을 지니고 있어야 하지만, 하나의 Route table 규칙을 여러 개의 Subnet에 연결하는 것은 가능하다. Subnet을 생성하고 별도의 Route table을 생성하지 않으면 클라우드가 자동으로 VPC의 메인 Route table을 연결한다. 
* IG가 Subnet Router에 attach 되있지 않으면, 해당 Subnet 내에서 호스팅되는 리소스는 인터넷에 접근할 수 없다. 이런 형태의 Subnet을 Private Subnet이라고 한다.





### 3. RDS 생성 및 접속

* MySQL로 RDS를 생성한다. 생성할 때 아래 옵션을 `publicly accessible` 로 설정해야 로컬에서 접속이 가능하다

<img width="890" alt="public" src="https://user-images.githubusercontent.com/80478750/198814915-ee8f75b0-1093-4442-ae9a-90dc069c7a28.png">

* 생성 이후 VPC security groups로 가서 Inbound Rules을 수정한다.
  * 현재 포트가 3306이므로, 3306 포트를 열어준다.
* 로컬에서 접속

```bash
mysql -u [user name] -p -h [rds endpoint]

# 혹은 MySQL Workbench를 이용해서 접속
```

* mysqldump 명령어를 사용해 데이터 복원: [내 GitHub > mysqldump](https://github.com/Moojun/TIL/blob/main/MySQL/Backup.md)



### 4. lambda를 사용하여 RDS 중단시키기

* RDS의 경우 EC2처럼 계속 중지가 불가능하며, `Stop temporaily` 를 사용해서 중단하면 7일 뒤에 자동으로 다시 RDS가 Start 되는 것을 확인할 수 있다.
* 나의 경우 RDS에 데이터를 백업한 뒤, 오랜 기간동안 중단시켜 놓는것이 목적이므로 lambda를 사용해서 RDS를 오랜 기간동안 중지시키거나, 혹은 특정 시간(e.g 화요일 12시 ~ 15시) 에만 켜 놓으려고 하였다. 

-> ***추후 작성 예정***

