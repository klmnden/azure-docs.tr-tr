---
title: Çok az kesinti PostgreSQL - tek bir sunucu için Azure veritabanı geçişi
description: Bu makalede, PostgreSQL - Azure veritabanı geçiş Hizmeti'ni kullanarak tek sunucu için Azure veritabanı bir PostgreSQL veritabanına bir kapalı kalma süresi en az geçişi gerçekleştirmek açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 93cd390889c023adf1c30a8470e1c2298598439e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067522"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Çok az kesinti PostgreSQL - tek bir sunucu için Azure veritabanı geçişi
Yeni çıkan kullanarak en az kapalı kalma süresi ile PostgreSQL için PostgreSQL geçişleri için Azure veritabanı gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) . Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, PostgreSQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [Microsoft Azure ile uygulama Modernizasyonu](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), PostgreSQL uygulamaları, PostgreSQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [PostgreSQL için Azure veritabanı geçiş PostgreSQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
