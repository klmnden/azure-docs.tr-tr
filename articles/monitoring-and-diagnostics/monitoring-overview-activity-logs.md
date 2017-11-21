---
title: "Azure etkinlik günlüğü'ne genel bakış | Microsoft Docs"
description: "Azure etkinlik günlüğü nedir öğrenin ve Azure aboneliğinizi içinde gerçekleşen olayların anlamak için nasıl kullanabilirsiniz."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: johnkem
ms.openlocfilehash: 4a796920d5ff76d4ff4d41afe2ec14aa89ae2265
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Azure etkinlik günlüğü ile abonelik etkinliğini izleme
**Azure etkinlik günlüğü** , Azure'da oluşan abonelik düzeyinde olaylar hakkında bilgi sağlayan bir abonelik günlüktür. Bu verileri, Azure Resource Manager işlemsel veri hizmeti sistem durumu olayları güncelleştirmeleri için bir aralığı içerir. Etkinlik günlüğü önceden aboneliklerinizi yönetim kategorisi raporları denetim düzlemi olayları itibaren "Denetim günlüklerini" veya "İşlem günlükleri," olarak bilinirdi. Etkinlik günlüğü kullanarak, belirleyebilirsiniz ' ne, kimin, ne zaman ve ' herhangi yazma işlemleri (PUT, POST, DELETE) aboneliğinizi kaynaklarında alınan için. İşleminin durumunu ve ilgili diğer özellikleri de anlayabilirsiniz. Etkinlik günlüğü okuma (GET) işlemleri veya işlemleri kullanan Klasik kaynakları için içermeyen / "RDFE" modeli.

![Etkinlik günlükleri vs günlükleri diğer türleri ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Şekil 1: Etkinlik günlükleri vs günlükleri diğer türleri

Etkinlik günlüğü farklıdır [tanılama günlükleri](monitoring-overview-of-diagnostic-logs.md). Etkinlik günlükleri dışarıdan kaynak işlemlerinin hakkında veriler sağlar ("Denetim düzlemi"). Tanılama günlüklerini bir kaynak tarafından gösterilen ve bu kaynağın ("veri düzlemi") işlemiyle ilgili bilgi sağlayın.

Olaylar, etkinlik CLI, PowerShell cmdlet'leri, Azure portalını kullanarak günlüğü alabilir ve Azure İzleyici REST API'si.


> [!WARNING]
> Azure etkinlik günlüğü öncelikle Azure Kaynak Yöneticisi'nde ortaya etkinlikleri içindir. Klasik/RDFE modeli kullanarak kaynakları izlemez. Bazı Klasik kaynak türleri proxy kaynak sağlayıcısı Azure Resource Manager (örneğin, Microsoft.ClassicCompute) sahiptir. Bir Klasik kaynak türü üzerinden Azure kaynak bu proxy kaynak sağlayıcılarını kullanarak Yöneticisi ile etkileşim işlemleri etkinlik günlüğünde görünür. Klasik portal veya Azure Resource Manager proxy'leri dışında başka bir Klasik kaynak türü ile etkileşim, eylemlerinizi yalnızca işlem günlüğüne kaydedilir. İşlem günlüğünün ayrı bir portal bölümde göz atılacak.
>
>

Etkinlik günlüğü Tanıtımı aşağıdaki videoyu izleyin.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-the-activity-log"></a>Etkinlik günlüğünde kategorileri
Etkinlik günlüğü verileri çeşitli kategorileri içerir. Bu kategoriler şemalara ilgili tam Ayrıntılar için [bu makaleye bakın](monitoring-activity-log-schema.md). Bunlar:
* **Yönetim** -bu kategorideki tüm kaydını içerir oluşturma, güncelleştirme, silme ve eylem işlemlerine Resource Manager aracılığıyla gerçekleştirilir. Bkz Bu kategoride olay türlerini örneklerindendir "sanal makine oluşturma" ve "belirli bir kaynak türü üzerinde bir işlem olarak bir kullanıcı veya Kaynak Yöneticisi'ni kullanarak uygulama tarafından gerçekleştirilen her eylem Modellenen ağ güvenlik grubunu sil". İşlem türü, yazma, silme veya eylemi ise, yönetim kategorisi kayıtları hem Başlangıç hem de başarılı veya başarısız bu işlemin kaydedilir. Yönetim kategorisi, rol tabanlı erişim denetimi yapılan değişikliklerin bir abonelikte de içerir.
* **Hizmet durumu** -bu kategoriyi Azure'da oluşan herhangi bir hizmet durumu olay kaydını içerir. Bu kategorideki görür olay türü "SQL Azure Doğu ABD kesinti yaşanıyor." örneğidir Hizmet sistem durumu olayları beş çeşit olarak gelir: eylem gerekli, Destekli kurtarma, olay, bakım, bilgi veya güvenlik ve olay için etkilenen Abonelikteki bir kaynağınız varsa yalnızca görünür.
* **Uyarı** -bu kategorideki tüm etkinleştirmeleri Azure uyarıların kaydını içerir. Bu kategorideki görür olay türü "myVM CPU % 80'den son 5 dakika için bırakıldı." örneğidir Azure sistemleri çeşitli sahip bir uyarı verme kavramı--bir kural çeşit tanımlayabilir ve bu kural için koşullara uyan bir bildirim alıyorsunuz. Her bir desteklenen Azure uyarı türü 'etkinleştirir,' veya bir bildirim oluşturmak için koşullar, bir kayıt etkinleştirme etkinlik günlüğü bu kategoriyi de gönderilir.
* **Otomatik ölçeklendirme** -bu kategori, aboneliğinizde tanımladığınız herhangi bir otomatik ölçeklendirme ayarı göre otomatik ölçeklendirme altyapısı işlemi ile ilgili olayları kaydını içerir. Bu kategorideki görür olayın türünü, "Otomatik ölçeklendirme ölçek büyütme eylemi başarısız oldu." örneğidir Otomatik ölçeklendirme'ni kullanarak, otomatik olarak ölçeğini veya ölçeklendirin desteklenen kaynak türü örneği sayısı bir otomatik ölçeklendirme ayarı kullanarak gün ve/veya yük (ölçüm) verileri zamanında temel. Zaman ölçek yukarı veya aşağı, başlangıç ve başarılı veya başarısız olaylar için koşullar kaydedilir bu kategorideki.
* **Öneri** -öneri olaylarından web siteleri ve SQL sunucuları gibi bazı kaynak türleri bu kategorisi içerir. Bu olaylar daha iyi kaynaklarınızı kullanmalarını ilişkin öneriler sunar. Öneriler yayma kaynaklarınız varsa, yalnızca bu tür olayları alırsınız.
* **Güvenlik** -Azure Güvenlik Merkezi tarafından oluşturulan tüm uyarıları kaydını bu kategorisi içerir. Bu kategorideki görür olayın türünü, "yürütülen şüpheli çift uzantısının." örneğidir
* **İlke ve kaynak durumu** -Bu kategorilerden tüm olaylar içermez; gelecekte kullanılmak üzere ayrılmıştır.

## <a name="event-schema-per-category"></a>Her kategori şeması
[Etkinlik günlüğü olay şemanın her kategori anlamak için bu makaleye bakın.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Etkinlik günlüğü ile yapabilecekleriniz
Etkinlik günlüğü ile yapabileceği şeylerden bazıları şunlardır:

![Azure etkinlik günlüğü](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Sorgulamak ve içinde görüntüleyebilir **Azure portal**.
* [Bir uyarı etkinlik günlüğü olay olarak oluşturun.](monitoring-activity-log-alerts.md)
* [Kendisine akışı bir **olay hub'ı** ](monitoring-stream-activity-logs-event-hubs.md) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
* Powerbı kullanarak Analiz [ **Power BI içerik paketi**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Kaydetmesi bir **depolama hesabı** arşivleme veya el ile İnceleme için](monitoring-archive-activity-log.md). Bekletme süresi (gün) kullanarak belirtebilirsiniz **günlük profili**.
* PowerShell Cmdlet, CLI veya REST API sorgu.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Sorgu Azure portalında etkinlik günlüğü
Azure portalı içinde çeşitli yerlerde, etkinlik günlüğü görüntüleyebilirsiniz:
* **Etkinlik günlüğü dikey**, Sol Gezinti Bölmesi'nde "Daha Hizmetleri" altında etkinlik günlüğü arayarak erişebileceğiniz.
* **İzleyici dikey**, varsayılan olarak sol gezinti bölmesinde görüntülenir. Etkinlik günlüğü bu Azure İzleyici dikey bölümüdür.
* Herhangi bir kaynağa ait **kaynak dikey**, örneğin, bir sanal makine için yapılandırma dikey. Etkinlik günlüğü, bu kaynak dikey çoğunu bölümleri biri ve otomatik olarak üzerine tıklayarak bu belirli bir kaynağa ilgili olayları filtreleyen ' dir.

Azure portalında, etkinlik günlüğü bu alanlara göre filtreleyebilirsiniz:
* TimeSpan - olaylar için başlangıç ve bitiş zamanı.
* Kategori - yukarıda açıklandığı gibi olay kategorisi.
* Abonelik - bir veya daha fazla Azure aboneliği adları.
* Kaynak grubu - bu abonelikleri içinde bir veya daha fazla kaynak gruplar.
* Kaynak (ad) - belirli bir kaynak adı.
* Kaynak türü - kaynak, örneğin, Microsoft.Compute/virtualmachines türü.
* İşlem adı - Örneğin, Microsoft.SQL/servers/Write bir Azure Resource Manager işlemin adı.
* Önem derecesi - (hata, uyarı, bilgilendirici kritik) olay önem derecesi.
* 'Arayan' ya da işlemi gerçekleştiren kullanıcı tarafından - başlatılan olay.
* Arama Aç - tüm olayları tüm alanları genelinde için bu dizeyi arar bir açık metin arama kutusu budur.

Bir filtre kümesi tanımladıktan sonra bu filtreler yeniden gelecekte uygulanmış aynı sorgulaması ihtiyacınız varsa oturumlarında kalıcı bir sorgu olarak kaydedebilirsiniz. Ayrıca, Azure Panonuzda her zaman bir belirli olayları takip etmek için bir sorgu sabitleyebilirsiniz.

"Uygulama" tıklatarak sorgunuzu çalıştırır ve tüm eşleşen olayları gösterir. Listedeki herhangi bir etkinliğe tıklayarak yanı sıra tam ham JSON o olayın olay özetini gösterir.

Daha fazla güç için tıklattığınız **günlük arama** etkinlik günlüğü verilerinizi görüntüler simgesi [günlük analizi etkinlik günlüğü analizi çözüm](../log-analytics/log-analytics-activity.md). Etkinlik günlüğü dikey günlükleri bir temel filtre/gözatma deneyimi sunar, ancak Özet, sorgu ve daha güçlü şekilde görselleştirmek günlük analizi sağlar.

## <a name="export-the-activity-log-with-a-log-profile"></a>Bir günlük profiliyle etkinlik günlüğü dışarı aktarma
A **günlük profili** nasıl etkinlik günlüğü dışarı denetler. Bir günlük profili kullanarak, aşağıdakileri yapılandırabilirsiniz:

* Etkinlik günlüğü (depolama hesabı veya olay hub'ları) burada gönderilmesi gereken
* Hangi olay kategorileri (yazma, silme, eylem) gönderilmelidir. *Günlük profillerinde ve etkinlik günlüğü olaylarını "kategorisi" anlamını farklıdır. Günlük profilinde "Kategorisi" işlem türü (yazma, silme, eylem) temsil eder. Bir etkinlik günlüğü olay kaynağı ya da (örneğin, yönetim, ServiceHealth, uyarı ve daha fazla) olay türü "kategorisi" özelliğini temsil eder.*
* Hangi bölgelerde (konumlara) aktarılması. Etkinlik günlüğünde birçok olay genel olaylar olarak eklemek "Genel" sağlayın.
* Ne kadar süreyle etkinlik günlüğü bir depolama hesabında tutulmalıdır.
    - Sıfır gün bekletme günlükleri sonsuza kadar tutulur anlamına gelir. Aksi takdirde, değer 1 ile 2147483647 arasındaki gün herhangi bir sayıda olabilir.
    - Bekletme ilkeleri ayarlanır, ancak yalnızca (örneğin, olay hub'ları veya OMS seçenekler seçilidir) günlükleri bir depolama hesabında depolama devre dışı bırakıldı, bekletme ilkeleri bir etkisi yoktur.
    - Bekletme ilkeleri uygulanan başına günlük, olduğundan, bir gün (UTC) dışında tutma sunulmuştur gün günlüklerinden sonunda İlkesi silinir. Örneğin, bir günlük bir Bekletme İlkesi nesneniz varsa, günün bugün başında dünden önceki gün günlüklerinden silinecek.

Günlükleri yayma olarak aynı abonelikte değil bir depolama hesabı veya olay hub'ı ad alanını kullanabilirsiniz. Ayar yapılandıran kullanıcının uygun RBAC her iki aboneliğin erişiminiz olmalıdır.

Bu ayarlar, etkinlik günlüğü dikey penceresinde Portalı'nda "Verme" seçeneği aracılığıyla yapılandırılabilir. Bunlar ayrıca program aracılığıyla yapılandırılabilir [Azure İzleyici REST API'sini kullanarak](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell cmdlet'leri veya CLI. Bir abonelik yalnızca bir günlük profil olabilir.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Azure portalını kullanarak günlük profillerini yapılandırma
Bir olay Hub'ına etkinlik günlüğü akışla aktarmak veya bunları Azure portalında "Export" seçeneğini kullanarak bir depolama hesabında depolamak.

1. Gidin **etkinlik günlüğü** portalın sol tarafta menüsünü kullanarak dikey.

    ![Etkinlik günlüğü portalında gidin](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Tıklatın **verme** dikey pencerenin üstündeki düğmesi.

    ![Portalında Dışarı Aktar](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Görüntülenen dikey penceresinde şunları seçebilirsiniz:  
  * olayları dışarı aktarmak istediğiniz bölgeleri
  * olayları kaydetmek istediğiniz depolama hesabı
  * Bu olaylar depolama korumak istediğiniz gün sayısı. 0 gün ayarı günlükleri sonsuza kadar saklar.
  * Bu olaylar akış için oluşturulacak bir Event Hub istediğiniz hizmet veri yolu Namespace.

     ![Etkinlik günlüğü dikey dışarı aktarma](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Tıklatın **kaydetmek** bu ayarları kaydetmek için. Ayarları olan hemen uygulanması aboneliğinize.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Azure PowerShell cmdlet'lerini kullanarak günlük profillerini yapılandırma
#### <a name="get-existing-log-profile"></a>Var olan günlük profilini Al
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Bir günlük profili ekleyin
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| Ad |Evet |Günlük profilinin adı. |
| StorageAccountId |Hayır |Etkinlik günlüğü kaydedilmesi gereken depolama hesabının kaynak kimliği. |
| serviceBusRuleId |Hayır |Hizmet veri yolu kuralı kimliği hizmet veri yolu ad alanı için oluşturduğunuz olay hub'ları sahip ister misiniz? Bu biçimde bir dize: `{service bus resource ID}/authorizationrules/{key name}`. |
| Konumlar |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgeler virgülle ayrılmış listesi. |
| retentionInDays |Evet |Hangi olayların tutulacağını için 1 ile 2147483647 arasında gün sayısı. Sıfır değeri günlükleri süresiz olarak depolar (sürekli). |
| Kategoriler |Hayır |Toplanması gereken olay kategorileri virgülle ayrılmış listesi. Olası değerler şunlardır: yazma, silme ve eylem. |

#### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Platformlar arası Azure CLI kullanarak günlük profillerini yapılandırma
#### <a name="get-existing-log-profile"></a>Var olan günlük profilini Al
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
`name` Özelliği, günlük profilinizin adı olmalıdır.

#### <a name="add-a-log-profile"></a>Bir günlük profili ekleyin
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Özellik | Gerekli | Açıklama |
| --- | --- | --- |
| ad |Evet |Günlük profilinin adı. |
| storageId |Hayır |Etkinlik günlüğü kaydedilmesi gereken depolama hesabının kaynak kimliği. |
| serviceBusRuleId |Hayır |Hizmet veri yolu kuralı kimliği hizmet veri yolu ad alanı için oluşturduğunuz olay hub'ları sahip ister misiniz? Bu biçimde bir dize: `{service bus resource ID}/authorizationrules/{key name}`. |
| Konumları |Evet |Etkinlik günlüğü olaylarını toplamak istediğiniz bölgeler virgülle ayrılmış listesi. |
| retentionInDays |Evet |Hangi olayların tutulacağını için 1 ile 2147483647 arasında gün sayısı. Sıfır değeri günlükleri süresiz olarak depolar (sürekli). |
| kategorileri |Hayır |Toplanması gereken olay kategorileri virgülle ayrılmış listesi. Olası değerler şunlardır: yazma, silme ve eylem. |

#### <a name="remove-a-log-profile"></a>Günlük profilini Kaldır
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Sonraki Adımlar
* [Etkinlik günlüğü (önceki adıyla denetim günlüklerini) hakkında daha fazla bilgi edinin](../azure-resource-manager/resource-group-audit.md)
* [Akış Event hubs'a Azure etkinlik günlüğü](monitoring-stream-activity-logs-event-hubs.md)
