---
title: PostgreSQL için Azure veritabanı performans önerileri
description: Bu makalede, PostgreSQL için Azure veritabanı performans önerisi özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/26/2018
ms.openlocfilehash: d8edbc2847c06e95e658a1324f2e85f1758e60be
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487949"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı performans önerileri

**Şunlara uygulanır:** 9.6 ve 10 PostgreSQL için Azure veritabanı

Performans önerileri özellik performansı için Azure veritabanı içinde PostgreSQL sunucusu oluşturulabilecek üst dizinleri tanımlar. Dizin önerileri üretmek için özelliği, şema ve sorgu Store tarafından bildirilen gibi iş yükü dahil olmak üzere, çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uyguladıktan sonra müşteriler bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir. 

## <a name="permissions"></a>İzinler
Performans Önerileri özelliğini kullanarak analiz çalıştırmak için **Sahip** veya **Katkıda bulunan** izinleri gereklidir.

## <a name="performance-recommendations"></a>Performans önerileri
[Performans Önerileri](concepts-performance-recommendations.md) özelliği, performansı iyileştirme potansiyeli olan dizinleri tanımlamak için sunucunuzdaki iş yüklerini analiz eder.

PostgreSQL sunucunuzun Azure portalı sayfasındaki menü çubuğunun **Destek + sorun giderme** bölümünden **Performans Önerileri**’ni açın.

![Performans Önerileri giriş sayfası](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

**Analiz**’i seçin ve bir veritabanı belirtin. Bu işlem analizi başlatır. İş yükünüze bağlı olarak bu tamamlanması birkaç dakika sürebilir. Analiz tamamlanınca portalda bir bildirim olur.

Hiçbir öneri bulunamamışsa, **Performans Önerileri** penceresi bir öneri listesi gösterir. Bir öneri, ilgili **Veritabanı**, **Tablo**, **Sütun** ve **Dizin Boyutu** ile ilgili bilgileri gösterir.

![Performans önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Öneriyi uygulamak için, sorgu metnini kopyalayın ve seçtiğiniz istemciden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
- PostgreSQL için Azure Veritabanı’nda [izleme ve ayarlama](concepts-monitoring.md) hakkında daha fazla bilgi edinin.

