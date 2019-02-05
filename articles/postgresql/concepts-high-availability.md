---
title: PostgreSQL için Azure veritabanı'nda yüksek kullanılabilirlik kavramları
description: Bu makalede, PostgreSQL için Azure veritabanı kullanırken, yüksek kullanılabilirlik bilgiler sağlar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: d43647b57469efa4581dc1c74a842a51e9d54ad7
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55699826"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı'nda yüksek kullanılabilirlik kavramları
Hizmet PostgreSQL için Azure veritabanı, garantili yüksek düzeyde kullanılabilirlik sağlar. Finansal destekli bir hizmet düzeyi sözleşmesi (SLA), genel kullanım sonrasında % 99,99 değerindedir. Neredeyse hiçbir uygulama kesinti işbu hizmeti kullanırken.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, nodu düzeyinde kesinti oluştuğunda yerleşik yük devretme mekanizmalarına temel alır. Bir düğüm düzeyinde kesinti nedeniyle bir donanım hatası veya hizmet dağıtımı yanıt oluşabilir.

Her zaman bir işlem bağlamında PostgreSQL veritabanı sunucusu için Azure veritabanı yapılan değişiklikler oluşur. İşlem tamamlandığında değişiklikler Azure depolama alanında eşzamanlı olarak kaydedilir. Nodu düzeyinde kesinti oluşursa, veritabanı sunucusu, otomatik olarak yeni bir düğüm oluşturur ve veri depolama için yeni düğüm ekler. Tüm etkin bağlantılar kesilir ve Hareket halindeki işlemler iletilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
PostgreSQL veritabanı uygulamaları algılayıp yeniden denemek için yerleşik bağlantıları bırakılan ve işlem başarısız oldu, önemlidir. Uygulamayı yeniden denediğinde uygulamanın bağlantı şeffaf bir şekilde için başarısız örneğini devreye yeni oluşturulan örneğine yönlendirilir.

Dahili olarak, Azure'da bir ağ geçidi bağlantıları yeni örneğine yeniden yönlendirmek için kullanılır. Bir kesinti sırasında tüm yük devretme işlemi genellikle onlarca saniye sürer. Dış bağlantı dizesi, yeniden yönlendirme ağ geçidi tarafından dahili olarak yönetilir olduğundan, istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Artırma veya azaltma
PostgreSQL için Azure veritabanı yukarı veya aşağı ölçeklendirildiğinde HA modeline benzer yeni bir sunucu örneği belirtilen boyut ile oluşturulur. Mevcut veri depolama, özgün örneğinden kullanımdan çıkarıldı ve yeni örneğine bağlı.

Ölçeklendirme işlemi sırasında veritabanı bağlantıları için bir kesinti oluşur. İstemci uygulamalarını kesilir ve açık kaydedilmemiş işlemleri iptal edilir. İstemci uygulama, bağlantı yeniden dener veya yeni bir bağlantı kurar sonra yeni boyutlu örnek bağlantı ağ geçidi yönlendirir. 

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [geçici bağlantı hataları işleme](concepts-connectivity.md)
- Bilgi nasıl [verilerinizi okuma yinelemelerle çoğaltın](howto-read-replicas-portal.md)
