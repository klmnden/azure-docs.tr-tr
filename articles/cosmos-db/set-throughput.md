---
title: Azure Cosmos DB sağlama verimliliğini | Microsoft Docs
description: Azure Cosmos DB containsers, koleksiyonları, grafikler ve tablolar için sağlanan işleme ayarlanacağını öğrenin.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: mimig
ms.openlocfilehash: b9ad94276287a5e46a9d3b3b9332547fba5627e9
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
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
            <td valign="top"><p>saniye başına 1000 isteği birim</p></td>
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
2. Sol çubuğunda **Azure Cosmos DB**, veya **tüm hizmetleri** kısımda, daha sonra kaydırın **veritabanları**ve ardından **Azure Cosmos DB**.
3. Cosmos DB hesabınızı seçin.
4. Yeni pencerede **Veri Gezgini** Gezinti menüsünde.
5. Yeni pencerede, veritabanı ve kapsayıcısını genişletin ve ardından **ölçek & ayarları**.
6. Yeni pencerede yeni işleme değeri yazın **işleme** kutusuna ve ardından **kaydetmek**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>.NET için SQL API'yi kullanarak üretilen işi ayarlamak için

Aşağıdaki kod parçacığını geçerli işleme alır ve 500 RU/s değiştirir. Tam kod örneği için bkz: [CollectionManagement](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L188-L216) GitHub projede.

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 500 request units per second
offer = new OfferV2(offer, 500);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Java için SQL API'yi kullanarak tarafından üretilen işi ayarlamak için

Aşağıdaki kod parçacığını geçerli işleme alır ve 500 RU/s değiştirir. Tam bir kod örneği için bkz: [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) github'da dosya. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Üretilen iş ile ilgili SSS

**My verimlilik değerinden 400 RU/s ayarlayabilir miyim?**

400 RU/s (1000 RU/s bölümlenmiş kapsayıcıları için en düşük gereksinimdir) Cosmos DB tek bölüm kapsayıcılarında kullanılabilir en düşük işleme ' dir. İstek birimleri 100 RU/s aralıklarla ayarlanmış, ancak işleme 100 RU/s veya herhangi bir değer 400 RU/s küçük olacak şekilde ayarlanamaz. Geliştirmek ve Cosmos DB sınamak için uygun maliyetli bir yöntem arıyorsanız, ücretsiz kullanabileceğiniz [Azure Cosmos DB öykünücüsü](local-emulator.md), hangi hiçbir ücret yerel olarak dağıtabilirsiniz. 

**MongoDB API kullanarak througput nasıl ayarlarım?**

Üretilen iş ayarlamak için MongoDB API uzantısı yok. SQL API kullanan gösterildiği gibi önerilir [.NET için SQL API'yi kullanarak üretilen işi ayarlamak için](#set-throughput-sdk).

## <a name="next-steps"></a>Sonraki adımlar

Sağlama ve devam eden planet ölçekli Cosmos DB ile hakkında daha fazla bilgi için bkz: [bölümleme ve Cosmos DB ile ölçeklendirme](partition-data.md).
