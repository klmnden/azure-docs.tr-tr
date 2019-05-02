---
title: OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş için sık sorulan sorular | Microsoft Docs
description: Log Analytics kullanıcılar OMS portalından Azure portalına geçiş için sık sorulan sorulara yanıtlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: b353e3c0ed14c7ae42e6b1f2c8a92904bf0befd7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64916285"
---
# <a name="common-questions-for-transition-from-oms-portal-to-azure-portal-for-log-analytics-users"></a>OMS portalından Log Analytics kullanıcılar için Azure portalına geçiş için sık sorulan sorular
Log Analytics, OMS portalında adı verilen kendi portalı başlangıçta yapılandırmasını yönetmek ve toplanan verileri analiz etmek için kullanılır.  Bu Portalı'ndan tüm işlevselliği Azure portalında nerede geliştirilecek sürdürecektir taşındı.

Bu makalede bu geçişi yaptıran kullanıcılar için sık sorulan sorular yanıtlanmaktadır.  OMS portalında Log Analytics kullandıysanız, nasıl aynı görevleri Azure Portalı'nda gerçekleştirebileceğiniz için yanıtlarını burada bulabilirsiniz.

## <a name="do-i-need-to-migrate-anything"></a>Herhangi bir şey geçirme gerekiyor mu?
Hayır. Bu yüzden geçirilmesi gereken bir şey Log Analytics'e kendisine yapılan bir değişiklik bulunmamaktadır. Değişiyor gereken tek şey erişmek için kullandığınız arabirimidir. Aslında, aynı çalışma alanları, çözümler, görünümleri erişmek için Azure portalını kullanın ve kullandığınız aramaları bugün OMS portalında oturum.

## <a name="where-do-i-find-log-analytics-in-azure"></a>Azure'da Log Analytics nerede bulabilirim?
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.  Tıklayın **tüm hizmetleri**ve kaynak listesinde yazın **Log Analytics**. Seçin **Log Analytics** ve ardından çalışma alanınızı seçin. Çalışma alanı için Özet sayfasında görüntülenir.

![Log Analytics çalışma alanı](media/oms-portal-faq/log-analytics.png)

## <a name="how-do-i-manage-permissions"></a>İzinleri nasıl yönetebilirim?
Azure portalında Log Analytics çalışma alanınıza erişimi yoksa, izinlerinizi kullanarak yapılandırmak gereken [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md). Çalışma alanı izinlerini yönetme hakkında daha fazla bilgi için bkz [çalışma alanlarını yönetme](manage-access.md#manage-accounts-and-users). Uyarılarla ilgili izinleri yönetme hakkında daha fazla bilgi için bkz. [Azure İzleyici ile güvenlik rolleri ve izinleri ile çalışmaya başlama](../../azure-monitor/platform/roles-permissions-security.md).

## <a name="how-do-i-create-a-new-workspace"></a>Yeni bir çalışma alanı nasıl oluşturulur? 
Azure portalında çalışma alanlarını listesinden tıklayın **Ekle** çalışma alanları listesinde.  Tüm Ayrıntılar için bkz. [Azure portalında Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md).

![Genel Bakış sayfası](media/oms-portal-faq/new-workspace.png)

## <a name="where-is-my-overview-page"></a>My genel bakış sayfası nerede?
OMS portalında ana ekran döşeme çalışma alanınızı ve oluşturduğunuz özel görünümleri yüklü olan tüm yönetim çözümleri için görüntüler. Azure portalında aynı bu görünümde kullanılabilir. Çalışma alanınızda seçin **çalışma özeti**.

![Genel Bakış sayfası](media/oms-portal-faq/overview.png)

## <a name="how-do-i-open-log-search-and-view-designer"></a>Günlük araması ve Görünüm Tasarımcısı nasıl açarım?
Her ikisi de **günlük araması** ve **Görünüm Tasarımcısı** sağa sonraki için ana sayfada ve Azure portalında, çalışma alanınızın soldaki menüde kullanılabilir **genel bakış**.

## <a name="where-do-i-find-settings"></a>Ayarlarını nerede bulabilirim?
Ayarları çoğu **ayarları** OMS portalında bir bölümünü kullanılabilir **Gelişmiş ayarlar** çalışma alanı için Azure portalındaki menü.

![Gelişmiş ayarlar](media/oms-portal-faq/advanced-settings.png)

Aşağıdaki bölümlerde bulunan daha önce kullanılan ayarların nasıl erişebileceğiniz tam listesini **ayarları** OMS Portalı'nın bölümü.

### <a name="accounts"></a>Hesaplar 
Hesap ayarları aşağıdaki tabloda açıklandığı gibi farklı yerlerde Azure portalında yönetilir.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Otomasyon Hesabı | **Otomasyon hesabı** çalışma alanı için menü. |
| Azure aboneliği ve veri planı | **Fiyatlandırma katmanı** çalışma alanı için menü. |
| Kullanıcıları yönetme | Azure rol tabanlı erişmek için kullandığınız [çalışma alanınız için izinleri Yönet](#how-do-i-manage-permissions). |
| Çalışma Alanı Bilgileri | Kullanılabilir bilgi **OMS çalışma alanı** çalışma alanı için menü. |

### <a name="alerts"></a>Uyarılar
Uyarı kuralları log Analytics sorgularına dayalı yönetilen artık [deneyimi uyarı birleşik](#how-do-i-create-and-manage-alerts). 

### <a name="computer-groups"></a>Bilgisayar Grupları
Bilgisayar grupları yönetmek **Gelişmiş ayarlar** çalışma alanı için menü. 

### <a name="connected-sources"></a>Bağlı Kaynaklar
Çoğu bağlı kaynak ayarlarını yönetme içinde **Gelişmiş ayarlar** çalışma alanı için menü. Aşağıdaki tabloda bu menü, her bölüm için Ayrıntılar sağlar.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Windows Sunucuları   | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Linux sunucuları   | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Azure Storage     | **Gelişmiş ayarlar** çalışma alanı için menü. |
| System Center     | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Office 365        | Bkz: [belgeleri Office 365 yönetim çözümü](../insights/solution-office-365.md) yapılandırma ayrıntıları için. |
| Windows Telemetrisi | Çözüm ayarları menüsü. Bkz: [Azure portalında Windows Analytics](/windows/deployment/update/windows-analytics-azure-portal) yapılandırma ayrıntıları için. |
| ITSM Bağlayıcısı    | Bkz: [bağlanma ITSM ürünler/hizmetler BT Hizmet Yönetimi Bağlayıcısı ile](itsmc-connections.md) ITSM hizmetine Log Analytics ile ilgili yönergeler için. |

### <a name="data"></a>Veriler
Çoğu veri ayarlarını yönetme içinde **Gelişmiş ayarlar** çalışma alanı için menü. Aşağıdaki tabloda bu menü, her bölüm için Ayrıntılar sağlar.

| OMS portalında ayarlama | Azure portalında eşdeğeri |
|:---|:---|
| Windows Olay Günlükleri           | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Windows performans sayaçları | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Linux performans sayaçları   | **Gelişmiş ayarlar** çalışma alanı için menü. |
| IIS Günlükleri                     | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Özel Alanlar                | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Özel Günlükler                  | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Syslog                       | **Gelişmiş ayarlar** çalışma alanı için menü. |
| Application Insights         | Log Analytics ve Application Insights, aynı veri altyapısı paylaşmak göre bu çözümü kullanım dışıdır.  |
| Windows Dosya İzleme        | **Değişiklik izleme** Azure automation'da menüsü. Bkz: [değişiklik izleme çözümüyle ortamınızdaki Değişiklikleri İzle](../../automation/change-tracking.md) Ayrıntılar için. |
| Windows Kayıt Defteri İzleme        | **Değişiklik izleme** Azure automation'da menüsü. Bkz: [değişiklik izleme çözümüyle ortamınızdaki Değişiklikleri İzle](../../automation/change-tracking.md) Ayrıntılar için. |
| Linux Dosya İzleme          | **Değişiklik izleme** Azure automation'da menüsü. Bkz: [değişiklik izleme çözümüyle ortamınızdaki Değişiklikleri İzle](../../automation/change-tracking.md) Ayrıntılar için. |

### <a name="solutions"></a>Çözümler
Çözümleri yönetme **çözümleri** çalışma alanı için menü. 

## <a name="how-do-i-install-and-remove-management-solutions"></a>Nasıl yükleyin ve yönetim çözümleri Kaldır?
OMS portalındaki çözüm galerisinden yönetim çözümlerini yükleme ve bunları kaldırıldı **ayarları**. Azure portalında [yönetim çözümlerini yükleme](../insights/solutions.md#install-a-monitoring-solution) Azure Market'ten. [Çözümleri kaldırma](../insights/solutions.md#remove-a-monitoring-solution) yüklü çözümleri listesinden.

## <a name="how-do-i-create-and-manage-alerts"></a>Nasıl oluştururum ve Uyarıları yönetme?
Uyarı kuralları log Analytics sorgularına dayalı yönetilen artık [deneyimi uyarı birleşik](../../azure-monitor/platform/alerts-metric.md). Bkz: [uyarıları Log Analytics'ten Azure uyarılarına genişletecektir genişletme](../../azure-monitor/platform/alerts-extend-tool.md) yapılandırma ve Azure portalında uyarıları kullanma hakkında bilgi.

## <a name="how-do-i-access-my-dashboards"></a>Panolarım nasıl erişim sağlanır?
**Panom'u** Log analytics'te özelliği kullanımdan kaldırıldı. Bu özellik, bir özel Görünüm Tasarımcısı bölümleri koleksiyonunuz için izin verilen ve yerleşik Azure Panosu işlevselliğe göre almıştır. Log Analytics kullanarak verileri görselleştirmek devam [Görünüm Tasarımcısı](view-designer.md) paylaşılan görünümler için. Görsel öğeler bu görünümden ya da için de sabitleyebilirsiniz [bireysel sorguya](../learn/tutorial-logs-dashboards.md) Azure panolara.

## <a name="how-do-i-check-my-usage"></a>Kullanımım nasıl kontrol edebilirim?
Artık kolayca görüntüleyebilir ve Log Analytics, maliyet ve kullanım seçerek yönetme **kullanım ve Tahmini maliyetler** çalışma alanınızdaki.

![Kullanım ve tahmini maliyetler](media/oms-portal-faq/usage.png)


## <a name="can-i-still-use-the-classic-portal"></a>Klasik portalı kullanabilir miyim?
Sınırlı bir süre için portal bu URL, kendi çalışma alanı adı ile erişmeye devam edebilirsiniz: https://\<çalışma alanınızın adının\>. portal.mms.microsoft.com. Ancak Azure portalını kullanmanızı öneririz ve bize geri bildirim sırasında sağlamak LAUpgradeFeedback@microsoft.com herhangi bir engelleyici soruna üzerinde.

## <a name="next-steps"></a>Sonraki adımlar

- [Bulma ve yönetim çözümlerini yükleme](../insights/solutions.md) Azure portalını kullanarak.
- Hakkında bilgi edinin [Azure portalında günlük araması](../log-query/portals.md).
