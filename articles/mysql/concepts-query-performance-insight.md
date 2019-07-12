---
title: MySQL için Azure veritabanı'nda sorgu performansı İçgörüleri
description: Bu makalede MySQL için Azure veritabanı'nda sorgu performansı İçgörüleri özelliği
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 8f142933ebf955cbe3aa119f42779109fb6ef7db
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67589063"
---
# <a name="query-performance-insight-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda sorgu performansı İçgörüleri

**İçin geçerlidir:**  MySQL 5.7 için Azure veritabanı

> [!NOTE]
> Sorgu performansı İçgörüleri Önizleme aşamasındadır.

Sorgu performansı İçgörüleri, uzun süre çalışan sorgular nedir, zaman içinde nasıl değiştiğini ve hangi bekler, bunları etkileşimimiz hızlıca tanımlamanıza yardımcı olur.

## <a name="common-scenarios"></a>Genel senaryolar

### <a name="long-running-queries"></a>Uzun süren sorgular

- Uzun çalışan sorguları geçmişte tanımlayan X saat
- Kaynaklarını bekleyen ilk N sorguları tanımlama
 
### <a name="wait-statistics"></a>İstatistikleri bekleyin

- Bir sorgu için bekleme yapısı anlama
- Kaynak bekler ve kaynak çekişmesi mevcut olduğu için eğilimleri anlama

## <a name="permissions"></a>İzinler

**Sahibi** veya **katkıda bulunan** sorgu performansı İçgörüleri, sorgu metnini görüntülemek için gerekli izinler. ** Okuyucu** grafikler ve tablolar görüntüleyebilir, ancak sorgu metni yok.

## <a name="prerequisites"></a>Önkoşullar

Veri işlevi için sorgu performansı İçgörüleri için mevcut olmalıdır [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Performans öngörüleri görüntüleme

Azure portaldaki [Sorgu Performansı İçgörüleri](concepts-query-performance-insight.md) görünümü, Query Store’dan alınan önemli bilgilerdeki görselleştirmeleri kullanıma açar.

MySQL için Azure veritabanı sunucunuza portal sayfasında, seçin **sorgu performansı İçgörüleri** altında **akıllı performans** menü çubuğu bölümü.

### <a name="long-running-queries"></a>Uzun süren sorgular

 **Uzun süre çalışan sorguların** 15 dakikalık aralıklarla toplu yürütme başına ortalama süreye göre ilk 5 sorguları sekme gösterir. Daha fazla sorgu seçerek görüntüleyebilirsiniz **numarası, sorguları** açılır. Bunu yaptığınızda, grafik renkleri belirli bir Sorgu Kimliği için değişebilir.

Belirli bir zaman penceresine daraltmak için grafikte tıklayıp sürükleyebilirsiniz. Alternatif olarak, yakınlaştırma ve simgeler uzaklaştırma küçük veya büyük süre sırasıyla görüntülemek için kullanın.

![Uzun süre çalışan sorguların sorgu performansı İçgörüleri](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png) 

### <a name="wait-statistics"></a>İstatistikleri bekleyin

> [!NOTE]
> Bekleme istatistikleri sorgu performansı sorunlarını gidermek için yöneliktir. Yalnızca sorun giderme amacıyla açık olması önerilir.

Bekleme istatistikleri, belirli bir sorgu yürütme sırasında gerçekleşen bekleme olayları bir görünümünü sağlar. Bekleme olay türleri hakkında daha fazla bilgi [MySQL altyapısı belgeleri](https://go.microsoft.com/fwlink/?linkid=2098206).

Seçin **bekleme istatistikleri** bekler sunucudaki karşılık gelen görsel öğeleri görüntülemek için sekmesinde.

Bekleme istatistikleri görüntülenmesini sorguları, belirtilen zaman aralığı boyunca büyük bekler sergileyen sorguları göre gruplandırılır.

![İstatistikleri sorgu performansı İçgörüleri bekler](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [izleme ve ayarlama](concepts-monitoring.md) MySQL için Azure veritabanı'nda.