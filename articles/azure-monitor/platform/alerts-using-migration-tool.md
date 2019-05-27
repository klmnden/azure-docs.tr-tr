---
title: Azure İzleyici'de klasik uyarıları gönüllü bir geçiş aracını kullanarak geçirme
description: Uyarı kurallarınızı Klasik geçirmek için gönüllü bir Geçiş Aracı'nı kullanmayı öğrenin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 00229cca1d7fb238b330ec98cd35d0bb59bc821a
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015633"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Uyarı kurallarınızı Klasik geçirmek için gönüllü bir geçiş aracını kullanma

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Eylül 2019 ' kullanımdan (başlangıçta Temmuz 2019 oluştu). Geçiş Aracı, Azure portalında Klasik uyarı kuralları kullanan ve kendilerini geçişini isteyen müşteriler için kullanılabilir. Bu makalede, Eylül 2019 ' otomatik geçiş başlamadan önce uyarı kurallarınızı Klasik gönüllü olarak geçirmek için geçiş aracı kullanmayı açıklar.

> [!NOTE]
> Sunum geçiş aracının gecikme nedeniyle, o tarihten Klasik uyarılar geçiş yapıldı [31 Ağustos 2019'için Genişletilmiş](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) ilk duyurulan tarihinden 30 Haziran 2019.

## <a name="benefits-of-new-alerts"></a>Yeni uyarılar avantajları

Klasik uyarılar, Azure İzleyici'de yeni ve birleştirilmiş uyarı tarafından değiştirilmektedir. Yeni uyarılar platformuna aşağıdaki faydaları sağlar:

- Çok boyutlu ölçümler için çeşitli uyarabilir [pek çok daha fazla Azure hizmeti](alerts-metric-near-real-time.md#metrics-and-dimensions-supported).
- Yeni ölçüm uyarıları Destek [çok kaynak uyarı kuralları](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) birçok kurallarını yönetme ek yükünü önemli ölçüde azaltabilir.
- Birleşik bildirim mekanizması destekler:
  - [Eylem grupları](action-groups.md), tüm yeni uyarı türleri ile (ölçümü, günlük ve etkinlik günlüğü) çalışan modüler bildirim mekanizması.
  - Yeni bildirim mekanizmaları, SMS, sesli ve ITSM Bağlayıcısı gibi.
- [Uyarı deneyimi birleşik](alerts-overview.md) tüm uyarıları farklı sinyaller (ölçümü, günlük ve etkinlik günlüğü) bir araya getiriyor.

## <a name="before-you-migrate"></a>Geçiş yapmadan önce

Geçiş işlemi, yeni, eşdeğer uyarı kuralları için Klasik uyarı kuralları dönüştürür ve Eylem grupları oluşturur. Aşağıdaki noktalara dikkat etmeniz hazırlık:

- Daha fazla özellik destekledikleri için hem bildirim yükü biçimi ve API'ler oluşturmak ve yeni uyarı kuralları yönetmek için Klasik uyarı kuralları olanlardan farklıdır. [Geçiş için hazırlama öğrenin](alerts-prepare-migration.md).

- Bazı Klasik uyarı kuralları aracı kullanılarak geçirilemez. [Hangi kuralları geçirilemez ve bunlarla yapmanız gerekenler bilgi](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > Geçiş işlemi, uyarı kurallarınızı Klasik değerlendirmesi etkilemez. Bunlar, çalıştırmak ve geçiş ve yeni uyarı kuralları etkili kadar uyarıları göndermek devam edeceğiz.

## <a name="how-to-use-the-migration-tool"></a>Geçiş aracını kullanma

Azure portalında Klasik uyarı kurallarınızı geçişini tetiklemek için bu adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com)seçin **İzleyici**.

1. Seçin **uyarılar**ve ardından **uyarı kurallarını yönet** veya **Klasik uyarıları görüntüleyip**.

1. Seçin **yeni kurallar geçiş** geçiş giriş sayfasına gidin. Bu sayfa, tüm aboneliklerinizi ve geçiş durumlarını gösterir:

    ![geçiş giriş](media/alerts-migration/migration-landing.png "geçirme kuralları")

    Aracı'nı kullanarak geçirilebilir tüm abonelikleri olarak işaretlenmiş **geçirmeye hazır**.

    > [!NOTE]
    > Geçiş Aracı aşamada Klasik uyarı kuralları kullanan tüm abonelikleri için sunulacak. Dağıtımının erken aşamalarında, geçiş için hazır olarak işaretlenmiş bazı abonelikler görebilirsiniz.

1. Bir veya daha fazla abonelik seçin ve ardından **Önizleme geçiş**.

    Sonuçta elde edilen sayfanın aynı anda bir abonelik için geçirilecek Klasik uyarı kuralları ayrıntılarını gösterir. Belirleyebilirsiniz **Bu abonelik için geçiş ayrıntılarını indir** ayrıntılarını bir CSV biçiminde alın.

    ![geçiş önizlemesi](media/alerts-migration/migration-preview.png "geçiş önizlemesi")

1. Geçiş durumu bildirim almak için bir veya daha fazla e-posta adreslerini belirtin. Geçiş tamamlandığında veya herhangi bir işlem yapmanız gerekirse, e-posta alırsınız.

1. Seçin **başlangıç geçiş**. Onay iletişim kutusunda gösterilen bilgileri okuyun ve geçiş işlemini başlatmaya hazır olduğunuzu doğrulayın.

    > [!IMPORTANT]
    > Geçiş için bir abonelik başlattıktan sonra düzenlemek veya bu abonelik için Klasik uyarı kuralları oluşturmak mümkün olmayacaktır. Bu kısıtlama uyarı kurallarınızı Klasik değişiklik yapmadan yeni kurallar geçiş sırasında kayıp olmasını sağlar. Uyarı kurallarınızı Klasik değiştirmesi mümkün olmayacaktır olsa da, bunlar çalıştırın ve Project.json'dan packagereference'a kadar uyarılar sağlamak için yine de devam edeceğiz. Aboneliğiniz için geçiş işlemi tamamlandıktan sonra klasik bir uyarı kuralları artık kullanamazsınız.

    ![Geçişi onaylamak](media/alerts-migration/migration-confirm.png "Onayla geçişi Başlat")

1. Geçiş tamamlandığında veya eylem gerçekleştirmenize gerek yoktur, daha önce sağlanan adreslerde bir e-posta alacaksınız. Durum geçiş giriş sayfası Portalı'nda da düzenli aralıklarla denetleyebilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Neden geçiş için hazır değil olarak Aboneliğimi listelenir?

Geçiş Aracı müşterilerine aşamalı olarak kullanıma sunulmaktadır. Erken aşamalarında olarak çoğu veya tüm aboneliklerinizi işaretlenebilir **geçiş için hazır değil**. 

Abonelik geçiş için hazır olduğunda, abonelik sahibi aracı kullanılabilir olduğunu belirten bir e-posta iletisi alırsınız. Bu ileti için takip.

### <a name="who-can-trigger-the-migration"></a>Kimin geçiş tetiklenebilir mi?

Abonelik düzeyinde atanmış izleme katkıda bulunan rolüne sahip kullanıcılar geçişini olanağına sahip olursunuz. [Geçiş işlemi için rol tabanlı erişim denetimi hakkında daha fazla bilgi](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>Geçiş ne kadar sürer?

Geçiş için çoğu aboneliklerinde bir saatten kısa tamamlanır. Geçiş giriş sayfasında geçişin ilerleme durumunu takip. Geçiş sırasında uyarılarınızı Klasik uyarılar sistemde veya yeni bir tane hala çalışıyor yararlandığından emin.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Bir sorunla karşılaşırsanız geçiş sırasında çalıştırırsanız ne yapabilirim?

Bkz: [sorun giderme kılavuzu](alerts-understand-migration.md#common-problems-and-remedies) yüz, geçiş sırasında sorunlarla ilgili Yardım için. Herhangi bir eylem, geçişi tamamlamak için gerekliyse, Aracı'nı sağladığınız e-posta adreslerindeki bildirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş Aracı nasıl çalıştığını anlamak](alerts-understand-migration.md)
