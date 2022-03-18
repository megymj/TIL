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
