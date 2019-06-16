---
title: MariaDB için Azure veritabanı'nda sorgu performansı İçgörüleri
description: Bu makalede MariaDB için Azure veritabanı'nda sorgu performansı İçgörüleri özelliği
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/12/2019
ms.openlocfilehash: be9d5c4745cb03d9d3eaa324b7191d82b9d4a14e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079424"
---
# <a name="query-performance-insight-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda sorgu performansı İçgörüleri

**İçin geçerlidir:**  10.2 MariaDB için Azure veritabanı

> [!NOTE]
> Sorgu performansı İçgörüleri Önizleme aşamasındadır. Azure portalındaki sorgu performansı İçgörüleri için desteği kullanıma sunulacaktır ve henüz bölgenizde kullanılamıyor olabilir.

Sorgu performansı İçgörüleri, uzun süre çalışan sorgular nedir, zaman içinde nasıl değiştiğini ve hangi bekler, bunları etkileşimimiz hızlıca tanımlamanıza yardımcı olur.

## <a name="permissions"></a>İzinler

**Sahibi** veya **katkıda bulunan** sorgu performansı İçgörüleri, sorgu metnini görüntülemek için gerekli izinler. ** Okuyucu** grafikler ve tablolar görüntüleyebilir, ancak sorgu metni yok.

## <a name="prerequisites"></a>Önkoşullar

Veri işlevi için sorgu performansı İçgörüleri için mevcut olmalıdır [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Performans öngörüleri görüntüleme

Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar.

MariaDB için Azure veritabanı sunucunuza portal sayfasında, seçin **sorgu performansı İçgörüleri** altında **akıllı performans** menü çubuğu bölümü.

![Uzun süre çalışan sorguların sorgu performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

 **Uzun süre çalışan sorguların** 15 dakikalık aralıklarla toplu yürütme başına ortalama süreye göre ilk 5 sorguları sekme gösterir. Daha fazla sorgu seçerek görüntüleyebilirsiniz **numarası, sorguları** açılır. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, yakınlaştırma ve simgeler uzaklaştırma küçük veya büyük süre sırasıyla görüntülemek için kullanın.

Seçin **bekleme istatistikleri** bekler sunucudaki karşılık gelen görsel öğeleri görüntülemek için sekmesinde.

Bekleme istatistikleri görüntülenmesini sorguları, belirtilen zaman aralığı boyunca büyük bekler sergileyen sorguları göre gruplandırılır.

![İstatistikleri sorgu performansı İçgörüleri bekler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [izleme ve ayarlama](concepts-monitoring.md) MariaDB için Azure veritabanı'nda.