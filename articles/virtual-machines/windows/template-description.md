---
title: "Sanal makineleri bir Azure Resource Manager şablonunda | Microsoft Azure"
description: "Sanal makine kaynak bir Azure Resource Manager şablonu nasıl tanımlanır hakkında daha fazla bilgi edinin."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.openlocfilehash: 9c0039987ec28601c9338d2b94633c38c31e01f8
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu içindeki sanal makineler

Bu makalede, sanal makineleri için geçerli bir Azure Resource Manager şablonu yönleri açıklanmaktadır. Bu makalede, bir sanal makine oluşturmak için tam bir şablonu açıklamaz; için depolama hesapları, ağ arabirimleri, ortak IP adresleri ve sanal ağlar için kaynak tanımı gerekir. Bu kaynakların nasıl birlikte tanımlanabilir hakkında daha fazla bilgi için bkz: [Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Vardır birçok [galerideki şablonları](https://azure.microsoft.com/documentation/templates/?term=VM) VM kaynak içerir. Bir şablona dahil tüm öğeleri burada açıklanmıştır.

Bu örnek belirtilen sayıda sanal makineleri oluşturmak için bir şablonu tipik kaynak bölümünü gösterir:

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
>Bu örnek daha önce oluşturulmuş bir depolama hesabı kullanır. Şablondan dağıtarak depolama hesabı oluşturabilirsiniz. Bu örnek ayrıca bir ağ arabirimi ve şablonda tanımlanan kaynaklarına bağımlı kullanır. Bu kaynaklar örnekte gösterilmez.
>
>

## <a name="api-version"></a>API sürümü

Bir şablonu kullanarak kaynak dağıtırken kullanmak üzere API sürümü belirtmeniz gerekir. Örnek bu apiVersion öğesini kullanarak sanal makine kaynağı gösterir:

```
"apiVersion": "2016-04-30-preview",
```

Şablonunuzda belirttiğiniz API sürümü şablonda tanımlayabilirsiniz hangi özelliklerin etkiler. Genel olarak, en son API sürümü şablonları oluştururken seçmeniz gerekir. Var olan şablonları için önceki bir API sürümü kullanmaya devam etmek istiyor veya şablonunuz yeni özelliklerden yararlanmak için en son sürümü için güncelleştirme olup olmadığını karar verebilirsiniz.

En son API sürümü almak için bu fırsatları kullanın:

- REST API - [tüm kaynak sağlayıcıları Listele](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)
- PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)
- Azure CLI 2.0 - [az sağlayıcısı Göster](https://docs.microsoft.com/cli/azure/provider#az_provider_show)

## <a name="parameters-and-variables"></a>Parametreler ve değişkenler

[Parametreleri](../../resource-group-authoring-templates.md) çalıştırdığınızda şablonu için değerler belirten kolaylaştırır. Bu Parametreler bölümünden örnekte kullanılır:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Örnek şablon dağıttığınızda, değerleri adı ve parola yönetici hesabının oluşturmak için her bir VM ve VM sayısını girin. Şablonla yönetilen ayrı bir dosyada parametre değerleri belirtme veya istendiğinde değerleri sağlayarak seçeneğiniz vardır.

[Değişkenleri](../../resource-group-authoring-templates.md) şablonda kullanılan art arda onu veya zamanla değiştirebilirsiniz değerlerini ayarlamak kolaylaştırır. Bu değişkenler bölümü örnekte kullanılır:

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

Örnek şablon dağıttığınızda, değişken değerleri adı ve önceden oluşturulmuş depolama hesabının tanımlayıcısı için kullanılır. Değişkenleri tanılama uzantısını ayarları sağlamak için de kullanılır. Kullanım [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](../../resource-manager-template-best-practices.md) nasıl parametreler ve değişkenler şablonunuzda yapısı istediğinize karar vermenize yardımcı olacak.

## <a name="resource-loops"></a>Kaynak döngüler

Uygulamanız için birden fazla sanal makine gerektiğinde bir şablonda bir kopya öğesi kullanabilirsiniz. Bu isteğe bağlı öğe, bir parametre olarak belirtilen VM'ler oluşturmada size döngü:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

Örnekte'de, fark döngü dizini bazı kaynak için değerleri belirtmek için kullanılır. Örneğin, üç örnek sayısı girdiyseniz, işletim sistemi disklerinde adlarını myOSDisk1, myOSDisk2 ve myOSDisk3 şunlardır:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>Bu örnek yönetilen diskleri sanal makineler için kullanır.
>
>

Şablonda bir kaynak için bir döngü oluşturma oluştururken veya diğer kaynaklara erişme döngü kullanmanızı gerektirebilir unutmayın. Örneğin, üç sanal makineleri oluşturmada size şablonunuzu döngü, aynı zamanda üç ağ arabirimleri oluşturmada size döngü gerekir böylece birden çok VM aynı ağ arabirimi, kullanamazsınız. Bir ağ arabirimi için bir VM atarken, döngü dizini tanımlamak için kullanılır:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Bağımlılıklar

En fazla kaynak düzgün çalışması için diğer kaynaklara bağımlı. Sanal makineler, bir ağ arabirimi gerektiği yapmak için bir sanal ağ ile ilişkilendirilmiş olması gerekir. [DependsOn](../../resource-group-define-dependencies.md) öğe ağ arabirimi sanal makineleri oluşturmadan önce kullanılmaya hazır olduğundan emin olmak için kullanılır:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Resource Manager dağıtılan başka bir kaynağa bağımlı olmayan tüm kaynakları paralel olarak dağıtır. Gereksiz bağımlılıkları belirterek dağıtımınızı yanlışlıkla yavaşlatabileceği için bağımlılıkları ayarlarken dikkatli olun. Bağımlılıklar birden fazla kaynak zincir. Örneğin, ağ arabirimi genel IP adresi ve sanal ağ kaynaklarına bağlıdır.

Bir bağımlılık gerekli olup olmadığını nasıl bilebilirsiniz? Şablonda ayarlanan değerlerle bakın. Bir öğe varsa sanal makine kaynak tanımı noktaları aynı şablonunda dağıtılan başka bir kaynak için bir bağımlılık gerekir. Örneğin, örnek sanal makine bir ağ profili tanımlar:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

Bu özelliği ayarlamak için ağ arabiriminin mevcut olması gerekir. Bu nedenle, bir bağımlılık gerekir. Ayrıca, bir kaynak (alt) içindeki başka bir kaynak (üst) tanımlandığında bir bağımlılık ayarlamanız gerekir. Örneğin, tanılama ayarlarını ve özel komut dosyası uzantılarını hem de sanal makine alt kaynaklar olarak tanımlanır. Sanal makinenin var, oluşturulamaz. Bu nedenle, her iki kaynağın sanal makineye bağlı olarak işaretlenir.

## <a name="profiles"></a>Profiller

Birkaç profil öğeler, bir sanal makine kaynağı tanımlarken kullanılır. Bazı gereklidir ve bazı isteğe bağlıdır. Örneğin, hardwareProfile, osProfile, storageProfile ve networkProfile öğeleri gerekiyor, ancak diagnosticsProfile isteğe bağlıdır. Bu profiller ayarları gibi tanımlayın:
   
- [boyutu](sizes.md)
- [ad](/architecture/best-practices/naming-conventions) ve kimlik bilgileri
- disk ve [işletim sistemi ayarları](cli-ps-findimage.md)
- [Ağ arabirimi](../../virtual-network/virtual-network-deploy-multinic-classic-ps.md) 
- Önyükleme tanılaması

## <a name="disks-and-images"></a>Diskleri ve görüntüleri
   
Azure'da, vhd dosyaları gösterebilir [disk veya görüntü](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Belirli bir VM'yi olması için bir vhd dosyasının işletim sisteminde özelleştirilmiş olduğunda, bir disk olarak bilinir. Birçok VM oluşturmak için kullanılacak genelleştirilmiş bir vhd dosyasının işletim sisteminde, bunu bir görüntü olarak bilinir.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Yeni sanal makineler ve yeni diskler bir platform görüntüsünden oluşturma

Bir VM oluşturduğunuzda, hangi işletim sistemi kullanmaya karar vermeniz gerekir. Imagereference öğesi, yeni bir VM işletim sistemini tanımlamak için kullanılır. Örnek bir Windows Server işletim sistemi için bir tanım gösterir:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Linux işletim sistemi oluşturmak istiyorsanız, bu tanımı kullanabilirsiniz:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

İşletim sistemi diski için yapılandırma ayarlarını osDisk öğeyle atanır. Örnek, önbelleğe alma modu ayarlandığında yeni bir yönetilen disk tanımlar **ReadWrite** ve disk alanından oluşturulmakta olan bir [platform görüntüsü](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Mevcut yönetilen diskleri yeni sanal makineler oluşturun

Sanal makineler var olan disklerden oluşturmak istiyorsanız, Imagereference ve osProfile öğeleri kaldırın ve bu disk ayarlarını tanımlayın:

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Yönetilen bir görüntüden yeni sanal makineler oluşturun

Yönetilen bir görüntüden sanal makine oluşturmak istiyorsanız, Imagereference öğesi değiştirin ve bu disk ayarlarını tanımlayın:

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

### <a name="attach-data-disks"></a>Veri diskleri ekleme

Veri diskleri sanal makineleri için isteğe bağlı olarak ekleyebilirsiniz. [Diskleri sayısı](sizes.md) kullandığınız işletim sistemi disk boyutuna bağlıdır. Standard_DS1_v2 için ayarlanmış VM'ler ile bunlara eklenemedi veri disklerinin sayısının iki boyutudur. Örnekte, bir yönetilen veri diski her VM'ye ekleniyor:

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

Ancak [uzantıları](extensions-features.md) ayrı bir kaynak, VM yakından bağlıdır. Uzantılar, bir alt kaynak VM veya farklı bir kaynak olarak eklenebilir. Örnekte gösterildiği [tanılama uzantısını](extensions-diagnostics-template.md) Vm'lere eklenmekte olan:

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

Bu uzantı kaynak storageName değişkeni ve tanılama değişkenleri değerlerini sağlamak için kullanır. Bu uzantı tarafından toplanan verileri değiştirmek istiyorsanız, daha fazla performans sayaçları wadperfcounters değişkenine ekleyebilirsiniz. Tanılama verilerini VM diskleri depolandığı daha farklı bir depolama hesabı içine yerleştirilecek seçebilir.

Bir VM'ye yükleyebilirsiniz birçok uzantıları vardır, ancak en büyük olasılıkla yararlıdır [özel betik uzantısının](extensions-customscript.md). İlk başladığında örnekte, her VM start.ps1 adlı bir PowerShell komut dosyasını çalıştırır:

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

Start.ps1 komut dosyası, birçok yapılandırma görevleri gerçekleştirebilirsiniz. Örneğin, örnek vm'lerinin eklenen veri diskleri başlatılamadı; bunları başlatmak için özel bir komut dosyası kullanabilirsiniz. Birden çok başlangıç görevleri yapmak için varsa, Azure depolama alanında başka PowerShell betikleri çağırmak için start.ps1 dosyasını kullanabilirsiniz. Örnek PowerShell kullanır, ancak işletim sistemi üzerinde kullanılabilir herhangi bir komut dosyası yöntemini kullanabilirsiniz.

Yüklü uzantılarla portalında uzantıları ayarlarından durumunu görebilirsiniz:

![Uzantı durumunu Al](./media/template-description/virtual-machines-show-extensions.png)

Kullanarak uzantısı bilgi edinebilirsiniz **Get-AzureRmVMExtension** PowerShell komutunu **vm uzantısı get** Azure CLI 2.0 komut veya **uzantısı bilgialma** REST API.

## <a name="deployments"></a>Dağıtımlar

Bir şablonu dağıttığınızda, Azure kaynakları grup olarak dağıtılmış ve otomatik olarak dağıtılan bu gruba bir isim atar izler. Dağıtım şablonu adı aynıdır.

Dağıtımdaki kaynakların durumunu hakkında merak ediyorsanız, Azure portalında kaynak grubu dikey kullanabilirsiniz:

![Dağıtım bilgileri alma](./media/template-description/virtual-machines-deployment-info.png)
    
Kaynakları oluşturun veya var olan kaynakların güncelleştirmek için aynı şablonu kullanmak için bir sorun teşkil etmez. Şablonları dağıtmak için komutları kullandığınızda, hangi söyleyin fırsatına sahip [modu](../../resource-group-template-deploy.md) kullanmak istediğiniz. Mod için ya da ayarlanabilir **tam** veya **artımlı**. Artımlı güncelleştirmeler yapmak için varsayılandır. Kullanırken dikkatli olun **tam** modu kaynakları yanlışlıkla silebilir olduğundan. Modu ayarlandığında **tam**, Resource Manager şablonunda olmayan tüm kaynaklar kaynak grubunda siler.

## <a name="next-steps"></a>Sonraki Adımlar

- Kendi şablonunu kullanarak oluşturduğunuz [Azure Resource Manager şablonları yazma](../../resource-group-authoring-templates.md).
- Kullanılarak oluşturulan şablonu dağıtmak [Resource Manager şablonu ile Windows sanal makine oluşturma](ps-template.md).
- Gözden geçirerek oluşturulan VM'ler yönetmeyi öğrenin [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
