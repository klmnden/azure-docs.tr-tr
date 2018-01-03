---
title: "Microsoft Azure ve Azure İzleyicisi'nde uyarılar genel bakış | Microsoft Docs"
description: "Uyarılar, Azure kaynak ölçümleri, olayları ve günlükleri izlemek ve belirttiğiniz bir koşul karşılandığında bildirilmesini sağlar."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c1f0182f27cfb8441a09abd2031b365a4ab4315a
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="what-are-alerts-in-microsoft-azure"></a>Microsoft Azure içindeki uyarıları nedir?
Bu makalede, Microsoft Azure, nelerdir uyarı çeşitli kaynaklardan bu uyarıları, faydaları ve bunları kullanmaya başlamak nasıl amacıyla açıklanır. Özellikle Azure İzleyicisi uygulanır ancak diğer hizmetlerin işaretçiler de uyarılar sağlar. Uyarıları verilerinde koşulları yapılandırın ve son izleme verilerini koşullara uyan bildirim hale olanak tanıyan Azure'da izleme, bir yöntem sunar.


## <a name="taxonomy-of-azure-alerts"></a>Azure uyarıları sınıflandırma
Azure uyarıları ve bunların işlevlerini açıklamak için aşağıdaki koşulları'nı kullanır:
* **Uyarı** -sağlandığında etkin hale gelir ölçüt (bir veya daha fazla kural ya da koşullar) tanımı.
* **Etkin** -bir uyarı tarafından tanımlanan ölçütler karşılandığında durumu.
* **Çözümlenen** -bir uyarı tarafından tanımlanan ölçütler artık karşılandığında önceden karşılanıp sonra durumu.
* **Bildirim** - etkin hale uyarı dışına tabanlı gerçekleştirilecek eylem.
* **Eylem** -bir alıcı (örneğin, bir adresi e-postayla gönderme veya bir Web kancası URL'si nakil) bir bildirim gönderilmesini belirli bir çağrı. Bildirimleri genellikle birden çok eylem tetikleyebilir.

    > [!NOTE]
    > Azure uyarıları evrimi bir parçası olarak, yeni bir birleşik deneyim Önizleme'de kullanılabilir. Yeni uyarılar (Önizleme) deneyimi farklı bir sınıflandırma kullanır. Daha fazla bilgi edinmek [uyarıları (Önizleme)](monitoring-overview-unified-alerts.md). 
    >

## <a name="alerts-in-different-azure-services"></a>Farklı Azure Hizmetleri uyarıları
Uyarıları hizmetlerini izleme birkaç Azure kullanılabilir. Hakkında bilgi edinmek ve bu hizmetleri kullanıldığı durumlar için [bu makaleye bakın](./monitoring-overview.md). Azure üzerinde kullanılabilir uyarı türlerini dökümünü şöyledir:


| Hizmet | Uyarı türü | Desteklenen hizmetler | Açıklama |
|---|---|---|---|
| Azure İzleyici | [Ölçüm uyarıları](./insights-alerts-portal.md) | [Azure İzleyicisi'nden desteklenen ölçümleri](./monitoring-supported-metrics.md) | Herhangi bir platform düzeyi ölçümü belirli bir koşulun karşıladığında bir bildirim alın (örneğin, bir VM CPU % son 5 dakika boyunca 90'dan büyük olduğu). |
|Azure İzleyici | [Yakın gerçek zamanlı ölçüm uyarıları (Önizleme)](./monitoring-near-real-time-metric-alerts.md)| [Desteklenen kaynaklardan Azure İzleyicisi](./monitoring-near-real-time-metric-alerts.md#what-resources-can-i-create-near-real-time-metric-alerts-for) | Bir veya daha fazla platform düzeyi ölçümlerini belirtilen koşulları karşıladığında ölçüm uyarıları daha hızlı bildirimi (örneğin, bir VM CPU % 90'dan büyük ve ağ içinde 500 MB'tan fazla son 5 dakika olarak). |
| Azure İzleyici | [Etkinlik günlüğü uyarıları](./monitoring-activity-log-alerts.md) | Azure Kaynak Yöneticisi'nde kullanılabilen tüm kaynak türleri | Bildirim herhangi seçildiğinde yeni olayda [Azure etkinlik günlüğü](./monitoring-overview-activity-logs.md) eşleşen belirli koşullar (örneğin, "VM silme" işlemi içinde myProductionResourceGroup meydana geldiğinde veya "Etkin" olarak durumu ile yeni bir hizmet durumu olay görüntülendiğinde). |
| Application Insights | [Ölçüm uyarıları](../application-insights/app-insights-alerts.md) | Application Insights'a veri göndermek için izleme eklenmiş herhangi bir uygulama | Herhangi bir uygulama düzeyi ölçümü belirli bir koşulun karşıladığında bir bildirim alın (örneğin, sunucu yanıt süresi 2 saniyeden uzun). |
| Application Insights | [Web testi uyarıları](../application-insights/app-insights-monitor-web-app-availability.md) | Application Insights'a veri göndermek için izleme eklenmiş tüm Web sitesi | Kullanılabilirlik ve yanıt hızını bir Web sitesinin beklentilerini altında olduğunda bir bildirim alırsınız. |
| Log Analytics | [Günlük analizi uyarıları](../log-analytics/log-analytics-alerts.md) | Günlük analizi veri göndermek için yapılandırılmış herhangi bir hizmeti | Günlük analizi arama sorgusu ölçüm ve/veya olay veriler üzerinde belirli ölçütleri karşıladığında bir bildirim alırsınız. |

## <a name="alerts-on-azure-monitor-data"></a>Azure İzleyicisi veri uyarılar
Azure izleyiciden--yakın gerçek zamanlı ölçüm uyarıları (Önizleme) ölçüm uyarıları ve etkinlik günlüğü uyarıları üç türde bir veri dışına uyarı yok.

* **Ölçüm uyarıları** -belirtilen bir ölçüm değerini atadığınız bir eşik kestiği bu uyarı tetikler. Uyarı, uyarının "(eşiği aşıldığında ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, "(eşiği yeniden çapraz ve koşulu artık karşılanmıyor olduğunda) çözümlendiğinde" bir bildirim oluşturur. Azure İzleyici tarafından desteklenen kullanılabilir ölçümler büyüyen bir listesi için bkz [Azure monitörde desteklenen ölçüm listesine](monitoring-supported-metrics.md).
* **Yakın gerçek zamanlı ölçüm uyarıları (Önizleme)** - Bu uyarılar için ölçüm uyarıları benzer, ancak bazı farklar. Adı da anlaşılacağı gibi ilk olarak, bu uyarılar de yakın gerçek zamanlı (kadar hızlı 1 dak) tetikleyebilir. Ayrıca birden çok (şu anda iki) ölçümleri izleme destekler.  Uyarı uyarı "(her ölçümü için eşikler aynı anda taşları ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, "çözümlendiğinde" bir bildirim oluşturur (zaman en az bir ölçüm kestiği eşiği yeniden ve koşul yok artık karşılanmış).

    > [!NOTE]
    > Gerçek zamanlı ölçüm uyarılar şu anda genel önizlemede. İşlevsellik ve kullanıcı deneyimi değiştirilebilir ' dir.
    >
    >

* **Etkinlik günlüğü uyarıları** -filtre atadığınız ölçütleri eşleşen bir etkinlik günlüğü olay oluşturulduğunda tetikleyen bir akış günlük uyarı. Bu uyarılar yalnızca bir duruma sahip "uyarı altyapısı, filtre ölçütünü yalnızca herhangi bir yeni olay uygulanır. bu yana, etkinleştirildi". Bu uyarılar yeni bir hizmet durumu olay gerçekleştiğinde veya bir kullanıcı veya uygulama "sanal makineyi silin.", aboneliğinizde, örneğin, bir işlem gerçekleştirdiğinde bildirim hale için kullanılabilir

Azure İzleyicisi aracılığıyla kullanılabilir tanılama günlük verilerini için günlük analizi veri Yönlendirme ve günlük analizi uyarıyı kullanarak öneririz. Aşağıdaki diyagramda Azure İzleyici ve, kavramsal olarak, bu verileri dışına nasıl uyarı veri kaynakları özetler.

![Açıklanan uyarıları](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>Bir Azure İzleyicisi uyarı bildirim nasıl alıyor musunuz?
Tarihsel olarak, farklı hizmetler Azure uyarılardan kendi yerleşik bildirim yöntemleri kullanılır. İleride, Azure İzleyici Eylem grupları adı verilen bir yeniden kullanılabilir bildirim gruplandırma sunar. --Tüm e-posta adresi sayısı, (SMS için) telefon numaralarını veya Web kancası URL'leri--bildirim alıcıları bir dizi eylem gruplarını belirtin ve eylem grubuna başvuruda dilediğiniz zaman bir uyarı etkinleştirildikten tüm alıcılar bu bildirim alırsınız. Bu, alıcılar (örneğin, üzerinde çağrısı mühendislik listenize) gruplandırması arasında çok sayıda uyarı nesneleri yeniden kullanmanıza olanak sağlar. Şu anda yalnızca etkinlik günlüğü uyarıları eylem gruplarını kullanın hale getirir, ancak diğer Azure birkaç uyarı türleri de Eylem grupları kullanarak çalışma.

Eylem grupları bildirim e-posta adreslerini ve SMS numaraları ek olarak bir Web kancası URL'si göndererek destekler. Bu otomasyon ve düzeltme, örneğin, kullanarak sağlar:
    - Azure Otomasyonu Runbook
    - Azure işlevi
    - Azure mantıksal uygulama
    - bir üçüncü taraf hizmeti

Gerçek zamanlı ölçümü uyarıları (Önizleme) ve etkinlik günlüğü uyarıları eylem gruplarını kullanabilirsiniz.

Ölçüm uyarıları Eylem grupları henüz kullanmayın. Tek bir ölçüm uyarıdaki bildirimleri yapılandırabilirsiniz:
* Hizmet Yöneticisi, ortak Yöneticiler veya belirttiğiniz ek e-posta adresleri e-posta bildirimleri gönderin.
* Ek Otomasyon eylemleri başlatmak sağlayan bir Web kancası çağırın.

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları ve bunları kullanarak yapılandırma hakkında bilgi alın:

* Daha fazla bilgi edinmek [ölçümleri](monitoring-overview-metrics.md)
* Yapılandırma [Azure Portalı aracılığıyla ölçüm uyarıları](insights-alerts-portal.md)
* Yapılandırma [ölçüm PowerShell uyarıları](insights-alerts-powershell.md)
* Yapılandırma [ölçüm uyarıları komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* Yapılandırma [ölçüm uyarıları Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Daha fazla bilgi edinmek [etkinlik günlüğü](monitoring-overview-activity-logs.md)
* Yapılandırma [Azure Portalı aracılığıyla etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları aracılığıyla kaynak yöneticisi](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
* Daha fazla bilgi edinmek [yakın gerçek zamanlı ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)
* Daha fazla bilgi edinmek [hizmet bildirimleri](monitoring-service-notifications.md)
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)
* Yapılandırma [uyarıları (Önizleme) uyarıları](monitor-alerts-unified-usage.md)
