---
title: "İzleme işlemleri, olaylar ve sayaçlar için Nsg'ler | Microsoft Docs"
description: "Sayaçlar, olayları ve Nsg'ler için işlem günlüğünü etkinleştirme hakkında bilgi edinin"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: 6beb9ae1b64e27df0a4eefefd592c7850efc7d2d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Ağ güvenlik grupları (NSG’ler) için Log Analytics

Aşağıdaki tanılama günlük kategorileri için Nsg'ler etkinleştirebilirsiniz:

* **Olay:** hangi NSG kuralları Vm'lere uygulanan ve örnek MAC adresine dayalı rolleri girişleri içerir. Bu kurallar durumunun her 60 saniyede toplanır.
* **Kural sayacı:** kaç kez her NSG için içerir girişleri kural reddetmek veya trafiğine izin vermek üzere uygulanır.

> [!NOTE]
> Tanılama günlükleri, yalnızca Azure Resource Manager dağıtım modeli aracılığıyla dağıtılan Nsg'ler için kullanılabilir. Klasik dağıtım modeli aracılığıyla dağıtılan Nsg'ler için tanılama günlüğü etkinleştiremezsiniz. Daha iyi iki modellerinin anlamak için başvuru [anlama Azure dağıtım modelleri](../resource-manager-deployment-model.md) makalesi.

Etkinlik günlüğü (daha önce denetim veya işlem günlükleri olarak bilinir) ya da Azure dağıtım modeliyle oluşturulan Nsg'ler için varsayılan olarak etkindir. Hangi işlemleri üzerinde Nsg'ler etkinlik günlüğünde tamamlandığını belirlemek için aşağıdaki kaynak türlerini içeren girdilerini arayın: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Okuma [Azure etkinlik günlüğü'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) etkinlik günlükleri hakkında daha fazla bilgi için makalenin. 

## <a name="enable-diagnostic-logging"></a>Tanılama günlüğünü etkinleştirme

Tanılama günlüğü etkin, için *her* için veri toplamak istediğiniz NSG. [Genel bakış, Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) makalede açıklanır tanılama günlüklerini burada gönderilebilir. Varolan bir NSG yoksa, bölümündeki adımları tamamlamanız [bir ağ güvenlik grubu oluşturun](virtual-networks-create-nsg-arm-pportal.md) makale bir tane oluşturun. NSG aşağıdaki yöntemlerden birini kullanarak oturum tanılama etkinleştirebilirsiniz:

### <a name="azure-portal"></a>Azure portalına

Günlük, oturum açma etkinleştirmek için portalı kullanmak için [portal](https://portal.azure.com). Tıklatın **tüm hizmetleri**, ardından *ağ güvenlik grubu*. İçin günlük kaydını etkinleştirmek istediğiniz NSG seçin. İşlem olmayan kaynakları için yönergeleri izleyin [portalında tanılama günlüklerini etkinleştirme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) makalesi. Seçin **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, veya her iki kategorilerini günlükleri.

### <a name="powershell"></a>PowerShell

Günlük kaydını etkinleştirmek için PowerShell kullanmak için ' ndaki yönergeleri izleyin [PowerShell aracılığıyla tanılama günlüklerini etkinleştirme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) makalesi. Bir komut makaleden girmeden önce aşağıdaki bilgileri değerlendirin:

- İçin kullanılacak bir değer belirleyebilirsiniz `-ResourceId` aşağıdaki değiştirerek parametresi [metin] uygun şekilde, sonra komutu girerek `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. Komut Kimliği çıktısı için benzer */subscriptions/ [abonelik Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG adı]*.
- Yalnızca günlük kategoriden veri toplamak istiyorsanız eklemek `-Categories [category]` kategori olduğu ya da makalede, komut sonuna *NetworkSecurityGroupEvent* veya *NetworkSecurityGroupRuleCounter*. Kullanmazsanız `-Categories` parametresi, veri toplama kategorileri hem günlük için etkinleştirildi.

### <a name="azure-command-line-interface-cli"></a>Azure komut satırı arabirimi (CLI)

Günlük kaydını etkinleştirmek için CLI kullanmak için ' ndaki yönergeleri izleyin [CLI aracılığıyla tanılama günlüklerini etkinleştirme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) makalesi. Bir komut makaleden girmeden önce aşağıdaki bilgileri değerlendirin:

- İçin kullanılacak bir değer belirleyebilirsiniz `-ResourceId` aşağıdaki değiştirerek parametresi [metin] uygun şekilde, sonra komutu girerek `azure network nsg show [resource-group-name] [nsg-name]`. Komut Kimliği çıktısı için benzer */subscriptions/ [abonelik Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG adı]*.
- Yalnızca günlük kategoriden veri toplamak istiyorsanız eklemek `-Categories [category]` kategori olduğu ya da makalede, komut sonuna *NetworkSecurityGroupEvent* veya *NetworkSecurityGroupRuleCounter*. Kullanmazsanız `-Categories` parametresi, veri toplama kategorileri hem günlük için etkinleştirildi.

## <a name="logged-data"></a>Günlüğe kaydedilen veriler

JSON biçimli veriler için her iki günlüklerine yazılır. Her günlük türü için yazılan özel veriler aşağıdaki bölümlerde listelenmiştir:

### <a name="event-log"></a>Olay günlüğü
Bu günlük kuralları Vm'lere uygulanan ve MAC adresine dayalı hizmet rolü örneklerinin bulut hangi NSG hakkında bilgiler içerir. Aşağıdaki örnek veriler, her olay için günlüğe kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Kural sayacı günlüğü

Bu günlük kaynaklara uygulanma her bir kural hakkındaki bilgileri içerir. Aşağıdaki örnek veriler her zaman bir kuralın uygulanacağı günlüğe kaydedilir:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Görüntülemek ve günlüklerini analiz edin

Etkinlik günlüğü verilerini görüntülemek nasıl öğrenmek için [Azure etkinlik günlüğü'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) makalesi. Tanılama günlük verilerini görüntülemek nasıl öğrenmek için [genel bakış, Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) makalesi. Günlük analizi için tanılama verilerini gönderirseniz, kullanabileceğiniz [Azure ağ güvenlik grubu analytics](../log-analytics/log-analytics-azure-networking-analytics.md) Gelişmiş ınsights (Önizleme) yönetimi çözümü. 
