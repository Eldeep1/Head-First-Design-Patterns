# The Strategy Pattern

## Scenario
* Assume you are creating software for a sewing machine. You have two types of fabric and both are sewn in the same way.<br>
Simply you will have a parent class that contains the implementation for the sewing mechanism.

```java
    abstract class Sewing{
        public void sew(){
            System.out.println("we are sewing "+getDescription());
        }
        abstract public String getDescription();
    }
```
Child Classes:
```java
    class TypeA extends Sewing{
        @Override
        public String getDescription(){
            return "type A";
        }
    }
   class TypeB extends Sewing{
        @Override
        public String getDescription(){
            return "type B";
        }
    }
     
```
* But, What if we add two more types of fabric with it's sewing mechanisms?

```java
class TypeC extends Sewing{

        @Override
        public void sew(){
            System.out.println("we are sewing "+getDescription()+" with second way");
        }

        @Override
        public String getDescription(){
            return "type C";
        }
    }
   class TypeD extends Sewing{
        @Override
        public void sew(){
            System.out.println("we are sewing "+getDescription()+" with second way");
        }
        @Override
        public String getDescription(){
            return "type D";
        }
    }

```
* Notice that both new subclasses have the same implementation, duplicated in both classes! and maybe you want to change the way they are being sewed, Would you have to modify each class manually?<Br>
* what if they are not two classes, instead they are 100 classes, it will be a mess to update the code 

* currently addressed problems:
    1. You can't update the sewing behaviour for cloth that are being sewed on the same way fast.
    2. You can't change the Sewing Behaviour on runtime.

## Solution:
* First, you need to recongnize parts that change and separate them. Themost noticeable part that varies is the `sew` function.
* We will create a class called `SewingBehaviours` which encapsulates the mechanisms of sewing inside of it.
```java
    abstract class SewingBehaviours{
        abstract public void sew(String description);
    }
```
* Then, we will implement it for the sewing behaviours that we have:
```java
    class typeASewingBehaviour extends SewingBehaviours{
        @Override
        public void sew(String description){
            System.out.println("we are sewing "+description);
        }
    }
    class typeBSewingBehaviour extends SewingBehaviours{
        @Override
        public void sew(String description){
            System.out.println("we are sewing "+description+" with the second way");
        }
    }
```
* The parent class will then look like this:
```java
abstract class Sewing{
        SewingBehaviour sewingBehaviour;
        public void sew(){
            sewingBehaviour.sew(getDescription());
        }
        abstract public String getDescription();
    }
```
* For child classes, we need a way to pass them the `SewingBehaviour`.
let's initially pass them in the constructor:
```java

class TypeA extends Sewing{
        TypeA(SewingBehaviour sewingBehaviour;){
            super.sewingBehaviour = sewingBehaviour;
        }
        @Override
        public String getDescription(){
            return "type A";
        }
    }
```

* you may have noticed, like that we don't have the ability to change the behaiour at runtime, so let's create a one final change:
```java
abstract class Sewing{
        SewingBehaviour sewingBehaviour;
        public void sew(){
            sewingBehaviour.sew(getDescription());
        }
        abstract public String getDescription();
        public void setSewingBehavior(SewingBehaviour sewingBehaviour){
            this.sewingBehaviour = sewingBehaviour ;
        }
    }
class TypeA extends Sewing{
        TypeA(SewingBehaviour sewingBehaviour){
            super.sewingBehaviour = sewingBehaviour;
        }
        @Override
        public String getDescription(){
            return "type A";
        }
    }
```
* Now the sewing behavior of each fabric can be changed at runtime.

## Key points:
1. Encapsulate what varies.
2. Favor composition over inheritance.
3. Program to interfaces, not
implementations.

