---
title: "Oluşturma ve bir expressroute bağlantı hattı değiştirme: PowerShell: Azure Klasik | Microsoft Docs"
description: "Bu makalede, oluşturma ve bir expressroute bağlantı hattı sağlama için adım adım anlatılmaktadır. Bu makalede ayrıca durumu, güncelleştirme veya silme denetleyin ve hattınız yetkisini kaldırma kullanmayı gösterir."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 93ddc2975db34053c6a776d1c3b931536f3f8ec7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Oluşturma ve PowerShell (Klasik) kullanarak bir expressroute bağlantı hattı değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede, PowerShell cmdlet'leri ve klasik dağıtım modeli kullanarak bir Azure expressroute bağlantı hattı oluşturmak için adım adım anlatılmaktadır. Bu makalede ayrıca durumu, güncelleştirme veya silme denetleyin ve bir expressroute bağlantı hattı yetkisini kaldırma kullanmayı gösterir.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Başlamadan önce
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a>1. Adım Önkoşulları ve iş akışı makaleleri gözden geçirin
Gözden geçirdiğinizden emin olun [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a>2. Adım Azure Hizmet Yönetimi (SM) PowerShell modülleri en son sürümlerini yükleyin
' Ndaki yönergeleri izleyin [Azure PowerShell cmdlet'leri ile çalışmaya başlama](/powershell/azure/overview) bilgisayarınızı Azure PowerShell modülleri kullanacak şekilde yapılandırma hakkında adım adım yönergeler için.

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. Adım Azure hesabınızda oturum açın ve bir abonelik seçin
1. PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

        Login-AzureRmAccount

2. Hesapla ilişkili abonelikleri kontrol edin.

        Get-AzureRmSubscription

3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

        Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_id"

4. Seçili abonelik kimliğine varsayılan olarak ayarlanmış olup olmadığını onaylayın.

        Get-AzureSubscription -default

## <a name="create-and-provision-an-expressroute-circuit"></a>Oluşturma ve bir expressroute bağlantı hattı sağlama
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a>1. Adım ExpressRoute için PowerShell modülleri alın
 Zaten yapmadıysanız, Azure ve ExpressRoute modüllerini PowerShell oturumuna ExpressRoute cmdlet'lerini kullanmaya başlamak için içeri aktarmanız gerekir. Yerel bilgisayarınızda yüklü olan bir konumdan modülleri içeri aktarın. Konumun modüllerini yüklemek için kullanılan yönteme bağlı olarak aşağıdaki örnekte gösterildiği farklı olabilir. Örnek gerekiyorsa değiştirin.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Adım Desteklenen sağlayıcılar, konumları ve bant genişlikleri listesini alma
Bir expressroute bağlantı hattı oluşturmadan önce desteklenen bağlantı sağlayıcıları, konumları ve bant seçeneklerini listesi gerekir.

PowerShell cmdlet `Get-AzureDedicatedCircuitServiceProvider` , sonraki adımlarda kullanacağınız bu bilgiler verir:

    Get-AzureDedicatedCircuitServiceProvider

Bağlantı sağlayıcınız listelenip listelenmediğini denetleyin. Bir bağlantı hattı oluşturduğunuzda, daha sonra gerekir çünkü aşağıdaki bilgileri not edin:

* Ad
* PeeringLocations
* BandwidthsOffered

Artık bir expressroute bağlantı hattı oluşturmak hazırsınız.         

### <a name="step-3-create-an-expressroute-circuit"></a>3. Adım ExpressRoute bağlantı hattı oluşturma
Aşağıdaki örnek 200 MB/sn expressroute bağlantı hattı üzerinden Equinix Silikon Vadisi'nde oluşturulacağını gösterir. Farklı bir sağlayıcı ve farklı ayarlar kullanıyorsanız, bu bilgileri isteğiniz yaptığınızda değiştirin.

> [!IMPORTANT]
> ExpressRoute bağlantı hattınız bir hizmet anahtarı verilen andan itibaren Fatura edilecek. Bağlantı sağlayıcı bağlantı hattı sağlamak hazır olduğunda bu işlemi gerçekleştirmek emin olun.
> 
> 

Yeni bir hizmet anahtarı için bir örnek isteği verilmiştir:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Veya, bir expressroute premium eklentisi ile oluşturmak istiyorsanız, aşağıdaki örneği kullanın. Başvurmak [ExpressRoute SSS](expressroute-faqs.md) premium eklentisi hakkında daha fazla ayrıntı için.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Yanıt hizmet anahtarını içerir. Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a>4. Adım. Tüm ExpressRoute bağlantı hatları listesi
Çalıştırabilirsiniz `Get-AzureDedicatedCircuit` oluşturduğunuz tüm expressroute bağlantı hatları listesini almak için komutu:

    Get-AzureDedicatedCircuit

Yanıt aşağıdaki örneğe benzer bir şey olacaktır:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Herhangi bir zamanda bu bilgileri kullanarak alabilirsiniz `Get-AzureDedicatedCircuit` cmdlet'i. Hiçbir parametre olmadan çağrıyı yapan tüm devreler listeler. Hizmet anahtarınız listelenen *ServiceKey* alan.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Adım. Hizmet anahtarı sağlamak için bağlantı sağlayıcınızı Gönder
*ServiceProviderProvisioningState* hizmet sağlayıcı tarafında sağlama geçerli durumu hakkında bilgi sağlar. *Durum* Microsoft tarafında durumunu sağlar. Bağlantı hattı durumları sağlama hakkında daha fazla bilgi için bkz: [iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states) makalesi.

Yeni bir expressroute bağlantı hattı oluşturduğunuzda, bağlantı hattı şu durumda olacaktır:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Bağlantı sağlayıcı onu sizin için etkinleştirme sürecinde olduğunda bağlantı hattı aşağıdaki durumuna gidin:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Bir expressroute bağlantı hattı, onu kullanabilmek aşağıdaki durumda olması gerekir:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Adım. Durum ve hattı anahtar durumunu düzenli aralıklarla denetleyin
Bu, sağlayıcınız hattınız etkin olduğunda bilmenizi sağlar. Bağlantı hattı yapılandırıldıktan sonra *ServiceProviderProvisioningState* olarak görüntülenir *hazırlandı* aşağıdaki örnekte gösterildiği gibi:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>7. Adım. Yönlendirme yapılandırması oluşturma
Başvurmak [expressroute bağlantı hattı yönlendirme yapılandırması (oluşturma ve hattı eşlemeler değiştirme)](expressroute-howto-routing-classic.md) makale adım adım yönergeler için.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yapılandırmak ve yönetmek için yönlendirme.
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. adım. ExpressRoute bağlantı hattına bir sanal ağı bağlama
Ardından, bir sanal ağ, expressroute bağlantı hattına bağlayın. Başvurmak [sanal ağlara bağlama ExpressRoute bağlantı hatları](expressroute-howto-linkvnet-classic.md) adım adım yönergeler için. ExpressRoute için Klasik dağıtım modeli kullanarak bir sanal ağ oluşturma gerekiyorsa bkz [ExpressRoute için bir sanal ağ oluşturma](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı durumunu alma
Herhangi bir zamanda bu bilgileri kullanarak alabilirsiniz `Get-AzureCircuit` cmdlet'i. Hiçbir parametre olmadan çağrıyı yapan tüm devreler listeler.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Çağrısına parametre olarak hizmet anahtarını geçirerek belirli bir expressroute bağlantı hattı hakkında bilgi alabilirsiniz.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Aşağıdaki örnek çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Bir expressroute bağlantı hattı değiştirme
Bağlantı etkilemeden belirli bir expressroute bağlantı hattı özelliklerini değiştirebilirsiniz.

Kapalı kalma süresi olmadan aşağıdakileri yapabilirsiniz:

* Etkinleştirmek veya devre dışı bir expressroute bağlantı hattı için ExpressRoute premium eklentisi.
* Sağlanmış kapasite kullanılabilir bağlantı noktası, expressroute bağlantı hattı bant genişliğini artırır. Bir bağlantı hattının bant genişliğini eski sürüme düşürmeyi desteklenmediğini unutmayın. 
* Ölçüm plan sınırsız veri ölçülen verilerden değiştirin. Ölçülen veri sınırsız verilerden ölçüm planı değiştirmek desteklenmez unutmayın.
* Etkinleştirme ve devre dışı *izin Klasik işlemleri*.

Başvurmak [ExpressRoute SSS](expressroute-faqs.md) sınırlar ve sınırlamalar hakkında daha fazla bilgi için.

### <a name="to-enable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi etkinleştirmek için
Aşağıdaki PowerShell cmdlet'ini kullanarak, varolan bağlantı hattınız için ExpressRoute premium eklentisi etkinleştirebilirsiniz:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Bağlantı hattınız şimdi etkin ExpressRoute premium eklentisi özellikleri sahip olur. Biz komutu başarıyla çalıştırıldı hemen premium eklenti özellik faturalama başlayacak unutmayın.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium eklentisi devre dışı bırakmak için
> [!IMPORTANT]
> Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, bu işlemi başarısız olabilir.
> 
> 

#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Standart Premium'dan düşürmek önce devresine bağlı sanal ağlar sayısı 10'dan az olduğundan emin olmalısınız. Bunu yapmazsanız, güncelleştirme isteği başarısız olur ve olması, premium oranları faturalandırılır.
* Tüm sanal ağları diğer coğrafi bölgelerde bağlantısını gerekir. Bunu yapmazsanız, güncelleştirme isteği başarısız olur ve olması, premium oranları faturalandırılır.
* Yol tablosu özel eşleme için 4. 000'den az yolları olmalıdır. Rota tablosu boyutunuz 4.000 yolları büyükse, BGP oturumu bırakacaktır ve 4.000 tanıtılan ön ek sayısı gider kadar yeniden iler hale olmaz.

#### <a name="disable-the-premium-add-on"></a>Premium eklentisi devre dışı bırak
Aşağıdaki PowerShell cmdlet'ini kullanarak var olan bağlantı hattınız için ExpressRoute premium eklentisi devre dışı bırakabilirsiniz:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute bağlantı hattı bant genişliğini güncelleştirmek için
Denetleme [ExpressRoute SSS](expressroute-faqs.md) desteklenen sağlayıcınız için bant seçenekleri. Sunucudaki fiziksel bağlantı noktası (hattınız oluşturulduğu) izin verdiği sürece, varolan bağlantı hattınız boyutundan büyük herhangi bir boyutta seçebilirsiniz.

> [!IMPORTANT]
> Varolan bir bağlantı üzerinde yetersiz kapasite ise expressroute bağlantı hattı yeniden başlatmanız gerekebilir. Varsa hiçbir ek kapasite kullanılabilir o konumda bağlantı hattı yükseltemezsiniz.
>
> Bir expressroute bağlantı hattı kesintiye uğratmadan bant indiremezsiniz. Bant genişliği eski sürüme düşürmeyi expressroute bağlantı hattı yetkisini kaldırma ve yeni bir expressroute bağlantı hattı yeniden hazırlayana gerektirir.
> 
> 

#### <a name="resize-a-circuit"></a>Bir bağlantı hattı yeniden boyutlandırma

Gereksinim boyutu karar verdikten sonra bağlantı hattınız yeniden boyutlandırmak için aşağıdaki komutu kullanabilirsiniz:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Bağlantı hattınız Microsoft tarafında boyutlandırılmış. Bu değişiklik eşleşecek şekilde kendi tarafında yapılandırmalar güncelleştirileceğini bağlantı sağlayıcınız başvurmanız gerekir. Biz üzerinde bu noktasından güncelleştirilmiş bant seçeneği için faturalama başlayacak unutmayın.

Bağlantı hattı bant genişliğini artırma olduğunda aşağıdaki hata görürseniz, bu var. Varolan hattınız oluşturulduğu fiziksel bağlantı noktası üzerinde hiçbir yeterli bant genişliği sol anlamına gelir. Bağlantı hattı silin ve yeni bir bağlantı hattı gereksinim boyutu oluşturmak zorunda. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Sağlamayı kaldırmayı ve bir expressroute bağlantı hattı silme

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Expressroute bağlantı hattı başarılı olması bu işlem için tüm sanal ağlardan bağlantısını gerekir. Bu işlem başarısız olursa, devresine bağlı sanal ağlar olup olmadığını denetleyin.
* Sağlama durumu ExpressRoute bağlantı hattı hizmet sağlayıcı ise **sağlama** veya **hazırlandı** kendi tarafında hattı yetkisini kaldırma için hizmet sağlayıcınıza birlikte çalışmalısınız. Kaynakları ayırabilir ve hizmet sağlayıcısı devre sağlama kaldırma işlemi tamamlandıktan ve bize bildiren kadar sizi faturalandırmak devam eder.
* Hizmet sağlayıcısı hattı sağlaması kaldırılıyor. sağlaması değilse (sağlama durumu hizmet sağlayıcısı kümesine **sağlanmadı**), ardından bağlantı hattı silebilirsiniz. Bu bağlantı hattı için fatura durdurur.

#### <a name="delete-a-circuit"></a>Bir bağlantı hattı Sil

Aşağıdaki komutu çalıştırarak, expressroute bağlantı hattı silebilirsiniz:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Sonraki adımlar
Bağlantı hattınız oluşturduktan sonra aşağıdakileri yaptığınızdan emin olun:

* [Oluşturma ve expressroute bağlantı hattı için yönlendirmeyi değiştirme](expressroute-howto-routing-classic.md)
* [Sanal ağ, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md)

