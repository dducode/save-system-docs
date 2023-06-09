﻿# Using Save System

Saving and loading data consist of the following steps:

* Implement the
  [IPersistentObject](../api/SaveSystem.IPersistentObject.yml) or
  [IPersistentObjectAsync](../api/SaveSystem.IPersistentObjectAsync.yml)
  interface in your object
* Implement Save and Load methods in it:
    * Write down data in the save method by using [UnityWriter](../api/SaveSystem.UnityWriter.yml)
    * Read data in the load method by using [UnityReader](../api/SaveSystem.UnityReader.yml)
* Pass the object to the DataManager:
    * Pass it to SaveObject method for saving
    * Or pass it to LoadObject to load

See [saving and loading data](saving-and-loading.md)
for more information about using of the DataManager

Then see [writing and reading data](writing-and-reading.md) for more about of
the UnityWriter and the UnityReader

And at the end see more information about asynchronous
data management in [this manual](async-management.md)