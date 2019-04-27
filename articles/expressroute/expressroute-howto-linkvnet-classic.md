---
title: 'Bir sanal ağı ExpressRoute devresine bağlama: PowerShell: Klasik: Azure | Microsoft Docs'
description: Bu belge, PowerShell ve klasik dağıtım modeli kullanarak ExpressRoute bağlantı hatları için sanal ağlar (Vnet'ler) bağlamak nasıl bir genel bakış sağlar.
services: expressroute
documentationcenter: na
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: cherylmc
ms.openlocfilehash: 21676ff329613f792d6570713f044bb7440e58d4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370637"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-powershell-classic"></a>PowerShell (Klasik) kullanarak bir ExpressRoute bağlantı hattına bir sanal ağı bağlama
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makalede, sanal ağlar (Vnet'ler) PowerShell kullanarak Azure ExpressRoute devreleri için bağlantı yardımcı olur. En fazla dört ExpressRoute bağlantı hatları için tek bir sanal ağa bağlanabilir. Adımlar bu makalede, bağlandığınız her ExpressRoute bağlantı hattı için yeni bir bağlantı oluşturmak için kullanın. ExpressRoute bağlantı hatları, aynı abonelik, farklı Aboneliklerde veya her ikisinin bir karışımı olabilir. Bu makale Klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar için geçerlidir.

En fazla 10 sanal ağları ExpressRoute devresine bağlayabilirsiniz. Tüm sanal ağları, aynı coğrafi bölgede olmalıdır. Çok sayıda sanal ağları ExpressRoute devreniz için veya diğer jeopolitik bölgeler ExpressRoute premium eklentisi etkinleştirirseniz olan bağlantıyı sanal ağlar bağlayabilirsiniz. Denetleme [SSS](expressroute-faqs.md) premium eklenti hakkında daha fazla ayrıntı için.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Gözden geçirme [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
   * Yönergelerini izleyin [ExpressRoute devresi oluşturma](expressroute-howto-circuit-classic.md) ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirin.
   * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md) makale için yönlendirme yönergeleri.
   * Azure özel eşdüzey hizmet sağlama yapılandırılır ve uçtan uca bağlantıyı etkinleştirmek üzere ağınız ile Microsoft arasında BGP eşliği ayarlama olduğundan emin olun.
   * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan sanal ağ geçidi olması gerekir. Yönergelerini izleyin [ExpressRoute için sanal ağ yapılandırma](expressroute-howto-vnet-portal-classic.md).

### <a name="download-the-latest-powershell-cmdlets"></a>En son PowerShell cmdlet'lerini indirin

Azure Hizmet Yönetimi (SM) PowerShell modüllerine ve ExpressRoute modülünün en son sürümlerini yükleyin. Aşağıdaki örnek kullanırken, cmdlet'leri daha yeni sürümleri çıktıkça sürüm numarasını (Bu örnekte, 5.1.1) değişeceğini unutmayın.

```powershell
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\Azure\Azure.psd1'
Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.1.1\ExpressRoute\ExpressRoute.psd1'
```

Azure PowerShell hakkında daha fazla bilgiye ihtiyacınız varsa bkz [Azure PowerShell cmdlet'lerini kullanmaya Başlarken](/powershell/azure/overview) bilgisayarınızın Azure PowerShell modüllerinin kullanacak şekilde yapılandırma hakkında adım adım yönergeler için.

### <a name="sign-in"></a>Oturum aç

Azure hesabınızda oturum açmak için aşağıdaki örnekleri kullanın:

1. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın.

   ```powershell
   Connect-AzAccount
   ```
2. Hesapla ilişkili abonelikleri kontrol edin.

   ```powershell
   Get-AzSubscription
   ```
3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```powershell
   Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
   ```

4. Ardından, Azure aboneliğiniz için PowerShell Klasik dağıtım modeli için eklemek için aşağıdaki cmdlet'i kullanın.

   ```powershell
   Add-AzureAccount
   ```

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Bir sanal ağ ile aynı abonelikte devreye bağlama
Aşağıdaki cmdlet'i kullanarak bir ExpressRoute bağlantı hattına bir sanal ağa bağlayabilirsiniz. Sanal ağ geçidi oluşturulur ve cmdlet çalıştırılmadan önce bağlamak için hazır olduğundan emin olun.

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
Provisioned
```
    
## <a name="remove-a-virtual-network-link-to-a-circuit"></a>Bir bağlantı hattına bir sanal ağ bağlantısını Kaldır
Aşağıdaki cmdlet'i kullanarak ExpressRoute bağlantı hattına bir sanal ağ bağlantısını kaldırabilirsiniz. Geçerli abonelik belirli bir sanal ağ için seçili olduğundan emin olun. 

```powershell
Remove-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```
 

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir ExpressRoute bağlantı hattı birden çok farklı abonelikler arasında paylaşabilirsiniz. Aşağıdaki şekilde birden fazla aboneliği analiz basit bir ExpressRoute bağlantı hatları için nasıl paylaşım Works şematik gösterir.

Her küçük bulutların büyük bulut içinde bir kuruluştaki farklı departmanlara ait abonelikleri temsil etmek için kullanılır. --Hizmetlerini ancak Departmanlar dağıtma, şirket içi ağınıza bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşmak için her kuruluşun anomaly kendi aboneliğini kullanabilirsiniz. Tek bir bölüm (Bu örnekte: BT) ExpressRoute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler, ExpressRoute bağlantı hattı kullanabilirsiniz.

> [!NOTE]
> ExpressRoute bağlantı hattı sahibinden için adanmış bir bağlantı hattı için bağlantı ve bant genişliği ücretleri uygulanır. Tüm sanal ağları, aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Yönetim
*Bağlantı hattı sahibinden* olan yönetici/Abonelikteki ExpressRoute bağlantı hattı oluşturulur. Bağlantı hattı sahibinden Yöneticiler/diğer yöneticiler denir, diğer abonelikler yetkilendirebilirsiniz *devre kullanıcıları*oldukları adanmış bir bağlantı hattı kullanmak için. Yetkileri sonra kuruluşun ExpressRoute bağlantı hattı kullanmaya yetkili olduğundan devre kullanıcıları aboneliklerinde sanal ağı ExpressRoute bağlantı hattına bağlayabilirsiniz.

Bağlantı hattı sahibinden yetkilendirme dilediğiniz zaman iptal et ve değiştirmek için gücüne sahiptir. Bir yetkilendirme iptal erişimini iptal edildi abonelikten silinen tüm bağlantıları neden olur.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bir yetkilendirme oluşturma**

Bağlantı hattı sahibinden belirtilen bağlantı hattı kullanılacak diğer abonelikleri yöneticileri yetkisi verir. Aşağıdaki örnekte, bağlantı hattının (Contoso BT) Yöneticisi (en fazla iki sanal ağı devreye bağlamak için geliştirme-Test) başka bir abonelik yöneticisine sağlar. Contoso BT yöneticisi bu geliştirme ve Test Microsoft kimliği belirtilerek sağlar. Cmdlet'i belirtilen Microsoft kimliği için e-posta göndermez Bağlantı hattı sahibinden açıkça bir abonelik sahibi yetkilendirme tamamlandığını bildirmek gerekir.

```powershell
New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'
```

  Döndür:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 2
  LinkAuthorizationId : **********************************
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Yetkilendirmeleri gözden geçirme**

Bağlantı hattı sahibinden belirli bir bağlantı hattı üzerinde aşağıdaki cmdlet'i çalıştırarak düzenlenen tüm yetkilendirmeleri gözden geçirebilirsiniz:

```powershell
Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"
```
  Döndür:

  ```powershell
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
  ```

**Yetkilendirmeleri güncelleştiriliyor**

Bağlantı hattı sahibinden yetkilendirme, aşağıdaki cmdlet'i kullanarak değiştirebilirsiniz:

```powershell
Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5
```

  Döndür:

  ```powershell
  Description         : Dev-Test Links
  Limit               : 5
  LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  MicrosoftIds        : devtest@contoso.com
  Used                : 0
  ```

**Yetkilendirmeleri siliniyor**

Bağlantı hattı sahibinden iptal etme/yetkilendirmeleri kullanıcı için aşağıdaki cmdlet'i çalıştırarak ya da silebilir:

```powershell
Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"
```

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

**Yetkilendirmeleri gözden geçirme**

Bağlantı hattı kullanıcısı, aşağıdaki cmdlet'i kullanarak yetkilendirmeleri gözden geçirebilirsiniz:

```powershell
Get-AzureAuthorizedDedicatedCircuit
```

  Döndür:

  ```powershell
  Bandwidth                        : 200
  CircuitName                      : ContosoIT
  Location                         : Washington DC
  MaximumAllowedLinks              : 2
  ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
  ServiceProviderName              : equinix
  ServiceProviderProvisioningState : Provisioned
  Status                           : Enabled
  UsedLinks                        : 0
  ```

**Bağlantı yetkilerini kuponumu kullanmakta**

Bağlantı hattı kullanıcısı bağlantı yetkilendirme kullanmak için aşağıdaki cmdlet'i çalıştırabilirsiniz:

```powershell
New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'
```

  Döndür:

  ```powershell
  State VnetName
  ----- --------
  Provisioned SalesVNET1
  ```

Yeni bağlantılı Abonelikteki sanal ağ için şu komutu çalıştırın:

```powershell
New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
