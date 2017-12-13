---
title: "Doğrulanıyor bağlantısı: Sorun giderme kılavuzu Azure ExpressRoute | Microsoft Docs"
description: "Bu sayfa, sorun giderme ve bir expressroute bağlantı hattı uçtan uca bağlantısını doğrulama yönergelerini sağlar."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: cherylmc
ms.openlocfilehash: e52e53255a1462522f297d8918eb1c347a460f77
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="verifying-expressroute-connectivity"></a>ExpressRoute bağlantısı doğrulanıyor
Bir şirket içi ağ bağlantı sağlayıcı tarafından kolaylaştırılan özel bir bağlantı üzerinden Microsoft bulutunu genişletir, ExpressRoute aşağıdaki üç farklı ağ bölgeleri içerir:

-   Müşteri ağ
-   Sağlayıcı ağ
-   Microsoft Veri merkezinde

Bu belgenin amacı, nereye tanımlamak için kullanıcı yardımcı olmaktır (veya bir olsa bile) bir bağlantı sorunu var ve böylece bu sorunu çözmek için uygun ekibinden yardım aramak için hangi bölgedeki. Bir sorunu gidermek için Microsoft destek gerekirse ile destek bilet [Microsoft Support][Support].

> [!IMPORTANT]
> Bu belge, tanılama ve basit sorunlarını giderme Yardımı için tasarlanmıştır. Microsoft destek için yenileme olması amaçlanmamıştır. Bir destek bileti ile açmak [Microsoft Support] [ Support] sağlanan yönergeleri kullanarak sorunu çözmeyi erişemiyorsanız.
>
>

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagramda bir müşteri ağı ExpressRoute kullanarak Microsoft ağına mantıksal bağlantısını gösterir.
[![1]][1]

Önceki diyagramda sayıları anahtar ağ noktalarını belirtin. Ağ noktaları genellikle ilişkili numaralarına göre bu makalede başvurulur.

ExpressRoute bağlantı modeli (bulut Exchange birlikte bulundurma, noktadan noktaya Ethernet bağlantısı veya Any herhangi bir (IPVPN)) bağlı olarak ağ noktalarını 3 ve 4 anahtarları (Katman 2 aygıtları) kullanılabilir. Gösterilen anahtar ağ noktaları aşağıdaki gibidir:

1.  Müşteri işlem aygıt (örneğin, bir sunucu veya bilgisayar)
2.  CEs: Müşteri sınır yönlendiricileri 
3.  PEs (CE'e yönelik): sağlayıcısı sınır yönlendiricileri/müşteri sınır yönlendiricileri karşılıklı anahtarları. İçin bu belgede PE CEs adlandırılır.
4.  PEs (MSEE'e yönelik): sağlayıcısı sınır yönlendiricileri/Msee'ler karşılıklı anahtarları. İçin bu belgede PE Msee'ler adlandırılır.
5.  Msee'ler: Microsoft Enterprise Edge (MSEE) ExpressRoute yönlendiricileri
6.  Sanal ağ (VNet) ağ geçidi
7.  Azure sanal cihazda işlem

Bulut Exchange birlikte bulundurma veya noktadan noktaya Ethernet bağlantısı bağlantı modeli kullanılıyorsa, müşteri sınır yönlendiricisi (2) BGP eşliği (5) Msee'ler ile oluşturmanız. Ağ noktalarını 3 ve 4 hala var ancak katman 2 cihaz olarak biraz saydam.

Any herhangi bir (IPVPN) bağlantı modeli kullanılıyorsa, (MSEE'e yönelik) PEs (4) BGP eşliği (5) Msee'ler ile kurmak. Yollar sonra geri müşteri ağ IPVPN hizmet sağlayıcısı ağ üzerinden yayılması.

>[!NOTE]
>ExpressRoute, yüksek kullanılabilirlik için yedek bir BGP oturumları Msee'ler (5) PE Msee'ler (4) arasındaki çift Microsoft gerektirir. Ağ yolları yedek çifti de müşteri ağ arasında PE CEs teşvik. Ancak, Any herhangi bir (IPVPN) bağlantı modelinde, tek bir CE aygıt (2) bir veya daha fazla PEs (3) için bağlı olabilir.
>
>

Bir expressroute bağlantı hattı doğrulamak için aşağıdaki adımları (noktasıyla ilişkili sayıyla ağ) ele alınmaktadır:
1. [Bağlantı hattı hazırlama ve durumu (5) doğrula](#validate-circuit-provisioning-and-state)
2. [En az bir ExpressRoute doğrulamak eşliği, yapılandırılmış (5)](#validate-peering-configuration)
3. [Microsoft ile hizmet arasında ARP doğrulama sağlayıcısı (4 ve 5 arasında bağlantı)](#validate-arp-between-microsoft-and-the-service-provider)
4. [BGP ve (BGP 4 ila 5 5-bir VNet bağlıysa, 6 arasındaki) MSEE yollara doğrula](#validate-bgp-and-routes-on-the-msee)
5. [Onay trafiği istatistikleri (trafik 5'ten geçirme)](#check-the-traffic-statistics)

Daha fazla doğrulama ve denetimleri geri gelecekteki iade aylık eklenecek!

##<a name="validate-circuit-provisioning-and-state"></a>Bağlantı hattı hazırlama ve durumunu doğrula
Bağlantı modeli bağımsız olarak bir expressroute bağlantı hattı oluşturulması gerekir ve bu nedenle devre sağlama için bir hizmet anahtarı oluşturulur. Bir expressroute bağlantı hattı sağlama PE Msee'ler (4) ve Msee'ler (5) arasında yedekli bir katman 2 bağlantı kurar. Makale oluşturma, değiştirme, sağlamak ve bir expressroute bağlantı hattı doğrulamanın nasıl yapılacağı hakkında daha fazla bilgi için bkz: [oluşturma ve bir expressroute bağlantı hattı değiştirme][CreateCircuit].

>[!TIP]
>Bir hizmet anahtarı, bir expressroute bağlantı hattı benzersiz olarak tanımlar. Bu belgede belirtilen powershell komutların çoğu için bu anahtar gereklidir. Ayrıca, bir ExpressRoute sorun giderme, bağlantı hattı kolaylıkla tanımlamak için hizmet anahtarı sağlamak için bir expressroute bağlantı ortağı Microsoft'tan ya da Yardım gerekir.
>
>

###<a name="verification-via-the-azure-portal"></a>Azure Portalı aracılığıyla doğrulama
Azure portalında seçerek bir expressroute bağlantı hattı durumunu denetlenebilir ![2][2] sol kenar çubuğu menü ve expressroute bağlantı hattı seçme. Bir expressroute bağlantı seçme "Tüm kaynaklar" altında listelenen bağlantı hattı ExpressRoute bağlantı hattı dikey penceresi açılır. İçinde ![3][3] dikey penceresinde aşağıdaki ekran görüntüsünde gösterildiği gibi essentials listelenen ExpressRoute bölümünü:

![4][4]    

ExpressRoute essentials'ta *hattı durum* Microsoft tarafında devre durumunu gösterir. *Sağlayıcı durumu* bağlantı hattı olup olmadığını gösteren *hazırlandı/değil sağlanan* hizmet sağlayıcı tarafındaki. 

İşlem, olacak şekilde bir expressroute bağlantı hattı için *hattı durumu* olmalıdır *etkin* ve *sağlayıcı durumu* olmalıdır *hazırlandı*.

>[!NOTE]
>Varsa *hattı durumu* olduğu etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *sağlayıcı durumu* olan sağlanan değil, hizmet sağlayıcınıza başvurun.
>
>

###<a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Bir kaynak grubundaki tüm ExpressRoute bağlantı hatları listelemek için aşağıdaki komutu kullanın:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Azure kaynak grubu adı elde edebilirsiniz. Bu belgenin önceki alt bölümüne bakın ve kaynak grubu adı örnek ekran görüntüsü listelendiğine dikkat edin.
>
>

Bir kaynak grubunda belirli bir expressroute bağlantı hattı seçmek için aşağıdaki komutu kullanın:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

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

Bir expressroute bağlantı hattı işletimsel olup olmadığını onaylamak için belirli aşağıdaki alanları dikkat edin:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
>Varsa *CircuitProvisioningState* olduğu etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *ServiceProviderProvisioningState* olan sağlanan değil, hizmet sağlayıcınıza başvurun.
>
>

###<a name="verification-via-powershell-classic"></a>PowerShell (Klasik) aracılığıyla doğrulama
Bir abonelik altında tüm ExpressRoute bağlantı hatları listelemek için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuit

Belirli bir expressroute bağlantı hattı seçmek için aşağıdaki komutu kullanın:

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

Bir expressroute bağlantı hattı işletimsel olup olmadığını onaylamak için belirli aşağıdaki alanları dikkat edin: ServiceProviderProvisioningState: sağlanan durum: etkin

>[!NOTE]
>Varsa *durum* olduğu etkin değilse, ilgili kişi [Microsoft Support][Support]. Varsa *ServiceProviderProvisioningState* olan sağlanan değil, hizmet sağlayıcınıza başvurun.
>
>

##<a name="validate-peering-configuration"></a>Eşleme yapılandırmasını doğrulayın
Hizmet sağlayıcısı expressroute bağlantı hattı Sağlama tamamlandıktan sonra bir yönlendirme yapılandırması MSEE PRs (4) Msee'ler (5) arasındaki expressroute bağlantı hattı üzerinden oluşturulabilir. Her expressroute bağlantı hattı etkin bir, iki veya üç yönlendirme bağlamları sahip olabilir: Azure özel eşleme (trafiğin Azure içindeki özel sanal ağların için), Azure ortak eşleme (trafiğin Azure genel IP adresleri için) ve Microsoft eşleme (trafiği Office 365 ve Dynamics 365). Oluşturma ve yönlendirme yapılandırmasını değiştirme hakkında daha fazla bilgi için bkz: [oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].

###<a name="verification-via-the-azure-portal"></a>Azure Portalı aracılığıyla doğrulama

>[!NOTE]
>Katman 3 hizmet sağlayıcısı tarafından sağlanır ve eşlemeler portalda boş, Yenile düğmesini protal üzerinde kullanarak hattı yapılandırmasını yenileyin. Bu işlem doğru yönlendirme yapılandırması hattınız üzerinde uygulanır. 
>
>

Azure portalında seçerek bir expressroute bağlantı hattı durumunu denetlenebilir ![2][2] sol kenar çubuğu menü ve expressroute bağlantı hattı seçme. Bir expressroute bağlantı seçme "Tüm kaynaklar" altında listelenen bağlantı hattı ExpressRoute bağlantı hattı dikey penceresi açılır. İçinde ![3][3] dikey penceresinde aşağıdaki ekran görüntüsünde gösterildiği gibi essentials'in listelenmesi ExpressRoute bölümünü:

![5][5]

Azure ortak ve Microsoft eşleme yönlendirme bağlamları etkin olmayan ancak önceki örnekte belirtilen Azure özel eşleme yönlendirme bağlamı olarak etkinleştirilir. Başarılı bir şekilde etkinleştirilmiş bir eşleme bağlamı listelenen (BGP için gerekli) birincil ve ikincil noktadan noktaya alt ağlar da gerekir. / 30 alt ağ arabiriminin IP adresini ve Msee'ler PE Msee için kullanılır. 

>[!NOTE]
>Bir eşleme etkin değilse, atanan birincil ve ikincil alt ağları PE Msee'ler yapılandırmasına eşleşip eşleşmediğini denetleyin. Aksi durumda, MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering]
>
>

###<a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Azure özel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Bir başarılı bir şekilde yapılandırılmış özel eşleme için bir örnek yanıt şöyledir:

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

 Başarılı bir şekilde etkinleştirilmiş bir eşleme bağlamı, listelenen birincil ve ikincil adres öneklerini gerekir. / 30 alt ağ arabiriminin IP adresini ve Msee'ler PE Msee için kullanılır.

Azure ortak eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

Microsoft eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

Bir eşleme yapılandırılmamışsa bir hata iletisi olacaktır. Belirtilen eşleme (Azure Bu örnekte eşliği genel) içinde hattı yapılandırılmadığında bir örnek yanıt:

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Bir eşleme etkin değilse, atanan birincil ve ikincil alt ağları Bağlantılı PE MSEE yapılandırmasına eşleşip eşleşmediğini denetleyin. Ayrıca denetleyin doğru *Vlanıd*, *AzureASN*, ve *PeerASN* üzerinde Msee'ler kullanılır ve bu değerleri eşleniyorsa bağlantılı PE MSEE üzerinde kullanılan olanlara. MD5 karma seçilirse, paylaşılan anahtar MSEE ve PE MSEE çiftine aynı olması gerekir. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>PowerShell (Klasik) aracılığıyla doğrulama
Azure özel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutu kullanın:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Özel bir başarılı bir şekilde yapılandırılmış eşleme için ek olarak, örnek yanıt şöyledir:

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

A başarılı bir şekilde, etkinleştirilmiş eşleme bağlamı, listelenen birincil ve ikincil eş alt ağlar sahip olabilir. / 30 alt ağ arabiriminin IP adresini ve Msee'ler PE Msee için kullanılır.

Azure ortak eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

Microsoft eşlemesi yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>Katman 3 eşlemeler hizmet sağlayıcısı tarafından ayarlanan, portal veya PowerShell üzerinden ExpressRoute eşlemeler ayarlama, servis sağlayıcı ayarları üzerine yazar. Sağlayıcı yan eşleme ayarları sıfırlama hizmet sağlayıcısının desteği gerektirir. Hizmet sağlayıcısı yalnızca Katman 2 hizmetleri sağlayan belirli ise yalnızca ExpressRoute eşlemeler değiştirin!
>
>

<p/>
>[!NOTE]
>Bir eşleme etkin değilse, atanan birincil ve ikincil eş alt ağları Bağlantılı PE MSEE yapılandırmasına eşleşip eşleşmediğini denetleyin. Ayrıca denetleyin doğru *Vlanıd*, *AzureAsn*, ve *PeerAsn* üzerinde Msee'ler kullanılır ve bu değerleri eşleniyorsa bağlantılı PE MSEE üzerinde kullanılan olanlara. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a>Microsoft hizmet sağlayıcısı arasında ARP doğrula
Bu bölüm, PowerShell (Klasik) komutlarını kullanır. Azure Resource Manager PowerShell komutları kullanıyorsanız, abonelik Yöneticisi/ortak yönetici erişimi olduğundan emin olun. Azure Resource Manager kullanarak sorun giderme için komutları lütfen [Resource Manager dağıtım modeli alma ARP tablolarda] [ ARP] belge.

>[!NOTE]
>ARP almak için Azure portalı ve Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutları ile bir hatayla karşılaşılmazsa Klasik PowerShell komutlarını Klasik PowerShell komutları de Azure Resource Manager ExpressRoute bağlantı hatları ile çalışması olarak çalışması gerekir.
>
>

Özel eşleme için birincil MSEE'nin yönlendiriciden ARP tablosu almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Bir örnek yanıt başarılı senaryoda komutu için:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Benzer şekilde, MSEE ARP tablosundan kontrol edebilirsiniz *birincil*/*ikincil* yolu için *özel*/*ortak*/*Microsoft* eşlemeleri.

Aşağıdaki örnek, bir eşleme için komut yanıtı yok gösterir.

    ARP Info:
       
>[!NOTE]
>ARP tablosu MAC adreslerinin eşlenen arabirimlerin IP adreslerini yoksa, aşağıdaki bilgileri gözden geçirin:
>1. Varsa/30 ilk IP adresini MSEE PR ve MSEE arasındaki bağlantıyı MSEE PR. arabirimde kullanılır atanan alt ağ Azure, ikinci IP adresi Msee için her zaman kullanır.
>2. Hizmet (S-Tag) VLAN etiketlerini ve müşteri (C-Tag) hem de MSEE PR ve MSEE çifti eşleşip eşleşmediğini denetleyin.
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a>BGP ve MSEE yollara doğrula
Bu bölüm, PowerShell (Klasik) komutlarını kullanır. Azure Resource Manager PowerShell komutları kullanıyorsanız, abonelik Yöneticisi/ortak yönetici erişimi olduğundan emin olun.

>[!NOTE]
>BGP bilgi almak için Azure portalı ve Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutları ile bir hatayla karşılaşılmazsa Klasik PowerShell komutlarını Klasik PowerShell komutları de Azure Resource Manager ExpressRoute bağlantı hatları ile çalışması olarak çalışması gerekir.
>
>

Belirli bir yönlendirme bağlamının (BGP komşu) yönlendirme tablosu özetini almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Bir örnek yanıt şöyledir:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Önceki örnekte gösterildiği gibi komut yönlendirme ne kadar süreyle bağlamı için kurulmuş belirlemek yararlıdır. Ayrıca, eşleme yönlendirici tarafından tanıtılan rota ön sayısını gösterir.

>[!NOTE]
>Durumu etkin veya boş ise, atanan birincil ve ikincil eş alt ağları Bağlantılı PE MSEE yapılandırmasına eşleşip eşleşmediğini denetleyin. Ayrıca denetleyin doğru *Vlanıd*, *AzureAsn*, ve *PeerAsn* üzerinde Msee'ler kullanılır ve bu değerleri eşleniyorsa bağlantılı PE MSEE üzerinde kullanılan olanlara. MD5 karma seçilirse, paylaşılan anahtar MSEE ve PE MSEE çiftine aynı olması gerekir. MSEE yönlendiricilerde yapılandırmasını değiştirmek için başvurmak [oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering].
>
>

<p/>
>[!NOTE]
>Belirli bir eşlemesi içinde belirli hedefler erişilebilir değilse, belirli eşleme bağlamına ait Msee'ler rota tablosu denetleyin. Yönlendirme tablosunda (NATed IP olabilir) eşleşen bir önek varsa yola NSG/güvenlik duvarları/ACL'leri varsa ve trafiğe izin olmadığını denetleyin.
>
>

Tam yönlendirme tablosu MSEE almak için *birincil* belirli yolu *özel* bağlamı yönlendirme, aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Komutu için bir örnek başarılı sonuç verilmiştir:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Benzer şekilde, MSEE yönlendirme tablosundan kontrol edebilirsiniz *birincil*/*ikincil* yolu için *özel*/*ortak*/*Microsoft* eşleme bağlamı.

Aşağıdaki örnek, bir eşleme için komut yanıtı yok gösterir:

    Route Table Info:

##<a name="check-the-traffic-statistics"></a>Onay trafiği istatistikleri
Birleşik birincil ve ikincil yol trafiğini istatistikleri--bayt ve bir kapatma--bir eşleme bağlamında almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Komutun bir örnek çıktı verilmiştir:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Örnek bir çıktı mevcut olmayan eşleme için komut şöyledir:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi veya Yardım için aşağıdaki bağlantıları kontrol edin:

- [Microsoft Destek][Support]
- [Oluşturma ve bir expressroute bağlantı hattı değiştirme][CreateCircuit]
- [Oluşturma ve bir expressroute bağlantı hattı için yönlendirmeyi değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "mantıksal hızlı Rota bağlantısı"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Tüm kaynaklar simgesi"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Genel Bakış simgesi"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials örnek ekran görüntüsü"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials örnek ekran görüntüsü"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






