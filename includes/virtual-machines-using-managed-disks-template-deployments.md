---
title: include dosyası
description: include dosyası
services: storage
author: jboeshart
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: jaboes
ms.custom: include file
ms.openlocfilehash: 904bd884bc09c1e2016f55ffc8e1e9f635974ac7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157691"
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Yönetilen diskler, Azure Resource Manager şablonlarını kullanma

Bu belge sanal makineler sağlamak için Azure Resource Manager şablonlarını kullanarak, yönetilen ve yönetilmeyen diskler arasındaki farklar açıklanmaktadır. Örnekler, yönetilmeyen diskleri yönetilen disklere kullanarak mevcut şablonları güncelleştirme yardımcı olur. Başvuru için kullanıyoruz [101-vm-basit-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) şablon olarak bir kılavuz. Her ikisini de kullanarak şablonu gördüğünüz [yönetilen diskler](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) ve bir önceki sürümünü kullanarak [yönetilmeyen diskler](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) doğrudan karşılaştırmak istiyorsanız.

## <a name="unmanaged-disks-template-formatting"></a>Yönetilmeyen diskler şablonu biçimlendirmesi

Başlamak için Şimdi Al nasıl yönetilmeyen diskler göz dağıtılır. Yönetilmeyen diskler oluştururken, VHD dosyalarını barındıracak bir depolama hesabı gerekir. Yeni bir depolama hesabı oluşturabilir veya zaten var olan bir kullanın. Bu makalede yeni bir depolama hesabının nasıl oluşturulacağını gösterir. Aşağıda gösterildiği gibi kaynakların bloğundaki bir depolama hesabı kaynağı oluşturun.

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2018-07-01",
    "name": "[variables('storageAccountName')]",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

İçinde sanal makine nesnesini, bir bağımlılık önce sanal makinenin oluşturulduğunu emin olmak için depolama hesabı ekleyin. İçinde `storageProfile` bölümünde, depolama hesabına başvuruyor ve işletim sistemi diski ve varsa veri diskleri için gerekli VHD konumunun tam bir URI belirtin.

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Yönetilen diskler şablonu biçimlendirmesi

Azure yönetilen diskler, disk üst düzey bir kaynakla olur ve artık kullanıcı tarafından oluşturulması için bir depolama hesabı gerektirir. Yönetilen diskler ilk olarak ifşa `2016-04-30-preview` API sürümü, bunlar tüm sonraki API sürümlerinde kullanılabilir ve varsayılan disk türü sunulmuştur. Aşağıdaki bölümlerde, varsayılan ayarları izlemek ve disklerinizi daha fazla özelleştirmek nasıl erişileceğini ayrıntılı.

> [!NOTE]
> Bir API sürümünü kullanmak için önerilen daha `2016-04-30-preview` arasında önemli değişiklikler olduğu `2016-04-30-preview` ve `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Yönetilen disk ayarlarını varsayılan

Yönetilen disklerle bir VM oluşturmak için artık depolama alanı oluşturmak için ihtiyacınız kaynak hesabı ve sanal makine kaynağınıza şu şekilde güncelleştirebilirsiniz. Özellikle dikkat `apiVersion` yansıtır `2017-03-30` ve `osDisk` ve `dataDisks` artık VHD için belirli bir URI bakın. Ek özellikleri belirtilmeden dağıtırken, VM boyutuna bağlı olarak bir depolama türü diski kullanacak. Örneğin, bir Premium özelliğine sahip VM boyutu (boyutları Standard_D2s_v3 gibi adında "s" ile) kullanıyorsanız, sistem Premium_LRS depolama kullanır. Sku ayarını disk depolama türü belirtmek için kullanın. Hiçbir ad belirtilmediği takdirde biçiminin sürdüğünü `<VMName>_OsDisk_1_<randomstring>` işletim sistemi diski için ve `<VMName>_disk<#>_<randomstring>` her veri diski için. Varsayılan olarak, Azure disk şifrelemesini devre dışı; önbelleğe alma okuma/yazma işletim sistemi diski ve veri diskleri için yok içindir. Aşağıdaki örnekte yine de bir depolama hesabı bağımlılığı bu yalnızca tanılama için depolama ve disk depolama alanı için gerekli değildir ancak fark.

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Bir üst düzey bir yönetilen disk kaynağı kullanma

Disk yapılandırması sanal makine nesnesinde belirterek alternatif olarak, bir üst düzey disk kaynağı oluşturabilir ve sanal makine oluşturmanın bir parçası olarak ekleyin. Örneğin, aşağıdaki gibi bir veri diski olarak kullanmak için bir disk kaynağı oluşturabilirsiniz.

```json
{
    "type": "Microsoft.Compute/disks",
    "apiVersion": "2018-06-01",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

VM nesnesinde eklenmesi disk nesne başvuru. Oluşturulan yönetilen diskin kaynak Kimliğini belirtme `managedDisk` özellik VM oluşturulurken disk bağlantısını sağlar. `apiVersion` Kaynak VM için ayarlandığından `2017-03-30`. Disk kaynağında bir bağımlılık önce VM oluşturma başarıyla oluşturulduğundan emin olun eklenir. 

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Yönetilen kullanılabilirlik kümeleri ile yönetilen diskleri kullanarak VM'ler oluşturma

Yönetilen kullanılabilirlik ile Vm'leri yönetilen diskleri kullanarak kümeleri, ekleme `sku` resource ayarlayabilirsiniz ve kullanılabilirliği bir nesnesine `name` özelliğini `Aligned`. Bu özellik, diskler her VM için yeterince tek hata noktalarından kaçınmak üzere birbirinden yalıtılmasını sağlar. Ayrıca `apiVersion` kaynak kullanılabilirlik kümesi için ayarlanmış `2018-10-01`.

```json
{
    "type": "Microsoft.Compute/availabilitySets",
    "apiVersion": "2018-10-01",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="standard-ssd-disks"></a>Standart SSD disk

Standart SSD disk oluşturmak için Resource Manager şablonunda gereken parametreleri aşağıdadır:

* *apiVersion* Microsoft.Compute olarak ayarlanması için `2018-04-01` (veya üzeri)
* Belirtin *managedDisk.storageAccountType* olarak `StandardSSD_LRS`

Aşağıdaki örnekte gösterildiği *properties.storageProfile.osDisk* bölüm standart SSD diskleri kullanan bir VM için:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Standart SSD disk ile bir şablon oluşturma tam şablon örneği için bkz [standart SSD veri diskleri ile bir Windows görüntüsünden VM oluşturma](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/).

### <a name="additional-scenarios-and-customizations"></a>Ek senaryolar ve özelleştirme

REST API belirtimlerini hakkında tam bilgi için lütfen inceleyin [REST API belgeleri yönetilen disk oluşturma](/rest/api/manageddisks/disks/disks-create-or-update). Varsayılan ve API şablon dağıtımları aracılığıyla gönderilebilir kabul edilebilir değerler yanı sıra ek senaryolar bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen diskleri kullanmak için tam şablonları Azure hızlı başlangıç depo için aşağıdaki bağlantıları ziyaret edin.
    * [Yönetilen disk ile Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Yönetilen disk ile Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Yönetilen disk şablonların tam listesi](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Ziyaret [Azure yönetilen disklere genel bakış](../articles/virtual-machines/windows/managed-disks-overview.md) yönetilen diskler hakkında daha fazla bilgi için belge.
* Sanal Makine kaynakları için şablon başvuru belgeleri ziyaret ederek gözden [Microsoft.Compute/virtualMachines şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines) belge.
* Şablon başvuru belgeleri için disk kaynaklarını ziyaret ederek gözden [Microsoft.Compute/disks şablon başvurusu](/azure/templates/microsoft.compute/disks) belge.
* Azure sanal makine ölçek kümeleri, yönetilen diskleri kullanma hakkında daha fazla bilgi için ziyaret [veri disklerini ölçek kümeleri ile kullanma](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks) belge.
