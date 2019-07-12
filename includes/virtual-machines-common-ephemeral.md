---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: a57335eccfce1e81fe0cc85ae6fa7b12aa27e1c3
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805886"
---
Kısa ömürlü işletim sistemi diskleri yerel sanal makine (VM) depolama alanında oluşturulur ve uzak bir Azure depolama için kaydedilmedi. İyi Durum bilgisiz iş yükleri, burada tek tek VM kesintilerine dayanıklı uygulamalar, ancak için daha fazla sanal makine dağıtım süresini tarafından etkilenen veya tek tek sanal makine örnekleri görüntüsü yeniden oluşturuluyor kısa ömürlü işletim sistemi diskleri çalışır. Kısa ömürlü işletim sistemi diski ile daha düşük okuma/yazma gecikme süresi işletim sistemi diski ve daha hızlı bir VM görüntüsü yeniden alın. 
 
Geçici diskler önemli özellikleri şunlardır: 
- Durum bilgisi olmayan uygulamalar için idealdir.
- Bunlar, Market hem de özel görüntüleri ile kullanılabilir.
- Hızlı sıfırlama veya yeniden görüntü oluşturma Vm'leri ve ölçeklendirme olanağı örnekleri özgün önyükleme durumuna ayarlayın.  
- Daha düşük gecikme süresi, geçici bir diskle için benzer. 
- Kısa ömürlü işletim sistemi diskleri ücretsiz olduğundan, işletim sistemi diski için hiçbir depolama maliyeti doğurur.
- Bunlar tüm Azure bölgelerinde kullanılabilir. 
- Kısa ömürlü işletim sistemi diski tarafından desteklenen [paylaşılan görüntü Galerisi](/azure/virtual-machines/linux/shared-image-galleries). 
 

 
Kalıcı ve kısa ömürlü işletim sistemi diskleri arasındaki temel farklar:

|                             | Kalıcı işletim sistemi diski                          | Kısa Ömürlü İşletim Sistemi Diski                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| İşletim sistemi diski için boyut sınırı      | 2 TiB                                                                                        | Önbellek boyutu için VM boyutunu veya 2TiB, hangisi daha küçükse. İçin **önbellek boyutu gib biriminde**, bkz: [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md), ve [GS](/azure/virtual-machines/linux/sizes-previous-gen#gs-series)              |
| Desteklenen VM boyutları          | Tümü                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Disk türü desteği           | Yönetilen ve yönetilmeyen işletim sistemi diski                                                                | Yalnızca yönetilen işletim sistemi diski                                                               |
| Bölge desteği              | Tüm bölgeler                                                                                  | Tüm bölgeler                              |
| Veri kalıcılığı            | İşletim sistemi diski için yazılan işletim sistemi disk verilerini Azure Depolama'da depolanır.                                  | İşletim sistemi diski için yazılan veriler yerel VM depolama alanına depolanır ve Azure Depolama'ya kalıcı değil. |
| Stop-serbest bırakılmış      | VM ölçek kümesi örneklerine durdu-serbest ve durdu-serbest durumundan yeniden başlatıldı | VM'ler ve ölçek kümesi örneklerine durdu-serbest olamaz                                  |
| Özelleştirilmiş işletim sistemi disk desteği | Evet                                                                                          | Hayır                                                                                 |
| İşletim sistemi diski yeniden boyutlandırma              | Desteklenen VM oluşturma sırasında ve sonra VM stop-serbest bırakıldı                                | VM oluşturma sırasında desteklenen                                                  |
| Yeni bir VM boyutu için yeniden boyutlandırma   | İşletim sistemi disk veriler korunur                                                                    | İşletim sistemi diskini şirket verileri silinir, işletim sistemi yeniden sağlandı                                      |

## <a name="size-requirements"></a>Boyutu gereksinimleri

VM önbellek boyutu en fazla VM ve örnek görüntüleri dağıtabilirsiniz. Örneğin, standart Windows Server görüntülerini Market 127 GiB daha büyük bir önbellek olan bir VM boyutu gerektiği anlamına gelir hakkında 127 GiB vardır. Bu durumda, [Standard_DS2_v2](/azure/virtual-machines/windows/sizes-general#dsv2-series) yeteri kadar büyük değil 86 GiB önbellek boyutuna sahip. Standard_DS2_v2 yeterince büyük bir önbellek boyutu 172 GiB vardır. Bu resimle kullanabileceğiniz DSv2 serisi Standard_DS3_v2 bu durumda, küçük boyutudur. Tarafından belirtilen Market ve Windows Server görüntülerini temel Linux görüntüleri `[smallsize]` yaklaşık 30 GiB dağıtılmıştır ve kullanılabilir VM boyutlarını çoğunu kullanabilirsiniz.

Geçici diskler, VM boyutu Premium depolamayı destekler de gerektirir. Genellikle (ama her zaman kullanılmaz) boyutlarına sahip bir `s` adında, DSv2 ve EsV3 gibi. Daha fazla bilgi için [Azure VM boyutlarını](../articles/virtual-machines/linux/sizes.md) boyutları destekleyen Premium depolama Ayrıntılar için.

## <a name="powershell"></a>PowerShell

Bir PowerShell VM dağıtımı için kısa ömürlü diski kullanmak için [kümesi AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) , VM yapılandırması. Ayarlama `-DiffDiskSetting` için `Local` ve `-Caching` için `ReadOnly`.     

```powershell
Set-AzVMOSDisk -DiffDiskSetting Local -Caching ReadOnly
```

Ölçek kümesi dağıtımları için kullanmak [kümesi AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) yapılandırmanızda cmdlet'i. Ayarlama `-DiffDiskSetting` için `Local` ve `-Caching` için `ReadOnly`.


```powershell
Set-AzVmssStorageProfile -DiffDiskSetting Local -OsDiskCaching ReadOnly
```

## <a name="cli"></a>CLI

CLI VM dağıtımı için kısa ömürlü diski kullanmak için ayarlanmış `--ephemeral-os-disk` parametresinde [az vm oluşturma](/cli/azure/vm#az-vm-create) için `true` ve `--os-disk-caching` parametresi `ReadOnly`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --ephemeral-os-disk true \
  --os-disk-caching ReadOnly \
  --admin-username azureuser \
  --generate-ssh-keys
```

Ölçek kümeleri için aynı kullandığınız `--ephemeral-os-disk true` parametresi için [az vmss oluşturma](/cli/azure/vmss#az-vmss-create) ayarlayıp `--os-disk-caching` parametresi `ReadOnly`.

## <a name="portal"></a>Portal   

Azure portalında açarak sanal makine dağıtımı sırasında kısa ömürlü diskleri kullanmayı da tercih edebilirsiniz **Gelişmiş** bölümünü **diskleri** sekmesi. İçin **kısa ömürlü işletim sistemi diski kullanma** seçin **Evet**.

![Kısa ömürlü işletim sistemi diski kullanmak seçme radyo düğmesini gösteren ekran görüntüsü](./media/virtual-machines-common-ephemeral/ephemeral-portal.png)

Kısa ömürlü diski kullanma seçeneği devre dışı ise, işletim sistemi görüntüsü büyük bir önbellek boyutu yok veya Premium depolama desteği olmayan bir VM boyutu seçmiş olabilirsiniz. Geri Git **Temelleri** sayfasında ve başka bir sanal makine boyutunu seçmeyi deneyin.

Portalı kullanarak kısa ömürlü işletim sistemi diskleri ile ölçek kümeleri de oluşturabilirsiniz. Yalnızca bir VM boyutu yeteri kadar büyük önbellek boyutu ve sonra da seçtiğinizden emin olun **kısa ömürlü işletim sistemi diski kullanma** seçin **Evet**.

![Kısa ömürlü işletim sistemi diski ölçek kümeniz için kullanılacak Seçim radyo düğmesini gösteren ekran görüntüsü](./media/virtual-machines-common-ephemeral/scale-set.png)

## <a name="scale-set-template-deployment"></a>Ölçek kümesi şablon dağıtımı  
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

## <a name="vm-template-deployment"></a>VM şablon dağıtımı 
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

Y: Platform ve özel görüntüler, burada tüm okuma/yazma için işletim sistemi diski sanal makineyle aynı düğümde yerel olacaktır VM önbellek boyutu kadar destekliyoruz. 

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
