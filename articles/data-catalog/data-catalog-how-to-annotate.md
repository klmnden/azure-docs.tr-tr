---
title: Azure veri Kataloğu'nda veri kaynaklarına açıklama ekleme
description: Nasıl yapılır makalesi kolay adlar, etiketler, açıklamalar ve uzmanlar dahil olmak üzere, Azure veri Kataloğu'nda veri varlıklarına açıklama ekleme vurgulama.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 3a66c8c5963972828723dd74ffe560a0e2240165
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61001945"
---
# <a name="how-to-annotate-data-sources"></a>Veri kaynaklarına açıklama ekleme
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Diğer bir deyişle, tüm veri kaynakları ve yardımcı kuruluşların var olan verilerden daha fazla değer elde kullanacağınızı bulmak ve anlamak yardımcı hakkında veri Kataloğu. Bir veri kaynağı, veri Kataloğu'na kaydedildiğinde meta verilerini kopyalanır ve hizmet tarafından ancak bir hikayesi vardır sonlanmıyor. Veri Kataloğu, açıklamaları ve etiketleri – veri kaynağından ayıklanan meta verileri desteklemek ve veri kaynağı için daha fazla insana daha anlaşılır hale getirmek için gibi – kendi açıklayıcı meta verilerini sağlamak kullanıcıların sağlar.

## <a name="annotation-and-crowdsourcing"></a>Ek açıklama ve kitle kaynak
Herkesin bir fikrim vardır. Ve bu iyi bir şeydir.
Veri Kataloğu, farklı kullanıcılar kurumsal veri kaynakları üzerinde farklı perspektiflerini sahip olduğunuzu ve her biri bu Perspektifleri yararlı olabileceğini tanır. Aşağıdaki senaryoyu göz önünde bulundurun:

* Sistem Yöneticisi, veri kaynağını barındıran sunucular veya hizmetleri için hizmet düzeyi sözleşmesi bilir.
* Veritabanı Yöneticisi, her bir veritabanı ve ETL işlemi izin verilen windows yedekleme zamanlamasını bilir.
* Sistem sahibinin, kullanıcılar veri kaynağına erişim isteme işlemini bilir.
* Varlıklar ve veri kaynağındaki öznitelikleri ve kurumsal veri modelini nasıl eşleştiği veri Görevlisi bilir.
* Analist, veri kendisinin destekler ve iş süreçleri bağlamında nasıl kullanıldığını bilir.

Bu perspektifler her değerlidir ve bir kitle kaynak yaklaşımı yakalanır ve kayıtlı veri kaynaklarına, eksiksiz bir görünümünü sağlamak için kullanılan her biri izin veren bir meta veri kataloğu kullanır. Her bir kullanıcı, veri Kataloğu portalını kullanarak ekleyebilir ve diğer kullanıcılar tarafından sağlanan ek açıklamaları görüntülemesi sırasında kendi ek açıklamalarını düzenleyebilirsiniz.

## <a name="different-types-of-annotations"></a>Farklı tür ek açıklamaları
Veri Kataloğu, ek açıklamaları aşağıdaki türlerini destekler:

| Ek Açıklama | Notlar |
| --- | --- |
| Kolay ad |Kolay adlar veri varlıkları daha kolay anlaşılmasını sağlamak için veri varlık düzeyinde sağlanabilir. Temel alınan nesne adının şifreli, kısaltılmış veya aksi halde anlamlı kullanıcılara olduğunda kolay adlar ekseriyetle faydalıdır. |
| Açıklama |Veri varlığına ve öznitelik tanımlarını sağlanabilir / sütun düzeyi. Açıklamaları olan kullanıcının açıklayan kısa serbest biçimli metin ek açıklamalar perspektif veri varlığı veya kullanımı. |
| Etiketler (kullanıcı etiketler) |Etiketler, veri varlığı ve öznitelik sağlanabilir / sütun düzeyi. Kullanıcı, kullanıcı tanımlı ve veri varlıklarını veya öznitelikleri kategorilere ayırmak için kullanılabilir etiketler etiketlerdir. |
| Etiketleri (sözlüğü etiketler) |Etiketler, veri varlığı ve öznitelik sağlanabilir / sütun düzeyi. Veri varlıklarını ya da ortak bir iş sınıflandırması kullanarak özniteliklerini kategorilerine ayırmak için kullanılan merkezi olarak tanımlı terimler sözlüğü etiketlerdir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md) |
| Uzmanlar |Uzmanlar veri varlık düzeyinde sağlanabilir. Uzmanlar kullanıcılar veya gruplar Uzman perspektiflerine sahip verileri tanımlamak ve kullanıcılar, kayıtlı veri kaynaklarını bulmak için iletişim noktaları görür ve mevcut ek açıklamalar tarafından bulamadığınız sorularınız varsa. |
| Erişim izni iste |Veri varlık düzeyinde erişim bilgi istek sağlanabilir. Bu bilgiler henüz erişim izni olmayan bir veri kaynağı için kullanıcılar içindir. Kullanıcılar, kullanıcı veya grubun erişim, kullanıcıların, erişim elde etmek için gereken araç ve işlem URL'sini verir e-posta adresini girin veya işlem bir metin girebilirsiniz. |
| Belgeler |Belgeleri veri varlık düzeyinde sağlanabilir. Varlık belgelere bağlantılar ve resimler ekleyebilirsiniz ve etiketleri ve açıklamaları ile ilettiği değil herhangi bir bilgi sunan zengin metin bilgilerdir. |

## <a name="annotating-multiple-assets"></a>Birden çok varlıklarına
Birden fazla veri varlığına veri Kataloğu Portalı'nda seçildiğinde, kullanıcılar tek bir işlemde tüm seçili varlıklar açıklama ekleyebilirsiniz. Ek açıklamalar kolay seçin ve ilgili veri varlıkları için etiketleri ve uzmanlar ve tutarlı bir açıklama sağlayın, tüm seçili varlıklar için geçerli olur.

> [!NOTE]
> Etiketleri ve uzmanlar kayıt aracı kayıt veri varlıklarını kullanarak veri Kataloğu veri kaynağı olduğunda da sağlanabilir.
>
>

Ne zaman birden çok tablo ve sütunları yalnızca seçilen tüm varlıklar ortak olan veri görünümleri seçme veri Kataloğu Portalı'nda görüntülenir. Bu etiketleri ve açıklamaları tüm sütunlar için seçilen tüm varlıklar için aynı adla sağlamak üzere kullanıcıları sağlar.

## <a name="annotations-and-discovery"></a>Ek açıklamalar ve bulma
Kayıt sırasında veri kaynağından ayıklanan meta verileri için veri kataloğu arama dizini eklemiş gibi kullanıcı tarafından sağlanan meta veriler ayrıca dizine alınır. Bu yalnızca ek açıklamaları, bulma verileri anlamak kullanıcılar için kolaylaştıran, ek açıklamalar Ayrıca kullanıcıların bunlara mantıklı koşulları kullanarak arama açıklamalı veri varlıklarını bulmak kolaylaştırır olduğunu gösterir.

## <a name="summary"></a>Özet
Veri Kataloğu ile bir veri kaynağı kaydetme verileri bulunabilir veri kaynağından yapısal ve açıklayıcı meta veri Kataloğu hizmetine kopyalayarak yapar. Bir veri kaynağı kaydedildikten sonra kullanıcıların bulunmasını ve gelen veri Kataloğu portalını anlaşılmasını kolaylaştırmak için ek açıklamalar sağlar.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) veri kaynaklarına açıklama nasıl hakkında bilgi için adım adım öğretici.
