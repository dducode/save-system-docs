# Saving and loading objects

[DataManager]()
is the main class for data management. It contains several
methods for loading and saving objects. If you want to
keep a MonoBehaviour object, you can write

```csharp
var myObject = FindObjectOfType<MyObject>();
DataManager.SaveObject(fileName, myObject);
// "fileName" is the name of the data file without
// full path and its extension (ex. "myDataFile")
```

To load the object

```csharp
if (DataManager.LoadObject(fileName, myObject)) {
    // some actions
}
```

The LoadObject method returns true if it successfully
loaded object. It returns false if a save file is missing.

The class of object must implement the
[IPersistentObject]()
interface and Save and Load methods.

```csharp
public class MyObject : MonoBehaviour, IPersistentObject {
    public void Save (UnityWriter writer) { }
    public void Load (UnityReader reader) { }
}
```

The DataManager can also save and load multiple objects
as array