---
title: PostgreSQL - tek bir sunucu için Azure veritabanı performans önerileri
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı performans önerisi özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 31d8c0fdf1b4df3ee00f3652c933b4b738384bea
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068835"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>PostgreSQL - tek bir sunucu için Azure veritabanı performans önerileri

**Şunlara uygulanır:** -Tek bir sunucu 9.6 ve 10 olan PostgreSQL için Azure veritabanı

Performans önerileri özellik Gelişmiş performans için özelleştirilmiş öneri oluşturmak için veritabanlarınızı olmaktadır. Öneriler üretmek için şema dahil olmak üzere çeşitli veritabanı özellikleri, analiz arar. Etkinleştirme [Query Store](concepts-query-store.md) performans önerileri özelliği tam olarak yararlanmak için sunucunuzdaki. Herhangi bir performans önerisi uyguladıktan sonra bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir. 

## <a name="permissions"></a>İzinler
Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri
[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

Açık **performans önerileri** gelen **akıllı performans** PostgreSQL sunucunuza Azure portal sayfasındaki menü çubuğunda bölümü.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Seçin **Çözümle** ve analizini başlayacak bir veritabanı seçin. İş yükünüze bağlı olarak, th analiz tamamlanması birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur. Çözümleme veritabanınızın derin bir incelemesini yapar. Yoğun olmayan dönemlerde analiz almanızı öneririz. 

**Önerileri** penceresi bulunmazsa, tüm önerilerin bir listesi gösterilir.

![Performans önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Önerileri otomatik olarak uygulanmaz. Öneri uygulamak için sorgu metni kopyalayın ve tercih ettiğiniz istemcinizden çalıştırın. Öneri değerlendirmek için izleme ve test unutmayın. 

## <a name="recommendation-types"></a>Öneri türleri

Şu anda önerileri iki türleri desteklenir: *Dizin oluşturma* ve *Drop Index*.

### <a name="create-index-recommendations"></a>Dizin önerileri oluşturma
*Dizin oluşturma* en sık çalıştırma ya da zaman harcayan iş yükü sorgularda hızlandırmak için yeni dizin önerileri önerin. Bu öneri duyacağı [Query Store](concepts-query-store.md) etkinleştirilecek. Query Store sorgu bilgilerini toplar ve analiz, öneride bulunmak için kullandığı ayrıntılı sorgu çalışma zamanı ve sıklığı istatistikler sağlar.

### <a name="drop-index-recommendations"></a>Bırakma dizin önerileri
Eksik dizinleri algılama yanı sıra PostgreSQL için Azure veritabanı, mevcut dizin performansını analiz eder. Dizin nadiren kullanılan veya yedek varsa, sürükleyip bırakarak Çözümleyicisi önerir.


## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.

