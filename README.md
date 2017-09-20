# Arrays

## `joined`

```swift
var someArray = ["one", "two", "three"]
someArray.joined(separator: ", ") // "one, two, three"
```

# Strings

## Displaying Rounded Numbers

### `String(format: String, _ arguments: CVarArg...)`

```swift
/*
  Syntax for format placeholder:

  `[parameter][flags][width][.precision][length]type`

  width:
    - minimum number of characters to output
    - a leading zero is interpreted as the zero-padding flag

  precision:
    - maximum limit on the output
    - for floating point types specifies number of digits (rounded)
 */
print(String(format: "%.2f", 3.14159)) // 3.14
print(String(format: "%.0f", -16.5)) // -16
```
[printf format placeholder specification][round-1]

### `lround`

`lround` works similarly, however it rounds halfway cases away from zero. Using a string formmater
is probably the preferred approach.

[How to round a Double to the nearest Int in swift?][round-2]


```swift
print(lround(-16.5)) // -17
```

[round-1]:https://en.wikipedia.org/wiki/Printf_format_string#Format_placeholder_specification
[round-2]:https://stackoverflow.com/questions/26350977/how-to-round-a-double-to-the-nearest-int-in-swift

# Threads

## Synchronization

### Arrays

```swift
class SynchronizedArray<T>
{
    private let syncQueue: DispatchQueue
    private var array: [T]
    
    init(_ array: [T],
         syncQueue: DispatchQueue = DispatchQueue(label: "sync-queue-\(UUID().uuidString)")) {
        
        self.array = array
        self.syncQueue = syncQueue
    }
    
    subscript(index: Int) -> T {
        get { return syncQueue.sync { self.array[index] } }
        set(newValue) { syncQueue.async { self.array[index] = newValue } }
    }
}

var array = SynchronizedArray(["one", "two", "three"])

print(array[1]) // two
array[1] = "dos"
print(array[1]) // dos
```
