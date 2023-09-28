> [!WARNING]
> The documentation contains the information about
> obsolete API

# Advanced data handling

In 99% cases you won't need to this, but you can
write and read data asynchronously by using
`UnityWriter` and `UnityReader`.

In past examples only objects was saved
asynchronously, but inside them data was written
sync

```csharp
public void Save (UnityWriter writer) {
    writer.Write(myData);
}
```

Instead, you can write down data async in the same
way as saving objects

```csharp
public async UniTask Save (UnityWriter writer) {
    await writer.WriteAsync(myData);
}
```

To do this, class of object must be implement the
[`IPersistentObjectAsync`](../../api/SaveSystem.IPersistentObjectAsync.yml)
interface instead of the `IPersistentObject`

```csharp
public class MyObject : IPersistentObjectAsync {
    public async UniTask Save (UnityWriter writer) { }
    public async UniTask Load (UnityReader reader) { }
}
```

To save the object, you can pass it to
`DataManager.Advanced.SaveObjectAsync()` method

```csharp
await DataManager.Advanced.SaveObjectAsync(fileName, myObject);
```

Notice that you don't pass `AsyncMode` to either
`Advanced.SaveObjectAsync()` method, or the data handlers,
because the data handlers handle data the same in both
modes, so only one mode was chosen which support in
every platforms.

## Separately about the mesh

If you're generating a mesh with more than one uv
channel and want to write down the generated mesh,
you must pass the number of channels to the
`WriteAsync()` method

```csharp
await writer.WriteAsync(generatedMeshData, uvChannels: 2);
```

Similarly with reading

```csharp
generatedMeshData = await reader.ReadMeshAsync(uvChannels: 2);
```

By default, asynchronous data handlers write and read
one uv channel

## When I need to this

You can use this when your objects have big arrays of
data. In this case, you can split the job of writing
and reading data. The every array will be read/write
in a separate frame