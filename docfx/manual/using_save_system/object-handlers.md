# Object Handlers

If you read "Asynchronous Management" chapter, you
may have noticed that using the DataManager's async methods
is a few fiddly. 
[`ObjectHandler`](../../api/SaveSystem.Handlers.ObjectHandler-1.yml) 
and 
[`AsyncObjectHandler`](../../api/SaveSystem.Handlers.AsyncObjectHandler-1.yml)
classes was created for this reason. Just look to example:

```csharp
// Earlier you was forced to use this method
await DataManager.SaveObjectsAsync(
    fileName,
    myObjects,
    AsyncMode.OnThreadPool,
    onComplete: () => Debug.Log("Objects saved")
);

// However you can use Object Handler class instead this
HandlingResult result = await ObjectHandlersFactory
   .CreateAsyncHandler(myObjects, fileName)
   .SaveAsync();
   
if (result == HandlingResult.Success)
    Debug.Log("Objects saved");
```

> One important change - `AsyncObjectHandler` doesn't accept onComplete
> callback, it returns HandlingResult instead this.

## More about the handlers

In short, the `ObjectHandler` created for handling of 
objects which implement `IPersistentObject` interface.
The `AsyncObjectHandler` handle `IPersistentObjectAsync` objects.
None of them support async modes, but you still can pass
`IProgress<float>` object to observe progress

```csharp
await asyncHandler.ObserveProgress(m_progress).SaveAsync();
// or
await asyncHandler.ObserveProgress(m_savingProgress, m_loadingProgress).SaveAsync();
```

Also you can create an `ObjectHandler`, set
its parameters and call saving later:

```csharp
private ObjectHandler<MyObject> m_handler;
private MyObject[] m_objects;

private void Start() {
    m_handler = ObjectHandlersFactory.CreateHandler(
        localFilePath: "MyGameProgress/GameObjects.bytes", m_objects
    );
}


private void OnApplicationQuit() {
    m_handler.Save();
}
```

If you need to async save objects on the thread pool, you can write

```csharp
await Task.Run(handler.Save, cancellationToken: token);
// or
await UniTask.RunOnThreadPool(() => handler.Save(), cancellationToken: token);
```

The `AsyncObjectHandler` accepts a cancellation token unlike the
`DataManager` (it accepts a cancellation token source).

```csharp
await DataManager.SaveObjectsAsync(filePath, objects, AsyncMode.OnPlayerLoop, source: m_tokenSource);
await asyncObjectHandler.SaveAsync(m_tokenSource.Token);
```

Since an async data processing can be canceled, `AsyncObjectHandler`
will be write the data to the buffer at first, and then to a file.
Otherwise (if it'll write directly to a file), when canceling
the operation, the data would be considered invalid.
This protects the file, but writes a more bytes than
directly writing.