---
title: "Azure günlük analizi yönetim çözümleri ekleme | Microsoft Docs"
description: "Azure yönetim çözümlerine kurallarının belirli sorunu etrafına özetlenebilir ölçümleri sağlayan mantığı, Görselleştirme ve veri alım koleksiyonudur."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6c7d8d6946d89e4c6541636287e3022c444e0eb8
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="add-azure-log-analytics-management-solutions-to-your-workspace"></a>Sunucunuzdan çalışma alanınıza Azure günlük analizi yönetim çözümleri Ekle

Günlük analizi yönetim çözümleri koleksiyonu olan **mantığı**, **görselleştirme**, ve **veri alım kuralları** belirli bir özetlenebilir ölçümleri sağlar sorun alanı. Bu makalede, günlük analizi tarafından desteklenen yönetim çözümleri listeler ve ekleme ve bir çalışma alanı için Azure portal kullanarak kaldırma gösterir.

Yönetim çözümleri için daha ayrıntılı Öngörüler izin ver:

* Araştırmak ve işletim sorunları daha hızlı çözülmesine yardımcı olur
* Toplamak ve makine verileri çeşitli türlerde ilişkilendirmek
* Çözüm sunan etkinlikleri ile öngörülü Yardım.

> [!NOTE]
> Etkinleştirmek için bir yönetim çözümü gerek kalmaması günlük analizi günlük arama işlevselliği içerir. Ancak, veri görselleştirmeleri, önerilen arar ve Öngörüler alanınıza yönetim çözümleri ekleyerek alırsınız.

Bu makalede'ı kullanarak yönetim çözümleri Market Azure portalını kullanarak bir çalışma alanına ekleyin. Bir çözüm ekledikten sonra veriler altyapınızdaki sunucularından toplanır ve günlük analizi için gönderilir. İşlem genellikle bir saat için birkaç dakika sürer. Veri Hizmeti işledikten sonra günlük analizi görüntüleyebilirsiniz.

Artık gerekli olmadığında bir yönetim çözümü kolayca kaldırabilirsiniz. Bir yönetim çözümü kaldırdığınızda, verileri için günlük analizi gönderilmez. Ücretsiz fiyatlandırma katmanı varsa, bir çözüm kaldırma veri günlük kotası altında olmanıza yardımcı kullanıldığında, veri miktarını azaltır.

## <a name="view-available-management-solutions"></a>Görünüm kullanılabilir yönetim çözümleri

Azure Market listesini içeren [yönetim çözümleri için günlük analizi](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Tıklayarak yönetim çözümleri Azure Marketi'nden yükleyebilirsiniz **Şimdi Al** her çözümün altındaki bağlantıyı.

## <a name="add-a-management-solution"></a>Bir yönetim çözümü ekleyin
1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
2. İçinde **yeni** altında dikey **Market**seçin **izleme + Yönetim**.
3. İçinde **izleme + Yönetim** dikey penceresinde tıklatın **tümünü görmek**.  
    ![İzleme + yönetim dikey penceresi](./media/log-analytics-add-solutions/monitoring-management-blade.png)  
4. Sağındaki **yönetim çözümleri**, tıklatın **daha fazla**.
5. İçinde **yönetim çözümleri** dikey penceresinde, bir çalışma alanına eklemek istediğiniz bir yönetim çözümü seçin.  
    ![İzleme + yönetim dikey penceresi](./media/log-analytics-add-solutions/management-solutions.png)  
6. Yönetim çözümü dikey penceresinde, yönetim çözümü hakkındaki bilgileri gözden geçirin ve ardından **oluşturma**.
7. *Yönetim çözümü adı* dikey penceresinde, yönetim çözümüyle ilişkilendirmek istediğiniz bir çalışma alanı seçin.
8. İsteğe bağlı olarak, Azure abonelik, kaynak grubunu ve konumu çalışma ayarlarını değiştirin. Ayrıca seçebilirsiniz **Otomasyon seçenekleri**. **Oluştur**’a tıklayın.  
    ![çözüm çalışma alanı](./media/log-analytics-add-solutions/solution-workspace.png)  
9. Sunucunuzdan çalışma alanınıza eklediğiniz yönetim çözümü kullanmaya başlamak için gidin **günlük analizi** > **abonelikleri** > ***çalışma alanı adı***  >  **Genel bakış**. Yeni bir kutucuk Yönetimi çözümünüz için görüntülenir. Açmak ve çözüm için veriler toplandıktan sonra çözüm kullanmaya başlamak için kutucuğa tıklayın.

## <a name="remove-a-management-solution"></a>Bir yönetim çözümünü Kaldır

1. İçinde [Azure portal](https://portal.azure.com), gitmek **günlük analizi** > **abonelikleri** > ***çalışma alanı adı*** ve ardından ***çalışma alanı adı*** dikey penceresinde tıklatın **çözümleri**.
2. Yönetim çözümleri listesinde, kaldırmak istediğiniz çözümü seçin.
3. Çalışma alanınız için çözüm dikey penceresinde tıklayın **silmek**.  
    ![Çözüm silme](./media/log-analytics-add-solutions/solution-delete.png)  
4. Onay iletişim kutusunda tıklatın **Evet**.

## <a name="offers-and-pricing-tiers"></a>Teklifler ve fiyatlandırma katmanları

Aşağıdaki tabloda her Operations Management Suite teklif yönetim çözümleri ait tanımlar.
Tablo aynı zamanda her yönetim çözümü için kullanılabilen fiyatlandırma katmanlarına tanımlar.
Aşağıdaki tabloda tüm çözümleri Azure portalı ve günlük analizi portalında Çözümleri Galerisi içinde kullanılabilir.

| Yönetim çözümü                                                                       | Sunduğu                                                                     | Fiyatlandırma katmanlarına<sup>1</sup>                                                 | Notlar |
| ---                                                                                       | ---                                                                       | ---                                                                                                       | ---   |
| [Activity Log Analytics](log-analytics-activity.md)                                                                   | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | 90 günlük veri kullanılabilir ücretsiz<br>Veri ücretsiz katmanı ucun tabi değil |
| [AD Değerlendirmesi](log-analytics-ad-assessment.md)                                           | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [AD Çoğaltma Durumu](log-analytics-ad-replication-status.md)                           | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Aracı Durumu](../operations-management-suite/oms-solution-agenthealth.md)                                                                                | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Veri ücretsiz katmanı ucun tabi değil<br> Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Uyarı Yönetimi](log-analytics-solution-alert-management.md)                            | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Uygulama Öngörüler Bağlayıcısı (Önizleme)](log-analytics-app-insights-connector.md)                                               | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Otomasyon karma çalışanı](../automation/automation-hybrid-runbook-worker.md)                                                                     | <ul><li>Otomasyon ve Denetim</li></ul>                                  | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir |
| [Azure uygulama ağ geçidi analizi](log-analytics-azure-networking-analytics.md)    | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Azure ağ güvenlik grubu analizi](log-analytics-azure-networking-analytics.md)     | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Azure SQL analizi (Önizleme)](log-analytics-azure-sql.md)                                                       | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br>Başına&nbsp;düğümü&nbsp;(OMS)                                                                          | Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir|
| [Azure Web Apps Analytics](log-analytics-azure-web-apps-analytics.md)     | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
|[Backup](../backup/backup-introduction-to-azure-backup.md)                                                                                 | <ul><li>Insight and Analytics</li></ul>                                   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                       | Klasik bir yedekleme kasası gerektirir.<br> Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Kapasite ve performans (Önizleme)](log-analytics-capacity.md)                                                   | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Değişiklik İzleme](log-analytics-change-tracking.md)                                       | <ul><li>Otomasyon ve Denetim</li></ul>                                  | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir |
| [Kapsayıcılar](log-analytics-containers.md)                                                 | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [BT Hizmet Yönetimi Bağlayıcısı](log-analytics-itsmc-overview.md)                                                | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)     | |
| Hdınsight HBase izleme <br>(Önizleme)                                                  | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Anahtar Kasası Analizi](log-analytics-azure-key-vault.md)                   | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Logic Apps B2B](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)                    | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Kötü Amaçlı Yazılım Değerlendirmesi](log-analytics-malware.md)                                            | <ul><li>Güvenlik ve Uyumluluk</li></ul>                                 | Ücretsiz<br> Tek Başına<br>Başına&nbsp;düğümü&nbsp;(OMS)                                                                           | Güvenlik ve uyumluluk çözümleri 19 Haziran 2017 sonra eklerseniz [faturalama olan düğüm başına](https://azure.microsoft.com/pricing/details/security-compliance/)ne olursa olsun, fiyatlandırma katmanı çalışma. İlk 60 gün ücretsizdir.  |
| [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) <br>  | <ul><li>Insight and Analytics</li></ul>                                   | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | |
| [Office 365 Analytics (Önizleme)](../operations-management-suite/oms-solution-office-365.md)                                                       | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Güvenlik ve Denetim](../operations-management-suite/oms-security-getting-started.md)      | <ul><li>Güvenlik&nbsp;ve&nbsp;uyumluluk</li></ul>                       | Ücretsiz<br> Tek Başına<br>Başına&nbsp;düğümü&nbsp;(OMS)                                                                           | Bu çözüm güvenlik olay günlüklerini toplama gerektirir<br>Güvenlik ve uyumluluk çözümleri 19 Haziran 2017 sonra eklerseniz [faturalama olan düğüm başına](https://azure.microsoft.com/pricing/details/security-compliance/)ne olursa olsun, fiyatlandırma katmanı çalışma. İlk 60 gün ücretsizdir. |
| [Service Fabric Analytics (Preview)](log-analytics-service-fabric.md)                     | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Hizmet eşlemesi (Önizleme)](../operations-management-suite/operations-management-suite-service-map.md) | <ul><li>Insight and Analytics</li></ul>                      | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Doğu ABD, Batı Avrupa ve Batı Orta ABD kullanılabilir    |
| [Site Recovery](../site-recovery/site-recovery-overview.md)                                                                               | <ul><li>Insight and Analytics</li></ul>                                   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                       | Klasik Site Recovery kasası gerektirir.<br> Azure portal/Marketi'nden eklemek kullanılamaz. |
| [SQL Değerlendirmesi](log-analytics-sql-assessment.md)                                         | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma<br>(Önizleme)                                              | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir |
| [SurfaceHub](log-analytics-surface-hubs.md)                                               | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Azure portal/Marketi'nden eklemek kullanılamaz. |
| [System Center Operations Manager değerlendirmesi (Önizleme)](log-analytics-scom-assessment.md)  | <ul><li>Insight and Analytics</li><li>Log Analytics</li></ul>        | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md)                                                                         | <ul><li>Otomasyon ve Denetim</li></ul>                                  | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir |
| [Güncelleştirme uyumluluğu (Önizleme)](https://docs.microsoft.com/windows/deployment/update/update-compliance-get-started)                                                             | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Ücret ödemeden verilerin veya düğümler<br>Veri ücretsiz katmanı ucun tabi değildir.<br> Azure portal/Marketi'nden eklemek kullanılamaz. |
| [Yükseltme Hazırlığı](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started)                                                          | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | Ücret ödemeden verilerin veya düğümler<br>Veri ücretsiz katmanı ucun tabi değildir.<br> Azure portal/Marketi'nden eklemek kullanılamaz. |
| [VMware izleme (Önizleme)](log-analytics-vmware.md)                                | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Standart<br> Premium&nbsp;(OMS)<br> Başına&nbsp;GB&nbsp;(tek başına)<br> Başına&nbsp;düğümü&nbsp;(OMS)   | |
| [Kablo verileri 2.0 (Önizleme)](log-analytics-wire-data.md)                                                                 | <ul><li>Insight and Analytics</li></ul>                                   | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)                                                                         | Doğu ABD, Batı Avrupa ve Batı Orta ABD kullanılabilir |

<sup>1</sup> *standart* ve *Premium (OMS)* fiyatlandırma katmanlarına bulunan ve yalnızca kendi günlük analizi çalışma alanı 21 Eylül 2016 önce oluşturulan müşteriler için.

### <a name="community-provided-management-solutions"></a>Yönetim çözümleri sağlanan topluluk

Sağlanan topluluk çözümleri web'da [Azure Şablon Galerisi](https://azure.microsoft.com/resources/templates/?term=Per&nbsp;Node&nbsp;(OMS)) ve yazarlar doğrudan.

| Yönetim çözümü               | Sunduğu                                                                     | Fiyatlandırma katmanları                         | Notlar |
| ---                               | ---                                                                       | ---                                   | ---   |
| Tüm sağlanan topluluk çözümleri  | <ul><li>Insight&nbsp;ve&nbsp;analizi</li><li>Log Analytics</li></ul>   | Ücretsiz<br> Başına&nbsp;düğümü&nbsp;(OMS)     |   Bir Otomasyon hesabı bağlanması için günlük analizi çalışma alanınız gerektirir |




## <a name="data-collection-details"></a>Veri toplama ayrıntıları
Aşağıdaki tablolarda, veri toplama yöntemleri ve günlük analizi yönetim çözümleri ve veri kaynakları için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir. Tablolar için eşitlemek çözümü teklifleri tarafından ayrılır [fiyatlandırma katmanlarına abonelik](https://go.microsoft.com/fwlink/?linkid=827926). Etkinlik günlüğü analiz çözümü tüm fiyatlandırma katmanlarına için ücretsiz olarak kullanılabilir.

Günlük analizi Windows aracısı ve System Center Operations Manager Aracısı temelde aynıdır. Windows Aracısı günlük analizi çalışma alanına bağlayın ve bir proxy üzerinden yönlendirmek izin vermek için ek işlevler içerir. Bir Operations Manager Aracısı kullanırsanız, günlük analizi ile iletişim kurmak için bir OMS aracısı olarak hedeflenen gerekir. Bu tabloda Operations Manager aracıları Operations Manager'a bağlı OMS aracılardır. Bkz: [Operations Manager'a günlük analizi](log-analytics-om-agents.md) günlük analizi için mevcut Operations Manager ortamınızı bağlanma hakkında bilgi için.

> [!NOTE]
> Kullandığınız aracısının tür veri aşağıdaki koşullarla günlük analizi için nasıl gönderildiğini belirler:
> - Ya da Windows aracısı veya Operations Manager bağlı OMS Aracısı kullanın.
> - Operations Manager gerekli olduğunda, Operations Manager aracısı veri çözüm için her zaman için günlük analizi Operations Manager yönetim grubu kullanılarak gönderilir. Ayrıca, Operations Manager gerekli olduğunda, yalnızca Operations Manager Aracısı çözüm tarafından kullanılıyor.
> - Operations Manager gerekli değildir ve tablo, Operations Manager Aracısı veriler için günlük analizi gönderilir gösterir yönetim grubu ve ardından Operations Manager kullanarak aracı verileri her zaman için günlük analizi Yönetim grupları kullanılarak gönderilir. Windows aracılarının yönetim grubu atlayabilir ve doğrudan günlük analizi verilerini Gönder.
> - Bir yönetim grubu kullanarak Operations Manager aracısı veri gönderilmez, verileri doğrudan günlük analizi gönderilir — yönetim grubu atlama.

### <a name="insight--analytics--log-analytics"></a>Insight & Analytics / Log Analytics

| Yönetim çözümü | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Etkinlik Günlüğü Analizi | Azure |   |   |   |   |   | bildirim |
| AD Değerlendirmesi |Windows |&#8226; |&#8226; |  |  |&#8226; |7 gün |
| AD Çoğaltma Durumu |Windows |&#8226; |&#8226; |  |  |&#8226; |5 gün |
| Aracı Durumu | Windows ve Linux | &#8226; | &#8226; |   |   | &#8226; | 1 dakika |
| Uyarı Yönetimi (Nagios) |Linux |&#8226; |  |  |  |  |geldiğinde |
| Uyarı Yönetimi (Zabbix) |Linux |&#8226; |  |  |  |  |1 dakika |
| Uyarı Yönetimi (Operations Manager) |Windows |  |&#8226; |  |&#8226; |&#8226; |3 dakika |
| Uygulama Öngörüler Bağlayıcısı (Önizleme) | Azure |   |   |   |   |   | bildirim |
| Azure uygulama ağ geçidi analizi | Azure |   |   |   |   |   | bildirim |
| Azure ağ güvenlik grubu analizi | Azure |   |   |   |   |   | bildirim |
| Azure SQL analizi (Önizleme) |Windows |  |  |  |  |  | 10 dakika |
| Kapasite Yönetimi |Windows |&#8226; |&#8226; |  |  |&#8226; |geldiğinde |
| Kapsayıcılar | Windows ve Linux | &#8226; | &#8226; |   |   |   | 3 dakika |
| Anahtar kasası analizi |Windows |  |  |  |  |  |bildirim |
| Ağ Performansı İzleyicisi | Windows | &#8226; | &#8226; |   |   |   | TCP el sıkışmaları veri her 5 saniye boyunca 3 dakikada gönderilen |
| Office 365 Analytics (Önizleme) |Windows |  |  |  |  |  |bildirim |
| Service Fabric Analytics |Windows |  |  |&#8226; |  |  |5 dakika |
| Hizmet Eşlemesi | Windows ve Linux | &#8226; | &#8226; |   |   |   | 15 saniye |
| SQL Değerlendirmesi |Windows |&#8226; |&#8226; |  |  |&#8226; |7 gün |
| SurfaceHub |Windows |&#8226; |  |  |  |  |geldiğinde |
| System Center Operations Manager değerlendirmesi (Önizleme) | Windows | &#8226; | &#8226; |   |   | &#8226; | yedi gün |
| Yükseltme Analytics (Önizleme) | Windows | &#8226; |   |   |   |   | 2 gün |
| VMware izleme (Önizleme) | Linux | &#8226; |   |   |   |   | 3 dakika |
| Kablo Verileri |Windows (2012 R2 / 8.1 veya üzeri) |&#8226; |&#8226; |  |  |  | 1 dakika |


### <a name="automation--control"></a>Otomasyon ve Denetim

| Yönetim çözümü | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Otomasyon karma çalışanı | Windows | &#8226; | &#8226; |   |   |   | yok |
| Değişiklik İzleme |Windows |&#8226; |&#8226; |  |  |&#8226; |saatlik |
| Değişiklik İzleme |Linux |&#8226; |  |  |  |  |saatlik |
| Güncelleştirme Yönetimi | Windows |&#8226; |&#8226; |  |  |&#8226; |en az 2 kez gün ve bir güncelleştirme yüklendikten sonra 15 dakika |

### <a name="security--compliance"></a>Güvenlik ve Uyumluluk

| Yönetim çözümü | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Kötü amaçlı yazılımdan koruma değerlendirme |Windows |&#8226; |&#8226; |  |  |&#8226; |saatlik |
| Güvenlik ve Denetim<sup>1</sup> | Windows ve Linux | Kısmi | Kısmi | Kısmi |   | Kısmi | çeşitli |

<sup>1</sup> güvenlik ve denetim çözümü, Windows, Operations Manager ve Linux aracılarını günlüklerini toplayabilir. Bkz: [veri kaynakları](#data-sources) veri toplama hakkında bilgi edinmek için:

- Syslog
- Windows Güvenlik olay günlükleri
- Windows Güvenlik duvarı günlükleri
- Windows olay günlükleri



### <a name="protection--recovery"></a>Koruma ve Kurtarma

| Yönetim çözümü | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Backup | Azure |   |   |   |   |   | yok |
| Azure Site Recovery | Azure |   |   |   |   |   | yok |


### <a name="data-sources"></a>Veri kaynakları


| Veri kaynağı | Platform | Microsoft İzleme Aracısı | Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Azure etkinlik günlükleri |Windows |  |  |  |  |  |bildirim |
| Azure tanılama günlükleri |Windows |  |  |  |  |  |bildirim |
| Azure tanılama ölçümleri |Windows |  |  |  |  |  |bildirim |
| ETW |Windows |  |  |&#8226; |  |  |5 dakika |
| IIS günlükleri |Windows |&#8226; |&#8226; |&#8226; |  |  |5 dakika |
| Performans Sayaçları |Windows |&#8226; |&#8226; |  |  |  |Zamanlandığı gibi en az 10 saniye |
| Performans Sayaçları |Linux |&#8226; |  |  |  |  |Zamanlandığı gibi en az 10 saniye |
| Syslog |Linux |&#8226; |  |  |  |  |Azure depolama biriminden: 10 dakika; aracısından: işle |
| Windows Güvenlik olay günlükleri |Windows |&#8226; |&#8226; |&#8226; |  |  |Azure Depolama: 10 min; aracının: işle |
| Windows Güvenlik duvarı günlükleri |Windows |&#8226; |&#8226; |  |  |  |geldiğinde |
| Windows olay günlükleri |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; |Azure Depolama: 10 min; aracının: işle |



## <a name="preview-management-solutions-and-features"></a>Önizleme yönetim çözümleri ve özellikleri
Bir hizmeti çalıştıran ve devops yöntemler aşağıdaki özelliklerin ve çözümlerin geliştirmek müşterilerle ortak mümkün duyuyoruz.

Özel önizleme sırasında özelliği veya geri bildirim sağlamak ve geliştirmeler yapmak için çözüm erken uygulaması için küçük bir grup müşteriler erişim sunuyoruz. Bu erken uygulaması en az özellikleri ve işletimsel yetenekleri sahiptir.

Amacımız biz neler çalışır ve ne işe yaramazsa bulabilmesi şeyler hızla denemektir. Özel önizleme müşterilerden gelen geri bildirim bize biz genel önizlemesi için hazır olduğunu bildirir. kadar size bu süreçte yineleme.

Genel Önizleme sırasında özellik ya da çözüm kullanılabilir tüm kullanıcıların daha fazla geri bildirim alma ve bizim ölçeklendirme doğrulamak ve verimlilik için vermiyoruz. Bu aşamada:

* Önizleme özellikleri ayarlar sekmesinde görünür ve herhangi bir kullanıcı tarafından etkinleştirilebilir.
* Önizleme Çözümleri Galerisi aracılığıyla eklenir veya bir komut dosyası kullanma.

### <a name="what-should-i-know-about-preview-features-and-solutions"></a>Ne ı Önizleme özellikleri ve çözümleri hakkında bilmeniz gereken?
Şu yeni özellikleri hakkında heyecan ve yönetim çözümleri ve biz memnuniyet geliştirmek için sizinle çalışma.

Önizleme özelliklerin ve çözümlerin herkes için doğru değil. Özel önizleme katılma veya genel Önizleme etkinleştirme emin olmak için onay isteyen önce Tamam geliştirilmekte olan bir şey çalıştığınız.

Portal üzerinden bir önizleme özelliği etkinleştirirken, özellik Önizleme sürümünde olduğu, anımsatma bir uyarı görürsünüz.

#### <a name="for-both-private-and-public-preview"></a>Her ikisi için de *özel* ve *ortak* Önizleme
Aşağıdaki bilgiler, ortak ve özel Önizleme için geçerlidir:

* Şeyler düzgün her zaman çalışmayabilir.
  * Aracılığıyla küçük olarak sıkıntı getirir hiç çalışmayan bir şey için engeller aralığı verir.
* Yoktur sistemleriniz üzerinde olumsuz için Önizleme için olası / ortamı.
  * Kullanmakta olduğunuz ancak bazen beklenmeyen durumlar ortaya çıkar sistemlere gerçekleştiği negatif şeyler önlemek deneyin.
* Veri kaybı / Bozulması meydana gelebilir.
* Tanılama günlüklerini veya sorunlarını gidermenize yardımcı olması için diğer verileri toplamak için sorun.
* Özellik veya çözüm (geçici veya kalıcı olarak) kaldırılmış olabilir.
  * Bizim learnings üzerinde biz özelliği ya da çözüm sürüm değil karar verebilir Önizleme süresince temel.
* Önizlemeler çalışmayabilir veya tüm yapılandırmaları test edilmemiştir ve biz sınırlayabilir:
  * Kullanılabilir işletim sistemleri (örneğin, bir özelliği yalnızca Linux Önizleme sırasında geçerli).
  * Kullanılabilir aracısının (MMA, Operations Manager) türünü (örneğin, bir özellik önizlemesinde Operations Manager'da çalışmayabilir).  
* Önizleme çözümleri ve özellikleri tarafından hizmet düzeyi sözleşmesi kapsamında değildir.
* Önizleme özelliklerinin kullanımını kullanım ücretleri doğurur.
* Özellikleri veya yetenekleri özelliği için gereksinim duyduğunuz / kullanışlı olması için çözüm yok veya eksik olabilir.
* Özellikler / çözümleri kullanılabilir olmayabilir tüm bölgelerde.
* Özellikler / çözümleri yerelleştirilmemiş.
* Özellikler / çözümleri, müşteriler veya kullanılabilmesi için cihazların sayısına bir sınır olabilir.
* Yapılandırmayı gerçekleştirmek için ve çözüm/özelliğini etkinleştirmek için komut dosyaları kullanmanız gerekebilir.
* Kullanıcı Arabirimi (UI) eksik ve gün gün değişebilir.
* Üretim için uygun / kritik ortak önizlemeleri olmayabilir sistemler.

#### <a name="for-private-preview"></a>İçin *özel* Önizleme
Yukarıdaki öğelerde ek olarak aşağıdaki bilgileri özel önizlemeleri özeldir:

* Böylece biz özelliği/çözümü daha iyi duruma getirebilirsiniz bize deneyiminiz hakkında geri bildirim sağlamak için bekliyoruz.
* Sizinle anketler, telefon aramaları veya e-posta kullanarak geri bildirim için iletişim.
* Öğeleri her zaman doğru çalışmaz.
* Biz olmayan açığa Sözleşmesi (NDA) için katılımını gerektirebilir veya gizli içeriğin içerebilir.
  * Blog, TWİTLEME veya aksi halde üçüncü taraflarla iletişim önce lütfen Program açığa üzerinde herhangi bir kısıtlamanın anlamak için Önizleme'yi sorumlu olan Yöneticisi ile denetleyin.
* Çalıştırmayın üretim / kritik sistemler.

### <a name="how-do-i-get-access-to-private-preview-features-and-solutions"></a>Özel önizleme özelliklerin ve çözümlerin erişimi nasıl sağlarım?
Önizleme bağlı olarak birkaç farklı şekilde aracılığıyla özel önizlemeleri müşterilere davet ediyoruz.

* Aylık müşteri anket yanıtlama ve bize ile izledikten izni vermiş özel önizlemesi için davet şansınız artırır.
* Microsoft hesap ekibi, belirleyebilirsiniz.
* Twitter'da gönderilen Ayrıntılar temel kaydolabilirsiniz [msopsmgmt](https://twitter.com/msopsmgmt).
* Ayrıntılar paylaşılan topluluk olaylara göre – kaydolabilirsiniz bize karşılamak Ara ups, konferans ve çevrimiçi topluluklar.

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) yönetim çözümleri tarafından toplanan ayrıntılı bilgileri görüntülemek için.
