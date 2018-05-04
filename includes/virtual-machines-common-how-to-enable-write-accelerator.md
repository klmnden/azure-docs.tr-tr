---
title: include dosyası
description: include dosyası
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 04/30/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 54faa5a50b3fe965bc7f95fc0da0fdda9388412f
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="write-accelerator"></a>Hızlandırıcı yazma
Hızlandırıcı bir disk özelliği için M-serisi sanal makinelerde (VM'ler) yönetilen Azure disklerle Premium depolama özel olarak yazma. Adını belirten, işlevselliği amacı Azure Premium Storage'a karşı yazma g/ç gecikmesi artırmak için aynıdır. Hızlandırıcı ideal burada günlük dosyası güncelleştirmelerini diske modern veritabanları için yüksek oranda kullanıcı şekilde kalıcı hale getirmek için gereken uygun yazma.

Hızlandırıcı genel bulut M-serisi VM'ler için genel kullanıma açıktır yazma.

## <a name="planning-for-using-write-accelerator"></a>Hızlandırıcı yazma kullanmayı planlama
Hızlandırıcı, işlem günlüğü içermiyor ya da bir DBMS günlüklerini Yinele birimlerini için kullanılması gereken yazma. Özellik günlük disklerde kullanılması için optimize edilmiştir olarak yazma Hızlandırıcı DBMS veri birimleri için kullanılmak üzere önerilmez.

Hızlandırıcı yalnızca çalışır birlikte yazma [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/). 


> [!IMPORTANT]
> Etkinleştirmek veya birden çok Azure Premium Storage disk oluşturulur ve Windows disk veya birim Yöneticisi kullanarak şeritli var olan bir birim için yazma Hızlandırıcı devre dışı bırakmak istiyorsanız, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, tüm Toplu derleme diskleri etkin veya yazma Hızlandırıcı için ayrı adımlarda devre dışı. **Etkinleştirme veya bu tür bir yapılandırmada yazma Hızlandırıcı devre dışı bırakma önce Azure VM kapatma**. 


> [!IMPORTANT]
> Toplu derleme Windows diske veya birime yöneticileri, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, Azure diski erişme iş yükü sahip birden çok disk dışında bir parçası olmayan mevcut bir Azure diske yazma Hızlandırıcı etkinleştirmek için kapatılması gerekiyor. Azure diski kullanarak veritabanı uygulamaları kapatılmalıdır.

> [!IMPORTANT]
> VM işletim sistemi diski için yazma Hızlandırıcı etkinleştirme VM yeniden başlatılır. 

İşletim sistemi diskleri için SAP gerekli olmamalıdır için yazma Hızlandırıcı etkinleştirme VM yapılandırmaları ilgili

### <a name="restrictions-when-using-write-accelerator"></a>Hızlandırıcı yazma kullanırken kısıtlamaları
Hızlandırıcı yazma Azure bir disk/VHD için kullanırken bu kısıtlamalar geçerlidir:

- Premium disk önbelleği 'None' ayarlanması gerekir ya da 'Salt okunur'. Diğer tüm önbelleğe alma modu desteklenmiyor.
- Yazma etkinleştirilmiş Hızlandırıcı disk üzerinde anlık görüntü henüz desteklenmiyor. Bu kısıtlama, Azure Backup hizmeti bir uygulamayla tutarlı anlık görüntü sanal makinenin tüm disklerinin gerçekleştirme yeteneğini engeller.
- Küçük g/ç boyutları hızlandırılmış yolu sürüyor. İş yükü verileri toplu mdan olduğu durumlarda yüklü olan veya depolama birimine kalıcı önce için büyük ölçüde doldurulmuş farklı DBMS işlem günlüğü arabelleklerini burada olasılığını yazılan g/ç disk hızlandırılmış yolu sürüyor değil.

Azure Premium Storage VHD'ler yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlarını şunlardır:

| VM SKU | Hızlandırıcı yazma disk sayısı | Hızlandırıcı Disk VM başına IOPS yazma |
| --- | --- | --- |
| M128ms | 16 | 8000 |
| M128s | 16 | 8000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Belirli bir disk üzerinde yazma Hızlandırıcı etkinleştirme
Sonraki birkaç bölümler, yazma Hızlandırıcı Azure Premium Storage Vhd'lere nasıl etkinleştirilebilir anlatmaktadır.


### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki Önkoşullar Bu anda yazma Hızlandırıcı kullanımı için geçerlidir:

- Azure yazma Hızlandırıcı karşı uygulamak istediğiniz diskleri olmasına gerek [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/) Premium depolama.
- M-serisi VM kullanıyor olmanız gerekir

### <a name="enabling-through-power-shell"></a>Power Shell etkinleştirme
Azure Power Shell modülü 5.5.0 sürümünden etkinleştirme veya devre dışı yazma Hızlandırıcı belirli Azure Premium Storage diskler için ilgili cmdlet'leri değişiklikleri içerir.
Aşağıdaki PowerShell komutlarını etkinleştirmek ya da yazma Hızlandırıcı tarafından desteklenen diskleri dağıtmak için değiştirildiği ve Hızlandırıcı yazmak için bir parametre kabul etmek için genişletilmiş.

Yeni bir anahtar parametre "WriteAccelerator" aşağıdaki cmdlet'leri eklenen: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Parametre vermiş değil özelliği false olarak ayarlar ve yazma Hızlandırıcı tarafından desteği yok diskleri dağıtacak.

Yeni bir anahtar parametre "OsDiskWriteAccelerator" aşağıdaki cmdlet'leri eklenmiştir: 

- Set-AzureRmVmssStorageProfile

Paramenter vermiş değil özelliği false olarak ayarlar ve yazma Hızlandırıcı yararlanıyor mu diskleri teslim eder.

Yeni bir isteğe bağlı Boolean (null) parametresi, "OsDiskWriteAccelerator" aşağıdaki cmdlet'leri eklenen: 

- Update-AzureRmVM
- Update-AzureRmVmss

$True veya $false Azure yazma Hızlandırıcı destek disklerle denetlemek için belirtin.

Komutları örnekleri gibi görünebilir:

```

New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false 

```

Aşağıdaki bölümlerde gösterildiği gibi iki ana senaryo betiği yazılabilir.

#### <a name="adding--new-disk-supported-by-write-accelerator"></a>Yazma Hızlandırıcı tarafından desteklenen yeni disk ekleme
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

 
