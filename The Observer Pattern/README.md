# The Observer Pattern

## <b>Scenario</b>
*  Assume you have a phone shop. You are selling products to customers, but you want a way to let the customers know when there's a new product on the stock.<br>
* You may collect their phone numbers and send a message to all of them when a new product arrives in stock, but what if someone wants to unsubscribe? They would have to contact you. If he wants to subscribe again, he will call you again, and so on.
* You may have noticed that it's not an ideal solution. We need a way so he can subscribe or unsubscribe themselves, and this is where the Observer Pattern comes in.
## <b>Solution</b>: <center><b>The Observer Pattern</b></center> 

* The observer pattern is a design pattern where you can subscribe to a publisher and see what he lists, there are two common approaches to delivering updates:
    1. push: the publisher sends updates directly to subscribers.
    2. pull: subscribers retrieve the information they need from the publisher.

* But to subscribe to a publisher, you need to implement an interface, so the publisher knows which methods it can call on its subscribers.
* Let's take a closer look:

```java
interface Subscriber{
    public void onUpdate(String newPhone); // to get notified when a new phone arrives
}
```
* And for the publisher, it also must implement an interface:
```java
interface Publisher{
    public void subscribe(Subscriber sc);
    public void unSubscribe(Subscriber sc);
    public void update(String phoneName); //this method will be called when a phone is added (we don't care who calls it)

}
```
* You may think that this interface isn't supporting the pull, it just notifies, and that's right actually... We will add the pull support later
* For the concrete publisher:
```java
class PhoneShop implements Publisher{
    List<Subscriber> subscribersList = new ArrayList<>();
    public void subscribe(Subscriber sc){
        subscribersList.add(sc);
    }
    public void unSubscribe(Subscriber sc){
        subscribersList.remove(sc);
    }
    public void update(String phoneName){
    for (Subscriber sc : subscribersList){
                sc.onUpdate(phoneName+" just landed!");
        }
    }
}
```
* For The Subscriber:
```java
class User implements Subscriber{
    Publisher publisher;
    User(Publisher publisher){
        this.publisher = publisher;
        publisher.subscribe(this);
    }
    public void onUpdate(String phoneName){
        System.out.println("do whatever needed action with "+phoneName);
    }
    //if the subscriber wants to remove themselves:
    public void unsubscribe(){
        publisher.unSubscribe(this);
    }
}
```

* To add pull mechanism, you will need to do some small changes:
1. In the publisher:
 ``` java
    interface Publisher{
    //same code
    public String getLatestPhone();
}
```
2. In the concrete phone shop:
```java
class PhoneShop implements Publisher{
    private String latestPhone;
    //same code
    @Override
    public void update(String phoneName) {
        latestPhone = phoneName;

        // You may remove this if you want to allow only pulling the updates
        for (Subscriber sc : subscribersList) {
            sc.onUpdate(latestPhone+" just landed!");
        }
    }
    @Override
    public String getLatestPhone() {
        return latestPhone;
    }
}
```
3. In The subscriber:
```java
class User implements Subscriber{
    Publisher publisher;
    User(Publisher publisher){
        this.publisher = publisher;
        //if you want to disable pushing, then comment it
        publisher.subscribe(this);
    }
    
    public void onUpdate(String phoneName){
        System.out.println("do whatever needed action with "+phoneName);
    }
    
    public void unsubscribe(){
        publisher.unSubscribe(this);
    }
    public void getUpdates(){
        String phone = publisher.getLatestPhone();
        System.out.println("The latest phone is "+ phone);
    }
}
```
## <b>Key points:</b>
1. Strive for loosely coupled designs between objects that interact.