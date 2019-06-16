---
title: Devre dışı bırakılan Azure Cosmos DB performans düzeyleri
description: Daha önce Azure Cosmos DB'de kullanılabilen S1, S2 ve S3 performans düzeyleri hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.openlocfilehash: 06fa98ae4acc2252d8866858ed0e2194ed84ff79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60928301"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>S1, S2 ve S3 performans düzeyleri devre dışı bırakma

> [!IMPORTANT] 
> Bu makalede ele alınan S1, S2 ve S3 performans düzeyleri devre dışı bırakılıyor ve artık yeni Azure Cosmos DB hesapları için kullanılabilir.
>

Bu makalede, S1, S2 ve S3 performans düzeyleri genel bir bakış sağlar ve bu performans düzeylerini kullanan koleksiyonları tek geçirilen bölümlenmiş koleksiyonlar nasıl olabileceğini açıklar. Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

- [S1, S2 ve S3 performans düzeyleri neden olan devre dışı bırakılıyor?](#why-retired)
- [Nasıl tek bölüm koleksiyonlarını ve bölümlenmiş koleksiyonları S1 ila, S2, S3 performans düzeylerindeki karşılaştırma?](#compare)
- [Verilerim kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?](#uninterrupted-access)
- [Koleksiyonum, geçişten sonra nasıl değiştirecek?](#collection-change)
- [Tek bölümlü koleksiyonlar için geçiş sonra Faturam nasıl değiştirecek?](#billing-change)
- [Birden fazla 10 GB depolama alanı ne gerekiyor?](#more-storage-needed)
- [Performans düzeyleri planlanan geçiş işleminden önce S1, S2 ve S3 arasında değiştirebilirim?](#change-before)
- [S1, S2, S3 performans düzeyleri tek bölümlü koleksiyonlar üzerinde kendi için nasıl geçirebilirim?](#migrate-diy)
- [Nasıl bir EA müşterisinin müşterisiysem, ı etkilenen?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>S1, S2 ve S3 performans neden olan devre dışı bırakılıyor düzeyleri?

S1, S2 ve S3 performans düzeyleri, standart Azure Cosmos DB teklif sağladığı esnekliği sunmaz. S1, S2, S3 performans düzeyleri, aktarım hızı ve depolama kapasitesi önceden ayarlanmış ve esneklik sunmadı. Azure Cosmos DB, aktarım hızı ve depolama, özelleştirme yeteneği gereksinimleriniz değiştikçe ölçeği yeteneğinizi çok daha fazla esneklik sunan olarak sunar.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Nasıl tek bölüm koleksiyonlarını ve bölümlenmiş koleksiyonları S1 ila, S2, S3 performans düzeylerindeki karşılaştırma?

Aşağıdaki tabloda, tek bölümlü koleksiyonlar, bölümlenmiş koleksiyonları ve S1, S2, S3 performans düzeyleri kullanılabilir aktarım hızı ve depolama seçenekleri karşılaştırılmaktadır. ABD Doğu 2 bölgesi için bir örnek aşağıda verilmiştir:

|   |Bölümlenmiş koleksiyonu|Tek bölümlü koleksiyona|S1|S2|S3|
|---|---|---|---|---|---|
|Aktarım hızı üst sınırı|Sınırsız|10 K RU/sn|250 RU/s|1 K RU/sn|2.5 K RU/sn|
|En az aktarım hızı|2.5 K RU/sn|400 RU/sn|250 RU/s|1 K RU/sn|2.5 K RU/sn|
|En fazla depolama|Sınırsız|10 GB|10 GB|10 GB|10 GB|
|Fiyat (aylık)|Aktarım hızı: 6 ABD Doları / 100 RU/sn<br><br>Depolama: 0,25 dolar/GB|Aktarım hızı: 6 ABD Doları / 100 RU/sn<br><br>Depolama: 0,25 dolar/GB|25 ABD DOLARI|50 ABD DOLARI|100 ABD DOLARI|

Bir Kurumsal Sözleşme müşterisi misiniz? Öyleyse bkz [nasıl miyim etkilenen bir EA müşterisinin müşterisiysem?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Verilerim kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?

Bir S1, S2 veya S3 koleksiyonu varsa, koleksiyon için bir tek bölümlü koleksiyona programlı olarak geçirmeniz gerekir [.NET SDK kullanarak](#migrate-diy). 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Koleksiyonum, geçişten sonra nasıl değiştirecek?

Bir S1 koleksiyonu varsa, bunları bir tek bölümlü koleksiyona 400 RU/sn aktarım hızı ile geçirebilirsiniz. Tek bölümlü koleksiyonları ile kullanılabilir en düşük aktarım hızı 400 RU/sn olan. Şekilde, çok 150 RU/sn kullanabileceğiniz için ödeme yaparsınız değil, bir S1 koleksiyonu ve 250 RU/sn – ile ödeme ancak maliyeti 400 RU/sn tek bölümlü bir koleksiyon içinde yaklaşık aynıdır.

Bir S2 koleksiyonu varsa, 1 K RU/sn ile tek bölümlü bir koleksiyon için geçiş yapabilirsiniz. Değişiklik, işleme düzeyinizi görürsünüz.

Bir S3 koleksiyonu varsa, bunları bir tek bölümlü koleksiyona 2,5 K RU/sn ile geçirebilirsiniz. Değişiklik, işleme düzeyinizi görürsünüz.

Koleksiyon geçiş yaptıktan sonra her durumda, işleme düzeyinizi özelleştirme ya da yukarı ve aşağı kullanıcılarınız için düşük gecikmeli erişim sağlamak için gerektiği gibi ölçeklendirin olacaktır. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Ben tek bölümlü koleksiyonlar için geçiş sonra Faturam nasıl değiştirecek?

Varsayılmıştır 10 S1 koleksiyonu, ABD Doğu bölgesinde her depolama alanı 1 GB olması ve bu 10 bir S1 koleksiyonu 400 RU/sn (en düşük düzey), 10 tek bölüm koleksiyonlarını geçirme. Tam bir ay için 10 tek bölüm koleksiyonlarını tutarsanız faturanız şu şekilde görünür:

![Nasıl 10 koleksiyonlar için S1 fiyatlandırma 10 koleksiyonu için bir tek bölümlü koleksiyona fiyatlandırması kullanılarak karşılaştırır](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Birden fazla 10 GB depolama alanı ne gerekiyor?

S1, S2 veya S3 performans düzeyi ile bir koleksiyon olması, mı tümü 10 GB depolama alanı Azure Cosmos DB veri geçiş aracı neredeyse verilerinizi ile bölümlenmiş bir koleksiyona taşımak için kullanabileceğiniz, kullanılabilir olan bir tek bölümlü koleksiyona sahip Sınırsız depolama. Bölünmüş bir koleksiyona avantajları hakkında daha fazla bilgi için bkz. [bölümleme ve ölçeklendirme Azure Cosmos DB'de](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Performans düzeyleri planlanan geçiş işleminden önce S1, S2 ve S3 arasında değiştirebilirim?

S1, S2 ve S3 performans yalnızca var olan hesaplar değiştirilebilir ve program aracılığıyla performans düzeyi katmanları alter [.NET SDK kullanarak](#migrate-diy). Bir tek bölümlü koleksiyona S1, S3 veya S3 değiştirirseniz, S1, S2 veya S3 performans düzeyleri için döndüremez.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>S1, S2, S3 performans düzeyleri tek bölümlü koleksiyonlar üzerinde kendi için nasıl geçirebilirim?

Tek bölümlü koleksiyonlar için programlı olarak S1, S2 ve S3 performans düzeylerinden geçirebilirsiniz [.NET SDK kullanarak](#migrate-diy). Tek bölümlü koleksiyonları ile kullanılabilen esnek aktarım hızı seçeneklerinden yararlanmak için planlanan geçiş işleminden önce kendi sunucunuzda bunu yapabilirsiniz.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>.NET SDK'sını kullanarak tek bölüm koleksiyonlarını geçirme

Bu bölüm, yalnızca bir koleksiyonun performans değiştirme kapsar kullanarak düzey [SQL .NET API](sql-api-sdk-dotnet.md), ancak diğer Larımız için benzer bir işlemdir.

5000 istek birimi / saniye için koleksiyon işleme hacmini değiştirmek için kod parçacığı aşağıda verilmiştir:
    
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

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Nasıl bir EA müşterisinin müşterisiysem, ı etkilenen?

EA müşterileri, geçerli sözleşme sonuna kadar korumalı fiyatı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
Fiyatlandırma ve Azure Cosmos DB ile verileri yönetme hakkında daha fazla bilgi edinmek için şu kaynakları araştırın:

1.  [Verileri Cosmos DB'de bölümleme](sql-api-partition-data.md). Sorunsuz bir şekilde ölçeklendirmek için bir bölümleme stratejisi uygularken ipuçları yanı sıra tek bölüm kapsayıcısının ve bölümlenmiş kapsayıcılar arasındaki farkı anlamak.
2.  [Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). Aktarım hızı sağlama ve depolama kullanma maliyeti hakkında bilgi edinin.
3.  [İstek birimleri](request-units.md). Farklı işlem türleri, örneğin okuma, yazma, sorgu aktarım hızı kullanımını anlayın.
