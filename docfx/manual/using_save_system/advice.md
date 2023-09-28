# Backward compatibility advice

For backward compatibility of your data files,
it is recommended define the data reading logic
based on the data file version. For this, you will
write a version of the file, then read its from the
data file and at the end reading data
depending on the file version

You can do it into any class by using any data handler.
You can use 
[Version](https://learn.microsoft.com/en-us/dotnet/api/system.version?view=net-8.0) 
class from System namespace and bind the file version
to an application version

```csharp
private readonly Version m_currentVersion = new Version(Application.version);
```

Let's say you decided to add some data to persist and
then read its. If you read the old file which doesn't
contain this data, you will get the 
[EndOfStreamException](https://learn.microsoft.com/en-us/dotnet/api/system.io.endofstreamexception?view=net-7.0),
because the UnityReader will try to read more bytes
than the data file contains.

The following code example shows how to read data
depending on the file version

```csharp
private readonly Version m_oldVersion1 = new Version("1.0.0");
private readonly Version m_oldVersion2 = new Version("1.1.0");
private readonly Version m_currentVersion =
    new Version(Application.version); // 1.2.0
    

public void Save (UnityWriter writer) {
    writer.Write(m_currentVersion);
    // other actions
}


public void Load (UnityReader reader) {
    var readVersion = reader.ReadVersion();

    if (readVersion >= m_oldVersion1) {
        // performing some actions depending on the file version
    }
    
    // other actions
    
    if (readVersion >= m_oldVersion2) {
        // performing some actions depending on the file version
    }
    
    // other actions
    
    if (readVersion == m_currentVersion) {
        // performing some actions depending on the file version
    }

}
```

This is where you store old versions of your file,
read the version of the data file, compare it to old
versions and current version and read other data
based on the read file version.

You can choose an other way for persist the old versions
(ex. in StreamingAssets folder in json file, or in some
other way). The main thing is that you was comparing
the file version to the old versions and the current one