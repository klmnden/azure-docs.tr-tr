---
title: Yönergeler için Azure depolama tablo Tasarım | Microsoft Docs
description: Okuma işlemleri verimli bir şekilde desteklemek için Azure tablo hizmeti tasarlayın.
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 04/23/2018
ms.date: 12/10/2018
ms.author: v-jay
ms.component: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269856"
---
# <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler

Azure depolama tablo hizmeti ile kullanmak için tabloları tasarlama çok ilişkisel bir veritabanı için tasarım konuları farklıdır. Bu makalede, verimli okunması ve verimli yazmak için tablo hizmeti çözümünüzü tasarlama yönergeleri açıklar.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Okuma açısından verimli olması için tablo hizmeti çözümünüzü tasarlayın

* ***Okuma yoğunluklu uygulamalar sorgulama için tasarlayın.*** Tablolarınızı tasarlarken, yürütülen sorguları hakkında (özellikle önemli olanları gecikme) düşünme önce varlıklarınızın nasıl güncelleştirilir hakkında düşünün. Bu genellikle, verimli ve yüksek performanslı bir çözümde de sonuçlanır.  
* ***PartitionKey ve RowKey hem sorgularınızdaki belirtin.*** *İşaret sorguları* en verimli tablo hizmeti sorguları bunlar gibi.  
* ***Varlıkları yinelenen kopyalarını depolamayı düşünün.*** Tablo depolama ucuz, bu nedenle aynı varlık daha verimli sorgular etkinleştirmek için birden çok kez (anahtarları farklı) depolama göz önünde bulundurun.  
* ***Verilerinizi normal durumdan çıkarmayı düşünün.*** Tablo depolama ucuz, böylece verilerinizi normal durumdan çıkarmayı düşünün. Örneğin, veri toplama için sorgular yalnızca tek bir varlık erişmesi gereken şekilde Özet varlıkları depolayın.  
* ***Bileşik anahtar değerlerini kullanın.*** Sahip olduğunuz yalnızca anahtarları **PartitionKey** ve **RowKey**. Örneğin, varlıklara alternatif anahtarlı erişim yolları etkinleştirmek için bileşik bir anahtar değerlerini kullanın.  
* ***Sorgu projeksiyon kullanın.*** Yalnızca istediğiniz alanları seçin sorgularını kullanarak ağ üzerinden aktarım veri miktarını azaltabilirsiniz.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Yazma etkili olması için tablo hizmeti çözümünüzü tasarlayın  

* ***Etkin bölümler oluşturmayın.*** Birden çok bölüm zaman herhangi bir noktada isteklerinizi yayılmış olanak tanıyan tuşlarını seçin.  
* ***Trafiğindeki ani artışları kaçının.*** Trafik makul bir süre kesintisiz ve ani trafik kaçının.
* ***Her varlık türü için ayrı bir tabloda mutlaka oluşturmayın.*** Varlık türlerinde atomik işlemler ihtiyaç duyduğunuzda, bu varlık türleri aynı tablodaki aynı bölümde depolayabilirsiniz.
* ***En fazla aktarım hızı elde etmek gerekir göz önünde bulundurun.*** Tablo hizmeti için ölçeklenebilirlik hedefleri bilmeniz ve tasarımınızı, bunları aşmasına neden olmayacak emin olmanız gerekir.  

Bu kılavuzu okumadan gibi tüm bu ilkeleri uygulamaya koymanın örnekler göreceksiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [Sorgulama için Tasarım](table-storage-design-for-query.md)
- [Tablo verilerini şifreleme](table-storage-design-encrypt-data.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)