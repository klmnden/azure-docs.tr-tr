---
title: Bir Azure Service Fabric kümesi yapılandırmasını yükseltme | Microsoft Docs
description: Resource Manager şablonu kullanarak Azure'da bir Service Fabric kümesi çalıştıran yapılandırma yükseltmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 818136f24eb063e2bd7217d5441bda19bf141317
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666605"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Azure'daki bir kümeye yapılandırmasını Yükselt 

Bu makalede, Service Fabric kümeniz için çeşitli yapı ayarları özelleştirmeyi açıklar. Azure'da barındırılan kümeler için ayarları aracılığıyla özelleştirebilirsiniz [Azure portalında](https://portal.azure.com) veya bir Azure Resource Manager şablonu kullanarak.

> [!NOTE]
> Tüm ayarlar, portalda kullanılabilir değildir ve bu bir [açısından en iyisi, bir Azure Resource Manager şablonu kullanarak özelleştirmek için](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code); Service Fabric Dev\Test senaryo için yalnızca portalıdır.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Resource Manager şablonlarını kullanarak küme ayarlarını özelleştirme
Azure kümeleri JSON Resource Manager şablonu aracılığıyla yapılandırılabilir. Farklı ayarlar hakkında daha fazla bilgi için bkz: [kümeleri için yapılandırma ayarlarını](service-fabric-cluster-fabric-settings.md). Örneğin, aşağıdaki adımları yeni bir ayar ekleme Göster *MaxDiskQuotaInMB* için *tanılama* Azure kaynak Gezgini'ni kullanarak bölümü.

1. Şuraya gidin: https://resources.azure.com
2. Aboneliğinize genişleterek gidin **abonelikleri** -> **\<aboneliğiniz >** -> **resourceGroups**  ->   **\<Uygulamanızın kaynak grubu >** -> **sağlayıcıları** -> **Microsoft.ServiceFabric**  ->  **kümeleri** -> **\<küme adınız >**
3. Sağ alt köşesinde üst **okuma/yazma.**
4. Seçin **Düzenle** ve güncelleştirme `fabricSettings` JSON öğesi ve yeni bir öğe ekleyin:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Ayrıca, Azure Resource Manager ile aşağıdaki yollardan biriyle küme ayarları özelleştirebilirsiniz:

- Kullanım [Azure portalında](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) dışarı aktarma ve Kaynak Yöneticisi şablonu güncelleştirmek için.
- Kullanım [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) dışarı aktarma ve Resource Manager şablonu güncelleştirmek için.
- Kullanım [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) dışarı aktarma ve Resource Manager şablonu güncelleştirmek için.
- Azure RM PowerShell kullanmak [kümesi AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Set-AzureRmServiceFabricSetting) ve [Remove-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Remove-AzureRmServiceFabricSetting) ayarı değiştirmek için komutları doğrudan.
- Azure clı'yi [az sf küme ayarı](https://docs.microsoft.com/cli/azure/sf/cluster/setting) ayarı değiştirmek için komutları doğrudan.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [Service Fabric küme ayarlarını](service-fabric-cluster-fabric-settings.md).
* Bilgi edinmek için nasıl [kümenizin ölçeğini daraltma ve genişletme](service-fabric-cluster-scale-up-down.md).
