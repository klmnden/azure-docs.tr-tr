---
title: Azure’da Windows VM’ler için kullanılabilirlik kümeleri öğreticisi | Microsoft Docs
description: Azure’da Windows VM’ler için Kullanılabilirlik Kümeleri hakkında bilgi edinin.
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
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 980dd0a96cee16598992f184579d2195c2be8b52
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-use-availability-sets"></a>Kullanılabilirlik kümelerini kullanma

Bu öğreticide, Kullanılabilirlik Kümeleri adlı bir özellik kullanarak Azure’da Sanal Makine çözümlerinizin kullanılabilirlik ve güvenilirliğini nasıl artıracağınızı öğreneceksiniz. Kullanılabilirlik kümeleri, Azure’da dağıttığınız VM’lerin birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Böylece, Azure’da bir donanım veya yazılım hatası oluşursa yalnızca sanal makinelerinizin bir alt kümesinin etkilenmesi ve genel çözümünüzün kullanılabilir ve çalışır durumda kalması sağlanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme
> * Azure Danışmanı’nı denetleme

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="availability-set-overview"></a>Kullanılabilirlik kümesine genel bakış

Kullanılabilirlik Kümesi, içine yerleştirdiğiniz sanal makine kaynaklarının, Azure veri merkezinde dağıtıldığında birbirinden yalıtılmasını sağlamak için Azure’da kullanabileceğiniz bir mantıksal gruplama yeteneğidir. Azure, bir Kullanılabilirlik Kümesi içine yerleştirdiğiniz sanal makinelerin birden fazla fiziksel sunucuda, bilgi işlem rafında, depolama biriminde ve ağ anahtarında çalışmasını sağlar. Bir donanım veya Azure yazılım hatası oluşursa, yalnızca sanal makinelerinizin bir alt kümesi etkilenir ve genel uygulama güncel kalıp müşterilerinizin kullanımına sunulmaya devam eder. Kullanılabilirlik Kümesi, güvenilir bulut çözümleri oluşturmak istediğinizde gerekli olan temel bir yetenektir.

Dört adet ön uç web sunucusuna sahip olabileceğiniz ve bir veritabanını barındıran iki adet arka uç sanal makine kullandığınız tipik bir sanal makine tabanlı çözümü düşünelim. Azure ile, sanal makinelerinizi dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: bir kullanılabilirlik kümesi, "web" katmanı için ve bir kullanılabilirlik kümesi de "veritabanı" katmanı için. Yeni bir sanal makine oluşturduğunuzda, az vm create komutuna parametre olarak kullanılabilirlik kümesini belirtebilirsiniz ve Azure da otomatik olarak kullanılabilirlik kümesi içinde oluşturduğunuz sanal makinelerin birden fazla fiziksel donanım kaynağı arasında yalıtılmasını sağlar. Web Sunucusu veya Veritabanı Sunucusu Sanal Makinelerinizden birinin çalıştığı fiziksel donanımda sorun varsa, Web Sunucunuzun ve Veritabanı Sanal Makinelerinizin diğer örneklerinin farklı donanımlarda bulunması nedeniyle çalışmaya devam edeceğini bilirsiniz.

Azure’da güvenilir sanal makine tabanlı çözümleri dağıtmak istediğinizde Kullanılabilirlik Kümelerini kullanın.

## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

[New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) komutunu kullanarak bir kullanılabilirlik kümesi oluşturabilirsiniz. Bu örnekte, *myResourceGroupAvailability* kaynak grubundaki *myAvailabilitySet* adlı kullanılabilirlik kümesi için *2* konumundaki güncelleştirme ve hata etki alanları sayısını ayarlayın.

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

[New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) komutunu `-sku aligned` parametresiyle kullanarak yönetilen bir kullanılabilirlik kümesi oluşturun.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Kullanılabilirlik kümesi içinde sanal makineler oluşturma
Donanım arasında doğru şekilde dağıtıldığından emin olmak için sanal makinelerin kullanılabilirlik kümesi içinde oluşturulması gerekir. Oluşturulduktan sonra kullanılabilirlik kümesine mevcut bir sanal makineyi ekleyemezsiniz. 

Konumdaki donanım, birden çok etki alanına ve hata etki alanına bölünmüştür. **Güncelleştirme etki alanı**, aynı anda yeniden başlatılabilen bir VM grubu ve temel alınan fiziksel donanımdır. Aynı **hata etki alanında** bulunan VM’ler, ortak güç kaynağı ve ağ anahtarıyla birlikte ortak depolama alanını paylaşır. 

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile yeni bir VM oluşturduğunuzda, kullanılabilirlik kümesinin adını belirtmek için `-AvailabilitySetName` parametresini kullanın.

İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi de kullanılabilirlik kümesinde [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile iki VM oluşturun.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzureRmVm `
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

PowerShell komut istemlerinin size döndürülmesi için `-AsJob` parametresi VM’yi arka plan görevi olarak oluşturur. Arka plan işlerinin ayrıntılarını `Job` cmdlet'i ile görüntüleyebilirsiniz. İki VM’yi de oluşturup yapılandırmak birkaç dakika sürer. Tamamlandığında, temel alınan donanım arasında dağıtılmış iki sanal makineye sahip olursunuz. 

Kaynak Grupları > myResourceGroupAvailability > myAvailabilitySet bölümüne giderek portaldaki kullanılabilirlik kümesine bakarsanız, sanal makinelerin iki hata ve güncelleştirme etki alanı arasında nasıl dağıtıldığını görmeniz gerekir.

![Portaldaki kullanılabilirlik kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir sanal makine boyutlarını denetleme 

Daha sonra kullanılabilirlik kümesine daha fazla sanal makine ekleyebilirsiniz; ancak donanımda hangi sanal makine boyutlarının kullanılabilir olduğunu bilmeniz gerekir. Kullanılabilirlik kümesi için donanım kümesindeki tüm kullanılabilir boyutları listelemek için [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) komutunu kullanın.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Azure Danışmanı’nı denetleme 

Ayrıca VM’lerinizin kullanılabilirliğini geliştirmeye yönelik yollar hakkında daha fazla bilgi edinmek için Azure Danışmanı’nı kullanabilirsiniz. Azure Danışmanı, Azure dağıtımlarınızı iyileştirecek en iyi uygulamaları izlemenize yardımcı olur. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

[Azure portal](https://portal.azure.com)’ında oturum açın, **Tüm hizmetler**’i seçin ve **Danışman** yazın. Danışman panosu, seçili abonelik için kişiselleştirilmiş önerileri görüntüler. Daha fazla bilgi için bkz. [Azure Danışmanı’nı kullanmaya başlayın](../../advisor/advisor-get-started.md).


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


