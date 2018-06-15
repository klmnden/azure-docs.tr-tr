---
title: SAP dağıtımları için Azure yazma Hızlandırıcı | Microsoft Docs
description: Azure sanal makinelerinde dağıtılan SAP HANA sistemleri için işlemler Kılavuzu.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89a4216a3893892eedd6c216c7e0e5d51cf64749
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2018
ms.locfileid: "31809990"
---
# <a name="azure-write-accelerator-for-sap-deployments"></a>SAP dağıtımları için Azure yazma Hızlandırıcı
Azure yazma Hızlandırıcı M-serisi VM'ler için özel olarak alınır bir işlevdir. Yazma Azure Hızlandırıcı herhangi diğer VM-serisi M-serisi dışında Azure ile kullanılamaz. Adını belirten, işlevselliği amacı Azure Premium Storage'a karşı yazma g/ç gecikmesi artırmak için aynıdır. 

M-serisi dağıtım genel önizlemede olarak Azure yazma Hızlandırıcı işlevler kullanılabilir:

- Batı US2
- Doğu US2
- Batı Avrupa
- Güneydoğu Asya

## <a name="planning-for-using-azure-write-accelerator"></a>Azure yazma Hızlandırıcı kullanmayı planlama
Azure yazma Hızlandırıcı, işlem günlüğü içermiyor ya da bir DBMS günlüklerini Yinele birimlerini için kullanılmalıdır. DBMS veri birimleri için Azure yazma Hızlandırıcı kullanmak için önerilmez. Bu kısıtlamanın neden Azure yazma Hızlandırıcı Azure Premium Storage ek okuma önbelleğe alma olmadan takılması Premium Storage için kullanılabilir olan VHD gerektirmesidir. Bu tür önbelleğe alma ile büyük avantajları geleneksel veritabanları ile gösterilebilir. Hızlandırıcı yazma yazma etkinlikleri yalnızca etkileyen ve okuma hızlandırmak değil olduğundan, işlem günlüğü karşı yazma Hızlandırıcı kullanın veya günlük sürücüler desteklenen SAP veritabanlarının yinelemek için SAP için desteklenen tasarım kalır. 

Azure yazma Hızlandırıcı yalnızca çalışır birlikte [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/). Bu nedenle, uygun şekilde planlamanız gerekir. 

>[!NOTE]
> Azure yazma Hızlandırıcı işlevselliğini hala genel önizlemede olduğundan, hiçbir üretim senaryosu dağıtımı işlevselliğini henüz desteklenmemektedir.

Azure Premium Storage VHD'ler Azure yazma Hızlandırıcı tarafından desteklenen VM başına sınırları vardır. Geçerli sınırlarını şunlardır:


| VM SKU | Hızlandırıcı yazma disk sayısı | Hızlandırıcı VM başına IOPS yazma |
| --- | --- | --- |
| M128ms | 16 | 8000 |
| M128s | 16 | 8000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 



> [!IMPORTANT]
> Etkinleştirmek veya Azure yazma Hızlandırıcı birden çok Azure Premium Storage disk oluşturulur ve Windows disk veya birim Yöneticisi kullanarak şeritli var olan bir birim için devre dışı bırakmak istiyorsanız, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, tüm diskleri birim oluşturma etkin veya yazma Hızlandırıcı için ayrı adımlarda devre dışı. **Etkinleştirme veya bu tür bir yapılandırmada yazma Hızlandırıcı devre dışı bırakma önce Azure VM kapatma**. 


> [!IMPORTANT]
> Toplu derleme Windows disk veya birim yöneticileri, Windows depolama alanları, Windows genişleme dosya sunucusu (SOFS), Linux LVM veya MDADM, sahip birden çok disk dışında parçası olmayan mevcut bir Azure diski için Azure yazma Hızlandırıcı etkinleştirmek için iş yükü erişme Azure disk kapatılması gerekiyor. Azure diski kullanarak veritabanı uygulamaları kapatılmalıdır.

> [!IMPORTANT]
> VM'in Azure VM işletim sistemi diski için yazma Hızlandırıcı etkinleştirme VM yeniden başlatılır. 

Diskleri işletim için SAP gerekli olmamalıdır için Azure yazma Hızlandırıcı etkinleştirme VM yapılandırmaları ilgili

### <a name="restrictions-when-using-azure-write-accelerator"></a>Azure yazma Hızlandırıcı kullanırken kısıtlamaları
Azure yazma Hızlandırıcı Azure bir disk/VHD için kullanırken bu kısıtlamalar geçerlidir:

- Premium disk önbelleği 'None' ayarlanması gerekir ya da 'Salt okunur'. Diğer tüm önbelleğe alma modu desteklenmiyor.
- Yazma etkinleştirilmiş Hızlandırıcı disk üzerinde anlık görüntü henüz desteklenmiyor. Bu kısıtlama, Azure Backup hizmeti bir uygulamayla tutarlı anlık görüntü sanal makinenin tüm disklerinin gerçekleştirme yeteneğini engeller.
- Küçük g/ç boyutları hızlandırılmış yolu sürüyor. İş yükü verileri toplu mdan olduğu durumlarda yüklü olan veya depolama birimine kalıcı önce için büyük ölçüde doldurulmuş farklı DBMS işlem günlüğü arabelleklerini burada olasılığını yazılan g/ç disk hızlandırılmış yolu sürüyor değil.


## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Belirli bir disk üzerinde yazma Hızlandırıcı etkinleştirme
Sonraki birkaç bölümlerde Azure yazma Hızlandırıcı Azure Premium Storage Vhd'lere nasıl etkinleştirilebilir anlatmaktadır.

Bu anda, yazma Hızlandırıcı Azure Rest API ve PowerShell aracılığıyla etkinleştirme yalnızca yöntemleridir. Diğer yöntemleri Azure desteği kadar portal esnasında sonraki birkaç hafta izler.

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki Önkoşullar Bu anda Azure yazma Hızlandırıcı kullanımı için geçerlidir:

- VM için VM ve depolama dağıtmak için kullanılan abonelik Kimliğinizi beyaz listelenen olması gerekir. Hesap Yöneticisi'nin abonelik Kimliğinizi alın beyaz listelenen veya GBB, Microsoft CSA başvurun. 
- Azure yazma Hızlandırıcı karşı uygulamak istediğiniz diskleri olmasına gerek [Azure yönetilen diskleri](https://azure.microsoft.com/services/managed-disks/).

### <a name="enabling-through-power-shell"></a>Power Shell etkinleştirme
Azure Power Shell modülü 5.5.0 sürümünden etkinleştirmek veya Azure yazma Hızlandırıcı belirli Azure Premium Storage diskler için devre dışı bırakmak için ilgili cmdlet'leri değişiklikleri içerir.
Aşağıdaki PowerShell komutlarını etkinleştirmek ya da yazma Hızlandırıcı tarafından desteklenen diskleri dağıtmak için değiştirildiği ve Hızlandırıcı yazmak için bir parametre kabul etmek için genişletilmiş.

Yeni bir anahtar parametre "WriteAccelerator" aşağıdaki cmdlet'leri eklenen: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Parametre vermiş değil özelliği false olarak ayarlar ve Azure yazma Hızlandırıcı tarafından desteği yok diskleri dağıtacak.

Yeni bir anahtar parametre "OsDiskWriteAccelerator" aşağıdaki cmdlet'leri eklenmiştir: 

- Set-AzureRmVmssStorageProfile

Ayarlar özelliği false olarak vermiş ve Azure yazma Hızlandırıcı yararlanıyor mu diskleri teslim eder.

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

#### <a name="adding--new-disk-supported-by-azure-write-accelerator"></a>Azure yazma Hızlandırıcı tarafından desteklenen yeni disk ekleme
VM için yeni bir disk eklemek için bu komut dosyasını kullanabilirsiniz. Bu komut dosyası ile oluşturduğunuz disk Azure yazma Hızlandırıcı kullanmak zordur.

```

# Specify your VM Name
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
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
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
#data disk name
$datadiskname = "testsap-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

VM, disk ve kaynak grubu adları uyarlamanız gerekir. Yukarıdaki betik yazmak için varolan bir diski '$true' $newstatus değeri ayarlanamıyor hızlandırıcısıdır ekler. '$False' değerini kullanarak yazma Hızlandırıcı belirli bir disk üzerinde devre dışı bırakır.

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

 