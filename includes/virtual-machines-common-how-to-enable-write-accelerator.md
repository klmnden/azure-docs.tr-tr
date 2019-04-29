---
title: include dosyası
description: include dosyası
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 02/22/2019
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 72d9ec52732a78e39f6481e2cb2d40f17f86f028
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60478165"
---
# <a name="enable-write-accelerator"></a>Yazma hızlandırıcıyı etkinleştirme

Yazma Hızlandırıcı bir disk için M serisi sanal makinelerde (VM) Azure yönetilen diskler ile Premium depolama yalnızca bir özelliktir. Adını belirten, amacı işlevselliği, Azure Premium Depolama'ya yönelik yazma işlemlerinin g/ç gecikme süresini iyileştirmek için aynıdır. Yazma Hızlandırıcı, ideal olarak diske modern veritabanları için yüksek performanslı şekilde kalıcı hale getirmek için günlük dosyası güncelleştirmeleri nerede gereken uygundur.

Yazma Hızlandırıcı M serisi vm'lerde genel bulutta kullanıma sunulacaktır.

## <a name="planning-for-using-write-accelerator"></a>Yazma Hızlandırıcı kullanmak için planlama

Yazma Hızlandırıcı, işlem günlüğü içeren veya DBMS günlüklerini Yinele birimler için kullanılmalıdır. Bu özellik, günlük diskleri karşı kullanılmak üzere iyileştirilmiştir DBMS veri hacimleri için yazma Hızlandırıcı kullanmak üzere önerilmez.

Yazma Hızlandırıcı, yalnızca çalışır birlikte [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/).

> [!IMPORTANT]
> Sanal makinenin işletim sistemi diski yazma Hızlandırıcı etkinleştirme VM yeniden başlatılır.
>
> Windows disk veya birim yöneticisi, Windows depolama alanları, Windows ölçek genişletme dosya sunucusu (SOFS), Linux LVM veya MDADM, Azure disk erişme iş yükü birden çok disk dışında toplu yapının bir parçası olmayan mevcut bir Azure diske yazma Hızlandırıcı etkinleştirmek için kapatılması gerekiyor. Azure diski kullanan veritabanı uygulamaları kapatılmalıdır.
>
> Windows depolama alanları, Windows ölçek genişletme dosya sunucusu (SOFS), Linux LVM veya MDADM, etkinleştirmek veya yazma Hızlandırıcı, birden çok Azure Premium depolama diskleri dışında yapılandırılan ve Windows disk veya birim Yöneticisi'ni kullanarak Şerit var olan bir birim için devre dışı bırakmak istiyorsanız, tüm disk birimi oluşturma etkin veya yazma Hızlandırıcı için ayrı adımlarda devre dışı. **Etkinleştirme veya bu tür bir yapılandırma yazma Hızlandırıcı devre dışı bırakma önce Azure sanal makineyi**.

Yazma Hızlandırıcı için işletim sistemi diskleri etkinleştirme SAP ilgili sanal makine yapılandırmaları için gerekli olmamalıdır.

### <a name="restrictions-when-using-write-accelerator"></a>Yazma Hızlandırıcı kullanımına yönelik kısıtlamalar

Yazma Hızlandırıcı Azure bir disk/VHD için kullanırken bu kısıtlamalar uygulanır:

- Premium disk önbelleği 'None' olarak ayarlanmalıdır ya da 'Read Only'. Diğer tüm önbelleğe alma modu desteklenmez.
- Anlık görüntü şu anda desteklenmemektedir yazma Hızlandırıcısı etkinleştirilmiş disk için. Azure Backup hizmeti, yedekleme sırasında VM'ye yazma Hızlandırıcısı etkinleştirilmiş disk otomatik olarak dışlar.
- Yalnızca küçük g/ç boyutları (< = 512 KiB) hızlandırılmış yolu edilmektedir. İş yükü verileri toplu sürekli olduğu durumlarda yüklenen ya da işlem günlüğü arabelleklerini farklı DBMS daha büyük bir dereceye kadar depolama alanına kalıcı önce doldurulur burada olasılığı olan yazılan g/ç diski hızlandırılmış yolu değil sürüyor.

Azure Premium depolama VHD'leri yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlar şu şekildedir:

| VM SKU | Yazma Hızlandırıcı disk sayısı | Yazma Hızlandırıcı Disk VM başına IOPS |
| --- | --- | --- |
| M128ms, 128s | 16 | 20000 |
| M64ms, M64ls, M64s | 8 | 10000 |
| M32ms, M32ls, M32ts, M32s | 4 | 5000 |
| M16ms, M16s | 2 | 2500 |
| M8ms, M8s | 1 | 1250 |

VM başına IOPS sınırları olduğundan ve *değil* disk başına. Tüm yazma Hızlandırıcı diskler, VM başına aynı IOPS sınırı paylaşır.

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Belirli bir diskte etkinleştirilmesi yazma Hızlandırıcısı

Yazma Hızlandırıcı, Azure Premium depolama VHD'lerde nasıl etkinleştirilebilir birkaç sonraki bölümlerde açıklanmıştır.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar Bu anda yazma Hızlandırıcı kullanımı için geçerlidir:

- Azure yazma Hızlandırıcı karşı uygulamak istediğiniz disklerin olmasına gerek [Azure yönetilen diskler](https://azure.microsoft.com/services/managed-disks/) Premium depolama.
- M serisi VM kullanıyor gerekir

## <a name="enabling-azure-write-accelerator-using-azure-powershell"></a>Azure PowerShell kullanarak Azure yazma Hızlandırıcı etkinleştirme

Azure PowerShell modülü 5.5.0 sürümünden etkinleştirme veya devre dışı yazma Hızlandırıcı belirli Azure Premium depolama diskleri için ilgili cmdlet'ler değişiklikleri içerir.
Etkinleştirmek veya yazma Hızlandırıcı tarafından desteklenen diskleri dağıtmak için aşağıdaki PowerShell komutları değiştirildi ve yazma Hızlandırıcı için bir parametre kabul etmek için genişletilmiş.

Yeni bir anahtar parametresi **- WriteAccelerator** için aşağıdaki cmdlet'leri eklendi:

- [Set-AzVMOsDisk](https://docs.microsoft.com/powershell/module/az.compute/set-azvmosdisk?view=azurermps-6.0.0)
- [Add-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVMDataDisk?view=azurermps-6.0.0)
- [Set-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/Set-AzVMDataDisk?view=azurermps-6.0.0)
- [Add-AzVmssDataDisk](https://docs.microsoft.com/powershell/module/az.compute/Add-AzVmssDataDisk?view=azurermps-6.0.0)

Parametre vererek değil özelliği false olarak ayarlar ve yazma Hızlandırıcı tarafından desteği yok diskleri dağıtır.

Yeni bir anahtar parametresi **- OsDiskWriteAccelerator** için aşağıdaki cmdlet'leri eklendi:

- [Set-AzVmssStorageProfile](https://docs.microsoft.com/powershell/module/az.compute/Set-AzVmssStorageProfile?view=azurermps-6.0.0)

Parametresini belirtmeden özelliği varsayılan olarak, yazma Hızlandırıcı yararlanan olmayan diskleri döndüren false olarak ayarlar.

Yeni bir isteğe bağlı Boolean (atanamayan) parametre, **- OsDiskWriteAccelerator** için aşağıdaki cmdlet'leri eklendi:

- [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/Update-AzVM?view=azurermps-6.0.0)
- [Güncelleştirme AzVmss](https://docs.microsoft.com/powershell/module/az.compute/Update-AzVmss?view=azurermps-6.0.0)

$True veya $false diskleri Azure yazma Hızlandırıcı desteğiyle denetlemek için belirtin.

Komutları örnekleri gibi görünebilir:

```powershell
New-AzVMConfig | Set-AzVMOsDisk | Add-AzVMDataDisk -Name "datadisk1" | Add-AzVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzVM

Get-AzVM | Update-AzVM -OsDiskWriteAccelerator $true

New-AzVmssConfig | Set-AzVmssStorageProfile -OsDiskWriteAccelerator | Add-AzVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzVmss

Get-AzVmss | Update-AzVmss -OsDiskWriteAccelerator:$false
```

İki ana senaryo, aşağıdaki bölümlerde gösterildiği şekilde yazılabilir.

### <a name="adding-a-new-disk-supported-by-write-accelerator-using-powershell"></a>PowerShell kullanarak yazma Hızlandırıcısı'tarafından desteklenen yeni bir disk ekleme

Bu betik, VM'nize yeni bir disk eklemek için kullanabilirsiniz. Bu betik ile oluşturduğunuz disk yazma Hızlandırıcı kullanır.

Değiştirin `myVM`, `myWAVMs`, `log001`, disk ve diskin belirli bir dağıtım için uygun değerlerle Lunıd boyut.

```powershell
# Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "log001"
#LUN Id
$lunid=8
#size
$size=1023
#Pulls the VM info for later
$vm=Get-AzVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzVM -ResourceGroupName $rgname -VM $vm
```

### <a name="enabling-write-accelerator-on-an-existing-azure-disk-using-powershell"></a>PowerShell kullanarak mevcut bir Azure diskinde etkinleştirilmesi yazma Hızlandırıcısı

Bu betik, mevcut bir diske yazma Hızlandırıcı etkinleştirmek için kullanabilirsiniz. Değiştirin `myVM`, `myWAVMs`, ve `test-log001` belirli bir dağıtım için uygun değerlerle. Betik yazma Hızlandırıcı için var olan bir diski ekler. burada değeri **$newstatus** '$true' olarak ayarlayın. ' % S'değeri '$false' kullanarak yazma Hızlandırıcı üzerinde belirli bir disk devre dışı bırakır.

```powershell
#Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzVM -ResourceGroupName $rgname -VM $vm
```

> [!Note]
> Yukarıdaki komut dosyası yürütme belirtilen disk ayırma, disk yazma hızlandırıcıyı etkinleştirme ve ardından diski yeniden eklemek

## <a name="enabling-write-accelerator-using-the-azure-portal"></a>Yazma Hızlandırıcı Azure portalını kullanarak etkinleştirme

Yazma Hızlandırıcı, önbelleğe alma ayarları, disk belirttiğiniz portalı üzerinden etkinleştirebilirsiniz:

![Azure portalında Hızlandırıcı yazma](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)

## <a name="enabling-write-accelerator-using-the-azure-cli"></a>Azure CLI kullanarak etkinleştirilmesi yazma Hızlandırıcısı

Kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) yazma hızlandırıcıyı etkinleştirme.

Yazma Hızlandırıcı üzerinde var olan bir diski etkinleştirmek için [az vm update](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-update), diskName VMName ve ResourceGroup kendi değerlerinizle değiştirin, aşağıdaki örneklerde kullanabilirsiniz: `az vm update -g group1 -n vm1 -write-accelerator 1=true`

Yazma Hızlandırıcı ile diski kullanım etkin [az vm disk ekleme](https://docs.microsoft.com/cli/azure/vm/disk?view=azure-cli-latest#az-vm-disk-attach), siz kendi değerlerinizi yerleştirin, aşağıdaki örnekte kullanabilirsiniz: `az vm disk attach -g group1 -vm-name vm1 -disk d1 --enable-write-accelerator`

Yazma Hızlandırıcı devre dışı bırakmak için [az vm update](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-update), özellikleri false olarak ayarlama: `az vm update -g group1 -n vm1 -write-accelerator 0=false 1=false`

## <a name="enabling-write-accelerator-using-rest-apis"></a>Yazma Hızlandırıcı Rest API'lerini kullanarak etkinleştirme

Azure Rest API aracılığıyla dağıtmak için Azure armclient yüklemeniz gerekir.

### <a name="install-armclient"></a>Armclient yükleyin

Armclient çalıştırmak için Chocolatey yüklemeniz gerekir. Cmd.exe veya powershell yükleyebilirsiniz. Bu komutlar ("Yönetici olarak çalıştır") için yükseltilmiş haklara kullanın.

Cmd.exe kullanarak, aşağıdaki komutu çalıştırın: `@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"`

PowerShell kullanarak, aşağıdaki komutu çalıştırın: `Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

Aşağıdaki komutta cmd.exe'i veya PowerShell kullanarak armclient şimdi yükleyebilirsiniz `choco install armclient`

### <a name="getting-your-current-vm-configuration"></a>Geçerli VM yapılandırmanızı alma

Disk yapılandırmanızı özniteliklerini değiştirmek için önce bir JSON dosyasında geçerli yapılandırmasını almanız gerekir. Aşağıdaki komutu yürüterek geçerli yapılandırmasını alabilirsiniz: `armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>`

<> <>İçindeki terimler JSON dosyada olması gereken dosya adı dahil olmak üzere verilerinizi ile değiştirin.

Çıkış, gibi görünebilir:

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Ardından, JSON dosyasını güncelleştirin ve 'log1' adlı disk üzerinde yazma hızlandırıcıyı etkinleştirme. Bu, bu öznitelik JSON dosyasına ekleyerek disk önbellek girişten sonra gerçekleştirilebilir.

```JSON
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "writeAcceleratorEnabled": true,
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Ardından bu komutla var olan dağıtım güncelleştirin: `armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>`

Çıktı bir aşağıdaki gibi görünmelidir. Bir disk için yazma Hızlandırıcısı etkinleştirilmiş görebilirsiniz.

```JSON
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "writeAcceleratorEnabled": true,
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"
```

Bu değişikliği yaptıktan sonra sürücü yazma Hızlandırıcı tarafından desteklenmelidir.
