---
title: Azure veri fabrikası veri akışı sütunu desen eşleştirme
description: Azure veri fabrikası sütun desenleri alanları temel alınan şema meta verilerini bakılmaksızın bir veri akışı dönüştürme genelleştirilmiş şablon modellerini oluşturmak için veri akışı eşleme kullanmayı öğrenin
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 5131329f6675bc86374f5a5c081e0aaa7d36c0fe
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155232"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Azure veri fabrikası eşleme veri akışları sütun desenleri

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Sabit kodlanmış sütun adları yerine desenleri temel şablon sütunları oluşturabilmesi çeşitli Azure veri fabrikası, veri akışı dönüşümleri "Sütunları desenleri" fikrini destekler. İfade oluşturucu içinde bu özellik sütunları xact, belirli bir alan adları yerine dönüştürme için eşleştirilecek desenleri tanımlamak için kullanabilirsiniz. Gelen kaynak alanları genellikle, özellikle metin dosyalarında veya NoSQL veritabanları sütunları değiştirme söz konusu olduğunda değiştirirseniz desenleri kullanışlıdır. Bu durum bazen "Şema değişikliklerini" adlandırılır.

![Sütun desenleri](media/data-flow/columnpattern2.png "sütun desenleri")

Sütun desenleri, hem şema değişikliklerini senaryoları, hem de genel senaryoları işlenmesi için kullanışlıdır. Bu, tam sütun adlarını bilmeniz mümkün olmadığı için koşullar geçerlidir. Desen sütun adına eşleşmesi ve sütun verilerini yazın ve bu işlemi herhangi bir alan veri akışında eşleşen gerçekleştirecek bir dönüştürme için bir ifade oluşturmak, `name`  &  `type` desenleri.

Bir ifade desenleri kabul eden bir dönüştürme eklerken, "Sütun deseni Ekle"'i seçin. Sütun desenleri şema değişikliklerini sütun eşleme desenleri sağlar.

Şablon sütun modellerini oluştururken kullanmanız `$$` ifadesindeki eşleşen her alan için başvuru giriş veri akışını göstermek için.

İfade Oluşturucu regex işlevlerden birini kullanmayı seçerseniz, ardından sonradan kullanabilirsiniz 1 USD $2, $3... regex ifadeniz eşleşen desenleri başvurmak için.

Sütun düzeni senaryoya örnek olarak, toplam gelen alan bir dizi kullanıyor. Toplama SUM toplama dönüşümü hesaplamalardır. Sonra alan türü her eşleşme "tamsayıyı" eşleşme TOPLAYIN ve her bir eşleştirmeyi ifadeniz başvurmak için $$'i kullanın.
