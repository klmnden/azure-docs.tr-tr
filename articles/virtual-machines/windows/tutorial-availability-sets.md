---
title: Öğretici - Azure’da Windows VM’ler için yüksek oranda kullanılabilirlik | Microsoft Docs
description: Bu öğreticide, Kullanılabilirlik Kümelerinde yüksek oranda kullanılabilir sanal makineler dağıtmak için Azure PowerShell kullanmayı öğreneceksiniz
documentationcenter: ''
services: virtual-machines-windows
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: afc3e550f0a3d135f1c62ee321fff8d7afc5cae6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60785270"
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-azure-powershell"></a>Öğretici: Azure PowerShell ile yüksek oranda kullanılabilir sanal makineler oluşturup dağıtma

Bu öğreticide, kullanılabilirlik ve kullanılabilirlik kümeleri kullanarak sanal makinelerinizi (VM) güvenilirliğini artırma konusunda bilgi edinin. Kullanılabilirlik kümeleri, Azure'da dağıttığınız VM'lerin birden fazla düğümde, yalıtılmış bir donanım, bir kümedeki dağıtıldığından emin olun. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme
> * Azure Danışmanı’nı denetleme


## <a name="availability-set-overview"></a>Kullanılabilirlik kümesine genel bakış

Bir kullanılabilirlik kümesi dağıtıldıkları VM kaynakları birbirinden yalıtmak için bir mantıksal gruplama yeteneğidir. Azure VM'ler, birden çok fiziksel sunucuda çalıştırma bir kullanılabilirlik kümesi içinde eklediğinden emin olur, raflar, depolama birimi ve ağ anahtarları işlem. Bir donanım veya yazılım hatası oluşursa yalnızca sanal makinelerinizin bir alt etkilenir ve genel çözümünüzün işletimsel kalır. Kullanılabilirlik kümeleri, güvenilir bulut çözümleri oluşturmak için gereklidir.

Dört adet ön uç web sunucusuna ve iki adet arka uç sanal makineye sahip olabileceğiniz tipik bir sanal makine tabanlı çözümü düşünelim. Azure ile sanal makinelerinizi dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: bir web katmanı ve geri katmanı için bir tane. Yeni bir VM oluşturduğunuzda, kullanılabilirlik kümesi parametre olarak belirtin. Azure Vm'lerini birden fazla fiziksel donanım kaynağı arasında yalıtılır emin olur. Sunucularınızın birini çalıştıran fiziksel donanımı bir sorun varsa, bunlar farklı bir donanım üzerinde olduğundan diğer örneklerin sunucularınızın çalışmaya devam edeceği bildirin.

Azure’da güvenilir sanal makine tabanlı çözümleri dağıtmak istediğinizde Kullanılabilirlik Kümelerini kullanın.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

Konumdaki donanım, birden çok etki alanına ve hata etki alanına bölünmüştür. **Güncelleştirme etki alanı**, aynı anda yeniden başlatılabilen bir VM grubu ve temel alınan fiziksel donanımdır. Aynı **hata etki alanında** bulunan VM’ler, ortak güç kaynağı ve ağ anahtarıyla birlikte ortak depolama alanını paylaşır.  

Kullanarak bir kullanılabilirlik kümesi oluşturabileceğiniz [yeni AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset). Bu örnekte, hem güncelleştirme ve hata etki alanları sayısıdır *2* ve kullanılabilirlik kümesi adlı *myAvailabilitySet*.

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup `
   -Name myResourceGroupAvailability `
   -Location EastUS
```

Kullanarak bir yönetilen kullanılabilirlik kümesi oluşturma [yeni AzAvailabilitySet](https://docs.microsoft.com/powershell/module/az.compute/new-azavailabilityset) ile `-sku aligned` parametresi.

```azurepowershell-interactive
New-AzAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Kullanılabilirlik kümesi içinde sanal makineler oluşturma
VM'lerin kullanılabilirlik donanım arasında doğru dağıtılmış emin olmak için kümesi içinde oluşturulması gerekir. Varolan bir VM'yi kullanılabilirlik oluşturulduktan sonra kümesine eklenemiyor. 


Bir VM oluşturduğunuzda, [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm), kullandığınız `-AvailabilitySetName` parametresini kullanarak kullanılabilirlik kümesinin adını belirtin.

İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Artık ile iki VM oluşturma [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) de kullanılabilirlik kümesinde.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

İki VM’yi de oluşturup yapılandırmak birkaç dakika sürer. Tamamlandığında, temel alınan donanım arasında dağıtılmış iki sanal makineye sahip olursunuz. 

Kullanılabilirlik giderek portalda kümesini bakmak, **kaynak grupları** > **myResourceGroupAvailability** > **myAvailabilitySet**, sanal makinelerin iki hata arasında dağıtılır ve güncelleştirme etki alanı nasıl görmeniz gerekir.

![Portaldaki kullanılabilirlik kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir sanal makine boyutlarını denetleme 

Daha sonra kullanılabilirlik kümesine daha fazla sanal makine ekleyebilirsiniz; ancak donanımda hangi sanal makine boyutlarının kullanılabilir olduğunu bilmeniz gerekir. Kullanım [Get-AzVMSize](https://docs.microsoft.com/powershell/module/az.compute/get-azvmsize) tüm küme kullanılabilirlik kümesi için donanımı kullanılabilir boyutları listelemek için.

```azurepowershell-interactive
Get-AzVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Azure Danışmanı’nı denetleme 

Azure Danışmanı, Vm'lerinizin kullanılabilirliğini geliştirmeye yönelik daha fazla bilgi almak için de kullanabilirsiniz. Azure Danışmanı, yapılandırmanızı ve kullanım telemetrinizi analiz sonra maliyet verimliliği, performans, kullanılabilirlik ve Azure kaynaklarınızın güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

[Azure portal](https://portal.azure.com)’ında oturum açın, **Tüm hizmetler**’i seçin ve **Danışman** yazın. Danışman Panosu, seçili abonelik için kişiselleştirilmiş öneriler gösterir. Daha fazla bilgi için bkz. [Azure Danışmanı’nı kullanmaya başlayın](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme
> * Azure Danışmanı’nı denetleme

Sanal makine ölçek kümeleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [VM ölçek kümesi oluşturma](tutorial-create-vmss.md)


