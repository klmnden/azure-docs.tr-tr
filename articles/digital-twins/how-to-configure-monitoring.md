---
title: Azure dijital İkizlerini izlemesini yapılandırma | Microsoft Docs
description: Azure dijital İkizlerini izlemeyi yapılandırma
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 5dc2587a0c127106d5afb41e20eca43919065f1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67118777"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Azure dijital İkizlerini izlemeyi yapılandırma

Azure dijital İkizlerini güçlü günlük kayıtları, izleme ve analiz destekler. Çözümleri geliştiriciler, Azure İzleyici günlükleri, tanılama günlükleri, etkinlik günlüğü ve diğer hizmetleri, IOT uygulama karmaşık izleme gereksinimlerini desteklemek için kullanabilirsiniz. Sorgu veya çeşitli hizmetlerdeki kayıtları görüntülemek ve pek çok hizmeti için ayrıntılı günlük kaydı kapsamı sağlamak için günlük kaydı seçeneklerine birleştirilebilir.

Bu makalede, günlüğe kaydetme ve izleme seçenekleri ve bunları Azure dijital İkizlerini belirli şekillerde birleştirmek nasıl özetlenir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Etkinlik günlüklerini gözden geçirin

Azure [etkinlik günlüklerini](../azure-monitor/platform/activity-logs-overview.md) her bir Azure hizmet örneği için abonelik düzeyinde olay ve işlem geçmişi hızlı Öngörüler sağlayın.

Abonelik düzeyindeki olayların şunlardır:

* Kaynak oluşturma
* Kaynak temizleme
* Uygulama parolaları oluşturma
* Diğer hizmetlerle tümleştirme

Azure dijital çiftleri için etkinlik günlüğü varsayılan olarak etkindir ve tarafından Azure portalından bulunabilir:

1. Azure dijital İkizlerini örneğinizi seçin.
1. Seçme **etkinlik günlüğü** görüntüleme paneli getirmek için:

    [![Etkinlik günlüğü](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Gelişmiş etkinlik günlüğü:

1. Seçin **günlükleri** görüntülemek için seçeneği **etkinlik Log Analytics genel bakışı**:

    [![Seçimi](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. **Etkinlik Log Analytics genel bakışı** temel etkinlik günlüğü verilerini özetlemenin:

    [![Etkinlik log analytics'e genel bakış]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

>[!TIP]
>Kullanım **etkinlik günlüklerini** abonelik düzeyindeki olayların hızlı Öngörüler için.

## <a name="enable-customer-diagnostic-logs"></a>Müşteri tanılama günlüklerini etkinleştirme

Azure [tanılama ayarları](../azure-monitor/platform/diagnostic-logs-overview.md) etkinlik günlüğü tamamlamak Azure her örneği için ayarlanabilir. Etkinlik günlükleri için abonelik düzeyindeki olayların ilgilidir, ancak Tanılama Günlüğü kaynaklardaki işletimsel geçmişini Öngörüler sağlar.

Tanılama günlüğüne kaydetme örnekleri şunlardır:

* Kullanıcı tanımlı bir işlev yürütme süresi
* Başarılı bir API isteğinin yanıtı durum kodu
* Bir uygulama anahtarı bir kasadan alınıyor

Bir örneği için tanılama günlüklerini etkinleştirmek için:

1. Azure portalında kaynak getirecek.
1. Seçin **tanılama ayarları**:

    [![Bir tanılama ayarları](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Seçin **tanılamayı Aç** (önceden etkinleştirilmemişse) verileri toplamak için.
1. İstenen alanları doldurun ve nasıl ve verilerin kaydedileceği seçin:

    [![Tanılama ayarları iki](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Tanılama günlükleri, kullanılarak genellikle kaydedilir [Azure dosya depolama](../storage/files/storage-files-deployment-guide.md) ve ile paylaşılan [Azure İzleyicisi](../azure-monitor/log-query/get-started-portal.md). İki seçenek de seçilebilir.

>[!TIP]
>Kullanım **tanılama günlükleri** kaynak işlemleri hakkında Öngörüler için.

## <a name="azure-monitor-and-log-analytics"></a>Azure İzleyici ve log analytics

IOT uygulamaları birbirinden farklı kaynakları, cihazlar, konumlar ve veri birine birleştiren. Ayrıntılı günlüğü her belirli parça, hizmet veya bileşen, genel uygulama mimarisi hakkında ayrıntılı bilgi sağlar ancak birleşik bir genel bakış, sık sık Bakım ve hata ayıklama için gereklidir.

Azure İzleyici görüntülenebilir ve tek bir yerde analiz için kaynakları günlüğü sağlayan güçlü log analytics hizmetine içerir. Azure İzleyici, bu nedenle Gelişmiş IOT uygulamaları içinde günlüklerini analiz için çok yararlı olur.

Kullanım örnekleri şunlardır:

* Birden çok tanılama günlük geçmişi sorgulanıyor
* Birkaç kullanıcı tanımlı işlevler için günlükleri görme
* İçinde belirli bir zaman çerçevesinde iki veya daha fazla hizmet günlüklerini görüntüleme

Tam günlüğü sorgulama aracılığıyla sağlanır [Azure İzleyicisi](../azure-monitor/log-query/log-query-overview.md). Bu güçlü özellikleri ayarlamak için:

1. Arama **Log Analytics** Azure portalında.
1. Kullanılabilir gördüğünüz **Log Analytics çalışma alanı** örnekleri. Bu seçeneklerden birini belirleyip seçin **günlükleri** sorgu için:

    [![Log analytics](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Henüz yoksa bir **Log Analytics çalışma alanı** örneği oluşturmak için kullanabileceğiniz bir çalışma alanı seçerek **Ekle** düğmesi:

    [![OMS oluşturma](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Bir kez, **Log Analytics çalışma alanı** girişleri katları günlükleri bulmak ya da kullanarak belirli ölçütleri kullanarak aramak için güçlü sorgular kullanabilir, örnek hazırlanır **günlük yönetimi**:

   [![Günlük yönetimi](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Güçlü sorgu işlemleri hakkında daha fazla bilgi için bkz: [sorguları ile çalışmaya başlama](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> 5 dakikalık bir gecikmeyle olayları gönderirken karşılaşabilirsiniz **Log Analytics çalışma alanı** ilk kez.

Azure İzleyici günlüklerine da sağlayan güçlü hata ve uyarı Bildirim Hizmetleri, seçilerek görüntülenebilir **tanılayın ve sorunlarını çözmek**:

   [![Uyarı ve hata bildirimleri](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Kullanım **Log Analytics çalışma alanı** sorgu günlük geçmişlerini birden çok uygulama İşlevler, abonelikler veya hizmetler için.

## <a name="other-options"></a>Diğer Seçenekler

Azure dijital İkizlerini, uygulamaya özgü günlük kaydı ve güvenlik denetimi de destekler. Azure dijital İkizlerini Örneğinize kullanılabilir olan tüm Azure günlük seçenekleri kapsamlı bir genel bakış için bkz [Azure günlük denetim](../security/azure-log-audit.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

- Azure hakkında daha fazla bilgi [etkinlik günlüklerini](../azure-monitor/platform/activity-logs-overview.md).

- Okuyarak ayrıntılar daha kapsamlı Azure tanılama ayarları bir [tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md).

- Daha fazla bilgi edinin [Azure İzleyicisi](../azure-monitor/log-query/get-started-portal.md).
