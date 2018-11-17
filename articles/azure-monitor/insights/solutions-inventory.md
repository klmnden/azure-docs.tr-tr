---
title: Azure yönetim çözümlerine için veri koleksiyonu ayrıntıları | Microsoft Docs
description: Azure yönetim çözümlerine bir belirli sorun alanına odaklanan ölçümler sağlayan mantık, Görselleştirme ve veri alımı kurallarından oluşan bir koleksiyondur.  Bu makalede, Microsoft ve bunların yöntemi ve veri toplama sıklığı hakkında ayrıntılar kullanılabilir Yönetimi çözümlerinin bir listesini sağlar.
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
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: ebbedb38256c997f39e30e27aa29139dc2496fd1
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51819262"
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Azure yönetim çözümlerine için veri koleksiyonu ayrıntıları
Bu makalede bir listesini içerir [yönetim çözümleri](solutions.md) kullanımına Microsoft gelen bağlantılarla ilgili ayrıntılı belgelere.  Ayrıca kendi yöntemi ve Log Analytics ile veri toplama sıklığı hakkında bilgiler sağlar.  Farklı çözümlerin tanımlamak ve farklı yönetim çözümleri için veri akışı ve bağlantı gereksinimlerini anlamak için bu makaledeki bilgileri kullanabilirsiniz. 

## <a name="list-of-management-solutions"></a>Yönetim çözümleri listesi

Aşağıdaki tabloda [yönetim çözümleri](solutions.md) Microsoft tarafından sağlanan Azure. Çözüm, Log Analytics'e veri toplar sütununda bir girişe bu yöntemi kullanmak anlamına gelir.  Seçili sütun yok bir çözümü varsa, ardından bunu doğrudan Log Analytics'e başka bir Azure hizmet yazar. Daha fazla bilgi için ayrıntılı belgelere her biri için bağlantıyı izleyin.

Sütunların açıklamaları aşağıdaki gibidir:

- **Microsoft İzleme Aracısı** -aracı Yönetim Paketi SCOM ve yönetim çözümleri azure'dan çalıştırılacağı Windows ve Linux üzerinde kullanılan. Bu yapılandırmada, bir Operations Manager yönetim grubuna bağlı olmadan doğrudan Log Analytics'e aracı bağlandı. 
- **Operations Manager** -Microsoft monitoring agent olarak aynı aracı. Bu yapılandırmada bulunan [bir Operations Manager yönetim grubuna bağlı](../../log-analytics/log-analytics-om-agents.md) Log Analytics'e bağlı. 
-  **Azure depolama** -çözüm, bir Azure depolama hesabından veri toplar. 
- **Operations Manager gerekli?** Bağlı Operations Manager yönetim grubu için veri toplama yönetim çözümü tarafından gereklidir. 
- **Operations Manager aracısı veri yönetim grubu gönderilen** - aracı [bir SCOM yönetim grubuna bağlı](../../log-analytics/log-analytics-om-agents.md), ardından veri yönetim sunucusundaki verileri Log Analytics'e gönderilir. Bu durumda, aracıyı doğrudan Log Analytics'e bağlama gerekmez. Bu kutusu seçili değilse, aracı bir SCOM yönetim grubuna bağlı olsa bile ardından veriler Aracıdan doğrudan Log Analytics'e gönderilir. Log analytics'e iletişim kurabilmesi gerekir [Log Analytics gateway](../../log-analytics/log-analytics-oms-gateway.md).
- **Toplama sıklığı** -sıklığı belirtir verileri yönetim çözümü tarafından toplanır. 



| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Activity Log Analytics](../../log-analytics/log-analytics-activity.md) | Azure | | | | | | bildirim |
| [AD Değerlendirmesi](../../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [AD Çoğaltma Durumu](../../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 gün |
| [Aracı Durumu](solution-agenthealth.md) | Windows ve Linux | &#8226; | &#8226; | | | &#8226; | 1 dakika |
| [Uyarı Yönetimi](../../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |geldiğinde |
| [Uyarı Yönetimi](../../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 dakika |
| [Uyarı Yönetimi](../../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 dakika |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | yok |
| [Application Insights Bağlayıcısı (Önizleme)](../../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | bildirim |
| [Otomasyon karma çalışanı](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | yok |
| [Azure uygulama ağ geçidi analizi](../../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| [Azure ağ güvenlik grubu Analytics (kullanım dışı)](../../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| [Azure SQL Analytics (Önizleme)](../../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 dakika |
| [Backup](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | bildirim |
| [Kapasite ve performans (Önizleme)](../../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |geldiğinde |
| [Değişiklik İzleme](../../automation/automation-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |saatlik |
| [Değişiklik İzleme](../../automation/automation-change-tracking.md) |Linux |&#8226; | | | | |saatlik |
| [Kapsayıcılar](../../log-analytics/log-analytics-containers.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 3 dakika |
| [Anahtar Kasası Analizi](../../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |bildirim |
| [Kötü Amaçlı Yazılım Değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |saatlik |
| [Ağ Performansı İzleyicisi](../../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP el sıkışmaları veri her 5 saniyede 3 dakikada gönderilen. |
| [Office 365 Analytics (Önizleme)](solution-office-365.md) |Windows | | | | | |bildirim |
| **Yönetim çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| [Service Fabric Analizi](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 dakika |
| [Hizmet Eşlemesi](../../azure-monitor/insights/service-map.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 15 saniye |
| [SQL Değerlendirmesi](../../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [SurfaceHub](../../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |geldiğinde |
| [System Center Operations Manager değerlendirmesi (Önizleme)](../../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | yedi gün |
| [Güncelleştirme yönetimi](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |en az 2 katı gün ve güncelleştirme yüklendikten sonra 15 dakika |
| [Yükseltme Hazırlığı](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 gün |
| [(Kullanım dışı) VMware izleme](../../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 dakika |
| [Wire Data 2.0 (Önizleme)](../../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2 / 8.1 veya üzeri) |&#8226; |&#8226; | | | | 1 dakika |




## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [sorguları oluşturma](../../log-analytics/log-analytics-queries.md) yönetim çözümleri tarafından toplanan verileri analiz etmek için.
