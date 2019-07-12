---
title: 'Bir ExpressRoute bağlantı hattı - Resource Manager şablonu oluşturma: Azure | Microsoft Docs'
description: Oluşturma, sağlama, silme ve bir ExpressRoute bağlantı hattının sağlamasını Kaldır.
services: expressroute;azure-resource-manager
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 07/05/2019
ms.author: cherylmc;ganesr
ms.openlocfilehash: bf56145d0a8cd3b01d0d74fcaf3348c1916cee5a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659690"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak ExpressRoute devresi oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Azure Resource Manager şablonu](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Azure PowerShell kullanarak bir Azure Resource Manager şablonu dağıtarak bir ExpressRoute bağlantı hattı oluşturmayı öğrenin. Resource Manager şablonları geliştirme hakkında daha fazla bilgi için bkz. [Resource Manager belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.network/expressroutecircuits).

## <a name="before-you-begin"></a>Başlamadan önce

* Gözden geçirme [önkoşulları](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Yeni ağ kaynakları oluşturma izni olduğundan emin olun. Doğru izinlere sahip değilsiniz, hesap yöneticinize başvurun.
* Yapabilecekleriniz [video görüntüleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) adımları daha iyi anlamak için başlamadan önce.

## <a name="create"></a>Oluşturma ve bir ExpressRoute bağlantı hattı sağlama

[Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/) Resource Manager şablonu iyi koleksiyonu vardır. Birini [var olan şablonları](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) ExpressRoute devresi oluşturma.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/101-expressroute-circuit-create/azuredeploy.json)]

İlgili daha fazla şablonları görmek için seçin [burada](https://azure.microsoft.com/resources/templates/?term=expressroute).

Bir şablonu dağıtarak bir ExpressRoute bağlantı hattı oluşturmak için:

1. Seçin **deneyin** gelen aşağıdaki kod bloğunu ve ardından Azure bulut kabuğunda oturum açmak için yönergeleri izleyin.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * **Katman** ExpressRoute standart ya da ExpressRoute premium eklenti etkin olup olmadığını belirler. Belirtebileceğiniz **standart** standart SKU'nun almak veya **Premium** premium eklenti için.

   * **Eşdüzey hizmet sağlama konumu** nerede sizin eşlemeyi Microsoft ile fiziksel konumu.

     > [!IMPORTANT]
     > Eşleme konumu gösteren [fiziksel konum](expressroute-locations.md) nerede sizin eşlemeyi Microsoft ile. Bu **değil** başvuran Azure ağ kaynak sağlayıcısı bulunduğu coğrafi konum "Location" özelliğine bağlı. Bunlar ilişkili değildir, ancak bir ağ kaynağı sağlayıcı eşleme konumu bağlantı hattının coğrafi olarak yakın seçmek için iyi bir uygulamadır.

    Service bus ad alanı adı ile kaynak grubu adı olan **rg** eklenir.

2. Seçin **kopyalama** PowerShell betiğini kopyalanacak.
3. Kabuk konsolun sağ tıklayın ve ardından **Yapıştır**.

Bir olay hub'ı oluşturmak için birkaç dakika sürer.

Azure PowerShell, bu öğreticide şablonu dağıtmak için kullanılır. Diğer şablon dağıtım yöntemleri için bkz:

* [Azure portalını kullanarak](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Azure CLI kullanarak](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [REST API'yi kullanarak](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="delete"></a>Sağlama kaldırmayı ve bir ExpressRoute bağlantı hattı siliniyor

ExpressRoute devreniz seçerek silebilirsiniz **Sil** simgesi. Aşağıdaki bilgileri not edin:

* ExpressRoute bağlantı hattınızdaki tüm sanal ağların bağlantısını kaldırmanız gerekir. Bu işlem başarısız olursa, herhangi bir sanal ağa bağlantı hattına bağlı olup olmadığını denetleyin.
* ExpressRoute bağlantı hattı Hizmet Sağlayıcısı sağlama durumu ise **sağlama** veya **sağlanan** kendi tarafında bağlantı hattını sağlamasını kaldırmak için hizmet sağlayıcınızla birlikte çalışmanız gerekir. Kaynak ayırmanıza ve hizmeti sağlayıcısı devreyi sağlamayı kaldırma tamamlandıktan ve bize bildiren kadar faturalandırılırsınız devam ediyoruz.
* Hizmet sağlayıcısı devreyi sağlamayı durdurduğunda varsa (Hizmet Sağlayıcısı sağlama durumu ayarlamak **sağlanmadı**), bağlantı hattının silebilirsiniz. Bu durumda bağlantı hattının faturalandırılması durdurulur.

Aşağıdaki PowerShell komutunu çalıştırarak, ExpressRoute devreniz silebilirsiniz:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı hattınızı oluşturduktan sonra sonraki adımlara devam edin:

* [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağınız, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)
