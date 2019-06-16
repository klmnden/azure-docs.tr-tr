---
title: MySQL için Azure veritabanı performans önerileri
description: Bu makalede MySQL için Azure veritabanı performans önerisi özelliği
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 569ef6e9f91fdd728c5d230e2a6c46a7b01e5a62
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078826"
---
# <a name="performance-recommendations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı performans önerileri

**Şunlara uygulanır:** MySQL 5.7 için Azure veritabanı

> [!NOTE]
> Performans önerileri önizlemededir. Performans önerileri için destek Azure portalında kullanıma sunulacaktır ve henüz bölgenizde kullanılamıyor olabilir.

Performans önerileri özelliği, performansı artırmak için özelleştirilmiş öneri oluşturmak için veritabanlarınızı analiz eder. Öneriler üretmek için şema dahil olmak üzere çeşitli veritabanı özellikleri, analiz arar. Etkinleştirme [Query Store](concepts-query-store.md) performans önerileri özelliği tam olarak yararlanmak için sunucunuzdaki. Performans şema kapalı ise, Query Store üzerinde kapatma performance_schema ve performans şema Instruments özellik için gerekli bir alt kümesi sağlar. Herhangi bir performans önerisi uyguladıktan sonra bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir.

## <a name="permissions"></a>İzinler

Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri

[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

Açık **performans önerileri** gelen **akıllı performans** MySQL sunucunuzun Azure portal sayfasındaki menü çubuğunda bölümü.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Seçin **Çözümle** ve analizini başlayacak bir veritabanı seçin. İş yükünüze bağlı olarak, analiz tamamlanması birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur. Çözümleme veritabanınızın derin bir incelemesini yapar. Yoğun olmayan dönemlerde analiz almanızı öneririz.

**Önerileri** penceresi karşılanmadığı, öneriler ve bu öneriyi oluşturulan ilişkili sorgu kimliği bir listesini gösterir. Sorgu kimliği ile kullanabileceğiniz [mysql.query_store](concepts-query-store.md#mysqlquery_store) sorgusu hakkında daha fazla bilgi için görünümü.

![Performans önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Önerileri otomatik olarak uygulanmaz. Öneri uygulamak için sorgu metni kopyalayın ve tercih ettiğiniz istemcinizden çalıştırın. Öneri değerlendirmek için izleme ve test unutmayın.

## <a name="recommendation-types"></a>Öneri türleri

Şu anda yalnızca *Create Index* önerileri desteklenir.

### <a name="create-index-recommendations"></a>Dizin önerileri oluşturma

*Dizin oluşturma* en sık çalıştırma ya da zaman harcayan iş yükü sorgularda hızlandırmak için yeni dizin önerileri önerin. Bu öneri duyacağı [Query Store](concepts-query-store.md) etkinleştirilecek. Query Store sorgu bilgilerini toplar ve analiz, öneride bulunmak için kullandığı ayrıntılı sorgu çalışma zamanı ve sıklığı istatistikler sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [izleme ve ayarlama](concepts-monitoring.md) MySQL için Azure veritabanı'nda.