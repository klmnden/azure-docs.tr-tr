---
title: include dosyası
description: include dosyası
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 6/8/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 21681a1af64754ef569f2ad4ff92f85a598007ac
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35323791"
---
# <a name="write-accelerator"></a>Hızlandırıcı yazma
Hızlandırıcı bir disk özelliği için M-serisi sanal makinelerde (VM'ler) yönetilen Azure disklerle Premium depolama özel olarak yazma. Adını belirten, işlevselliği amacı Azure Premium Storage'a karşı yazma g/ç gecikmesi artırmak için aynıdır. Hızlandırıcı ideal burada günlük dosyası güncelleştirmelerini diske modern veritabanları için yüksek oranda kullanıcı şekilde kalıcı hale getirmek için gereken uygun yazma.

Hızlandırıcı genel bulut M-serisi VM'ler için genel kullanıma açıktır yazma.

## <a name="planning-for-using-write-accelerator"></a>Hızlandırıcı yazma kullanmayı planlama
Hızlandırıcı işlem günlüğü içeren veya DBMS günlüklerini Yinele birimler için kullanılması gereken yazma. Özellik günlük disklerde kullanılması için optimize edilmiştir olarak yazma Hızlandırıcı DBMS veri birimleri için kullanılmak üzere önerilmez.

Hızlandırıcı yalnızca çalışır birlikte yazma [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/). 


> [!IMPORTANT]
> Etkinleştirmek veya birden çok Azure Premium Storage disk oluşturulur ve Windows disk veya birim Yöneticisi kullanarak şeritli var olan bir birim için yazma Hızlandırıcı devre dışı bırakmak istiyorsanız, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, tüm Toplu derleme diskleri etkin veya yazma Hızlandırıcı için ayrı adımlarda devre dışı. **Etkinleştirme veya bu tür bir yapılandırmada yazma Hızlandırıcı devre dışı bırakma önce Azure VM kapatma**. 


> [!IMPORTANT]
> Toplu derleme Windows diske veya birime yöneticileri, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, Azure diski erişme iş yükü sahip birden çok disk dışında bir parçası olmayan mevcut bir Azure diske yazma Hızlandırıcı etkinleştirmek için kapatılması gerekiyor. Azure diski kullanarak veritabanı uygulamaları kapatılmalıdır.

> [!IMPORTANT]
> VM işletim sistemi diski için yazma Hızlandırıcı etkinleştirme VM yeniden başlatılır. 

İşletim sistemi disklerin yazma Hızlandırıcı etkinleştirme SAP ilgili VM yapılandırmaları için gerekli olmamalıdır

### <a name="restrictions-when-using-write-accelerator"></a>Hızlandırıcı yazma kullanırken kısıtlamaları
Hızlandırıcı yazma Azure bir disk/VHD için kullanırken bu kısıtlamalar geçerlidir:

- Premium disk önbelleği 'None' olarak ayarlanmalıdır ya da 'Salt okunur'. Diğer tüm önbelleğe alma modu desteklenmiyor.
- Yazma etkinleştirilmiş Hızlandırıcı disk üzerinde anlık görüntü henüz desteklenmiyor. Bu kısıtlama, Azure Backup hizmeti bir uygulamayla tutarlı anlık görüntü sanal makinenin tüm disklerinin gerçekleştirme yeteneğini engeller.
- Yalnızca küçük g/ç boyutları (< = 32 KiB) hızlandırılmış yolu sürüyor. İş yükü verileri toplu mdan olduğu durumlarda yüklü olan veya depolama birimine kalıcı önce için büyük ölçüde doldurulmuş farklı DBMS işlem günlüğü arabelleklerini burada olasılığını yazılan g/ç disk hızlandırılmış yolu sürüyor değil.

Azure Premium Storage VHD'ler yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlarını şunlardır:

| VM SKU | Hızlandırıcı yazma disk sayısı | Hızlandırıcı Disk VM başına IOPS yazma |
| --- | --- | --- |
| M128ms, 128s | 16 | 8000 |
| M64ms, M64ls, M64s | 8 | 4000 |
| M32ms, M32ls, M32ts, M32s | 4 | 2000 | 
| M16ms, M16s | 2 | 1000 | 
| M8ms, M8s | 1 | 500 | 

VM başına IOPS sınırları olduğundan ve *değil* disk başına. Tüm yazma Hızlandırıcı diskleri VM başına aynı IOPS sınırı paylaşır.
## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Belirli bir disk üzerinde yazma Hızlandırıcı etkinleştirme
Sonraki birkaç bölümler, yazma Hızlandırıcı Azure Premium Storage Vhd'lere nasıl etkinleştirilebilir anlatmaktadır.


### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki Önkoşullar Bu anda yazma Hızlandırıcı kullanımı için geçerlidir:

- Azure yazma Hızlandırıcı karşı uygulamak istediğiniz diskleri olmasına gerek [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/) Premium depolama.
- M-serisi VM kullanıyor olmanız gerekir

## <a name="enabling-azure-write-accelerator-using-azure-powershell"></a>Azure PowerShell kullanarak Azure yazma Hızlandırıcı etkinleştirme
Azure Power Shell modülü 5.5.0 sürümünden etkinleştirme veya devre dışı yazma Hızlandırıcı belirli Azure Premium Storage diskler için ilgili cmdlet'leri değişiklikleri içerir.
Aşağıdaki PowerShell komutlarını etkinleştirmek ya da yazma Hızlandırıcı tarafından desteklenen diskleri dağıtmak için değiştirildiği ve Hızlandırıcı yazmak için bir parametre kabul etmek için genişletilmiş.

Yeni bir anahtar parametre "WriteAccelerator" aşağıdaki cmdlet'leri eklenen: 

- [Set-AzureRmVMOsDisk](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmosdisk?view=azurermps-6.0.0)
- [Add-AzureRmVMDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Add-AzureRmVMDataDisk?view=azurermps-6.0.0)
- [Set-AzureRmVMDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Set-AzureRmVMDataDisk?view=azurermps-6.0.0)
- [Add-AzureRmVmssDataDisk](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Add-AzureRmVmssDataDisk?view=azurermps-6.0.0)

Parametre vermiş değil özelliği false olarak ayarlar ve yazma Hızlandırıcı tarafından desteği yok diskleri dağıtacak.

Yeni bir anahtar parametre "OsDiskWriteAccelerator" aşağıdaki cmdlet'leri eklenmiştir: 

- [Set-AzureRmVmssStorageProfile](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile?view=azurermps-6.0.0)

Parametre vermiş değil özelliği false olarak ayarlar ve yazma Hızlandırıcı yararlanıyor mu diskleri teslim eder.

Yeni bir isteğe bağlı Boolean (null) parametresi, "OsDiskWriteAccelerator" aşağıdaki cmdlet'leri eklenen: 

- [Update-AzureRmVM](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Update-AzureRmVM?view=azurermps-6.0.0)
- [Update-AzureRmVmss](https://docs.microsoft.com/en-us/powershell/module/AzureRM.Compute/Update-AzureRmVmss?view=azurermps-6.0.0)

$True veya $false Azure yazma Hızlandırıcı destek disklerle denetlemek için belirtin.

Komutları örnekleri gibi görünebilir:

```

New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false 

```

Aşağıdaki bölümlerde gösterildiği gibi iki ana senaryo betiği yazılabilir.

#### <a name="adding-a-new-disk-supported-by-write-accelerator"></a>Yazma Hızlandırıcı tarafından desteklenen yeni bir disk ekleme
VM için yeni bir disk eklemek için bu komut dosyasını kullanabilirsiniz. Bu komut dosyası ile oluşturduğunuz disk yazma Hızlandırıcı kullanmak zordur.

```

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
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzureRmVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```
VM, disk, kaynak grubu, diskin boyutunu ve belirli bir dağıtım için disk Lunıd adlarını uyarlamanız gerekir.


#### <a name="enabling-azure-write-accelerator-on-an-existing-azure-disk"></a>Var olan bir Azure diskinde Azure yazma Hızlandırıcı etkinleştirme
Varolan bir diske yazma Hızlandırıcı etkinleştirmek gerekiyorsa, görevi gerçekleştirmek için bu komut dosyasını kullanabilirsiniz:

```

#Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

VM, disk ve kaynak grubu adları uyarlamanız gerekir. Yukarıdaki komut dosyası yazma Hızlandırıcı $newstatus değeri '$true' olarak ayarlandığı varolan bir diski ekler. '$False' değerini kullanarak yazma Hızlandırıcı belirli bir disk üzerinde devre dışı bırakır.

> [!Note]
> Yukarıdaki komut dosyası yürütme belirtilen disk ayırma, yazma Hızlandırıcı karşı disk etkinleştirir ve diski yeniden ekleyin

### <a name="enabling-azure-write-accelerator-using-the-azure-portal"></a>Azure Portalı'nı kullanarak Azure yazma Hızlandırıcı etkinleştirme

Önbelleğe alma ayarları diskinizin belirlediğiniz portal yazma Hızlandırıcı etkinleştirebilirsiniz: 

![Azure Portal'da Hızlandırıcı yazma](./media/virtual-machines-common-how-to-enable-write-accelerator/wa_scrnsht.png)

## <a name="enabling-through-azure-cli"></a>Azure CLI aracılığıyla etkinleştirme
Kullanabileceğiniz [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) yazma Hızlandırıcı etkinleştirmek için. 

Varolan bir diske yazma Hızlandırıcı etkinleştirmek için kullanın [az vm güncelleştirmesi](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az-vm-update), diskName, VMName ve kaynak grubu kendi için yerine, varsa, aşağıdaki örneklerde kullanabilirsiniz:
 
```
az vm update -g group1 -n vm1 -write-accelerator 1=true
```
Disk yazma Hızlandırıcı ile eklemek için kullanım etkin [az vm disk ekleme](https://docs.microsoft.com/en-us/cli/azure/vm/disk?view=azure-cli-latest#az-vm-disk-attach), içinde kendi değerlerinizi yerleştirin, aşağıdaki örnekte kullanabilirsiniz:
```
az vm disk attach -g group1 -vm-name vm1 -disk d1 --enable-write-accelerator
```
Hızlandırıcı yazma devre dışı bırakmak için [az vm güncelleştirmesi](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az-vm-update), özellikleri false olarak ayarlanırsa: 
```
az vm update -g group1 -n vm1 -write-accelerator 0=false 1=false
```

### <a name="enabling-through-rest-apis"></a>REST API'leri aracılığıyla etkinleştirme
Azure Rest API aracılığıyla dağıtmak için Azure armclient yüklemeniz gerekir

#### <a name="install-armclient"></a>Armclient yükleyin

Armclient çalıştırmak için Chocolatey yüklemeniz gerekir. Cmd.exe veya powershell aracılığıyla yükleyebilirsiniz. Bu komutlar ("Yönetici olarak çalıştır") için yükseltilmiş haklara kullanın.

Aşağıdaki komutu çalıştırın cmd.exe kullanma:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

Güç Kabuğu'nu kullanarak kullanmak vardır:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Aşağıdaki komutunu cmd.exe veya Power Shell ile armclient şimdi yükleyebilirsiniz

```
choco install armclient
```

#### <a name="getting-your-current-vm-configuration"></a>Geçerli VM yapılandırmanızı alma
Disk yapılandırmanızı özniteliklerini değiştirmek için önce bir JSON dosyasının geçerli yapılandırma almanız gerekir. Aşağıdaki komutu çalıştırarak geçerli yapılandırmasını alabilirsiniz:

```
armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>
```
'<> <>' İçindeki terimler JSON dosyası olmalıdır dosya adı dahil olmak üzere, verilerle değiştirin.

Çıktı gibi görünebilir:

```
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

Sonraki adım JSON dosyasını güncelleştirin ve 'log1' adlı disk üzerinde yazma Hızlandırıcı etkinleştirmek üzere olacaktır. Bu adım, bu öznitelik JSON dosyasına ekleyerek disk önbellek girişi sonra gerçekleştirilebilir. 

```
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Ardından bu komutla mevcut dağıtımını güncelleştirin:

```
armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>

```

Çıkışı bir aşağıdaki gibi görünmelidir. Yazma etkinleştirilmiş bir disk için Hızlandırıcı olduğunu görebilirsiniz.

```
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
          **"writeAcceleratorEnabled": true,**
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

Değişikliğin noktasından sürücü yazma Hızlandırıcı tarafından desteklenmelidir.

 
