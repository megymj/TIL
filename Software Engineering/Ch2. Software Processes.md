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
