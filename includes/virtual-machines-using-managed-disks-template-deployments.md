# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonları diskleri kullanılarak yönetilir

Bu belge sanal makineler sağlamak için Azure Resource Manager şablonları kullanarak yönetilen ve yönetilmeyen diskler arasındaki farklar açıklanmaktadır. Bu, yönetilmeyen diskleri yönetilen disklere kullanarak var olan şablonları güncelleştirme yardımcı olur. Başvuru için kullanıyoruz [101 vm basit windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) şablon bir kılavuz olarak. Her ikisini de kullanarak şablonu görebilirsiniz [yönetilen disklerde](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) ve kullanarak bir önceki sürüm [yönetilmeyen diskleri](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) doğrudan karşılaştırmak istiyorsanız.

## <a name="unmanaged-disks-template-formatting"></a>Yönetilmeyen diskleri şablonu biçimlendirmesi

Başlamak için biz göz nasıl yönetilmeyen disklere dağıtılır. Yönetilmeyen diskleri oluştururken, VHD dosyalarını tutmak için bir depolama hesabı gerekir. Yeni bir depolama hesabı oluşturun veya zaten varolan bir kullanın. Bu makale yeni bir depolama hesabının nasıl oluşturulacağını gösterir. Bunu başarmak için aşağıda gösterildiği gibi kaynakları blok depolama hesabı kaynak gerekir.

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

Sanal makine nesnesi içinde bir bağımlılığı önce sanal makine oluşturulduğundan emin olmak için depolama hesabı gerekir. İçinde `storageProfile` bölümünde, ardından belirttiğimiz depolama hesabına başvuruyor ve işletim sistemi diski ve veri diskleri için gerekli VHD konumunun tam URI. 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
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

## <a name="managed-disks-template-formatting"></a>Şablon diskleri biçimlendirme yönetilen

Azure yönetilen disklerle diski bir üst düzey kaynak haline gelir ve artık kullanıcı tarafından oluşturulacak bir depolama hesabı gerektirir. Yönetilen diskleri ilk ortaya çıkarılan `2016-04-30-preview` API sürümü, bunlar tüm sonraki API sürümlerinde kullanılabilir ve varsayılan disk türü sunulmuştur. Aşağıdaki bölümlerde, varsayılan ayarları'nda yol ve disklerinizi daha fazla özelleştirmek nasıl ayrıntı.

> [!NOTE]
> Bir API sürümü kullanmak için önerilen daha `2016-04-30-preview` arasında önemli değişiklikler olduğu `2016-04-30-preview` ve `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Varsayılan yönetilen disk ayarları

Yönetilen disklerle bir VM oluşturmak için artık depolama oluşturmanıza gerek hesap kaynak ve sanal makine kaynağınızın şu şekilde güncelleştirebilirsiniz. Özellikle dikkat edin `apiVersion` yansıtır `2017-03-30` ve `osDisk` ve `dataDisks` artık VHD için belirli bir URI bakın. Ek özellikleri belirtmeden dağıtırken diski kullanacak [standart LRS depolama](../articles/storage/common/storage-redundancy.md). Ad belirtilmezse, biçimini alır `<VMName>_OsDisk_1_<randomstring>` işletim sistemi diski için ve `<VMName>_disk<#>_<randomstring>` her veri diski için. Varsayılan olarak, Azure disk şifrelemesi; devre dışı önbelleğe alma okuma/yazma işletim sistemi diski ve veri diskleri için hiçbiri içindir. Aşağıdaki örnekte hala bir depolama hesabı bağımlılık, bu yalnızca tanılama için depolama ve disk depolaması için gerekli değildir ancak fark.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
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

### <a name="using-a-top-level-managed-disk-resource"></a>Bir üst düzey yönetilen disk kaynağı kullanarak

Disk yapılandırması sanal makine nesnesinde belirterek alternatif olarak, bir üst düzey disk kaynağı oluşturun ve sanal makine oluşturmanın bir parçası olarak ekleyin. Örneğin, aşağıdaki gibi bir veri diski kullanmak için bir disk kaynağı oluşturabiliriz.

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
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

VM nesnesi içinde Biz bu disk nesne eklenmiş sonra başvurabilirsiniz. Oluşturduğumuz, yönetilen disk kaynak Kimliğini belirtme `managedDisk` özelliği VM oluşturulduğunda ek diskin izin verir. Unutmayın `apiVersion` VM için kaynak ayarlamak `2017-03-30`. Ayrıca, bir bağımlılık başarıyla VM oluşturması işleminden önce oluşturulan emin olmak için disk kaynağına oluşturduk olduğunu unutmayın. 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
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

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Yönetilen diskleri kullanarak VM'ler ile yönetilen kullanılabilirlik kümeleri oluşturma

Yönetilen oluşturmak için kullanılabilirlik VM'ler ile yönetilen diskleri kullanarak ayarlar, ekleme `sku` kaynak ve ayarlanmış kullanılabilirlik nesnesine `name` özelliğine `Aligned`. Bu, diskler her VM için yeterince yalıtılmış tek hata noktaları bulundurmaktan önlemek için birbirinden olmasını sağlar. Ayrıca `apiVersion` kaynak kullanılabilirlik kümesi için ayarlamak `2017-03-30`.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
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

### <a name="additional-scenarios-and-customizations"></a>İlave Senaryolar ve özelleştirmeleri

REST API belirtimlerini hakkında tam bilgi bulmak için lütfen inceleyin [yönetilen bir disk REST API belgeleri oluşturmak](/rest/api/manageddisks/disks/disks-create-or-update). Varsayılan ve API şablon dağıtımlarına aracılığıyla gönderilebilir kabul edilebilir değerler yanı sıra ek senaryolar bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen diskleri kullanmak için tam şablonları aşağıdaki Azure hızlı başlangıç depodaki bağlantıları ziyaret edin.
    * [Windows VM yönetilen diski](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Linux VM yönetilen diski](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Yönetilen disk şablonları tam listesi](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Ziyaret [Azure yönetilen diskleri genel bakış](../articles/virtual-machines/windows/managed-disks-overview.md) belge yönetilen diskler hakkında daha fazla bilgi edinin.
* Sanal Makine kaynakları için şablon başvuru belgeleri ziyaret ederek gözden [Microsoft.Compute/virtualMachines şablon başvurusu](/templates/microsoft.compute/virtualmachines) belge.
* Disk kaynakları için şablon başvuru belgeleri ziyaret ederek gözden [Microsoft.Compute/disks şablon başvurusu](/templates/microsoft.compute/disks) belge.
 
