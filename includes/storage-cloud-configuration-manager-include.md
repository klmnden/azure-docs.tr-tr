---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 761c3a9aecadd9c1eabdb586f95c47e2988720d8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114618"
---
[.NET için Microsoft Azure Yapılandırma Yöneticisi Kitaplığı](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/), yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar. [CloudConfigurationManager sınıfı](https://msdn.microsoft.com/library/azure/mt634650.aspx), Azure sanal cihazdaki veya Azure bulut hizmetindeki bir masaüstünde, bir mobil cihazda istemci uygulamasının çalışıp çalışmadığını göz ardı ederek yapılandırma ayarlarını ayrıştırır.

CloudConfigurationManager paketine başvurmak için şu `using` yönergeyi ekleyin:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage;
```

Burada, yapılandırma dosyasından bir bağlantı dizesinin nasıl alındığını gösteren bir örnek bulunmaktadır:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Azure Yapılandırma Yöneticisi'ni kullanmak isteğe bağlıdır. .NET Framework'ün [ConfigurationManager sınıfı](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) gibi bir API de kullanabilirsiniz.

