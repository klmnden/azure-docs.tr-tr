---
title: PostgreSQL için Azure veritabanı performans önerileri
description: Bu makalede, PostgreSQL için Azure veritabanı'nda edinebilirsiniz performans önerilerini açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 6de302dbcfa9d6d1d2b311f41b03d8e54aeb63f6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395449"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı performans önerileri

**İçin geçerlidir:** 9.6 ve 10 PostgreSQL için Azure veritabanı

> [!IMPORTANT]
> Performans önerileri olduğu sınırlı sayıdaki bölgede genel önizlemede.

Performans önerileri özellik performansı için Azure veritabanı içinde PostgreSQL sunucusu oluşturulabilecek üst dizinleri tanımlar. Dizin önerileri üretmek için özelliği, şema ve sorgu Store tarafından bildirilen gibi iş yükü dahil olmak üzere, çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uyguladıktan sonra müşteriler bu değişikliklerin etkisini değerlendirmek için performans test etmeniz gerekir. 

## <a name="permissions"></a>İzinler
**Sahibi** veya **katkıda bulunan** performans önerileri özelliğini kullanarak analizi çalıştırmak için gereken izinler.

## <a name="performance-recommendations"></a>Performans önerileri
[Performans önerileri](concepts-performance-recommendations.md) özelliği iş yükleri arasında sunucunuzun performansını artırmak için olası ile dizinleri belirlemek için inceler.

Açık **performans önerileri** gelen **destek + sorun giderme** PostgreSQL sunucunuza Azure portal sayfasındaki menü çubuğunda bölümü.

![Giriş sayfası performans önerileri](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Seçin **Çözümle** ve bir veritabanı seçin. Bu analiz başlar. İş yükünüze bağlı olarak bu tamamlanması birkaç dakika sürebilir. Analiz tamamlandıktan sonra portalda bir bildirim olacaktır.

**Performans önerileri** penceresi bulunmazsa, tüm önerilerin bir listesi gösterilir. Bir öneri ilgili hakkında bilgi gösterir **veritabanı**, **tablo**, **sütun**, ve **dizin boyutu**.

![Performans önerileri yeni sayfa](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Öneriyi uygulamak için sorgu metni kopyalayın ve tercih ettiğiniz istemcinizden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [izleme ve ayarlama](concepts-monitoring.md) PostgreSQL için Azure veritabanı'nda.

