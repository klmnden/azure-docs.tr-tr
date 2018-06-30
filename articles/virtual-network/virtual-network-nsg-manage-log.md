---
title: Ağ güvenlik grubu olayı ve kural sayacı Azure tanılama günlüklerini | Microsoft Docs
description: Bir Azure ağ güvenlik grubu için sayaç tanılama günlükleri Olay ve kuralı etkinleştirmek öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: jdial
ms.openlocfilehash: 81809660bdda957eb4502e02799b9f7f5538ae51
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114032"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Bir ağ güvenlik grubu için tanılama günlüğü

Bir ağ güvenlik grubu (NSG) izin veren veya trafiği sanal ağ alt ağı, ağ arabirimi veya her ikisine de reddeden kurallar içerir. Tanılama için bir NSG günlüğü etkinleştirdiğinizde, aşağıdaki bilgi kategorileri arasında kaydedebilirsiniz:

* **Olay:** girişler için hangi NSG kuralları uygulanır MAC adresine dayalı VM'ler kaydedilir. Bu kurallar durumunun her 60 saniyede toplanır.
* **Kural sayacı:** kaç kez her NSG için içerir girişleri kural reddetmek veya trafiğine izin vermek üzere uygulanır.

Tanılama günlükleri, yalnızca Azure Resource Manager dağıtım modeli aracılığıyla dağıtılan Nsg'ler için kullanılabilir. Klasik dağıtım modeli aracılığıyla dağıtılan Nsg'ler için tanılama günlüğü etkinleştiremezsiniz. Daha iyi iki modellerinin anlamak için bkz: [anlama Azure dağıtım modelleri](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tanılama günlüğü etkin ayrı ayrı için *her* için tanılama veri toplamak istediğiniz NSG. İlginizi çekiyorsa işletimsel, veya etkinlik, günlükleri bunun yerine, Azure bkz [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Kullanabileceğiniz [Azure Portal](#azure-portal), [PowerShell](#powershell), veya [Azure CLI](#azure-cli) tanılama günlük kaydını etkinleştirmek için.

### <a name="azure-portal"></a>Azure Portal

1. [Portalda](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**, ardından *ağ güvenlik grubu*. Zaman **ağ güvenlik grupları** arama sonuçlarında görünecek, onu seçin.
3. İçin günlük kaydını etkinleştirmek istediğiniz NSG seçin.
4. Altında **izleme**seçin **tanılama günlükleri**ve ardından **tanılamayı açın**, aşağıdaki resimde gösterildiği gibi:
 
    ![Tanılamayı açma](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. Altında **tanılama ayarları**girin veya aşağıdaki bilgileri seçin ve ardından **kaydetmek**:

    | Ayar                                                                                     | Değer                                                          |
    | ---------                                                                                   |---------                                                       |
    | Ad                                                                                        | Seçtiğiniz adı.  Örneğin: *myNsgDiagnostics*      |
    | **Arşiv depolama hesabı**, **bir olay hub'ına akış**, ve **için günlük analizi Gönder** | Seçtiğiniz gibi sayıda hedefleri seçebilirsiniz. Her hakkında daha fazla bilgi için bkz: [oturum hedefleri](#log-destinations).                                                                                                                                           |
    | GÜNLÜK                                                                                         | Her ikisi de günlük kategorilerini seçin. Her kategori için günlüğe kaydedilen veriler hakkında daha fazla bilgi için bkz: [oturum kategorileri](#log-categories).                                                                                                                                             |
6. Görüntüleyin ve günlüklerini analiz edin. Daha fazla bilgi için bkz: [Görünüm ve günlüklerini analiz edin](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure bulut Kabuk ücretsiz etkileşimli kabuk ' dir. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü, sürüm 6.1.1 veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` bilgisayarınızda yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `Login-AzureRmAccount` Azure'a sahip bir hesap ile oturum açmak için [gerekli izinleri](virtual-network-network-interface.md#permissions)].

Tanılama günlük kaydını etkinleştirmek için var olan bir NSG kimliği gerekir. Varolan bir NSG yoksa, biriyle oluşturabilirsiniz [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).

Tanılama ile için günlüğü etkinleştirmek istediğiniz ağ güvenlik grubu almak [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup). Örneğin, bir NSG almak için adlı *myNsg* adlı bir kaynak grubunda var *myResourceGroup*, aşağıdaki komutu girin:

```azurepowershell-interactive
$Nsg=Get-AzureRmNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

Tanılama günlüklerini üç hedef türlerine yazabilirsiniz. Daha fazla bilgi için bkz: [oturum hedefleri](#log-destinations). Bu makalede, günlükleri gönderilen *günlük analizi* bir örnek olarak hedef. Var olan bir günlük analizi çalışma ile almak [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace). Örneğin, var olan bir çalışma almak için adlı *myWorkspace* bir kaynak grubunda adlı *myWorkspaces*, aşağıdaki komutu girin:

```azurepowershell-interactive
$Oms=Get-AzureRmOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

Varolan bir çalışma alanı yoksa, biriyle oluşturabilirsiniz [yeni AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace).

Günlükleri için etkinleştirebilirsiniz günlük iki kategorisi vardır. Daha fazla bilgi için bkz: [oturum kategorileri](#log-categories). NSG ile için tanılama günlük kaydını etkinleştir [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting). Aşağıdaki örnek olay ve sayacı kategori verileri kimlikleri NSG ve daha önce listelene çalışma için kullanarak, bir NSG için çalışma alanına kaydeder:

```azurepowershell-interactive
Set-AzureRmDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Yalnızca bir kategori veya diğer yerine her ikisi için verileri günlüğe kaydetmek istiyorsanız, eklemeniz `-Categories` ve ardından önceki komutuna seçenek *NetworkSecurityGroupEvent* veya *NetworkSecurityGroupRuleCounter*. Farklı bir oturum istiyorsanız [hedef](#log-destinations) günlük analizi çalışma alanı uygun parametreleri Azure kullanmak [depolama hesabı](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [olay hub'ı](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Görüntüleyin ve günlüklerini analiz edin. Daha fazla bilgi için bkz: [Görünüm ve günlüklerini analiz edin](#view-and-analyze-logs).

### <a name="azure-cli"></a>Azure CLI

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/bash), veya Azure CLI bilgisayarınızdan çalıştırarak. Azure bulut Kabuk ücretsiz etkileşimli kabuk ' dir. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. CLI bilgisayarınızdan çalıştırırsanız, sürüm 2.0.38 gerekir ya da daha sonra. Çalıştırma `az --version` bilgisayarınızda yüklü olan sürümü bulunamıyor. Yükseltme gerekiyorsa, bkz: [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest). CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure'a sahip bir hesap ile oturum açmak için [gerekli izinleri](virtual-network-network-interface.md#permissions).

Tanılama günlük kaydını etkinleştirmek için var olan bir NSG kimliği gerekir. Varolan bir NSG yoksa, biriyle oluşturabilirsiniz [az ağ nsg oluşturma](/cli/azure/network/nsg#az-network-nsg-create).

Tanılama ile için günlüğü etkinleştirmek istediğiniz ağ güvenlik grubu almak [az ağ nsg Göster](/cli/azure/network/nsg#az-network-nsg-show). Örneğin, bir NSG almak için adlı *myNsg* adlı bir kaynak grubunda var *myResourceGroup*, aşağıdaki komutu girin:

```azurecli-interactive
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

Tanılama günlüklerini üç hedef türlerine yazabilirsiniz. Daha fazla bilgi için bkz: [oturum hedefleri](#log-destinations). Bu makalede, günlükleri gönderilen *günlük analizi* bir örnek olarak hedef. Daha fazla bilgi için bkz: [oturum kategorileri](#log-categories). 

NSG ile için tanılama günlük kaydını etkinleştir [az İzleyici tanılama ayarlarını oluştur](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create). Aşağıdaki örnek olay ve sayacı kategori verileri adlı mevcut bir çalışma alanına oturum *myWorkspace*, adlı bir kaynak grubunda var *myWorkspaces*ve, alınan NSG kimliği daha önce:

```azurecli-interactive
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs '[ { "category": "NetworkSecurityGroupEvent", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } }, { "category": "NetworkSecurityGroupRuleCounter", "enabled": true, "retentionPolicy": { "days": 30, "enabled": true } } ]' \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

Varolan bir çalışma alanı yoksa, kullanarak bir tane oluşturabilirsiniz [Azure portal](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [PowerShell](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace). Günlükleri için etkinleştirebilirsiniz günlük iki kategorisi vardır. 

Yalnızca bir kategori veya diğer verileri günlüğe kaydetmek istiyorsanız, önceki komutta verilerini oturum istemediğiniz kategoriyi kaldırın. Farklı bir oturum istiyorsanız [hedef](#log-destinations) günlük analizi çalışma alanı uygun parametreleri Azure kullanmak [depolama hesabı](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [olay hub'ı](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Görüntüleyin ve günlüklerini analiz edin. Daha fazla bilgi için bkz: [Görünüm ve günlüklerini analiz edin](#view-and-analyze-logs).

## <a name="log-destinations"></a>Günlük hedefleri

Tanılama veri olabilir:
- [Bir Azure depolama hesabına yazılan](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), denetim veya el ile İnceleme için. Kaynak tanılama ayarlarını kullanarak bekletme süresi (gün cinsinden) belirtebilirsiniz.
- [Olay hub'ına akışı](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
- [Azure günlük analizi için yazılmış](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Günlük kategorileri

JSON biçimli veriler için aşağıdaki günlük kategorileri yazılır:

### <a name="event"></a>Olay

Olay günlüğü hakkında NSG kuralları MAC adresine dayalı VM'ler uygulanan bilgiler içerir. Aşağıdaki örnek veriler, her olay için günlüğe kaydedilir:

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
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"[PORT RANGE]",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter"></a>Kural sayacı

Kural sayacın kaynaklara uygulanma her bir kural hakkındaki bilgileri içerir. Aşağıdaki örnek veriler her zaman bir kuralın uygulanacağı günlüğe kaydedilir:

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
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> Kaynak IP adresi iletişimi için günlüğe kaydedilmez. Etkinleştirebilirsiniz [NSG akış günlük](../network-watcher/network-watcher-nsg-flow-logging-portal.md) için bir NSG ancak, hangi günlükleri tüm iletişimi başlatılan kaynak IP adresi yanı sıra, kural sayacı bilgileri. NSG akış günlüğü verileri bir Azure Depolama hesabına yazılır. Verilerle çözümleyebilirsiniz [trafiği analytics](../network-watcher/traffic-analytics.md) Azure Ağ İzleyicisi yeteneği.

## <a name="view-and-analyze-logs"></a>Görüntülemek ve günlüklerini analiz edin

Tanılama günlük verilerini görüntülemek öğrenmek için bkz: [Azure tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Tanılama verileri gönder ise:
- **Günlük analizi**: kullanabileceğiniz [ağ güvenlik grubu analytics](../log-analytics/log-analytics-azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-log-analytics
) Gelişmiş ınsights çözümü. Çözüm izin veren veya reddeden her bir sanal makinede ağ arabiriminin MAC adresi trafiği NSG kuralları görselleştirmeleri sağlar.
- **Azure depolama hesabı**: veri PT1H.json dosyasına yazılır. Bulabilirsiniz:
    - Olay Günlüğü'nde aşağıdaki yolu: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
    - Sayaç günlüğünde aşağıdaki yol kuralı: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), daha önce denetim veya işlem günlükleri olarak bilinir. Etkinlik günlüğü ya da Azure dağıtım modeliyle oluşturulan Nsg'ler için varsayılan olarak etkindir. Hangi işlemleri üzerinde Nsg'ler etkinlik günlüğünde tamamlandığını belirlemek için aşağıdaki kaynak türlerini içeren girdilerini arayın:
    - Microsoft.ClassicNetwork/networkSecurityGroups
    - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
    - Microsoft.Network/networkSecurityGroups
    - Microsoft.Network/networkSecurityGroups/securityRules
- Her akış için kaynak IP adresi eklemek için tanı bilgilerini günlüğe kaydetme hakkında bilgi edinmek için bkz: [NSG akış günlüğü](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
