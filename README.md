# Array Synchronization

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

print(array[1])
array[1] = "dos"
print(array[1])
```
