---
title: Azure veri fabrikası veri akışı sütunu desen eşleştirme
description: Azure veri fabrikası eşleme veri akışı sütunu desenleri alanları temel alınan şema meta verilerini bakılmaksızın bir veri akışı dönüştürme genelleştirilmiş şablon modellerini oluşturmak için kullanılır
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 53d3300ea11a86c34909ba6ce0fd6c8c0c38b4b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269713"
---
# <a name="azure-data-factory-mapping-data-flow-concepts"></a>Azure veri fabrikası veri akışı kavramları eşleme

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Sabit kodlanmış sütun adları yerine desenleri temel şablon sütunları oluşturabilmesi çeşitli Azure veri fabrikası, veri akışı dönüşümleri "Sütunları desenleri" fikrini destekler. İfade oluşturucu içinde bu özellik sütunları xact, belirli bir alan adları yerine dönüştürme için eşleştirilecek desenleri tanımlamak için kullanabilirsiniz. Gelen kaynak alanları genellikle, özellikle sütunları metin dosyalarında veya NoSQL veritabanları söz konusu olduğunda değiştirmek için desen yararlıdır. Bu bazen "Şema değişikliklerini" adlandırılır.

![Sütun desenleri](media/data-flow/columnpattern2.png "sütun desenleri")

Sütun desenleri, hem şema değişikliklerini senaryoları, hem de genel senaryoları işlenmesi için kullanışlıdır. Bu, tam sütun adlarını bilmeniz mümkün olmadığı için koşullar geçerlidir. Desen sütun adına eşleşmesi ve sütun verilerini yazın ve bu işlemi herhangi bir alan veri akışında eşleşen gerçekleştirecek bir dönüştürme için bir ifade oluşturmak, `name`  &  `type` desenleri.

Bir ifade desenleri kabul eden bir dönüştürme eklerken, "Sütun deseni Ekle"'i seçin. Sütun desenleri şema değişikliklerini sütun eşleme desenleri sağlar.

Şablon sütun modellerini oluştururken kullanmanız `$$` ifadesindeki eşleşen her alan için başvuru giriş veri akışını göstermek için.

İfade Oluşturucu regex işlevlerden birini kullanmayı seçerseniz, ardından sonradan kullanabilirsiniz 1 USD $2, $3... regex ifadeniz eşleşen desenleri başvurmak için.

Sütun düzeni senaryoya örnek olarak, toplam gelen alan bir dizi kullanıyor. Toplama SUM toplama dönüşümü hesaplamalardır. Sonra alan türü her eşleşme "tamsayıyı" eşleşme TOPLAYIN ve her bir eşleştirmeyi ifadeniz başvurmak için $$'i kullanın.
