---
title: "Azure veritabanında PostgreSQL için yüksek kullanılabilirlik kavramları | Microsoft Docs"
description: "Azure veritabanı PostgreSQL için kullanırken bu konu, yüksek kullanılabilirlik bilgileri sağlar"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/19/2017
ms.openlocfilehash: 600896cf064770a5b294f874dc29081f0ce7d942
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
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
