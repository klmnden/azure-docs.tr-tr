---
title: "Bir sanal ağ için bir expressroute bağlantı: PowerShell: Klasik: Azure | Microsoft Docs"
description: "Bu belge, Klasik dağıtım modeli ve PowerShell kullanarak, ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlamak nasıl bir genel bakış sağlar."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 8df8a4c6ff0897c821e13248e0494b17e1a4992d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>Bir sanal ağ (Klasik) PowerShell kullanarak bir expressroute bağlantı
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makale Klasik dağıtım modeli ve PowerShell kullanarak Azure ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlantı yardımcı olur. Sanal ağlar aynı abonelikte ya da olabilir veya başka bir abonelik parçası olabilir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları
1. Azure PowerShell modüllerinin en son sürümünü gerekir. En son PowerShell modülleri PowerShell bölümünden indirebilirsiniz [Azure indirmeler sayfası](https://azure.microsoft.com/downloads/). ' Ndaki yönergeleri izleyin [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) bilgisayarınızı Azure PowerShell modülleri kullanacak şekilde yapılandırma hakkında adım adım yönergeler için.
2. Gözden geçirmeniz gereken [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
3. Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
   * Yönergelerini izleyin [bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md) ve bağlantı sağlayıcınız bağlantı hattı etkinleştirin.
   * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md) yönlendirme yönergeleri için makalenin.
   * Azure özel eşleme yapılandırılır ve uçtan uca bağlantı etkinleştirebilmeniz için ağınız ve Microsoft arasında BGP eşliği yukarı olduğundan emin olun.
   * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan bir sanal ağ geçidi olmalıdır. Yönergeleri izleyerek [sanal ağ ExpressRoute için yapılandırma](expressroute-howto-vnet-portal-classic.md).

Bir expressroute bağlantı hattı için en fazla 10 sanal ağlara bağlantı oluşturabilirsiniz. Tüm sanal ağları aynı coğrafi bölgede olması gerekir. Çok sayıda expressroute bağlantı hattına sanal ağları veya ExpressRoute premium eklentisi etkinse diğer coğrafi bölgelerde bağlantı sanal ağlar bağlayabilirsiniz. Denetleme [SSS](expressroute-faqs.md) premium eklentisi hakkında daha fazla ayrıntı için.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Bir sanal ağ ile aynı abonelikte bir devreye bağlanmak
Aşağıdaki cmdlet'i kullanarak bir expressroute bağlantı hattı için bir sanal ağa bağlayabilirsiniz. Sanal ağ geçidi oluşturulur ve cmdlet'ini çalıştırmadan önce bağlama için hazır olduğundan emin olun.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir expressroute bağlantı hattı birden çok abonelik paylaşabilirsiniz. Aşağıdaki şekilde arasında birden fazla abonelik basit bir ExpressRoute bağlantı hatları için nasıl paylaşım works'ün şematik gösterilmektedir.

Her büyük bulut içinde daha küçük bulut, kuruluş içindeki farklı departmanlara ait abonelikleri temsil etmek için kullanılır. --Hizmetlerini ancak Departmanlar dağıtma, şirket içi ağınıza bağlanmak için tek bir expressroute bağlantı hattı paylaşmak için her kuruluş içinde bölümlerin kendi aboneliği kullanabilirsiniz. Tek bir bölüm (Bu örnekte: BT) expressroute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler expressroute bağlantı hattı kullanabilirsiniz.

> [!NOTE]
> Bağlantı ve bant genişliği ücretleri ayrılmış bağlantı hattı için ExpressRoute bağlantı hattı sahibine uygulanır. Tüm sanal ağları aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Yönetim
*Devre sahibinden* olan yönetici/Abonelikteki expressroute bağlantı hattı oluşturulur. Devre sahibinden Yöneticiler/coadministrators olarak adlandırılan diğer abonelikler yetkilendirebilir *hattı kullanıcılar*, sahip oldukları adanmış devre kullanın. Yetkileri sonra kuruluşun expressroute bağlantı hattı kullanmak için yetkilendirilmesini hattı kullanıcılar kendi Abonelikteki sanal ağ expressroute bağlantı hattı bağlayabilirsiniz.

Devre sahibinden yetkilerini herhangi bir zamanda iptal etme ve değiştirmek için power sahiptir. Bir yetkilendirme iptal erişimini iptal edildi abonelikten silinen tüm bağlantılar neden olur.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bir yetkilendirme oluşturma**

Devre sahibinden diğer abonelikler yöneticilerinin belirtilen bağlantı hattı kullanmasını yetkilendirir. Aşağıdaki örnekte bağlantı hattının (Contoso BT) Yöneticisi yöneticinin başka bir abonelik (en fazla iki sanal ağlara bağlantı hattına geliştirme-Test) sağlar. Contoso BT yöneticisi bu geliştirme, Test Microsoft kimliği belirtilerek sağlar. Cmdlet'i belirtilen Microsoft kimliği e-posta göndermez Devre sahibinden yetkilendirme tamamlandıktan bir abonelik sahibi açıkça bildirmesi gerekir.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Yetkilerini gözden geçirme**

Devre sahibinden aşağıdaki cmdlet'i çalıştırarak belirli bir bağlantı hattı üzerinde verilen tüm yetkilerini gözden geçirebilirsiniz:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Yetkilerini güncelleştiriliyor**

Devre sahibinden yetkilerini aşağıdaki cmdlet'i kullanarak değiştirebilirsiniz:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Yetkilerini silme**

Devre sahibinden revoke/yetkilerini kullanıcı için aşağıdaki cmdlet'i çalıştırarak silme:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

**Yetkilerini gözden geçirme**

Bağlantı hattı kullanıcı, aşağıdaki cmdlet'i kullanarak yetkilerini gözden geçirebilirsiniz:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Bağlantı yetkilerini itibaren**

Bağlantı hattı kullanıcı bağlantısı yetkilendirme kullanmak için aşağıdaki cmdlet'i çalıştırabilirsiniz:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Yeni bağlantılı abonelik sanal ağ için bu komutu çalıştırın:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).

