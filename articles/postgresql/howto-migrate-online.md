---
title: PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
description: Bu makalede, bir kapalı kalma süresi en az bir PostgreSQL veritabanı için Azure veritabanı PostgreSQL için Azure veritabanı geçiş hizmeti kullanarak geçirmek açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 0c8c3443a19c26dade9699560e883969d3c074df
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53010850"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı en az-kapalı kalma süresiyle geçiş
Yeni çıkan kullanarak en az kapalı kalma süresi ile PostgreSQL için PostgreSQL geçişleri için Azure veritabanı gerçekleştirebilirsiniz **sürekli eşitleme özelliği** için [Azure veritabanı geçiş hizmeti](https://aka.ms/get-dms) (DMS) . Bu işlev, uygulama tarafından tahakkuk ettirilen kapalı kalma süresi miktarını sınırlar.

## <a name="overview"></a>Genel Bakış
Azure DMS, PostgreSQL için bir başlangıç şirket içi Azure veritabanı yüklemesini gerçekleştirir ve uygulama çalışmaya devam ederken yeni işlemler azure'a sürekli olarak eşitler. Hedef Azure tarafında verilerini yakalar sonra kısa bir süre (en düşük kapalı kalma süresi) uygulamayı durdurun, verilerin (uygulama herhangi bir yeni trafiği almak etkili bir şekilde kullanılabilir olana kadar uygulama durdurmanız zamandan) son toplu iş bekleyin yakalamak için Hedef ayarlama ve sonra bağlantı dizenizi Azure'a işaret edecek şekilde güncelleştirin. İşlemi tamamladığınızda, uygulamanız Azure'da Canlı olacak!

![Azure veritabanı geçiş hizmeti ile sürekli eşitleme](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Sonraki adımlar
- Videoyu görüntülemek [Microsoft Azure ile uygulama Modernizasyonu](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), PostgreSQL uygulamaları, PostgreSQL için Azure veritabanı'na geçirme gösteren bir tanıtımı içerir.
- Öğreticiye bakın [PostgreSQL için Azure veritabanı geçiş PostgreSQL çevrimiçi DMS kullanarak](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
