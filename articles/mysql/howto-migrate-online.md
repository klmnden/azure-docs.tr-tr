---
title: MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir MySQL veritabanı için Azure veritabanı MySQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 55d5cf97225508d6c7c490347cfe21ced832300e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091731"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en düşük kapalı kalma süresi ile MySQL için Azure veritabanı için MySQL geçişi gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS). Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
DMS, MySQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

- DMS geçiş MySQL kaynakları şu anda genel Önizleme aşamasındadır. MySQL iş yüklerinizi geçirmeye hizmetini deneyin istiyorsanız, portalda devam edin. Geri bildiriminiz yardımcı olacak daha fazla hizmeti geliştirmek için benzersizdir.

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [MySQL/PostgreSQL kolayca geçirin uygulamalarını azure'a yönetilen hizmet](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), MySQL uygulamaları, MySQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- [MySQL, MySQL için Azure veritabanı'na geçirme çevrimiçi DMS kullanarak] (https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
