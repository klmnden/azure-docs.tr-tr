---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3b596e5bad8202d88ea06c7eee114bec1063a35f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075700"
---
# <a name="enabling-azure-ultra-ssds"></a>Azure ultra SSD etkinleştirme

Azure ultra SSD, Azure Iaas Vm'leri için yüksek performans, yüksek IOPS ve tutarlı düşük gecikme süreli disk depolama alanı sunun. Bu yeni teklif, var olan diskleri tekliflerimizi aynı kullanılabilirlik düzeylerinde satırı performansının üst sağlar. Ultra yüksek SSD ek avantajları sanal makinelerinizi yeniden başlatmaya gerek kalmadan iş yüklerinizi yanı sıra disk performansını dinamik olarak değiştirme özelliği içerir. Ultra yüksek SSD SAP HANA, en çok katmanı veritabanları ve işlem yoğunluklu iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur.

Şu anda, ultra SSD olan Önizleme aşamasındadır ve gerekir [kaydetme](https://aka.ms/UltraSSDPreviewSignUp) erişebilmeleri için önizlemede.

Onayladıktan sonra hangi Doğu ultra diskinize dağıtmak için ABD 2 bölgesinde belirlemek için aşağıdaki komutlardan birini çalıştırın:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

Yanıt X olduğu bölge Doğu ABD 2 bölgesinde kullanmak için aşağıdaki formu, benzer olacaktır. X 1, 2 veya 3 olabilir.

|ResourceType  |Ad  |Location  |Bölgeler  |Kısıtlama  |Özellik  |Değer  |
|---------|---------|---------|---------|---------|---------|---------|
|diskler     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Komutundan yanıt gelmezse, geliyor özelliğine kaydınızı ya da hala beklemede veya henüz onaylanmadı.

Dağıtmak için hangi bölgeyi artık bildiğinize göre bu makalede, ilk Vm'lerinizi ultra SSD ile dağıtılan almak için dağıtım adımları izleyin.

## <a name="deploying-an-ultra-ssd"></a>Ultra yüksek bir SSD dağıtma

İlk olarak dağıtmak için VM boyutunu belirler. Bu önizleme kapsamında, yalnızca DsV3 ve EsV3 VM aileleri desteklenir. Bu ikinci tablo başvurmak [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) bu VM boyutları hakkında ek ayrıntılar için.
Ayrıca örneğe bakın [birden çok ultra SSD ile VM oluşturma](https://aka.ms/UltraSSDTemplate), birden çok ultra SSD ile VM oluşturma işlemini gösterir.

Aşağıdaki Resource Manager şablonu yeni/değiştirilmiş değişiklikleri açıklar: **apiVersion** için `Microsoft.Compute/virtualMachines` ve `Microsoft.Compute/Disks` olarak ayarlanmalıdır `2018-06-01` (veya üzeri).

Sku UltraSSD_LRS diski, disk kapasitesi, IOPS ve aktarım hızı Ultra yüksek bir disk oluşturmak için MB/sn olarak belirtin. Bir disk ile 1.024 GiB oluşturan bir örneği verilmiştir (GiB = 2 ^ 30 bayt), üst sınır: 80.000 IOPS ve 1.200 MB/sn (MB/sn = 10 ^ 6 bayt / saniye):

```json
"properties": {  
    "creationData": {  
    "createOption": "Empty"  
},  
"diskSizeGB": 1024,  
"diskIOPSReadWrite": 80000,  
"diskMBpsReadWrite": 1200,  
}
```

Bir ek özellik etkin, ultra belirtmek için sanal makinenin özellikleri ekleyin (başvurmak [örnek](https://aka.ms/UltraSSDTemplate) tam Resource Manager şablonu için):

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "properties": {
                    "hardwareProfile": {},
                    "additionalCapabilities" : {
                                    "ultraSSDEnabled" : "true"
                    },
                    "osProfile": {},
                    "storageProfile": {},
                    "networkProfile": {}
    }
}
```

VM oluşturulduktan sonra bölüm ve veri diskleri Biçimlendir ve bunları iş yükleriniz için yapılandırabilirsiniz.

## <a name="additional-ultra-ssd-scenarios"></a>Ek ultra SSD senaryoları

- VM oluşturma sırasında ultra SSD örtük olarak da oluşturulabilir. Ancak, bu diskleri (500) IOPS ve aktarım hızı (8 MiB/sn) için varsayılan değeri alacaktır.
- Uyumlu sanal makinelerinizi ek ultra SSD eklenebilir.
- Ultra yüksek SSD desteği (IOPS ve aktarım hızı) disk performansı özniteliklerini ayarlama Çalışma zamanında sanal makine diski ayırmayı olmadan. Disk performansı yeniden boyutlandırma işlemi bir diskte verildikten sonra bu değişikliğin gerçekten etkili bir saate kadar sürebilir.
- Disk kapasitesini büyüyen paylaştırılmamış olacak şekilde bir sanal makine gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Yeni disk türü denemek ister misiniz ve önizleme için henüz kaydolmamış [erişim aracılığıyla bu anketi isteği](https://aka.ms/UltraSSDPreviewSignUp).
