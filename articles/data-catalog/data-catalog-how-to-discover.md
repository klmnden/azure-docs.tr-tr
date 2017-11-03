---
title: "Azure veri Kataloğu'nda veri kaynaklarını bulma | Microsoft Docs"
description: "Bu makalede, arama ve filtreleme ve Azure veri Kataloğu portalını isabet vurgulama özelliklerini kullanarak da dahil olmak üzere Azure veri Kataloğu ile kayıtlı veri varlıklarını nasıl bulacağınızı vurgular."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 9ff67dcb5ecb00440f73f979fd8d2b79a570c674
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri kaynaklarını bulma
## <a name="introduction"></a>Giriş
Azure veri Kataloğu kayıt ve bulma kurumsal veri kaynakları için bir sistem görevi gören bir tam olarak yönetilen bir bulut hizmetidir. Diğer bir deyişle, Bul, anlamak ve veri kaynaklarını kullanan kişilerin veri Kataloğu yardımcı olur ve daha fazla değer, var olan verilerden alma kuruluşlar yardımcı olur. Bir veri kaynağına veri Kataloğu ile kaydedildikten sonra kolayca gereksinim duyduğunuz verileri bulmak için arama yapabilmesi meta verilerini hizmeti tarafından dizine alınır.

## <a name="searching-and-filtering"></a>Arama ve filtreleme
Veri Kataloğu bulma işlemi iki birincil mekanizmayı kullanır: arama ve filtreleme.

Arama hem sezgisel hem de güçlü olacak şekilde tasarlanmıştır. Varsayılan olarak, arama terimleri kullanıcı tarafından ek açıklamalar dahil olmak üzere katalogdaki herhangi bir özellikle eşleştirilir.

Filtreleme, aramayı tamamlamak üzere tasarlanmıştır. Uzmanlar, veri kaynağı türü, nesne türü ve etiketler gibi belirli özellikleri seçebilirsiniz. Yalnızca eşleşen veri varlıklarını görüntülemek ve arama sonuçlarını eşleşen sınırlayın.

Arama ve filtreleme, bir birleşimini kullanarak, gereksinim duyduğunuz veri kaynaklarını bulmak için veri Kataloğu'na kayıtlı veri kaynaklarına hızlıca gidebilirsiniz.

## <a name="search-syntax"></a>Söz dizimi arama
Varsayılan serbest metin arama basit ve sezgisel olsa da, arama sonuçları üzerinde daha fazla denetim için veri kataloğu arama söz dizimi de kullanabilirsiniz. Veri kataloğu arama aşağıdaki teknikler destekler:

| Teknik | Kullanım | Örnek |
| --- | --- | --- |
| Basit Arama |Bir veya daha fazla arama terimi kullanan basit arama. Sonuçlar herhangi bir özelliği bir veya daha fazla belirtilen terimlerin eşleşen tüm varlıkları içerir. |`sales data` |
| Özellik kapsamı |Burada arama teriminin belirtilen özellikle eşleştirildiği veri kaynakları döndür. |`name:finance` |
| Boole işleçleri |Genişletmek veya bir aramayı Boole işleçleri kullanarak daraltın. |`finance NOT corporate` |
| Parantez ile gruplandırma |Özellikle, Boole işleçleri ile birlikte mantıksal ayırma sağlamak için sorgunun bölümlerini gruplandırmak üzere parantez kullanın. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Karşılaştırma işleçleri |Sayısal ve tarih veri türlerine sahip özellikler için eşitlik dışındaki karşılaştırmaları kullanın. |`modifiedTime > "11/05/2014"` |

Veri kataloğu arama hakkında daha fazla bilgi için bkz: [Azure veri Kataloğu](https://msdn.microsoft.com/library/azure/mt267594.aspx) makalesi.

## <a name="hit-highlighting"></a>İsabet vurgulama
Arama sonuçları, (örneğin, veri varlık adı, açıklama ve etiketleri) belirli arama terimleriyle eşleşen özellikleri sağlamak için vurgulanır herhangi görüntülenen görüntülerken belirli veri varlığını neden belirleyebilmek için tarafından verilen bir arama döndürüldü.

> [!NOTE]
> İsabet vurgulama devre dışı bırakmak için kullanmak **vurgulayın** geçin veri Kataloğu portalında.
>
>

Arama sonuçları görüntülediğinizde, bir veri varlığına bile isabet etkin vurgulama içindedir neden bu her zaman belirgin olmayabilir. Tüm özellikleri varsayılan olarak aranır olduğundan, bir veri varlığına bir eşleşme nedeniyle bir sütun düzeyinde özellik döndürülmesi. Ve birden çok kullanıcı kendi etiketleri ve açıklamaları ile kayıtlı veri varlıklarına açıklama çünkü tüm meta veri arama sonuçları listesinde görüntülenen.

Döşeme görünümü varsayılan olarak, arama sonuçlarında görüntülenen her bölme içerir bir **görünüm arama terimini** simgesini eşleşiyor ve konumlarını ve istiyorsanız, bunları atlamak için hızlı bir şekilde görüntüleyebilmek.

 ![İsabet vurgulama ve Azure veri Kataloğu Portalı'nda arama sonuçları](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Özet
Veri Kataloğu ile veri kaynağı kaydetme yapısal ve açıklayıcı meta veri kaynağından Kataloğu hizmetine kopyalar için veri kaynağını bulup anlamak daha kolay olur. Bir veri kaynağı kaydınız sonra filtreleme kullanarak bulmak ve gelen veri Kataloğu portalını arayın.

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarını bulma hakkında adım adım ayrıntılar için bkz: [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md).
