---
title: Azure kaynaklarınızı keşfedin
description: Kaynaklarınızı keşfedin ve nasıl bağlandığını keşfetmek için kaynak grafik sorgu dilini kullanmayı öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 3174e74dc3fb8c56279c0c9708a67d99ae19724a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795979"
---
# <a name="explore-your-azure-resources-with-resource-graph"></a>Kaynak Grafiği ile Azure kaynaklarınızı keşfedin

Azure Kaynak Grafiği keşfedin ve hızla ve uygun ölçekte Azure kaynaklarınıza bulma olanağı sağlar. Hızlı yanıtlar için tasarlanmış, ortamınız hakkında ayrıca Azure kaynaklarınızın olun özellikleri hakkında bilgi edinmek için harika bir yoludur.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="explore-virtual-machines"></a>Sanal makineleri keşfedin

Azure'da ortak bir kaynak bir sanal makinedir. Bir kaynak türü sanal makineler, sorgulanabilir birçok özelliklere sahip. Her bir özellik, filtreleme veya tam olarak aradığınız kaynak bulmak için bir seçenek sunar.

### <a name="virtual-machine-discovery"></a>Sanal makine bulma

Tek bir VM bizim ortamından almak ve dönen özelliklerine bakmak için basit bir sorgu başlayalım.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| limit 1
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | limit 1" | ConvertTo-Json -Depth 100
```

> [!NOTE]
> Azure PowerShell `Search-AzGraph` cmdlet döndürür bir **PSCustomObject** varsayılan olarak. Aynı Azure CLI tarafından döndürülen olarak ara çıktı elde etmek `ConvertTo-Json` cmdlet'i kullanılır. İçin varsayılan değer **derinliği** olduğu _2_. Bu ayarın _100_ döndürülen tüm düzeyleri dönüştürmeniz gerekir.

JSON sonuçları şu örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "aliases": {
      "Microsoft.Compute/imageId": null,
      "Microsoft.Compute/imageOffer": "WindowsServer",
      "Microsoft.Compute/imagePublisher": "MicrosoftWindowsServer",
      "Microsoft.Compute/imageSku": "2016-Datacenter",
      "Microsoft.Compute/imageVersion": "latest",
      "Microsoft.Compute/licenseType": null,
      "Microsoft.Compute/virtualMachines/availabilitySet.id": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics.enabled": null,
      "Microsoft.Compute/virtualMachines/diagnosticsProfile.bootDiagnostics.storageUri": null,
      "Microsoft.Compute/virtualMachines/imageOffer": "WindowsServer",
      "Microsoft.Compute/virtualMachines/imagePublisher": "MicrosoftWindowsServer",
      "Microsoft.Compute/virtualMachines/imageSku": "2016-Datacenter",
      "Microsoft.Compute/virtualMachines/imageVersion": "latest",
      "Microsoft.Compute/virtualMachines/networkInterfaces[*].id": [
        "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535"
      ],
      "Microsoft.Compute/virtualMachines/osDisk.Uri": null,
      "Microsoft.Compute/virtualMachines/osProfile.adminPassword": null,
      "Microsoft.Compute/virtualMachines/osProfile.adminUsername": "localAdmin",
      "Microsoft.Compute/virtualMachines/osProfile.linuxConfiguration": null,
      "Microsoft.Compute/virtualMachines/osProfile.linuxConfiguration.disablePasswordAuthentication": null,
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration": {
        "enableAutomaticUpdates": true,
        "provisionVMAgent": true
      },
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration.enableAutomaticUpdates": true,
      "Microsoft.Compute/virtualMachines/osProfile.windowsConfiguration.provisionVMAgent": true,
      "Microsoft.Compute/virtualMachines/sku.name": "Standard_B2s",
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].caching": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].createOption": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].diskSizeGB": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].image.uri": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].lun": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].managedDisk.id": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].managedDisk.storageAccountType": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].name": [],
      "Microsoft.Compute/virtualMachines/storageProfile.dataDisks[*].vhd.uri": [],
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.caching": "ReadWrite",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.createOption": "FromImage",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.encryptionSettings": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.encryptionSettings.enabled": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.managedDisk.storageAccountType": "Premium_LRS",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.osType": "Windows",
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.vhd": null,
      "Microsoft.Compute/virtualMachines/storageProfile.osDisk.vhd.uri": null
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1",
    "plan": {},
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_B2s"
      },
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "/subscriptions/<subscriptionId>/MyResourceGroup/providers/Microsoft.Network/networkInterfaces/contosovm1535",
            "resourceGroup": "MyResourceGroup"
          }
        ]
      },
      "osProfile": {
        "adminUsername": "localAdmin",
        "computerName": "ContosoVM1",
        "secrets": [],
        "windowsConfiguration": {
          "enableAutomaticUpdates": true,
          "provisionVMAgent": true
        }
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "dataDisks": [],
        "imageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": 127,
          "managedDisk": {
            "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
            "resourceGroup": "MyResourceGroup",
            "storageAccountType": "Premium_LRS"
          },
          "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
          "osType": "Windows"
        }
      },
      "vmId": "bbb9b451-6dc7-4117-bec5-c971eb1118c6"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {},
    "subscriptionId": "<subscriptionId>",
    "tags": {},
    "type": "microsoft.compute/virtualmachines"
  }
]
```

İlk altındaki özellikler kümesini **diğer adlar** ilgili özellik değerlerini sunar. Diğer adları hakkında daha fazla bilgi edinin ve hangi diğer adların kullanılabilir olduğunu öğrenmek için bkz: [Azure İlkesi tanım yapısı - diğer adlar](../../policy/concepts/definition-structure.md#aliases). Diğer adlar, öncelikli olarak kuruluş kuralları ve idare uyumluluğu yönetmek için Azure İlkesi tarafından kullanılır.

Diğer özellikleri bize sanal makine kaynağının, SKU, işletim sistemi, diskleri, etiketler, her şeyi hakkında ek bilgi ve üyesi olduğu kaynak grubu ve abonelik.

### <a name="virtual-machines-by-location"></a>Konuma göre sanal makineler

Sanal makineler kaynak hakkında öğrendiklerimizi uygularsanız kullanalım **konumu** özelliği konuma göre tüm sanal makinelerin sayısı. Sorguyu güncellemek için biz limitinizi kaldırmak ve konum değerlerin sayısını özetler.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by location
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by location"
```

JSON sonuçları şu örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "count_": 386,
    "location": "eastus"
  },
  {
    "count_": 215,
    "location": "southcentralus"
  },
  {
    "count_": 59,
    "location": "westus"
  }
]
```

Her bir Azure bölgesinde sahibiz kaç sanal makineler artık görebiliriz.

### <a name="virtual-machines-by-sku"></a>Sanal makineler tarafından SKU

Özgün sanal makine özellikleri için yukarıda bahsedilen SKU boyutunu sahip tüm sanal makineleri bulmak deneyelim **Standard_B2s**. Döndürülen JSON yanıtına bakarak, içinde depolanan olduğunu görüyoruz **properties.hardwareprofile.vmsize**. Bu boyut uyan ve yalnızca VM ve bölge adını döndürün tüm Vm'leri bulmak için sorgu güncelleştireceğiz.

```Query
where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s
| project name, resourceGroup"
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | project name, resourceGroup"
```

### <a name="virtual-machines-connected-to-premium-managed-disks"></a>Premium yönetilen disklere bağlı sanal makineleri

Bu bağlı premium yönetilen diskler ayrıntılarını almak istediyseniz **Standard_B2s** sanal makineler, size bu yönetilen disklerin kaynak kimliği bulunun için sorguyu genişletebilirsiniz.

```Query
where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s'
| extend disk = properties.storageProfile.osDisk.managedDisk
| where disk.storageAccountType == 'Premium_LRS'
| project disk.id
```

> [!NOTE]
> SKU başka bir yolunu kullanarak olabilirdi **diğer adlar** özelliği **Microsoft.Compute/virtualMachines/sku.name**.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

```azurepowershell-interactive
  Search-AzGraph -Query "where type =~ 'Microsoft.Compute/virtualmachines' and properties.hardwareProfile.vmSize == 'Standard_B2s' | extend disk = properties.storageProfile.osDisk.managedDisk | where disk.storageAccountType == 'Premium_LRS' | project disk.id"
```

Disk kimlikleri listesini sonucudur.

### <a name="managed-disk-discovery"></a>Yönetilen disk bulma

Önceki sorgunun ilk kaydıyla şunları ilk sanal makineye bağlı yönetilen disk üzerinde bulunan özellikler keşfedeceğiz. Güncelleştirilmiş sorgu disk kimliği kullanır ve türü değiştirir.

Örnek, önceki sorgudan örnek çıktı:

```json
[
  {
    "disk_id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166"
  }
]
```

```Query
where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'
```

Sorguyu çalıştırmadan önce nasıl biz biliyor muydunuz **türü** artık olmalıdır **Microsoft.Compute/disks**?
Tam kimliği bakarsanız, göreceğiniz **/providers/Microsoft.Compute/disks/** dizesinin parçası olarak. Bu dize parçasını aramak için ne tür dair bir ipucu sağlar. Alternatif bir yöntem türü sınırı kaldırın ve bunun yerine yalnızca arama tarafından Kimliği alanı olacaktır. Kimliği benzersiz olduğundan, yalnızca bir kayıtla döndürülür ve **türü** özelliği üzerindeki bu ayrıntı sağlar.

> [!NOTE]
> Çalışmak bu örnek için kendi ortamından bir sonuçla Kimliği alanı değiştirmelisiniz.

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'Microsoft.Compute/disks' and id == '/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166'"
```

JSON sonuçları şu örneğe benzer şekilde yapılandırılmıştır:

```json
[
  {
    "aliases": {
      "Microsoft.Compute/disks/sku.name": "Premium_LRS",
      "Microsoft.Compute/imageId": null,
      "Microsoft.Compute/imageOffer": null,
      "Microsoft.Compute/imagePublisher": null,
      "Microsoft.Compute/imageSku": null,
      "Microsoft.Compute/imageVersion": null
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/disks/ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "kind": "",
    "location": "westus2",
    "managedBy": "",
    "name": "ContosoVM1_OsDisk_1_9676b7e1b3c44e2cb672338ebe6f5166",
    "plan": {},
    "properties": {
      "creationData": {
        "createOption": "Empty"
      },
      "diskSizeGB": 127,
      "diskState": "ActiveSAS",
      "provisioningState": "Succeeded",
      "timeCreated": "2018-09-14T12:17:32.2570000Z"
    },
    "resourceGroup": "MyResourceGroup",
    "sku": {
      "name": "Premium_LRS",
      "tier": "Premium"
    },
    "subscriptionId": "<subscriptionId>",
    "tags": {
      "environment": "prod"
    },
    "type": "microsoft.compute/disks"
  }
]
```

## <a name="explore-virtual-machines-to-find-public-ip-addresses"></a>Genel IP adreslerini bulmak için sanal makineleri keşfedin

İlk sorgu bu Azure CLI kümesi bulur ve tüm ağ arabirimleri (NIC) kaynaklarına sanal makinelere bağlanan depolar. Ardından NIC listesini bir genel IP adresidir her IP adresi kaynağı bulmak için kullanır ve bu değerleri depolamak. Son olarak, genel IP adreslerinden oluşan bir liste sağlar.

```azurecli-interactive
# Use Resource Graph to get all NICs and store in the 'nic' variable
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project nic = tostring(properties['networkProfile']['networkInterfaces'][0]['id']) | where isnotempty(nic) | distinct nic | limit 20" --output table | tail -n +3 > nics.txt

# Review the output of the query stored in 'nics.txt'
cat nics.txt
```

Kullanım `nics.txt` ilgili ağ arabiriminin kaynakların ayrıntılarını almak için sonraki sorgu dosyasında bir genel IP adresi bir NIC'ye bağlı olduğu

```azurecli-interactive
# Use Resource Graph with the 'nics.txt' file to get all related public IP addresses and store in 'publicIp.txt' file
az graph query -q="where type =~ 'Microsoft.Network/networkInterfaces' | where id in ('$(awk -vORS="','" '{print $0}' nics.txt | sed 's/,$//')') | project publicIp = tostring(properties['ipConfigurations'][0]['properties']['publicIPAddress']['id']) | where isnotempty(publicIp) | distinct publicIp" --output table | tail -n +3 > ips.txt

# Review the output of the query stored in 'ips.txt'
cat ips.txt
```

Son, genel IP adresi kaynakların listesini depolanan kullanımı `ips.txt` bunları gerçek genel IP adresini almak ve görüntülemek için.

```azurecli-interactive
# Use Resource Graph with the 'ips.txt' file to get the IP address of the public IP address resources
az graph query -q="where type =~ 'Microsoft.Network/publicIPAddresses' | where id in ('$(awk -vORS="','" '{print $0}' ips.txt | sed 's/,$//')') | project ip = tostring(properties['ipAddress']) | where isnotempty(ip) | distinct ip" --output table
```

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu dili](query-language.md) hakkında daha fazla bilgi edinin
- Kullanımda dili bakın [başlangıç sorguları](../samples/starter.md)
- Bkz: Gelişmiş kullanır [Gelişmiş sorgular](../samples/advanced.md)