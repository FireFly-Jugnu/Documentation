# Reference Collections
Firebase allows you to reference documents of other collections. 

Consider the example below of Person and Anddress

```
@FirebaseCollection()
class Address {
    @DocumentKey(DocumentKeyType.UserDefined)
    addrKey: String = "";

    @DocumentField
    streetName?: String;

    @DocumentField
    houseNumer?: String;
}
```

The Person document has a address field that stores reference to the address
```
@FirebaseCollection()
class Person{

    @DocumentKey(DocumentKeyType.UserDefined)
    name: String;

    @DocumentField
    age: Number;

    location?: String;

    @DocumentField
    phone: String;

    @DocumentField
    homeAddress?: Address;

    constructor(name: String){
        this.name = name;
    }

}
```

## Create Document
Create the address by the normal method.
```
const addressCollection = jugnu.createFirebaseCollection(Address);

const addr = new Address();
addr.addrKey = "Varun-Addr";
addr.streetName = "My Street Address";
addr.houseNumer = "My House No";
addressCollection.create(addr);     // Create new Address
```

Create person, with reference to Address.
```
const personCollection = jugnu.createFirebaseCollection(Person);

const varun = new Person("Varun Verma");
varun.age = 25;
varun.location = "India";
varun.phone = "+91-1234567890";
varun.homeAddress = addr;           // Set address in Person

personCollection.create(varun); 

```

The person document is created with Address as a reference document
![Person Document](../images/person_document.png) 


## Creating Document with collection of references
We have seen how to store the refence of other documents. Sometimes, we have to store a collection of document references. 

For e.g. store a list of many children in a Person Profile.

```
@FirebaseCollection()
class Person {
    @DocumentKey(jugnu.Types.DocumentKeyType.UserDefined)
    id: string;

    @DocumentField
    name: string;

    @DocumentField
    parent: Person;     // Reference to Self Collection

    @DocumentArrayField(Person)
    children: Person[];   // Reference to Self Collection

    constructor(id: string){
        this.id = this.name = id;
        this.children = [];
    }
}

const createFunction = async() => { 

    const vv = new Person("Varun");

    vv.parent = new Person("Papa");

    const beta = new Person("Beta");
    const beti = new Person("Beti");
    
    vv.children.push(beta);
    vv.children.push(beti);

    const id = await personCollection.create(vv);
    console.log(vv);

};
```
The peron document is created with references
![Person Document](../images/Person_Doc2.png) 



## Query Single Document with Key
Querying document will **NOT** expand the reference documents. This is the default behaviour. 

However, you can pass the `expandRefrences = true` to also query the referenced documents and populate their properties.
```
let p: Person;
p = await personCollection.getDocument("Varun Verma", true);
console.log("Person Details", p);
```
The output will be: 
```
Person {
  age: 25,
  phone: '+91-1234567890',
  homeAddress: {
    houseNumer: 'My House No',
    addrKey: 'Varun-Addr',
    streetName: 'My Street Address'
  },
  name: 'Varun Verma'
}
```

In this case, the `homeAddress` is expanded

## Get Reference of the document
To get a reference of the document : 
```
const addr = new Address("Home");
const addressRef = addressCollection.getReference(addr);
```


## Delete Document
Delete will delete only the main document. The referenced document will not be deleted.