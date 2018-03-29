---
title: Azure CosmosDB tablo API .NET SDK'sını & kaynakları | Microsoft Docs
description: Tüm Azure Cosmos DB tablo yayın tarih, sona erme tarihlerini ve her bir sürümü arasında yapılan değişiklikler dahil olmak üzere API hakkında bilgi edinin.
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/26/2018
ms.author: mimig
ms.openlocfilehash: 2afd7df65e7b223845752fc6bea5bc0ab4d3efd8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB tablo .NET API: İndirme ve sürüm notları
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK yükleme**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API belgeleri**|[.NET API başvuru belgeleri](https://aka.ms/acdbtableapiref)|
|**Hızlı Başlangıç**|[Azure Cosmos DB: .NET ve tablo API ile uygulama oluşturma](create-table-dotnet.md)|
|**Öğretici**|[Azure Cosmos DB: .NET API tabloda geliştirin](tutorial-develop-table-dotnet.md)|
|**Geçerli desteklenen çerçevelerden**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> Önizleme sırasında bir Tablo API hesabı oluşturduysanız, genel kullanıma açık Tablo API SDK’ları ile çalışmak için lütfen [yeni Tablo API hesabı](create-table-dotnet.md#create-a-database-account) oluşturun.
>

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Hatalı biçimlendirilmiş Etag'ler doğrudan modunda için eklenen doğrulama.
* Ağ geçidi modunda sabit LINQ Sorgu hata.
* Zaman uyumlu API'leri artık iş parçacığı havuzu SynchronizationContext ile çalıştırın.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* TableQueryMaxItemCount, TableQueryEnableScan, TableQueryMaxDegreeOfParallelism ve TableQueryContinuationTokenLimitInKb TableRequestOptions için ekleme
* Hata Düzeltmeleri

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Genel kullanılabilirlik sürümü

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* İlk önizleme yayını

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

[WindowsAzure.Storage PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) Önizleme paketini kullanım ve değiştirilmiştir [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) paket. WindowsAzure.Storage PremiumTable SDK'sı 15 Kasım 2018 kullanımdan kaldırılır, hangi zaman isteklerini en Çekildi SDK değil izin verilir.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, bu nedenle, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz önerilir. 

Kullanımdan Kaldırılan SDK kullanarak Azure Cosmos DB yapılan tüm isteklere hizmet tarafından reddedilir.
<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
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

Microsoft.Azure.CosmosDB.Table NuGet paketini kullanmak çalışırken, sorunu düzeltmek için iki seçeneğiniz vardır:

* Microsoft.Azure.CosmosDB.Table paketi ve bağımlılıklarını yüklemek için paket yönetmek konsolunu kullanın. Bunu yapmak için Paket Yöneticisi konsolunda, çözümünüz için aşağıdaki komutu yazın. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Tercih edilen Nuget paketi Yönetim Aracı'nı kullanarak, Microsoft.Azure.CosmosDB.Table yüklemeden önce Microsoft.Azure.Storage.Common Nuget paketini yükleyin.

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Azure Cosmos DB tablo API'si hakkında daha fazla bilgi için bkz: [Azure Cosmos DB tablo API giriş](table-introduction.md). 
