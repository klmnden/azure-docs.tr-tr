---
title: Azure Cosmos DB tablo API .NET SDK'sı & kaynakları | Microsoft Docs
description: Tüm yayın tarihleri, sona erme tarihlerini ve her bir sürümü arasında yapılan değişiklikler dahil olmak üzere Azure Cosmos DB tablo API'si hakkında bilgi edinin.
services: cosmos-db
author: rnagpal
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/26/2018
ms.author: rnagpal
ms.openlocfilehash: 2fba67b247ad0b53e11ca012969163a68013e82f
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126720"
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB tablosu .NET API'si: İndirme ve sürüm notları
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK'sını indirme**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API belgeleri**|[.NET API başvuru belgeleri](https://aka.ms/acdbtableapiref)|
|**Hızlı Başlangıç**|[Azure Cosmos DB: .NET ve tablo API'si ile bir uygulama derleme](create-table-dotnet.md)|
|**Öğretici**|[Azure Cosmos DB:. NET'te tablo API'si ile geliştirme](tutorial-develop-table-dotnet.md)|
|**Geçerli desteklenen çerçevesi**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> Önizleme sırasında bir Tablo API hesabı oluşturduysanız, genel kullanıma açık Tablo API SDK’ları ile çalışmak için lütfen [yeni Tablo API hesabı](create-table-dotnet.md#create-a-database-account) oluşturun.
>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name113113"></a><a name="1.1.3"/>1.1.3
* Sabit Nuget Paket bağımlılıklarını Microsoft.Azure.Storage.Common ve Microsoft.Azure.DocumentDB.
* Hata düzeltmeleri JsonConvert.DefaultSettings yapılandırıldığında tablo serileştirme hakkında.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Ek doğrulama için hatalı biçimlendirilmiş Etag'ler doğrudan modunda.
* Ağ geçidi modunda LINQ Sorgu hata düzeltildi.
* Zaman uyumlu API'leri artık SynchronizationContext iş parçacığı havuzuyla çalıştırın.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* TableQueryMaxItemCount, TableQueryEnableScan TableQueryMaxDegreeOfParallelism ve TableQueryContinuationTokenLimitInKb TableRequestOptions için ekleyin
* Hata düzeltmeleri

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Genel kullanılabilirlik sürümü

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* İlk önizleme yayını

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

[WindowsAzure.Storage-PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) Önizleme paket kullanım dışı ve yerine [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) paket. WindowsAzure.Storage-PremiumTable SDK'sı 15 Kasım 2018'de kullanımdan kaldırılacaktır, hangi zaman isteklerini devre dışı bırakılan SDK'sını değil izin verilir.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, bu nedenle, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz önerilir. 

Devre dışı bırakılan bir SDK'sı kullanarak Azure Cosmos DB yapılan tüm isteklere hizmet tarafından reddedilir.
<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [1.1.3](#1.1.3) |17 Temmuz 2018|--- |
| [1.1.1](#1.1.1) |26 Mart 2018|--- |
| [1.1.0](#1.1.0) |21 Şubat 2018|--- |
| [1.0.0](#1.0.0) |15 Kasım 2017|--- |
| [0.9.0-Preview](#0.9.0-preview) |11 Kasım 2017 |--- |

## <a name="troubleshooting"></a>Sorun giderme

Hata iletisi alırsanız 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

Microsoft.Azure.CosmosDB.Table NuGet paketini kullanmaya çalıştığınızda, sorunu düzeltmek için iki seçeneğiniz vardır:

* Microsoft.Azure.CosmosDB.Table paketi ve bağımlılıklarını yüklemek için paket yönetme Konsolu'nu kullanın. Bunu yapmak için çözümünüz için Paket Yöneticisi konsolunda aşağıdaki komutu yazın. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Nuget paket, tercih edilen Yönetim Aracı'nı kullanarak, Microsoft.Azure.CosmosDB.Table yüklemeden önce Microsoft.Azure.Storage.Common Nuget paketini yükleyin.

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Azure Cosmos DB tablo API'si hakkında daha fazla bilgi için bkz: [Azure Cosmos DB tablo API'sine giriş](table-introduction.md). 
