---
title: PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir PostgreSQL veritabanı için Azure veritabanı PostgreSQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/07/2018
ms.openlocfilehash: ceb64781dc7e5243f785ad239c24e5f21b0481ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60421260"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en az kapalı kalma süresi ile PostgreSQL için PostgreSQL geçişleri için Azure veritabanı gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) . Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, PostgreSQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [Microsoft Azure ile uygulama Modernizasyonu](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), PostgreSQL uygulamaları, PostgreSQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [PostgreSQL için Azure veritabanı geçiş PostgreSQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
