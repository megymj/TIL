# Software Engineering

## Chapter 3 Agile Software Development

**Topics**


- Agile methods
- Agile development techniques
- Agile project management
- Agile planning

## Rapid software development


- Rapid development and delivery is now often the most important
  requirement for software systems
     - Businesses operate in a fast–changing requirement and it is
       **_practically impossible to produce a set of stable software_**
       **_requirements_**.
     - **_Software has to evolve quickly to reflect changing business_**
       **_needs_**.
- Plan-driven development is essential for some types of system but
  does not meet these business needs.
- Agile development methods emerged in the late 1990s whose aim was
  to radically reduce the delivery time for working software systems.

## Agile development


- Program specification, design and implementation are inter-
  leaved.
- The system is developed as a series of versions or increments
  with stakeholders involved in version specification and
  evaluation.
- Frequent delivery of new versions for evaluation.
- Extensive tool support (e.g. automated testing tools) used to
  support development.
- `Minimal documentation – focus on working code.`

## Plan-driven and agile development
![image](https://user-images.githubusercontent.com/80478750/159145381-01fbfecf-dfa4-461a-9194-b3c7e425e6da.png)

* **Plan-driven development**
  * A plan-driven approach to software engineering is based around separate development stages with the outputs to be produced at each of these stages planned in advance.
  * ***Not necessarily waterfall model - plan-driven, incremental development is possible***.
  * Iteration occurs within activities.

* **Agile development**
  * Specification, design, implementation and testing are inter-leaved and
        the outputs from the development process are decided through a
        process of negotiation during the software development process.



# Agile methods

## Agile methods


- Dissatisfaction with the overheads involved in software design
  methods of the 1980s and 1990s led to the creation of agile
  methods. These methods:
     - `Focus on the code` rather than the design
     - Are based on an `iterative approach` to software development
     - Are `intended to deliver working software quickly` and evolve this
       quickly to meet changing requirements.
- The aim of agile methods is to reduce overheads in the software
  process (e.g. by limiting documentation) and to be able to respond
  quickly to changing requirements without excessive rework.

## Agile manifesto(성명서)


- We are uncovering better ways of developing software by doing it
  and helping others do it.
- Through this work we have come to value:
  - **_Individuals and interactions_** over processes and tools.
  - **_Working software_** over comprehensive documentation.
  - **_Customer collaboration_** over contract negotiation.
  - **_Responding to change_** over following a plan.
- That is, while there is value in the items on the right, we value the
  items on the left more.




## The principles of agile methods


| Principle            | Description                                                  |
| :------------------- | ------------------------------------------------------------ |
| Customer involvement | Customers' role is provide and prioritize new system requirements and to evaluate the iterations of the system. |
| Incremental delivery | The software is developed in increments with the customer specifying the requirements to be included in each increment. |
| People not process   | The skills of the development team should be recognized and exploited. Team members should be left to develop their own ways of working without prescriptive processes. |
| Embrace change       | Expect the system requirements to change and so design the system to accommodate these changes. |
| Maintain simplicity  | Focus on simplicity in both the software being developed and in the development process. |

##  Agile method applicability


- Product development where a software company is **_developing_**
  **_a small or medium-sized product_** for sale.
  - Virtually(`대체로`) all software products and apps are now developed
    using an agile approach.
- Custom system development within an organization, where there
  is a clear commitment from the **_customer to become involved_**
  **_in the development process_** and where there are **_few external_**
  **_rules and regulations_** that affect the software.



# Agile development techniques

## Extreme programming(XP)


- A very influential agile method, developed in the late 1990s, that
  introduced a range of agile development techniques.
- Extreme Programming (XP) takes an ‘extreme’ approach to
  iterative development.
  - New versions may be built several times per day;
  - Increments are delivered to customers every 2 weeks;
  - All tests must be run for every build and the build is only
    accepted if tests run successfully.

```
Exterme programming은 요즘에는 현실과 잘 맞지 않는 부분도 있어서, 이야기를 잘 하지 않는다
```

## The extreme programming release cycle
![image](https://user-images.githubusercontent.com/80478750/159145625-3dcd72be-fe5e-4b9d-b1c3-b4ca02122feb.png)



## Extreme programming practices

| Principle or practice  | Description                                                  |
| :--------------------- | ------------------------------------------------------------ |
| Incremental planning   | Requirements are recorded on story cards and the stories to be included in a release are determined by the time available and their relative priority. The developers break these stories into development ‘Tasks’. |
| Small releases         | The minimal useful set of functionality that provides business value is developed first. Releases of the system are frequent and incrementally add functionality to the first release. |
| Simple design          | Enough design is carried out to meet the current requirements and no more. |
| Test-first development | An automated unit test framework is used to write tests for a new piece of functionality before that functionality itself is implemented. |
| **Refactoring**        | All developers are expected to refactor the code continuously, and keep the code simple and maintainable. |
| ~~Pair programming~~   | ~~Developers work in pairs, checking each other’s work and<br/>providing the support to always do a good job.~~ |
| Collective ownership   | The pairs of developers work on all areas of the system, so<br/>that no islands of expertise develop and all the developers<br/>take responsibility for all of the code. |
| Continuous integration | As soon as the work on a task is complete, it is integrated into the whole system. |
| ~~Sustainable pace~~   | ~~Large amounts of overtime are not considered acceptable as<br/>the net effect is often to reduce code quality and medium<br/>term productivity~~ |
| On-site customer       | In an extreme programming process, the customer is a member of the development team and is responsible for bringing system requirements to the team for implementation. |



## XP and agile principles


- Incremental development is supported through small, frequent
  system releases.
- Customer involvement means full-time customer engagement(`on-site customer`) with the team.
- People not process through pair programming, collective
  ownership and a process that avoids long working hours.
- Change supported through regular system releases.
- Maintaining simplicity through constant `refactoring` of code.

## Influential XP practices


- Extreme programming has a technical focus and is not easy to
  integrate with management practice in most organizations.
- Consequently, while agile development uses practices from XP, the
  method as originally defined is not widely used.
- Key practices
  - `User stories for specification`
  - `Refactoring`
  - `Test-first development`
  - Pair programming -> `Pair programming은 현실에서 쉽지 않다. 대신 Code Review 등의 방식을 사용한다`

## User stories for requirements


- In XP, a customer or user is part of the XP team and is
  responsible for making decisions on requirements.
- User requirements are expressed as user stories or scenarios.
- These are written on cards and the development team break
  them down into implementation tasks. These tasks are the basis
  of schedule and cost estimates.
- The customer chooses the stories for inclusion in the next
  release based on their priorities and the schedule estimates.

## A Story Example


- Kim has three credit & debit cards with different benefits.
- Every time she pays with a card, she needs to choose one of the three
  cards considering benefits and conditions.
     - She uses a card if she can get cash rebate or discount from the card
       in the current store.
     - She uses a card if there is no cash rebate or discount, but she can
       fill up the required spending amount for the card.
- However, keeping track of all the conditions and benefits is quite
  complicated and time-consuming.
- She often thinks that it would be nice if she was freed from this burden.


- So, she thinks about a mobile app which does the work for her.
- She can register all her cards to the app.
  - The app automatically collects information about benefits and conditions of
    the registered cards.
- Then every time Kim wants to pay with the cards, she simply opens the app, and
  shows a QR code generated by the app.
- When a payment request is received, the app calculates the most suitable card
  for the payment,
     - and relays the request to that card's payment server.
- The app also shows the remaining spending quota and benefits for the cards,
  - so that Kim can obtain necessary information for her expenses.

## Task obtained from the story


- **Task 1** : Collecting credit & debit cards information.
  - Crawling web pages for existing cards, or manually input the information.
  - Using card id to store each card information, and make it easy to be retrieved.
  - Need to keep updating the latest information.
- **Task 2** : Computing the most suitable card.
  - Select a card for a payment, based on the card's benefits and conditions.
  - Need to be very quick, so that payments are smooth and seamless.
  - Refer to users' preference settings when selecting a card.
- Any other tasks?
