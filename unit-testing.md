# Unit Testing

## Core Data Managed Objects

To write a test for code that requires an `NSManagedObject`, it is not possible to instantiate a `NSManagedObject` instance directly.

[NSManagedObject Documentation][cd-1]
> A managed object is associated with an entity description (an instance of NSEntityDescription) that provides metadata about the object (including the name of the entity that the object represents and the names of its attributes and relationships) and with a managed object context that tracks changes to the object graph. It is important that a managed object is properly configured for use with Core Data. If you instantiate a managed object directly, you must call the designated initializer (
init(entity:insertInto:)
).

Otherwise, an error message may result at runtime:
```
[EmployeeManagedObject setFirstName:]: unrecognized selector sent to instance
```

An alternative is to have a protocol that the `NSManagedObject` subclass conforms to that mirrors the properties that the code or test requires.

```swift

protocol EmployeeType
{
    var firstName: String? { get set }
    var lastName: String? { get set }
    var employeeUUID: String? { get set }
}

@objc(EmployeeManagedObject)
class EmployeeManagedObject: NSManagedObject
{
    @NSManaged var firstName: String?
    @NSManaged var lastName: String?
    @NSManaged var employeeUUID: String?
}

class MockEmployee: EmployeeType
{
    var firstName: String?
    var lastName: String?
    var employeeUUID: String?

    init() {}
}

let mock = MockEmployee()
mock.firstName = "mock first name"
mock.lastName = "mock last name"
mock.employeeUUID = "mock employee UUID"

```

[cd-1]: https://developer.apple.com/documentation/coredata/nsmanagedobject