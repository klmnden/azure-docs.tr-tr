---
title: "Bir uygulamayı bir Azure sanal makine ölçek kümesini dağıtma | Microsoft Docs"
description: "Ölçek kümesindeki sanal makine örnekleri Linux ve Windows uygulamaları dağıtmayı öğrenin"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: iainfou
ms.openlocfilehash: 7e03d5e2bbdb1b3b206fa7fa455f7dce7951f02b
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri, uygulamanızda dağıtma
Ölçek kümesindeki sanal makine (VM) örneklerinde uygulamalarını çalıştırmak için önce gerekli dosyaları ve uygulama bileşenleri yüklemeniz gerekir. Bu makalede bir ölçek durumlarda ayarlayın ya da var olan VM örneklerini otomatik olarak yükleme betikleri çalıştırmak için özel bir VM görüntüsü oluşturmak için yollar sağlar. Ayrıca uygulama veya işletim sistemi güncelleştirmelerini ölçek kümesi boyunca yönetmeyi öğrenin.


## <a name="build-a-custom-vm-image"></a>Özel bir VM görüntüsü oluşturma
Azure platformu görüntülerden birini ölçek kümesinde örnekleri oluşturmak için kullandığınızda, başka bir yazılım yüklenmedi veya yapılandırılmadı. Bu bileşenlerin yüklenmesi ancak ölçek kümesi VM örnekleri sağlamak için gereken süreyi ekleyen otomatikleştirebilirsiniz. VM örnekleri için birçok yapılandırma değişiklikleri uygularsanız, yönetim ek yükü ile bu yapılandırma komut dosyaları ve görevleri yoktur.

Yapılandırma yönetimi ve bir VM sağlayacak süresini azaltmak için bir örnek sağlanan hemen sonra uygulamanızın ölçek kümesinde çalıştırılmaya hazır özel bir VM görüntüsü oluşturabilirsiniz. Ölçek için özel bir VM görüntüsü kümesi örnekleri oluşturmak için genel işlem aşağıdaki gibidir:

1. Özel bir VM görüntüsü için ölçek kümesi örnekleri oluşturmak için oluşturmak ve bir VM için oturum açma sonra yükleyin ve uygulamayı yapılandırın. Packer tanımlamak ve oluşturmak için kullanabileceğiniz bir [Linux](../virtual-machines/linux/build-image-with-packer.md) veya [Windows](../virtual-machines/windows/build-image-with-packer.md) VM görüntüsü. Veya el ile oluşturun ve VM yapılandırın:

    - Bir Linux VM oluşturma [Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md), [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md), veya [portal](../virtual-machines/linux/quick-create-portal.md).
    - Bir Windows VM ile oluşturma [Azure PowerShell](../virtual-machines/windows/quick-create-powershell.md), [Azure CLI 2.0](../virtual-machines/windows/quick-create-cli.md), veya [portal](../virtual-machines/windows/quick-create-portal.md).
    - Oturum bir [Linux](../virtual-machines/linux/mac-create-ssh-keys.md#use-the-ssh-key-pair) veya [Windows](../virtual-machines/windows/connect-logon.md) VM.
    - Yükleyin ve gereken araçları ve uygulamalarını yapılandırın. Bir kitaplık veya çalışma zamanı belirli sürümlerini gerekiyorsa, özel bir VM görüntüsü, bir sürüm tanımlamanıza olanak sağlar ve 

2. VM ile yakalama [Azure CLI 2.0](../virtual-machines/linux/capture-image.md) veya [Azure PowerShell](../virtual-machines/windows/capture-image.md). Bu adım bir ölçek kümesindeki örnekleri dağıtmak için kullanılan özel VM görüntüsü oluşturur.

3. [Bir ölçek kümesi oluşturma](virtual-machine-scale-sets-create.md) ve önceki adımlarda oluşturduğunuz özel VM görüntüsü belirtin.


## <a name="already-provisioned"></a>Uygulama özel betik uzantısı ile yükleme
Özel betik uzantısının indirir ve Azure Vm'lerinde komut dosyaları çalıştırılır. Bu dağıtım yapılandırmaları, yazılım yükleme veya başka bir yapılandırma için yararlı bir uzantısıdır / yönetim görevi. Komut dosyaları Azure depolama veya GitHub indirilen veya Azure portalı uzantısı çalışma zamanında sağlanan.

Özel betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalında veya Azure sanal makine REST API'sini kullanarak da çalıştırılabilir. 

Daha fazla bilgi için bkz: [özel betik uzantısının genel bakış](../virtual-machines/windows/extensions-customscript.md).


### <a name="use-azure-powershell"></a>Azure PowerShell kullanma
PowerShell indirmek için dosyayı depolamak için karma tablo ve yürütülecek komut kullanır. Aşağıdaki örnek:

- Bir komut dosyası Github'dan - indirmek için VM örnekleri bildirir *https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1*
- Bir yükleme komut dosyasını çalıştırmak için uzantı ayarlar-`powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1`
- Bir ölçek kümesi hakkındaki bilgileri alır [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- VM örnekleriyle uzantısı uygulandığı [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

Özel betik uzantısının uygulandığı *myScaleSet* adlı kaynak grubunu VM örnekleri *myResourceGroup*. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
# Define the script for your Custom Script Extension to run
$customConfig = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Ölçek kümesindeki yükseltme ilkesi ise *el ile*, VM örnekleriyle güncelleştirme [güncelleştirme AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Bu cmdlet, güncelleştirilmiş ölçek kümesi yapılandırması VM örnekleri için geçerlidir ve uygulamanızı yükler.


### <a name="use-azure-cli-20"></a>Azure CLI 2.0 kullanın
Özel betik uzantısının Azure CLI ile kullanmak için hangi almak için dosyaları ve yürütülecek komut tanımlayan bir JSON dosyası oluşturun. Bu JSON tanımları tutarlı uygulama yüklemelerini uygulamak için ölçek kümesi dağıtımlar arasında yeniden kullanılabilir.

Geçerli kabuğunuzu adlı bir dosya oluşturun *customConfig.json* ve aşağıdaki yapılandırma yapıştırın. Örneğin, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Girin `sensible-editor cloudConfig.json` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Ölçek kümesi VM örnekleri özel betik uzantısı yapılandırma uygulamak [az vmss uzantı kümesi](/cli/azure/vmss/extension#set). Aşağıdaki örnek uygular *customConfig.json* yapılandırmasına *myScaleSet* adlı kaynak grubunu VM örnekleri *myResourceGroup*. Aşağıdaki gibi kendi adlarınızı girin:

```azurecli
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroup \
    --vmss-name myScaleSet \
    --settings @customConfig.json
```

Ölçek kümesindeki yükseltme ilkesi ise *el ile*, VM örnekleriyle güncelleştirme [az vmss güncelleştirme-örnekleri](/cli/azure/vmss#update-instances). Bu cmdlet, güncelleştirilmiş ölçek kümesi yapılandırması VM örnekleri için geçerlidir ve uygulamanızı yükler.


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Bir Windows PowerShell DSC ile VM uygulama yükleme
[PowerShell istenen durum yapılandırması (DSC)](https://msdn.microsoft.com/en-us/powershell/dsc/overview) hedef makinelere yapılandırmasını tanımlamak için bir yönetim platformu. DSC yapılandırmaları ne bir makineye yüklemek ve ana bilgisayar yapılandırma tanımlayın. İstenen eylemlerini basılmış yapılandırmalarını temel alan işler her bir hedef düğümde yerel Configuration Manager (LCM'yi) altyapısı çalışır.

PowerShell DSC uzantısı ölçeği PowerShell ile Ayarla VM örnekleri özelleştirmenizi sağlar. Aşağıdaki örnek:

- Github'dan - DSC paketini indirmek için VM örnekleri bildirir *https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip*
- Bir yükleme komut dosyasını çalıştırmak için uzantı ayarlar-`configure-http.ps1`
- Bir ölçek kümesi hakkındaki bilgileri alır [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- VM örnekleriyle uzantısı uygulandığı [güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)

DSC uzantı uygulandığı *myScaleSet* adlı kaynak grubunu VM örnekleri *myResourceGroup*. Aşağıdaki gibi kendi adlarınızı girin:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/iainfoulds/azure-samples/raw/master/dsc.zip";
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
[Bulut init](https://cloudinit.readthedocs.io/latest/) ilk kez önyükleme gibi bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. Bulut init paketleri yüklemek ve dosyaları yazma veya kullanıcılar ve güvenlik yapılandırmak için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çalışırken, ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.

Bulut init dağıtımları üzerinde de çalışır. Örneğin, kullanmadığınız **get apt yükleme** veya **yum yükleme** bir paketi yüklemek için. Bunun yerine, yüklemek için paketlerin listesini tanımlayabilirsiniz. Bulut init otomatik olarak seçtiğiniz distro için yerel paket Yönetim Aracı'nı kullanır.

Daha fazla bilgi için bir örnek de dahil olmak üzere *bulut init.txt* dosya için bkz: [Azure VM'ler özelleştirmek için bulut init kullanın](../virtual-machines/linux/using-cloud-init.md).

Bir ölçek kümesi oluşturmak ve bir bulut init dosyası kullanmak için add `--custom-data` parametresi [az vmss oluşturma](/cli/azure/vmss#create) komut ve bir bulut init dosyasının adını belirtin. Aşağıdaki örnek, ölçeği adlandırılmış Ayarla oluşturur *myScaleSet* içinde *myResourceGroup* ve VM örneği adlı bir dosya ile yapılandırır *bulut init.txt*. Aşağıdaki gibi kendi adlarınızı girin:

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


## <a name="install-applications-as-a-set-scales-out"></a>Bir çıkış olarak uygulamaları yükleme
Ölçek kümeleri, uygulamanızı çalıştıran VM örneği sayısını artırmak izin verir. Bu işlem genişletme el ile başlatıldı veya ölçümleri CPU veya bellek kullanımı gibi temel alınarak otomatik olarak.

Ölçek kümesi için bir özel betik uzantısının uyguladıysanız, uygulama her yeni VM örneğine yüklenir. Ölçek kümesi üzerinde özel bir görüntü uygulamanın önceden yüklü olduğu temel alıyorsa, her yeni VM örneği kullanılabilir durumda dağıtılır. 

Ölçek kümesi VM örnekleri kapsayıcı ana bilgisayar varsa, özel betik uzantısının çekme ve kapsayıcı görüntüleri gereken çalıştırmak için kullanabilirsiniz. Özel betik uzantısı, Azure kapsayıcı hizmeti gibi bir orchestrator ile yeni VM örneği de kaydı yapılamadı.


## <a name="deploy-application-updates"></a>Uygulama güncelleştirmelerini dağıtma
Uygulama kodu, kitaplıkları veya paketler güncelleştirirseniz, Ölçek kümesi VM örnekleri en son uygulama durumu gönderebilir. Varsa özel betik uzantısı, uygulamanız için güncelleştirmeleri kullanın ve otomatik olarak dağıtılır. Güncelleştirilmiş sürüm ada sahip bir yükleme komut dosyası işaret edecek şekilde gibi özel komut dosyası yapılandırmasını değiştirin. Önceki örnekte, özel betik uzantısının adlı bir komut dosyası kullanan *automate_nginx.sh* gibi:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx.sh"],
  "commandToExecute": "./automate_nginx.sh"
}
```

Betik değişiklikleri yüklemediğiniz sürece, uygulamanız için yaptığınız herhangi bir güncelleştirme için özel betik uzantısı sunulmaz. Uygulamanız ile artışlarla yayımları bir sürüm numarası dahil bir yaklaşımdır. Özel betik uzantısı artık başvurabilir *automate_nginx_v2.sh* gibi:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate_nginx_v2.sh"],
  "commandToExecute": "./automate_nginx_v2.sh"
}
```

Özel betik uzantısının şimdi son uygulama güncelleştirmeleri uygulamak için VM örnekleri karşı çalışır.


### <a name="install-applications-with-os-updates"></a>Uygulamaları ile işletim sistemi güncelleştirmelerini yükleyin
Yeni işletim sistemi sürümleri kullanılabilir olduğunda kullanabilir veya yeni bir özel görüntü oluşturabilirsiniz ve [işletim sistemi yükseltme dağıtmak](virtual-machine-scale-sets-upgrade-scale-set.md) bir ölçek kümesi. Her VM örneği, belirttiğiniz en son görüntüyü yükseltilir. Yükseltme gerçekleştirirken uygulamanızı otomatik olarak kullanılabilir olması için önceden yüklenmiş uygulamayla özel betik uzantısı veya PowerShell DSC özel bir görüntü kullanabilirsiniz. Uyumluluk sorunları hiçbir sürüm olduğundan emin olmak için bu işlemi gerçekleştirmek için uygulama bakım planı gerekebilir.

Uygulamanın önceden yüklü olduğu özel bir VM görüntüsü kullanıyorsanız, yeni görüntüleri oluşturmak ve işletim sistemi yükseltmeleri ölçek kümesi boyunca dağıtmak için bir dağıtım ardışık düzen uygulama güncelleştirmeleri tümleştirileceğini. Bu yaklaşım, en son uygulama derlemeleri çekme, oluşturma ve bir VM görüntüsü doğrulayarak yeniden ölçek kümesi VM örnekleri yükseltme ardışık düzen sağlar. Derlemeler özel VM görüntüleri uygulama güncelleştirmeleri dağıtan bir dağıtım ardışık düzeni ve çalıştırmak için kullanabilirsiniz [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Spinnaker](https://www.spinnaker.io/), veya [Jenkins](https://jenkins.io/) .


## <a name="next-steps"></a>Sonraki adımlar
Derleme ve ölçek kümeleri uygulamaları dağıtmak gibi gözden geçirebilirsiniz [ölçek kümesi tasarım genel bakış](virtual-machine-scale-sets-design-overview.md). Ölçek kümenizi yönetme hakkında daha fazla bilgi için bkz: [kullanın, Ölçek kümesini yönetmek için PowerShell](virtual-machine-scale-sets-windows-manage.md).
