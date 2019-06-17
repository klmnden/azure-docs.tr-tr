---
title: Bir Azure sanal makine ölçek kümesine uygulama dağıtma | Microsoft Docs
description: Linux ve Windows sanal makine örnekleri bir ölçek kümesindeki uygulamaları dağıtmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 09145612821cb669e26e3ccb8d15611112eca700
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60618634"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerinde uygulamanızı dağıtma

Bir ölçek kümesindeki sanal makine (VM) örneklerinde uygulamaları çalıştırmak için önce uygulama bileşenlerini ve gerekli dosyaları yüklemeniz gerekir. Bu makalede örnek bir ölçek kümesi veya otomatik olarak var olan VM örneklerinde yükleme betikleri çalıştırma için özel bir VM görüntüsü oluşturmak için yollar sağlar. Ayrıca bir ölçek kümesi üzerinde uygulama veya işletim sistemi güncelleştirmelerini yönetmeyi öğrenin.


## <a name="build-a-custom-vm-image"></a>Özel bir VM görüntüsü oluşturma
Ölçek kümenizdeki örnekleri oluşturmak için Azure platformu görüntülerden birini kullandığınızda, başka bir yazılım yüklü veya yapılandırılmış. Bu bileşenlerin yüklenmesini ancak ölçek kümeleriniz için sanal makine örnekleri sağlamak için gereken süreyi ekleyen otomatikleştirebilirsiniz. Sanal makine örneklerine birçok yapılandırma değişiklikleri uygularsanız, yönetim ek yükü ile bu yapılandırma betikleri ve görevleri yoktur.

Yapılandırma yönetimi ve bir VM sağlama süresini azaltmak için bir örneğin hazırlandığı hemen sonra ölçek kümesindeki uygulamanızı çalıştırmak hazır olan özel bir VM görüntüsü oluşturabilirsiniz. Oluşturma ve ölçek ile özel VM görüntüsü kullanma hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Azure CLI](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="already-provisioned"></a>Özel betik uzantısı ile uygulama yükleme
Özel Betik Uzantısı, Azure VM’lerinde betik indirir ve yürütür. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Oluşturma ve ölçek ile özel VM görüntüsü kullanma hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:

- [Azure CLI](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Azure Resource Manager şablonu](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Bir Windows PowerShell DSC ile VM için bir uygulama yükleyin
[PowerShell Desired State Configuration (DSC)](https://msdn.microsoft.com/powershell/dsc/overview) hedef makineler tanımlamak için bir yönetim platformu. DSC yapılandırmaları ne bir makineye yükleyin ve ana bilgisayar yapılandırma tanımlar. İstenen Eylemler gönderilen yapılandırmalarına göre işler her hedef düğümde yerel Configuration Manager'ı (LCM) altyapısı çalıştırır.

PowerShell DSC uzantısı PowerShell ile ölçek kümesi VM örnekleri özelleştirmenizi sağlar. Aşağıdaki örnekte:

- Github'dan bir DSC paketini indirmek için sanal makine örneklerine yönlendirir- *https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Uzantı yükleme betiğini çalıştırmak için ayarlar- `configure-http.ps1`
- Bir ölçek kümesi hakkında bilgi alır [Get-AzVmss](/powershell/module/az.compute/get-azvmss)
- Uzantısı ile sanal makine örnekleri için geçerlidir [AzVmss güncelleştirme](/powershell/module/az.compute/update-azvmss)

DSC uzantısı uygulanan *myScaleSet* adlı kaynak grubunda sanal makine örneklerine *myResourceGroup*. Şu şekilde kendi adlarınızı girin:

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
$vmss = Get-AzVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Ölçek kümenizi yükseltme ilkesindeki ise *el ile*, güncelleştirme, VM örnekleri ile [güncelleştirme AzVmssInstance](/powershell/module/az.compute/update-azvmssinstance). Bu cmdlet, güncelleştirilmiş bir ölçek kümesi yapılandırması VM örnekleri için geçerlidir ve uygulamanız yükler.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Cloud-init ile Linux VM için bir uygulama yükleyin
[Cloud-init](https://cloudinit.readthedocs.io/en/latest/index.html), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. cloud-init önyükleme işlemi sırasında çalışırken, yapılandırmanıza uygulayabileceğiniz ek adım veya gerekli aracı yoktur.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Daha fazla bilgi için bir örnek de dahil olmak üzere *cloud-init.txt* bkz [Azure Vm'leri özelleştirmek için cloud-init kullanma](../virtual-machines/linux/using-cloud-init.md).

Bir ölçek kümesi oluşturma ve cloud-init dosyası kullanmak için ekleme `--custom-data` parametresi [az vmss oluşturma](/cli/azure/vmss) komut ve cloud-init dosyası adını belirtin. Aşağıdaki örnekte adlı bir ölçek kümesi oluşturur *myScaleSet* içinde *myResourceGroup* ve adlı bir dosya ile sanal makine örnekleri yapılandırır *cloud-init.txt*. Şu şekilde kendi adlarınızı girin:

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


### <a name="install-applications-with-os-updates"></a>İşletim sistemi güncelleştirmelerini uygulama yükleme
Yeni işletim sistemi sürümleri kullanılabilir olduğunda kullanabilir veya yeni bir özel görüntü oluşturun ve [işletim sistemi yükseltmelerini dağıtma](virtual-machine-scale-sets-upgrade-scale-set.md) bir ölçek kümesi. Her sanal makine örneği, belirttiğiniz en son görüntüye yükseltilir. Yükseltmeyi gerçekleştirirken uygulamanızı otomatik olarak kullanılabilir olması için önceden yüklenmiş, uygulama ile özel betik uzantısı ya da PowerShell DSC özel bir görüntü kullanabilirsiniz. Uyumluluk sorunlarını sürüm olduğundan emin olmak için bu işlemi gerçekleştirmek için Uygulama Bakımı Planla gerekebilir.

Uygulamanın önceden yüklü olduğu bir özel VM görüntüsü kullanıyorsanız, uygulama güncelleştirmeleri, yeni görüntülerinizi oluşturmak ve işletim sistemi yükseltmelerini ölçek kümesi arasında dağıtmak için bir dağıtım işlem hattı ile de tümleştirebilirsiniz. Bu yaklaşım, en son uygulama yapılarını seçin, oluşturma ve bir VM görüntüsü doğrulayarak yeniden yükseltme ölçek kümesindeki sanal makine örnekleri için işlem hattı sağlar. Derlemeler ve uygulama güncelleştirmeleri özel VM görüntülerini dağıtan bir dağıtım işlem hattı çalıştırmak için şunları yapabilirsiniz: [Packer görüntü oluşturup Azure DevOps hizmetleriyle](/azure/devops/pipelines/apps/cd/azure/deploy-azure-scaleset), veya başka bir platform kullanın [Spinnaker](https://www.spinnaker.io/) veya [Jenkins](https://jenkins.io/).


## <a name="next-steps"></a>Sonraki adımlar
Derleme ve ölçek kümelerinize uygulama dağıtma yaparken inceleyebilirsiniz [ölçek kümesi tasarım genel bakış](virtual-machine-scale-sets-design-overview.md). Ölçek kümenizi yönetme hakkında daha fazla bilgi için bkz. [ölçek kümenizi yönetmek için PowerShell kullanma](virtual-machine-scale-sets-windows-manage.md).
