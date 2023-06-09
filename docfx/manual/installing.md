﻿# Installing

Do these steps to install the package:

1. Click on the green button "Code"

   ![](../images/Screenshot_1.png)

2. Then click on copy button, in "HTTPS" tab 

    ![](../images/Screenshot_3.png)

3. In Unity open the Package Manager window (Window/Package Manager)

    ![](../images/Screenshot_6.png)

4. Then click on plus sign and select "Add package from git URL"

    ![](../images/Screenshot_2.png)

5. Insert copied link and click on "Add"

    ![](../images/Screenshot_4.png)

6. After a while the package will be added to your project

    ![](../images/Screenshot_5.png)

However, you can see some errors in the debug console. The
save system has a dependency on UniTask.
Unfortunately, Unity Package Manager cannot resolve 
dependencies between Git repositories, so you need to install 
UniTask manually. 

Copy this link 
`https://github.com/Cysharp/UniTask.git?path=src/UniTask/Assets/Plugins/UniTask`
and install UniTask in the same way as you installed
the save system