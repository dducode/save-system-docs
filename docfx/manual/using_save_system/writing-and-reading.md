# Writing and reading data

To write and read data, the save system provides two
classes -
[`UnityWriter`](../../api/SaveSystem.UnityHandlers.UnityWriter.yml)
and
[`UnityReader`](../../api/SaveSystem.UnityHandlers.UnityReader.yml).
They can write and read data respectively. Your
`MonoBehaviour` classes get these data handler from the
`DataManager` when you're saving or loading them. To write and
read data you would write:

```csharp
public void Save (UnityWriter writer) {
    writer.Write(transform.position);
    writer.Write(transform.rotation);

    writer.Write(material.color);
    
    writer.Write(myClass);
    writer.Write(myClasses);
}


public void Load (UnityReader reader) {
    transform.position = reader.ReadVector3();
    transform.rotation = reader.ReadRotation();

    material.color = reader.ReadColor();
    
    myClass = reader.ReadObject<MyClass>();
    myClasses = reader.ReadArrayObjects<MyClass>();
}
```

> [!IMPORTANT]
> You must read data in the same order as you write it.
> If you don't want to worry about this, implement 
> [`IStorable`](../../api/SaveSystem.IStorable.yml) interface and use
> [`SmartHandler`](../../api/SaveSystem.Handlers.SmartHandler-1.yml).
> Read [this](data-buffer.md) article for more information.

To learn how to control custom classes and structs see 
[this part](objects-control.md).