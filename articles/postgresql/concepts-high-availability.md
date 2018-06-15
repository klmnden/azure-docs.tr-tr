---
title: Azure veritabanı PostgreSQL için yüksek kullanılabilirlik kavramlar
description: Bu makalede, Azure veritabanı için PostgreSQL kullanırken yüksek kullanılabilirlik bilgi verilmektedir.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 203a142a21153935e172508e62b813dca95468cb
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29687091"
---
# <a name="high-availability-concepts-in-azure-database-for-postgresql"></a>Azure veritabanı PostgreSQL için yüksek kullanılabilirlik kavramlar
Azure veritabanı PostgreSQL hizmeti için bir garanti edilen yüksek düzeyde kullanılabilirlik sağlar. Mali yedeklenmiş hizmet düzeyi sözleşmesi (SLA) genel kullanılabilirliğine bağlı % 99,99 ' dir. Uygulama yok neredeyse kesinti işbu hizmeti kullanırken.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, bir düğüm düzeyi kesinti oluştuğunda yerleşik yük devretme mekanizmalarına temel alır. Bir düğüm düzeyi kesintisi bir donanım hatası nedeniyle veya bir hizmet dağıtım için yanıt oluşabilir.

Her zaman bir işlem bağlamında PostgreSQL veritabanı sunucusu için bir Azure veritabanına yapılan değişiklikleri oluşur. İşlem tamamlandığında değişiklikler Azure depolama alanında eşzamanlı olarak kaydedilir. Bir düğüm düzeyi kesinti oluşursa, veritabanı sunucusu, otomatik olarak yeni bir düğüm oluşturur ve veri depolama yeni düğüme iliştirir. Tüm etkin bağlantılar kesilir ve ınflight işlemler iletilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
Veritabanı uygulamaları algılamak ve yeniden denemek için oluşturulan PostgreSQL bağlantıları bırakılan ve işlemleri başarısız önemlidir. Uygulama yinelerken, uygulamanın bağlantısı saydam başarısız örneğinin devreye girer yeni oluşturulan örneği yeniden yönlendirilir.

Dahili olarak Azure içinde bir ağ geçidi bağlantıları için yeni örnek yönlendirmek için kullanılır. Bir kesinti tüm yük devretme işlemi genellikle on saniye sürer. Yeniden yönlendirme ağ geçidi tarafından dahili olarak işlenir olduğundan, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Yukarı veya aşağı Ölçeklendirmesi
Azure veritabanı PostgreSQL için yukarı veya aşağı ölçeklendirilir zaman HA modeline benzer, belirtilen boyutta yeni bir sunucu örneği oluşturulur. Mevcut veri depolama, özgün örneğinden ayrılmış ve yeni örneğine bağlı.

Veritabanı bağlantıları için bir kesinti ölçeklendirme işlemi sırasında oluşur. İstemci uygulamaları kesilir ve açık kaydedilmemiş işlemleri iptal edilir. İstemci uygulaması bağlantı yeniden dener veya yeni bir bağlantı yaptıktan sonra ağ geçidi bağlantısı yeni boyutlandırılmış örneğe yönlendirir. 

## <a name="next-steps"></a>Sonraki adımlar
- Hizmeti genel bakış için bkz: [Azure veritabanı PostgreSQL genel bakış](overview.md)
