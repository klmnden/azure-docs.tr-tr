---
title: Azure uygulama ağ geçidi hatalı ağ geçidi (502) hatalarında sorun giderme | Microsoft Docs
description: Uygulama ağ geçidi 502 hatalarında sorun giderme hakkında bilgi edinin
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: d50f25fbe10fc5ac4e834141fe7ac45fbed918ab
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309035"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Application Gateway'de hatalı ağ geçidi hatalarını giderme

Application gateway kullanırken alınan hatalı ağ geçidi (502) hatalarında sorun giderme hakkında bilgi edinin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Bir uygulama ağ geçidi yapılandırdıktan sonra kullanıcının karşılaşabileceği hatalardan biri olan "sunucu hatası: 502 - Web sunucusu bir ağ geçidi veya proxy sunucu olarak çalışırken geçersiz yanıt aldı" hatasıyla karşılaşırsanız. Bu hata, aşağıdaki ana nedenlerden ötürü oluşabilir:

* NSG, UDR veya özel DNS arka uç havuzu üyelerine erişimi engelliyor.
* Arka uç sanal makineleri veya sanal makine ölçek kümesi örnekleri varsayılan sistem durumu araştırmasına yanıt vermiyor.
* Özel sistem durumu araştırmaları geçersiz veya hatalı yapılandırması.
* Azure uygulama ağ geçidinin [arka uç havuzu yapılandırılmış ya da boş değil](#empty-backendaddresspool).
* Vm'leri veya durumlarda hiçbiri [sağlıklı sanal makine ölçek kümesi](#unhealthy-instances-in-backendaddresspool).
* [İsteği zaman aşımı veya bağlantı sorunları](#request-time-out) kullanıcı istekleriyle.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Ağ güvenlik grubu, kullanıcı tanımlı yolun veya özel DNS sorun

### <a name="cause"></a>Nedeni

NSG, UDR veya özel DNS varlığını nedeniyle arka uç için erişim engellendi, Application Gateway örneğinden arka uç havuzuna erişmek mümkün olmayacaktır ve 502 hatalara neden araştırması hatalarına neden. NSG/UDR uygulama VM'ler dağıtıldığı mevcut uygulama ağ geçidi alt ağı veya alt olabileceğini unutmayın. Benzer şekilde sanal ağda özel DNS varlığını de FQDN arka uç havuzu üyeleri için kullanılıyorsa ve VNET için doğru kullanıcı tarafından yapılandırılmış DNS sunucusu tarafından çözülmemişse sorunlar neden olabilir.

### <a name="solution"></a>Çözüm

Aşağıdaki adımları izleyerek giderek NSG, UDR ve DNS yapılandırmasını doğrula:
* Uygulama ağ geçidi alt ağı ile ilişkili Nsg'ler denetleyin. Arka uca iletişimi engellenmediğinden emin olun.
* Uygulama ağ geçidi alt ağı ile ilişkili UDR denetleyin. UDR uzağa arka uç alt ağı trafiği yönlendiren değil olun - ağ sanal Gereçleri veya uygulama ağ geçidi alt ağı ExpressRoute/VPN aracılığıyla tanıtılan varsayılan yollar için yönlendirme için örneğin denetleyin.

```powershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Etkin NSG ve arka uç VM'den yol denetleyin

```powershell
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
Varsa, DNS sunucusu arka uç havuzu ÜYESİNİN doğru çözümleyemiyorsa olduğundan emin olun.

## <a name="problems-with-default-health-probe"></a>Varsayılan durum araştırması ile ilgili sorunlar

### <a name="cause"></a>Nedeni

502 hataları, sık sık göstergeleri varsayılan durum araştırması arka uç sanal makine ulaşabildiğinden değil de olabilir. Application Gateway örneği sağlandığında varsayılan durum araştırması için her BackendAddressPool Backendhttpsetting'de özelliklerini kullanarak otomatik olarak yapılandırır. Bu araştırma ayarlamak için kullanıcı müdahalesi gerekir. Özellikle, bir Yük Dengeleme kuralı yapılandırıldığında, ilişkilendirme Backendhttpsetting'de ve BackendAddressPool arasında yapılır. Varsayılan araştırma her bu ilişkilendirmeleri için yapılandırılır ve Application Gateway düzenli sistem durumu denetimi bağlantı BackendAddressPool Backendhttpsetting'de öğesinde belirtilen bağlantı noktasındaki her bir örneği başlatır. Aşağıdaki tabloda, varsayılan sistem durumu araştırma URL'siyle ilişkili değerleri listelenmektedir.

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Yoklama URL'si |http://127.0.0.1/ |URL yolu |
| Interval |30 |Saniye cinsinden yoklama aralığı |
| Zaman aşımı |30 |Saniye cinsinden yoklama zaman aşımı |
| İyi durumda olmayan eşik |3 |Yeniden deneme sayısı araştırma. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

### <a name="solution"></a>Çözüm

* Varsayılan site yapılandırılır ve 127.0.0.1 dinliyor emin olun.
* 80 dışında bir bağlantı noktası Backendhttpsetting'de belirtiyorsa, varsayılan site Bu bağlantı noktasını dinlemek üzere yapılandırılması gerekir.
* Çağrı http://127.0.0.1:port 200 bir HTTP sonuç kodunu döndürmelidir. Bu 30 sn zaman aşımı süresi içinde yönlendirileceksiniz.
* Yapılandırılan bağlantı noktası açık olduğunu ve hiçbir güvenlik duvarı kuralları veya yapılandırılmış bağlantı noktasında gelen veya giden trafiği engellemeye Azure ağ güvenlik grupları olduğundan emin olun.
* Azure Klasik sanal makineleri veya Bulut hizmeti FQDN veya genel IP ile kullanılıyorsa, karşılık gelen emin [uç nokta](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) açılır.
* VM'nin Azure Resource Manager aracılığıyla yapılandırılır ve Application Gateway dağıtıldığı, VNet dışından [ağ güvenlik grubu](../virtual-network/security-overview.md) istediğiniz bağlantı noktasını erişimine izin verecek şekilde yapılandırılması gerekir.

## <a name="problems-with-custom-health-probe"></a>Özel durum araştırması ile ilgili sorunlar

### <a name="cause"></a>Nedeni

Özel sistem durumu araştırmaları, varsayılan davranışı yoklaması için daha fazla esneklik sağlar. Özel araştırmalar kullanırken, kullanıcılar araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretleme önce kabul etmek için kaç başarısız yanıtları yapılandırabilir. Aşağıdaki ek özellikler eklenir.

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırma adı. Bu ad, arka uç HTTP ayarlarında araştırma başvurmak için kullanılır. |
| Protokol |Araştırma göndermek için kullanılan protokol. Araştırma arka uç HTTP Ayarları'nda tanımlanan protokolünü kullanır. |
| Host |Araştırma göndermek için ana bilgisayar adı. Çok siteli uygulama ağ geçidinde yalnızca yapılandırıldığı durumlarda uygulanabilir. Bu VM ana bilgisayar adından farklıdır. |
| Yol |Araştırma göreli yolu. Geçerli yol başlatılır '/'. Yoklama için gönderilen \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\> |
| Interval |Aralık saniye cinsinden araştırma. İki ardışık araştırmaları arasındaki zaman aralığını budur. |
| Zaman aşımı |Zaman aşımını saniye cinsinden araştırma. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmazsa, araştırma başarısız olarak işaretlenir. |
| İyi durumda olmayan eşik |Yeniden deneme sayısı araştırma. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

### <a name="solution"></a>Çözüm

Özel durum yoklaması önceki tabloda olarak doğru şekilde yapılandırıldığını doğrulayın. Önceki sorun giderme adımları yanı sıra, ayrıca aşağıdakilerden emin olun:

* Araştırma olarak başına doğru belirtildiğinden emin olun [Kılavuzu](application-gateway-create-probe-ps.md).
* Application Gateway için tek bir site yapılandırdıysanız, varsayılan olarak ana bilgisayar adı '127.0.0.1' özel araştırma aksi şekilde yapılandırılmadıkça belirtilmelidir.
* Http:// çağrısı emin\<konak\>:\<bağlantı noktası\>\<yolu\> 200 bir HTTP sonuç kodunu döndürür.
* Aralığını, zaman aşımı ve UnhealtyThreshold kabul edilebilir aralıkta olduğundan emin olun.
* Bir HTTPS kullanarak araştırma, arka uç sunucusunda kendisi bir geri dönüş sertifikası yapılandırarak arka uç sunucusuna SNI gerektirmeyen emin olun.

## <a name="request-time-out"></a>İstek zaman aşımı

### <a name="cause"></a>Nedeni

Bir kullanıcı isteği alındığında, uygulama ağ geçidi isteği için yapılandırılan kuralları uygular ve bir arka uç havuzu örneğine yönlendirir. Arka uç örnek yanıttan saati, yapılandırılabilir bir aralıkta bekler. Varsayılan olarak, bu aralığıdır **30 saniye**. Application Gateway, bu aralıkta arka uç uygulamasından bir yanıt almazsa, kullanıcı isteği 502 bir hata görebilirsiniz.

### <a name="solution"></a>Çözüm

Uygulama ağ geçidi aracılığıyla farklı havuzlar için uygulanabilir Backendhttpsetting'de, bu ayarı yapılandırmak kullanıcıların sağlar. Farklı arka uç havuzları farklı Backendhttpsetting'de ve bu nedenle farklı istek zaman aşımı yapılandırılmış olabilir.

```powershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Boş BackendAddressPool

### <a name="cause"></a>Nedeni

Uygulama ağ geçidi Vm'leri ya da sanal makine ölçek kümesi arka uç adres havuzundaki yapılandırılmış, herhangi bir müşteri istek yönlendirme yapamazsınız ve hatalı ağ geçidi hata oluşturur.

### <a name="solution"></a>Çözüm

Arka uç adres havuzundaki boş olmadığından emin olun. Bu, PowerShell, CLI veya Portalı aracılığıyla yapılabilir.

```powershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

Yukarıdaki cmdlet çıktısından, boş olmayan arka uç adres havuzu içermelidir. Aşağıda, bir örnek Burada, hangi döndürülen iki havuz arka uç sanal makineleri için FQDN veya IP adresleriyle yapılandırılmış. BackendAddressPool sağlama durumu 'başarılı' olması gerekir.

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

Ardından Application Gateway BackendAddressPool öğesinin tüm örneklerini sağlam olmaması ise kullanıcının istek için tüm arka uç bulunmaz. Arka uç örneklerinin sağlıklı olduğunu ancak dağıtılan uygulamayı gerekli izniniz yok, bu durum olabilir.

### <a name="solution"></a>Çözüm

Örnekleri sağlıklı olduğunu ve uygulama düzgün yapılandırıldığından emin olun. Arka uç örneklerinin aynı vnet'teki başka bir VM'den bir pinge yanıt verebilir durumda olup olmadığını denetleyin. Bir genel uç noktası ile yapılandırdıysanız, web uygulaması bir tarayıcı isteğini tutulabilmesi olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).

