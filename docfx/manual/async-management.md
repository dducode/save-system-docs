# Asynchronous management

[DataManager]() can also save and load objects
asynchronously. For example, if you're generating
a heavy mesh and want to write down it, you wouldn't
want to do it at runtime, else your game will freeze.

To do it you can persist MyGenerator by using
SaveObjectAsync method. You can await it or continue

```csharp
public async void Saving () {
    await DataManager.SaveObjectAsync(fileName, myGenerator, AsyncMode.OnThreadPool);
    // some actions
}

// or

public void Saving () {
    DataManager.SaveObjectAsync(fileName, myGenerator, AsyncMode.OnThreadPool);
    // some actions
}
```

And then in MyGenerator you can write the mesh

```csharp
public void Save (UnityWriter writer) {
    writer.Write(m_meshData);
}
```

> [!NOTE]
> Before write down the mesh, you must get MeshData 
> struct inside an other method and save it as class 
> field to pass it to writer.Write method. You just 
> can cast Mesh to MeshData, ex.
> ```csharp
> public class MyGenerator : IPersistentObject {
>
>    private MeshData m_meshData;
>    private Mesh m_generatedMesh;
>    
>    public void PreSave () {
>        m_meshData = m_generatedMesh;
>    }
>    
>    // other actions
> }
> ```

For more information about AsyncMode, see the chapter 
[async modes]()

If you are saving a lot of objects, you can observe 
the progress by passing a object to the DataManager 
which implement the 
[IProgress<float\>](https://learn.microsoft.com/en-us/dotnet/api/system.iprogress-1?view=net-7.0) 
interface

```csharp
var progress = new Progress(); // it implements the IProgress<float> interface
await DataManager.SaveObjectsAsync(
    fileName,
    myObjects,
    AsyncMode.OnThreadPool,
    progress
);
```

Also you can cancel operation by passing the 
cancellation token source to the DataManager

```csharp
var source = new CancellationTokenSource();
DataManager.SaveObjectsAsync(
    fileName,
    myObjects,
    AsyncMode.OnThreadPool,
    progress: null,
    source
);
// some actions
source.Cancel();
```

If you cancel the save, DataManager will delete the 
save file.

Finally you can pass a lambda to the manager. 
The lambda will be called after the completion of 
the process

```csharp
DataManager.SaveObjectsAsync(
    fileName,
    myObjects,
    AsyncMode.OnThreadPool,
    progress: null,
    source: null,
    () => Debug.Log("Objects saved")
);
```

When you load objects, you can await the result in 
the same way as when you load objects synchronously

```csharp
public async void Loading () {
    if (await DataManager.LoadObjectsAsync(fileName, myObjects, AsyncMode.OnThreadPool)) {
        // some actions
    }
}
```
