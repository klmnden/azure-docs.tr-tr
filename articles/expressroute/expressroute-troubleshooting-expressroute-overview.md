---
title: 'Sorun giderme kılavuzu ExpressRoute bağlantısı - doğrulayın: Azure | Microsoft Docs'
description: Bu sayfa, sorun giderme ve ExpressRoute bağlantı hattının uçtan uca bağlantıyı doğrulama yönergelerini sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 09/26/2017
ms.author: rambala
ms.custom: seodec18
ms.openlocfilehash: 888f4dedf2fda0f54297d42a5f813abf73ded748
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66117855"
---
# <a name="verifying-expressroute-connectivity"></a>ExpressRoute bağlantısını doğrulama
Bu makalede doğrulayın ve ExpressRoute bağlantınızın gidermenize yardımcı olur. Bir şirket içi ağ, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden Microsoft bulutuna genişleten, ExpressRoute aşağıdaki üç ayrı ağ alanları içerir:

-   Müşteri Ağı
-   Sağlayıcı ağı
-   Microsoft Veri merkezinde

Bu belgenin amacı, nereye tanımlamak için kullanıcı yardımcı olmaktır (veya bir olsa bile) bir bağlantı sorunu var ve böylece bu sorunu gidermek için uygun ekibinden Yardım arama için hangi bölge içinde. Bir sorunu çözmek için Microsoft destek gerekirse, bir destek bileti açın [Microsoft Support][Support].

> [!IMPORTANT]
> Bu belge, basit sorun tanılanıp yardımcı olmak içindir. Microsoft destek için bir değişiklik olacak şekilde tasarlanmamıştır. Bir destek bileti açın [Microsoft Support] [ Support] sağlanan yönergeleri kullanarak sorunu çözmeyi erişemiyorsanız.
>
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagramda, mantıksal bir müşteri ağı ExpressRoute kullanarak Microsoft ağına bağlantısı gösterir.
[![1]][1]

Yukarıdaki diyagramda, numaralar anahtar ağ noktalarını gösterir. Ağ noktalarını, genellikle bu makalede ilişkili numaralarına tarafından başvurulur.

ExpressRoute bağlantı modeli (bulut Exchange birlikte bulundurma, noktadan noktaya Ethernet bağlantısı veya herhangi bir ağdan herhangi bir (IPVPN)) bağlı olarak 3 ve 4 ağ noktalarını, anahtarları (Katman 2 cihazlar) olabilir. Gösterilen anahtar ağ noktaları aşağıdaki gibidir:

1.  Müşteri hesaplama cihazı (örneğin, bir sunucu veya PC)
2.  CEs: Müşteri sınır yönlendiricileri 
3.  PEs (CE'e yönelik): Müşteri sınır yönlendiricileri karşılıklı sağlayıcısı edge yönlendiriciler/anahtarlar. İçin bu belgede PE CEs adlandırılır.
4.  PEs (MSEE'e yönelik): Msee karşılıklı sağlayıcısı edge yönlendiriciler/anahtarlar. İçin bu belgede PE Msee adlandırılır.
5.  Msee: Microsoft Enterprise Edge (MSEE) ExpressRoute yönlendiricileri
6.  Sanal ağ (VNet) ağ geçidi
7.  Azure sanal ağı cihazda işlem

Bulut Exchange birlikte bulundurma veya noktadan noktaya Ethernet bağlantısı bağlantı modelleri kullanılıyorsa, (2) müşteri uç yönlendiricisinde BGP eşlemesi Msee (5) ile oluşturmanız. Ağ noktalarını 3 ve 4 hala mevcut ancak katman 2 cihazlar olarak biraz saydam.

Herhangi bir ağdan herhangi bir (IPVPN) bağlantı modeli kullanılıyorsa, (MSEE'e yönelik) PEs (4) BGP eşlemesi Msee (5) ile oluşturmak. Yolları tekrar IPVPN hizmet sağlayıcısı ağ aracılığıyla müşteri ağa yayar.

> [!NOTE]
>ExpressRoute için yüksek kullanılabilirlik, bir çift yedekli BGP oturumları Msee'ler (5) ile PE-Msee (4) arasında Microsoft gerektirir. Bir çift yedekli ağ yollarını da PE CEs müşteri ağı arasında teşvik edilir. Ancak, herhangi bir ağdan herhangi bir (IPVPN) bağlantı modelinde, bir veya daha fazla PEs (3) için tek bir CE cihaz (2) bağlanabilir.
>
>

Bir ExpressRoute bağlantı hattını doğrulamak için aşağıdaki adımları (noktasıyla ilişkili sayıyla ağ) ele alınmaktadır:
1. [Bağlantı hattı sağlama ve durumu (5) doğrulama](#validate-circuit-provisioning-and-state)
2. [En az bir ExpressRoute doğrulamak (5) yapılandırılmış olan eşleme](#validate-peering-configuration)
3. [Microsoft ile hizmet arasında ARP doğrulama sağlayıcısı (4 ve 5 arasında bağlantı)](#validate-arp-between-microsoft-and-the-service-provider)
4. [BGP ve (4-5 ile 5-bir sanal ağa bağlı olup olmadığını 6 arasındaki BGP) MSEE yollara doğrula](#validate-bgp-and-routes-on-the-msee)
5. [Onay akışı istatistiklerini (trafik 5'ten geçirme)](#check-the-traffic-statistics)

Daha fazla doğrulamaları ve denetimleri geri gelecek iade aylık eklenecek!

## <a name="validate-circuit-provisioning-and-state"></a>Bağlantı hattı sağlama ve durumunu doğrulama
Bağlantı modeli bağımsız olarak bir ExpressRoute bağlantı hattı oluşturulması gerekir ve bu nedenle bağlantı hattı sağlama için bir hizmet anahtarı oluşturulur. Bir ExpressRoute bağlantı hattı sağlama Msee (5) PE-Msee (4) arasındaki yedekli bir katman 2 bağlantıları oluşturur. Oluşturma, değiştirme, sağlamak ve bir ExpressRoute bağlantı hattı doğrulama konusunda daha fazla bilgi için bkz [oluşturun ve bir ExpressRoute bağlantı hattını değiştirme][CreateCircuit].

>[!TIP]
>Bir hizmet anahtarı, bir ExpressRoute bağlantı hattı benzersiz olarak tanımlar. Bu belgede belirtilen powershell komutların çoğu için bu anahtar gereklidir. Ayrıca, Yardım, Microsoft'tan veya bir ExpressRoute iş ortağı, ExpressRoute sorun gidermek için bağlantı hattını kolayca tanımlamak için hizmet anahtarınızı sağlamanız gerekir.
>
>

### <a name="verification-via-the-azure-portal"></a>Azure portal aracılığıyla doğrulama
Azure portalında bir ExpressRoute bağlantı hattı durumu seçerek denetlenebilir ![2][2] sol kenar çubuğu menüsünü ve sonra ExpressRoute bağlantı hattı seçerek. Bir ExpressRoute seçme "Tüm kaynaklar" altında listelenen bağlantı hattı ExpressRoute bağlantı hattı dikey penceresi açılır. İçinde ![3][3] dikey penceresinde, aşağıdaki ekran görüntüsünde gösterildiği gibi essentials listelenen ExpressRoute bölümünü:

![4][4]    

ExpressRoute essentials'ta *devre durumu* Microsoft tarafında bağlantı hattının durumunu gösterir. *Sağlayıcı durumu* devre uygulanmadığını gösteren *sağlanan/değil sağlanan* hizmet sağlayıcı tarafında. 

İşletimsel, olması bir ExpressRoute bağlantı hattı için *devre durumu* olmalıdır *etkin* ve *sağlayıcısı durumu* olmalıdır *sağlanan*.

> [!NOTE]
> Varsa *devre durumu* olan etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *sağlayıcısı durumu* olan sağlanmadı, hizmet sağlayıcınıza başvurun.
>
>

### <a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Bir kaynak grubundaki tüm ExpressRoute devreleri listelemek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Azure kaynak grubu adınız alabilirsiniz. Bu belgenin önceki alt bölümüne bakın ve örnek ekran görüntüsünde kaynak grubu adı listelendiğine dikkat edin.
>
>

Bir kaynak grubunda belirli bir ExpressRoute bağlantı hattı seçmek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Örnek yanıt şöyledir:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

Bir ExpressRoute bağlantı hattı işletimsel olup olmadığını onaylamak için özellikle aşağıdaki alanlara dikkat edin:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

> [!NOTE]
> Varsa *CircuitProvisioningState* olan etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *ServiceProviderProvisioningState* olan sağlanmadı, hizmet sağlayıcınıza başvurun.
>
>

### <a name="verification-via-powershell-classic"></a>PowerShell (Klasik) aracılığıyla doğrulama
Bir abonelik altındaki tüm ExpressRoute devreleri listelemek için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuit

Belirli bir ExpressRoute bağlantı hattı seçmek için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

Örnek yanıt şöyledir:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Bir ExpressRoute bağlantı hattı işletimsel olup olmadığını onaylamak için özellikle aşağıdaki alanlara dikkat edin: ServiceProviderProvisioningState: Sağlanan durumu: Enabled

> [!NOTE]
> Varsa *durumu* olan etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *ServiceProviderProvisioningState* olan sağlanmadı, hizmet sağlayıcınıza başvurun.
>
>

## <a name="validate-peering-configuration"></a>Eşleme yapılandırmasını doğrula
Hizmet sağlayıcısı, ExpressRoute bağlantı hattı Sağlama tamamlandıktan sonra ExpressRoute bağlantı hattı arasında MSEE çekme isteklerini (4) ve Msee (5) üzerinden yönlendirme yapılandırması oluşturulabilir. Her ExpressRoute bağlantı hattı, bir, iki veya üç yönlendirme bağlamı etkin olabilir: Azure özel eşleme (trafiği Azure özel sanal ağlar için), Azure ortak eşleme (trafik azure'da genel IP adresleri için) ve Microsoft eşleme (trafiği Office 365 ve Dynamics 365). Yönlendirme yapılandırması oluşturma ve değiştirme konusunda daha fazla bilgi için bkz [oluşturun ve bir ExpressRoute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].

### <a name="verification-via-the-azure-portal"></a>Azure portal aracılığıyla doğrulama

> [!NOTE]
> Katman 3 hizmet sağlayıcısı tarafından sağlanır ve eşlemeleri, Portal'da boş, portalı Yenile düğmesini kullanarak bağlantı hattı yapılandırma yenileyin. Bu işlem hattınız üzerinde doğru yönlendirme yapılandırması uygulanır. 
>
>

Azure portalında bir ExpressRoute bağlantı hattının durumunu seçilerek denetlenebilir ![2][2] sol kenar çubuğu menüsünü ve sonra ExpressRoute bağlantı hattı seçerek. Bir ExpressRoute seçme "Tüm kaynaklar" altında listelenen bağlantı hattı ExpressRoute bağlantı hattı dikey penceresi açılır. İçinde ![3][3] dikey penceresinde, aşağıdaki ekran görüntüsünde gösterildiği gibi temel bileşenler'in listelenmesi ExpressRoute bölümünü:

![5][5]

Azure genel ve Microsoft eşleme yönlendirme bağlamları etkin ancak önceki örnekte belirtilen Azure özel eşleme yönlendirme bağlam, etkin. Başarılı bir şekilde etkinleştirilmiş bir eşleme içeriği listelenen (için BGP gereklidir), birincil ve ikincil noktadan noktaya alt ağlar da gerekir. / 30 alt ağ arabirimi IP adresini ve Msee PE Msee için kullanılır. 

> [!NOTE]
> Bir eşleme etkin değilse, atanan birincil ve ikincil alt ağları PE Msee yapılandırmasına eşleşip eşleşmediğini denetleyin. Aksi takdirde, MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturma ve bir ExpressRoute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering]
>
>

### <a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Azure özel eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Bir başarıyla yapılandırıldı özel eşleme, bir örnek, yanıttır:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Başarılı bir şekilde etkinleştirilmiş bir eşleme bağlamı, listelenen birincil ve ikincil adres ön eklerini gerekir. / 30 alt ağ arabirimi IP adresini ve Msee PE Msee için kullanılır.

Azure ortak eşleme yapılandırmasını ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt

Microsoft eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
     Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Bir eşleme yapılandırılmamışsa bir hata iletisi olacaktır. Belirtilen eşleme (Azure Bu örnekte eşlemesi genel) devresi içinde yapılandırılmadığı zaman bir örnek yanıt:

    Get-AzExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


> [!NOTE]
> Bir eşleme etkin değilse, atanan birincil ve ikincil alt ağları yapılandırmasına bağlı PE MSEE eşleşip eşleşmediğini denetleyin. Ayrıca, kontrol doğru *Vlanıd*, *AzureASN*, ve *PeerASN* Msee üzerinde kullanılır ve bu değerleri eşleniyorsa bağlı PE MSEE üzerinde kullanılan olanlara. MD5 karma seçilirse, paylaşılan anahtarı MSEE ve PE MSEE çiftine aynı olmalıdır. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturun ve bir ExpressRoute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>PowerShell (Klasik) aracılığıyla doğrulama
Azure özel eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutu kullanın:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Bir başarıyla yapılandırıldı özel eşleme için bir örnek, yanıttır:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

Başarılı bir şekilde, bir etkin eşleme bağlam listelenen birincil ve ikincil eş alt ağlara sahip. / 30 alt ağ arabirimi IP adresini ve Msee PE Msee için kullanılır.

Azure ortak eşleme yapılandırmasını ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

Microsoft eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

> [!IMPORTANT]
> ExpressRoute eşlemeleri portal veya PowerShell aracılığıyla ayarlama Katman 3 eşlemeler hizmet sağlayıcısı tarafından ayarlandıysa, hizmet sağlayıcı ayarlarını üzerine yazar. Sağlayıcı yan eşleme ayarlarını sıfırlama hizmeti sağlayıcı desteği gerektirir. Hizmet sağlayıcısı yalnızca Katman 2 Hizmetleri sağlanacağıdır belirli ise yalnızca ExpressRoute eşlemeleri Değiştir!
>
>

> [!NOTE]
> Bir eşleme etkin değilse, atanan birincil ve ikincil eş alt ağları yapılandırmasına bağlı PE MSEE eşleşip eşleşmediğini denetleyin. Ayrıca, kontrol doğru *Vlanıd*, *AzureAsn*, ve *PeerAsn* Msee üzerinde kullanılır ve bu değerleri eşleniyorsa bağlı PE MSEE üzerinde kullanılan olanlara. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturun ve bir ExpressRoute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a>Hizmet sağlayıcısı ile Microsoft arasındaki ARP doğrula
Bu bölümde PowerShell (Klasik) komutları kullanır. Azure Resource Manager PowerShell komutlarını kullandıysanız, abonelik yönetici/ortak yönetici erişimi olduğundan emin olun. Azure Resource Manager kullanarak sorun giderme için komutları edinmek [Resource Manager dağıtım modelinde ARP alma tabloları] [ ARP] belge.

> [!NOTE]
>ARP almak için Azure portalı ve Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutları ile bir hatayla karşılaşılmazsa Klasik PowerShell komutlarını Klasik PowerShell komutları, Azure Resource Manager ExpressRoute bağlantı hatları ile de çalışır olarak çalışması gerekir.
>
>

Özel eşleme için birincil MSEE'nin yönlendiriciden ARP tablosu almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Örnek yanıt başarılı senaryosu, komut için:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Benzer şekilde, içinde MSEE ARP tablosundan denetleyebilirsiniz *birincil*/*ikincil* yolu için *özel*/*genel*  / *Microsoft* eşlemeleri.

Aşağıdaki örnek, mevcut bir eşdüzey hizmet sağlama için komut yanıtı gösterir.

    ARP Info:
       
> [!NOTE]
> ARP tablosu MAC adresleriyle eşlenen arabirimlerinin IP adresleri yoksa, aşağıdaki bilgileri gözden geçirin:
>1. İlk IP adresini/30 alt MSEE çekme isteği MSEE arasındaki bağlantı MSEE PR'yi arabirimde kullanılır atanan Azure, her zaman Msee için ikinci IP adresini kullanır.
>2. Hizmet (S-Tag) VLAN etiketlerini ve müşteri (C-Tag) hem de MSEE çekme isteği ve MSEE çifti eşleşip eşleşmediğini denetleyin.
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a>BGP ve MSEE yollara doğrula
Bu bölümde PowerShell (Klasik) komutları kullanır. Azure Resource Manager PowerShell komutlarını kullandıysanız, abonelik yönetici/ortak yönetici erişimi olduğundan emin olun.

> [!NOTE]
>BGP bilgi almak için Azure portalı ve Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutları ile bir hatayla karşılaşılmazsa Klasik PowerShell komutlarını Klasik PowerShell komutları, Azure Resource Manager ExpressRoute bağlantı hatları ile de çalışır olarak çalışması gerekir.
>
>

Yönlendirme tablosu (BGP komşu) için belirli bir yönlendirme bağlam özeti almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Bir yanıt örneği verilmiştir:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Yukarıdaki örnekte gösterildiği gibi komut yönlendirme ne kadar bağlamı kuruldu belirlemek yararlıdır. Ayrıca, eşleme yönlendirici tarafından tanıtılan yol ön ek sayısı gösterir.

> [!NOTE]
> Durumu etkin veya boş ise, atanan birincil ve ikincil eş alt ağları yapılandırmasına bağlı PE MSEE eşleşip eşleşmediğini denetleyin. Ayrıca, kontrol doğru *Vlanıd*, *AzureAsn*, ve *PeerAsn* Msee üzerinde kullanılır ve bu değerleri eşleniyorsa bağlı PE MSEE üzerinde kullanılan olanlara. MD5 karma seçilirse, paylaşılan anahtarı MSEE ve PE MSEE çiftine aynı olmalıdır. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturun ve bir ExpressRoute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].
>
>

> [!NOTE]
> Belirli hedefleri belirli eşleme üzerinden ulaşılamıyor belirli eşleme bağlamına ait Msee'ler yol tablosuna bakın. Yönlendirme tablosunda (NATed IP olabilir) eşleşen bir önek varsa, yolda bir güvenlik duvarı/NSG/ACL varsa ve bunlar trafiğe denetleyin.
>
>

Tam bir yönlendirme tablosu MSEE almanıza imkan *birincil* belirli yolu *özel* bağlam yönlendirme, aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Komut için bir örnek başarılı sonuç elde edilir:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Benzer şekilde, içinde MSEE yönlendirme tablosundan denetleyebilirsiniz *birincil*/*ikincil* yolu için *özel* /  *Genel*/*Microsoft* eşleme bağlamı.

Aşağıdaki örnek, mevcut bir eşdüzey hizmet sağlama için komut yanıtı gösterir:

    Route Table Info:

## <a name="check-the-traffic-statistics"></a>Onay trafiği istatistikleri
Birleşik birincil ve ikincil yolu trafiğini istatistikleri--bayt ve bir kapatma--bir eşleme içeriğini almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Komutun bir örnek çıktı.

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Komut eşleme var olmayan bir örnek çıktısı şöyledir:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi veya Yardım için aşağıdaki bağlantıları kontrol edin:

- [Microsoft desteği][Support]
- [ExpressRoute devre oluşturma ve değiştirme][CreateCircuit]
- [ExpressRoute devresi için yönlendirme oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "mantıksal Expressroute bağlantısı"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Tüm kaynaklar simgesi"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Genel Bakış simgesi"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials örnek ekran görüntüsü"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials örnek ekran görüntüsü"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






