# Handle objects using data buffer

The Save System provides a more convenient way for data handling.
You can implement [`IStorable`](../../api/SaveSystem.IStorable.yml)
interface in your objects for simplified writing and reading data
using [`DataBuffer`](../../api/SaveSystem.DataBuffer.yml) structure:

```csharp
public class MyObject : MonoBehaviour, IStorable {

    private MeshRenderer m_renderer;
    
    
    public DataBuffer Save () {
        return new DataBuffer {
            vector3 = transform.position,
            quaternion = transfrom.rotation,
            color = m_renderer.material.color  
        };
    }
    
    
    public void Load (DataBuffer buffer) {
        m_renderer.material.color = buffer.color;
        transform.position = buffer.vector3;
        transform.rotation = buffer.quaternion
    }
    
}
```

And then pass it to [`SmartHandler`](../../api/SaveSystem.Handlers.SmartHandler-1.yml):

```csharp
m_smartHalder = ObjectHandlersFactory.CreateSmartHandler(m_filePath, m_object);
// some actions
await m_smartHalder.SaveAsync();
```

Passing of the cancellation token has no effect.

The `SmartHandler` can be passed to `SaveSystemCore` just like
others (see [this part](save-system-core.md) about `SaveSystemCore`)

```csharp
SaveSystemCore.RegisterAsyncHandler(m_smartHalder);
```

What are the benefits of this approach?

1. You don't have to worry about the order of writing and reading.
2. Data collecting is processed by synchronously,
   the system receives a snapshot of the data in an
   once game frame and then writes the data asynchronously.
3. Since a data writing cannot be canceled,
   the data will be written to file immediately
   and then it'll take up less memory.