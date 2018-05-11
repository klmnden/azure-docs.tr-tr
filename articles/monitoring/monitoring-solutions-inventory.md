---
title: Azure yönetim çözümleri için veri toplama ayrıntılarına | Microsoft Docs
description: Azure yönetim çözümlerine kurallarının belirli sorunu etrafına özetlenebilir ölçümleri sağlayan mantığı, Görselleştirme ve veri alım koleksiyonudur.  Bu makalede, Microsoft ve bunların yöntemi ve veri toplama sıklığını ayrıntılarını kullanılabilir yönetim çözümleri listesini sağlar.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: bwren
ms.openlocfilehash: ab07a11883b3462c4b9d0f9adab6c55e4fe49d78
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Azure yönetim çözümleri için veri toplama ayrıntıları
Bu makalede bir listesini içeren [yönetim çözümleri](monitoring-solutions.md) kullanımına Microsoft'tan bağlantılarla kendi ayrıntılı belgeler.  Ayrıca, yöntemi ve günlük analizi veri koleksiyonuna sıklığını hakkında bilgiler sağlar.  Kullanılabilir farklı çözümler tanımlamak ve farklı yönetim çözümleri için veri akışı ve bağlantı gereksinimleri anlamak için bu makaledeki bilgileri kullanabilirsiniz. 

## <a name="list-of-management-solutions"></a>Yönetim çözümleri listesi

Aşağıdaki tabloda [yönetim çözümleri](monitoring-solutions.md) Microsoft tarafından sağlanan Azure. Bir giriş sütun çözüm günlük analizi toplar bu yöntemi kullanarak anlamına gelir.  Seçili sütun yok bir çözüm varsa, ardından bunu doğrudan günlük analizi için başka bir Azure hizmet yazar. Daha fazla bilgi için ayrıntılı belgelere her biri için bağlantıyı izleyin.

Sütun açıklamaları aşağıdaki gibidir:

- **Microsoft İzleme Aracısı** -Aracısı Windows ve Linux Yönetim Paketi SCOM ve yönetim çözümleri azure'dan çalıştırmak için kullanılır. Bu yapılandırmada, bir Operations Manager yönetim grubuna bağlı olmadan doğrudan günlük analizi aracı bağlandı. 
- **Operations Manager** -Microsoft monitoring agent olarak aynı aracı. Bu yapılandırmada, bunun [bir Operations Manager yönetim grubuna bağlı](../log-analytics/log-analytics-om-agents.md) için günlük analizi bağlı. 
-  **Azure depolama** -çözüm, bir Azure depolama hesabından verileri toplar. 
- **Operations Manager gerekli?** Bağlı Operations Manager yönetim grubu için veri toplama yönetim çözümü tarafından gerekli değildir. 
- **Operations Manager Aracısı verilerinin yönetim grubu gönderilen** - aracı ise [SCOM yönetim grubuna bağlı](../log-analytics/log-analytics-om-agents.md), veriler için günlük analizi yönetim sunucusundan gönderilir sonra. Bu durumda, aracı doğrudan Log Analytics'e bağlanmak gerekmez. Bu kutusu seçili değilse, aracı SCOM yönetim grubuna bağlı olsa bile veri Aracısı'ndan doğrudan günlük analizi gönderilir. ya da günlük analizi ile iletişim kurabilmesi gerekir bir [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md).
- **Toplama sıklığı** -sıklığı belirten verileri yönetim çözümü tarafından toplanır. 



| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager Aracısı verilerinin yönetim grubu gönderilen** | **Toplama sıklığı** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Activity Log Analytics](../log-analytics/log-analytics-activity.md) | Azure | | | | | | bildirim |
| [AD Değerlendirmesi](../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [AD Çoğaltma Durumu](../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 gün |
| [Aracı Durumu](../operations-management-suite/oms-solution-agenthealth.md) | Windows ve Linux | &#8226; | &#8226; | | | &#8226; | 1 dakika |
| [Uyarı Yönetimi](../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |geldiğinde |
| [Uyarı Yönetimi](../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 dakika |
| [Uyarı Yönetimi](../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 dakika |
| [Azure Site Recovery](../site-recovery/site-recovery-overview.md) | Azure | | | | | | yok |
| [Uygulama Öngörüler Bağlayıcısı (Önizleme)](../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | bildirim |
| [Otomasyon karma çalışanı](../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | yok |
| [Azure uygulama ağ geçidi analizi](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager Aracısı verilerinin yönetim grubu gönderilen** | **Toplama sıklığı** |
| [Azure ağ güvenlik grubu analizi](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| [Azure SQL analizi (Önizleme)](../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 dakika |
| [Backup](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | bildirim |
| [Kapasite ve performans (Önizleme)](../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |geldiğinde |
| [Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |saatlik |
| [Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md) |Linux |&#8226; | | | | |saatlik |
| [Kapsayıcılar](../log-analytics/log-analytics-containers.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 3 dakika |
| [Anahtar Kasası Analizi](../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |bildirim |
| [Kötü Amaçlı Yazılım Değerlendirmesi](../log-analytics/log-analytics-malware.md) |Windows |&#8226; |&#8226; | | |&#8226; |saatlik |
| [Ağ Performansı İzleyicisi](../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP el sıkışmaları veri her 5 saniye boyunca 3 dakikada gönderilen |
| [Office 365 Analytics (Önizleme)](../operations-management-suite/oms-solution-office-365.md) |Windows | | | | | |bildirim |
| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager Aracısı verilerinin yönetim grubu gönderilen** | **Toplama sıklığı** |
| [Güvenlik ve Denetim](../operations-management-suite/oms-security-getting-started.md) (Syslog) | Linux | &#8226; | | |  |  | geldiğinde |
| [Güvenlik ve Denetim](../operations-management-suite/oms-security-getting-started.md) (güvenlik olay günlüklerini) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | geldiğinde |
| [Güvenlik ve Denetim](../operations-management-suite/oms-security-getting-started.md) (güvenlik duvarı günlüklerini) |Windows |&#8226; |&#8226; |  |  |  |geldiğinde |
| [Service Fabric Analytics (Önizleme)](../log-analytics/log-analytics-service-fabric.md) |Windows | | |&#8226; | | |5 dakika |
| [Hizmet Eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 15 saniye |
| [SQL Değerlendirmesi](../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [SurfaceHub](../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |geldiğinde |
| [System Center Operations Manager değerlendirmesi (Önizleme)](../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | yedi gün |
| [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |en az 2 kez gün ve bir güncelleştirme yüklendikten sonra 15 dakika |
| [Yükseltme Hazırlığı](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 gün |
| [VMware izleme (Önizleme)](../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 dakika |
| [Kablo verileri 2.0 (Önizleme)](../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2 / 8.1 veya üzeri) |&#8226; |&#8226; | | | | 1 dakika |




## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [sorgular oluşturmak](../log-analytics/log-analytics-log-searches.md) yönetim çözümleri tarafından toplanan verileri çözümlemek için.
