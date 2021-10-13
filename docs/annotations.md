# Annotation Details

## FirebaseCollection()
Annotate a class with `@FirebaseCollection()`. 

If no parameter is provided, then the class name is taken as the Collection name.

If a parameter (of type string) is provided, then the parameter value is taken as Collection Name
For e.g. 
```
@FirebaseCollection('StudentCollection')
class Student{

}
```
In this case the firebase collection is created with name: `StudentCollection`

## DocumentKey
Annotate a property with `@DocumentKey`

This property is used as the key of the document. 

## DocumentField
Annotate a property with `@DocumentField`

If any property is **NOT** annotated with this annotation, this field will **NOT** be saved in the document. 
This gives users the flexibility to skip some fields which are not to be saved in documents.

## StorageFile
Annotate a property with `@StorageFile`

Properties annotated like this will be considered as storage file. They will be uploaded into the Storage bucket. 
The fields should be of type: `jugnu.Types.StorageFile`

The type definition is as follows: 
```
interface StorageFile{
    name: String,       // Name of file
    uri: String         // The file path
}
```