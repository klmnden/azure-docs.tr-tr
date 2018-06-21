---
title: MySQL için Azure veritabanı en düşük kapalı kalma geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir MySQL veritabanı Azure veritabanı için MySQL Azure veritabanı geçiş hizmetini kullanarak geçirmek açıklar.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: ecbd35bd45bd11292bbe4a032329d704858d4c77
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293930"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en düşük kapalı kalma geçiş
Yeni sunulan kullanarak en az kapalı kalma MySQL için Azure veritabanı için MySQL geçiş gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS). Bu işlev uygulama tarafından oluşturulan kapalı kalma süresini sınırlar.

## <a name="overview"></a>Genel Bakış
DMS MySQL için şirket içi Azure veritabanı için bir başlangıç yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler Azure için sürekli olarak eşitlenir. Hedef Azure yan verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiğini almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure işaret edecek şekilde güncelleştirin. İşiniz bittiğinde, uygulamanızı Azure üzerinde dinamik olur!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

DMS geçiş MySQL kaynakları şu anda önizlemede değil. MySQL iş yüklerinizi geçirmek için servisi denemek istiyorsanız, Azure DMS kaydolun [Önizleme sayfası](https://aka.ms/dms-preview) ilginize ifade etmek için. Geri bildiriminiz çok daha fazla hizmeti geliştirmek için yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar
- Videosunu görüntüleyin [MySQL/PostgreSQL kolayca geçiş hizmeti ile yönetilen uygulamalar Azure](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), MySQL uygulamaları için Azure veritabanı için MySQL geçirmek nasıl gösteren bir tanıtım içerir.
- MySQL Azure veritabanı en düşük kapalı kalma geçişler aracılığıyla Azure DMS MySQL için sınırlı önizlemesi için kaydolun [Önizleme sayfası](https://aka.ms/dms-preview).
