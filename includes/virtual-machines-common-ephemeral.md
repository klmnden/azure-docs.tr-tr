---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/02/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 3e9885466d422a0428311ed3013e2ab34341cd25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66391470"
---
Kısa ömürlü işletim sistemi diskleri yerel sanal makine (VM) depolama alanında oluşturulur ve uzak Azure Depolama'da kalıcı hale değil. Burada uygulamaları tek tek VM kesintilerine dayanıklı olsa da, büyük ölçekli dağıtımlar için geçen süreyi veya tek tek sanal makine örneği görüntüsünü yeniden oluşturmak zaman hakkında daha fazla endişe iyi durum bilgisiz iş yükleri için kısa ömürlü işletim sistemi diskleri çalışır. Resource Manager dağıtım modeline taşımak Klasik dağıtım modeli kullanılarak dağıtılan uygulamalar için de uygundur. Kısa Ömürlü İşletim Sistemi diskleriyle, İşletim Sistemi disklerinde okuma/yazma gecikme süresinin kısaldığını ve yeniden görüntü oluşturmanın hızlandığını görebilirsiniz. Ayrıca, kısa ömürlü işletim sistemi diski ücretsizdir, işletim sistemi diski için hiçbir depolama maliyeti doğurur. 
 
Geçici diskler önemli özellikleri şunlardır: 
- Bunlar, hem Market görüntüleri hem de özel görüntüleri ile kullanılabilir.
- VM önbellek boyutu en fazla VM görüntülerini dağıtabilirsiniz.
- Hızlı, sıfırlama veya Vm'lerini özgün durumuna önyükleme görüntüsünü yeniden oluşturma yeteneği.  
- Alt çalışma zamanı gecikme için geçici bir diskle benzer. 
- İşletim sistemi diski için hiçbir ücret. 
 
 
Kalıcı ve kısa ömürlü işletim sistemi diskleri arasındaki temel farklar:

|                             | Kalıcı işletim sistemi diski                          | Kısa Ömürlü İşletim Sistemi Diski                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| İşletim sistemi diski için boyut sınırı      | 2 TiB                                                                                        | Önbellek boyutu için VM boyutunu veya 2TiB, hangisi daha küçük - [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md), ve [GS](../articles/virtual-machines/linux/sizes-memory.md)              |
| Desteklenen VM boyutları          | Tümü                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Disk türü desteği           | Yönetilen ve yönetilmeyen işletim sistemi diski                                                                | Yalnızca yönetilen işletim sistemi diski                                                               |
| Bölge desteği              | Tüm bölgeler                                                                                  | Tüm bölgeler                              |
| Veri kalıcılığı            | İşletim sistemi diski için yazılan işletim sistemi disk verilerini Azure Depolama'da depolanır.                                  | İşletim sistemi diski için yazılan veriler yerel VM depolama alanına depolanır ve Azure Depolama'ya kalıcı değil. |
| Stop-serbest bırakılmış      | VM ölçek kümesi örneklerine durdu-serbest ve durdu-serbest durumundan yeniden başlatıldı | VM'ler ve ölçek kümesi örneklerine durdu-serbest olamaz                                  |
| Özelleştirilmiş işletim sistemi disk desteği | Evet                                                                                          | Hayır                                                                                 |
| İşletim sistemi diski yeniden boyutlandırma              | Desteklenen VM oluşturma sırasında ve sonra VM stop-serbest bırakıldı                                | VM oluşturma sırasında desteklenen                                                  |
| Yeni bir VM boyutu için yeniden boyutlandırma   | İşletim sistemi disk veriler korunur                                                                    | İşletim sistemi diskini şirket verileri silinir, işletim sistemi yeniden sağlandı                                      |

## <a name="scale-set-deployment"></a>Ölçek kümesi dağıtımı  
Kısa ömürlü işletim sistemi diski kullanan bir ölçek kümesi oluşturma işlemini eklemektir `diffDiskSettings` özelliğini `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` şablonunda kaynak türü. Ayrıca, önbelleğe alma ilkesini ayarlamanız gerekir `ReadOnly` kısa ömürlü işletim sistemi diski için. 


```json
{ 
  "type": "Microsoft.Compute/virtualMachineScaleSets", 
  "name": "myScaleSet", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "sku": { 
    "name": "Standard_DS2_v2", 
    "capacity": "2" 
  }, 
  "properties": { 
    "upgradePolicy": { 
      "mode": "Automatic" 
    }, 
    "virtualMachineProfile": { 
       "storageProfile": { 
        "osDisk": { 
          "diffDiskSettings": { 
                "option": "Local" 
          }, 
          "caching": "ReadOnly", 
          "createOption": "FromImage" 
        }, 
        "imageReference":  { 
          "publisher": "Canonical", 
          "offer": "UbuntuServer", 
          "sku": "16.04-LTS", 
          "version": "latest" 
        } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvmss", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
  } 
}  
```

## <a name="vm-deployment"></a>VM dağıtımı 
Şablon kullanarak kısa ömürlü işletim sistemi diski ile bir VM dağıtabilirsiniz. Kısa ömürlü işletim sistemi diskleri kullanan bir VM oluşturma işlemini eklemektir `diffDiskSettings` şablondaki Microsoft.Compute/virtualMachines kaynak türü için özellik. Ayrıca, önbelleğe alma ilkesini ayarlamanız gerekir `ReadOnly` kısa ömürlü işletim sistemi diski için. 

```json
{ 
  "type": "Microsoft.Compute/virtualMachines", 
  "name": "myVirtualMachine", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "properties": { 
       "storageProfile": { 
            "osDisk": { 
              "diffDiskSettings": { 
                "option": "Local" 
              }, 
              "caching": "ReadOnly", 
              "createOption": "FromImage" 
            }, 
            "imageReference": { 
                "publisher": "MicrosoftWindowsServer", 
                "offer": "WindowsServer", 
                "sku": "2016-Datacenter-smalldisk", 
                "version": "latest" 
            }, 
            "hardwareProfile": { 
                 "vmSize": "Standard_DS2_v2" 
             } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvirtualmachine", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
 } 
```


## <a name="reimage-a-vm-using-rest"></a>REST kullanarak VM görüntüsünü yeniden oluştur
Şu anda, kısa ömürlü işletim sistemi diski ile bir sanal makine örneği görüntüsünü yeniden oluşturmak için yalnızca REST API kullanarak aracılığıyla yöntemdir. Ölçek kümeleri için yeniden görüntü zaten Powershell, CLI ve portal kullanılabilir.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Yerel işletim sistemi disk boyutu nedir?**

Y: Önizleme için platform ve/veya görüntüleri burada tüm okuma/yazma için işletim sistemi diski sanal makineyle aynı düğümde yerel olacaktır VM önbellek boyutu kadar destekleyeceğiz. 

**S: Kısa ömürlü işletim sistemi diskini yeniden boyutlandırılabilir?**

Y: Hayır, kısa ömürlü işletim sistemi diski sağlandıktan sonra işletim sistemi diski yeniden boyutlandırılamaz. 

**S: Kısa ömürlü bir VM'ye yönetilen bir disk ekleyebilir miyim?**

Y: Evet, kısa ömürlü işletim sistemi diski kullanan bir VM için yönetilen bir veri diski ekleyebilirsiniz. 

**S: Tüm VM boyutları için kısa ömürlü işletim sistemi diskleri desteklenecek?**

Y: Hayır, Premium depolama VM boyutlarının hepsi, B serisi dışında desteklenen (DS, ES, FS, GS ve M) olan N serisi ve H serisi boyutları.  
 
**S: Kısa ömürlü işletim sistemi diski mevcut Vm'lere uygulanması ve ölçek kümeleri?**

Y: Hayır, işletim sistemi diski yalnızca VM sırasında kullanılabilir ve ölçek kümesi oluşturma kısa ömürlü. 

**S: İşletim sistemi diskleri kısa ömürlü ve normal bir ölçek kümesindeki karıştırabilir miyim?**

Y: Hayır, kısa ömürlü ve sürekli işletim sistemi disk örnekleri aynı ölçek kümesindeki bir karışımını sahip olamaz. 

**S: Kısa ömürlü işletim sistemi diski, Powershell veya CLI kullanarak oluşturulabilir mi?**

Y: Evet, REST, şablonlar, PowerShell ve CLI kullanarak kısa ömürlü işletim sistemi diski ile Vm'leri oluşturabilirsiniz.

**S: Hangi özelliklerin kısa ömürlü işletim sistemi diski ile desteklenmez?**

Y: Geçici diskler desteklemez:
- VM görüntüsü yakalama
- Disk anlık görüntüleri 
- Azure Disk Şifrelemesi 
- Azure Backup
- Azure Site Recovery  
- İşletim sistemi diskini değiştirme 
