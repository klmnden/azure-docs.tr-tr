---
title: 'Hızlı Başlangıç: Standart yük dengeleyici - Azure Resource Manager şablonu oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak bir Standard load balancer oluşturma işlemi gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 00dbb29cac30f2a3bbecf71727c79617e24af3e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441307"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-an-azure-resource-manager-template"></a>Hızlı Başlangıç: Sanal makineleri Azure Resource Manager şablonu kullanarak Yük Dengelemesi için standart yük dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye (VM) yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu hızlı başlangıçta sanal makinelerin Yük Dengelemesi için standart yük dengeleyici oluşturan bir Azure Resource Manager şablonu dağıtma gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-standard-load-balancer"></a>Standard load balancer oluşturma

Standart yük dengeleyici yalnızca standart genel IP adresi destekler. Standard load balancer'ı oluşturduğunuzda, standart yük dengeleyici için ön ucu olarak yapılandırılmış yeni bir standart genel IP adresi de oluşturmanız gerekir.

Standart yük dengeleyici oluşturmak için birçok yöntem kullanabilirsiniz. Bu hızlı başlangıçta, Azure PowerShell dağıtmak için kullandığınız bir [Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır.

Azure çözümlerinizi yönetme ve dağıtma ile ilgili kavramları anlamak için bkz: [Azure Resource Manager belgelerini](/azure/azure-resource-manager/). Azure yük dengeleyici ile ilgili daha fazla şablon bulmak için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

1. Seçin **deneyin** aşağıdaki kod bloğunun Azure Cloud Shell'i açın ve ardından Azure'da oturum açmak için yönergeleri izleyin.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Konsolundan istemi görene kadar bekleyin.

1. Seçin **kopyalama** PowerShell betiğini kopyalamak için önceki kod bloğunun.

1. Kabuk Konsol bölmesinde sağ tıklayın ve ardından **Yapıştır**.

1. Değerleri girin.

   Şablon dağıtımı üç kullanılabilirlik alanı oluşturur. Kullanılabilirlik alanları yalnızca desteklenen [belirli bölgelerde](../availability-zones/az-overview.md). Desteklenen bölgeleri birini kullanın. Emin değilseniz, girin **centralus**.

   Kaynak grubu adı ile proje adınızdır **rg** eklenir. Sonraki bölümde kaynak grubu adı ihtiyacınız vardır.

Şablonu dağıtmak için yaklaşık 10 dakika sürer.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **kaynak grupları** sol bölmeden.

1. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. Varsayılan kaynak grubu adı ile proje adınızdır **rg** eklenir.

1. Yük Dengeleyiciyi seçin. Varsayılan ad proje adıdır ile **-lb** eklenir.

1. Yalnızca genel IP adresi IP adresi bölümü kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. Tarayıcı, Internet Information Services (IIS) web sunucusunun varsayılan sayfası görüntüler.

   ![IIS web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği üç VM'ye dağıtmasını görmek için İstemci makinesinden web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında kaynak grubunu, Yük Dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için Azure portalında, Yük Dengeleyiciyi içeren kaynak grubunu seçin ve ardından Git **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, standart yük dengeleyici oluşturdunuz, Vm'lere bağlı, yük dengeleyici trafik kuralını yapılandırdınız durum araştırması yaptığınız ve sonra da Yük Dengeleyiciyi test ettiniz.

Daha fazla bilgi için yük dengeleyici için öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
 