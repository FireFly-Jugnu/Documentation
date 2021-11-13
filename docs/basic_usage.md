# Usage
Jugnu is compaitable with other Javascript frameworks.

## Annotations
Annotate your typescript classes and members. Create any class that represents an entity.
```
@FirebaseCollection()
class Person{

    @DocumentKey(DocumentKeyType.UserDefined)
    name: string;

    @DocumentField
    age: number;

    @DocumentField
    city: string;

    location?: string;

    @DocumentField
    phone: string;

    constructor(name: string){
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
varun.city = "Bangalore";
varun.phone = "+91-1234567890";

personCollection.create(varun);
```

If a field is not annotated with `@DocumentField`, then it wont be saved. 

## Query Documents
The query function takes where conditions as defined by the [Firebase](https://firebase.google.com/docs/firestore/query-data/queries)
```
const personList: Person[];
const searchCondition = [{
    field: "city",
    condition: "==",
    value: "Bangalore"
}];
personList = await personCollection.query(searchCondition); 
console.log(personList);
```
To query all documents -- pass empty search condition
```
personList = await personCollection.query([]); 
```

## Query Single Document with Key
```
let p: Person;
p = await personCollection.getDocument("Varun Verma");
console.log("Person Details", p);
```

## Update an Document
```
let p: Person;
p = await personCollection.getDocument("Varun Verma");
p.city = "Delhi";
personCollection.update(p);
```

## Delete Document
```
await personCollection.delete(varun);
```