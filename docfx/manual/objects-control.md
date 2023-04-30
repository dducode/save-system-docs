# Objects control

In addition to unity structures and base data types, you 
can write down and read custom classes and structures.

```csharp
writer.Write<MyClass>(myClass);
myClass = reader.ReadObject<MyClass>();
```

To write down (not save) a custom class, 
it must be serializable

```csharp
[Serializable]
public class MyClass {

    public int intValue; // it will be saved

    [SerializeField]
    private bool boolValue; // and this
    
    private int m_intValue; // but not this
    public bool BoolValue { get; set; } // and not this
    
}
```

By default, its will be written as json string. If you want
to override this, you can define extensions. For example

```csharp
public static class SaveSystemExtensions {

    public static void WriteMyClass (this UnityWriter writer, MyClass myClass) {
        writer.Write(myClass.position);
        writer.Write(myClass.rotation);
        writer.Write(myClass.boolValue);
        writer.Write(myClass.intValue);
    }


    public static MyClass ReadMyClass (this UnityReader reader) {
        return new MyClass {
            position = reader.ReadVector3(),
            rotation = reader.ReadRotation(),
            boolValue = reader.ReadBool(),
            intValue = reader.ReadInt()
        };
    }
}
```

After then you can write down the class and read its as 
binary

```csharp
writer.WriteMyClass(myClass);
myClass = reader.ReadMyClass();
```