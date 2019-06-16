---
title: İzlemeye genel bakış için Azure Application Gateway durumu
description: Azure Application Gateway izleme özellikleri hakkında bilgi edinin
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 8/6/2018
ms.author: victorh
ms.openlocfilehash: d0c425bcb9961fde9fb319991148c18c6a9ff57b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66135206"
---
# <a name="application-gateway-health-monitoring-overview"></a>Uygulama ağ geçidi sistem durumu izlemeye genel bakış

Varsayılan olarak Azure Application Gateway, arka uç havuzunda tüm kaynakların izler ve herhangi bir kaynak havuzundan iyi durumda olmadığı kabul otomatik olarak kaldırır. Application Gateway, iyi durumda olmayan örnekler izlemeye devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmaları yanıt sonra bunları sağlıklı arka uç havuzuna ekler. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir. Bu yapılandırma, araştırma müşteriler arka ucuna bağlanmak için kullanılmasını aynı bağlantı noktasını sınıyor sağlar.

![Uygulama ağ geçidi araştırma örneği][1]

Varsayılan sistem durumu izleme yoklaması kullanmanın yanı sıra durum yoklaması, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Bu makalede, hem varsayılan hem de özel sistem durumu araştırmaları ele alınmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Varsayılan durum araştırması

Özel araştırma yapılandırmaların ayarlamazsanız, bir uygulama ağ geçidi varsayılan durum araştırması otomatik olarak yapılandırır. Davranış izleme, arka uç havuzu için yapılandırılmış IP adresleri için bir HTTP isteği yaparak çalışır. Arka uç http ayarları, HTTPS için yapılandırılmış olması halinde varsayılan araştırmaları için araştırma de arka uçları durumunu test etmek için HTTPS kullanır.

Örneğin: Uygulama ağ geçidinizin bağlantı noktası 80 üzerinde HTTP ağ trafiği almak için arka uç sunucularının A, B ve C kullanmak için yapılandırın. Varsayılan sistem durumu izlemeyi her 30 saniyede sağlıklı bir HTTP yanıtı için üç sunucu sınar. Sağlıklı bir HTTP yanıtı sahip bir [durum kodu](https://msdn.microsoft.com/library/aa287675.aspx) 200 399 arasındaki.

Bir sunucu için varsayılan araştırma denetimi başarısız olursa, application gateway, arka uç havuzundan kaldırır ve bu sunucuya giden ağ trafiğini durdurur. Varsayılan araştırma, yine de her 30 saniyede bir sunucu için denetlemeye devam eder. Sunucu varsayılan durum araştırması başarıyla bir isteğe yanıt verdiğinde, bu geri sağlıklı olarak arka uç havuzuna eklenir ve sunucuya yeniden akan trafiği başlatır.

### <a name="probe-matching"></a>Eşleşen araştırma

Varsayılan olarak, bir HTTP (S) yanıt durum kodu 200 399 arasındaki sağlıklı olarak değerlendirilir. Özel sistem durumu araştırmaları, ayrıca iki eşleştirme ölçütü destekler. Ölçütlerle eşleşen isteğe bağlı olarak sağlıklı bir yanıt nelerden varsayılan yorumlama değiştirmek için kullanılabilir.

Şu ölçütlerle eşleşen: 

- **HTTP yanıtı durum kodu eşleşme** - araştırma eşleştirme ölçütü kabul etmek için kullanıcı belirtilen http yanıt kodu veya yanıt kodu aralığı. Virgülle ayrılmış tek bir yanıt durum kodları veya durum kodu aralığı desteklenir.
- **HTTP yanıt gövdesi eşleşme** - eşleştirme ölçütü HTTP yanıt gövdesinde ve bir eşleşme bakar dizesi belirtilen araştırma. Kullanıcı tarafından belirtilen varlığını yalnızca eşleşme arar, yanıt gövdesi içinde dize ve tam normal ifade eşleştirmesi değildir.

Eşleşme ölçütlerini kullanılarak belirtilebilir `New-AzApplicationGatewayProbeHealthResponseMatch` cmdlet'i.

Örneğin:

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Eşleşme ölçütlerini belirtilen sonra bu yapılandırmayı kullanarak araştırma iliştirilebilir bir `-Match` PowerShell parametresi.

### <a name="default-health-probe-settings"></a>Varsayılan durum yoklama ayarları

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Araştırma URL'si |http://127.0.0.1:\<port\>/ |URL yolu |
| Interval |30 |Sonraki durum araştırması önce beklenecek saniye cinsinden süreyi gönderilir.|
| Zaman aşımı |30 |Süreyi saniye cinsinden, uygulama ağ geçidi araştırma sağlıksız olarak işaretlemek için bir araştırma yanıt bekler. Bir araştırma sağlıklı olarak döndürürse, karşılık gelen arka uç hemen sağlıklı olarak işaretlenir.|
| Sağlıksız durum eşiği |3 |Kaç yoktur normal durum yoklaması bir arıza durumunda gönderilecek araştırmaları yönetir. Bu ek sistem durumu araştırmaları, sayfayı hızlı bir şekilde arka uç durumunu hızlı bir şekilde belirlemek için gönderilir ve araştırma aralığı için beklemez. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

> [!NOTE]
> Bağlantı noktasını arka uç HTTP ayarları olarak aynı bağlantı noktasıdır.

Varsayılan araştırma yalnızca http arar:\//127.0.0.1:\<bağlantı noktası\> sistem durumunu belirlemek için. Bir özel URL'ye gidin veya diğer herhangi bir ayarı değiştirmek için sistem durumu araştırma yapılandırmanız gerekiyorsa, özel araştırmalar kullanmanız gerekir.

### <a name="probe-intervals"></a>Araştırma aralığı

Birbirinden bağımsız arka uç uygulama ağ geçidi tüm örneklerini yoklama. Her bir uygulama ağ geçidi örneğine aynı araştırma yapılandırması uygular. Örneğin, araştırma yapılandırması sistem durumu araştırmaları her 30 saniyede göndermek ve uygulama ağ geçidinin iki örnek olduğuna her iki örnek her 30 saniyede durum yoklaması gönderir.

Ayrıca birden çok dinleyici varsa, her dinleyici birbirinden bağımsız arka uç araştırmaları. İki dinleyici aynı arka uç havuzuna (iki arka uç http ayarları tarafından yapılandırılır) iki farklı noktalarında işaret eden varsa, ardından her dinleyici aynı arka uç bağımsız olarak araştırmaları. Bu durumda, her uygulama ağ geçidi örneğinden iki araştırmaları iki dinleyici için de vardır. Bu senaryoda uygulama ağ geçidinin iki örneği varsa, arka uç sanal makine yapılandırılmış yoklama aralığı başına dört araştırmaları görür.

## <a name="custom-health-probe"></a>Özel durum araştırması

Özel araştırmalar sistem durumu izleme daha ayrıntılı denetime sahip olmanızı sağlar. Özel araştırmalar kullanırken, araştırma aralığı, URL ve test etmek için yolu ve arka uç havuzu örnek sağlıksız olarak işaretleme önce kabul etmek için kaç başarısız yanıtları yapılandırabilirsiniz.

### <a name="custom-health-probe-settings"></a>Özel durum yoklama ayarları

Aşağıdaki tabloda, bir özel durum araştırması özelliklerini tanımlarını sağlar.

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırma adı. Bu ad, arka uç HTTP ayarlarında araştırma başvurmak için kullanılır. |
| Protocol |Araştırma göndermek için kullanılan protokol. Araştırma arka uç HTTP Ayarları'nda tanımlanan protokolünü kullanır. |
| Ana bilgisayar |Araştırma göndermek için ana bilgisayar adı. Geçerli çok siteli, yalnızca uygulama ağ geçidinde yapılandırılan, aksi takdirde '127.0.0.1' kullanın. Bu değer, VM'nin ana bilgisayar adından farklıdır. |
| `Path` |Araştırma göreli yolu. Geçerli yol başlatılır '/'. |
| Interval |Aralık saniye cinsinden araştırma. İki ardışık araştırmaları arasındaki zaman aralığını değerdir. |
| Zaman aşımı |Zaman aşımını saniye cinsinden araştırma. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmazsa, araştırma başarısız olarak işaretlenir.  |
| Sağlıksız durum eşiği |Yeniden deneme sayısı araştırma. Sağlıksız durum eşiği ardışık araştırma hatası sayısı ulaştıktan sonra arka uç sunucu işaretlenir. |

> [!IMPORTANT]
> Application Gateway için tek bir site yapılandırdıysanız, varsayılan olarak ana bilgisayar adı '127.0.0.1' özel araştırma aksi şekilde yapılandırılmadıkça belirtilmelidir.
> Özel bir araştırma gönderilir başvurusunu \<Protokolü\>://\<konak\>:\<bağlantı noktası\>\<yolu\>. Kullanılan bağlantı noktasını arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası olacaktır.

## <a name="nsg-considerations"></a>NSG konuları

Bir uygulama ağ geçidi alt ağı üzerinde ağ güvenlik grubu (NSG) varsa, bağlantı noktası aralıkları 65503 65534 uygulama ağ geçidi alt ağının gelen trafik için açılmalıdır. Bu bağlantı noktaları, arka uç sistem durumu çalışmak üzere API için gereklidir.

Buna ek olarak, giden Internet bağlantısı engellenemez ve AzureLoadBalancer etiketini Yakında gelen trafiğe izin verilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama ağ geçidi sistem durumu izleme hakkında daha fazla edindikten sonra yapılandırdığınız bir [özel durum araştırması](application-gateway-create-probe-portal.md) Azure portalında veya [özel durum araştırması](application-gateway-create-probe-ps.md) PowerShell ve Azure Resource Manager kullanarak dağıtım modeli.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
