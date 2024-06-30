---
title: Design Pattern - Observer Pattern
date: 2016-02-28 13:15:42
categories:
- Design Pattern
tags:
- design pattern
- observer
- java
---
>
Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically. - By GOF


#### Concept
In this pattern, there are many `observers`(object) which are observing a particular `subject`(object).Observers are basically interested and want to notified when there is a change made inside that `subject`. So, they register themselves to that `subject`. When they lose interest in the `subject` they simply unregister from the `subject`. Sometimes this model is also refered to as the Publisher-Subscriber model.

#### Real-Life Example
We can think about a celebrity who has many fans. Each of these fans wants to get all the latest updates of his/her favorite celebrity. So, he/she can follow the celebrity as long as his/her interest persists. When he loses interest, he simple stops following that celebrity. Here we can think of the fan as an `observer` and the celebrity as a `subject`.

#### UML Diagram

![uml diagram] (/images/uml/observer_uml_diagram.png )

#### Implementation
The participants classes in this pattern are:
- Observable - interface or abstract class defining the operations for attaching and de-attaching observers to the client. In the GOF book this class/interface is known as **Subject**.
- ConcreteObservable - concrete Observable class. It maintain the state of the object and when a change in the state occurs it notifies the attached **Observers**.
- Observer - interface or abstract class defining the operations to be used to notify this object.
- ConcreteObserverA, ConcreteObserver2 - concrete **Observer** implementations.

The flow is simple: the main framework instantiate the ConcreteObservable object, Then it instantiate and attaches the concrete observers to it using the methods defined in the Observable interface. Each time the state of the subject it's changing it notifies all the attached Observers using the methods defined in the Observer interfae. When a new Observer is added to the application, all we need to do is to instantiate it in the main framework and to add attach it to the Observable object, The classes alreadly created will retmain unchanged.

#### Coding in Java

##### Some Features

- Model the "independent" functionality with a "subject" abstraction
- Model the "dependent" functionality with "observer" hierarchy
- The Subject is coupled only to the Observer base class
- Observers register themselves with the Subject
- The Subject broadcasts events to all registered Observers
- Observers "pull" the information they need from the Subject
- Client configures the number and type of Observers

```
/**
 * ObserverPatternDemo Created by lechance .
 */
public class ObserverPatternDemo {

    public static void main(String[] args) {

        Subjection subjection = new Subjection();

        new AObserver(subjection);
        new BObserver(subjection);

        subjection.setState(100);
        subjection.setState(23);
        subjection.setState(3232);
        subjection.setState(432);

        //or 
//
//        Scanner scanner = new Scanner(System.in);
//        while (true) {
//            System.out.println("Enter a number:");
//            subjection.setState(scanner.nextInt());
//        }
    }
}

abstract class Observer {

    protected Subjection subject;

    public abstract void update();
}

class BObserver extends Observer {

    public BObserver(Subjection sub) {
        super.subject = sub;
        sub.attach(this);
    }

    @Override
    public void update() {
        System.out.println("Observer B: " + Integer.toString(subject.getState()));
    }
}

class AObserver extends Observer {

    public AObserver(Subjection sub) {
        super.subject = sub;
        sub.attach(this);
    }

    @Override
    public void update() {
        System.out.println("Observer A: " + Integer.toString(subject.getState()));
    }
}

//Subject
class Subjection {

    public final int MAX_LIMITED_OBSERVERS = 9;

    private Observer[] observers = new Observer[MAX_LIMITED_OBSERVERS];
    private int totalobs;
    private int state;

    public int getState() {
        return this.state;
    }

    public void setState(int state) {
        this.state = state;
        notifyObservers();
    }

    public void attach(Observer observer) {

        if (totalobs < MAX_LIMITED_OBSERVERS) {
            observers[totalobs++] = observer;
        }
    }

    public void notifyObservers() {
        for (int i = 0; i < totalobs; i++) {
            observers[i].update();
        }
    }
}

```
