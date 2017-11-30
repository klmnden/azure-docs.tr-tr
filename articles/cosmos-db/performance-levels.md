---
title: "DocumentDB API performans düzeyleri | Microsoft Docs"
description: "DocumentDB API performans düzeyleri, üretilen iş başına kapsayıcı olarak ayırmak nasıl etkinleştirme hakkında bilgi edinin."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 62767163213383c577e74e0aa8fbd07f891cb694
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>S1, S2 ve S3 performans düzeyleri devre dışı bırakma

> [!IMPORTANT] 
> Bu makalede açıklanan S1, S2 ve S3 performans düzeyleri kullanımdan kaldırılacak ve artık yeni DocumentDB API hesapları için kullanılabilir.
>

Bu makalede, S1, S2 ve S3 performans düzeyleri genel bir bakış sağlar ve geç 2017 içinde nasıl bu performans düzeyleri kullanmak koleksiyonlar için tek bölüm koleksiyonları geçirilecek açıklanır. Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

- [S1, S2 ve S3 performans neden olan kaldırılan düzeyleri?](#why-retired)
- [Nasıl tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1 için S2, S3 performans düzeyleri karşılaştırması nedir?](#compare)
- [Verilerimi kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?](#uninterrupted-access)
- [Nasıl kendi koleksiyonuma geçişten sonra değişir mi?](#collection-change)
- [Tek bölüm koleksiyonları geçişi sonra nasıl my faturalama değişir mi?](#billing-change)
- [Ne birden fazla 10 GB depolama alanı ihtiyacım var?](#more-storage-needed)
- [Planlanan geçişten önce performans düzeyleri arasında S1, S2 ve S3 değiştirebilirim?](#change-before)
- [Ne zaman kendi koleksiyonuma geçirildiğini nasıl anlarım?](#when-migrated)
- [S3 performans düzeyleri üzerinde kendi tek bölüm koleksiyonları nasıl S1, S2, geçişini?](#migrate-diy)
- [Nasıl bir EA müşteri ben varsa ı etkilenen?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>S1, S2 ve S3 performans neden olan kaldırılan düzeyleri?

S1, S2 ve S3 performans düzeyleri esneklik, DocumentDB API koleksiyonları teklif sağlamaz. S1, S2, S3 performans düzeyleri ile üretilen iş ve depolama kapasitesini önceden ayarlanmış ve esneklik sunmadı. Azure Cosmos DB işleme ve depolama, özelleştirme yeteneği gereksinimleriniz değiştikçe ölçeklendirme yeteneğinizi çok daha fazla esneklik sunumu olarak sunar.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Nasıl tek bölüm koleksiyonları ve bölümlenmiş koleksiyonlar S1 için S2, S3 performans düzeyleri karşılaştırması nedir?

Aşağıdaki tabloda tek bölüm koleksiyonları, bölümlenmiş koleksiyonlar ve S1, S2, S3 performans düzeyleri kullanılabilir işleme ve depolama seçenekleri karşılaştırılmaktadır. ABD Doğu 2 bölge için örnek aşağıda verilmiştir:

|   |Bölümlenmiş koleksiyonu|Tek bir bölüm koleksiyonu|S1|S2|S3|
|---|---|---|---|---|---|
|En yüksek verimlilik|Sınırsız|10 K RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|En düşük işleme|2.5 K RU/s|400 RU/s|250 RU/s|1 K RU/s|2.5 K RU/s|
|Maksimum depolama|Sınırsız|10 GB|10 GB|10 GB|10 GB|
|Fiyat (ay)|Verimlilik: $6 / 100 RU/s<br><br>Depolama: $ 0,25/GB|Verimlilik: $6 / 100 RU/s<br><br>Depolama: $ 0,25/GB|$25 ABD DOLARI|$50 ABD DOLARI|$100 ABD DOLARI|

EA müşteri misiniz? Aksi takdirde bkz [nasıl ı etkilenen EA müşteri ben varsa?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Verilerimi kesintisiz erişim sağlamak için yapmanız gerekenler nelerdir?

Hiçbir şey Cosmos DB geçiş sizin için işler. S1, S2 ve S3 koleksiyonu varsa, geçerli bir tek bölüm koleksiyonunu geç 2017 geçirilecektir. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Nasıl kendi koleksiyonuma geçişten sonra değişir mi?

S1 koleksiyonu 400 RU/s üretilen iş ile bir tek bölümlü bir koleksiyon için bir geçişi yapılmaz. Tek bölüm koleksiyonları ile kullanılabilen en düşük işleme 400 RU/s olur. Çok 150 RU/s için kullanabileceğiniz ödeme yapıyorsanız olmayan şekilde S1 koleksiyonunuzu ve 250 RU/s – ödeme ancak maliyeti 400 RU/s tek bölümlü bir koleksiyon içinde yaklaşık aynıdır.

S2 koleksiyonu 1 K RU/s ile bir tek bölümlü bir koleksiyon için bir geçişi yapılmaz. Herhangi bir değişiklik, üretilen iş düzeyi görürsünüz.

S3 koleksiyonu 2,5 K RU/s ile bir tek bölümlü bir koleksiyon için bir geçişi yapılmaz. Herhangi bir değişiklik, üretilen iş düzeyi görürsünüz.

Koleksiyonunuz geçirildikten sonra her durumda, üretilen iş düzeyi özelleştirebilir ya da yukarı ve aşağı kullanıcılarınız için düşük gecikmeli erişim sağlamak için gerektiği şekilde ölçeklendirme kuramaz. Koleksiyonunuz geçirildikten sonra işleme düzeyini değiştirmek için yalnızca Azure portalında Cosmos DB hesabınızı açın, Ölçek'ı tıklatın, koleksiyonunuzu seçin ve sonra aşağıdaki ekran görüntüsünde gösterildiği gibi üretilen iş düzeyi ayarlayın:

![Azure portalında üretilen iş ölçeklendirme](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Tek bölüm koleksiyonları geçişi sonra nasıl my faturalama değişir mi?

Varsayılmıştır 10 S1 koleksiyonları, 1 GB depolama alanı BİZE Doğu bölgede her sahiptir ve 10 tek bölüm koleksiyonları 400 RU/sn (en düşük düzey), bu 10 S1 koleksiyonlarını geçirme. Tam bir ay için 10 tek bölüm koleksiyonları tutarsanız faturanızı şu şekilde görünür:

![Nasıl S1 10 koleksiyonlar için fiyatlandırma tek bölümlü bir koleksiyon için fiyatlandırma kullanarak 10 koleksiyonları karşılaştırır](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Ne birden fazla 10 GB depolama alanı ihtiyacım var?

S1, S2 ve S3 bir performans düzeyine sahip bir koleksiyona sahip, mı da 10 GB depolama alanı kullanılabilir Cosmos DB veri geçiş aracı verilerinizi ile bölümlenmiş bir koleksiyon neredeyse geçirmek için kullanabileceğiniz sahip tek bölümlü bir koleksiyon, sahip Sınırsız depolama. Bölümlendirilmiş bir koleksiyon avantajları hakkında daha fazla bilgi için bkz [bölümleme ve Azure Cosmos DB'de ölçeklendirme](documentdb-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Planlanan geçişten önce performans düzeyleri arasında S1, S2 ve S3 değiştirebilirim?

Yalnızca var olan hesapları S1, S2 ve S3 performansı ile değiştirin ve performans düzeyi katmanları portalı üzerinden veya program aracılığıyla alter mümkün olacaktır. Tek bölümlü bir koleksiyon için S1, S3 veya S3 değiştirirseniz, S1, S2 ve S3 performans düzeyleri döndüremez.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Ne zaman kendi koleksiyonuma geçirildiğini nasıl anlarım?

Geçiş üzerinde geç 2017 içinde meydana gelir. S1 kullanan bir koleksiyon varsa, geçiş gerçekleşmeden önce S2 veya S3 performans düzeyleri, Cosmos DB takım, e-posta ile başvurun. Geçiş işlemi tamamlandıktan sonra Azure portalında koleksiyonunuzu standart fiyatlandırma kullandığını gösterir.

![Fiyatlandırma katmanı standart koleksiyonunuzu onaylamak nasıl geçirildiğini](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>S3 performans düzeyleri üzerinde kendi tek bölüm koleksiyonları nasıl S1, S2, geçişini?

Azure Portalı'nı kullanarak tek bölüm koleksiyonları S1, S2 ve S3 performans düzeylerinden geçirebileceğiniz veya program aracılığıyla. Tek bölüm koleksiyonları ile kullanılabilen esnek işleme seçeneklerinden yararlanmak için planlanan geçiş işleminden önce kendi bunu yapabilirsiniz veya koleksiyonlarınızı, geç 2017 içinde geçirirsiniz.

**Azure Portalı'nı kullanarak tek bölüm koleksiyonları geçirmek için**

1. İçinde [ **Azure portal**](https://portal.azure.com), tıklatın **Azure Cosmos DB**, sonra değiştirmek için Cosmos DB hesabı seçin. 
 
    Varsa **Azure Cosmos DB** olan atlama çubuğu değil üzerinde tıklayın >, kaydırın **veritabanları**seçin **Azure Cosmos DB**ve bir hesap seçin.  

2. Kaynak menüsünde altında **kapsayıcıları**, tıklatın **ölçek**, aşağı açılan listeden değiştirin ve ardından istediğiniz koleksiyonu seçmek **fiyatlandırma katmanı**. Önceden tanımlanmış verimlilik kullanarak hesapları S1, S2 ve S3 fiyatlandırma katmanı vardır.  İçinde **fiyatlandırma katmanınızı seçin** sayfasında, **standart** kullanıcı tanımlı verimlilik değiştirin ve ardından **seçin** değişikliklerinizi kaydetmek için.

    ![Üretilen iş değerini değiştirmek nereye gösteren ayarları sayfasının ekran görüntüsü](./media/performance-levels/change-performance-set-thoughput.png)

3. Geri **ölçek** sayfasında **fiyatlandırma katmanı** değiştirilir **standart** ve **üretilen işi (RU/s)** kutusu ile bir varsayılan görüntülenir 400 değeri. İşleme 400-10.000 arasında ayarlamak [istek birimleri](request-units.md)/second (RU/s). **Tahmini aylık fatura** aylık maliyeti tahmini otomatik olarak sağlamak üzere Sayfa güncelleştirmelerini sonundaki. 

    >[!IMPORTANT] 
    > Yaptığınız değişiklikleri kaydedin ve fiyatlandırma katmanı standart taşıma sonra S1, S2 ve S3 performans düzeyleri için geri alamazsınız.

4. Tıklatın **kaydetmek** yaptığınız değişiklikleri kaydetmek için.

    Daha fazla verimlilik (10. 000'ru / s büyük) veya daha fazla depolama alanı (10 GB'den büyük) gerekli belirlerseniz, bölümlendirilmiş bir koleksiyon oluşturabilirsiniz. Tek bölümlü bir koleksiyon için bölümlendirilmiş bir koleksiyon geçirmek için bkz [tek bölümünden bölümlenmiş koleksiyonlar için geçiş](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Standart S1, S2 ve S3 değiştirme iki dakika kadar sürebilir.
    > 
    > 

**.NET SDK kullanarak tek bölüm koleksiyonları geçirmek için**

Koleksiyonları performans düzeylerini değiştirmek için başka bir seçenek Azure Cosmos DB SDK'ları olur. Bu bölüm, yalnızca bir koleksiyona ait performansının değiştirilmesi kapsar kullanarak düzey [DocumentDB .NET API](documentdb-sdk-dotnet.md), ancak bizim diğer SDK için benzer bir işlemdir.

Saniye başına 5.000 istek birimlerine koleksiyonu verimlilik değiştirmek için bir kod parçacığı aşağıda verilmiştir:
    
```C#
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

1.  [Cosmos DB'de veri bölümlendirme](documentdb-partition-data.md). Tek bölümlü bir kapsayıcı ve bölümlenmiş kapsayıcıları yanı sıra, sorunsuz bir şekilde ölçeklendirmek için bölümleme stratejisine uygulama ipuçları arasındaki farkı anlama.
2.  [Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/). Üretilen iş sağlama ve depolama tüketme maliyeti hakkında bilgi edinin.
3.  [İstek birimleri](request-units.md). Farklı işlem türleri için örneğin okuma, yazma, sorgu işleme tüketiminin anlayın.
