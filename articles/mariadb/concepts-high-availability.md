---
title: MariaDB için Azure veritabanı'nda yüksek kullanılabilirlik kavramları
description: Bu konuda MariaDB için Azure veritabanı kullanırken, yüksek kullanılabilirlik bilgileri sağlar
author: jasonwhowell
ms.author: jasonh
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: e1e4103360c2864f6a63f2eb0c7f146cac779cb6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53538166"
---
# <a name="high-availability-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda yüksek kullanılabilirlik kavramları
MariaDB hizmeti için Azure veritabanı, garantili yüksek düzeyde kullanılabilirlik sağlar. Finansal destekli bir hizmet düzeyi sözleşmesi (SLA), genel kullanım sonrasında % 99,99 değerindedir. Neredeyse hiçbir uygulama kesinti işbu hizmeti kullanırken.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, bir düğüm düzeyinde kesinti oluştuğunda yerleşik yük devretme mekanizmalarına temel alır. Bir düğüm düzeyinde kesinti nedeniyle bir donanım hatası veya hizmet dağıtımı yanıt oluşabilir.

Her zaman bir işlem bağlamında MariaDB veritabanı sunucusu için Azure veritabanı yapılan değişiklikler oluşur. İşlem tamamlandığında değişiklikler Azure depolama alanında eşzamanlı olarak kaydedilir. Nodu düzeyinde kesinti oluşursa, veritabanı sunucusu, otomatik olarak yeni bir düğüm oluşturur ve veri depolama için yeni düğüm ekler. Tüm etkin bağlantılar kesilir ve Hareket halindeki işlemler iletilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
MariaDB veritabanı uygulamaları algılayıp yeniden denemek için yerleşik bağlantıları bırakılan ve işlem başarısız oldu, önemlidir. Uygulamayı yeniden denediğinde uygulamanın bağlantı şeffaf bir şekilde için başarısız örneğini devreye yeni oluşturulan örneğine yönlendirilir.

Dahili olarak, Azure'da bir ağ geçidi bağlantıları yeni örneğine yeniden yönlendirmek için kullanılır. Bir kesinti sırasında tüm yük devretme işlemi genellikle onlarca saniye sürer. Dış bağlantı dizesi, yeniden yönlendirme ağ geçidi tarafından dahili olarak yönetilir olduğundan, istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Artırma veya azaltma
MariaDB için Azure veritabanı yukarı veya aşağı ölçeklendirildiğinde HA modeline benzer yeni bir sunucu örneği belirtilen boyut ile oluşturulur. Mevcut veri depolama, özgün örneğinden kullanımdan çıkarıldı ve yeni örneğine bağlı.

Ölçeklendirme işlemi sırasında veritabanı bağlantıları için bir kesinti oluşur. İstemci uygulamalarını kesilir ve açık kaydedilmemiş işlemleri iptal edilir. İstemci uygulama, bağlantı yeniden dener veya yeni bir bağlantı kurar sonra yeni boyutlu örnek bağlantı ağ geçidi yönlendirir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetine genel bakış için bkz. [MariaDB genel bakış için Azure veritabanı](overview.md)
