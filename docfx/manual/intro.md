# About Save System

The save system is a package for saving and loading game data. 
This system allows you to save the state of the game at a
some moment (ex. before quitting) and restore it after 
entering. You can choose one of the following ways to commit
the state of the game:

* Synchronous
* Asynchronous single threading mode (on the player loop)
* Asynchronous multithreading mode (on the thread pool)

In low level you can control unity structures such as
Vector2, Vector3, Vector4, Quaternion, Color, Color32,
Matrix4X4, Mesh.

Also you can write down and read base data types and custom
classes and structures.

## Installing

Read [Installing Manual](installing.md) to install this package

## Using Save System

See [Using Save System](using.md) to learn how to use this system