---
title: Microsoft Azure ve Azure İzleyicisi'nde Klasik uyarılar genel bakış | Microsoft Docs
description: Uyarılar, Azure kaynak ölçümleri, olayları ve günlükleri izlemek ve belirttiğiniz bir koşul karşılandığında bildirilmesini sağlar.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: robb
ms.openlocfilehash: 06ba05f71cf1f696033099c04448526a0421ad42
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft Azure Klasik uyarılar nelerdir?

> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni neredeyse gerçek zamanlı ölçüm uyarıları](monitoring-overview-unified-alerts.md)
>

Bu makalede, Microsoft Azure, nelerdir uyarı çeşitli kaynaklardan bu uyarıları, faydaları ve bunları kullanmaya başlamak nasıl amacıyla açıklanır. Özellikle, Klasik Azure izleme uyarıları için geçerlidir. Uyarıları verilerinde koşulları yapılandırın ve son izleme verilerini koşullara uyan bildirim hale olanak tanıyan Azure'da izleme, bir yöntem sunar.


## <a name="taxonomy-of-azure-monitor-classic-alerts"></a>Sınıflandırma Azure İzleyici Klasik uyarıları
Azure Klasik uyarıları ve bunların işlevlerini açıklamak için aşağıdaki koşulları'nı kullanır:
* **Uyarı** -sağlandığında etkin hale gelir ölçüt (bir veya daha fazla kural ya da koşullar) tanımı.
* **Etkin** -klasik bir uyarı tarafından tanımlanan ölçütler karşılandığında durumu.
* **Çözümlenen** -klasik bir uyarı tarafından tanımlanan ölçütler artık karşılandığında önceden karşılanıp sonra durumu.
* **Bildirim** - etkin hale kapatmayı Klasik uyarı tabanlı gerçekleştirilecek eylem.
* **Eylem** -bir alıcı (örneğin, bir adresi e-postayla gönderme veya bir Web kancası URL'si nakil) bir bildirim gönderilmesini belirli bir çağrı. Bildirimleri genellikle birden çok eylem tetikleyebilir.


## <a name="alerts-on-azure-monitor-data"></a>Azure İzleyicisi veri uyarılar
Azure monitör--yakın gerçek zamanlı ölçüm uyarıları ve etkinlik günlüğü uyarılar ölçüm uyarıları kullanılabilir üç türde bir uyarı verilerini temizleyebilirsiniz.

* **Klasik ölçüm uyarıları** -belirtilen bir ölçüm değerini atadığınız bir eşik kestiği bu uyarı tetikler. Uyarı, uyarının "(eşiği aşıldığında ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, "(eşiği yeniden çapraz ve koşulu artık karşılanmıyor olduğunda) çözümlendiğinde" bir bildirim oluşturur. Bu eski ölçüm uyarılar. Ölçüm yeni uyarılar için aşağıya bakın.

* **Yakın gerçek zamanlı ölçüm uyarıları** (yeni uyarı deneyimi) - bunlar önceki ölçüm uyarılar karşılaştırıldığında geliştirilmiş özellikleri ölçüm uyarılarla yeni nesil. Bu uyarılar 1 dak sıklığında çalıştırabilirsiniz. Ayrıca birden çok (şu anda iki) ölçümleri izleme destekler.  Uyarı uyarı "(her ölçümü için eşikler aynı anda taşları ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, "çözümlendiğinde" bir bildirim oluşturur (zaman en az bir ölçüm kestiği eşiği yeniden ve koşul yok artık karşılanmış).

* **Klasik etkinlik günlüğü uyarıları** -filtre atadığınız ölçütleri eşleşen bir etkinlik günlüğü olay oluşturulduğunda tetikleyen bir akış günlük uyarı. Bu uyarılar yalnızca bir duruma sahip "uyarı altyapısı, filtre ölçütünü yalnızca herhangi bir yeni olay uygulanır. bu yana, etkinleştirildi". Bu uyarılar yeni bir hizmet durumu olay gerçekleştiğinde veya bir kullanıcı veya uygulama "sanal makineyi silin.", aboneliğinizde, örneğin, bir işlem gerçekleştirdiğinde bildirim hale için kullanılabilir

Azure İzleyicisi aracılığıyla kullanılabilir tanılama günlük verilerini için günlük analizi veri Yönlendirme ve günlük analizi uyarıyı kullanarak öneririz. Aşağıdaki diyagramda Azure İzleyici ve, kavramsal olarak, bu verileri dışına nasıl uyarı veri kaynakları özetler.

![Açıklanan uyarıları](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-classic-alert"></a>Bir Azure İzleyici Klasik uyarı bildirim nasıl alıyor musunuz?
Tarihsel olarak, farklı hizmetler Azure uyarılardan kendi yerleşik bildirim yöntemleri kullanılır. İleride, Azure İzleyici Eylem grupları adı verilen bir yeniden kullanılabilir bildirim gruplandırma sunar. --Tüm e-posta adresi sayısı, (SMS için) telefon numaralarını veya Web kancası URL'leri--bildirim alıcıları bir dizi eylem gruplarını belirtin ve eylem grubuna başvuruda dilediğiniz zaman bir uyarı etkinleştirildikten tüm alıcılar bu bildirim alırsınız. Bu, alıcılar (örneğin, üzerinde çağrısı mühendislik listenize) gruplandırması arasında çok sayıda uyarı nesneleri yeniden kullanmanıza olanak sağlar. Şu anda yalnızca etkinlik günlüğü uyarıları eylem gruplarını kullanın hale getirir, ancak diğer Azure birkaç uyarı türleri de Eylem grupları kullanarak çalışma.

Eylem grupları bildirim e-posta adreslerini ve SMS numaraları ek olarak bir Web kancası URL'si göndererek destekler. Bu otomasyon ve düzeltme, örneğin, kullanarak sağlar:
    - Azure Otomasyonu Runbook
    - Azure işlevi
    - Azure Logic App
    - bir üçüncü taraf hizmeti

Gerçek zamanlı ölçümü uyarıları ve etkinlik günlüğü uyarıları eylem gruplarını kullanabilirsiniz.

Eski ölçüm uyarıları uyarılar (Klasik) altında kullanılabilir Eylem grupları kullanmayın. Tek bir ölçüm uyarıdaki bildirimleri yapılandırabilirsiniz:
* Hizmet Yöneticisi, ortak Yöneticiler veya belirttiğiniz ek e-posta adresleri e-posta bildirimleri gönderin.
* Ek Otomasyon eylemleri başlatmak sağlayan bir Web kancası çağırın.

## <a name="next-steps"></a>Sonraki adımlar
Uyarı kuralları ve bunları kullanarak yapılandırma hakkında bilgi alın:

* Daha fazla bilgi edinmek [ölçümleri](monitoring-overview-metrics.md)
* Yapılandırma [Azure portalında Klasik ölçüm uyarıları](insights-alerts-portal.md)
* Yapılandırma [Klasik ölçüm uyarıları PowerShell](insights-alerts-powershell.md)
* Yapılandırma [Klasik ölçüm uyarıları komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* Yapılandırma [Klasik ölçüm uyarıları Azure İzleyici REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Daha fazla bilgi edinmek [etkinlik günlüğü](monitoring-overview-activity-logs.md)
* Yapılandırma [Azure Portalı aracılığıyla etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları aracılığıyla kaynak yöneticisi](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
* Daha fazla bilgi edinmek [yeni ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md)
* Daha fazla bilgi edinmek [hizmet bildirimleri](monitoring-service-notifications.md)
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)
* Yapılandırma [uyarıları](monitor-alerts-unified-usage.md)
