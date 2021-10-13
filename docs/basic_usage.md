# Usage
Jugnu is compaitable with other Javascript frameworks.

## Annotations
Annotate your typescript classes and members. Create any class that represents an entity.
```
@FirebaseCollection
class Person{

    @DocumentKey
    name: String;

    @DocumentField
    age: Number;

    location?: String;

    @DocumentField
    phone: String;

    constructor(name: String){
        this.name = name;
    }
}
```


## Create Document
```
const personCollection = jugnu.createFirebaseCollection(Person);

const varun = new Person("Varun Verma");
varun.age = 25;
varun.location = "India";
varun.phone = "+91-1234567890";

personCollection.create(varun);
```

## Query Documents
```
const personList: Person[];
personList = await personCollection.query([]); 
console.log(personList);
```

## Query Single Document with Key
```
let p: Person;
p = await personCollection.getDocument("Varun Verma");
console.log("Person Details", p);
```

## Delete Document
```
await personCollection.delete(varun);
```