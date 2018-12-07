---
title: MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir MySQL veritabanı için Azure veritabanı MySQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 267212d8f832b96bf66145b97c3464471e28593d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014200"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>MySQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en düşük kapalı kalma süresi ile MySQL için Azure veritabanı için MySQL geçişi gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS). Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, MySQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [MySQL/PostgreSQL kolayca geçirin uygulamalarını azure'a yönetilen hizmet](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), MySQL uygulamaları, MySQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [MySQL için Azure veritabanı geçiş MySQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
