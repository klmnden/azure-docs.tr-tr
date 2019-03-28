---
title: Azure tanılama günlükleri'ne genel bakış
description: Azure tanılama günlükleri nelerdir ve bir Azure kaynağı içinde gerçekleşen olaylar anlamak için bunları nasıl kullanabileceğinizi öğrenin.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 890f2224a4053ec8cad65b44b85eab0e31be3b64
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519400"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Toplama ve Azure kaynaklarınızdan günlük verilerini kullanma

## <a name="what-are-azure-monitor-diagnostic-logs"></a>Azure İzleyici tanılama günlükleri nelerdir

**Azure İzleyici tanılama günlükleri** olan hizmet işlemiyle ilgili zengin, sık sık veri sağlayan bir Azure hizmeti tarafından günlüklerdir. Azure İzleyici, kullanılabilir iki tür Tanılama Günlüğü yapar:
* **Kiracı günlükleri** -Azure Active Directory günlükleri gibi mevcut bir Azure aboneliği dışında Kiracı düzeyi hizmetler bu günlükleri gelir.
* **Kaynak günlükleri** -Bu günlükleri, ağ güvenlik grupları veya depolama hesapları gibi bir Azure aboneliğinde kaynakları dağıtma, Azure hizmetlerinden gelir.

    ![Kaynak tanılama günlükleri diğer türleri vs günlükleri](./media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

Bu günlüklerin içeriği, Azure hizmeti ve kaynak türüne göre değişir. Örneğin, ağ güvenliği Grup Kuralı sayaçları ve Key Vault denetimleri iki, tanılama günlüğü türleridir.

Bu günlükler farklı [etkinlik günlüğü](activity-logs-overview.md). Etkinlik günlüğü, Kaynak Yöneticisi'ni kullanarak, örneğin, bir sanal makine oluştururken veya bir mantıksal uygulama siliniyor, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüğünün abonelik düzeyinde günlüktür. Kaynak düzeyinde tanılama günlükleri, kaynağının içinde Örneğin, Key Vault'tan bir gizli dizi alma gerçekleştirilen işlemler hakkında bilgi sağlar.

Bu günlükler de konuk işletim sistemi düzeyinde tanılama günlüklerinden farklıdır. Konuk işletim sistemi tanılama günlükleri, bu sanal makine içinde çalışan bir aracının tarafından toplanan veya diğer desteklenen kaynak türü ' dir. Konuk işletim sistemi düzeyinde tanılama günlükleri, işletim sistemi ve bir sanal makinede çalışan uygulamalardan veri yakalama işlemi sırasında kaynak düzeyinde tanılama günlükleri, Azure platformu kendisine hiçbir aracı ve yakalama kaynağa özgü veri gerektirir.

Tüm hizmetler burada açıklanan tanılama günlükleri'ni destekler. [Bu makalede hangi hizmetlerin Desteği Tanılama günlükleri bir bölüm listesi içeriyor](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>Tanılama günlükleri ile yapabilecekleriniz
Tanılama günlükleri ile yapabileceklerinizden bazıları şunlardır:

![Mantıksal yerleşimini tanılama günlükleri](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Kaydetmek için bir [ **depolama hesabı** ](../../azure-monitor/platform/archive-diagnostic-logs.md) denetim veya el ile İnceleme. Bekletme süresi (gün cinsinden) kullanarak belirtebilirsiniz **kaynak tanılama ayarlarını**.
* [Bunları Stream **Event Hubs** ](diagnostic-logs-stream-event-hubs.md) alımı üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
* Bunları analiz [Azure İzleyici](../../azure-monitor/platform/collect-azure-metrics-logs.md), verileri hemen Azure İzleyici ile ilk veri depolama alanına yazmaya gerek yazıldığı.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Günlükleri yayan biri ile aynı abonelikte değil Event Hubs ad alanı veya bir depolama hesabını kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneliğin uygun RBAC erişiminiz olması gerekir.

> [!NOTE]
>  Şu anda, ağ akışı günlükleri güvenli bir sanal ağda olduğu bir depolama hesabına arşivlenemiyor.

## <a name="diagnostic-settings"></a>Tanılama ayarları

Kaynak tanılama günlükleri, kaynak tanılama ayarlarını kullanarak yapılandırılır. Kiracı tanılama günlükleri, Kiracı tanılama ayarı kullanılarak yapılandırılır. **Tanılama ayarları** hizmet denetimi için:

* (Depolama hesabı, olay hub'ları ve/veya Azure İzleyici) burada tanılama günlükleri ve ölçümleri gönderilir.
* Ölçüm verilerini de gönderilip ve hangi günlük kategorileri gönderilir.
* Bir depolama hesabında günlük kategorileri ne kadar süre tutulacağını
    - Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 365 arasında olabilir.
    - Bekletme ilkeleri ayarlayın, ancak yalnızca (örneğin, Event Hubs veya Log Analytics seçeneği seçili) günlükleri bir depolama hesabında depolama devre dışı, bekletme ilkeleri bir etkisi yoktur.
    - Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir. Gece yarısı UTC, ancak bu günlükleri depolama hesabınızdan silinecek 24 saate kadar sürebilir not silme işlemi başlar.

Bu ayarlar portal, Azure PowerShell ve CLI komutlarıyla veya kullanarak tanılama ayarlarını kolayca yapılandırılır [Azure İzleyici REST API](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Tanılama günlükleri koleksiyonunu etkinleştirme

Tanılama günlükleri koleksiyonunu etkinleştirilebilir [bir kaynak bir Resource Manager şablonu oluşturma işleminin parçası olarak](./../../azure-monitor/platform/diagnostic-logs-stream-template.md) veya o kaynağın sayfasından portalında bir kaynak oluşturulduktan sonra. Azure PowerShell veya CLI komutlarını kullanarak veya Azure İzleyici REST API'sini kullanarak herhangi bir noktada koleksiyonu da etkinleştirebilirsiniz.

> [!TIP]
> Bu yönergeler, doğrudan her kaynak için geçerli olmayabilir. Belirli kaynak türlerine uygulanabilir özel adımları anlamak için bu sayfanın alt kısmındaki şema bağlantılara bakın.

### <a name="enable-collection-of-diagnostic-logs-in-the-portal"></a>Tanılama günlükleri portalında koleksiyonunu etkinleştir

Belirli bir kaynağa giderek veya Azure İzleyicisi'ne giderek bir kaynak oluşturulduktan sonra Azure portalında kaynak tanılama günlükleri koleksiyonunu etkinleştirebilirsiniz. Azure İzleyici aracılığıyla bunu etkinleştirmek için:

1. İçinde [Azure portalında](https://portal.azure.com)Azure İzleyicisi'ne gidin ve tıklayarak **tanılama ayarları**

    ![Azure İzleyicisi İzleme](media/diagnostic-logs-overview/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türe göre listeyi filtreleyin ve kaynağın tanılama ayarını ayarlamak istediğiniz'i tıklatın.

3. Hiçbir ayar kaynağı varsa, istenen bir ayar oluşturmak için seçtiğiniz. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/diagnostic-logs-overview/diagnostic-settings-none.png)

   Kaynakta mevcut ayarları varsa, bu kaynakta zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-overview/diagnostic-settings-multiple.png)

3. Ayar bir adı verin, veri göndermek ve her hedef için hangi kaynağın kullanılacağını yapılandırmak istediğiniz her hedef için onay kutularını işaretleyin. İsteğe bağlı olarak, bu günlükleri kullanarak saklanacağı gün sayısını ayarlayın **bekletme (gün)** kaydırıcıları (yalnızca depolama hesabı hedef için geçerlidir). Bekletme günü sayısının sıfır günlükler süresiz olarak depolar.

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-overview/diagnostic-settings-configure.png)

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve oluşturulan yeni olay verilerini hemen sonra tanılama günlüklerini belirtilen hedefe gönderilir.

Kiracı tanılama ayarları yalnızca Kiracı hizmeti - portal dikey penceresinde bu ayarlar Azure İzleyici tanılama ayarları dikey penceresinde görünmez yapılandırılabilir. Örneğin, Azure Active Directory denetim günlüklerini tıklayarak yapılandırılmış **veri dışa aktarma ayarları** denetim günlükleri dikey penceresinde.

![AAD tanılama ayarları](./media/diagnostic-logs-overview/diagnostic-settings-aad.png)

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Kaynak tanılama günlükleri PowerShell aracılığıyla koleksiyonunu etkinleştir

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure PowerShell aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştirmek için aşağıdaki komutları kullanın:

Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için bu komutu kullanın:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

Depolama hesabı kimliği günlükleri göndermek istediğiniz depolama hesabı için kaynak kimliğidir.

Olay hub'ına tanılama günlüklerinin akışını etkinleştirmek için bu komutu kullanın:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

Service bus kural kimliği şu biçime sahip bir dizedir: `{Service Bus resource ID}/authorizationrules/{key name}`.

Tanılama günlüklerinin bir Log Analytics çalışma alanına gönderilmesine izin vermek için bu komutu kullanın:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Aşağıdaki komutu kullanarak Log Analytics çalışma alanınızın kaynak kimliği elde edebilirsiniz:

```powershell
(Get-AzOperationalInsightsWorkspace).ResourceId
```

Birden çok çıkış seçeneği etkinleştirmek için şu parametreleri birleştirebilirsiniz.

Şu anda, Azure PowerShell kullanarak Kiracı tanılama ayarlarını yapılandıramazsınız.

### <a name="enable-collection-of-resource-diagnostic-logs-via-the-azure-cli"></a>Kaynak tanılama günlükleri, Azure CLI aracılığıyla koleksiyonunu etkinleştir

Azure CLI aracılığıyla kaynak tanılama günlükleri toplamayı etkinleştirmek için kullandığınız [az İzleyici diagnostic-settings oluşturma](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) komutu.

Tanılama günlükleri bir depolama hesabında depolama etkinleştirmek için:

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

`--resource-group` Bağımsız değişken, yalnızca gerekli if `--storage-account` bir nesne kimliği değil.

Olay hub'ına tanılama günlüklerinin akışını etkinleştirmek için:

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

Kural Kimliği şu biçime sahip bir dizedir: `{Service Bus resource ID}/authorizationrules/{key name}`.

Tanılama günlüklerinin bir Log Analytics çalışma alanına gönderme etkinleştirmek için:

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

`--resource-group` Bağımsız değişken, yalnızca gerekli if `--workspace` bir nesne kimliği değil

Herhangi bir komutu ile ek kategoriler için tanılama günlük olarak geçirilen JSON dizisi sözlükleri ekleyerek ekleyebileceğiniz `--logs` parametresi. Birleştirebilirsiniz `--storage-account`, `--event-hub`, ve `--workspace` birden çok çıkış seçeneği etkinleştirmek için parametreleri.

CLI kullanarak Kiracı tanılama ayarları şu anda yapılandıramazsınız.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Kaynak tanılama günlükleri REST API aracılığıyla koleksiyonunu etkinleştir

Azure İzleyici REST API'sini kullanarak tanılama ayarlarını değiştirmek için bkz [bu belgeyi](https://docs.microsoft.com/rest/api/monitor/).

Azure İzleyici REST API'sini kullanarak Kiracı tanılama ayarları şu anda yapılandıramazsınız.

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Portalda kaynak tanılama ayarlarını yönetme

Tüm kaynaklarınızı tanılama ayarlarını ayarlandığından emin olun. Gidin **İzleyici** portalı ve açık **tanılama ayarları**.

![Portalında tanılama günlükleri dikey penceresi](./media/diagnostic-logs-overview/diagnostic-settings-nav.png)

"Tüm hizmetler" tıklamanız gerekebilir izleme bölümü bulunamıyor.

Burada görüntüleyebilir ve tanılaması etkin olup olmadığını görmek için tanılama ayarları destekleyen tüm kaynak filtreleyin. Ayarları birden çok kaynak üzerinde ayarlanmış olan ve hangi depolama hesabı, Event Hubs ad alanı ve/veya veri akışını Log Analytics çalışma alanını kontrol edin da görmek için detaya gidebilirsiniz.

![Tanılama günlükleri sonuçlarını portalında](./media/diagnostic-logs-overview/diagnostic-settings-blade.png)

Tanılama ayarı ekleme nerede etkinleştirebilir, devre dışı bırakmak veya seçili kaynak için tanılama ayarlarınızı değiştirme tanılama ayarları görünümü getirir.

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Desteklenen hizmetler, kategoriler ve şemalar tanılama günlükleri

[Bu makaleye bakın](../../azure-monitor/platform/diagnostic-logs-schema.md) desteklenen hizmetler ve günlük kategorileri ve bu hizmetler tarafından kullanılan şemalar tam listesi için.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak tanılama günlükleri için Stream **olay hub'ları**](diagnostic-logs-stream-event-hubs.md)
* [Azure İzleyici REST API'sini kullanarak kaynak tanılama ayarlarını değiştirme](https://docs.microsoft.com/rest/api/monitor/)
* [Azure İzleyici ile Azure depolama biriminden günlüklerini çözümleme](collect-azure-metrics-logs.md)
