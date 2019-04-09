---
title: Azure tanılama günlükleri, ağ güvenlik grubu olayı ve kural sayaç
titlesuffix: Azure Virtual Network
description: Azure ağ güvenlik grubu için sayaç tanılama günlüklerini Olay ve kuralını etkinleştirme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: jdial
ms.openlocfilehash: f718e57e257a79a18ad4d0b6b47c10f855b6db60
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59007006"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Bir ağ güvenlik grubu tanılama günlüğüne kaydetme

Bir ağ güvenlik grubu (NSG), izin veren veya trafiği bir sanal ağ alt ağı, ağ arabirimi veya her ikisi de reddeden kuralları içerir. Tanılama günlüğü için bir NSG etkinleştirdiğinizde, bilgi aşağıdaki kategorileri günlüğe kaydedebilirsiniz:

* **olay:** Girişler için hangi NSG kuralları MAC adresini temel alarak, vm'lere uygulanır günlüğe kaydedilir. Bu kurallar durumu, 60 saniyede toplanır.
* **Kural sayacı:** Girişleri için kaç kez trafiğine izin vermek veya reddetmek için uygulanan her bir NSG kuralı içerir.

Tanılama günlükleri, yalnızca Azure Resource Manager dağıtım modeliyle dağıtılan Nsg'ler için kullanılabilir. Klasik dağıtım modeliyle dağıtılan Nsg'ler için tanılama günlüğüne kaydetme etkinleştirilemiyor. Daha iyi iki modelleri anlamak için bkz: [anlama Azure dağıtım modellerini](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tanılama günlüğüne kaydetme etkin ayrı olarak için *her* NSG için Tanılama verileri toplamak istediğiniz. İlginizi çekiyorsa işletimsel, veya etkinlik, bunun yerine günlükleri, Azure bkz [etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Kullanabileceğiniz [Azure portalı](#azure-portal), [PowerShell](#powershell), veya [Azure CLI](#azure-cli) tanılama günlük kaydını etkinleştirmek için.

### <a name="azure-portal"></a>Azure Portal

1. [Portalda](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**, Anahtar'a *ağ güvenlik grupları*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünmesini, onu seçin.
3. Günlüğe kaydetmeyi etkinleştirmek istediğiniz NSG seçin.
4. Altında **izleme**seçin **tanılama günlükleri**ve ardından **tanılamayı Aç**, aşağıdaki resimde gösterildiği gibi:

   ![Tanılamayı açma](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. Altında **tanılama ayarları**girin veya aşağıdaki bilgileri seçin ve ardından **Kaydet**:

    | Ayar                                                                                     | Değer                                                          |
    | ---------                                                                                   |---------                                                       |
    | Ad                                                                                        | Seçtiğiniz bir adı.  Örneğin: *myNsgDiagnostics*      |
    | **Bir depolama hesabında arşivle**, **Stream olay hub'ına**, ve **Log Analytics'e gönderme** | Seçtiğiniz kadar hedef seçebilirsiniz. Her hakkında daha fazla bilgi için bkz. [oturum hedefleri](#log-destinations).                                                                                                                                           |
    | GÜNLÜK                                                                                         | Ya da her ikisi de günlük kategorileri seçin. Her kategori için günlüğe kaydedilen veriler hakkında daha fazla bilgi için bkz: [günlük kategorileri](#log-categories).                                                                                                                                             |
6. Görüntüleyebilir ve günlüklerini analiz edin. Daha fazla bilgi için [görünümü ve günlüklerini çözümleme](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell kullanarak bilgisayarınızdan çalıştırırsanız, Azure PowerShell modülü, sürüm 1.0.0 gerekir veya üzeri. Çalıştırma `Get-Module -ListAvailable Az` yüklü sürümü bulmak için bilgisayarınızda. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure'a olan bir hesapla oturum açmak için [gerekli izinleri](virtual-network-network-interface.md#permissions).

Tanılama günlüğüne kaydetmeyi etkinleştirmek için mevcut bir NSG kimliği gerekir. Mevcut bir NSG yoksa biriyle oluşturabilirsiniz [yeni AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

Tanılama ile günlük etkinleştirmek istediğiniz ağ güvenlik grubu [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup). Örneğin, bir NSG alınacak adlı *myNsg* adlı bir kaynak grubunda mevcut olan *myResourceGroup*, aşağıdaki komutu girin:

```azurepowershell-interactive
$Nsg=Get-AzNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

Tanılama günlükleri için hedef üç yazabilirsiniz. Daha fazla bilgi için [oturum hedefleri](#log-destinations). Bu makalede, günlükleri gönderilen *Log Analytics* örnek olarak bir hedef. Mevcut bir Log Analytics çalışma alanıyla almak [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace). Örneğin, mevcut bir çalışma alanı almak için adlı *myWorkspace* adlı bir kaynak grubu içinde *myWorkspaces*, aşağıdaki komutu girin:

```azurepowershell-interactive
$Oms=Get-AzOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Mevcut bir çalışma alanı yoksa, biriyle oluşturabilirsiniz [yeni AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace).

Günlüğe kaydetme için günlükleri etkinleştirebilirsiniz iki kategorisi vardır. Daha fazla bilgi için [günlük kategorileri](#log-categories). NSG'yi için tanılama günlüğünü etkinleştirme [kümesi AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting). Aşağıdaki örnekte olay hem sayaç kategorisi verileri, kimlikleri NSG ve daha önce aldığınız çalışma alanı için kullanarak, bir NSG için çalışma alanına kaydeder:

```azurepowershell-interactive
Set-AzDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Yalnızca bir kategori veya diğer yerine için hem de verilerini günlüğe kaydetmek istiyorsanız, ekleme `-Categories` ardında önceki komuta seçeneği *NetworkSecurityGroupEvent* veya *NetworkSecurityGroupRuleCounter*. Farklı bir oturum istiyorsanız [hedef](#log-destinations) bir Log Analytics çalışma alanı uygun parametreleri için bir Azure kullanın [depolama hesabı](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [olay hub'ı](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Görüntüleyebilir ve günlüklerini analiz edin. Daha fazla bilgi için [görünümü ve günlüklerini çözümleme](#view-and-analyze-logs).

### <a name="azure-cli"></a>Azure CLI

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/bash), veya Azure CLI kullanarak bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. CLI bilgisayarınızdan çalıştırırsanız, sürüm 2.0.38 gerekir veya üzeri. Çalıştırma `az --version` yüklü sürümü bulmak için bilgisayarınızda. Yükseltmeniz gerekirse bkz [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest). CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` Azure'a olan bir hesapla oturum açmak için [gerekli izinleri](virtual-network-network-interface.md#permissions).

Tanılama günlüğüne kaydetmeyi etkinleştirmek için mevcut bir NSG kimliği gerekir. Mevcut bir NSG yoksa biriyle oluşturabilirsiniz [az ağ nsg oluşturma](/cli/azure/network/nsg#az-network-nsg-create).

Tanılama ile günlük etkinleştirmek istediğiniz ağ güvenlik grubu [az ağ nsg show](/cli/azure/network/nsg#az-network-nsg-show). Örneğin, bir NSG alınacak adlı *myNsg* adlı bir kaynak grubunda mevcut olan *myResourceGroup*, aşağıdaki komutu girin:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

Tanılama günlükleri için hedef üç yazabilirsiniz. Daha fazla bilgi için [oturum hedefleri](#log-destinations). Bu makalede, günlükleri gönderilen *Log Analytics* örnek olarak bir hedef. Daha fazla bilgi için [günlük kategorileri](#log-categories).

NSG'yi için tanılama günlüğünü etkinleştirme [az İzleyici diagnostic-settings oluşturma](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Aşağıdaki örnekte adlı mevcut bir çalışma alanına olay hem sayaç kategorisi veri günlükleri *myWorkspace*, adlı bir kaynak grubunda mevcut *myWorkspaces*ve aldığınız NSG kimliği daha önce:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Mevcut bir çalışma alanı yoksa, kullanarak bir tane oluşturabilirsiniz [Azure portalında](../azure-monitor/learn/quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [PowerShell](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace). Günlüğe kaydetme için günlükleri etkinleştirebilirsiniz iki kategorisi vardır.

Yalnızca bir kategori veya diğer verilerini günlüğe kaydetmek istiyorsanız, önceki komutta verileri açmaya istemediğiniz kategori kaldırın. Farklı bir oturum istiyorsanız [hedef](#log-destinations) bir Log Analytics çalışma alanı uygun parametreleri için bir Azure kullanın [depolama hesabı](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [olay hub'ı](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Görüntüleyebilir ve günlüklerini analiz edin. Daha fazla bilgi için [görünümü ve günlüklerini çözümleme](#view-and-analyze-logs).

## <a name="log-destinations"></a>Günlük hedefleri

Tanılama verilerini olabilir:
- [Bir Azure depolama hesabına yazılan](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), denetim ya da el ile İnceleme. Kaynak tanılama ayarlarını kullanarak elde tutma süresi (gün cinsinden) belirtebilirsiniz.
- [Olay hub'ına akış](../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) alımı bir üçüncü taraf hizmeti veya Power BI gibi özel analiz çözümü için.
- [Azure İzleyici günlüklerine yazılan](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Günlük kategorileri

Aşağıdaki günlük kategorileri için JSON biçimli verilerin yazılır:

### <a name="event"></a>Olay

Olay günlüğü hakkında NSG kuralları MAC adresini temel alan VM'ler, uygulanan bilgileri içerir. Aşağıdaki veriler, her bir olay günlüğe kaydedilir. Aşağıdaki örnekte, bir sanal makine IP adresi 192.168.1.4 ve 00-0D-3A-92-6A-7C bir MAC adresi için veriler günlüğe kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":"[PRIORITY-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>Kural sayacı

Kural sayacın kaynaklara uygulanan her bir kural hakkındaki bilgileri içerir. Aşağıdaki örnek verileri, kuralın uygulanacağı her zaman günlüğe kaydedilir. Aşağıdaki örnekte, bir sanal makine IP adresi 192.168.1.4 ve 00-0D-3A-92-6A-7C bir MAC adresi için veriler günlüğe kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> İletişim için kaynak IP adresini günlüğe kaydedilmez. Etkinleştirebilirsiniz [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-portal.md) için bir NSG ancak hangi günlükleri tüm kural sayaç bilgisinin yanı sıra iletişim tarafından başlatılan kaynak IP adresi. NSG akış günlüğü verileri bir Azure Depolama hesabına yazılır. İle verileri analiz edebilirsiniz [trafik analizi](../network-watcher/traffic-analytics.md) Azure Ağ İzleyicisi özelliğidir.

## <a name="view-and-analyze-logs"></a>Görüntüleme ve günlüklerini çözümleme

Tanılama günlük verilerini görüntüleme hakkında bilgi edinmek için [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tanılama verileri göndermek istediğinizde:
- **Azure İzleyici günlüklerine**: Kullanabileceğiniz [ağ güvenlik grubu analizi](../azure-monitor/insights/azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-azure-monitor
) Gelişmiş ınsights çözümü. Çözüm izin veren veya reddeden trafik, bir sanal makine ağ arabiriminin MAC adresi başına NSG kuralları için görsel öğeler sağlar.
- **Azure depolama hesabı**: Veriler bir PT1H.json dosyasına yazılır. Bulabilirsiniz:
  - Olay günlüğünde aşağıdaki yolu: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
  - Sayaç günlüğünde aşağıdaki yol kuralı: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [etkinlik günlüğü](../azure-monitor/platform/diagnostic-logs-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), daha önce denetim veya işlem günlükleri olarak bilinir. Etkinlik günlüğü, her iki Azure dağıtım modeliyle oluşturulan Nsg'ler için varsayılan olarak etkindir. Hangi işlemlerin etkinlik günlüğünde ağlardaki Nsg'ler tamamlandığını belirlemek için aşağıdaki kaynak türlerini içeren girişlerini arayın:
  - Microsoft.ClassicNetwork/networkSecurityGroups
  - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
  - Microsoft.Network/networkSecurityGroups
  - Microsoft.Network/networkSecurityGroups/securityRules
- Her bir akışa ilişkin kaynak IP adresi eklemek için tanılama bilgilerini günlüğe kaydetme hakkında bilgi edinmek için bkz. [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).