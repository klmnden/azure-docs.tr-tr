---
title: PostgreSQL için Azure veritabanı'nda sorgu performansı İçgörüleri
description: Bu makalede, PostgreSQL için Azure veritabanı'nda sorgu performansı İçgörüleri özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 1a71fb81acc91036ce12ff15f6b2762b808c7473
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53541633"
---
# <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri 

**Şunlara uygulanır:** 9.6 ve 10 PostgreSQL için Azure veritabanı

> [!IMPORTANT]
> Sorgu performansı İçgörüleri özelliği genel Önizleme aşamasındadır. 

Sorgu performansı İçgörüleri, uzun süre çalışan sorgular nedir, zaman içinde nasıl değiştiğini ve hangi bekler, bunları etkileşimimiz hızlıca tanımlamanıza yardımcı olur.

## <a name="permissions"></a>İzinler
Sorgu Performansı İçgörüleri’ndeki metni görünüm için **Sahip** veya **Katkıda bulunan** izinleri gereklidir. **Okuyucu**, grafikleri ve tabloları görüntüleyebilir ancak metni sorgulayamaz.

## <a name="prerequisites"></a>Önkoşullar
Veri işlevi için sorgu performansı İçgörüleri için mevcut olmalıdır [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Performans öngörüleri görüntüleme
Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar. 

PostgreSQL için Azure veritabanı sunucunuza portal sayfasında, seçin **sorgu performansı İçgörüleri** altında **destek + sorun giderme** menü çubuğu bölümü.

![Uzun süre çalışan sorguların sorgu performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

**Uzun süren sorgular** sekmesi, yürütme başına ortalama 15 dakikalık aralıklarla toplanan en iyi 5 sorguyu gösterir. **Sorgu Sayısı** açılır menüsünden seçerek, daha fazla sorgu görüntüleyebilirsiniz. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, yakınlaştırma ve simgeler uzaklaştırma küçük veya büyük bir süre boyunca sırasıyla görüntülemek için kullanın.

Grafiği aşağıdaki tabloda, bu zaman penceresinde uzun süre çalışan sorguları hakkında daha fazla ayrıntı sağlar.

Sunucudaki beklemelerle ilgili görselleştirmeleri görüntülemek için **Bekleme İstatistikleri** sekmesini seçin.

![Sorgu Performansı İçgörüleri bekleme istatistikleri](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.


