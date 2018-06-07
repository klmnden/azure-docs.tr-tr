---
title: Azure depolama için yönergeler Tablo Tasarımı | Microsoft Docs
description: Okuma işlemleri verimli bir şekilde desteklemek için Azure tablo hizmetinizi tasarlayın.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 5329d33aee1bb1a55e9982b1ba9e3e8329246980
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661042"
---
# <a name="guidelines-for-table-design"></a>Tablo Tasarımı için yönergeler

Azure depolama tablo hizmeti ile kullanılmak üzere tabloları tasarlama çok ilişkisel veritabanı için tasarım değerlendirmeleri farklıdır. Bu makalede verimli okunması ve verimli yazmak için tablo hizmeti çözümünüzü tasarlama yönergeleri açıklar.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Okuma etkili olması için tablo hizmeti çözümünüzü tasarlama

* ***Okuma ağır uygulamalarda sorgulamaya tasarlayın.*** Tabloları tasarlarken, yürütecek sorguları hakkında (özellikle önemli olanları gecikme) düşündüğünüz varlıklarınızı nasıl güncelleştirecektir hakkında düşünmek önce. Bu genellikle bir verimli ve kullanıcı çözümde sonuçlanır.  
* ***PartitionKey ve RowKey sorgularınızda belirtin.*** *Noktası sorguları* en verimli tablo hizmeti sorguları bunlar gibi.  
* ***Varlıkları yinelenen kopyalarını depolamayı düşünün.*** Tablo depolama ucuz böylece daha verimli sorguları etkinleştirmek için birden çok kez (anahtarları farklı) aynı varlığa depolama göz önünde bulundurun.  
* ***Verilerinizi denormalizing göz önünde bulundurun.*** Tablo depolama ucuz böylece verilerinizi denormalizing göz önünde bulundurun. Örneğin, veri toplama için sorgular yalnızca tek bir varlık erişmesi gereken şekilde Özet varlıklar depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Sahip olduğunuz yalnızca anahtarlar **PartitionKey** ve **RowKey**. Örneğin, bileşik anahtar değerlerinden varlıklar için alternatif anahtarlı erişim yolları etkinleştirmek için kullanın.  
* ***Sorgu projeksiyon kullanın.*** Gereksinim alanları seçin sorgularını kullanarak ağ üzerinden aktarım veri miktarını azaltır.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Yazma etkili olması için tablo hizmeti çözümünüzü tasarlama  

* ***Sık kullanılan bölümleri oluşturmayın.*** İsteklerinizi birden çok bölüm zaman herhangi bir noktada üzerinden yayılan sağlayan anahtarları'i seçin.  
* ***Ani trafiğinin kaçının.*** Trafiği makul bir süre boyunca kesintisiz ve ani trafiğinin kaçının.
* ***Her varlık türü için ayrı bir tablo mutlaka oluşturmayın.*** Varlık türlerine atomik işlemleri gerektirdiğinde bu birden çok varlık türleri aynı tabloda aynı bölüm depolayabilirsiniz.
* ***En yüksek verimlilik elde gerekir göz önünde bulundurun.*** Tablo hizmeti ölçeklenebilirlik hedefleri farkında olmanız ve tasarımınızı, bunları aşmasına neden olmanız gerekir.  

Bu kılavuzu okumadan gibi tüm bu ilkeler uygulamaya koymanın örnekler görürsünüz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verileri şifrele](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)