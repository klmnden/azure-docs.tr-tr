---
title: Azure Cosmos DB sağlama verimliliğini | Microsoft Docs
description: Azure Cosmos DB containsers, koleksiyonları, grafikler ve tablolar için sağlanan işleme ayarlanacağını öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 0a53bb0a23fae386abbe71de944b073cbb93d502
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers"></a>Ayarlama ve verimlilik için Azure Cosmos DB kapsayıcılar alma

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

## <a id="GetLastRequestStatistics"></a>MongoDB API'nin GetLastRequestStatistics komutunu kullanarak işleme al

Özel bir komut MongoDB API destekler *getLastRequestStatistics*, belirtilen işlem için istek ücret alma.

Örneğin, istek ücreti doğrulamak istediğiniz işlem Mongo kabuğunu yürütün.
```
> db.sample.find()
```

Ardından, komutu yürütün *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Bu durum dikkate alınarak, uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir uygulamanız tarafından kullanılan bir temsili öğesi karşı normal işlemleri çalıştırılması ile ilişkili istek birimi ücret kaydedin ve ardından tahmin yöntemdir saniyede gerçekleştirmek düşündüğünüz işlemlerinin sayısı.

> [!NOTE]
> Hangi boyutu ve Dizinli Özellikler sayısı bakımından önemli ölçüde farklılık gösterecektir öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlem istek birimi ücret kayıt *türü* tipik öğesi.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>MongoDB API portal ölçümleri kullanarak işleme alma

En basit yolu MongoDB API veritabanınızı kullanmak için bir iyi isteğinin birim ücretleri kestirmek [Azure portal](https://portal.azure.com) ölçümleri. İle *istek sayısı* ve *isteği ücret* grafikler, kaç tane istek birimleri her işlem harcayan ve kaç tane istek birimleri bunlar tüketen birbirine göre tahmini alabilirsiniz.

![MongoDB API portal ölçümleri][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> MongoDB API aşan ayrılmış işleme sınırları
Tüketim oranını sağlanan işleme hızının altına düşene kadar bir kapsayıcı için sağlanan işleme aşan uygulamalar oranı sınırlı olur. Bir hızını sınırlama ortaya çıktığında, arka uç istekle erken önlem sona erer bir `16500` hata kodu - `Too Many Requests`. Varsayılan olarak, MongoDB API otomatik olarak en fazla 10 kez döndürmeden önce yeniden deneme bir `Too Many Requests` hata kodu. Birçok alıyorsanız `Too Many Requests` hata kodları, ya da yeniden deneme mantığı, uygulamanızın hata yordamları işleme ekleme düşünmek isteyebilirsiniz veya [kapsayıcı sağlanan verimliliğini artırmak](set-throughput.md).

## <a name="throughput-faq"></a>Üretilen iş ile ilgili SSS

**My verimlilik değerinden 400 RU/s ayarlayabilir miyim?**

400 RU/s (1000 RU/s bölümlenmiş kapsayıcıları için en düşük gereksinimdir) Cosmos DB tek bölüm kapsayıcılarında kullanılabilir en düşük işleme ' dir. İstek birimleri 100 RU/s aralıklarla ayarlanmış, ancak işleme 100 RU/s veya herhangi bir değer 400 RU/s küçük olacak şekilde ayarlanamaz. Geliştirmek ve Cosmos DB sınamak için uygun maliyetli bir yöntem arıyorsanız, ücretsiz kullanabileceğiniz [Azure Cosmos DB öykünücüsü](local-emulator.md), hangi hiçbir ücret yerel olarak dağıtabilirsiniz. 

**MongoDB API kullanarak througput nasıl ayarlarım?**

Üretilen iş ayarlamak için MongoDB API uzantısı yok. SQL API kullanan gösterildiği gibi önerilir [.NET için SQL API'yi kullanarak üretilen işi ayarlamak için](#set-throughput-sdk).

## <a name="next-steps"></a>Sonraki adımlar

Sağlama ve devam eden planet ölçekli Cosmos DB ile hakkında daha fazla bilgi için bkz: [bölümleme ve Cosmos DB ile ölçeklendirme](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png