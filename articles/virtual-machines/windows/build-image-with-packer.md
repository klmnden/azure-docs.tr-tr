---
title: Packer ile Windows Azure VM görüntüleri oluşturma | Microsoft Docs
description: Packer ile Azure Windows sanal makine görüntüleri oluşturmak için kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/29/2018
ms.author: cynthn
ms.openlocfilehash: 5f19a6cb356332e95f96484953f1be3df006dd09
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931931"
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Azure'da Windows sanal makine görüntüleri oluşturmak için Packer kullanma
Azure'daki her sanal makine (VM) Windows Dağıtım ve işletim sistemi sürümünü tanımlayan bir görüntüden oluşturulur. Görüntüleri, önceden yüklenmiş uygulamalar ve yapılandırmalar içerebilir. Azure marketi, en yaygın işletim sistemi için birinci ve üçüncü taraf çok sayıda görüntü sağlar ve uygulama ortamları veya uygulamanızın ihtiyaçlarına yönelik kendi özel görüntülerinizi oluşturabilir. Bu makalede, açık kaynaklı aracının nasıl kullanılacağı ayrıntılı [Packer](https://www.packer.io/) tanımlama ve azure'da özel görüntü oluşturma.


## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun
Kaynak VM oluştururken derleme işlemi sırasında geçici Azure kaynaklarını Packer oluşturur. Bir görüntü olarak kullanmak için kaynak VM yakalamak için bir kaynak grubu tanımlamanız gerekir. Packer yapı işleminin çıktısı, bu kaynak grubunda depolanır.

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Azure kimlik bilgileri oluşturma
Packer ile Azure hizmet sorumlusu kullanarak kimliğini doğrular. Bir Azure hizmet sorumlusu, uygulamaları, hizmetleri ve Packer gibi Otomasyon araçları ile kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlayın.

Bir hizmet sorumlusu oluşturma [yeni AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) ve hizmet sorumlusuna sahip kaynakları oluşturup yönetmek için izinleri atamak [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "AzurePacker" `
    -Password (ConvertTo-SecureString "P@ssw0rd!" -AsPlainText -Force)
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Azure için kimlik doğrulaması için Azure kiracısı ve abonelik kimlikleri ile elde etmeniz [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId[0]
$sub.SubscriptionId[0]
```

Sonraki adımda bu iki kimliklerinin kullanırsınız.


## <a name="define-packer-template"></a>Packer şablonu tanımlama
Görüntüleri oluşturmak için bir şablon bir JSON dosyası oluşturun. Şablonda, Oluşturucular ve gerçek derleme işlemini gerçekleştirmek provisioners siz tanımlarsınız. Packer sahip bir [Azure için oluşturucu](https://www.packer.io/docs/builders/azure.html) sağlayan Azure kaynakları tanımlamak adım önceki oluşturulan hizmet sorumlusu kimlik bilgileri gibi.

Adlı bir dosya oluşturun *windows.json* ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | İle hizmet sorumlusu kimliği görüntüle `$sp.applicationId` |
| *client_secret*                     | Belirttiğiniz parola `$securePassword` |
| *Kiracı*                         | Çıktı `$sub.TenantId` komutu |
| *subscription_id*                   | Çıktı `$sub.SubscriptionId` komutu |
| *object_id*                         | Görünüm hizmet sorumlusu nesne kimliği ile `$sp.Id` |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsü için ad |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": true,
    "winrm_insecure": true,
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "& $env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /quiet /quit",
      "while($true) { $imageState = Get-ItemProperty HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Setup\\State | Select ImageState; if($imageState.ImageState -ne 'IMAGE_STATE_GENERALIZE_RESEAL_TO_OOBE') { Write-Output $imageState.ImageState; Start-Sleep -s 10  } else { break } }"
    ]
  }]
}
```

Bu şablon bir Windows Server 2016 VM oluşturur, IIS yükler ve sonra Sysprep ile VM'yi genelleştirir. IIS yüklemesini ek komutlarını çalıştırmak için PowerShell sağlayıcısı nasıl kullanabileceğinizi gösterir. Son Packer görüntü daha sonra gerekli yazılım yükleme ve yapılandırma içerir.


## <a name="build-packer-image"></a>Packer görüntü oluşturma
Yerel makinenizde yüklü Packer yoksa [Packer yükleme yönergelerini izleyin](https://www.packer.io/docs/install/index.html).

Görüntü, Packer belirterek şablon dosyası şu şekilde oluşturun:

```bash
./packer build windows.json
```

Yukarıdaki komutlarda bir örnek çıktı aşağıdaki gibidir:

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

VM oluşturmak için provisioners çalıştırıp dağıtımını temizleme Packer birkaç dakika sürer.


## <a name="create-a-vm-from-the-packer-image"></a>Packer görüntüsünden VM oluşturma
Artık bir VM ile görüntüsünden oluşturabilirsiniz [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Zaten mevcut değilse, destekleyici ağ kaynakları oluşturulur. İstendiğinde, bir yönetici kullanıcı adı ve VM'deki oluşturulması için parola girin. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* gelen *myPackerImage*:

```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Image "myPackerImage"
```

Farklı kaynak grubuna ya da, Packer görüntü bölgesinden Vm'leri oluşturmak istiyorsanız, görüntü adı yerine görüntü kimliği belirtin. Görüntü kimliği ile edinebilirsiniz [Get-Azurermımage](/powershell/module/AzureRM.Compute/Get-AzureRmImage).

Bu, Packer görüntüsünden VM oluşturma birkaç dakika sürer.


## <a name="test-vm-and-webserver"></a>Test VM ve Web sunucusu
[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile VM’nizin genel IP adresini alın. Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Packer sağlayıcısı IIS yükle eylemini içeren, VM'nize görmek için genel IP adresini bir web tarayıcısına girin.

![Varsayılan IIS sitesi](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, Packer ile IIS zaten yüklü bir VM görüntüsü oluşturmak için kullanılır. Team Services, Ansible, Chef veya Puppet ile görüntüden oluşturulan sanal makineler için uygulamanızı dağıtmak için olduğu gibi bu VM görüntüsü mevcut dağıtım iş akışları ile birlikte kullanabilirsiniz.

Diğer Windows distro'lara için ek örnek Packer şablonları için bkz: [bu GitHub deposunu](https://github.com/hashicorp/packer/tree/master/examples/azure).
