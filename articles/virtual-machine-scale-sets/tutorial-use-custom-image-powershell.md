---
title: Öğretici - Azure PowerShell ile bir ölçek kümesindeki özel sanal makine görüntüsünü kullanma | Microsoft Docs
description: Azure PowerShell kullanarak, sanal makine ölçek kümesini dağıtmak için kullanabileceğiniz bir özel sanal makine görüntüsünün nasıl oluşturulacağını öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6fa1deb8e7a1a7ddd28583b6df7bad9df57738ed
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="tutorial-create-and-use-a-custom-image-for-virtual-machine-scale-sets-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümeleri için özel görüntü oluşturma ve kullanma
Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. Sanal makine örnekleri dağıtıldıktan sonraki görev sayısını azaltmak için özel bir sanal makine görüntüsünü kullanabilirsiniz. Bu özel sanal makine görüntüsü, gerekli uygulama yüklemelerini veya yapılandırmalarını içerir. Ölçek kümesinde oluşturulan tüm sanal makine örnekleri, özel sanal makine görüntüsünü kullanır ve uygulama trafiğinizi sunmaya hazır olur. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma ve özelleştirme
> * Sanal makinenin sağlamasını kaldırma ve sanal makineyi genelleştirme
> * Kaynak sanal makineden özel bir sanal makine görüntüsü oluşturma
> * Özel sanal makine görüntüsünü kullanan bir ölçek kümesini dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.6.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 


## <a name="create-and-configure-a-source-vm"></a>Kaynak sanal makine oluşturma ve yapılandırma

>[!NOTE]
> Bu öğreticide, genelleştirilmiş bir sanal makine görüntüsü oluşturma ve kullanma işlemi gösterilmektedir. Özelleştirilmiş bir sanal makine görüntüsünden ölçek kümesi oluşturulması desteklenmez.

İlk olarak, [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile bir kaynak grubu oluşturun ve sonra [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile bir sanal makine oluşturun. Bu sanal makine daha sonra özel bir sanal makine görüntüsü için kaynak olarak kullanılır. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myCustomVM* adlı bir sanal makine oluşturur. İstendiğinde, sanal makine için oturum açma kimlik bilgileri olarak kullanılacak bir kullanıcı adı ve parola girin:

```azurepowershell-interactive
# Create a resource a group
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"

# Create a Windows Server 2016 Datacenter VM
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Name "myCustomVM" `
  -ImageName "Win2016Datacenter" `
  -OpenPorts 3389
```

Sanal makinenize bağlanmak için aşağıdaki adımları uygulayarak [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile genel IP adresini listeleyin:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Sanal makineyle uzaktan bağlantı oluşturun. Azure Cloud Shell kullanıyorsanız, bu adımı yerel PowerShell isteminden veya Uzak Masaüstü İstemcisinden gerçekleştirin. Önceki komuttan kendi IP adresinizi sağlayın. İstendiğinde, ilk adımda sanal makineyi oluştururken kullandığınız kimlik bilgilerini girin:

```powershell
mstsc /v:<IpAddress>
```

Şimdi sanal makinenizi özelleştirmek için bir temel web sunucusu yükleyelim. Ölçek kümesindeki sanal makine örneği dağıtılacağı zaman, bir web uygulamasını çalıştırmak için gerekli tüm paketleri içerir. Sanal makinede yerel bir PowerShell istemini açın ve aşağıdaki adımları uygulayarak [Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature) ile IIS web sunucusunu yükleyin:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Sanal makinenizi özel görüntü olarak kullanılmaya hazırlamanın son adımı, sanal makinenizin genelleştirilmesidir. Sysprep tüm kişisel hesap bilgilerinizi ve yapılandırmalarınızı kaldırır ve sanal makineyi gelecekteki dağıtımlar için sıfırlayıp temiz bir duruma getirir. Daha fazla bilgi için bkz. [Sysprep İşlemini Kullanma: Giriş](http://technet.microsoft.com/library/bb457073.aspx).

Sanal makineyi genelleştirmek için Sysprep işlemini çalıştırıp sanal makineyi ayarlayarak hızlı bir deneyim elde edin. İşlem tamamlandığında, Sysprep işlemine sanal makineyi kapatmasını bildirin:

```powershell
C:\Windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
```

Sysprep, işlemi tamamladığında ve sanal makine kapatıldığında sanal makineyle uzaktan bağlantı otomatik olarak kapanır.


## <a name="create-a-custom-vm-image-from-the-source-vm"></a>Kaynak sanal makineden özel bir sanal makine görüntüsü oluşturma
Kaynak sanal makine şimdi IIS web sunucusunun yüklenmesiyle birlikte özelleştirilir. Şimdi ölçek kümesi ile kullanılacak özel sanal makine görüntüsü oluşturalım.

Bir görüntü oluşturmak için VM’nin serbest bırakılması gerekir. [Stop-AzureRmVm](/powershell/module/azurerm.compute/stop-azurermvm) ile sanal makineyi serbest bırakın. Daha sonra, Azure platformunun sanal makinenin özel bir görüntüyle kullanılmaya hazır olduğunu bilmesi için [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) komutunu kullanarak sanal makinenin durumunu genelleştirilmiş olarak ayarlayın. Yalnızca genelleştirilmiş bir sanal makineden görüntü oluşturabilirsiniz:

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Force
Set-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Generalized
```

Sanal makinenin serbest bırakılıp genelleştirilmesi birkaç dakika sürebilir.

Şimdi [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) ve [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage) ile sanal makinenin bir görüntüsünü oluşturun. Aşağıdaki örnek, sanal makinenizden *myImage* adlı bir görüntü oluşturur:

```azurepowershell-interactive
# Get VM object
$vm = Get-AzureRmVM -Name "myCustomVM" -ResourceGroupName "myResourceGroup"

# Create the VM image configuration based on the source VM
$image = New-AzureRmImageConfig -Location "EastUS" -SourceVirtualMachineId $vm.ID 

# Create the custom VM image
New-AzureRmImage -Image $image -ImageName "myImage" -ResourceGroupName "myResourceGroup"
```


## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Özel bir sanal makine görüntüsünden ölçek kümesi oluşturma
Şimdi, önceki adımda oluşturulan özel sanal makine görüntüsünü tanımlamak için `-ImageName` parametresini kullanan [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) ile bir ölçek kümesi oluşturun. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem TCP bağlantı noktası 80 üzerinden trafiği dağıtmak hem de TCP bağlantı noktası 3389 üzerinden uzak masaüstü trafiğine hem de TCP bağlantı noktası 5985 üzerinden PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -ImageName "myImage"
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümenizi çalışır halde görmek için [Get-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress) ile yük dengeleyicinizin genel IP adresini alın:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Name "myPublicIPAddress" | Select IpAddress
```

Genel IP adresini bir web tarayıcınıza yazın. Aşağıdaki örnekte gösterildiği gibi varsayılan IIS web sayfası görüntülenir:

![Özel sanal makine görüntüsünden çalıştırılan IIS](media/tutorial-use-custom-image-powershell/default-iis-website.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve ek kaynaklarınızı kaldırmak için [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve bu kaynak grubunun tüm kaynaklarını silin. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure PowerShell ile ölçek kümeleriniz için özel sanal makine görüntüsü oluşturma ve kullanma işleminin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma ve özelleştirme
> * Sanal makinenin sağlamasını kaldırma ve sanal makineyi genelleştirme
> * Özel bir sanal makine görüntüsü oluşturma
> * Özel sanal makine görüntüsünü kullanan bir ölçek kümesini dağıtma

Uygulamaların ölçek kümenize nasıl dağıtılacağını öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Ölçek kümelerinize uygulama dağıtma](tutorial-install-apps-powershell.md)