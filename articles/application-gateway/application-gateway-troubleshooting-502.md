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
ms.openlocfilehash: 4eca6a588d2c95189f0ba995b8db195907e9dc39
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34356044"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Uygulama ağ geçidi olarak hatalı ağ geçidi hatalarında sorun giderme

Uygulama ağ geçidi kullanırken karşılaşılan hatalı ağ geçidi (502) hataları giderme öğrenin.

## <a name="overview"></a>Genel Bakış

Bir uygulama ağ geçidi yapılandırdıktan sonra kullanıcılar karşılaşabilirsiniz hataları biri olan "sunucu hatası: 502 - Web sunucusu alınan geçersiz bir yanıt bir ağ geçidi veya proxy sunucu olarak çalışırken". Bu hata, aşağıdaki ana nedenlerden ötürü oluşabilir:

* NSG, UDR veya özel DNS arka uç havuzu üyelerine erişimi engelliyor.
* Arka uç VM'ler ya da sanal makine ölçek kümesinin örneklerini [varsayılan durumu araştırması için yanıt vermiyor](#problems-with-default-health-probe.md).
* Geçersiz veya hatalı [özel sistem durumu araştırmalarının yapılandırmasını](#problems-with-custom-health-probe.md).
* Azure uygulama ağ geçidi [arka uç havuzu yapılandırılmış ya da boş değil](#empty-backendaddresspool).
* Sanal makineler veya durumlarda hiçbiri [sanal makine ölçek kümesi sağlıklı](#unhealthy-instances-in-backendaddresspool).
* [İsteği zaman aşımı veya bağlantı sorunları](#request-time-out) kullanıcı istekleri ile.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Ağ güvenlik grubu, kullanıcı tanımlı yol veya özel DNS sorunu

### <a name="cause"></a>Nedeni

NSG, UDR veya özel DNS varlığını nedeniyle arka uç için erişim engellendi, uygulama ağ geçidi örnekleri arka uç havuzu ulaşması mümkün olmaz ve 502 hatalara neden araştırma hatalarına neden olur. NSG/UDR uygulama VM'ler dağıtıldığı uygulama ağ geçidi alt ağı veya alt ağı mevcut olabileceğini unutmayın. Benzer şekilde VNET içindeki özel DNS varlığını de FQDN arka uç havuzu üyeleri için kullanılıyorsa ve doğru şekilde kullanıcı tarafından yapılandırılmış DNS sunucusu tarafından VNET için çözümlenmez sorunlar neden olabilir.

### <a name="solution"></a>Çözüm

NSG, UDR ve DNS yapılandırması aşağıdaki adımlardan giderek doğrulama:
* Uygulama ağ geçidi alt ağ ile ilişkilendirilmiş Nsg'ler denetleyin. Arka uç iletişimi engellenmez emin olun.
* Uygulama ağ geçidi alt ağ ile ilişkilendirilmiş UDR denetleyin. UDR çıktığınızda arka uç alt ağ trafiği yönlendirerek değil olun - örneğin ağ sanal Gereçleri veya uygulama ağ geçidi alt ağına ExpressRoute/VPN aracılığıyla tanıtılan varsayılan yolların yönlendirme için denetleyin.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzureRmVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Geçerli NSG ve arka uç VM rotası denetleyin

```powershell
Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName nic1 -ResourceGroupName testrg
Get-AzureRmEffectiveRouteTable -NetworkInterfaceName nic1 -ResourceGroupName testrg
```

* VNet içinde özel DNS bulunmadığını denetleyin. DNS, çıktı VNet özelliklerinde ayrıntılarını bakarak denetlenebilir.

```json
Get-AzureRmVirtualNetwork -Name vnetName -ResourceGroupName rgName 
DhcpOptions            : {
                           "DnsServers": [
                             "x.x.x.x"
                           ]
                         }
```
Varsa, DNS sunucusu arka uç havuzu üyenin FQDN doğru çözümleyemiyorsa olduğundan emin olun.

## <a name="problems-with-default-health-probe"></a>Varsayılan durumu araştırması sorunları

### <a name="cause"></a>Nedeni

502 hataları sık göstergeleri varsayılan durumu araştırması arka uç VM'ler ulaşabilmesi değil de olabilir. Bir uygulama ağ geçidi örneği sağlandığında, varsayılan durumu araştırması BackendHttpSetting özelliklerini kullanarak her BackendAddressPool için otomatik olarak yapılandırır. Kullanıcı girişi yok, bu araştırma ayarlamak için gereklidir. Özellikle, bir Yük Dengeleme kuralı yapılandırıldığında, bir ilişki BackendHttpSetting ve BackendAddressPool arasında yapılır. Bir varsayılan araştırma her bu ilişkileri için yapılandırılır ve uygulama ağ geçidi BackendHttpSetting öğesinde belirtilen bağlantı noktasında BackendAddressPool her örnek düzenli sistem durumu denetimi bağlantı başlatır. Aşağıdaki tabloda varsayılan sistem durumu araştırma URL'siyle ilişkili değerleri listelenmektedir.

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Yoklama URL'si |http://127.0.0.1/ |URL yolu |
| Aralık |30 |Saniye cinsinden yoklama aralığı |
| Zaman aşımı |30 |Zaman aşımı saniye cinsinden araştırma |
| Sağlıksız durum eşiği. |3 |Yeniden deneme sayısı araştırma. Sağlıksız eşik arka arkaya araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenmiş. |

### <a name="solution"></a>Çözüm

* Varsayılan site yapılandırıldığından ve 127.0.0.1 dinleme emin olun.
* BackendHttpSetting 80 dışında bir bağlantı noktası belirtiyorsa, varsayılan site Bu bağlantı noktasını dinlemek üzere yapılandırılması gerekir.
* Çağrı http://127.0.0.1:port bir HTTP Sonuç kodu 200 döndürmelidir. Bu 30 saniye zaman aşımı süresi içinde döndürülmelidir.
* Yapılandırılan bağlantı noktası açık olduğundan ve güvenlik duvarı kuralları ya da yapılandırılan bağlantı noktasındaki gelen veya giden trafiği engellemek Azure ağ güvenlik grupları olduğundan emin olun.
* Azure Klasik sanal makineleri veya Bulut hizmeti FQDN veya genel IP ile kullanılırsa, karşılık gelen emin [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) açılır.
* VM Azure Resource Manager aracılığıyla yapılandırılır ve uygulama ağ geçidi dağıtıldığı, sanal ağ dışında ise [ağ güvenlik grubu](../virtual-network/security-overview.md) istenen bağlantı noktası erişime izin verecek şekilde yapılandırılması gerekir.

## <a name="problems-with-custom-health-probe"></a>Özel durum araştırması sorunları

### <a name="cause"></a>Nedeni

Özel sistem durumu araştırmalarının davranışı yoklama varsayılan ek esneklik sağlar. Özel araştırmalara kullanırken, kullanıcılar yoklama aralığı, URL ve test etmek için yolu ve arka uç havuzu örneği sağlıksız olarak işaretleme önce kabul etmek için kaç tane başarısız yanıtları yapılandırabilir. Aşağıdaki ek özellikler eklenir.

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırma adı. Bu ad, arka uç HTTP Ayarları'nda araştırma başvurmak için kullanılır. |
| Protokol |Araştırma göndermek için kullanılan protokol. Araştırma arka uç HTTP Ayarları'nda tanımlanan protokolünü kullanır |
| Host |Araştırma göndermek için ana bilgisayar adı. Yalnızca uygulama ağ geçidinde çok siteli yapılandırıldığında uygulanabilir. Bu VM ana bilgisayar adından farklıdır. |
| Yol |Araştırma göreli yolu. Geçerli yol başlar '/'. Yoklama için gönderilen \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\> |
| Aralık |Aralığı saniye cinsinden araştırma. Bu iki ardışık araştırmalar arasında zaman aralığıdır. |
| Zaman aşımı |Zaman aşımı saniye cinsinden araştırma. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmazsa, araştırma başarısız olarak işaretlenir. |
| Sağlıksız durum eşiği. |Yeniden deneme sayısı araştırma. Sağlıksız eşik arka arkaya araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenmiş. |

### <a name="solution"></a>Çözüm

Özel durumu araştırma yukarıdaki tabloda olarak doğru bir şekilde yapılandırıldığını doğrulayın. Önceki sorun giderme adımları yanı sıra, aynı zamanda aşağıdakilerden emin olun:

* Araştırma göre doğru belirtildiğinden emin olun [Kılavuzu](application-gateway-create-probe-ps.md).
* Uygulama ağ geçidi tek bir site için yapılandırılmışsa, varsayılan olarak ana bilgisayar adı '127.0.0.1' içinde özel araştırma aksi belirtilmedikçe belirtilmesi gerekir.
* Http:// çağrısına emin\<konak\>:\<bağlantı noktası\>\<yolu\> 200 bir HTTP Sonuç kodu döndürür.
* Aralık, zaman aşımı ve UnhealtyThreshold kabul edilebilir aralık içinde olduğundan emin olun.
* Bir HTTPS kullanarak araştırma, arka uç sunucusunun kendisi üzerinde bir geri dönüş sertifikası yapılandırarak arka uç sunucusuna SNI gerektirmeyen emin olun.

## <a name="request-time-out"></a>İstek zaman aşımı

### <a name="cause"></a>Nedeni

Bir kullanıcı isteği alındığında, uygulama ağ geçidi isteği yapılandırılmış kurallarını uygular ve bir arka uç havuzu örneğine yönlendirir. Arka uç örnek yanıt süresinin yapılandırılabilir bir zaman aralığı için bekler. Varsayılan olarak, bu aralığıdır **30 saniye**. Uygulama ağ geçidi, bu aralıkta arka uç uygulamasından bir yanıt alamazsa, kullanıcı isteği 502 bir hata görür.

### <a name="solution"></a>Çözüm

Uygulama ağ geçidi için farklı havuzlar uygulanabilir BackendHttpSetting aracılığıyla bu ayarı yapılandırmak kullanıcıların sağlar. Farklı arka uç havuzları farklı BackendHttpSetting ve bu nedenle farklı istek zaman aşımını yapılandırılmış olabilir.

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Boş BackendAddressPool

### <a name="cause"></a>Nedeni

Uygulama ağ geçidi hiçbir sanal makineler veya sanal makine ölçek kümesi varsa, arka uç adres havuzu yapılandırılmış herhangi bir müşteri istek yönlendiremezsiniz ve hatalı ağ geçidi hata oluşturur.

### <a name="solution"></a>Çözüm

Arka uç adres havuzu boş olmadığından emin olun. Bu da PowerShell'i, CLI veya portal aracılığıyla yapılabilir.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

Yukarıdaki cmdlet çıktısından boş arka uç adres havuzu içermelidir. Aşağıdaki iki havuzları, burada döndürülen bir örnek verilmiştir arka uç VM'ler için FQDN veya IP adresleriyle yapılandırılmış. BackendAddressPool sağlama durumunun 'başarılı' olması gerekir.

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

## <a name="unhealthy-instances-in-backendaddresspool"></a>BackendAddressPool kötü durumda

### <a name="cause"></a>Nedeni

BackendAddressPool tüm örneklerini sağlam olmaması ise, uygulama ağ geçidi kullanıcı istek için tüm arka uç bulunmaz. Arka uç örnekleri sağlıklı ancak dağıtılan gerekli uygulama yoksa olduğunda bu durum olabilir.

### <a name="solution"></a>Çözüm

Örnekleri sağlıklı olduğunu ve uygulama'nın düzgün yapılandırıldığından emin olun. Arka uç örnekleri aynı sanal ağda başka bir VM'den bir ping işlemine yanıt verebilir durumda olup olmadığını denetleyin. Ortak bir uç nokta ile yapılandırılmışsa, bir tarayıcı isteğini web uygulamasına hizmet verilebilen olduğundan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki adımlar sorunu çözmezse, açık bir [destek bileti](https://azure.microsoft.com/support/options/).

