---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 69dc0e1c14bc88cdbf0aa48700f95058ba759cc0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140219"
---
1. Çözüm görünümünde (veya **Çözüm Gezgini** Visual Studio'da), sağ tıklayın **bileşenleri** klasörünü tıklatın **daha alma bileşenleri...** , arama **Google Cloud Messaging istemcisi** bileşen ve projeye ekleyin.
2. ToDoActivity.cs proje dosyasını açın ve aşağıdaki using deyimini sınıfa:

    ```csharp
    using Gcm.Client;
    ```

3. İçinde **ToDoActivity** sınıfında, aşağıdaki yeni kodu ekleyin: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Bu, mobil istemci örneği anında iletme işleyicisi hizmeti işleminin erişmenize olanak sağlar.
4. Aşağıdaki kodu ekleyin **OnCreate** yöntemi, sonra **MobileServiceClient** oluşturulur:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

**ToDoActivity** artık anında iletme bildirimleri ekleme için hazır.
