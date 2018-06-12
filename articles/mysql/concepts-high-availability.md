---
title: MySQL için Azure veritabanında yüksek kullanılabilirlik kavramları
description: Azure veritabanı için MySQL kullanırken bu konu, yüksek kullanılabilirlik bilgileri sağlar
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 90dc603c0ee520774bd22531c7136e0949f6cf90
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264189"
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>MySQL için Azure veritabanında yüksek kullanılabilirlik kavramları
MySQL hizmeti için Azure veritabanı bir garanti edilen yüksek düzeyde kullanılabilirlik sağlar. Mali yedeklenmiş hizmet düzeyi sözleşmesi (SLA) genel kullanılabilirliğine bağlı % 99,99 ' dir. Uygulama yok neredeyse kesinti işbu hizmeti kullanırken.

## <a name="high-availability"></a>Yüksek kullanılabilirlik
Yüksek kullanılabilirlik (HA) modeli, bir düğüm düzeyi kesinti oluştuğunda yerleşik yük devretme mekanizmalarına temel alır. Bir düğüm düzeyi kesintisi bir donanım hatası nedeniyle veya bir hizmet dağıtım için yanıt oluşabilir.

MySQL veritabanı sunucusu için bir Azure veritabanına yapılan değişiklikleri her zaman bir işlem bağlamında oluşur. İşlem tamamlandığında değişiklikler Azure depolama alanında eşzamanlı olarak kaydedilir. Bir düğüm düzeyi kesinti oluşursa, veritabanı sunucusu, otomatik olarak yeni bir düğüm oluşturur ve veri depolama yeni düğüme iliştirir. Tüm etkin bağlantılar kesilir ve ınflight işlemler iletilmez.

## <a name="application-retry-logic-is-essential"></a>Uygulama yeniden deneme mantığı gereklidir
MySQL veritabanı uygulamaları algılamak ve yeniden denemek için oluşturulan bağlantıları bırakılan ve işlemleri başarısız önemlidir. Uygulama yinelerken, uygulamanın bağlantısı saydam başarısız örneğinin devreye girer yeni oluşturulan örneği yeniden yönlendirilir.

Dahili olarak Azure içinde bir ağ geçidi bağlantıları için yeni örnek yönlendirmek için kullanılır. Bir kesinti tüm yük devretme işlemi genellikle on saniye sürer. Yeniden yönlendirme ağ geçidi tarafından dahili olarak işlenir olduğundan, dış bağlantı dizesi istemci uygulamaları için aynı kalır.

## <a name="scaling-up-or-down"></a>Yukarı veya aşağı Ölçeklendirmesi
Bir Azure veritabanı için MySQL yukarı veya aşağı ölçeklendirilir zaman HA modeline benzer, belirtilen boyutta yeni bir sunucu örneği oluşturulur. Mevcut veri depolama, özgün örneğinden ayrılmış ve yeni örneğine bağlı.

Veritabanı bağlantıları için bir kesinti ölçeklendirme işlemi sırasında oluşur. İstemci uygulamaları kesilir ve açık kaydedilmemiş işlemleri iptal edilir. İstemci uygulaması bağlantı yeniden dener veya yeni bir bağlantı yaptıktan sonra ağ geçidi bağlantısı yeni boyutlandırılmış örneğe yönlendirir. 

## <a name="next-steps"></a>Sonraki adımlar
- Hizmeti genel bakış için bkz: [Azure veritabanı için MySQL genel bakış](overview.md)
