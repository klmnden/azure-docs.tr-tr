---
title: Bir uygulamayı bir Azure sanal makine ölçek kümesini dağıtma | Microsoft Docs
description: Ölçek kümesindeki sanal makine örnekleri Linux ve Windows uygulamaları dağıtmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: bbbe677b0a0d47147ace41ff5a229282f80bbf1b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839524"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri, uygulamanızda dağıtma
Bir ölçek kümesindeki sanal makine (VM) örneklerinde uygulamaları çalıştırmak için önce uygulama bileşenlerini ve gerekli dosyaları yüklemeniz gerekir. Bu makalede bir ölçek durumlarda ayarlayın ya da var olan VM örneklerini otomatik olarak yükleme betikleri çalıştırmak için özel bir VM görüntüsü oluşturmak için yollar sağlar. Ayrıca uygulama veya işletim sistemi güncelleştirmelerini ölçek kümesi boyunca yönetmeyi öğrenin.


## <a name="build-a-custom-vm-image"></a>Özel bir VM görüntüsü oluşturma
Azure platformu görüntülerden birini ölçek kümesinde örnekleri oluşturmak için kullandığınızda, başka bir yazılım yüklenmedi veya yapılandırılmadı. Bu bileşenlerin yüklenmesi ancak ölçek kümesi VM örnekleri sağlamak için gereken süreyi ekleyen otomatikleştirebilirsiniz. VM örnekleri için birçok yapılandırma değişiklikleri uygularsanız, yönetim ek yükü ile bu yapılandırma komut dosyaları ve görevleri yoktur.

Yapılandırma yönetimi ve bir VM sağlayacak süresini azaltmak için bir örnek sağlanan hemen sonra uygulamanızın ölçek kümesinde çalıştırılmaya hazır özel bir VM görüntüsü oluşturabilirsiniz. Oluşturma ve özel bir VM görüntüsü ölçek ile kullanma hakkında daha fazla bilgi için aşağıdaki öğreticiler bakın:

- [Azure CLI 2.0](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="already-provisioned"></a>Uygulama özel betik uzantısı ile yükleme
Özel Betik Uzantısı, Azure VM’lerinde betik indirir ve yürütür. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Oluşturma ve özel bir VM görüntüsü ölçek ile kullanma hakkında daha fazla bilgi için aşağıdaki öğreticiler bakın:

- [Azure CLI 2.0](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Azure Resource Manager şablonu](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Bir Windows PowerShell DSC ile VM uygulama yükleme
[PowerShell istenen durum yapılandırması (DSC)](https://msdn.microsoft.com/powershell/dsc/overview) hedef makinelere yapılandırmasını tanımlamak için bir yönetim platformu. DSC yapılandırmaları ne bir makineye yüklemek ve ana bilgisayar yapılandırma tanımlayın. İstenen eylemlerini basılmış yapılandırmalarını temel alan işler her bir hedef düğümde yerel Configuration Manager (LCM'yi) altyapısı çalışır.

PowerShell DSC uzantısı ölçeği PowerShell ile Ayarla VM örnekleri özelleştirmenizi sağlar. Aşağıdaki örnek:

- Github'dan DSC paketini indirmek için VM örnekleri bildirir- *https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Bir yükleme komut dosyasını çalıştırmak için uzantı ayarlar- `configure-http.ps1`
- Bir ölçek kümesi hakkındaki bilgileri alır [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- VM örnekleriyle uzantısı uygulandığı [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

DSC uzantı uygulandığı *myScaleSet* adlı kaynak grubunu VM örnekleri *myResourceGroup*. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Ölçek kümesindeki yükseltme ilkesi ise *el ile*, VM örnekleriyle güncelleştirme [güncelleştirme AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Bu cmdlet, güncelleştirilmiş ölçek kümesi yapılandırması VM örnekleri için geçerlidir ve uygulamanızı yükler.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Bir Linux VM bulut init ile uygulama yükleme
[Cloud-init](https://cloudinit.readthedocs.io/latest/), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. cloud-init önyükleme işlemi sırasında çalışırken, yapılandırmanıza uygulayabileceğiniz ek adım veya gerekli aracı yoktur.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Daha fazla bilgi için bir örnek de dahil olmak üzere *bulut init.txt* dosya için bkz: [Azure VM'ler özelleştirmek için bulut init kullanın](../virtual-machines/linux/using-cloud-init.md).

Bir ölçek kümesi oluşturmak ve bir bulut init dosyası kullanmak için add `--custom-data` parametresi [az vmss oluşturma](/cli/azure/vmss#az_vmss_create) komut ve bir bulut init dosyasının adını belirtin. Aşağıdaki örnek, ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *myResourceGroup* ve VM örneği adlı bir dosya ile yapılandırır *bulut init.txt*. Aşağıdaki gibi kendi adlarınızı girin:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


### <a name="install-applications-with-os-updates"></a>Uygulamaları ile işletim sistemi güncelleştirmelerini yükleyin
Yeni işletim sistemi sürümleri kullanılabilir olduğunda kullanabilir veya yeni bir özel görüntü oluşturabilirsiniz ve [işletim sistemi yükseltme dağıtmak](virtual-machine-scale-sets-upgrade-scale-set.md) bir ölçek kümesi. Her VM örneği, belirttiğiniz en son görüntüyü yükseltilir. Yükseltme gerçekleştirirken uygulamanızı otomatik olarak kullanılabilir olması için önceden yüklenmiş uygulamayla özel betik uzantısı veya PowerShell DSC özel bir görüntü kullanabilirsiniz. Uyumluluk sorunları hiçbir sürüm olduğundan emin olmak için bu işlemi gerçekleştirmek için uygulama bakım planı gerekebilir.

Uygulamanın önceden yüklü olduğu özel bir VM görüntüsü kullanıyorsanız, yeni görüntüleri oluşturmak ve işletim sistemi yükseltmeleri ölçek kümesi boyunca dağıtmak için bir dağıtım ardışık düzen uygulama güncelleştirmeleri tümleştirileceğini. Bu yaklaşım, en son uygulama derlemeleri çekme, oluşturma ve bir VM görüntüsü doğrulayarak yeniden ölçek kümesi VM örnekleri yükseltme ardışık düzen sağlar. Derlemeler özel VM görüntüleri uygulama güncelleştirmeleri dağıtan bir dağıtım ardışık düzeni ve çalıştırmak için verebilir [Packer görüntü oluşturma ve Visual Studio Team Services ile dağıtma](/vsts/pipelines/apps/cd/azure/deploy-azure-scaleset), veya başka bir platform gibi kullandığınız [Spinnaker ](https://www.spinnaker.io/) veya [Jenkins](https://jenkins.io/).


## <a name="next-steps"></a>Sonraki adımlar
Derleme ve ölçek kümeleri uygulamaları dağıtmak gibi gözden geçirebilirsiniz [ölçek kümesi tasarım genel bakış](virtual-machine-scale-sets-design-overview.md). Ölçek kümenizi yönetme hakkında daha fazla bilgi için bkz: [kullanın, Ölçek kümesini yönetmek için PowerShell](virtual-machine-scale-sets-windows-manage.md).
