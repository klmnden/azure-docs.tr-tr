---
title: Azure İzleyici'de gönüllü bir geçiş aracını kullanarak Klasik uyarılarınızı geçirme
description: Uyarı kurallarınızı Klasik geçirmek için gönüllü bir Geçiş Aracı'nı kullanmayı öğrenin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 58c664beee942fe7115c7fff38a039c23bed6ac3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60346068"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Uyarı kurallarınızı Klasik geçirmek için gönüllü bir geçiş aracını kullanma

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Temmuz 2019 ' kullanımdan. Geçiş Aracı, geçiş gönüllü olarak tetiklemek için Azure portalında kullanılabilir ve klasik uyarı kuralları kullanan müşteriler için kullanıma sunuluyor. Bu makalede, Temmuz 2019 ' otomatik geçiş başlamadan önce uyarı kurallarınızı Klasik gönüllü olarak geçirmek için geçiş aracını kullanma konusunda size yol gösterir.

## <a name="benefits-of-new-alerts"></a>Yeni uyarılar avantajları

Klasik uyarılar, yeni birleştirilmiş Azure İzleyicisi'nde uyarı tarafından değiştirilmektedir. Yeni uyarılar platformuna aşağıdaki faydaları sağlar:

- Çok boyutlu ölçümler için çeşitli uyarı [pek çok daha fazla Azure hizmeti](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)
- Yeni ölçüm uyarıları Destek [çok kaynak uyarı kuralları](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) , büyük ölçüde azaltmak çok kurallarını yönetme yükü.
- Birleşik bildirim mekanizması
  - [Eylem grupları](action-groups.md) tüm yeni uyarı türleri ile (ölçümü, günlük ve etkinlik günlüğüne) çalışan bir modüler bildirim mekanizması
  - Ayrıca SMS, sesli ve ITSM Bağlayıcısı gibi yeni bildirim mekanizması yararlanmak mümkün olacaktır
- [Uyarı deneyimi birleşik](alerts-overview.md) tüm uyarılar üzerinde farklı sinyale getirir (ölçüm, Etkinlik günlüğünü ve günlük) tek bir yerde içine

## <a name="before-you-migrate"></a>Geçiş yapmadan önce

Geçişin bir parçası olarak Klasik uyarı kuralları eşdeğer Yeni Uyarı kurallarına dönüştürülür ve Eylem grupları oluşturulur.

- Bildirim yükü biçimi ve bunun yanı sıra oluşturmak ve yeni uyarı kuralları yönetmek için API'ler Klasik uyarı kuralları olanlardan farklı daha fazla özellik destekledikleri gibi. Bilgi [geçişe hazırlamak nasıl](alerts-prepare-migration.md).

- Bazı Klasik uyarı kuralları aracı kullanılarak geçirilemez. [Hangi kuralları geçirilemeyebilir değildir ve geçmeleri onlarla nasıl bilgi](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > Geçiş işlemi, uyarı kurallarınızı Klasik değerlendirmesi etkilemez. Bunlar, çalıştırın ve yeni uyarı kuralları değerlendirme işlemini başlatır ve bu ayarların geçişi yapılır kadar uyarıları göndermek devam eder.


## <a name="how-to-use-the-migration-tool"></a>Geçiş aracını kullanma

Aşağıdaki yordamda, Azure portalında Klasik uyarı kurallarınızı geçişini tetiklemek açıklanmaktadır:

1. İçinde [Azure portalında](https://portal.azure.com), tıklayarak **İzleyici**.

2. Tıklayın **uyarılar** ardından **uyarı kurallarını yönet** veya **Klasik uyarıları görüntüleyip**.

3. Tıklayın **yeni kurallar geçiş** geçiş giriş sayfasına gidin. Bu sayfa, tüm aboneliklerinizi ve bunlar için geçiş durumu listesini gösterir.

    ![geçiş giriş](media/alerts-migration/migration-landing.png "geçirme kuralları")

4. Aracı'nı kullanarak geçirilebilir tüm abonelikleri olarak işaretlenmiş **geçirmeye hazır**.

    > [!NOTE]
    > Geçiş Aracı aşamada Klasik uyarı kuralları kullanan tüm abonelikleri için sunulacak. Sunum erken aşamalarında bazı abonelik geçiş için hazır değil olarak görebilirsiniz.

5. Bir veya daha fazla abonelik seçin ve tıklayın **geçiş önizlemesi**

6. Bu sayfada, aynı anda bir abonelik için geçirilecek Klasik uyarı kuralları ayrıntılarını görebilirsiniz. Ayrıca **Bu abonelik için geçiş ayrıntılarını indir** .csv biçiminde.

    ![geçiş önizlemesi](media/alerts-migration/migration-preview.png "geçiş önizlemesi")

7. Bir veya daha fazla sağlamak **e-posta adreslerini** geçiş durumunu almak. Geçiş tamamlandığında veya bir eylem gerçekleştirmeniz gerekmiyor olduğunda size bir e-posta göndereceğiz.

8. Tıklayarak **başlangıç geçiş**. Onay iletişim kutusunda gösterilen bilgileri okuyun ve geçiş işlemini başlatmak hazır olup olmadığını onaylayın.

    >[!IMPORTANT]
    > Bir abonelik için geçiş işlemi başlatma sonra klasik bir uyarı kuralları abonelik için düzenleme/oluşturma mümkün olmayacaktır. Ancak, Klasik uyarı kurallarınızı çalıştıran ve üzerinde geçirildiğini kadar uyarılar sağlayan devam eder. Klasik bir uyarı kuralları ve geçiş sırasında oluşturulan yeni kurallar arasında güvenilirlik sağlamak için budur. Aboneliğiniz için geçiş işlemi tamamlandıktan sonra klasik bir uyarı kuralları artık kullanılamıyor.

    ![Geçişi onaylamak](media/alerts-migration/migration-confirm.png "Onayla geçişi Başlat")

9. Olarak geçişi Tamamla veya bir işlem yapmanıza gerek, adım 8'deki sağlanan e-posta adreslerinde e-posta alırsınız. Durum portalındaki geçiş giriş sayfasından da düzenli aralıklarla denetleyebilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-my-subscriptions-listed-as-not-ready-for-migration"></a>**Neden benim abonelikleri listelenir değil geçiş için hazır?**

Geçiş Aracı tüm müşterilerine aşamalı olarak sunuluyor. Erken aşama, çoğu veya tüm aboneliklerinizi olarak işaretlenir **geçiş için hazır değil**. Ancak, tüm abonelikleri mid-Nisan tarafından geçirmeye hazır olmalıdır.

Abonelik geçiş için hazır olduğunda, abonelik sahipleri aracı kullanılabilirliğini bildiren bir e-posta alırsınız. Bu bildirim için takip.

### <a name="who-can-trigger-the-migration"></a>**Kimin geçiş tetiklenebilir mi?**

Abonelik düzeyinde atanmış izleme katkıda bulunan rolüne sahip kullanıcılar geçiş tetiklemeniz mümkün olacaktır. Daha fazla bilgi edinin [geçiş işlemi için rol tabanlı erişim denetimi](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-is-the-migration-going-to-take"></a>**Ne kadar Geçiş yapılacak geçiyor?**

Çoğu abonelikler için geçişi genellikle bir saat altında tamamlar. Geçişin ilerleme durumunu geçiş giriş sayfasından takip.  Bu sırada, lütfen Klasik uyarılar sistemde veya yeni bir tane uyarılarınızı hala çalıştığını güvence altına alınabilir.

### <a name="what-can-i-do-if-i-run-into-an-issue-during-migration"></a>**Bir sorunla geçiş sırasında çalıştırırsanız ne yapabilirim?**

Lütfen izleyin [sorun giderme kılavuzu yüz, geçiş sırasında herhangi bir sorun için düzeltme adımları görmek için](alerts-understand-migration.md#common-issues-and-remediations). Herhangi bir eylem, geçişi tamamlamak için gerekliyse, geçiş sırasında sağlanan e-posta adresleri üzerinde bildirim alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş Aracı nasıl çalıştığını anlamak](alerts-understand-migration.md)
