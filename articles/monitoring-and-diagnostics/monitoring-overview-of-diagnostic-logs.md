---
title: Azure tanılama günlükleri'ne genel bakış | Microsoft Docs
description: Azure tanılama günlüklerini nelerdir ve bir Azure kaynağı içinde gerçekleşen olayların anlamak için bunları nasıl kullanacağınızı öğrenin.
author: johnkemnetz
manager: orenr
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: johnkem; magoedte
ms.openlocfilehash: 7d1ab75146c9899bf2699309cd5dd4ed523096ef
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638815"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma

## <a name="what-are-azure-resource-diagnostic-logs"></a>Azure kaynak tanılama günlüklerini nelerdir

**Azure kaynak düzeyi tanılama günlüklerini** olan bir kaynak tarafından gösterilen günlükleri, o kaynak işlemi hakkında zengin, sık veriler sağlar. Bu günlükler içeriğini kaynak türüne göre değişir. Örneğin, ağ güvenlik grubu kural sayaçları ve anahtar kasası denetimleri kaynak günlükleri iki kategorisi vardır.

Kaynak düzeyi tanılama günlükleri farklı [etkinlik günlüğü](monitoring-overview-activity-logs.md). Etkinlik günlüğü kaynakları Kaynak Yöneticisi'ni kullanarak, örneğin, bir sanal makine oluşturma veya bir mantıksal uygulama silme gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüğü bir abonelik düzeyi günlüğü bulunmaktadır. Kaynak düzeyi tanılama günlükleri, bu kaynak içinde kendisi, örneğin, gizli bir anahtar Kasası'nı alma gerçekleştirilen işlemler hakkında bilgi sağlar.

Kaynak düzeyi tanılama günlüklerini de konuk işletim sistemi düzeyinde tanılama günlükleri farklılık gösterir. Konuk işletim sistemi tanılama günlüklerini bu sanal makine içinde çalışan bir aracının tarafından toplanan veya diğer kaynak türü desteklenir. Konuk işletim sistemi düzeyinde tanılama günlüklerini işletim sistemi ve sanal makine üzerinde çalışan uygulamalardan veri yakalama işlemi sırasında kaynak düzeyi tanılama günlüklerini Azure platformu kendisini hiçbir aracı ve yakalama kaynak özgü veri gerektirir.

Tüm kaynakları kaynak tanılama günlüklerini burada açıklanan yeni türünü desteklemez. Bu makale, hangi kaynak türlerinin yeni kaynak düzeyi tanılama günlüklerini destek listeleyen bir bölüm içerir.

![Kaynağın tanılama günlükleri diğer türleri vs günlükleri ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Kaynak düzeyi tanılama günlükleri ile yapabilecekleriniz
Kaynağın tanılama günlükleri ile yapabileceği şeylerden bazıları şunlardır:

![Kaynağın tanılama günlüklerinin mantıksal yerleştirme](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)

* Bunları kaydetmek bir [ **depolama hesabı** ](monitoring-archive-diagnostic-logs.md) denetim veya el ile İnceleme için. Bekletme süresi (gün) kullanarak belirtebilirsiniz **kaynak tanılama ayarlarını**.
* [Bunları akış **Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
* Bunları ile analiz [günlük analizi](../log-analytics/log-analytics-azure-storage.md)

Bir depolama hesabı veya günlükleri yayma biri ile aynı abonelikte değil olay hub'ları ad alanı kullanabilirsiniz. Ayar yapılandıran kullanıcının uygun RBAC her iki aboneliğin erişiminiz olmalıdır.

## <a name="resource-diagnostic-settings"></a>Kaynak tanılama ayarları

Kaynağın tanılama günlüklerini olmayan-kaynakları kaynak tanılama ayarları kullanılarak yapılandırılmış olan işlem. **Kaynak tanılama ayarlarını** kaynak denetimi için:

* Kaynağın tanılama günlüklerini ve ölçümleri (depolama hesabı, olay hub'ları ve/veya günlük analizi) gönderildiği.
* Günlük kategorilerini gönderilir ve ölçüm verileri de gönderilip.
* Her günlük kategori bir depolama hesabında ne kadar süre tutulacağını
    - Sıfır gün bekletme günlükleri sonsuza kadar tutulur anlamına gelir. Aksi takdirde, değer 1 ile 2147483647 arasındaki gün herhangi bir sayıda olabilir.
    - Bekletme ilkeleri ayarlanır, ancak yalnızca (örneğin, olay hub'ları veya günlük analizi seçenekler seçilidir) günlükleri bir depolama hesabında depolama devre dışı bırakıldı, bekletme ilkeleri bir etkisi yoktur.
    - Bekletme ilkeleri uygulanan başına günlük, olduğundan, bir gün (UTC) dışında tutma sunulmuştur gün günlüklerinden sonunda İlkesi silinir. Örneğin, bir günlük bir Bekletme İlkesi nesneniz varsa, günün bugün başında dünden önceki gün günlüklerinden silinecek. Silme işlemi gece yarısı UTC ancak günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir Not başlar.

Bu ayarlar Azure portalında bir kaynak için tanılama ayarları aracılığıyla, CLI komutları ve Azure PowerShell aracılığıyla veya aracılığıyla kolayca yapılandırılır [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

> [!WARNING]
> Tanılama günlüklerini ve konuk işletim sistemi katmanından işlem kaynakları (örneğin, VM'ler veya Service Fabric) kullanım ölçümleri [yapılandırması ve çıkışları seçimi için ayrı bir mekanizma](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Kaynağın tanılama günlüklerini koleksiyonunu etkinleştirme

Kaynağın tanılama günlüklerini koleksiyonunu etkinleştirilebilir [bir Resource Manager şablonunda kaynak oluştururken bir parçası olarak](./monitoring-enable-diagnostic-logs-using-template.md) veya bir kaynak Portalı'nda bu kaynağın sayfasından oluşturulduktan sonra. Azure PowerShell veya CLI komutları kullanarak ya da Azure İzleyici REST API'sini kullanarak herhangi bir noktada koleksiyonu de etkinleştirebilirsiniz.

> [!TIP]
> Bu yönergeleri doğrudan her kaynak için geçerli olmayabilir. Bazı kaynak türleri için geçerli olmayabilir özel adımlarını anlamak için bu sayfanın sonundaki şema bağlantılara bakın.

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Portaldaki kaynak tanılama günlükleri toplamayı etkinleştir

Bir kaynak için belirli bir kaynak giderek ya da Azure İzleyicisi gezinme oluşturulduktan sonra kaynak tanılama günlüklerini Azure portalında koleksiyonunu etkinleştirebilirsiniz. Bu Azure izleyicisini etkinleştirmek için:

1. İçinde [Azure portal](http://portal.azure.com), Azure izleyicisine gidin ve tıklayın **tanılama ayarları**

    ![Azure İzleyicisi İzleme bölümü](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türü listeyi filtrelemek sonra tanılama ayarını ayarlamak istediğiniz kaynak'ı tıklatın.

3. Hiçbir ayar kaynağı varsa, sizden istenir bir ayar oluşturmak için seçtiğiniz. "Tanılamayı açın."'i tıklatın

   ![Tanılama ayarını - ayar Ekle](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Kaynak üzerinde var olan ayarları varsa, bu kaynak üzerinde zaten yapılandırılmış ayarları listesini görürsünüz. "Tanılama ayarını Ekle" yi tıklatın.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Ayar bir adı verin, veri göndermek ve hangi kaynak her hedefi için kullanılan yapılandırmak istediğiniz her hedef için kutuları işaretleyin. İsteğe bağlı olarak, bu günlükler kullanarak korumak için gün sayısını ayarlayın **bekletme (gün)** kaydırıcılar (yalnızca depolama hesabı hedef için geçerlidir). Sıfır gün bekletme günlükleri süresiz olarak depolar.

   ![Tanılama ayarını ayarlar varolan - Ekle](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni bir ayar bu kaynak için ayarları listesi görüntülenir ve yeni olay verilerini oluşturulan hemen tanılama günlüklerini belirtilen hedefe gönderilir.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>PowerShell aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştir

Azure PowerShell aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştirmek için aşağıdaki komutları kullanın:

Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için bu komutu kullanın:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

Depolama hesabı kimliği günlükleri göndermek istediğiniz depolama hesabını kaynak kimliğidir.

Bir olay hub'ına tanılama günlüklerini akışını etkinleştirmek için bu komutu kullanın:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

Hizmet veri yolu kural kimliği bu biçiminde bir dizedir: `{Service Bus resource ID}/authorizationrules/{key name}`.

Günlük analizi çalışma alanı için tanılama günlüklerinin gönderilmesine izin vermek için bu komutu kullanın:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Aşağıdaki komutu kullanarak, günlük analizi çalışma alanı kaynak Kimliğini elde edebilirsiniz:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Birden çok çıktı seçenekleri etkinleştirmek için bu parametreler birleştirebilirsiniz.

### <a name="enable-collection-of-resource-diagnostic-logs-via-azure-cli-20"></a>Azure CLI 2.0 aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştir

Azure CLI 2.0 aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştirmek için kullandığınız [az İzleyici tanılama ayarlarını oluştur](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) komutu.

Tanılama günlüklerinin bir depolama hesabındaki depolama etkinleştirmek için:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

`--resource-group` Bağımsız değişken, yalnızca gerekli olduğunda `--storage-account` bir nesne kimliği değil

Bir olay hub'ına tanılama günlüklerini akışını etkinleştirmek için:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Kural Kimliği bu biçiminde bir dizedir: `{Service Bus resource ID}/authorizationrules/{key name}`.

Günlük analizi çalışma alanı için tanılama günlüklerini göndermeye etkinleştirmek için:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

`--resource-group` Bağımsız değişken, yalnızca gerekli olduğunda `--workspace` bir nesne kimliği değil

Herhangi bir komutu ile ek kategoriler tanılama günlük olarak geçirilen JSON dizisine sözlükler ekleyerek ekleyebileceğiniz `--logs` parametresi. Birleştirebilirsiniz `--storage-account`, `--event-hub`, ve `--workspace` birden çok çıktı seçenekleri etkinleştirmek için parametreler.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>REST API aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştir

Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirmek için bkz: [bu belgeyi](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Portaldaki kaynak tanılama ayarlarını yönet

Tüm kaynaklarınız tanılama ayarları ile ayarlandığından emin olun. Gidin **İzleyici** portal ve açık **tanılama ayarlarını**.

![Tanılama günlüklerini dikey penceresinde portalı](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

"Tüm hizmetleri" öğesine tıklamanız gerekebilir izleme bölümü bulunamıyor.

Burada görebilirsiniz ve tanılaması etkin olup olmadığını görmek için tanılama ayarları destekleyen tüm kaynakları filtreleyin. Birden çok ayarları bir kaynak üzerinde ayarlanmış ve hangi depolama hesabı, olay hub'ları ad ve/veya veri akışının günlük analizi çalışma alanı denetleyin görmek için ayrıntıya inebilir.

![Tanılama günlüklerini sonuçları portalında](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Burada etkinleştir, devre dışı bırakmak veya seçilen kaynak için tanılama ayarlarınızı değiştirmek için tanılama ayarları görünümü oluşturan bir tanılama ayarı ekleme getirir.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Desteklenen hizmetler, kategoriler ve kaynak tanılama günlükleri için şemaları

[Bu makaleye bakın](monitoring-diagnostic-logs-schema.md) desteklenen Hizmetleri ve günlük kategorileri ve bu hizmetler tarafından kullanılan şemalar tam bir listesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Akış kaynak tanılama günlüklerine **olay hub'ları**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Günlük analizi ile Azure depolama biriminden günlüklerini analiz edin](../log-analytics/log-analytics-azure-storage.md)
