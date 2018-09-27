---
title: PostgreSQL için Azure veritabanı'nda sorgu performansı İçgörüleri
description: Bu makalede, PostgreSQL için Azure veritabanı'nda sorgu performansı İçgörüleri özelliği açıklanır.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: cc041104169ca8c4344b9d3de597283d122e63db
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394769"
---
# <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri 

**İçin geçerlidir:** 9.6 ve 10 PostgreSQL için Azure veritabanı

> [!IMPORTANT]
> Sınırlı sayıdaki bölgede genel önizlemede sorgu performansı İçgörüleri özelliğidir. 

Sorgu performansı İçgörüleri, uzun süre çalışan sorgular nedir, zaman içinde nasıl değiştiğini ve hangi bekler, bunları etkileşimimiz hızlıca tanımlamanıza yardımcı olur.

## <a name="permissions"></a>İzinler
**Sahibi** veya **katkıda bulunan** sorgu performansı İçgörüleri, sorgu metnini görüntülemek için gerekli izinleri. **Okuyucu** grafikler ve tablolar görüntüleyebilir, ancak sorgu metni yok.

## <a name="prerequisites"></a>Önkoşullar
Veri işlevi için sorgu performansı İçgörüleri için mevcut olmalıdır [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Performans öngörüleri görüntüleme
[Sorgu performansı İçgörüleri](concepts-query-performance-insight.md) Azure portalında görünümü Query Store ' anahtar bilgilerini bir görselleştirmeyi yüzey. 

PostgreSQL için Azure veritabanı sunucunuza portal sayfasında, seçin **sorgu performansı İçgörüleri** altında **destek + sorun giderme** menü çubuğu bölümü.

![Uzun süre çalışan sorguların sorgu performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

**Uzun süre çalışan sorguların** 15 dakikalık aralıklarla toplu yürütme başına ortalama süreye göre ilk 5 sorguları sekme gösterir. Daha fazla sorgu seçerek görüntüleyebilirsiniz **numarası, sorguları** açılır. Bunu yaptığınızda, grafiğin renkleri için belirli bir sorgu kimliği değişebilir.

' A tıklayın ve belirli bir zaman penceresine daraltmak için grafiğe sürükleyin. Alternatif olarak, yakınlaştırma ve simgeler uzaklaştırma küçük veya büyük bir süre boyunca sırasıyla görüntülemek için kullanın.

Grafiği aşağıdaki tabloda, bu zaman penceresinde uzun süre çalışan sorguları hakkında daha fazla ayrıntı sağlar.

Seçin **bekleme istatistikleri** bekler sunucudaki karşılık gelen görsel öğeleri görüntülemek için sekmesinde.

![Sorgu performansı İçgörüleri bekleme istatistikleri](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [izleme ve ayarlama](concepts-monitoring.md) PostgreSQL için Azure veritabanı'nda.


