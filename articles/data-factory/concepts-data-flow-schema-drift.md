---
title: Azure veri fabrikası veri akışı şema değişikliklerini eşleme
description: Esnek şema değişikliklerini ile Azure Data factory'de veri akışları oluşturun
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 6fd610afc0a21a97a8544b9e4b173f207f5fb50f
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722875"
---
# <a name="mapping-data-flow-schema-drift"></a>Veri akışı şema değişikliklerini eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Şema değişikliklerini kavramını burada kaynaklarınızı meta verileri genellikle değiştirme durumdur. Alanlar, sütunlar, türleri, vb. eklenebilir, kaldırıldı veya çalışma sırasında değiştirildi. Şema değişikliklerini için işleme olmadan, veri akışı Yukarı Akış veri kaynağı değişiklikleri değişikliklere karşı savunmasız hale gelir. Gelen sütun ve alanları değiştirdiğinizde, bu kaynak adlarını bağlanması eğilimli olduğundan tipik ETL düzenlerindeki başarısız.

Şema değişikliklerini karşı korumak için bir veri mühendisi izin vermek için bir veri akışı aracında olanakları sağlamak önemlidir:

* Mutable alan adları, veri türleri, değerleri ve boyutları olan kaynakları tanımlama
* Sabit kodlanmış alanları ve değerleri yerine veri desenleri çalışabilirsiniz dönüştürme parametrelerini tanımlayın
* Ad alanları kullanmak yerine gelen alanları eşleştirmeye biçimlerini ifadelerini tanımlama

## <a name="how-to-implement-schema-drift"></a>Şema değişikliklerini gerçekleştirme

* "Kaynak Dönüşümünüzü şema değişikliklerini izin ver" seçin

<img src="media/data-flow/schemadrift001.png" width="400">

* Bu seçeneği seçtiğinizde, tüm gelen alanları her veri akışı yürütme kaynağınızdan okuyun ve havuz için tüm akışı geçirilir.

* Böylece tüm yeni alanlar çekilen yukarı alıp, hedefte geldiğimizi tüm yeni alanlar, havuz dönüşümünde eşlemek için "Otomatik-Map"'ı kullandığınızdan emin olun.

<img src="media/data-flow/automap.png" width="400">

* Yeni alanlar, basit bir senaryoyla sunulduğunda her şey çalışır kaynak havuzu -> (kopyalamak olarak da bilinir) eşleme.

* Şema değişikliklerini işleyen bu iş akışında dönüşümleri eklemek için desen eşleştirme sütun adı, türü ve değeri ile eşleşmesi için kullanabilirsiniz.

* "Ekle sütun deseni" türetilmiş sütun veya toplama dönüştürme "Şema değişikliklerini" anlayan bir dönüştürme isterseniz tıklayın.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> Akışınızın tamamında şema değişikliklerini kabul etmek için veri akışında bir mimari karar vermeniz gerekir. Bunu yaptığınızda, kaynaktan şema değişikliklerine karşı koruyabilir. Ancak, veri akışı bağlama erken sütunları ve türleri kaybedersiniz. Geç bağlama akışları Azure Data Factory işler şema değişikliklerini akışlar şekilde Bağlantılarınızdaki yapılandırdığınızda, sütun adlarını, akışı boyunca şema görünümlerde kullanılabilir olmaz.

<img src="media/data-flow/taxidrift1.png" width="400">

Taksi tanıtım örnek veri akışı, bir örnek şema değişikliklerini TripFare kaynak alt veri akışı yok. Toplama dönüşümü, biz "sütun deseni" tasarım toplama alanları kullanıyoruz dikkat edin. Belirli sütunlarını adlandırmaya veya konuma göre sütunlar için bakarak yerine verileri değiştirebilir ve çalıştırmaları arasında aynı sırada görünmeyebilir varsayıyoruz.

Azure Data Factory, veri akışı şema değişikliklerini işleyen, bu örnekte, oluşturduğumuz ve toplama, veri etki alanının her bir gidiş dönüş için fiyatlar içerdiğini bilerek 'double', türü sütunlar tarar. Ardından bir toplama matematik hesaplama kaynağında burada sütunu gölünüzdeki bağımsız olarak ve sütunun adlandırma bağımsız olarak tüm double alanları genelinde gerçekleştirebiliriz.

Azure Data Factory, veri akışı söz dizimi $ eşleşen her sütun, eşleşen deseni temsil etmek için kullanır. Sütun adları karmaşık dize arama ve normal ifade işlevleri kullanarak da eşleştirebilirsiniz. Bu durumda, sütun 'double' türünün her eşleşmeye göre yeni bir birleşik alan adı oluşturun ve metin ekleme kullanacağız ```_total``` bunların her biri için eşleşen adları: 

```concat($$, '_total')```

Daha sonra yuvarlama ve her eşleşen sütunlar için değerlerin toplamı:

```round(sum ($$))```

Azure Data Factory, veri akışı örnek "Taksi tanıtım" ile bu genişletme test edebilirsiniz. Sonuçlarınızı etkileşimli olarak görebilmeniz için veri akışı tasarım yüzeyinin üst kısmında hata ayıklama iki durumlu düğmeyi kullanarak hata ayıklama oturumunda anahtarı:

<img src="media/data-flow/taxidrift2.png" width="800">

## <a name="access-new-columns-downstream"></a>Yeni sütunlar aşağı yönde erişim

Yeni sütunlar sütun desenlerle oluşturduğunuzda, bu yeni sütunların "byName" ifadesi işlevini kullanarak daha sonra veri akışı dönüşümleri erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

İçinde [veri akışı ifade dili](data-flow-expression-functions.md) sütun desenleri ve şema değişikliklerini "byName" ve "byPosition" gibi ek özellikleri bulabilirsiniz.
