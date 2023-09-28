# Dynamic objects handling

Let's first define the terms - by static objects we mean
such objects that are already in the scene
and will never be destroyed;
By dynamic objects we mean such objects that aren't initially
in the scene, but they'll be spawned later.

You can create empty `ObjectHandler` (that hasn't any static objects)
and pass dynamic objects to it later, but first you should
pass a factory function to it (this is necessary to automatically
spawn dynamic objects before loading)

```csharp
m_handler = ObjectHandlersFactory.CreateHandler(
    filePath, () => Object.Instantiate(m_prefab)
);
m_handler.Load();
// ...
m_handler.AddObject(m_spawnedObject);
```

Also you can pass the factory function to other handlers

```csharp
m_oldHandler.SetFactoryFunc(() => Object.Instantiate(m_prefab));
```

This allows you to choose an objects spawn method other than
the default. For example, you use Zenject framework and you
need to create an object from a factory to
inject dependencies into it, so, you could write

```csharp
m_oldHandler.SetFactoryFunc(() => m_factory.Create(m_prefab));
```

The handlers automatically remove destroyed objects before saving