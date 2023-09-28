# Documentation to code of Save System package

## Base classes of the save system:

* [`DataManager`](../api/SaveSystem.DataManager.yml) -
  saving and loading your objects
* [`SaveSystemCore`](../api/SaveSystem.Core.SaveSystemCore.yml) -
the main class of the Save System for all types of saving
* [`ObjectHandler`](../api/SaveSystem.Handlers.ObjectHandler-1.yml)
and [`AsyncObjectHandler`](../api/SaveSystem.Handlers.AsyncObjectHandler-1.yml) -
  more convenient classes for saving and loading of the objects
* [`ObjectHandlersFactory`](../api/SaveSystem.Handlers.ObjectHandlersFactory.yml) -
  use it to create the handlers
* [`CheckPointsFactory`](../api/SaveSystem.CheckPoints.CheckPointsFactory.yml) -
  use it to create a checkpoints at runtime
* [`UnityWriter`](../api/SaveSystem.UnityHandlers.UnityWriter.yml) -
  writing your data
* [`UnityReader`](../api/SaveSystem.UnityHandlers.UnityReader.yml) -
  reading your data

## Dependencies:

* [UniTask](https://github.com/Cysharp/UniTask) -
  adapts async\await to Unity