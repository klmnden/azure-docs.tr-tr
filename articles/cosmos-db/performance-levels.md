---
title: Kullanımdan Azure Cosmos DB performans düzeyleri | Microsoft Docs
description: Daha önce Azure Cosmos DB'de kullanılabilir S1, S2 ve S3 performans düzeyleri hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d1bb7551e6dfb6c42853ab95096f17f5285c69c1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796657"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>S1, S2 ve S3 performans düzeyleri devre dışı bırakma

> [!IMPORTANT] 
> Bu makalede açıklanan S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılacak ve artık yeni Azure Cosmos DB hesapları için kullanılabilir.
>

Bu makalede, S1, S2 ve S3 performans düzeyleri genel bir bakış sağlar ve bu performans düzeyleri kullanmak koleksiyonları tek geçirilen bölümlenmiş koleksiyonlar nasıl olabilir açıklanır. Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

- [S1, S2 ve S3 performans düzeyleri neden olan kaldırılan?](#why-retired)
- [Nasıl tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1 için S2, S3 performans düzeyleri karşılaştırması nedir?](#compare)
- [Verilerimi kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?](#uninterrupted-access)
- [Nasıl kendi koleksiyonuma geçişten sonra değişir mi?](#collection-change)
- [Tek bölüm koleksiyonları geçişi sonra nasıl my faturalama değişir mi?](#billing-change)
- [Ne birden fazla 10 GB depolama alanı ihtiyacım var?](#more-storage-needed)
- [Planlanan geçişten önce performans düzeyleri arasında S1, S2 ve S3 değiştirebilirim?](#change-before)
- [S3 performans düzeyleri üzerinde kendi tek bölüm koleksiyonları nasıl S1, S2, geçişini?](#migrate-diy)
- [Nasıl bir EA müşteri ben varsa ı etkilenen?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>S1, S2 ve S3 performans neden olan kaldırılan düzeyleri?

S1, S2 ve S3 performans düzeyleri standart Azure Cosmos DB teklif sağlar esnekliği sağlamaz. S1, S2, S3 performans düzeyleri ile üretilen iş ve depolama kapasitesini önceden ayarlanmış ve esneklik sunmadı. Azure Cosmos DB işleme ve depolama, özelleştirme yeteneği gereksinimleriniz değiştikçe ölçeklendirme yeteneğinizi çok daha fazla esneklik sunumu olarak sunar.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Nasıl tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1 için S2, S3 performans düzeyleri karşılaştırması nedir?

Aşağıdaki tabloda tek bölüm koleksiyonları, bölümlenmiş koleksiyonlar ve S1, S2, S3 performans düzeyleri kullanılabilir işleme ve depolama seçenekleri karşılaştırılmaktadır. ABD Doğu 2 bölge için örnek aşağıda verilmiştir:

|   |Bölümlenmiş koleksiyonu|Tek bir bölüm koleksiyonu|S1|S2|S3|
|---|---|---|---|---|---|
|En yüksek verimlilik|Sınırsız|10 K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|En az aktarım hızı|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maksimum depolama|Sınırsız|10 GB|10 GB|10 GB|10 GB|
|Fiyat (ay)|Verimlilik: $6 / 100 RU/s<br><br>Depolama: $ 0,25/GB|Verimlilik: $6 / 100 RU/s<br><br>Depolama: $ 0,25/GB|$25 ABD DOLARI|$50 ABD DOLARI|$100 ABD DOLARI|

EA müşteri misiniz? Aksi takdirde bkz [nasıl ı etkilenen EA müşteri ben varsa?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Verilerimi kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?

S1, S2 ve S3 koleksiyonu varsa, koleksiyonu tek bölümlü bir koleksiyon için program aracılığıyla geçirmelisiniz [.NET SDK kullanarak](#migrate-diy). 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Nasıl kendi koleksiyonuma geçişten sonra değişir mi?

S1 koleksiyonu varsa, bunları 400 RU/s üretilen iş ile bir tek bölümlü bir koleksiyon geçirebilirsiniz. Tek bölüm koleksiyonları ile kullanılabilen en düşük işleme 400 RU/s olur. Çok 150 RU/s için kullanabileceğiniz ödeme yapıyorsanız olmayan şekilde S1 koleksiyonunuzu ve 250 RU/s – ödeme ancak maliyeti 400 RU/s tek bölümlü bir koleksiyon içinde yaklaşık aynıdır.

S2 koleksiyonu varsa, bunları 1 K RU/s ile bir tek bölümlü bir koleksiyon geçirebilirsiniz. Herhangi bir değişiklik, üretilen iş düzeyi görürsünüz.

S3 koleksiyonu varsa, bunları 2,5 K RU/s ile bir tek bölümlü bir koleksiyon geçirebilirsiniz. Herhangi bir değişiklik, üretilen iş düzeyi görürsünüz.

Koleksiyon geçirdikten sonra her durumda, üretilen iş düzeyi özelleştirebilir ya da yukarı ve aşağı kullanıcılarınız için düşük gecikmeli erişim sağlamak için gerektiği şekilde ölçeklendirme kuramaz. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Tek bölüm koleksiyonları geçişi sonra nasıl my faturalama değişir mi?

Varsayılmıştır 10 S1 koleksiyonları, 1 GB depolama alanı BİZE Doğu bölgede her sahiptir ve 10 tek bölüm koleksiyonları 400 RU/sn (en düşük düzey), bu 10 S1 koleksiyonlarını geçirme. Tam bir ay için 10 tek bölüm koleksiyonları tutarsanız faturanızı şu şekilde görünür:

![Nasıl S1 10 koleksiyonlar için fiyatlandırma tek bölümlü bir koleksiyon için fiyatlandırma kullanarak 10 koleksiyonları karşılaştırır](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Ne birden fazla 10 GB depolama alanı ihtiyacım var?

S1, S2 ve S3 performans düzeyine sahip bir koleksiyona sahip, mı da 10 GB depolama alanı kullanılabilir Azure Cosmos DB veri geçiş aracı verilerinizi ile bölümlenmiş bir koleksiyon neredeyse geçirmek için kullanabileceğiniz sahip tek bölümlü bir koleksiyon, sahip Sınırsız depolama. Bölümlendirilmiş bir koleksiyon avantajları hakkında daha fazla bilgi için bkz [bölümleme ve Azure Cosmos DB'de ölçeklendirme](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Planlanan geçişten önce performans düzeyleri arasında S1, S2 ve S3 değiştirebilirim?

S1, S2 ve S3 performans yalnızca var olan hesapları değiştirilebilir ve performans düzeyi katmanları program aracılığıyla alter [.NET SDK kullanarak](#migrate-diy). Tek bölümlü bir koleksiyon için S1, S3 veya S3 değiştirirseniz, S1, S2 ve S3 performans düzeyleri döndüremez.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>S3 performans düzeyleri üzerinde kendi tek bölüm koleksiyonları nasıl S1, S2, geçişini?

S1, S2 ve S3 performans düzeylerinden tek bölüm koleksiyonları için programlı olarak geçirebileceğiniz [.NET SDK kullanarak](#migrate-diy). Tek bölüm koleksiyonları ile kullanılabilen esnek işleme seçeneklerinden yararlanmak için planlanan geçiş işleminden önce kendi bunu yapabilirsiniz.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>Tek bölüm koleksiyonları için .NET SDK kullanarak geçirme

Bu bölüm, yalnızca bir koleksiyona ait performansının değiştirilmesi kapsar kullanarak düzey [SQL .NET API](sql-api-sdk-dotnet.md), ancak bizim diğer SDK için benzer bir işlemdir.

Saniye başına 5.000 istek birimlerine koleksiyonu verimlilik değiştirmek için bir kod parçacığı aşağıda verilmiştir:
    
```csharp
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Ziyaret [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) ek örnekler görüntüleyip bizim teklif yöntemleri hakkında daha fazla bilgi için:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Nasıl bir EA müşteri ben varsa ı etkilenen?

EA müşteriler kendi geçerli sözleşmenin sonuna kadar korumalı fiyat olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Fiyatlandırma ve Azure Cosmos DB ile verileri yönetme hakkında daha fazla bilgi edinmek için şu kaynakları araştırın:

1.  [Cosmos DB'de veri bölümlendirme](sql-api-partition-data.md). Tek bölümlü bir kapsayıcı ve bölümlenmiş kapsayıcıları yanı sıra, sorunsuz bir şekilde ölçeklendirmek için bölümleme stratejisine uygulama ipuçları arasındaki farkı anlama.
2.  [Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). Üretilen iş sağlama ve depolama tüketme maliyeti hakkında bilgi edinin.
3.  [İstek birimleri](request-units.md). Farklı işlem türleri için örneğin okuma, yazma, sorgu işleme tüketiminin anlayın.
