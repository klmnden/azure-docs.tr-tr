---
title: Azure çözümleri izleme envanteri | Microsoft Docs
description: Azure İzleyici'de izleme çözümleri, belirli sorun alanı çerçevesinde özetlenmiş ölçümler sağlayan mantık, Görselleştirme ve veri alımı kurallarından oluşan bir koleksiyondur.  Bu makalede izleme çözümleri, yöntem ve veri toplama sıklığı hakkında ayrıntıları ve Microsoft gelen kullanılabilir bir listesini sağlar.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/26/2018
ms.author: bwren
ms.openlocfilehash: 9398815ea75c0eacd99a6e40c569254fac671cbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66234027"
---
# <a name="inventory-and-data-collection-details-for-monitoring-solutions-in-azure"></a>Azure çözümlerini izleme için Envanter ve veri koleksiyonu ayrıntıları
[İzleme çözümleri](solutions.md) belirli bir uygulama veya hizmet işlemi ek Öngörüler sağlar, Azure hizmetlerinde yararlanın. İzleme çözümleri, genellikle günlük verilerini toplamak ve sorguları ve toplanan verileri çözümlemek için görünümler sağlar. Azure İzleyici, tüm uygulamaları ve Hizmetleri için izleme çözümleri ekleyebilirsiniz. Bunlar genellikle, kullanım ücretleri çağırabilirsiniz maliyet ancak toplama veri yok kullanılabilir.

Bu makalede bir listesini içerir [montioring çözümleri](solutions.md) kullanımına Microsoft gelen bağlantılarla ilgili ayrıntılı belgelere.  Ayrıca kendi yöntemi ve Azure İzleyici ile veri toplama sıklığı hakkında bilgiler sağlar.  Farklı çözümlerin tanımlamak ve farklı izleme çözümleri veri akışı ve bağlantı gereksinimlerini anlamak için bu makaledeki bilgileri kullanabilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-monitoring-solutions"></a>İzleme çözümleri listesi

Aşağıdaki tabloda [izleme çözümleri](solutions.md) Microsoft tarafından sağlanan Azure. Çözüm, Azure İzleyici ile veri toplar sütununda bir girişe bu yöntemi kullanmak anlamına gelir.  Seçili sütun yok bir çözümü varsa, ardından, doğrudan Azure İzleyici için başka bir Azure hizmet yazar. Daha fazla bilgi için ayrıntılı belgelere her biri için bağlantıyı izleyin.

Sütunların açıklamaları aşağıdaki gibidir:

- **Microsoft İzleme Aracısı** - Yönetim Paketi SCOM'den çalıştırmak için Windows ve Linux üzerinde kullanılan ve Azure çözümlerini İzleme Aracısı. Bu yapılandırmada, bir Operations Manager yönetim grubuna bağlı olmadan doğrudan Azure İzleyici aracı bağlandı. 
- **Operations Manager** -Microsoft monitoring agent olarak aynı aracı. Bu yapılandırmada bulunan [bir Operations Manager yönetim grubuna bağlı](../platform/om-agents.md) Azure İzleyicisi'ne bağlı. 
-  **Azure depolama** -çözüm, bir Azure depolama hesabından veri toplar. 
- **Operations Manager gerekli?** Bağlı Operations Manager yönetim grubu için veri koleksiyonu izleme çözümüyle gereklidir. 
- **Operations Manager aracısı veri yönetim grubu gönderilen** - aracı [bir SCOM yönetim grubuna bağlı](../platform/om-agents.md), ardından veri için Azure İzleyici, yönetim sunucusundan gönderilir. Bu durumda, aracıyı doğrudan Azure İzleyicisi ile bağlantı gerekmez. Bu kutusu seçili değilse, aracı bir SCOM yönetim grubuna bağlı olsa bile ardından veriler Aracıdan doğrudan Azure İzleyici gönderilir. Azure İzleyicisi iletişim kurabilmesi gerekir [Log Analytics gateway](../platform/gateway.md).
- **Toplama sıklığı** -sıklığı belirtir izleme çözüm tarafından toplanan veriler. 



| **İzleme çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Etkinlik günlüğü analizi](../platform/activity-log-collect.md) | Azure | | | | | | bildirim |
| [AD Değerlendirmesi](ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [AD Çoğaltma Durumu](ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 gün |
| [Aracı Durumu](solution-agenthealth.md) | Windows ve Linux | &#8226; | &#8226; | | | &#8226; | 1 dakika |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |geldiğinde |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 dakika |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 dakika |
| [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | yok |
| [Application Insights Bağlayıcısı (kullanım dışı)](../platform/app-insights-connector.md) | Azure | | | |  |  | bildirim |
| [Otomasyon karma çalışanı](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | yok |
| [Azure uygulama ağ geçidi analizi](azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| **İzleme çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| [Azure ağ güvenlik grubu Analytics (kullanım dışı)](azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirim |
| [Azure SQL Analytics (Önizleme)](azure-sql.md) | Windows | | | | | | 1 dakika |
| [Backup](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | bildirim |
| [Kapasite ve performans (Önizleme)](capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |geldiğinde |
| [Değişiklik İzleme](../../automation/change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |[Değişir](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Değişiklik İzleme](../../automation/change-tracking.md) |Linux |&#8226; | | | | |[Değişir](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Kapsayıcılar](containers.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 3 dakika |
| [Anahtar Kasası Analizi](azure-key-vault.md) |Windows | | | | | |bildirim |
| [Kötü Amaçlı Yazılım Değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |saatlik |
| [Ağ Performansı İzleyicisi](network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP el sıkışmaları veri her 5 saniyede 3 dakikada gönderilen. |
| [Office 365 Analytics (Önizleme)](solution-office-365.md) |Windows | | | | | |bildirim |
| **İzleme çözümü** | **Platform** | **Microsoft İzleme Aracısı** | **Operations Manager Aracısı** | **Azure depolama alanı** | **Operations Manager gerekli?** | **Operations Manager aracısı veri yönetim grubu gönderilir.** | **Toplama sıklığı** |
| [Service Fabric Analizi](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 dakika |
| [Hizmet Eşlemesi](service-map.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 15 saniye |
| [SQL Değerlendirmesi](sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [SurfaceHub](surface-hubs.md) |Windows |&#8226; | | | | |geldiğinde |
| [System Center Operations Manager değerlendirmesi (Önizleme)](scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | yedi gün |
| [Güncelleştirme yönetimi](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |en az 2 katı gün ve güncelleştirme yüklendikten sonra 15 dakika |
| [Yükseltme Hazırlığı](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 gün |
| [(Kullanım dışı) VMware izleme](vmware.md) | Linux | &#8226; |  |  |  |  | 3 dakika |
| [Wire Data 2.0 (Önizleme)](wire-data.md) |Windows (2012 R2 / 8.1 veya üzeri) |&#8226; |&#8226; | | | | 1 dakika |




## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [yükleme ve kullanım izleme çözümleri](solutions.md).
* Bilgi nasıl [sorguları oluşturma](../log-query/log-query-overview.md) izleme çözümleri tarafından toplanan verileri analiz etmek için.
