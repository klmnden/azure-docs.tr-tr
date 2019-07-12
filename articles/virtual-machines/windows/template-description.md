---
title: Sanal makineler bir Azure Resource Manager şablonu | Microsoft Azure
description: Sanal makine kaynağı bir Azure Resource Manager şablonunun nasıl tanımlandığı hakkında daha fazla bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.openlocfilehash: fd4fad40ef4809c756321493854f38fd813569ca
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710278"
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Sanal makineler bir Azure Resource Manager şablonu

Bu makalede sanal makineleri için geçerli bir Azure Resource Manager şablonu yönlerini açıklar. Bu makalede, bir sanal makine oluşturmak için tam bir şablon olarak açıklanmamaktadır; Bunun için depolama hesapları, ağ arabirimleri, ortak IP adresleri ve sanal ağlar için kaynak tanımları gerekir. Bu kaynakları nasıl birlikte tanımlanabilir hakkında daha fazla bilgi için bkz. [Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Kullanabileceğiniz birçok [galerideki şablonları](https://azure.microsoft.com/documentation/templates/?term=VM) VM kaynağını içerir. Bir şablona dahil tüm öğeler burada açıklanmıştır.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

Bu örnek belirtilen sayıda VM'ler oluşturmak için bir şablon tipik kaynak bölümünü gösterir:

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
        "adminUsername": "[parameters('adminUsername')]", 
        "adminPassword": "[parameters('adminPassword')]" 
      }, 
      "storageProfile": { 
        "imageReference": { 
          "publisher": "MicrosoftWindowsServer", 
          "offer": "WindowsServer", 
          "sku": "2012-R2-Datacenter", 
          "version": "latest" 
        }, 
        "osDisk": { 
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
>Bu örnek, daha önce oluşturulmuş bir depolama hesabı kullanır. Şablonu dağıtarak, depolama hesabı oluşturabilirsiniz. Bu örnek ayrıca bir ağ arabirimi ve şablonda tanımlanan bağımlı kaynaklarını kullanır. Bu kaynaklar, örnekte gösterilmez.
>
>

## <a name="api-version"></a>API sürümü

Şablon kullanarak kaynakları dağıtırken, kullanılacak API sürümünü belirtmeniz gerekir. Örneğin, bu apiVersion öğesini kullanarak sanal makine kaynağı gösterir:

```
"apiVersion": "2016-04-30-preview",
```

Şablonunuzda belirttiğiniz API sürümü, şablonda tanımladığınız hangi özellikleri etkiler. Genel olarak, şablon oluştururken en güncel API sürümünü seçmelisiniz. Var olan şablonları için önceki bir API sürümü kullanmaya devam etmek istiyor veya şablonunuzu yeni özelliklerden yararlanmak için en son sürümü için güncelleştirme olup olmadığını karar verebilirsiniz.

En son API sürümlerini almak için bu fırsatlar kullanın:

- REST API - [tüm kaynak sağlayıcıları Listele](https://docs.microsoft.com/rest/api/resources/providers)
- PowerShell - [Get-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/get-azresourceprovider)
- Azure CLI - [az provider show](https://docs.microsoft.com/cli/azure/provider)


## <a name="parameters-and-variables"></a>Parametreler ve değişkenler

[Parametreleri](../../resource-group-authoring-templates.md) çalıştırdığınızda şablon değerlerini belirtmek kolaylaştırır. Bu parametreler bölümü örnekte kullanılır:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Örnek şablonu dağıtırken, değerleri adı ve parola yönetici hesabı oluşturmak için her sanal makine ve sanal makinelerin sayısını girin. Şablonla yönetilen ayrı bir dosyada parametre değerleri belirtme veya sorulduğunda değerleri sağlayarak seçeneğiniz vardır.

[Değişkenleri](../../resource-group-authoring-templates.md) şablonunda kullanılan sürekli olarak bunu veya zamanla değiştirebilirsiniz değerlerini ayarlamak kolaylaştırır. Bu değişkenler bölümü örnekte kullanılır:

```
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

Örnek şablonu dağıtırken, değişken değerler adı ve önceden oluşturduğunuz depolama hesabının tanımlayıcısı için kullanılır. Değişkenleri de tanılama uzantısı ayarlarını belirtmek için kullanılır. Kullanım [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](../../resource-manager-template-best-practices.md) nasıl parametreler ve değişkenler şablonunuzdaki yapısı istediğinize karar verirken size yardımcı olmak için.

## <a name="resource-loops"></a>Kaynak döngüler

Uygulamanız için birden fazla sanal makine gerektiğinde, copy öğesinde bir şablon kullanabilirsiniz. Bu isteğe bağlı öğe, bir parametre olarak belirtilen VM sayısını oluşturma işleminde döngüsü:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

Ayrıca, örnekte dikkat edin. döngü dizini belirtirken bazı kaynak için değerler kullanılır. Örneğin, üç örnek sayısı girdiyseniz, işletim sistemi diskleri adlarını myOSDisk1 myOSDisk2 ve myOSDisk3 şunlardır:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>Bu örnek sanal makineler için yönetilen diskler kullanır.
>
>

Bu şablonda bir kaynak için bir döngü oluşturma oluştururken ya da diğer kaynaklara erişim döngü kullanmanızı gerektirebilir göz önünde bulundurun. Örneğin, üç VM oluşturma işleminde, şablonunuzu döngü, aynı zamanda üç ağ arabirimlerini oluşturma işleminde döngü gerekir böylece birden çok sanal makine aynı ağ arabirimini kullanamazsınız. Bir VM'ye ağ arabirimi atamasını yaparken, döngü dizini tanımlamak için kullanılır:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Bağımlılıkları

En fazla kaynak düzgün çalışması için diğer kaynaklara bağımlı. Sanal makineler, bir ağ arabirimi gerekiyor yapmak için bir sanal ağ ile ilişkili olmalıdır. [DependsOn](../../resource-group-define-dependencies.md) öğesi, ağ arabiriminin VM oluşturmadan önce kullanıma hazır olduğundan emin olmak için kullanılır:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Resource Manager dağıtılan başka bir kaynağa bağımlı olmayan tüm kaynakları paralel olarak dağıtır. Yanlışlıkla gereksiz bağımlılıkları belirterek dağıtımınızı yavaşlatabileceği için bağımlılıklar ayarlarken dikkatli olun. Bağımlılıklar, birden çok kaynaklarında zincirleyebilirsiniz. Örneğin, ağ arabirimi genel IP adresi ve sanal ağ kaynaklarına bağlıdır.

Nasıl bir bağımlılık gerekli olup olmadığını bilebilirsiniz? Şablonda ayarlanan değerlerle bakın. Bir öğe varsa sanal makine kaynak tanımı noktaları aynı şablonda dağıtılan başka bir kaynak için bir bağımlılık gerekir. Örneğin, örnek sanal makine bir ağ profili tanımlar:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

Bu özelliği ayarlamak için ağ arabirimi mevcut olması gerekir. Bu nedenle, bir bağımlılık gerekir. Ayrıca başka bir kaynak (üst) içinde bir kaynak (alt) tanımlandığında, bir bağımlılık ayarlamanız gerekir. Örneğin, tanılama ayarları ve özel betik uzantıları hem de sanal makine alt kaynakları tanımlanır. Sanal makinenin var kadar bunlar oluşturulamaz. Bu nedenle, her iki kaynaklar sanal makineye bağlı olarak işaretlenir.

## <a name="profiles"></a>Profiller

Birkaç profil öğeleri, bir sanal makine kaynağı tanımlarken kullanılır. Bazı gerekli ve isteğe bağlı bazılarıdır. Örneğin, hardwareProfile, osProfile Datadisks ve networkProfile öğeleri gereklidir, ancak diagnosticsProfile isteğe bağlıdır. Bu profiller ayarları gibi tanımlayın:
   
- [Boyutu](sizes.md)
- [adı](/azure/architecture/best-practices/naming-conventions) ve kimlik bilgileri
- disk ve [işletim sistemi ayarları](cli-ps-findimage.md)
- [Ağ arabirimi](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md) 
- Önyükleme tanılaması

## <a name="disks-and-images"></a>Diskleri ve görüntüleri
   
Azure'da, vhd dosyalarını temsil edebilen [disk veya görüntü](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Belirli bir VM'ye olması için işletim sistemi vhd dosyasındaki özel olduğunda, bir disk olarak adlandırılır. Çok sayıda VM oluşturmak için kullanılacak genelleştirilmiş bir vhd dosyasındaki işletim sistemi, bunu bir görüntü olarak adlandırılır.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Yeni sanal makineler ve yeni diskler bir platform görüntüsünden oluşturma

Bir VM oluşturduğunuzda, hangi işletim sistemi kullanmaya karar vermeniz gerekir. Imagereference öğesi, yeni bir sanal makine işletim sistemini tanımlamak için kullanılır. Örnek, bir Windows Server işletim sistemi için bir tanımı göstermektedir:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Bir Linux işletim sistemi oluşturmak istiyorsanız, bu tanımı kullanabilirsiniz:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

İşletim sistemi diski için yapılandırma ayarlarını osDisk öğeyle atanır. Önbelleğe alma modu ayarlandığında yeni bir yönetilen disk örnek tanımlar **ReadWrite** ve disk alanından oluşturulmakta olan bir [platform görüntüsü](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Mevcut yönetilen diskleri yeni sanal makineler oluşturun

Var olan disklerden sanal makineleri oluşturmak istiyorsanız, Imagereference ve osProfile öğeleri kaldırın ve bu disk ayarlarını tanımlayın:

```
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Yönetilen bir görüntüden yeni sanal makineler oluşturma

Yönetilen bir görüntüden sanal makine oluşturmak istiyorsanız, Imagereference öğeyi değiştirmek ve bu disk ayarlarını tanımlayın:

```
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a>Veri diski ekleme

İsteğe bağlı olarak, Vm'lere veri diskleri ekleyebilirsiniz. [Diskleri sayısı](sizes.md) kullandığınız işletim sistemi disk boyutuna bağlıdır. Standard_DS1_v2'için ayarlanmış Vm'leri ile kendisine eklenemedi veri diskleri sayısı iki boyutudur. Örnekte, her VM için bir yönetilen veri diski eklenirse:

```
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a>Uzantılar

Ancak [uzantıları](extensions-features.md) ayrı bir kaynak, Vm'lere yakından bağlı değilsiniz. Uzantılar alt kaynak sanal makinenin veya ayrı bir kaynak olarak eklenebilir. Örnekte gösterildiği [tanılama uzantısını](extensions-diagnostics-template.md) Vm'lere eklenen:

```
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

Bu uzantı kaynak storageName değişkeni ve tanılama değişkenlerin değerlerini sağlamak için kullanır. Bu uzantı tarafından toplanan verileri değiştirmek istiyorsanız, daha fazla performans sayaçları wadperfcounters değişkene ekleyebilirsiniz. VM diskleri depolandığı değerinden farklı bir depolama hesabına tanılama verilerini yerleştirmek seçebilirsiniz.

Bir VM'ye yükleyebilirsiniz birçok uzantılar vardır, ancak büyük olasılıkla en kullanışlı olduğu [özel betik uzantısı](extensions-customscript.md). İlk kez başlatıldığında örnekte, her sanal makinede start.ps1 adlı bir PowerShell komut dosyasını çalıştırır:

```
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

Start.ps1 betiği çok sayıda yapılandırma görevleri gerçekleştirebilirsiniz. Örneğin, örnekte vm'lere eklenen veri disklerini başlatılmış değil; özel bir betik, bunları başlatmak için kullanabilirsiniz. Birden çok başlangıç görevleri yapmak için varsa, Azure depolama alanında başka PowerShell betikleri çağrılacak start.ps1 dosyası kullanabilirsiniz. Bu örnek PowerShell kullanır ancak kullanmakta olduğunuz işletim sisteminde kullanılabilir olan herhangi bir komut dosyası yöntemi kullanabilirsiniz.

Yüklü uzantılar portalında uzantılarının ayarlarından durumunu görebilirsiniz:

![Uzantı durumu alma](./media/template-description/virtual-machines-show-extensions.png)

Uzantı bilgileri kullanarak da alabilirsiniz **Get-AzVMExtension** PowerShell komutunu **vm uzantısı get** Azure CLI komutunu veya **uzantısı bilgialma**REST API.

## <a name="deployments"></a>Dağıtımlar

Şablon dağıtımı yaptığınızda Azure kaynakları bir grup olarak dağıtılan ve otomatik olarak dağıtılan bu grup için bir ad atar izler. Dağıtım adı şablon adı ile aynıdır.

Kaynakların dağıtım durumu hakkında merak ediyorsanız, Azure portalında kaynak grubunu görüntüleyin:

![Dağıtım bilgileri Al](./media/template-description/virtual-machines-deployment-info.png)
    
Bu kaynakları oluşturmak veya var olan kaynakları güncelleştirme için aynı şablonu kullanmak için bir sorun değildir. Şablon dağıtımı komutlarını kullanırken, hangi söylemek fırsatına sahip [modu](../../resource-group-template-deploy.md) kullanmak istiyorsunuz. Modu olarak ayarlanabilir **tam** veya **artımlı**. Artımlı güncelleştirmeler yapmak için varsayılandır. Kullanırken dikkatli olun **tam** modu nedeniyle yanlışlıkla kaynakları da silebilirsiniz. Modu ayarlandığında **tam**, Resource Manager şablon olmayan kaynak grubundaki tüm kaynakları siler.

## <a name="next-steps"></a>Sonraki Adımlar

- Kendi şablonunu kullanarak oluşturduğunuz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).
- Kullanılarak oluşturulan şablonu dağıtma [Resource Manager şablonu ile Windows sanal makine oluşturma](ps-template.md).
- Geçirerek, oluşturduğunuz sanal makineleri yönetmeyi öğrenin [oluşturun ve Azure PowerShell modülü ile Windows Vm'leri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- JSON söz dizimi ve özelliklerini şablonlarında kaynak türleri için bkz [Azure Resource Manager şablon başvurusu](/azure/templates/).
