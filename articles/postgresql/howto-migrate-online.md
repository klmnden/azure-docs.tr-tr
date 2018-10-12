---
title: PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir PostgreSQL veritabanı için Azure veritabanı PostgreSQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 80e5d30677735b35d90fda6288d7bf6f2ea4aa1b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093842"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en az kapalı kalma süresi ile PostgreSQL için PostgreSQL geçişleri için Azure veritabanı gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) . Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
DMS, PostgreSQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

DMS geçiş PostgreSQL kaynakları şu anda Önizleme aşamasındadır. PostgreSQL iş yüklerinizi geçirmeye hizmetini deneyin istiyorsanız, Azure DMS kaydolun [Önizleme sayfası](https://aka.ms/dms-preview) ilgi ifade etmek için. Geri bildiriminiz yardımcı olacak daha fazla hizmeti geliştirmek için benzersizdir.

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [Microsoft Azure ile uygulama Modernizasyonu](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), PostgreSQL uygulamaları, PostgreSQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [PostgreSQL için Azure veritabanı geçiş PostgreSQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
