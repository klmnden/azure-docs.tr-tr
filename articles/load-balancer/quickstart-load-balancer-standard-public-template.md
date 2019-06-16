---
title: 'Hızlı Başlangıç: Standart yük dengeleyici - Azure Resource Manager şablonu oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure Resource Manager şablonu kullanarak bir Standard Load Balancer oluşturma işlemi gösterilmektedir.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load Balancer by using Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: c418041c5de343d7210dbd153ebe6cea0af95c42
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066810"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Hızlı Başlangıç: Bir standart Azure Resource Manager şablonu kullanarak Yük Dengelemesi VM'ler için yük dengeleyici oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerde (VM) Yük Dengeleme için bir yük dengeleyici oluşturmak için bir Azure Resource Manager şablonu dağıtabilirsiniz. Bu hızlı başlangıçta Standart Yük Dengeleyici kullanılarak sanal makinelerde yük dengelemesi yapacağınız gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Bu bölümde, standart yük yardımcı olan sanal makinelerin yük dengelemesini dengeleyici oluşturun. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standart Yük Dengeleyici oluşturduğunuzda, Standart Yük Dengeleyici için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. Standart yük dengeleyici oluşturmak için kullanılan birçok yöntem vardır. Bu hızlı başlangıçta, Azure PowerShell dağıtmak için kullandığınız bir [Resource Manager şablonu](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager şablonları, çözümünüz için dağıtmanız gereken kaynakları tanımlayan JSON dosyalarıdır. Azure çözümlerinizi yönetme ve dağıtma ile ilgili kavramları anlamak için bkz: [Azure Resource Manager belgelerini](/azure/azure-resource-manager/). Daha fazla Azure Load Balancer ilgili şablon bulmak için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

Şablonu dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açın ve aşağıdaki PowerShell betiğini shell penceresine yapıştırın. Kod yapıştırmak için shell penceresine sağ tıklayın ve ardından **yapıştırın**. Azure sanal makineler için kullanılabilirlik alanı desteği bölgelerin bir listesi için bkz. [burada](../availability-zones/az-overview.md).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
$adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

Write-Host "Press [ENTER] to continue."

```

 >[!NOTE]
 >Kaynak grubu adı ile proje adınızdır **rg** eklenir. Sonraki bölümde kaynak grubu adı ihtiyacınız vardır.  Bu kaynakları oluşturmak için birkaç dakika sürer.

## <a name="test-the-load-balancer"></a>Yük Dengeleyiciyi test etme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **kaynak grupları** sol bölmeden.
1. Son bölümde oluşturduğunuz kaynak grubunu seçin.  Varsayılan kaynak grubu adı ile proje adınızdır **rg** eklenir.
1. Yük Dengeleyiciyi seçin.  Yalnızca bir yük dengeleyici yoktur. Varsayılan ad proje adıdır ile **-lb** eklenir.
1. Genel IP adresini (IP adresi bölümü yalnızca) kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği, tüm üç VM'ye dağıtmasını görmek için İstemci makinesinden web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, Yük Dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için Azure portalından Yük Dengeleyiciyi içeren kaynak grubunu seçin ve ardından **kaynak grubunu Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir standart yük dengeleyici oluşturdunuz, Vm'lere bağlı, yük dengeleyici trafik kuralını ve durum yoklaması, yapılandırılmış ve sonra da Yük Dengeleyiciyi test ettiniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
