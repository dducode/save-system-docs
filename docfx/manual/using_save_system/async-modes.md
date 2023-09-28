> [!WARNING]
> The documentation contains the information about
> obsolete API

# Async modes

You can choose how to save and load objects -
in the player loop or in the thread pool. For example,
to save objects in the player loop you must pass
AsyncMode.OnPlayerLoop mode to SaveObject method

```csharp
await DataManager.SaveObjectAsync(fileName, myObjects, AsyncMode.OnPlayerLoop);
```

Async mode defines where the objects will be saved -
in the main thread (in the player loop, single thread
mode), or in a background (in the thread pool,
multithreading mode).

> [!TIP]
> 1. Running in the thread pool will be faster than
     running in the player loop, other things being equal
> 2. If you save one object in the player loop asynchronously,
     it will be equivalent to synchronous save (however,
     in this case onComplete action will be called in the
     next frame)

## How to choose async mode

Usually you will write down and read data in the
thread pool, especially if you are saving and loading
a lot of objects. But if you can't use multithreading
(ex. when you're creating a game for the WebGL platform),
you need to select a saving/loading in the player loop.

|  AsyncMode   |      Threading mode       |
|:------------:|:-------------------------:|
| OnPlayerLoop |   The main thread usage   |
| OnThreadPool | A background thread usage |