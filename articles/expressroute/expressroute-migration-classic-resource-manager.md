---
title: "ExpressRoute ilişkili sanal ağları Klasikten Resource Manager geçirme: Azure: PowerShell | Microsoft Docs"
description: "Bu sayfayı hattınız taşıdıktan sonra ilişkili sanal ağlar için Resource Manager geçirmeyi açıklar."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 336f68308f7d4b4dd3c7476a4fabd793939e9e85
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>ExpressRoute ilişkili sanal ağları Klasikten Resource Manager geçirme

Bu makalede Azure ExpressRoute ilişkili sanal ağlar Klasik dağıtım modelinden Azure Resource Manager dağıtım modeli için expressroute bağlantı hattı taşıdıktan sonra nasıl geçirileceği açıklanmaktadır. 


## <a name="before-you-begin"></a>Başlamadan önce
* Azure PowerShell modüllerinin en son sürümüne sahip olduğunuzu doğrulayın. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
* Gözden geçirdiğinizden emin olun [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Altında sağlanan bilgileri gözden [bir expressroute bağlantı hattını Klasikten Resource Manager taşıma](expressroute-move.md). Tam olarak sınırları ve kısıtlamaları anladığınızdan emin olun.
* Bağlantı hattı Klasik dağıtım modelinde tam olarak işlevsel olduğunu doğrulayın.
* Resource Manager dağıtım modelinde oluşturulmuş bir kaynak grubu olduğundan emin olun.
* Aşağıdaki kaynak geçişi belgelerini gözden geçirin:

    * [Iaas Klasik kaynaklardan Azure Resource Manager'a Platform desteklenen geçişini](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Sık sorulan sorular: Iaas Klasik kaynaklardan Azure Resource Manager Platform desteklenen geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [En yaygın Geçiş hataları ve bunları azaltmanın yollarını gözden geçirin](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Desteklenen ve desteklenmeyen senaryolar

* Bir expressroute bağlantı hattı Resource Manager ortamına herhangi kesinti olmadan Klasikten yeniden taşınabilir. Kapalı kalma süresi olmadan Resource Manager ortamına Klasikten tüm expressroute bağlantı hattını taşıyabilirsiniz. ' Ndaki yönergeleri izleyin [ExpressRoute bağlantı hatlarını Klasikten PowerShell kullanarak Resource Manager dağıtım modeline taşıma](expressroute-howto-move-arm.md). Bu sanal ağa bağlı kaynakları taşımak için bir önkoşuldur.
* Sanal ağlar, ağ geçitleri ve sanal ağ içinde bir expressroute bağlantı hattı aynı abonelikte bağlı ilişkili dağıtımları herhangi kesinti olmadan Resource Manager ortamına geçirilebilir. Daha sonra sanal ağlar, ağ geçitleri ve sanal ağ içindeki dağıtılan sanal makineleri gibi kaynakları geçirmek için açıklanan adımları izleyebilirsiniz. Bunlar geçirilmeden önce sanal ağlar doğru şekilde yapılandırıldığından emin olmanız gerekir. 
* Sanal ağlar, ağ geçitleri ve expressroute bağlantı hattı ile aynı abonelikte değil ilişkili dağıtımlar sanal ağ içinde geçiş işlemini tamamlamak için bazı kapalı kalma süresi gerektirir. Belge son Kısım kaynaklarını geçirmek için izlenmesi gereken adımları açıklar.
* Bir sanal ağ ExpressRoute ağ geçidi ve VPN ağ geçidi ile geçirilemez.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>Bir expressroute bağlantı hattını Klasikten Resource Manager taşımak
Expressroute bağlantı hattı için bağlı kaynaklar geçirmeyi denemeden önce bir expressroute bağlantı hattı Klasikten Resource Manager ortamına taşımanız gerekir. Bu görevi gerçekleştirmek için aşağıdaki makalelere bakın:

* Altında sağlanan bilgileri gözden [bir expressroute bağlantı hattını Klasikten Resource Manager taşıma](expressroute-move.md).
* [Bir bağlantı hattı Klasikten Resource Manager'da Azure PowerShell kullanarak Taşı](expressroute-howto-move-arm.md).
* Azure Hizmet Yönetimi Portalı'nı kullanın. İş akışına izleyebilirsiniz [yeni bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) ve içe aktarma seçeneği seçin. 

Bu işlem, kapalı kalma süresi gerektirmez. Geçiş işlemi devam ederken, şirket içi ve Microsoft arasında veri aktarmak devam edebilirsiniz.

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Sanal ağlar, ağ geçitleri ve ilişkili dağıtımları geçirme

Geçirmek için izleyeceğiniz adımlar kaynaklarınız aynı abonelik, farklı Aboneliklerde veya her ikisi de olan bağlıdır.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Sanal ağlar, ağ geçitleri ve expressroute bağlantı hattı ile aynı abonelikte ilişkili dağıtımları geçirme
Bu bölüm bir sanal ağ, ağ geçidi ve expressroute bağlantı hattı ile aynı abonelikte ilişkili dağıtımları geçirmek için izlenmesi gereken adımları açıklar. Kapalı kalma süresi olmadan, bu geçiş ile ilişkilidir. Geçiş işlemi aracılığıyla tüm kaynakları kullanmaya devam edebilirsiniz. Geçiş işlemi devam ederken Yönetim düzeyi kilitlendi. 

1. Expressroute bağlantı hattı Resource Manager ortamına Klasikten taşındığından emin olun.
2. Sanal ağ geçiş için uygun şekilde hazırlandığından emin olun.
3. Aboneliğiniz kaynak geçişi için kaydolun. Aboneliğiniz için kaynak geçişi kaydetmek için aşağıdaki PowerShell parçacığını kullanın:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Doğrulama, hazırlama ve geçirilir. Sanal ağ taşımak için aşağıdaki PowerShell parçacığını kullanın:

  ```powershell
  Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
  Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
  Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
  ```

  Ayrıca aşağıdaki PowerShell cmdlet'ini çalıştırarak geçiş iptal edebilirsiniz:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

## <a name="next-steps"></a>Sonraki adımlar
* [Iaas Klasik kaynaklardan Azure Resource Manager'a Platform desteklenen geçişini](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Teknik ya ilişkin ayrıntılar platform desteklenen geçiş Klasik'ten Azure Kaynak Yöneticisi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Sık sorulan sorular: Iaas Klasik kaynaklardan Azure Resource Manager Platform desteklenen geçişi](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [En yaygın Geçiş hataları ve bunları azaltmanın yollarını gözden geçirin](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
