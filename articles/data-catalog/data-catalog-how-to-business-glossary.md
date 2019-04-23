---
title: Azure veri Kataloğu'nda iş sözlüğünü ayarlama
description: Kayıtlı veri varlıklarını tanımlama ve etiket için ortak bir iş sözlüğü kullanmak için Azure veri Kataloğu'nda iş sözlüğünü vurgulama nasıl yapılır makalesi.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 649a842c8c8890713bda938c8e11740c5c8be7aa
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009727"
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>İş sözlüğünü yönetilen etiketleme için ayarlama

## <a name="introduction"></a>Giriş

Kolayca keşfedin ve analiz ve karar vermek için gereken veri kaynaklarını anlama azure veri Kataloğu veri kaynağı bulmayı sağlar. Bu özellikler bulabilir ve çeşitli kullanılabilir veri kaynaklarını anlama en büyük etkiyi olun.

Etiketleme varlıklar veri büyük anlama yükseltir bir veri Kataloğu özelliğini. Etiketleme kullanarak bir varlığı veya varlık arama veya tarama aracılığıyla bulmak daha kolay hale sırayla bir sütun ile anahtar sözcükleri ilişkilendirebilirsiniz. Etiketleme de daha fazla kolayca varlık amacı ve bağlam anlamanıza yardımcı olur.

Ancak, etiketleme bazen kendi sorunlara neden olabilir. Etiketleme artmasına neden olabilen sorunları bazı örnekleri şunlardır:

* Bazı varlıklar üzerinde kısaltmalar ve diğerleri üzerinde genişletilmiş metin kullanın. Amaç varlıkları aynı etiketi ile etiketlemek için olmasına rağmen bu tutarsızlığı varlıklar, bulunmasını yavaşlattığını.
* Bağlama bağlı olarak, yani olası farklılığı. Örneğin, bir etiket olarak adlandırılan *gelir* bir Müşteri geliri müşteri tarafından veri kümesi gelebilir, ancak aynı etiketi üç ayda bir satış veri kümesini şirket için aylık gelir anlamına gelebilir.  

Bunlar ve diğer benzer zorlukları gidermek için veri Kataloğu, bir iş sözlüğü içerir.

Veri Kataloğu iş sözlüğünü kullanarak, bir kuruluşun temel iş terimlerini ve tanımlarını bir ortak bir iş sözlüğü oluşturmak için belge. Bu idare kuruluş genelinde veri kullanımı tutarlılık sağlar. Bir terimi iş sözlüğü tanımlandıktan sonra bir veri varlığına kataloğunda atanabilir. Bu yaklaşım *etiketleme yönetilen*, etiketleme olarak aynı yaklaşımdır.

## <a name="glossary-availability-and-privileges"></a>Sözlük kullanılabilirlik ve ayrıcalıkları

İş sözlüğü, yalnızca, Azure veri Kataloğu standart sürümü içinde kullanılabilir. Ücretsiz sürüm veri Kataloğu'nun bir sözlük içermez ve özellikleri yönetilen etiketleme için sağlamaz.

İş sözlüğü aracılığıyla erişebileceğiniz **sözlüğü** veri Kataloğu portalının Gezinti menüsündeki seçeneği.  

![İş sözlüğünü erişme](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Veri Kataloğu yöneticilerinin ve sözlük Yöneticileri rolünün üyeleri, oluşturabilir, düzenleme ve iş sözlüğü, sözlük terimleri silin. Tüm veri Kataloğu kullanıcılarının terim tanımları ve etiket varlıklar sözlük terimlerini görüntüleyebilirsiniz.

![Yeni bir sözlük terimini ekleme](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Sözlük girişi oluşturma

Veri Kataloğu yöneticilerinin ve sözlük yöneticileri oluşturabilir sözlük terimleri tıklayarak **yeni terim** düğmesi. Her sözlük terimini aşağıdaki alanları içerir:

* Bir iş tanımı bir dönem
* Varlık veya sütun için iş kuralları ve kullanım amacını yakalayan açıklaması
* En iyi terim hakkında bilmeniz hissedarlar listesi
* Hangi terim düzenlenir hiyerarşi tanımlar üst terim

## <a name="glossary-term-hierarchies"></a>Sözlük terimi hiyerarşiler

Veri Kataloğu iş sözlüğünü kullanarak, bir kuruluşun kendi iş sözlüğünü terimlerin bir hiyerarşi tanımlayabilir ve kendi iş sınıflandırma daha iyi temsil eden bir sınıflandırma koşulları oluşturabilirsiniz.

Bir terimi hiyerarşinin belirli bir düzeyde benzersiz olması gerekir. Yinelenen adlara izin verilmez. Bir hiyerarşi düzeylerini sayısına bir sınır yoktur, ancak üç düzeyde veya daha az olduğunda bir hiyerarşi genellikle daha kolay anlaşılır.

İş sözlüğünü hiyerarşileri kullanımı isteğe bağlıdır. Üst terimler için terimi alan boş bırakarak sözlükteki koşulları (hiyerarşik olmayan) düz bir listesini oluşturur.  

## <a name="tagging-assets-with-glossary-terms"></a>Sözlük Terimleri varlıklarla etiketleme

Sözlük Terimleri katalog tanımlandıktan sonra Varlık etiketleme deneyimi sözlüğü bir kullanıcının bir etiket olarak aramak için optimize edilmiştir. Veri Kataloğu portalı eşleşen sözlük terimleri aralarından seçim listesini görüntüler. Kullanıcı, listeden bir sözlük terimini seçerse, terimi için varlık (bir sözlük etiketi olarak da bilinir) bir etiket olarak eklenir. Sözlükte olmayan bir terimi yazarak yeni bir etiket oluşturmak de kullanıcı seçebilir (bir kullanıcı etiketi olarak da bilinir).

![Veri varlığı bir kullanıcı etiketi ve iki sözlüğü etiketleri ile etiketlenmiş](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Kullanıcı etiketleri yalnızca ücretsiz sürüm, veri Kataloğu'nda desteklenen etiket türüdür.

### <a name="hover-behavior-on-tags"></a>Etiket davranışı üzerine gelin

Veri Kataloğu Portalı'nda, iki etiket görsel olarak farklı ve mevcut farklı vurgulu davranışları türleridir. Bir kullanıcı etiketin üzerine geldiğinizde, etiket metni ve kullanıcı veya etiket eklediğiniz kullanıcıları görebilirsiniz. Bir sözlük etiketin üzerine geldiğinizde, ayrıca sözlük terimini ve terimi tam açıklamasını görüntülemek için iş sözlüğünü açmaya yönelik bir bağlantı tanımına bakın.

### <a name="search-filters-for-tags"></a>Etiketler için arama filtreleri

Sözlük etiketleri kullanıcı etiketleri hem de aranabilir olması ve arama filtrelerini olarak uygulayabilirsiniz.

## <a name="summary"></a>Özet

Azure veri Kataloğu'na ve yönetilen bağlayabileceğinizi etiketleme iş sözlüğünü kullanarak, tanımlayın, yönetmek ve tutarlı bir şekilde veri varlıklarını bulma. İş sözlüğünü kuruluş üyeleri tarafından iş sözlüğü, öğrenme yükseltebilirsiniz. Varlık bulma ve anlama basitleştirir anlamlı meta verileri yakalama sözlüğü da destekler.

## <a name="next-steps"></a>Sonraki adımlar

* [İş sözlüğü işlemleri için REST API belgeleri](/rest/api/datacatalog/data-catalog-glossary)
