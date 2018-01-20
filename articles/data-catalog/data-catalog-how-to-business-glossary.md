---
title: "İş sözlüğünü yönetilen Azure veri Kataloğu'nda etiketleme için ayarlama | Microsoft Docs"
description: "Kayıtlı veri varlıklarını tanımlama ve etiket için ortak bir iş sözlüğü kullanmak için Azure veri Kataloğu'ndaki iş sözlüğünü vurgulama nasıl yapılır makalesi."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: a80b7fd0c21851a6670431e9b8647ca5cf5f51ec
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>İş sözlüğünü yönetilen etiketleme için ayarlama
## <a name="introduction"></a>Giriş
Kolayca bulmak ve analiz gerçekleştirmek ve kararlar için gereken veri kaynakları anlamak için azure veri Kataloğu veri kaynağı bulma sağlar. Bulma ve kullanılabilir veri kaynakları çok çeşitli anlamak, bu yetenekleri büyük etkisi yapın.

Varlıklar veri büyük anlama yükseltir bir veri Kataloğu özellik etiketleme. Etiketleme kullanarak, bir varlık veya sırayla arama veya gözatma yoluyla varlık bulmak daha kolay anlaşılır bir sütunu anahtar sözcükleri ilişkilendirebilirsiniz. Etiketleme da daha fazla kolayca bağlamı ve varlık amacı anlamanıza yardımcı olur.

Ancak, etiketleme bazen kendi sorunlara neden olabilir. Etiketleme getirebilir sorunları bazı örnekleri şunlardır:

* Bazı varlıkların üzerinde kısaltmalar ve diğerleri üzerinde genişletilmiş metin kullanımı. Hedefi aynı etiketi ile varlıkları etiketlemek için olmasına rağmen bu tutarsızlığı varlıklar, bulma yavaşlattığını.
* Bağlam bağlı olarak bir anlam olası Çeşitlemeler. Örneğin, bir etiketi adlı *gelir* bir Müşteri geliri müşteri tarafından veri kümesi gelebilir, ancak üç aylık satış veri kümesi üzerinde aynı etiketi şirket için üç aylık gelir anlamına gelebilir.  

Bunlar ve diğer benzer sorunları gidermek için veri kataloğu bir iş sözlüğü içerir.

Veri Kataloğu iş sözlüğünü kullanarak, bir kuruluşun anahtar iş hüküm ve ortak bir iş sözlüğü oluşturmak için tanımlarını belge. Bu idare kuruluş genelinde tutarlılık veri kullanımı sağlar. Bir terim iş sözlüğünü tanımlandıktan sonra bir veri varlığına kataloğunda atanabilir. Bu yaklaşım *etiketleme yönetilen*, etiketleme olarak aynı yaklaşımdır.

## <a name="glossary-availability-and-privileges"></a>Sözlük kullanılabilirlik ve ayrıcalıkları
İş sözlüğünü yalnızca, Azure veri Kataloğu standart sürümü mevcuttur. Boş veri Kataloğu sürümü bir sözlük içermez ve yetenekleri yönetilen etiketleme için sağlamaz.

İş sözlüğünü aracılığıyla erişebilirsiniz **sözlüğü** veri Kataloğu portal'ın gezinti menüsünde seçeneği.  

![İş sözlüğünü erişme](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Veri Kataloğu Yöneticiler ve sözlük administrators rolünün üyeleri oluşturabilir, düzenleme ve terimler iş sözlüğünü de silebilirsiniz. Tüm veri Kataloğu kullanıcılara terim tanımları ve sözlük terimlerini etiketi varlıkları görüntüleyebilirsiniz.

![Yeni bir sözlük terim ekleme](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Terimler oluşturma
Veri Kataloğu yöneticileri ve sözlük yöneticileri oluşturabilirsiniz terimler tıklayarak **yeni terim** düğmesi. Her sözlük terimi aşağıdaki alanları içerir:

* Terim için bir iş tanımı
* Varlık veya sütun için iş kuralları ve kullanım yakalayan bir açıklama
* En çok terimi hakkında bilmeniz Paydaşlar listesi
* Terim düzenlenir hiyerarşi tanımlar üst terimi

## <a name="glossary-term-hierarchies"></a>Sözlük terimi hiyerarşileri
Veri Kataloğu iş sözlüğünü kullanarak, bir kuruluşun kendi iş sözlüğü terimlerin hiyerarşi olarak tanımlayabilir ve daha iyi iş sınıflandırması temsil eden bir sınıflandırma koşulları oluşturabilirsiniz.

Bir terim hiyerarşisinin belirli bir düzeyde benzersiz olması gerekir. Yinelenen adlara izin verilmiyor. Hiyerarşisindeki düzeylerin sayısına bir sınır yoktur, ancak üç düzeyi veya daha az olduğunda bir hiyerarşi genellikle daha kolay anlaşılır.

İş sözlüğünü hiyerarşileri kullanımı isteğe bağlıdır. Üst terimler için terim alan boş bırakılırsa sözlükteki koşulları düz (hiyerarşik olmayan) listesi oluşturur.  

## <a name="tagging-assets-with-glossary-terms"></a>Sözlük terimlerini varlıklarına etiketleme
Terimler katalog içindeki tanımlandıktan sonra varlıklar etiketleme deneyimi bir kullanıcı bir etiket türleri gibi sözlüğü aramak için optimize edilmiştir. Veri Kataloğu portalını aralarından seçim yapabileceğiniz eşleşen terimler listesini görüntüler. Kullanıcı listesinden bir sözlük terimi seçerse, terimi varlık için (bir sözlük etiketi olarak da bilinir) bir etiket olarak eklenir. Kullanıcı sözlükteki değil bir terimi yazarak yeni bir etiket oluşturmak de seçebilir (bir kullanıcı etiketi olarak da bilinir).

![Bir kullanıcı etiketi ve iki sözlüğü etiketleri veri varlığına etiketli](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Kullanıcı etiketleri etiketi boş veri Kataloğu sürümü desteklenen tek türüdür.
>
>

### <a name="hover-behavior-on-tags"></a>Etiketler hakkında vurgulu davranışı
Veri Kataloğu portalında iki etiketleri görsel olarak farklı ve mevcut farklı vurgulu davranışları türleridir. Bir kullanıcının etiket üzerine geldiğinizde, etiket metni ve kullanıcı veya etiketi eklediğiniz kullanıcıları görebilirsiniz. Bir sözlük etiketi geldiğinizde, sözlük terimi ve tam terimin tanımını görüntülemek için iş sözlüğünü açmak için bir bağlantı tanımını de görebilirsiniz.

### <a name="search-filters-for-tags"></a>Etiketleri için arama filtreleri
Sözlük etiketleri kullanıcı etiketleri hem de aranabilir olması ve aramaya filtre uygulayabilirsiniz.

## <a name="summary"></a>Özet
Azure veri kataloğu ve kullanıma açar governed etiketleme iş sözlüğünü kullanarak tanımlamak, yönetmek ve tutarlı bir şekilde veri varlıklarını bulma. İş sözlüğünü kuruluş üyeleri tarafından iş sözlüğü öğrenme yükseltebilirsiniz. Varlık bulma ve anlama basitleştirir anlamlı meta veri yakalama sözlüğü de destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [İş sözlüğü işlemleri için REST API belgeleri](https://msdn.microsoft.com/library/mt708855.aspx)
