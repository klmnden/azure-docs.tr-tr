---
title: PostgreSQL için Azure veritabanı performans önerileri
description: Bu makalede, PostgreSQL için Azure veritabanı'nda edinebilirsiniz performans önerilerini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 1dedc08f27d1a483290dc61cce879290ca66592d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548111"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı performans önerileri

**Şunlara uygulanır:** 9.6 ve 10 PostgreSQL için Azure veritabanı

> [!IMPORTANT]
> Performans önerileri şu genel Önizleme aşamasındadır.

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

