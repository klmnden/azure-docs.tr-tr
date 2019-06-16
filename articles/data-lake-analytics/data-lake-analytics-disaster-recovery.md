---
title: Azure Data Lake Analytics için olağanüstü durum kurtarma Kılavuzu
description: Azure Data Lake Analytics hesaplarınızın olağanüstü durum kurtarma planlaması hakkında bilgi edinin.
services: data-lake-analytics
author: MikeRys
ms.author: mrys
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: ea1d4020aa9be23b4839690ae0b386d35bce8a23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66498897"
---
# <a name="disaster-recovery-guidance-for-azure-data-lake-analytics"></a>Azure Data Lake Analytics için olağanüstü durum kurtarma Kılavuzu

Azure Data Lake Analytics, büyük verileri kolaylaştıran, isteğe bağlı bir analiz işi hizmetidir. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. Bu makalede, nadir bölge çapında kesintilerden ya da yanlışlıkla silinmekten işlerinizi koruma hakkında yönergeler sağlanır.

## <a name="disaster-recovery-guidance"></a>Olağanüstü durum kurtarma kılavuzu

Azure Data Lake Analytics'i kullanarak, kendi olağanüstü durum kurtarma planını hazırlaması kritik. Bu makale, bir olağanüstü durum kurtarma planınızı oluşturmak için size yardımcı olur. Kendi planınızı oluşturmanıza yardımcı olabilecek ek kaynaklar verilmiştir:
- [Azure uygulamaları için hata ve olağanüstü durum kurtarma](/azure/architecture/reliability/disaster-recovery)
- [Azure dayanıklılık teknik kılavuzu](/azure/architecture/reliability)

## <a name="best-practices-and-scenario-guidance"></a>En iyi yöntemler ve senaryo Kılavuzu

Yinelenen bir U-SQL işi bir ADLA hesabı okuyan ve yapılandırılmamış verilerin yanı sıra, U-SQL tabloları yazan bir bölgede çalıştırabilirsiniz.  Bu adımları uygulayarak bir olağanüstü durum için hazırlayın:

1. İkincil bölgedeki bir kesinti sırasında kullanılacak ADLA ve ADLS hesapları oluşturun.

   > [!NOTE]
   > Hesabı adları benzersiz olduğundan, hangi hesabı ikincil gösteren tutarlı bir adlandırma düzeni kullanın.

2. Yapılandırılmamış veriler için başvuru [Azure Data Lake depolama Gen1 veriler için olağanüstü durum kurtarma Kılavuzu](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

3. ADLA tablolar ve veritabanlarında depolanan yapılandırılmış veriler için veritabanı, tablolar, tablo değerli işlevler ve derlemeler gibi meta veri yapılarını kopyalarını oluşturun. Üretim ortamında değişiklik meydana geldiğinde bu yapıları düzenli aralıklarla yeniden eşitlenmesi gerekir. Örneğin, yeni eklenen verileri ikincil bölgeye veri kopyalama ve ikincil tablosuna ekleme çoğaltılması gerekir.

   > [!NOTE]
   > Bu nesne adlarını ikincil hesabına kapsamlı ve birincil üretim hesabı olduğu gibi aynı ada sahip oldukları için genel olarak benzersiz değil.

Kesinti sırasında betiklerinizi giriş yollarından ikincil uç noktaya işaret edecek şekilde güncelleştirmeniz gerekir. Ardından kullanıcıların işlerini ikincil bölgedeki ADLA hesabına gönderin. İş çıktısı, sonra ikincil bölgedeki ADLA ve ADLS hesabına yazılır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Lake depolama Gen1 veriler için olağanüstü durum kurtarma Kılavuzu](../data-lake-store/data-lake-store-disaster-recovery-guidance.md)
