---
title: "Veri kaynaklarına açıklama ekleme | Microsoft Docs"
description: "Nasıl yapılır makalesi kolay adlar, etiketler, açıklamalar ve uzmanlar dahil olmak üzere Azure veri Kataloğu'nda veri varlıklarına açıklama ekleme vurgulama."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 4518fc126c717cc79ca7950c0b1ddcd9f1d8c7d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-annotate-data-sources"></a>Veri kaynaklarına açıklama ekleme
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** bir kayıt ve sistemi kurumsal veri kaynakları için bulma görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Diğer bir deyişle, verileri tüm bulmak, anlamak ve veri kaynakları ve yardımcı kuruluşların kendi var olan verilerden daha fazla değer almak için kullanmak kişiler yardımcı olma hakkında kataloğudur. Bir veri kaynağına veri Kataloğu ile kaydedildikten sonra meta verilerini kopyalanır ve hizmet tarafından dizine ancak Öykü yok sonlanmıyor. Veri Kataloğu, açıklamalar ve etiketleri – veri kaynağından ayıklanan meta verileri desteklemek ve veri kaynağı için daha fazla kişinin daha anlaşılır hale gibi – kendi açıklayıcı meta verilerini sağlamak kullanıcıların sağlar.

## <a name="annotation-and-crowdsourcing"></a>Ek açıklama ve kitle kaynak
Herkesin bir fikir vardır. Ve bu iyi bir şeydir.
Veri Kataloğu, farklı kullanıcıların farklı perspektiflerini kurumsal veri kaynakları varsa ve her bu Perspektifleri değerli olabileceğini tanır. Aşağıdaki senaryoyu göz önünde bulundurun:

* Sistem Yöneticisi veri kaynağı barındıran sunucular veya hizmetler için hizmet düzeyi sözleşmesi bilir.
* Veritabanı Yöneticisi, her veritabanı ve izin verilen ETL işleme windows yedekleme zamanlaması bilir.
* Sistem sahibinin işlemi veri kaynağına erişim istemek kullanıcılar için bilir.
* Veri steward nasıl kurumsal veri modeline varlıklar ve veri kaynağındaki öznitelikleri eşlemek bilir.
* Analist verilerin kendisi destekleyen iş süreçlerini bağlamında nasıl kullanıldığını bilir.

Her bu Perspektifleri değerlidir ve veri Kataloğu yakalanan ve kayıtlı veri kaynaklarının eksiksiz bir görünümünü sağlamak için kullanılan her biri izin veren meta veriler için kitle kaynak yaklaşımı kullanır. Veri Kataloğu Portalı'nı kullanarak, her bir kullanıcı ekleyin ve diğer kullanıcılar tarafından sağlanan ek açıklamaları görmeye devam ederken kendi ek açıklamalarını düzenleyin.

## <a name="different-types-of-annotations"></a>Farklı türde ek açıklamaların
Veri Kataloğu ek açıklamaları aşağıdaki türlerini destekler:

| Ek açıklama | Notlar |
| --- | --- |
| Kolay ad |Kolay adlar daha kolay anlaşılan veri varlıklarını yapmak için veri varlık düzeyinde sağlanabilir. Temel alınan nesnenin adı şifreli, kısaltılmış veya aksi halde anlamlı kullanıcılara olduğunda kolay adlar en yararlı olur. |
| Açıklama |Açıklamaları sağlanan veri varlığına ve öznitelik / sütun düzeyi. Açıklamaları olan kullanıcının açıklamak serbest biçimli kısa metin ek açıklamaları perspektif veri varlığına veya kendi kullanın. |
| Etiketler (kullanıcı etiketleri) |Etiketler sağlanan veri varlığına ve öznitelik / sütun düzeyi. Kullanıcı, veri varlıklarını veya öznitelikleri sınıflandırmak için kullanılan kullanıcı tanımlı etiketleri etiketleridir. |
| Etiketler (sözlüğü etiketleri) |Etiketler sağlanan veri varlığına ve öznitelik / sütun düzeyi. Veri varlıklarını veya ortak bir iş sınıflandırması kullanarak özniteliklerini sınıflandırmak için kullanılabilecek merkezi olarak tanımlanması terimler sözlüğü etiketleridir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md) |
| Uzmanlar |Uzmanlar veri varlık düzeyinde sağlanabilir. Uzmanlar veri Uzman perspektiflerine sahip kullanıcıları veya grupları tanımlamak ve kullanıcılar, kayıtlı veri kaynaklarını bulmak için iletişim noktaları olarak hizmet ve var olan ek açıklamaları tarafından bulamadığınız sorularınız varsa. |
| Erişim isteği |İstek erişim bilgileri veri varlık düzeyinde sağlanabilir. Bu bilgiler henüz erişim izni olmayan bir veri kaynağını Bul kullanıcılar için olur. Kullanıcılar kullanıcı veya grubun erişimi, işlem ya da kullanıcıların erişim, gerek aracı URL'sini verir e-posta adresi girin veya işlem metin olarak girebilirsiniz. |
| Belgeler |Belge veri varlık düzeyinde sağlanabilir. Varlık belgeleri, bağlantılar ve resimler içerebilir ve etiketleri ve açıklamaları ile ilettiği olmayan herhangi bir bilgi sağlayan zengin metin bilgilerdir. |

## <a name="annotating-multiple-assets"></a>Birden çok varlıklarına açıklama
Birden çok veri varlıklarını veri Kataloğu portalında seçerken, kullanıcılar tek bir işlemde tüm seçili varlıklar açıklama ekleyebilirsiniz. Ek açıklamalar seçin ve ilgili veri varlıkları için tutarlı bir açıklama ve etiketleri ve uzmanlar kümelerinin sağlamaya kolaylaşır tüm seçili varlıklar için geçerli olur.

> [!NOTE]
> Kayıt Aracı veri Kataloğu verileri kullanarak kayıt veri varlıklarını kaynaktaki etiketleri ve uzmanlar de girilebilir.
>
>

Ne zaman birden çok tablo ve görünümler, yalnızca sütunlara tüm varlıklar ortak olan veri seçilen seçerek veri Kataloğu Portalı'nda görüntülenir. Bu, seçilen tüm varlıklar için aynı adla etiketleri ve açıklamaları tüm sütunlar için sağlamak için kullanıcıların sağlar.

## <a name="annotations-and-discovery"></a>Ek açıklamalar ve bulma
Kayıt sırasında veri kaynağından ayıklanan meta veri kataloğu arama dizini eklemiş gibi kullanıcı tarafından sağlanan meta verileri de dizine alınır. Bu anlamına yalnızca kullanıcıların bunlar bulmak için verileri anlamak kolaylaştırmak ek açıklamaları ek açıklamaları Ayrıca kullanıcıların kendilerine anlamlı Koşulları'nı kullanarak arama açıklamalı veri varlıklarını bulmak kolaylaştırır.

## <a name="summary"></a>Özet
Veri Kataloğu ile veri kaynağı kaydetme verileri bulunabilirlik yapısal ve açıklayıcı meta veri kaynağından Kataloğu hizmetine kopyalayarak hale getirir. Bir veri kaynağı kaydedildikten sonra kullanıcılara bulup gelen veri Kataloğu portalını anlamak daha kolay hale getirmek için ek açıklama sağlayabilir.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md) veri kaynaklarına açıklama ekleme hakkında adım adım ayrıntılar için Öğreticisi.
