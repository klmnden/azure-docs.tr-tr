---
title: Azure uygulama ağ geçidi hatalı ağ geçidi (502) hatalarında sorun giderme
description: Uygulama ağ geçidi 502 hatalarında sorun giderme hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: amsriva
ms.openlocfilehash: 2a1c7e480e896da6852949c9d765d17290e4e9ce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64697156"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Application Gateway'de hatalı ağ geçidi hatalarını giderme

Azure Application Gateway kullanırken alınan hatalı ağ geçidi (502) hatalarında sorun giderme hakkında bilgi edinin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Bir uygulama ağ geçidi yapılandırdıktan sonra görebileceğiniz hatalar biri olan "sunucu hatası: 502 - Web sunucusu bir ağ geçidi veya proxy sunucu olarak çalışırken geçersiz yanıt aldı" hatasıyla karşılaşırsanız. Bu hata, aşağıdaki ana nedenlerle oluşabilir:

* NSG, UDR veya özel DNS arka uç havuzu üyelerine erişimi engelliyor.
* Arka uç sanal makineleri veya sanal makine ölçek kümesi örnekleri varsayılan sistem durumu araştırmasına yanıt değil.
* Özel sistem durumu araştırmaları geçersiz veya hatalı yapılandırması.
* Azure uygulama ağ geçidinin [arka uç havuzu, yapılandırılmış veya boş değilse](#empty-backendaddresspool).
* Vm'leri veya durumlarda hiçbiri [sağlıklı sanal makine ölçek kümesi](#unhealthy-instances-in-backendaddresspool).
* [İsteği zaman aşımı veya bağlantı sorunları](#request-time-out) kullanıcı istekleriyle.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Ağ güvenlik grubu, kullanıcı tanımlı yolun veya özel DNS sorun

### <a name="cause"></a>Nedeni

Arka uç erişimi bir NSG, UDR veya özel DNS nedeniyle engellenirse, uygulama ağ geçidi örnekleri arka uç havuzuna erişemiyor. Bu araştırma hatası, 502 hatalarla sonuçlanır neden olur.

Uygulama Vm'leri dağıtıldığı NSG/UDR uygulama ağ geçidi alt ağı veya alt ağın mevcut olabilir.

Benzer şekilde, sanal ağda özel DNS varlığını sorunları da neden olabilirdi. Arka uç havuzu üyeleri için kullanılan bir FQDN doğru kullanıcı tarafından yapılandırılmış DNS sunucusu tarafından VNet için çözebilir değil.

### <a name="solution"></a>Çözüm

Aşağıdaki adımları izleyerek giderek NSG, UDR ve DNS yapılandırmasını doğrula:

* Nsg'ler, uygulama ağ geçidi alt ağı ile ilişkili denetleyin. Arka uca iletişimi engellenmemesini sağlamaya.
* UDR uygulama ağ geçidi alt ağı ile ilişkili denetleyin. UDR uzağa arka uç alt ağı trafiği yönlendiren olmadığından emin olun. Örneğin, ağ sanal Gereçleri veya uygulama ağ geçidi alt ağı ExpressRoute/VPN aracılığıyla tanıtılan varsayılan yollar için yönlendirme için denetleyin.

```azurepowershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Etkin NSG ve arka uç VM'den yol denetleyin

```azurepowershell
Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName nic1 -ResourceGroupName testrg
Get-AzEffectiveRouteTable -NetworkInterfaceName nic1 -ResourceGroupName testrg
```

* Sanal ağda özel DNS varlığını denetleyin. DNS, çıkış VNet özellikleri ayrıntılarını bakarak denetlenebilir.

```json
Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName 
DhcpOptions            : {
                           "DnsServers": [
                             "x.x.x.x"
                           ]
                         }
```
Varsa, DNS sunucusunun doğru arka uç havuzu ÜYESİNİN çözümleyebileceğinden emin olun.

## <a name="problems-with-default-health-probe"></a>Varsayılan durum araştırması ile ilgili sorunlar

### <a name="cause"></a>Nedeni

502 hataları da sık göstergeleri varsayılan durum araştırması arka uç sanal makine erişilemiyor olabilir.

Uygulama ağ geçidi örneğini sağlandığında varsayılan durum araştırması için her BackendAddressPool Backendhttpsetting'de özelliklerini kullanarak otomatik olarak yapılandırır. Bu araştırma ayarlamak için kullanıcı müdahalesi gerekir. Özellikle, bir Yük Dengeleme kuralı yapılandırıldığında, ilişkilendirme bir Backendhttpsetting'de ve bir BackendAddressPool arasında yapılır. Varsayılan araştırma her bu ilişkilendirmeleri için yapılandırılır ve uygulama ağ geçidi Backendhttpsetting'de öğesinde belirtilen bağlantı noktasında BackendAddressPool düzenli sistem durumu denetimi bağlantı her örneği başlatır. 

Aşağıdaki tabloda, varsayılan sistem durumu araştırma URL'siyle ilişkili değerleri listeler:

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Araştırma URL'si |`http://127.0.0.1/` |URL yolu |
| Interval |30 |Saniye cinsinden yoklama aralığı |
| Zaman aşımı |30 |Saniye cinsinden yoklama zaman aşımı |
| Sağlıksız durum eşiği |3 |Yeniden deneme sayısı araştırma. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

### <a name="solution"></a>Çözüm

* Varsayılan site yapılandırılır ve 127.0.0.1 dinliyor emin olun.
* 80 dışında bir bağlantı noktası Backendhttpsetting'de belirtiyorsa, varsayılan site Bu bağlantı noktasını dinlemek üzere yapılandırılması gerekir.
* Çağrı `http://127.0.0.1:port` 200 bir HTTP sonuç kodunu döndürmelidir. Bu 30 saniyelik zaman aşımı süresi içinde yönlendirileceksiniz.
* Yapılandırılmış bağlantı noktasının açık olduğunu ve hiçbir güvenlik duvarı kuralları veya yapılandırılmış bağlantı noktasında gelen veya giden trafiği engellemeye Azure ağ güvenlik grupları olduğundan emin olun.
* Azure Klasik sanal makineleri veya Bulut hizmeti bir FQDN veya bir genel IP ile kullanılıyorsa, karşılık gelen emin [uç nokta](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) açılır.
* VM'nin Azure Resource Manager aracılığıyla yapılandırılır ve uygulama ağ geçidi dağıtıldığı, VNet dışından bir [ağ güvenlik grubu](../virtual-network/security-overview.md) istediğiniz bağlantı noktasını erişimine izin verecek şekilde yapılandırılması gerekir.

## <a name="problems-with-custom-health-probe"></a>Özel durum araştırması ile ilgili sorunlar

### <a name="cause"></a>Nedeni

Özel sistem durumu araştırmaları, varsayılan davranışı yoklaması için daha fazla esneklik sağlar. Özel araştırmalar kullandığınızda, araştırma aralığı, URL, test etmek için yolun ve arka uç havuzu örnek sağlıksız olarak işaretleme önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz.

Aşağıdaki ek özellikler eklenir:

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırma adı. Bu ad, arka uç HTTP ayarlarında araştırma başvurmak için kullanılır. |
| Protocol |Araştırma göndermek için kullanılan protokol. Araştırma arka uç HTTP Ayarları'nda tanımlanan protokolünü kullanır. |
| Ana bilgisayar |Araştırma göndermek için ana bilgisayar adı. Çok siteli uygulama ağ geçidinde yalnızca yapılandırıldığı durumlarda uygulanabilir. Bu VM ana bilgisayar adından farklıdır. |
| `Path` |Araştırma göreli yolu. Geçerli yol başlatılır '/'. Yoklama için gönderilen \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\> |
| Interval |Aralık saniye cinsinden araştırma. İki ardışık araştırmaları arasındaki zaman aralığını budur. |
| Zaman aşımı |Zaman aşımını saniye cinsinden araştırma. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmazsa, araştırma başarısız olarak işaretlenir. |
| Sağlıksız durum eşiği |Yeniden deneme sayısı araştırma. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

### <a name="solution"></a>Çözüm

Özel durum yoklaması önceki tabloda olarak doğru şekilde yapılandırıldığını doğrulayın. Önceki sorun giderme adımları yanı sıra, ayrıca aşağıdakilerden emin olun:

* Araştırma olarak başına doğru belirtildiğinden emin olun [Kılavuzu](application-gateway-create-probe-ps.md).
* Uygulama ağ geçidi için tek bir site yapılandırdıysanız, varsayılan olarak ana bilgisayar adı olarak belirtilmelidir `127.0.0.1`, içinde özel araştırma aksi şekilde yapılandırılmadıkça.
* Http:// çağrısı emin\<konak\>:\<bağlantı noktası\>\<yolu\> 200 bir HTTP sonuç kodunu döndürür.
* Aralığını, zaman aşımı ve UnhealtyThreshold kabul edilebilir aralıkta olduğundan emin olun.
* Bir HTTPS kullanarak araştırma, arka uç sunucusunda kendisi bir geri dönüş sertifikası yapılandırarak arka uç sunucusuna SNI gerektirmeyen emin olun.

## <a name="request-time-out"></a>İstek zaman aşımı

### <a name="cause"></a>Nedeni

Bir kullanıcı isteği alındığında, uygulama ağ geçidi isteği için yapılandırılan kuralları uygular ve bir arka uç havuzu örneğine yönlendirir. Arka uç örnek yanıttan saati, yapılandırılabilir bir aralıkta bekler. Varsayılan olarak, bu aralığıdır **20** saniye. Uygulama ağ geçidi, bu aralıkta arka uç uygulamasından bir yanıt almazsa, kullanıcı isteği bir 502 hatasını alır.

### <a name="solution"></a>Çözüm

Uygulama ağ geçidi, bu ayar için farklı havuzlar uygulanabilir Backendhttpsetting'de aracılığıyla yapılandırmanıza olanak sağlar. Farklı arka uç havuzları farklı Backendhttpsetting'de ve yapılandırılmış farklı istek zaman aşımı olabilir.

```azurepowershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Boş BackendAddressPool

### <a name="cause"></a>Nedeni

Uygulama ağ geçidi Vm'leri ya da sanal makine ölçek kümesi varsa arka uç adres havuzundaki yapılandırılmış, herhangi bir müşteri istek yönlendirme yapamazsınız ve hatalı ağ geçidi hata gönderir.

### <a name="solution"></a>Çözüm

Arka uç adres havuzundaki boş olmadığından emin olun. Bu, PowerShell, CLI veya Portalı aracılığıyla yapılabilir.

```azurepowershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

Yukarıdaki cmdlet çıktısından, boş olmayan arka uç adres havuzu içermelidir. Aşağıdaki örnek, bir FQDN veya arka uç sanal makineleri için bir IP adresi ile yapılandırılmış iki havuz döndürülen gösterir. BackendAddressPool sağlama durumu 'başarılı' olması gerekir.

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>BackendAddressPool iyi durumda olmayan örnekleri

### <a name="cause"></a>Nedeni

BackendAddressPool öğesinin tüm örneklerini sağlam olmaması ise uygulama ağ geçidi rota Kullanıcı isteği için bir arka uç sahip değil. Arka uç örneklerinin sağlıklı olduğunu ancak dağıtılan gerekli uygulama yoksa bu durum da olabilir.

### <a name="solution"></a>Çözüm

Örnekleri sağlıklı olduğunu ve uygulama düzgün yapılandırıldığından emin olun. Arka uç örneklerinin aynı vnet'teki başka bir VM'den bir pinge yanıt vermezse denetleyin. Bir genel uç noktası ile yapılandırdıysanız, web uygulaması bir tarayıcı isteğini tutulabilmesi emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).

