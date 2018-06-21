---
title: Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için
description: Bu makalede, Azure veritabanı geçiş hizmetini kullanarak PostgreSQL için Azure veritabanı PostgreSQL veritabanına en az kapalı kalma geçişini gerçekleştirmek açıklar.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 9ab5d4615a8baf763d0b7ee47bf0890124f8665c
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292551"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Azure veritabanı en düşük kapalı kalma geçiş PostgreSQL için
Yeni sunulan kullanarak en az kapalı kalma süresi ile PostgreSQL için Azure veritabanı PostgreSQL geçiş gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) . Bu işlev uygulama tarafından oluşturulan kapalı kalma süresini sınırlar.

## <a name="overview"></a>Genel Bakış
DMS başlangıç bir şirket içi Azure veritabanı yüklemesini PostgreSQL için gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler Azure için sürekli olarak eşitlenir. Hedef Azure yan verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiğini almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure işaret edecek şekilde güncelleştirin. İşiniz bittiğinde, uygulamanızı Azure üzerinde dinamik olur!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

DMS geçiş PostgreSQL kaynakları şu anda önizlemede değil. Servisi PostgreSQL iş yüklerinizi geçirmek için denemek istiyorsanız, Azure DMS kaydolun [Önizleme sayfası](https://aka.ms/dms-preview) ilginize ifade etmek için. Geri bildiriminiz çok daha fazla hizmeti geliştirmek için yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar
- Video görüntüleme [Microsoft Azure ile uygulama Modernization](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), PostgreSQL uygulamalar için PostgreSQL Azure veritabanına geçirme yöntemleri gösteren bir tanıtım içerir.
- Azure veritabanı PostgreSQL en düşük kapalı kalma geçişler aracılığıyla Azure DMS PostgreSQL için sınırlı önizlemesi için kaydolun [Önizleme sayfası](https://aka.ms/dms-preview).
