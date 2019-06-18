---
title: Azure etkinlik günlüğü'ne genel bakış
description: Azure etkinlik günlüğü ' ne olduğunu öğrenin ve Azure aboneliğinizde gerçekleşen olaylar anlamak için nasıl kullanabilirsiniz.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 6fc00bf0dfb83f349da91989a579f31be2027ff0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071679"
---
# <a name="overview-of-azure-activity-log"></a>Azure etkinlik günlüğü'ne genel bakış

**Azure etkinlik günlüğü** oluşan Azure'da abonelik düzeyindeki olayların hakkında Öngörüler sağlar. Bu verileri, hizmet durumu olayları güncelleştirmelerinin Azure Resource Manager işletimsel verileri içerir. Etkinlik günlüğü önceden olarak biliniyordu _denetim günlüklerini_ veya _işlem günlüklerini_, Abonelikleriniz için Denetim düzlemi olayları yönetim kategorisi raporları olduğundan. 

Etkinlik günlüğü belirlemek için kullanın _ne_, _kimin_, ve _olduğunda_ işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen herhangi bir yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz. 

Etkinlik günlüğünü okuma (GET) işlemlerini ya da Klasik/RDFE modeli kullandığınız kaynaklar için işlemleri içermez.

## <a name="comparison-to-diagnostic-logs"></a>Tanılama günlükleri ile karşılaştırma
Her Azure aboneliği için tek bir etkinlik günlüğü yoktur. Bir dış kaynaktan işlemleri hakkında veri sağlar ("Denetim düzlemi"). [Tanılama günlükleri](diagnostic-logs-overview.md) bir kaynak tarafından gönderilir ve bu kaynağın ("veri düzlemi") işlemi hakkında bilgi sağlar. Her kaynak için tanılama ayarını etkinleştirmeniz gerekir.

![Tanılama günlükleri karşılaştırıldığında etkinlik günlükleri](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)


> [!NOTE]
> Azure Kaynak Yöneticisi'nde gerçekleşen etkinlikler için öncelikle Azure etkinlik günlüğü içindir. Klasik/RDFE modeli kullanarak kaynakları izlemez. Bazı Klasik kaynak türleri bir proxy kaynak sağlayıcısı, Azure Resource Manager (örneğin, Microsoft.ClassicCompute) sahip. Bir Klasik kaynak türü aracılığıyla Azure kaynak bu proxy kaynak sağlayıcılarını kullanarak Manager ile etkileşim işlemleri etkinlik günlüğü'nde görünür. Azure Resource Manager proxy'leri dışında bir Klasik kaynak türü ile etkileşim, eylemlerinizi yalnızca işlem günlüğüne kaydedilir. İşlem günlüğü portal'ın ayrı bir bölümde gözatılabilir.

## <a name="activity-log-retention"></a>Etkinlik günlük tutma
Oluşturulduktan sonra etkinlik günlüğü girdileri değiştirilmez veya sistem tarafından silindi. Ayrıca, bunları arabiriminde veya programlama yoluyla değiştiremezsiniz. Etkinlik günlüğü olayları 90 gün boyunca saklanır. Uzun süreler boyunca bu verileri depolamak için [Azure İzleyici'de toplama](activity-log-collect.md) veya [depolama veya olay hub'larına dışarı](activity-log-export.md).

## <a name="view-the-activity-log"></a>Etkinlik günlüğünü görüntüleyin
Tüm kaynaklardan için Etkinlik günlüğünü görüntüleyin **İzleyici** Azure portalındaki menü. Belirli bir kaynak için Etkinlik günlüğünü görüntüleyin **etkinlik günlüğü** o kaynağın menü seçeneği. Ayrıca, PowerShell, CLI veya REST API ile etkinlik günlüğü kayıtlarını alabilir.  Bkz: [görüntüleyin ve alma Azure etkinlik günlüğü olaylarının](activity-log-view.md).

![Etkinlik Günlüğü Görüntüle](./media/activity-logs-overview/view-activity-log.png)

## <a name="collect-activity-log-in-azure-monitor"></a>Azure İzleyicisi'nde etkinlik günlüğü toplayın
Azure İzleyici ile diğer izleme verilerinin analiz etmek ve 90 günden daha uzun süre saklamak için bir Log Analytics çalışma alanına etkinlik günlüğü toplayın. Bkz: [toplayıp Azure İzleyici'de Log Analytics çalışma alanında Azure etkinlik günlüklerini analiz](activity-log-collect.md).

![Sorgu etkinlik günlüğü](./media/activity-logs-overview/query-activity-log.png)

## <a name="export-activity-log"></a>Etkinlik günlüğünü dışarı aktarma
Etkinlik günlüğünü arşivleme için Azure Depolama'ya dışarı aktarma veya bu alımı için olay Hub'ına bir üçüncü taraf hizmeti veya özel bir analiz çözümü tarafından akış. Bkz: [Azure etkinlik günlüğü dışarı aktarma](activity-log-export.md). Ayrıca Power BI'ı kullanarak Etkinlik günlüğü olayları analiz [ **Power BI içerik paketi**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

## <a name="alert-on-activity-log"></a>Etkinlik günlüğü Uyarısı
İle etkinlik günlüğünde belirli olaylar oluşturulduğunda bir uyarı oluşturabileceğiniz bir [etkinlik günlüğü Uyarısı](activity-log-alerts.md). Bir uyarı kullanarak da oluşturabilirsiniz bir [günlük sorgusu](alerts-log-query.md) ne zaman etkinlik günlüğünüzü bir Log Analytics çalışma alanına bağlı, ancak oturum bir maliyeti yoktur uyarılar sorgu. Etkinlik günlüğü uyarıları için herhangi bir maliyet yoktur.

## <a name="categories-in-the-activity-log"></a>Etkinlik günlüğünde kategorileri
Etkinlik günlüğü'ndeki her olay aşağıdaki tabloda açıklanan belirli bir kategoriye sahiptir. Bu kategorilerin şemaların hakkında tam Ayrıntılar için bkz: [Azure etkinlik günlüğü olay şeması](activity-log-schema.md). 

| Kategori | Açıklama |
|:---|:---|
| Yönetim | Tüm kaydı içeren oluşturma, güncelleştirme, silme ve eylem işlemlerine Resource Manager aracılığıyla gerçekleştirilir. Yönetici olayları örnekler _sanal makine oluşturma_ ve _ağ güvenlik grubunu Sil_.<br><br>Belirli bir kaynak türü üzerinde bir işlem olarak bir kullanıcı ya da Resource Manager kullanarak uygulama tarafından gerçekleştirilen her eylemi modellenir. İşlem türü ise _yazma_, _Sil_, veya _eylem_, kayıtları hem Başlangıç hem de başarılı veya başarısız bu işlem yönetim kategorisi kaydedilir. Yönetici olayları, bir abonelikte rol tabanlı erişim denetimi değişiklikleri de içerir. |
| Hizmet Durumu | Azure'da gerçekleşen tüm hizmet durumu olayları kaydını içerir. Hizmet durumu olay örneği _SQL Azure Doğu ABD kesinti yaşıyor_. <br><br>Hizmet durumu olayları beş çeşit olarak gelir: _Eylem gerekiyor_, _kurtarma Yardımlı_, _olay_, _Bakım_, _bilgi_, veya  _Güvenlik_. Olaydan etkilenen aboneliğindeki bir kaynak varsa bu olaylar yalnızca oluşturulur.
| Kaynak Durumu | Azure kaynaklarınızı gerçekleşen herhangi bir kaynak sistem durumu olayları kaydını içerir. Kaynak sistem durumu olayı örneğidir _sanal makine sistem durumu değiştiğinde kullanılamaz_.<br><br>Kaynak sistem durumu olayları dört durum durumlardan birini temsil edebilir: _Kullanılabilir_, _kullanılamaz_, _düşürülmüş_, ve _bilinmeyen_. Ayrıca, kaynak durumu olayları olacak şekilde sınıflandırılabilir _Platform başlatılan_ veya _kullanıcı tarafından başlatılan_. |
| Uyarı | Azure uyarıları için etkinleştirme kaydını içerir. Bir uyarı olayının örneğidir _myVM üzerindeki CPU % 80'den önceki 5 dakika boyunca oluştu_.|
| Otomatik Ölçeklendirme | Kayıt işlemi herhangi bir otomatik ölçeklendirme ayarı, aboneliğinizde tanımladığınız dayalı otomatik ölçeklendirme altyapısı ilgili olayların içerir. Bir otomatik ölçeklendirme olayının örneğidir _otomatik ölçek ölçeği artırma eylemi başarısız oldu_. |
| Öneri | Azure Danışmanı için öneri olaylardan içerir. |
| Güvenlik | Azure Güvenlik Merkezi tarafından oluşturulan herhangi bir uyarı kaydı içerir. Bir güvenlik olayı örneğidir _şüpheli çift uzantılı dosya yürütüldü_. |
| İlke | Azure İlkesi tarafından gerçekleştirilen tüm etkin eylem işlemlerin kayıtları içerir. İlke olaylarını örnekler _denetim_ ve _Reddet_. İlke tarafından gerçekleştirilen her eylemi, bir kaynak üzerinde bir işlem olarak modellenir. |


## <a name="next-steps"></a>Sonraki Adımlar

* [Azure etkinlik günlüğü dışarı aktarmak için bir günlük profili oluşturma](activity-log-export.md)
* [Azure etkinlik günlüğünün Event Hubs'a Stream](activity-logs-stream-event-hubs.md)
* [Arşiv Depolama'ya Azure etkinlik günlüğü](archive-activity-log.md)

