---
title: Azure ExpressRoute genel uygulama senaryosuna ulaşmak | Microsoft Docs
description: Bu sayfa, Global erişim için bir uygulama senaryosu sağlar.
documentationcenter: na
services: networking
author: cherylmc
manager: tracsman
ms.service: expressroute,global reach
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: cherylmc
ms.openlocfilehash: 2adb8debf641a9b3875c5c386df7a35273f2a258
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428247"
---
# <a name="expressroute-global-reach-application-scenario"></a>ExpressRoute Global erişim uygulama senaryosu

ExpressRoute Global erişim hakkında daha fazla bilgi için bkz: [ExpressRoute Global erişim][Global Reach]. Bu makalede, şimdi bir uygulama senaryoyu inceleme, birkaç çözüm ExpressRoute Global erişim çözümü karşılaştırın, Global erişim Örnek senaryo için yapılandırmak ve bağlantıları doğrulayın. 

##<a name="application-scenario"></a>Uygulama senaryosu

Fabrikam Inc. büyük bulunmasını ve Doğu ABD Azure dağıtım var. Fabrikam, şirket içi ve ExpressRoute aracılığıyla Azure dağıtımlar arasında arka uç bağlantısı vardır. Contoso Ltd., bir varlığı ve Batı ABD Azure dağıtımında sahiptir. Contoso, şirket içi ve ExpressRoute aracılığıyla Azure dağıtımlar arasında arka uç bağlantısı vardır.  

Fabrikam Inc. acquires Contoso Ltd. Ağlar arasında bağlantı kurmak, Fabrikam birleşme istemektedir. Aşağıdaki şekil senaryo gösterilmektedir:

 [![1]][1]

Yukarıdaki şekilde ortasında kesikli oklar, gerekli ağ Connect gösterir. Aşağıdaki tabloda birincil özel, ExpressRoute, Contoso Ltd. birleşme önce eşleme rota tablosu gösterilmektedir.

[![2]][2]

Aşağıdaki tabloda birincil özel ExpressRoute, Fabrikam, Inc.'in birleşme önce eşleme rota tablosu gösterilmektedir.

[![3]][3]

## <a name="traditional-solutions"></a>Geleneksel çözümleri

### <a name="option-1-interconnect-on-premises-networks"></a>1. seçenek: Şirket içi ağlara bağlantı

Aşağıdaki şekil, bu seçenek gösterir. Gösterildiği gibi siteden siteye VPN veya diğer geniş bant bağlantı seçeneklerini kullanarak iki şirket içi ağ bağlantısı ve iki varlık arasında tüm yönlendirme yönetin. 

[![4]][4]

Bu seçenek, aşağıdaki dezavantajları vardır:

- Yetersiz bir rota dağıtımı için Azure bölgesel arası iletişimi zorlama.
- Microsoft omurga ağı bölgesel arası iletişim için yüksek kullanılabilirliği avantajı reddetme.
- Bölgesel arası iletişim için tam yönlendirme sorumluluğu yönlendiriyoruz.

### <a name="option-2-expressroute-cross-connectivity-and-interconnect-on-premises-networks"></a>2. seçenek: ExpressRoute bağlantı arası ve şirket içi ağlara bağlantı

Aşağıdaki şekil, bu seçenek gösterir.

[![5]][5]

Yukarıdaki şekilde gösterildiği gibi ayrıca ExpressRoute bağlantı hatları için çapraz bölgesel sanal ağlar arasında bağlantı kurabilirsiniz. ExpressRoute bağlantı hatları için sanal ağ arasında çapraz bölgesel bağlantı aşağıdaki iletişimleri etkinleştirebilirsiniz:

- ABD Batı / sırasıyla Fabrikam/Contoso ile iletişim kurmak için ABD Doğu Vnet'ler şirket içi ağlar.
- ABD Doğu sanal ağ ile iletişim kurmak için ABD Batı VNet.

İki şirket içi ağ arasında bağlantısı, şirket içi ağların birbirleri ile iletişim kurmak hala gereklidir.

Bu seçenek, Microsoft omurga üzerinden bulutumuzda özel dağıtımı için Azure bölgesel arası iletişim ve dış ağ üzerinden gerçekleştirilmesi için şirket içi ağ arasındaki iletişimi sağlar. Ancak, çözümün asıl sakıncası, Bakım ve sorun giderme karmaşık birden çok çapraz bölgesel bağlantıları kurmak olmanızın gerekmesidir. Ayrıca, seçeneği iki şirket içi ağ arasında iletişim kurmak için yüksek düzeyde kullanılabilir Microsoft Genel omurga yararlanmanıza izin vermez.

### <a name="option-3-vnet-peering-and-interconnect-on-premises-networks"></a>Seçenek 3: VNet eşlemesi ve şirket içi ağlara bağlantı

Aşağıdaki şekil, bu seçenek gösterir. VNet eşlemesi arası bölgesel sanal ağ iletişimi için seçeneğini kullanır. Bölgeler arası sanal ağ ile şirket içi iletişimi (noktalı ok satırlarını ortadaki aracılığıyla gösterilir) ele almaz olarak gösterildiği gibi seçeneği daha eksik. Şirket içi kullan (1. seçenek) olduğu gibi şirket içi bağlantı için ya da ExpressRoute için bölgeler arası bağlantı (seçenek 2) olduğu gibi şirket içi iletişimi arası.

[![6]][6]

Bu seçenek, arası bölgesel sanal ağ iletişimi için en uygun yönlendirme sağlar. Ancak, Fabrikam veya Contoso bir merkez-uç sanal ağ modeli gibi daha karmaşık bir Azure dağıtım varsa kısa döner. Ayrıca, önceki iki seçenek gibi bu seçenek, iki şirket içi ağ arasında iletişim kurmak için yüksek düzeyde kullanılabilir Microsoft Genel omurga yararlanmak izin vermez.

## <a name="global-reach"></a>Global Erişim

ExpressRoute genel erişmek, özel ExpressRoute bağlantı hatları aşağıdaki resimde gösterildiği gibi eşleme bağlantıları:

[![7]][7]

Global erişim arasında iki ExpressRoute bağlantı hatları yapılandırma iki şirket içi ağ ve iki bölgeleri Azure dağıtımında arasındaki özel iletişimi sağlar. Bu nedenle, Global erişim (Bu makalenin ilk şekilde kesik çizgi ile gösterilir) istenen iletişim Microsoft omurga ağı karşılar.

### <a name="configure-global-reach"></a>Küresel erişim yapılandırma

ExpressRoute Global erişim yapılandırma konusunda bilgi için bkz: [yapılandırma Global erişim][Configure Global Reach]. 

Fabrikam Inc. ve Contoso Ltd. eklenmedi Azure ayrı şirket olarak, iki farklı Azure aboneliklerinde iki şirketlerinin ExpressRoute devreleri olduğundan. Global erişim abonelikler arasında oluşturmak için bir yetkilendirme Contoso Ltd. ait ExpressRoute bağlantı hattı oluşturma ve Fabrikam Inc. geçmesi gerekir ExpressRoute bağlantı hattı.


İçin bir yetkilendirme Contoso'nun ExpressRoute bağlantı hattı için ilk oturum açma Contoso'nun Azure hesabı oluşturun ve (birden fazla aboneliğiniz varsa) uygun aboneliği seçin. Bu adımlar için PowerShell komutlarını verilmiştir:

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```
Bir ExpressRoute bağlantı hattı yetkilendirme oluşturmaya ilişkin adımlar aşağıda listelenmiştir:

```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```

'Set-AzureRmExpressRouteCircuit' çıkış ExpressRouteCircuit listeler. Özel Eşleme Kimliği ve listenin sonuna doğru listelenen yetkilendirme anahtarını not edin. Bir örnek PowerShell çıkış aşağıdaki kod parçacığında bakın:

```powershell
ServiceKey                       : <serviceKey>
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 65020,
                                       "PrimaryPeerAddressPrefix": "192.168.11.16/30",
                                       "SecondaryPeerAddressPrefix": "192.168.11.20/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 110,
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "4",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : [
                                     {
                                       "Name": "Auth4-ASH-Cust11",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/authorizations/Auth4-ASH-Cust11",
                                       "AuthorizationKey": "<authorizationKey>",
                                       "AuthorizationUseStatus": "Used",
                                       "ProvisioningState": "Succeeded"
                                     }
                                   ]
AllowClassicOperations           : False
GatewayManagerEtag               : 4
```

Eşleme Kimliği ve yetkilendirme anahtarını ile Global erişim Fabrikam'ın ExpressRoute bağlantı hattı altında oluşturabilirsiniz. Fabrikam'ın Azure hesabında oturum açın. Birden fazla aboneliğiniz varsa uygun aboneliği seçin.

Global erişimli arasında iki MSEE çiftleri noktadan noktaya bağlantılar yedekli bir dizi oluşturur. İki noktadan noktaya bağlantılar için bir/29 belirtmeniz gereken adres ön eki (çalışan bir örnek için 192.168.11.64/29 kullanalım). Global erişim bağlantısı oluşturmak için aşağıdaki komutları kullanın:

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix "__.__.__.__/29" -AuthorizationKey "########-####-####-####-############"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Yukarıdaki komutları yürütülürken sonra birkaç Global erişim yedekli bağlantılar oluşturmak için dakika sürer.

### <a name="verify-global-reach"></a>Küresel erişim doğrulayın

Aşağıdaki tabloda birincil özel ExpressRoute, Contoso Ltd. Global erişim yapılandırdıktan sonra eşleme rota tablosu gösterilmektedir.

[![8]][8]

Aşağıdaki tabloda birincil özel ExpressRoute, Fabrikam Inc. Global erişim yapılandırdıktan sonra eşleme rota tablosu gösterilmektedir.

[![9]][9]

Yukarıdaki tabloların tüm beklenen hedef 'Ağ' ön eklerin görüyoruz ve uygun 'sonraki atlama' listelenir.

Yukarıdaki 'Özel eşleme' altında Azure portalında bir ExpressRoute bağlantı hattı erişilebilir 'Get yol tablosu' dikey penceresini görürsünüz. Ayrıca aşağıdaki PowerShell komutunu kullanarak bir ExpressRoute yönlendirme tablosu listeleyebilirsiniz:

```powershell
Get-AzExpressRouteCircuitRouteTable -DevicePath 'primary' -ExpressRouteCircuitName "Your_circuit_name" -PeeringType AzurePrivatePeering -ResourceGroupName "Your_resource_group"
```

İKİNCİL yol tablosuna görmek için 'birincil' anahtar sözcüğü 'yukarıdaki komutta ikincil' ile değiştirin.

Ayrıca farklı ağlardaki ping işlemi farklı bir hedef veri düzlemi bağlantıdan doğrulayalım. Aşağıdaki üç ping Contoso Azure sanal ağı, Contoso şirket içi ağ veri düzlemi bağlantısını doğrulayın ve Fabrikam Fabrikam Azure sanal ağı şirket içi.


    C:\Users\FabrikamUser>ping 10.1.11.10
    
    Pinging 10.1.11.10 with 32 bytes of data:
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    
    Ping statistics for 10.1.11.10:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 69ms, Maximum = 69ms, Average = 69ms
    
    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=82ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=81ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 82ms, Average = 78ms
    
    C:\Users\FabrikamUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=2ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 3ms, Average = 2ms


Aşağıdaki iki ping Contoso şirket içi ağdan Azure VNet Contoso ve Fabrikam Azure sanal ağı için veri düzlemi bağlantısını kontrol edin.

    C:\Users\ContosoUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=6ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 5ms, Maximum = 6ms, Average = 5ms
    
    C:\Users\ContosoUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=74ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 73ms, Maximum = 74ms, Average = 73ms

Aşağıdaki ping veri düzlemi bağlantısı Contoso Azure Vnet'e Fabrikam Azure sanal ağdan doğrular.

    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=78ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 78ms, Average = 77ms

## <a name="optimization-for-latency-sensitive-traffic"></a>Gecikme süresi duyarlı trafik için iyileştirme

Küresel erişim, Microsoft Edge cihazları aracılığıyla trafiği yönlendirir. Bu makalede kabul belirli senaryo için daha fazla iyi sanal ağ arasında eşleme etkinleştirerek iki sanal ağ arasında yönlendirme elde edebilirsiniz. Benzer şekilde daha iyi siteler arasında daha doğrudan bir WAN bağlantısı sağlayabilen bir hizmet sağlayıcısı üzerinden iki şirket içi ağ arasında yönlendirme elde edebilirsiniz. Böyle senaryolarda, Global erişim için bu bağlantıları başarısız geri seçeneği olarak Yönlendirme kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Küresel erişim ülkeye göre ülke olarak alınır. Global erişim istediğiniz ülkede kullanılabilir olup olmadığını görmek için bkz: [ExpressRoute Global erişim][Global Reach].

<!--Image References-->
[1]: ./media/expressroute-globalreach-applicationscenario/PreMergerScenario.png "uygulama senaryosu"
[2]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Premerger.png "birleşme önce Contoso ExpressRoute yol tablosu"
[3]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Premerger.png "birleşme önce Fabrikam ExpressRoute yol tablosu"
[4]: ./media/expressroute-globalreach-applicationscenario/S2SVPN-Solution.png "şirket içi ağlar interconnecting"
[5]: ./media/expressroute-globalreach-applicationscenario/ExR-XConnect-Solution.png "çapraz ExpressRoute bağlanma"
[6]: ./media/expressroute-globalreach-applicationscenario/VNet-peering-Solution.png "VNet eşlemesi"
[7]: ./media/expressroute-globalreach-applicationscenario/GlobalReach-Solution.png "küresel erişim"
[8]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Postmerger.png "Contoso ExpressRoute yönlendirme tablosu ile Global erişim"
[9]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Postmerger.png "Fabrikam ExpressRoute yönlendirme tablosu ile Global erişim"

<!--Link References-->
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach





