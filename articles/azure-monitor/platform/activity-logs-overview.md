---
title: Azure etkinlik günlüğü'ne genel bakış
description: Azure etkinlik günlüğü ' ne olduğunu öğrenin ve Azure aboneliğinizde gerçekleşen olaylar anlamak için nasıl kullanabilirsiniz.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: b84238e8a659358f2c065eb1533f0d21a5335d43
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59496888"
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Azure etkinlik günlüğü ile abonelik etkinliğini izleme

**Azure etkinlik günlüğü** oluşan Azure'da abonelik düzeyindeki olayların sağlayan bir abonelik günlüktür. Bu verileri, hizmet durumu olayları güncelleştirmelerinin Azure Resource Manager işletimsel verileri içerir. Etkinlik günlüğü daha önce Abonelikleriniz için yönetim kategorisi raporları denetim düzlemi olayları beri "Denetim günlüklerini" veya "Operasyonel günlükler," olarak biliniyordu. Etkinlik günlüğü'nü kullanarak belirleyebilirsiniz ' ne, kim ve ne zaman ' işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen herhangi bir yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz. Etkinlik günlüğünü okuma (GET) işlemlerini ya da kullanan Klasik kaynakları işlemlerinde içerip içermediğini / "RDFE" modeli.

![Etkinlik Günlükleri ve diğer tür günlüğü](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)

Şekil 1: Etkinlik Günlükleri ve diğer tür günlüğü

Etkinlik günlüğü farklıdır [tanılama günlükleri](diagnostic-logs-overview.md). Etkinlik günlükleri bir dış kaynaktan işlemleri hakkında veri sağlar ("Denetim düzlemi"). Tanılama günlükleri, kaynak tarafından gönderilir ve bu kaynağın ("veri düzlemi") işlemiyle ilgili bilgi sağlar.

> [!WARNING]
> Azure Kaynak Yöneticisi'nde gerçekleşen etkinlikler için öncelikle Azure etkinlik günlüğü içindir. Klasik/RDFE modeli kullanarak kaynakları izlemez. Bazı Klasik kaynak türleri bir proxy kaynak sağlayıcısı, Azure Resource Manager (örneğin, Microsoft.ClassicCompute) sahip. Bir Klasik kaynak türü aracılığıyla Azure kaynak bu proxy kaynak sağlayıcılarını kullanarak Manager ile etkileşim işlemleri etkinlik günlüğü'nde görünür. Azure Resource Manager proxy'leri dışında bir Klasik kaynak türü ile etkileşim, eylemlerinizi yalnızca işlem günlüğüne kaydedilir. İşlem günlüğü portal'ın ayrı bir bölümde gözatılabilir.
>
>

Azure portalı, CLI, PowerShell cmdlet'lerini kullanarak, etkinlik günlüğünde olayları alabilirsiniz ve Azure İzleyici REST API'si.

> [!NOTE]
> [Yeni uyarılarda](../../azure-monitor/platform/alerts-overview.md) oluşturma ve yönetme etkinlik günlük uyarısı kuralları Gelişmiş bir deneyim sunar.  [Daha fazla bilgi edinin](../../azure-monitor/platform/alerts-activity-log.md).

## <a name="categories-in-the-activity-log"></a>Etkinlik günlüğünde kategorileri
Etkinlik günlüğü birkaç veri kategorilerini içerir. Bu kategorilerin şemaların ilgili tam Ayrıntılar için [bu makaleye bakın](../../azure-monitor/platform/activity-log-schema.md). Bunlar:
* **Yönetim** -Bu kategoride tüm kaydı oluşturma, güncelleştirme, silme ve eylem işlemlerine Resource Manager aracılığıyla gerçekleştirilir. Görmek Bu kategoride olay türlerini örnekleri arasında "sanal makine oluşturma" ve "bir kullanıcı ya da Resource Manager kullanarak uygulama tarafından gerçekleştirilen her eylemi modellenmiş bir işlemi belirli bir kaynak türü olarak ağ güvenlik grubunu sil". İşlem türü, yazma, silme veya eylem ise, hem Başlangıç hem de başarılı kayıtlar veya bu işlemin başarısız yönetim kategorisi kaydedilir. Yönetim kategorisi, bir abonelikte rol tabanlı erişim denetimi değişiklikleri de içerir.
* **Hizmet durumu** -bu kategori, Azure'da ortaya çıkan tüm hizmet durumu olayları kaydını içerir. Bu kategoride göreceğiniz olay türünü "SQL Azure Doğu ABD, kesinti yaşanıyor." örneğidir Hizmet durumu olayları beş çeşit olarak bulunur: Yardımlı kurtarma, olay, bakım, bilgi veya güvenlik, gerekli, eylem ve olaydan etkilenen aboneliğindeki bir kaynak varsa yalnızca görünür.
* **Kaynak durumu** -bu kategori, Azure kaynaklarınıza ortaya çıkan herhangi bir kaynak sistem durumu olayları kaydını içerir. Bu kategoride göreceğiniz olay türünü, "sanal makine sistem durumu kullanılamaz değiştirildi." örneğidir Kaynak sistem durumu olayları dört durum durumlardan birini temsil edebilir: Kullanılabilir, yok, düşürülmüş ve bilinmeyen. Ayrıca, kaynak sistem durumu olayları, Platform tarafından başlatılan veya kullanıcı tarafından başlatılan olacak şekilde sınıflandırılabilir.
* **Uyarı** -bu kategorideki tüm etkinleştirmeleri Azure uyarıları kaydını içerir. Bu kategoride göreceğiniz olay türünü "myVM üzerindeki CPU % 80'den önceki 5 dakika boyunca bırakıldı." örneğidir Çeşitli Azure sistemleri sahip bir uyarı verme kavramı--tür bir kural tanımlamak ve bu kuralda veya ek koşullarla eşleşen bir bildirim alırsınız. Her bir desteklenen Azure uyarı türü 'etkinleştirir,' veya bir bildirim oluşturmak için koşullar karşılandığında, bir kaydı etkinleştirme etkinlik günlüğü, bu kategoriye de gönderilir.
* **Otomatik ölçeklendirme** -kayıt işlemi herhangi bir otomatik ölçeklendirme ayarı, aboneliğinizde tanımlanmış temel otomatik ölçeklendirme altyapısı ilgili olayların bu kategorisi içerir. Bu kategoride göreceğiniz olay türünü, "Otomatik ölçek ölçeği artırma eylemi başarısız oldu." örneğidir Otomatik ölçeklendirme kullanarak, otomatik olarak ölçeği genişletme veya ölçeklendirme desteklenen kaynak türü örneği sayısını gün ve/veya yük (ölçüm) verileri kullanarak bir otomatik ölçeklendirme ayarı zamanında temel. Ne zaman ölçeği artırmak veya azaltmak için başlangıç ve başarılı veya başarısız olayları koşulların kaydedilir bu kategorideki.
* **Öneri** -Azure Danışmanı için öneri olaylardan bu kategorisi içerir.
* **Güvenlik** -Azure Güvenlik Merkezi tarafından oluşturulan herhangi bir uyarı kaydı bu kategorisi içerir. Bu kategoride göreceğiniz olay türünü, "şüpheli çift uzantılı dosya yürütüldü." örneğidir
* **İlke** -işlemlerinin Azure İlkesi tarafından gerçekleştirilen tüm etkin eylem kayıtları bu kategorisi içerir. Bu kategoride göreceğiniz olay türlerini denetim ve reddetme verilebilir. İlke tarafından gerçekleştirilen her eylemi, bir kaynak üzerinde bir işlem olarak modellenir.

## <a name="event-schema-per-category"></a>Kategori başına olay şeması

[Kategori başına etkinlik günlüğü olay şeması anlamak için bu makaleye bakın.](../../azure-monitor/platform/activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Etkinlik günlüğü ile yapabilecekleriniz

Etkinlik günlüğü ile yapabileceğiniz çok şey bazıları şunlardır:

![Azure Etkinlik günlüğü](./media/activity-logs-overview/Activity_Log_Overview_v3.png)


* Sorgulamak ve içinde görüntüleyebilir **Azure portalında**.
* [Bir etkinlik günlüğü olayında uyarı oluşturun.](../../azure-monitor/platform/activity-log-alerts.md)
* [Kendisine Stream bir **olay hub'ı** ](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) alımı bir üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
* Power BI kullanarak Analiz [ **Power BI içerik paketi**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Kaydetmesi bir **depolama hesabı** arşivleme veya el ile İnceleme](../../azure-monitor/platform/archive-activity-log.md). Bekletme süresi (gün cinsinden) kullanarak belirtebilirsiniz **günlük profilini**.
* PowerShell cmdlet'i, CLI veya REST API sorgulayın.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Azure portalında etkinlik günlüğü sorgulama

Azure portalının içinde çeşitli yerlerde, etkinlik günlüğü görüntüleyebilirsiniz:
* **Etkinlik günlüğü** etkinlik günlüğü altında arayarak erişebileceğiniz **tüm hizmetleri** sol taraftaki gezinti bölmesinde.
* **İzleyici** varsayılan sol taraftaki gezinti bölmesinde görünür. Etkinlik günlüğü Azure İzleyici bölümüdür.
* Çoğu **kaynakları**, örneğin, bir sanal makine yapılandırma dikey. Etkinlik günlüğü çoğu kaynak dikey pencerelerinin üzerinde bir bölümdür ve bu belirli o kaynakla ilgili olayları otomatik olarak üzerine tıklayarak filtreler.

Azure portalında etkinlik günlüğünüz bu alanlara göre filtreleyebilirsiniz:
* TimeSpan - olaylar için başlangıç ve bitiş zamanı.
* Kategori - yukarıda açıklandığı gibi olay kategorisi.
* Abonelik - bir veya daha fazla Azure aboneliği adı.
* Kaynak grubu - bu Aboneliklerdeki içinde bir veya daha fazla kaynak grupları.
* Kaynak (ad) - belirli bir kaynağın adı.
* Kaynak türü - örneğin, Microsoft.Compute/virtualmachines kaynak türü.
* İşlem adı - Microsoft.SQL/servers/Write Örneğin, bir Azure Resource Manager işlem adı.
* Önem derecesi - (Kritik hata, uyarı, bilgilendirici) olay önem derecesi.
* 'Arayan' ya da işlemi gerçekleştiren kullanıcı - tarafından başlatılan olay.
* Açık arama - tüm olaylarda tüm alanlar boyunca o dize için arama bir açık metin arama kutusu budur.

Bir filtre kümesi tanımlandıktan sonra bir sorgu her zaman belirli olayları takip etmek için Azure panonuza sabitleyebilirsiniz.

Daha fazla güç için tıklayabilirsiniz **günlükleri** etkinlik günlüğü verilerinizi gösteren bir simge [toplayıp analiz etkinlik günlükleri çözümü](../../azure-monitor/platform/collect-activity-logs.md). Etkinlik günlüğü dikey penceresi, günlükleri, ancak Özet için sorgu ve daha güçlü şekilde görselleştirin Azure İzleyici günlükleri özelliğini etkinleştirir temel filtreleme/göz atma deneyimi sunar.

## <a name="export-the-activity-log-with-a-log-profile"></a>Günlük profilini ile Etkinlik günlüğünü dışarı aktarma

A **günlük profilini** etkinlik günlüğünüzü nasıl verilir denetimleri. Günlük profilini kullanarak, aşağıdakileri yapılandırabilirsiniz:

* Etkinlik günlüğü'nü (depolama hesabına veya olay hub'ları) nereye gönderileceğini
* Hangi olay kategorileri (yazma, silme, eylem) gönderilmelidir. *Günlük profilleri ve etkinlik günlüğü olayları "Kategori" anlamını farklıdır. Günlük profilinde "Kategori" işlem türü (yazma, silme, eylem) temsil eder. Bir etkinlik günlüğü olayında "Kategori" özelliği, kaynak veya olay (örneğin, yönetim, ServiceHealth, uyarı ve daha fazlası) türünü temsil eder.*
* Hangi bölgeler (konumlara) aktarılması. Etkinlik günlüğünde çok sayıda olayları genel olaylar olarak eklemek "Genel" sağlayın.
* Ne kadar süreyle etkinlik günlüğü, depolama hesabında tutulmalıdır.
    - Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 365 arasında olabilir.
    - Bekletme ilkeleri ayarlayın, ancak yalnızca (örneğin, Event Hubs veya Log Analytics seçeneği seçili) günlükleri bir depolama hesabında depolama devre dışı, bekletme ilkeleri bir etkisi yoktur.
    - Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar.

Günlükleri yayan bir aynı abonelikte değil bir depolama hesabına veya olay hub'ı ad alanını kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneliğin uygun RBAC erişiminiz olması gerekir.

Bu ayarlar, portalda etkinlik günlüğü dikey penceresindeki "Export" seçeneği aracılığıyla yapılandırılabilir. Bunlar aynı zamanda program aracılığıyla yapılandırılabilir [Azure İzleyici REST API'sini kullanarak](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell cmdlet'leri veya CLI. Bir abonelikte yalnızca tek bir günlük profili olabilir.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Azure portalını kullanarak günlük profillerini yapılandırma

Etkinlik günlüğü olay Hub'ına akış ya da Azure portalında "Dışarı aktarmak için Event Hub" seçeneğini kullanarak bir depolama hesabında depolayın.

1. Gidin **etkinlik günlüğü** portalın sol tarafındaki menüyü kullanarak.

    ![Portalda etkinlik günlüğüne gidin](./media/activity-logs-overview/activity-logs-portal-navigate-v2.png)
2. Tıklayın **dışarı aktarma, olay Hub'ına** dikey penceresinin üstünde düğme.

    ![Portal, Dışarı Aktar](./media/activity-logs-overview/activity-logs-portal-export-v2.png)
3. Görüntülenen dikey penceresinde şunları seçebilirsiniz:
   * olayları dışarı aktarmak istediğiniz bölgeleri
   * olayları kaydetmek istediğiniz depolama hesabı
   * Bu olaylar depolamadaki saklamak istediğiniz gün sayısı. 0 gün ayarı günlükler süresiz korur.
   * Bu olayları akış için oluşturulacak bir olay hub'ı istediğiniz hizmet veri yolu Namespace.

     ![Etkinlik günlüğü dikey dışarı aktarma](./media/activity-logs-overview/activity-logs-portal-export-blade.png)
4. Tıklayın **Kaydet** bu ayarları kaydedin. Ayarlar aboneliğinize hemen uygulanır.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'lerini kullanarak günlük profillerini yapılandırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

#### <a name="get-existing-log-profile"></a>Var olan günlük profilini Al

```powershell
Get-AzLogProfile
```

#### <a name="add-a-log-profile"></a>Günlük profilini ekleyin

```powershell
Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Name |Evet |Günlük profilinin adı. |
| StorageAccountId |Hayır |Etkinlik günlüğünün kaydedileceği depolama hesabı kaynak kimliği. |
| serviceBusRuleId |Hayır |Service Bus kural kimliği oluşturulan olay hub'ları olmasını istediğiniz Service Bus ad alanı. Şu biçime sahip bir dizedir: `{service bus resource ID}/authorizationrules/{key name}`. |
| Location |Evet |Etkinlik günlüğü olayları toplamak istiyorsanız bölgelerin virgülle ayrılmış listesi. |
| Retentionındays |Evet |Hangi olayların tutulacağını, 1 ile 2147483647 arasında bir gün sayısı. Sıfır değeri günlükler süresiz olarak depolar (sonsuz). |
| Category |Hayır |Virgülle ayrılmış liste toplanması gereken olay kategorileri. Olası değerler şunlardır: yazma, silme ve eylem. |

#### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır

```powershell
Remove-AzLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cli"></a>Azure CLI'yı kullanarak günlük profillerini yapılandırma

#### <a name="get-existing-log-profile"></a>Var olan günlük profilini Al

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

`name` Özelliği, günlük profilinin adı olmalıdır.

#### <a name="add-a-log-profile"></a>Günlük profilini ekleyin

```azurecli
az monitor log-profiles create --name <profile name> \
    --locations <location1 location2 ...> \
    --location <location> \
    --categories <category1 category2 ...>
```

CLI ile bir izleme profili oluşturmak için tam belgeleri için bkz. [CLI komut başvurusu](/cli/azure/monitor/log-profiles#az-monitor-log-profiles-create)

#### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="next-steps"></a>Sonraki Adımlar

* [Etkinlik günlüğü'nü (eski adıyla denetim günlükleri) hakkında daha fazla bilgi edinin](../../azure-resource-manager/resource-group-audit.md)
* [Azure etkinlik günlüğünün Event Hubs'a Stream](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)
