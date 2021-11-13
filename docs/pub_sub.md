# Pub-Sub with Jugnu
The aim of Jugnu Framework is to integrate well into the GCP / Firebase Apps. 

Pub-Sub is an important concept in developing modern applications. So, with Jugnu, we can easily publish Events whenever a document is created / updated or deleted. 

Annotate the entity class to enable Event Publishing.

```
@FirebaseCollection()
@PublishEvent(jugnu.Types.EventName.OnCreate)
@PublishEvent(jugnu.Types.EventName.OnUpdate)
@PublishEvent(jugnu.Types.EventName.OnDelete)
class SalesOrder {

    @DocumentKey(jugnu.Types.DocumentKeyType.AutoIncrement)
    id: string;
    
    @DocumentField
    totalAmount: number = 0;

    @DocumentField
    orderDate: Date;

    @DocumentField
    @SystemAdminData
    sysAdminData?: jugnu.Types.SystemAdminData;

    constructor(){
        this.orderDate = new Date();
    }
}

const orderCollection = jugnu.createFirebaseCollection(SalesOrder);

// Create a new Order.
const order = new SalesOrder();
const id = await orderCollection.create(order);     // After order create, an event will automatically be published.


order.totalAmount = order.totalAmount + 100;
orderCollection.update(order);      // After order create, an event will automatically be published.


await orderCollection.delete(order);        // After order create, an event will automatically be published.
```

## Topic Names
If a topic name is explicitly specified, its taken. Otherwise, the topic name is automatically generated. 

The topic name is generated as follows: `{{EventName}}-{{EntityName}}`.

For e.g. the topic name will be as follows: 
- On create of SalesOrder : `OnCreated-SalesOrder`
- On update of SalesOrder : `OnUpdate-SalesOrder`
- On delete of SalesOrder : `OnDelete-SalesOrder`

If the topic does not exist already, then a topic will be created.