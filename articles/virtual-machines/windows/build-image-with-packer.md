---
title: Azure'da Packer ile Windows VM görüntüleri oluşturma | Microsoft Docs
description: Packer ile Azure Windows sanal makine görüntüleri oluşturmak için kullanmayı öğrenin
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/22/2019
ms.author: cynthn
ms.openlocfilehash: 905f330af7052b7d39058b5d84fb51a70311248d
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719330"
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a>Azure'da Windows sanal makine görüntüleri oluşturmak için Packer kullanma
Azure'daki her sanal makine (VM) Windows Dağıtım ve işletim sistemi sürümünü tanımlayan bir görüntüden oluşturulur. Görüntüleri, önceden yüklenmiş uygulamalar ve yapılandırmalar içerebilir. Azure marketi, en yaygın işletim sistemi için birinci ve üçüncü taraf çok sayıda görüntü sağlar ve uygulama ortamları veya uygulamanızın ihtiyaçlarına yönelik kendi özel görüntülerinizi oluşturabilir. Bu makalede, açık kaynaklı aracının nasıl kullanılacağı ayrıntılı [Packer](https://www.packer.io/) tanımlama ve azure'da özel görüntü oluşturma.

Bu makalede son 21/2/2019 kullanarak test [Az PowerShell Modülü](https://docs.microsoft.com/powershell/azure/install-az-ps) sürüm 1.3.0 ve [Packer](https://www.packer.io/docs/install/index.html) 1.3.4 sürümü.

> [!NOTE]
> Azure artık Azure Görüntü Oluşturucu (Önizleme) bir hizmet tanımlama ve kendi özel görüntülerinizi oluşturmak için var. Hatta mevcut Packer Kabuk sağlayıcısı betiklerinizi ile kullanabilmesi için azure Görüntü Oluşturucu Packer üzerinde oluşturulmuştur. Azure Görüntü Oluşturucu ile çalışmaya başlamak için bkz. [Azure Görüntü Oluşturucu ile Windows VM oluşturma](image-builder.md).

## <a name="create-azure-resource-group"></a>Azure kaynak grubu oluşturun
Kaynak VM oluştururken derleme işlemi sırasında geçici Azure kaynaklarını Packer oluşturur. Bir görüntü olarak kullanmak için kaynak VM yakalamak için bir kaynak grubu tanımlamanız gerekir. Packer yapı işleminin çıktısı, bu kaynak grubunda depolanır.

Bir kaynak grubu oluşturun [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a>Azure kimlik bilgilerini oluşturma
Packer ile Azure hizmet sorumlusu kullanarak kimliğini doğrular. Bir Azure hizmet sorumlusu, uygulamaları, hizmetleri ve Packer gibi Otomasyon araçları ile kullanabileceğiniz bir güvenlik kimliğidir. Denetim ve hizmet sorumlusu Azure'da gerçekleştirebilirsiniz ne gibi işlemler için izinler tanımlayın.

Bir hizmet sorumlusu oluşturma [yeni AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal) ve hizmet sorumlusuna sahip kaynakları oluşturup yönetmek için izinleri atamak [yeni AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment). Değeri `-DisplayName` benzersiz; olması gereken gerektiğinde kendi değeriyle değiştirin.  

```azurepowershell
$sp = New-AzADServicePrincipal -DisplayName "PackerServicePrincipal"
$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret)
$plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Parola ve uygulama kimliği'ı çıkışı

```powershell
$plainPassword
$sp.ApplicationId
```


Azure için kimlik doğrulaması için Azure kiracısı ve abonelik kimlikleri ile elde etmeniz [Get-AzSubscription](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription):

```powershell
Get-AzSubscription
```


## <a name="define-packer-template"></a>Packer şablonu tanımlama
Görüntüleri oluşturmak için bir şablon bir JSON dosyası oluşturun. Şablonda, Oluşturucular ve gerçek derleme işlemini gerçekleştirmek provisioners siz tanımlarsınız. Packer sahip bir [Azure için oluşturucu](https://www.packer.io/docs/builders/azure.html) sağlayan Azure kaynakları tanımlamak adım önceki oluşturulan hizmet sorumlusu kimlik bilgileri gibi.

Adlı bir dosya oluşturun *windows.json* ve aşağıdaki içeriği yapıştırın. Aşağıdakiler için kendi değerlerinizi girin:

| Parametre                           | Nereden |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | İle hizmet sorumlusu kimliği görüntüle `$sp.applicationId` |
| *client_secret*                     | Otomatik olarak oluşturulan parola ile görüntüleme `$plainPassword` |
| *Kiracı*                         | Çıktı `$sub.TenantId` komutu |
| *subscription_id*                   | Çıktı `$sub.SubscriptionId` komutu |
| *managed_image_resource_group_name* | İlk adımda oluşturduğunuz kaynak grubunun adı |
| *managed_image_name*                | Oluşturulan yönetilen disk görüntüsü için ad |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "client_secret": "ppppppp-pppp-pppp-pppp-ppppppppppp",
    "tenant_id": "zzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
    "subscription_id": "yyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyy",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": true,
    "winrm_insecure": true,
    "winrm_timeout": "5m",
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

Görüntüyü şablon dosyası gibi bir komut istemi açmak ve, Packer belirterek oluşturma:

```
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
Artık bir VM ile görüntüsünden oluşturabilirsiniz [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Zaten mevcut değilse, destekleyici ağ kaynakları oluşturulur. İstendiğinde, bir yönetici kullanıcı adı ve VM'deki oluşturulması için parola girin. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* gelen *myPackerImage*:

```powershell
New-AzVm `
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

Farklı kaynak grubuna ya da, Packer görüntü bölgesinden Vm'leri oluşturmak istiyorsanız, görüntü adı yerine görüntü kimliği belirtin. Görüntü kimliği ile edinebilirsiniz [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/Get-AzImage).

Bu, Packer görüntüsünden VM oluşturma birkaç dakika sürer.


## <a name="test-vm-and-webserver"></a>Test VM ve Web sunucusu
İle sanal makinenizin genel IP adresini [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress). Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```powershell
Get-AzPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIPAddress" | select "IpAddress"
```

Packer sağlayıcısı IIS yükle eylemini içeren, VM'nize görmek için genel IP adresini bir web tarayıcısına girin.

![Varsayılan IIS sitesi](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a>Sonraki adımlar
Mevcut Packer sağlayıcısı betiklerle kullanabilirsiniz [Azure Görüntü Oluşturucu](image-builder.md).
