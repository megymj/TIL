# Software Engineering
## Chapter 2 Software Processes

**Topics**

- Software process models
- Process activities
- Coping with change

## The software processes 
- A structured set of activities required to develop a software system.
- Many different software processes but all involve:
    - `Specification` – defining what the system should do;
    - `Design and implementation` – defining the organization of the system and implementing the system;
    - `Validation` – checking that it does what the customer wants;
    - `Evolution` – changing the system in response to changing customer needs. 
- A software process model is an abstract representation of a process. It presents a description of a process from some particular perspective.
```
Validation: 정말로 잘 만들었는지 확인하는 과정(꼭 필요)
Evolution: 과거-유지보수, 현재-소프트웨어 자체가 회사의 중요한 자산인 경우가 많다. 
따라서, 유지보수하며 땜빵하듯이 쓰고 다음 것을 새로 만드는 것이 아니라, 끊임없이 진화시켜가며 쓴다. 
```

## Software process descriptions
- Process descriptions: Activities + Ordering.
- Process descriptions may also include:
    - Products, which are the outcomes of a process activity;
    - Roles, which reflect the responsibilities of the people involvedin the process;
    - Pre-and post-conditions, which are statements that are true before and after a process activity has been enacted or a
       product produced.

## Plan-driven and agile processes
- Plan-driven processes
    - All of the process activities are planned in advance and progress is measured against this plan.
- Agile processes
    - planning is `incremental` and it is easier to change the process to reflect changing customer requirements.
- In practice, most practical processes include elements of `both plan-driven and agile approaches.`
- There are ***no right or wrong software processes***.

```
Plan-driven processes
- 과거에 일반적인 방식이었다
- 사전에 미리 계획을 짜고, 그것을 기반으로 개발하는 방식
- 계획에서 벗어나는 것을 하기가 힘들다

Agile processes
- 항상 좋은 것이 아니다. 적합할 때가 있고, 그렇지 않을 때가 있다. 현재 많은 소프트웨어에서 agile process를 도입하고 있음
```

# Software process models

* `The waterfall model - plan-driven`
    - Separate and distinct phases of specification and development.
* `Incremental development - plan-driven or agile`
    - Specification, development and validation are interleaved.
* `Integration and configuration - plan-driven or agile`
    - The system is assembled from existing configurable components.
- In practice, most large systems are developed using a process that
    incorporates elements from all of these models.
    
```
model의 장, 단점을 잘 이해해야, 어떤 model을 사용해야 할지 알 수 있다
```

## The waterfall model
- The main drawback of the waterfall model is the difficulty of accommodating changes after the process is underway.
- ***In principle, a phase has to be complete before moving onto the next phase***.

![image](https://user-images.githubusercontent.com/80478750/159023605-cc9230bc-b68d-4d58-a212-4a1cfdf77db6.png)

```
이전 단계가 끝나야지만 다음 단계로 넘어간다.(굉장히 큰 제약)
```

## Waterfall model problems
- Inflexible partitioning of the project into distinct stages makes it
    difficult to respond to changing customer requirements.
     - Therefore, this model is only appropriate **_when the_**
       **_requirements are well-understood and changes will be_**
       **_fairly limited during the design process_**.
    - `Few` business systems have stable requirements.
- The waterfall model is mostly used for large systems engineering
    projects where a system is developed at several sites.
    - In those circumstances, the plan-driven nature of the waterfall
       model helps coordinate the work.

```
- 할 일을 명확하게 정의하지 않으면 아예 설계도 못하고 구현도 못한다.
- requirements가 이미 잘 이해되어 있어서, 그대로 따라하면 될 때는 유리하다. 하지만 이런 경우는 굉장히 제한적이다.
- 요즘은 많이 사용되지 않는 방식이다.

마지막 문단에 대한 예시) 큰 시스템을 개발할때, 5명씩 총 40팀이 개발을 하는 경우, agile 개발은 불가능하다. 200명이 완벽히 communication을 할 수 없다. 
처음에 큰 그림을 그릴 때 waterfall model로 그리고, 각 팀별로 자신들이 원하는 방식으로 개발을 하고, 나중에 종합한다. 
```

## Incremental development
Incremental delivery와 혼동하지 않도록 주의한다.
![image](https://user-images.githubusercontent.com/80478750/159023561-fb57e84b-62a1-4b59-9486-3c837e40b433.png)

## Incremental development benefits
- Reduced cost for accommodating customer requirements changes.
    - The amount of analysis and documentation that has to be redone
       is much less than is required with the waterfall model.
- Easy to get customer feedback.
    - Customers can comment on demonstrations of the software and
       see how much has been implemented.
- More rapid delivery and deployment to the customer.
    - Customers are able to use and gain value from the software
       earlier than is possible with a waterfall process.

```
waterfall model의 경우, 고객의 요구사항이 변경되면 개발 전체가 다 끝나야 처음부터 다시 앞으로 돌아가서 specification을 만들고 진행한다.(상당히 비효율적)

More rapid delivery and deployment to the customer.
- 전체가 100이라면 20까지 구현하고 먼저 배포한다. 고객은 미리 일부의 기능을 하는 소프트웨어를 받을 수 있다.
```
## Incremental development problems
- The process is not visible.
  - Managers need regular deliverables to measure progress.
  - If systems are developed quickly, it is not cost-effective to
    produce documents that reflect every version of the system.
- System structure tends to degrade as new increments are added_._
  - Unless time and money is spent on `refactoring` to improve the
    software, regular change tends to corrupt its structure.
  - Incorporating further software changes becomes increasingly
    difficult and costly.

```
The process is not visible.
- 프로세스가 어떻게 진행되는지 가늠하기 어렵다. 즉, 눈에 보이는 결과물이 잘 나오지 않는다
- 눈으로 보이는 문서 등이 없어서, 경험이 많이 없다면 진행 사항을 파악하기 어렵다
- 개발 인원의 능력이 높아야 한다

System structure tends to degrade as new increments are added
- 시간이 지날수록, 코드가 점점 더 안좋아 질 수 있다. 따라서, refactoring이 중요하다. 내가 코드를 짜면서, github에 올릴 때, 검토할 때 등 항상 refactoring을 해야한다
```


## Integration and configuration
- Based on software reuse where systems are integrated from
  existing components or application systems.
  - These existing components are sometimes called
    _COTS(Commercial-off-the-shelf)_ systems.
- Reused elements may be configured to adapt their behaviour
  and functionality to a user’s requirements.
- **_Reuse is now the standard approach_** _for building many types_
  _of business system_.
  - Reuse covered in more depth in Chapter 15.

```
창업을 하는 경우, idea를 처음부터 다 개발을 하게 되면 시간이 너무 오래 걸린다. 내가 가져다 쓸 수 있는 것을 최대한 가져다 쓰려면 이 방식을 잘 알아야 한다
```

## Types of reusable software
- **_Stand-alone application systems (COTS)_** that are configured
  for use in a particular environment.
- **_Collections of objects_** that are developed as a package to be
  integrated with a component framework such as .NET or J2EE.
- **_Web services_** that are developed according to service standards
  and which are available for remote invocation.
- Even **_entire systems_** can be considered as reusable software.^
  - Cloud Services like AWS, Containers supported by Docker,
    etc.

## Reuse-oriented software engineering

**Key Process Stages**

이 부분은 나중에 추가하기


## Advantages and disadvantages
- Advantages
  - **_Reduced costs and risks_** , as less software is developed from
    scratch.
  - **_Faster delivery and deployment_** of system.
- Disadvantages
  - **_Requirements compromises are inevitable_** so system may
    not meet real needs of users.
  - **_Loss of control_** over evolution of reused system elements.

```
Reduced costs and risks
- 가져다 쓰므로 빠르게 개발할 수 있고, reusable한 sw는 이미 검증이 된 sw이다. 다른 사람이 개발 과정에서 test 및 validation 했을 것이고 심지어 이 sw를 가져다 쓴 사람도 validation을 했을 것이다. 그래서 문제가 발견된 것이 있으면 어느정도는 고쳐졌을 확률이 높다.

Loss of control
- 내가 만든게 아니므로 내가 마음대로 고치기가 좀 그렇다. 만약 가져다 쓴 sw의 version이 바뀌었는데, 우리의 system과 호환이 안 될수도 있다. 
```


# Process activities

## Process activities 
- Real software processes are inter-leaved sequences of technical,
  collaborative and managerial activities,
  - with the overall goal of specifying, designing, implementing
    and testing a software system.
- The four basic process activities of **_specification(also known as requirements)_** , **_design and implementation_** , **_validation_** and **_evolution_** are organized
  differently in different development processes.
- For example, in the waterfall model, they are organized in
  sequence, whereas in incremental development they are
  interleaved.

## Software specification
- The process of establishing what services are required and the constraints
  on the system’s operation and development.
- Requirements engineering process
  - **_Requirements elicitation and analysis_** - system description
    - What do the system stakeholders require or expect from the system?
  - **_Requirements specification_** - user and system requirements
    - Defining the requirements `in detail`
  - **_Requirements validation_** - requirements document
    - Checking the validity of the requirements

## Software design and implementation

- The process _of_ **_converting the system specification into an_**
  **_executable system_**.
- Software design
  - Design a software structure that realises the specification;
- Implementation
  - `Translate` this structure into an executable program;
- The activities of design and implementation are `closely related`
  and may be inter-leaved.

```
요구사항을 실제로 충족시킬 수 있는 시스템을 만드는 것. design과 implementation을 요새는 따로 분리하지 않음. 자기가 알아서 디자인과 구현을 진행해야 한다
```

**A general model of the design process**

나중에 추가하기


## Design activities
_-_ **_Architectural design_** _,_ where you identify the overall structure of
    the system, the principal components (subsystems or modules),
    their relationships and how they are distributed.
_-_ **_Database design_** _,_ where you design the system data structures
    and how these are to be represented in a database.
_-_ **_Interface design_** _,_ where you define the interfaces between
    system components.
_-_ **_Component selection and design_** _,_ where you search for
    `reusable` components. If unavailable, you design how it will
    operate.

## System implementation
- The software is implemented either by developing a program/
  programs, or by configuring an application system.
- Design and implementation are interleaved activities for most
  types of software system.
- `Programming` is an individual activity with no standard process.
- `Debugging` is the activity of finding program faults and correcting
  these faults.

## Software validation
- Verification and validation (V & V) is intended to show that a
  system **_conforms to its specification and meets the_**
  **_requirements_** of the system customer.
  - **_Verification_** : Is it correctly implemented?
  - **_Validation_** : Does it satisfy customers?

```
software validation <- 요새 중요하게 여겨지는 부분
난이도: Verification << Validation
* Verification: 정확하게 구현이 되었는가? 기준: 스펙(요구사항 목록). testing을 굉장히 많이 사용함(e.g. game Cyberpunk)
* Validation: 위와 조금 개념이 다르다. Verification을 완벽히 통과해도 Validation에서는 우리가 만족하는게 아닐 수 있다(e.g. game The East of us part 2)
```

- Involves `checking and review processes` and system testing.
- System testing involves executing the system with test cases
  that are derived from the specification of the real data to be
  processed by the system.
- `Testing` is the most commonly used V & V activity.

```
review process
- Code Review: long term으로 봤을 때 효과적이며, 많은 소프트웨어 회사들이 이 방식을 도입하고 있다. 만약 회사에 입사했는데 code review system이 없으면.. 이직을 고려할 것
```

## Testing stages

* **Component testing**
  * Individual components are tested independently;
  * Components may be functions or objects or coherent groupings of these entities.
* **System testing**
  * Testing of the system as a whole.
  * Testing of emergent properties is particularly important
* **Customer testing(Acceptance testing)**
  * Testing with customer data to check that the system meets the `customer's needs.`

```
Component testing
- unit testing이 여기에 포함된다. 내가 작성한 funciton, method, class 등과 같은 unit을 대상으로 수행함

Customer testing
- 위의 두 testing에 비해 상당히 어렵다. QA team에서 면밀하게 검토를 잘 해줘야 한다
```

## Software evolution
- Software is `inherently` flexible and can change.
- As requirements change through changing business circumstances,
  - the software that supports the business must also evolve and
    change.
- Although there has been a demarcation between development and
  evolution (maintenance),
     - this is increasingly irrelevant as fewer and fewer systems are
       completely new.
     - e.g.) DevOps(Development + Operation)

# Coping with change

## Coping with change 
- Change is inevitable in all large software projects.
  - Business changes lead to new and changed system
    requirements.
  - New technologies open up new possibilities for improving
    implementations.
  - Changing platforms require application changes.
- Change leads to rework so the costs of change include both
  rework (e.g. re-analysing requirements),
  - as well as the costs of implementing new functionality

```
변경된 부분을 받아들여야 한다면, 그럼 어떻게 효율적으로 대처를 할 것인가? 변경된 부분을 어떻게 처리할 것인가?
```

## Reducing the costs of rework
- **_Change anticipation_** , where the software process includes activities that
  can `anticipate` possible changes before significant rework is required.
     - For example, a prototype system may be developed to show some key
       features of the system to customers.
- **_Change tolerance_** , where the process is designed so that changes can
  be accommodated at relatively low cost.
     - This normally involves some form of incremental development.^
       - Proposed changes may be implemented in increments that have
         not yet been developed.
     - If this is impossible, then only a single increment (a small part of the
       system) may have be altered to incorporate the change.

```
우리는 모든 변경사항을 예측할 수 없다.Change Tolerance- 처음에 설계 및 구현을 할 때, 이런 변경 사항이 발생하더라도 잘 적용될 수 있도록 시스템을 설계하는 것(모듈화와 관련이 있다)
```

## Coping with changing requirements
- `System prototyping`
  - A prototype is developed quickly to check the customer’s
    requirements and the feasibility of design decisions.
  - This approach supports change anticipation.
- `Incremental delivery`
  - system increments are delivered to the customer for comment
    and experimentation.
  - This supports both change avoidance and change tolerance.

```
아예 requirements가 바뀌는 경우(요구사항이 변경되는 경우)두 방법의 핵심 idea: 고객에게 미리 일부를 보여준다. 다 하기 전에 미리 보여주고, 요구사항에 맞추어서 진행하는 것. (e.g. 100을 해야하면 20을 먼저 보여주고, 요구사항이 변경되면 지금 미리 얘기하도록 하는 방식)System prototyping- prototyping을 해주는 도구를 많이 사용한다. Demo를 보여주기보다는, ppt 등으로 슬라이드를 만들어서 이미지를 대략 보여준다Incremental delivery- 각각의 version들을 계속해서 고객에게 전달할 수 있는 형태로 보여준다
```

## Software prototyping
- A prototype is an initial version of a system used to demonstrate
  concepts and try out design options.
- A prototype can be used in:
  - The requirements engineering process to help with
    requirements elicitation and validation;
  - In design processes to explore options and develop a UI
    design;
  - ~~In the testing process to run back-to-back tests.~~

## Benefits of prototyping
- Improved system usability.
- `A closer match to users’ real needs.`(`= Validation이 잘 된다`)
- Improved design quality.
- Improved maintainability.
- `Reduced development effort.`

## Prototype development
- May be based on rapid prototyping languages or tools.
- May involve leaving out functionality.
  - Prototype should focus on areas of the product that are not
    well-understood;
  - Error checking and recovery may not be included in the
    prototype;
  - Focus on functional rather than non-functional requirements
    such as reliability and security.

## Throw-away prototypes
- Prototypes should be discarded after development as they are
  not a good basis for a production system:
  - It may be impossible to tune the system to meet non-
    functional requirements;
  - Prototypes are normally undocumented;^
  - The prototype structure is usually degraded through rapid
    change;
  - The prototype probably will not meet normal organisational
    quality standards.

```
prototype은 보여주기 위한 것이므로 과감히 버린다. 이후 처음부터 다시 엄밀하게 설계하면서 개발 시작
```

## Incremental delivery
- Instead of a single delivery, the development and delivery is
  broken down into increments,
     - with each increment delivering part of the required functionality.
- User requirements are prioritised.
  - The highest priority requirements are included in early
    increments.
- Once the development of an increment is started, the
  requirements are frozen,
     - though requirements for later increments can continue to evolve.

```
Final version만 고객에게 전달하는 것은 Incremental delivery가 아니다. 개발을 Incremental하게 한 것이다. 
Incremental delivery는 일단 여기까지 만들고, 배포를 한다(e.g. 앱스토어에서 일단 배포가 된 다음, 업데이트가 계속 진행되는 것)
```

## Incremental development and delivery
- Incremental development
  - Develop the system in increments and evaluate each increment before
    proceeding to the development of the next increment;
  - Normal approach used in agile methods;
  - Evaluation done by `user/customer proxy`.
- Incremental delivery
  - `Deploy` an increment for use by `end-users`;
  - More realistic evaluation about practical use of software;
  - Difficult to implement for replacement systems as increments have less
    functionality than the system being replaced.

## Incremental delivery

이미지



## Incremental delivery advantages
- Customer value can be delivered with each increment so system
  functionality is available earlier.
- Early increments act as a prototype to help elicit requirements for
  later increments.
- Lower risk of overall project failure.
- The highest priority system services tend to receive the most
  testing.

## Incremental delivery problems
- Most systems require a set of `basic facilities` that are used by different parts of the system.
     - As requirements are not defined in detail until an increment is to be
       implemented,
          - it can be hard to identify common facilities that are needed by all
            increments.
- The essence of iterative processes is that the specification is developed in
  conjunction with the software.
     - However, this conflicts with the procurement model of many organizations.
       - The complete system specification is a part of the system development
         contract.

```
Agile process가 Incremental development/delivery와 강한 연관이 있는데, Agile이 사용되는 회사들은 대부분 자기들이 운영하는 서비스를 가지고 있고 그 서비스를 내부에서 돌린다.(customer가 자기 자신이 되는 경우 원하는대로 개발이 가능함)
외부에 서비스를 만들어서 판매하는 경우(e.g. 정부 기관에서 management system 에 대해 입찰 공고를 냈다. 이런 경우에는 애초에 계약할 때부터 요구사항이 구체적으로 정해지므로, Incremental delivery가 불가능하다)
```

## Summary
- Software process models are generally `involved with four software
  activities`.
     - Specification, Design and Implementation, Validation and
       Evolution.
     - Differences between models are based on how the activities are
       placed in the process models.
- Software changes are inevitable.
  - Hence we need to know how to deal with such changes.
  - Change anticipation, Change tolerance, Prototyping, and
    Incremental Delivery.
