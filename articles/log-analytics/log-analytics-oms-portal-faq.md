---
title: Ortak sorular için günlük analizi kullanıcılar için Azure portalı OMS portalı geçiş | Microsoft Docs
description: Günlük analizi kullanıcılar Azure portalına OMS Portalı'ndan geçiş için sık sorulan soruların yanıtları.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: bwren
ms.openlocfilehash: 1e0fd56b6e420103b4f786985f71a84737db642d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333641"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>Günlük analizi kullanıcılar için Azure portalı OMS portalı geçiş için ortak soruları
Günlük analizi OMS portalı adlı kendi portal yapılandırmasını yönetmek ve toplanan verileri çözümlemek için başlangıçta kullanılır.  Bu portalı tüm işlevinden burada geliştirilecek devam edecek Azure portalına taşındı.

Bu makalede, bu geçiş yapma kullanıcılar için sık sorulan soruları yanıtlar.  Günlük analizi OMS portalında kullandıysanız, nasıl aynı görevleri Azure Portalı'nda gerçekleştirebileceğiniz için yanıtlar burada bulabilirsiniz.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Azure günlük analizi nerede bulabilirim?
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.  Tıklatın **tüm hizmetleri**ve kaynak listesinde yazın **günlük analizi**. Seçin **günlük analizi** ve çalışma alanınızı seçin. Çalışma alanı için Özet sayfasında görüntülenir.

![Log Analytics çalışma alanı](media/log-analytics-new-portal/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>İzinleri nasıl yönetebilirim?
Azure portalında günlük analizi çalışma alanınıza erişimi yoksa, kullanarak izinlerinizi yapılandırmanız gereken [Azure rol tabanlı erişim](../active-directory/role-based-access-control-configure.md). Çalışma alanı izinleri yönetme hakkında daha fazla bilgi için bkz: [çalışma alanlarını yönet](../log-analytics/log-analytics-manage-access.md#manage-accounts-and-users). Uyarılar için izinleri yönetme hakkında daha fazla bilgi için bkz: [rolleri, izinleri ve güvenlik Azure İzleyicisi ile başlayın](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Yeni bir çalışma alanı nasıl oluşturulur? 
Azure portalında çalışma alanları listesinden tıklatın **Ekle** çalışma alanları listesinde.  Tüm Ayrıntılar için bkz: [Azure portalında günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md).

![Genel Bakış sayfası](media/log-analytics-new-portal/new-workspace.png)

## <a name="where-is-my-overview-page"></a>My genel bakış sayfası nerede?
OMS portalında ana ekran çalışma alanınızı ve oluşturduğunuz özel görünümleri yüklenen tüm yönetim çözümleri için kutucukları görüntüler. Bu aynı görünüm Azure portalında kullanılabilir. Alanınızdan, seçin **çalışma Özet**.

![Genel Bakış sayfası](media/log-analytics-new-portal/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Nasıl ı günlük arama ve Görünüm Tasarımcısı açmak?
Her ikisi de **günlük arama** ve **Görünüm Tasarımcısı** sağ sonraki için ana sayfada ve Azure portalında çalışma alanınızın soldaki menüde kullanılabilir **genel bakış**.

## <a name="where-do-i-find-settings"></a>Ayarları nereden bulabilirim?
Ayarların çoğu **ayarları** OMS portalı bölümünde bulunan **Gelişmiş ayarları** çalışma alanı için Azure portal menüsünde.

![Gelişmiş ayarlar](media/log-analytics-new-portal/advanced-settings.png)

Aşağıdaki bölümlerde daha önce bulunan ayarları nasıl erişim tam bir listesi sağlanmaktadır **ayarları** OMS portalı bölümü.

### <a name="accounts"></a>Hesaplar 
Hesap ayarları aşağıdaki tabloda açıklandığı gibi Azure portalında farklı yerlerde yönetilir.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Otomasyon Hesabı | **Otomasyon hesabı** çalışma alanı menüsüne. |
| Azure aboneliği & veri planı | **Fiyatlandırma katmanı** çalışma alanı menüsüne. |
| Kullanıcıları yönetme | Azure rol tabanlı erişmek için kullandığınız [çalışma alanınızı yönetmek](#how-do-i-manage-permissions). |
| Çalışma Alanı Bilgileri | Kullanılabilir bilgi **OMS çalışma** çalışma alanı menüsüne. |

### <a name="alerts"></a>Uyarılar
Günlük analizi sorgularına dayalı uyarı kuralları şimdi yönetilir [deneyimi uyarı birleşik](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Bilgisayar Grupları
Bilgisayar gruplarını yönetme **Gelişmiş ayarları** çalışma alanı menüsüne. 

### <a name="connected-sources"></a>Bağlı Kaynaklar
Çoğu bağlı kaynak ayarlarını yönet içinde **Gelişmiş ayarları** çalışma alanı menüsüne. Aşağıdaki tabloda bu menü her bölüm için Ayrıntılar sağlar.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Windows Sunucuları   | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Linux sunucuları   | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Azure Storage     | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| System Center     | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Office 365        | Bkz: [Office 365 Yönetim çözümünü belgelere](../operations-management-suite/oms-solution-office-365.md) yapılandırma ayrıntıları için. |
| Windows Telemetrisi | Henüz kullanılamıyor Azure portalında. |
| ITSM Bağlayıcısı    | Bkz: [bağlanmak ITSM ürünler/hizmetler BT Hizmet Yönetimi Bağlayıcısı ile](../log-analytics/log-analytics-itsmc-connections.md) ITSM hizmetinizi günlük analizi ile bağlanma hakkında yönergeler için. |

### <a name="data"></a>Veriler
Çoğu veri ayarlarını yönet içinde **Gelişmiş ayarları** çalışma alanı menüsüne. Aşağıdaki tabloda bu menü her bölüm için Ayrıntılar sağlar.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Windows Olay Günlükleri           | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Windows performans sayaçları | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Linux performans sayaçları   | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| IIS Günlükleri                     | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Özel Alanlar                | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Özel Günlükler                  | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Syslog                       | **Gelişmiş ayarlar** çalışma alanı menüsüne. |
| Application Insights         | Günlük analizi ve Application Insights aynı veri altyapısı paylaşır göre bu çözüm kullanım dışıdır.  |
| Windows Dosya İzleme        | **Değişiklik izleme** Azure Automation menüsü. Bkz: [değişiklik izleme çözümü ile ortamınızdaki Değişiklikleri İzle](../automation/automation-change-tracking.md) Ayrıntılar için. |
| Windows Kayıt Defteri İzleme        | **Değişiklik izleme** Azure Automation menüsü. Bkz: [değişiklik izleme çözümü ile ortamınızdaki Değişiklikleri İzle](../automation/automation-change-tracking.md) Ayrıntılar için. |
| Linux Dosya İzleme          | **Değişiklik izleme** Azure Automation menüsü. Bkz: [değişiklik izleme çözümü ile ortamınızdaki Değişiklikleri İzle](../automation/automation-change-tracking.md) Ayrıntılar için. |

### <a name="solutions"></a>Çözümler
Çözümleri yönetmek **çözümleri** çalışma alanı menüsüne. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Nasıl yükleyebilir ve yönetim çözümleri kaldırmak?
OMS portalında çözümleri Galeriden yönetim çözümleri yükleyin ve bunları kaldırılan **ayarları**. Azure portalında [yönetim çözümleri yüklemek](../monitoring/monitoring-solutions.md#install-a-management-solution) Azure Marketi'nden. [Çözümleri kaldırmak](../monitoring/monitoring-solutions.md#remove-a-management-solution) yüklü çözümleri listesinden.

## <a name="how-do-i-create-and-manage-alerts"></a>Nasıl oluşturmak ve Uyarıları yönetme?
Günlük analizi sorgularına dayalı uyarı kuralları şimdi yönetilir [deneyimi uyarı birleşik](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). Bkz: [uyarıları günlük Analytics'ten Azure Uyarıları ' genişletme](../monitoring-and-diagnostics/monitoring-alerts-extend-tool.md) yapılandırma ve Azure portalında uyarılar kullanma hakkında bilgi.

## <a name="how-do-i-access-my-dashboards"></a>My panoları nasıl erişirim?
[Panolar](../log-analytics/log-analytics-dashboards.md) günlük analizi kullanım dışı bırakıldı.  Günlük analizi kullanarak verileri Görselleştir [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) ek işlevler ve PIN sorgu ve Azure Pano görünümlerine sahiptir.

## <a name="how-do-i-check-my-usage"></a>Benim kullanımım nasıl kontrol edilsin mi?
Şimdi kolayca görüntüleyebilir ve kullanım ve günlük analizi maliyetini seçerek yönetmek **kullanım ve tahmini maliyetleri** çalışma alanınızdaki.

![Kullanım ve tahmini maliyetler](media/log-analytics-new-portal/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Klasik Portalı'nı kullanabilir miyim?
Sınırlı bir süre için portal bu URL, kendi çalışma alanı adı ile erişmeye devam edebilirsiniz: https://\<çalışma alanı adınız\>. portal.mms.microsoft.com. Ancak Azure portalını kullanarak önerilir ve bize geri bildirim sırasında sağlamak LAUpgradeFeedback@microsoft.com herhangi bir engelleyici soruna üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

- [Bulma ve yönetim çözümleri yükleme](../monitoring/monitoring-solutions.md) Azure portalını kullanarak.
- Hakkında bilgi edinin [Azure portalında günlük arama](log-analytics-log-search-portals.md).