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
ms.date: 05/15/2018
ms.author: robb
ms.openlocfilehash: 039ab7226b4ea7e011e1c0cbb4cac528b5237483
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Microsoft Azure Klasik uyarılar nelerdir?

> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni neredeyse gerçek zamanlı ölçüm uyarıları ve yeni uyarılar deneyimi](monitoring-overview-unified-alerts.md). 
>

Uyarılar, verilerin üzerine koşulları yapılandırın ve son izleme verilerini koşullara uyan bildirim duruma olanak tanır.


## <a name="alerts-on-azure-monitor-data"></a>Azure İzleyicisi veri uyarılar
İki Klasik uyarı türleri kullanılabilir - ölçüm uyarılar ve etkinlik günlüğü uyarıları vardır.

* **Klasik ölçüm uyarıları** -belirtilen bir ölçüm değerini atadığınız bir eşik kestiği bu uyarı tetikler. Uyarı, uyarının "(eşiği aşıldığında ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, "(eşiği yeniden çapraz ve koşulu artık karşılanmıyor olduğunda) çözümlendiğinde" bir bildirim oluşturur. Ölçüm yeni uyarılar için aşağıya bakın.

* **Klasik etkinlik günlüğü uyarıları** -filtre atadığınız ölçütleri eşleşen bir etkinlik günlüğü olay oluşturulduğunda tetikleyen bir akış günlük uyarı. Bu uyarılar yalnızca bir duruma sahip "uyarı altyapısı, filtre ölçütünü yalnızca herhangi bir yeni olay uygulanır. bu yana, etkinleştirildi". Bu uyarılar yeni bir hizmet durumu olay gerçekleştiğinde veya bir kullanıcı veya uygulama "sanal makineyi silin.", aboneliğinizde, örneğin, bir işlem gerçekleştirdiğinde bildirim hale için kullanılabilir

Azure İzleyicisi aracılığıyla kullanılabilir tanılama günlük verilerini için günlük analizi (önceki adıyla OMS) veri Yönlendirme ve günlük analizi sorgu uyarıyı kullanarak öneririz. Analytics şimdi kullandığı oturum [yöntemi yeni uyarı](monitoring-overview-unified-alerts.md) 

Aşağıdaki diyagramda Azure İzleyici ve, kavramsal olarak, bu verileri dışına nasıl uyarı veri kaynakları özetler.

![Açıklanan uyarıları](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Sınıflandırma Azure İzleyicisi uyarı (Klasik)
Azure Klasik uyarıları ve bunların işlevlerini açıklamak için aşağıdaki koşulları'nı kullanır:
* **Uyarı** -sağlandığında etkin hale gelir ölçüt (bir veya daha fazla kural ya da koşullar) tanımı.
* **Etkin** -klasik bir uyarı tarafından tanımlanan ölçütler karşılandığında durumu.
* **Çözümlenen** -klasik bir uyarı tarafından tanımlanan ölçütler artık karşılandığında önceden karşılanıp sonra durumu.
* **Bildirim** - etkin hale klasik bir uyarı dışına tabanlı gerçekleştirilecek eylem.
* **Eylem** -bir alıcı (örneğin, bir adresi e-postayla gönderme veya bir Web kancası URL'si nakil) bir bildirim gönderilmesini belirli bir çağrı. Bildirimleri genellikle birden çok eylem tetikleyebilir.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Bir Azure İzleyici Klasik uyarıdan nasıl bir bildirim alıyorsunuz?
Tarihsel olarak, farklı hizmetler Azure uyarılardan kendi yerleşik bildirim yöntemleri kullanılır. 

Azure İzleyici çağrılan gruplandırma yeniden kullanılabilir bir bildirim sunar artık *Eylem grupları*. Alıcıları bildirim için bir dizi eylem gruplarını belirtin ve eylem grubuna başvuruda dilediğiniz zaman bir uyarı etkinleştirildikten tüm alıcılar bu bildirim alırsınız. Bu, alıcılar (örneğin, üzerinde çağrısı mühendislik listenize) gruplandırması arasında çok sayıda uyarı nesneleri yeniden kullanmanıza olanak sağlar. Eylem grupları bildirim e-posta adresleri, SMS numaraları ve çeşitli diğer eylemleri yanı sıra bir Web kancası URL'si göndererek destekler.  Daha fazla bilgi için bkz: [Eylem grupları](monitoring-action-groups.md). 

Eski classic etkinlik günlüğü uyarıları eylem gruplarını kullanın.

Ancak, eski ölçüm uyarıları Eylem grupları kullanmayın. Bunun yerine, aşağıdaki eylemleri yapılandırabilirsiniz: 
* Hizmet Yöneticisi, ortak Yöneticiler veya belirttiğiniz ek e-posta adresleri e-posta bildirimleri gönderin.
* Ek Otomasyon eylemleri başlatmak sağlayan bir Web kancası çağırın.

Web kancası otomasyon ve düzeltme, örneğin, kullanarak sağlar:
    - Azure Otomasyonu Runbook
    - Azure İşlevi
    - Azure mantıksal uygulama
    - bir üçüncü taraf hizmeti

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
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)
* Yapılandırma [yeni uyarılar](monitor-alerts-unified-usage.md)
