---
title: Azure Service Fabric iyi kod olarak altyapı | Microsoft Docs
description: Service Fabric kod olarak altyapı olarak yönetmek için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 2a4c3adb39ebf6e58770348eccc8c78e8ff92167
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804476"
---
# <a name="infrastructure-as-code"></a>Kod olarak altyapı

Bir üretim senaryosunda, Resource Manager şablonlarını kullanarak Azure Service Fabric kümeleri oluşturma. Resource Manager şablonları, kaynak özelliklerinin daha fazla denetim sağlamak ve tutarlı bir kaynak modeli olduğundan emin olun.

Windows ve Linux için örnek Resource Manager şablonları kullanılabilir [github'daki Azure örnekleri](https://github.com/Azure-Samples/service-fabric-cluster-templates). Bu şablonlar, küme şablonunuza için başlangıç noktası olarak kullanılabilir. İndirme `azuredeploy.json` ve `azuredeploy.parameters.json` ve onları özel gereksinimlerinizi karşılayacak şekilde düzenleyebilirsiniz.

Dağıtılacak `azuredeploy.json` ve `azuredeploy.parameters.json` yukarıda indirdiğiniz şablonları aşağıdaki Azure CLI komutları kullanın:

```azurecli
ResourceGroupName="sfclustergroup"
Location="westus"

az group create --name $ResourceGroupName --location $Location 
az group deployment create --name $ResourceGroupName  --template-file azuredeploy.json --parameters @azuredeploy.parameters.json
```

PowerShell kullanarak bir kaynak oluşturma

```powershell
$ResourceGroupName="sfclustergroup"
$Location="westus"
$Template="azuredeploy.json"
$Parameters="azuredeploy.parameters.json"

New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
New-AzureRmResourceGroupDeployment -Name $ResourceGroupName -TemplateFile $Template -TemplateParameterFile $Parameters
```

## <a name="azure-service-fabric-resources"></a>Azure Service Fabric kaynakları

Service Fabric kümenizi Azure Resource Manager aracılığıyla üzerine uygulamaları ve Hizmetleri dağıtın. Bkz: [uygulamaları ve Hizmetleri Azure Resource Manager kaynaklarını yönetmek](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-arm-resource) Ayrıntılar için. Resource Manager şablonu kaynaklarınızı eklemek için en iyi yöntem Service Fabric uygulaması belirli kaynaklar aşağıda verilmiştir.

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

Azure Resource Manager kullanarak uygulamanızı dağıtmak için önce [bir sfpkg oluşturma](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg) Service Fabric uygulama paketi. Aşağıdaki python betiğini bir sfpkg oluşturmak nasıl bir örneğidir:

```python
# Create SFPKG that needs to be uploaded to Azure Storage Blob Container
microservices_sfpkg = zipfile.ZipFile(self.microservices_app_package_name, 'w', zipfile.ZIP_DEFLATED)
package_length = len(self.microservices_app_package_path)

for root, dirs, files in os.walk(self.microservices_app_package_path):
    root_folder = root[package_length:]
    for file in files:
        microservices_sfpkg.write(os.path.join(root, file), os.path.join(root_folder, file))

microservices_sfpkg.close()
```

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* Bir küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturun: [Bir Linux kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin