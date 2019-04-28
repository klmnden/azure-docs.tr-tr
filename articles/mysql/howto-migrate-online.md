---
title: MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir MySQL veritabanı için Azure veritabanı MySQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/07/2018
ms.openlocfilehash: 49e2662f215d845d416e46246b03e4408fae118b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61424175"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en düşük kapalı kalma süresi ile MySQL için Azure veritabanı için MySQL geçişi gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS). Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, MySQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [MySQL/PostgreSQL kolayca geçirin uygulamalarını azure'a yönetilen hizmet](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), MySQL uygulamaları, MySQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [MySQL için Azure veritabanı geçiş MySQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
