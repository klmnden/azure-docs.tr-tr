---
title: "Azure Cosmos DB sağlama verimliliğini | Microsoft Docs"
description: "Azure Cosmos DB containsers, koleksiyonları, grafikler ve tablolar için sağlanan işleme ayarlanacağını öğrenin."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: mimig
ms.openlocfilehash: d541bb19ba7e5ecb44c9fe91b1e232d4d9c2170e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Azure Cosmos DB kapsayıcıları için kümesi işleme

Azure portalında veya istemci SDK'ları kullanarak, Azure Cosmos DB kapsayıcıları için işleme ayarlayabilirsiniz. 

Aşağıdaki tabloda, üretilen iş için kapsayıcıları kullanılabilir listelenmektedir:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Tek bir bölüm kapsayıcısı</strong></p></td>
            <td valign="top"><p><strong>Bölümlenmiş kapsayıcısı</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>En düşük işleme</p></td>
            <td valign="top"><p>saniye başına 400 istek birimleri</p></td>
            <td valign="top"><p>saniye başına 2.500 istek birimleri</p></td>
        </tr>
        <tr>
            <td valign="top"><p>En yüksek verimlilik</p></td>
            <td valign="top"><p>saniye başına 10.000 istek birimleri</p></td>
            <td valign="top"><p>Sınırsız</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Azure portalını kullanarak üretilen işi ayarlamak için

1. Yeni bir pencerede açmak [Azure portal](https://portal.azure.com).
2. Sol çubuğunda **Azure Cosmos DB**, veya **daha Hizmetleri** kısımda, daha sonra kaydırın **veritabanları**ve ardından **Azure Cosmos DB**.
3. Cosmos DB hesabınızı seçin.
4. Yeni pencerede **Veri Gezgini (Önizleme)** Gezinti menüsünde.
5. Yeni pencerede, veritabanı ve kapsayıcısını genişletin ve ardından **ölçek & ayarları**.
6. Yeni pencerede yeni işleme değeri yazın **işleme** kutusuna ve ardından **kaydetmek**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-documentdb-api-for-net"></a>.NET için DocumentDB API'sini kullanarak üretilen işi ayarlamak için

```C#
//Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

//Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

## <a name="throughput-faq"></a>Üretilen iş ile ilgili SSS

**My verimlilik değerinden 400 RU/s ayarlayabilir miyim?**

Cosmos DB tek bölüm koleksiyonları (2500 RU/s bölümlenmiş koleksiyonlar için en düşük gereksinimdir) üzerinde kullanılabilir en düşük işleme 400 RU/s olur. İstek birimleri 100 RU/s aralıklarla ayarlanmış, ancak işleme 100 RU/s veya herhangi bir değer 400 RU/s küçük olacak şekilde ayarlanamaz. Geliştirmek ve Cosmos DB sınamak için uygun maliyetli bir yöntem arıyorsanız, ücretsiz kullanabileceğiniz [Azure Cosmos DB öykünücüsü](local-emulator.md), hangi hiçbir ücret yerel olarak dağıtabilirsiniz. 

**MongoDB API kullanarak througput nasıl ayarlarım?**

Üretilen iş ayarlamak için MongoDB API uzantısı yok. DocumentDB API kullanan gösterildiği gibi önerilir [.NET için DocumentDB API'sini kullanarak üretilen işi ayarlamak için](#set-throughput-sdk).

## <a name="next-steps"></a>Sonraki adımlar

Sağlama ve devam eden planet ölçekli Cosmos DB ile hakkında daha fazla bilgi için bkz: [bölümleme ve Cosmos DB ile ölçeklendirme](partition-data.md).
