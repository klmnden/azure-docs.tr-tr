---
title: Premium Azure Redis Cache için sanal ağ yapılandırma | Microsoft Docs
description: Oluşturma ve yönetme, Premium katman Azure Redis Cache örnekleri için sanal ağ desteği hakkında bilgi edinin
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: wesmc
ms.openlocfilehash: 250c66c3a39519a6eddc1ecb51259ec1944c88a9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38671131"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Premium Azure Redis Cache için sanal ağ desteğini yapılandırma
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini, kümeleme, Kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri dahil olmak üzere tercih ettiğiniz esneklik sağlayan farklı bir önbellek teklifleri sahiptir. Bir sanal ağ, bulutta özel bir ağdır. Azure Redis Cache örneğine bir VNet ile yapılandırıldığında, genel olarak adreslenebilir değildir ve yalnızca sanal makineler ve sanal ağ içindeki uygulamalardan erişilebilir. Bu makalede, bir premium Azure Redis Cache örneği için sanal ağ desteğini yapılandırma açıklanır.

> [!NOTE]
> Azure Redis Cache destekleyen her iki Klasik ve Resource Manager sanal ağları.
> 
> 

Diğer premium önbellek özelliklerini hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Neden Vnet'e?
[Azure sanal ağı (VNet)](https://azure.microsoft.com/services/virtual-network/) dağıtım geliştirilmiş güvenlik ve yalıtım için Azure Redis Cache yanı sıra alt ağlar, erişim denetim ilkeleri sağlar ve diğer özellikleri daha da fazla erişimi.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Sanal ağ (VNet) desteği üzerinde yapılandırılmış **yeni Redis Cache** önbellek oluşturma sırasında dikey. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Premium fiyatlandırma katmanının seçtikten sonra aynı abonelik ve konumdaki önbelleğinizi olan bir sanal ağ'ı seçerek Redis VNet tümleştirmesi yapılandırabilirsiniz. Yeni bir sanal ağ kullanmak için ilk adımları izleyerek oluşturun [Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network) veya [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve ardından içindönüş**Yeni Redis Cache** oluşturma ve premium önbelleğinizi yapılandırma dikey penceresi.

VNet yeni önbelleğiniz için yapılandırmak için tıklayın **sanal ağ** üzerinde **yeni Redis Cache** dikey penceresinde ve aşağı açılan listesinden istediğiniz sanal ağ seçin.

![Sanal ağ][redis-cache-vnet]

İstenen alt ağdan seçin **alt** aşağı açılan liste ve istenen belirtin **statik IP adresi**. Klasik bir VNet kullanıyorsanız **statik IP adresi** alan isteğe bağlıdır ve belirtilmezse, seçilen alt ağından bir seçilir.

> [!IMPORTANT]
> Azure Redis Cache için Resource Manager Vnet'i dağıtırken, önbellek Azure Redis Cache örneğinin dışında başka kaynaklar içeren ayrılmış alt ağında olmalıdır. Azure Redis Cache bir alt ağ için bir Resource Manager Vnet'i dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur.
> 
> 

![Sanal ağ][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure her alt ağ içinde bazı IP adreslerini ayırır ve bu adresi kullanılamaz. Alt ağların ilk ve son IP adresleri, Azure Hizmetleri için kullanılan üç daha fazla adres birlikte protokol uyumluluğu için ayrılmıştır. Daha fazla bilgi için [bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Azure sanal ağ altyapısı tarafından kullanılan IP adreslerinin yanı sıra, her Redis örneği parça başına alt ağı kullanan iki IP adresi ve yük dengeleyici için bir ek IP adresi. Kümelenmemiş bir önbellek bir parça olduğu kabul edilir.
> 
> 

Önbellek oluşturulduktan sonra VNet yapılandırmasını tıklayarak görüntüleyebilirsiniz **sanal ağ** gelen **kaynak menüsünde**.

![Sanal ağ][redis-cache-vnet-info]

Bir sanal ağ kullanılırken, Azure Redis cache örneğine bağlanmak için aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde önbelleğinizin konak adı belirtin:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis Cache sanal ağ hakkında SSS
Aşağıdaki liste, Azure Redis Cache ölçeklendirme hakkında sık sorulan soruların yanıtlarını içerir.

* [Azure Redis Cache ve sanal ağlar ile bazı yaygın hatalı yapılandırma sorunları nelerdir?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Önbelleğimin sanal ağ içinde çalıştığını nasıl doğrulayabilirim?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [My Redis önbelleğinde bir sanal AĞA bağlanmaya çalışırken uzak sertifika geçersiz bildiren bir hata neden alıyorum?](#when-trying-to-connect-to-my-redis-cache-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid)
* [Sanal ağlar bir standart veya temel önbellek ile kullanabilir miyim?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Neden Redis önbelleği oluşturma bazı alt ağlar, ancak diğerlerini başarısız oluyor?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Alt ağ adres alanı gereksinimleri nelerdir?](#what-are-the-subnet-address-space-requirements)
* [Tüm önbellek özellikleri, bir sanal ağ içindeki bir önbellek barındırırken çalışıyor mu?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Azure Redis Cache ve sanal ağlar ile bazı yaygın hatalı yapılandırma sorunları nelerdir?
Azure Redis Cache sanal ağ içinde barındırıldığında, bağlantı noktaları aşağıdaki tablolarda kullanılır. 

>[!IMPORTANT]
>Bağlantı noktalarını aşağıdaki tablolarda engellenirse, önbellek düzgün çalışmayabilir. Bir veya daha fazla engellenen Bu bağlantı noktalarına sahip bir vnet'te Azure Redis Cache kullanırken en yaygın hatalı yapılandırma sorunu olduğunu.
> 
> 

- [Giden bağlantı noktası gereksinimleri](#outbound-port-requirements)
- [Gelen bağlantı noktası gereksinimleri](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Giden bağlantı noktası gereksinimleri

Yedi giden bağlantı noktası gereksinimleri vardır.

- İstenen, tüm giden bağlantılar internet üzerinden yapılan, istemcinin denetim cihaz şirket içi.
- Üç bağlantı noktalarının Azure depolama ve Azure DNS hizmeti Azure uç noktaları için trafiği yönlendirme.
- Kalan bağlantı noktası aralıkları ve dahili Redis alt ağ iletişimi için. Hiçbir alt ağda NSG kuralları iç Redis alt ağ iletişimleri için gerekli değildir.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Giden |TCP |Redis bağımlılıklar üzerinde Azure depolama/PKI (Internet) | (Alt ağ redis) |* |
| 53 |Giden |TCP/UDP |DNS (Internet/VNet) bağımlılıkları redis | (Alt ağ redis) |* |
| 8443 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) | (Alt ağ redis) |
| 10221-10231 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) | (Alt ağ redis) |
| 20226 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |
| 13000-13999 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |
| 15000-15999 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |
| 6379-6380 |Giden |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |


#### <a name="inbound-port-requirements"></a>Gelen bağlantı noktası gereksinimleri

Sekiz gelen bağlantı noktası aralığı gereksinimi yoktur. Bu aralıklardaki gelen istekler, aynı sanal ağda barındırılan diğer hizmetlerden gelen veya iç Redis alt ağ iletişimi için.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Gelen |TCP |Redis, Azure Yük Dengeleme istemci iletişimi | (Alt ağ redis) | (Redis. alt ağ), sanal ağ, Azure Load Balancer |
| 8443 |Gelen |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |
| 8500 |Gelen |TCP/UDP |Azure Yük Dengeleme | (Alt ağ redis) |Azure Load Balancer |
| 10221-10231 |Gelen |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Redis. alt ağ), Azure Load Balancer |
| 13000-13999 |Gelen |TCP |İstemci iletişimi için Redis kümeleri, Azure Yük Dengeleme | (Alt ağ redis) |Sanal ağ, Azure yük dengeleyici |
| 15000-15999 |Gelen |TCP |İstemci iletişimi Redis kümeleri için Azure Yük Dengeleme | (Alt ağ redis) |Sanal ağ, Azure yük dengeleyici |
| 16001 |Gelen |TCP/UDP |Azure Yük Dengeleme | (Alt ağ redis) |Azure Load Balancer |
| 20226 |Gelen |TCP |Redis için iç iletişimler | (Alt ağ redis) |(Alt ağ redis) |

#### <a name="additional-vnet-network-connectivity-requirements"></a>Ek sanal ağ ağ bağlantı gereksinimleri

Azure Redis Cache sanal ağ içinde başlangıçta karşılanıyor değil için ağ bağlantı gereksinimleri vardır. Azure Redis Cache sanal ağ içinde kullanıldığında düzgün şekilde çalışabilmesi için aşağıdaki tüm öğeleri gerektirir.

* Dünya çapındaki Azure depolama uç noktalarına giden ağ bağlantısını. Bu depolama uç noktaları yer yanı sıra, Azure Redis Cache örneği aynı bölgede bulunan uç noktaları içerir **diğer** Azure bölgeleri. Azure depolama uç noktaları aşağıdaki DNS etki alanları altında çözün: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*ve *file.core.windows.net*. 
* Giden ağ bağlantısını *ocsp.msocsp.com*, *mscrl.microsoft.com*, ve *crl.microsoft.com*. Bu bağlantı, SSL işlevselliği desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve etki alanları önceki noktaların bahsedilen çözme özelliğine sahip olması gerekir. Bu DNS gereksinimleri, geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan sağlayarak karşılanabilir.
* Aşağıdaki DNS etki alanları altında çözümlemek, aşağıdaki Azure izleme uç noktaları, giden ağ bağlantısını: shoebox2-black.shoebox2.metrics.nsatc.net Kuzey-prod2.prod2.metrics.nsatc.net azglobal black.azglobal.metrics.nsatc.net , shoebox2-red.shoebox2.metrics.nsatc.net Doğu-prod2.prod2.metrics.nsatc.net azglobal red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Önbelleğimin sanal ağ içinde çalıştığını nasıl doğrulayabilirim?

>[!IMPORTANT]
>Sanal ağ içinde barındırılan bir Azure Redis Cache örneğine bağlanırken, önbellek istemcilerinize aynı sanal ağda veya bir sanal ağ içindeki etkin VNET eşlemesi ile olmalıdır. Bu, herhangi bir test uygulamaları veya ping işlemi tanılama araçları içerir. İstemcinin ağ trafiğini Redis örneği ulaşmasına izin verilir, istemci uygulamanın barındırıldığı bağımsız olarak, ağ güvenlik grupları yapılandırılması gerekir.
>
>

Bağlantı noktası gereksinimleri önceki bölümde açıklandığı gibi yapılandırıldıktan sonra aşağıdaki adımları uygulayarak önbelleğinizi çalışıp çalışmadığını doğrulayabilirsiniz.

- [Yeniden başlatma](cache-administration.md#reboot) tüm önbellek düğümleri. Tüm gerekli önbellek bağımlılıklarını ulaşılamıyor varsa (açıklandığı gibi [gelen bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#inbound-port-requirements) ve [giden bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#outbound-port-requirements)), önbellek başarıyla yeniden başlatmanız mümkün olmayacaktır.
- Önbellek düğümleri (Azure Portalı'nda önbellek durumu tarafından bildirilen gibi) yeniden başlattıktan sonra aşağıdaki testleri gerçekleştirebilirsiniz:
  - (bağlantı noktası 6380 kullanarak) önbellek uç noktası önbelleği olarak aynı VNET içindeki bir makineden ping işlemi kullanarak [Telnet](https://www.elifulkerson.com/projects/tcping.php). Örneğin:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Varsa `tcping` aracın bildirdiği bağlantı noktasının açık olduğundan, önbellek sanal ağ içindeki istemcilerden gelen bağlantı için kullanılabilir.

  - Test etmek için başka bir yolu, bir test önbellek istemcisi oluşturmaktır (basit olabilir StackExchange.Redis kullanarak bir konsol uygulaması), önbelleğe bağlanır ve ekler ve önbellekten bazı öğeleri alır. Örnek istemci uygulama, önbellek aynı sanal AĞA ve uygulamayı önbelleğe bağlantısını doğrulamak için bir VM üzerine yükleyin.


### <a name="when-trying-to-connect-to-my-redis-cache-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>My Redis önbelleğinde bir sanal AĞA bağlanmaya çalışırken uzak sertifika geçersiz bildiren bir hata neden alıyorum?

Bir sanal ağda bir Redis önbelleğine bağlantı kurmaya çalıştığında, bunun gibi bir sertifika doğrulama hatası görürsünüz:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

IP adresine göre konağa bağlanırken neden olabilir. Ana bilgisayar adı kullanmanızı öneririz. Diğer bir deyişle, aşağıdakini kullanın:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Aşağıdaki bağlantı dizesi benzer bir IP adresi kullanmaktan kaçının:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

DNS adı çözümlenemiyor na erişemiyorsanız, bazı istemci kitaplıkları gibi yapılandırma seçeneklerini kapsar `sslHost` StackExchange.Redis istemcisi tarafından sağlanır. Bu sertifika doğrulama için kullanılan ana bilgisayar adı geçersiz kılmanıza da olanak sağlar. Örneğin:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Sanal ağlar bir standart veya temel önbellek ile kullanabilir miyim?
Sanal ağlar, yalnızca premium önbellekler ile kullanılabilir.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Neden Redis önbelleği oluşturma bazı alt ağlar, ancak diğerlerini başarısız oluyor?
Bir Resource Manager sanal ağı için bir Azure Redis Cache'e dağıtıyorsanız, önbelleği başka bir kaynak türü içeren bir ayrılmış alt ağında olmalıdır. Bir Resource Manager sanal ağ alt ağı için bir Azure Redis Cache dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur. Yeni bir Redis önbelleği oluşturmadan önce alt ağ içinde mevcut kaynakları silmeniz gerekir.

Yeterli IP adresi kullanılabilir olduğu sürece, birden çok kaynak klasik bir sanal ağa dağıtabilirsiniz.

### <a name="what-are-the-subnet-address-space-requirements"></a>Alt ağ adres alanı gereksinimleri nelerdir?
Azure her alt ağ içinde bazı IP adreslerini ayırır ve bu adresi kullanılamaz. Alt ağların ilk ve son IP adresleri, Azure Hizmetleri için kullanılan üç daha fazla adres birlikte protokol uyumluluğu için ayrılmıştır. Daha fazla bilgi için [bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure sanal ağ altyapısı tarafından kullanılan IP adreslerinin yanı sıra, her Redis örneği parça başına alt ağı kullanan iki IP adresi ve yük dengeleyici için bir ek IP adresi. Kümelenmemiş bir önbellek bir parça olduğu kabul edilir.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Tüm önbellek özellikleri, bir sanal ağ içindeki bir önbellek barındırırken çalışıyor mu?
Önbelleğinizi bir sanal ağın parçası olduğunda, yalnızca sanal ağ istemcilerinde önbelleğe erişebilir. Sonuç olarak, aşağıdaki önbellek yönetim özellikleri, şu anda çalışmıyor.

* Redis Konsolu - Redis Konsolu sanal ağ dışında olan yerel tarayıcınızda çalıştığından önbelleğinize bağlantı kurulamıyor.


## <a name="use-expressroute-with-azure-redis-cache"></a>ExpressRoute ile Azure Redis önbelleği kullanmalısınız?

Müşteriler bağlanabilir bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattına sanal ağ altyapılarını böylece kendi şirket içi ağı Azure'a genişletme. 

Varsayılan olarak, yeni oluşturulan bir ExpressRoute bağlantı hattı zorlamalı tünel gerçekleştirmez (varsayılan bir yol reklam 0.0.0.0/0) bir VNET üzerinde. Sonuç olarak, doğrudan sanal ağdan giden Internet bağlantısına izin verilir ve istemci uygulamaları Azure Redis Cache de dahil olmak üzere diğer Azure Uç noktalara bağlanmak olanağına sahip olursunuz.

Ancak yaygın müşteri kullanmak için bir yapılandırmadır zorlamalı tünel (varsayılan rota tanıtma) bunun yerine şirket içi akış giden Internet trafiğini zorlar. Bu trafik akışını giden trafik ise Azure Redis Cache ile bağlantısını keser. ardından Azure Redis Cache örneği bağımlılıkları ile iletişim kurabildiğini değil, şirket içi engellendi.

Azure Redis Cache içeren alt ağda bir (veya daha fazla) kullanıcı tanımlı yollar (Udr) tanımlamak için kullanılan çözümüdür. Varsayılan yol yerine getirilmez alt özel yollar UDR tanımlar.

Mümkün olduğunda, aşağıdaki yapılandırmayı kullanın önerilir:

* ExpressRoute yapılandırması 0.0.0.0/0 tanıtır ve varsayılan olarak, tüm giden trafiği şirket içi zorla tünel oluşturur.
* Azure Redis Cache içeren alt ağa uygulanan UDR, genel internet'e TCP/IP'yi trafiği için bir çalışma rotayla 0.0.0.0/0 tanımlar; Örneğin sonraki atlama türü 'İnternet' olarak ayarlanıyor.

Bu adımların etkilerini, alt düzey UDR tünel, zorlamalı ExpressRoute üzerinden bu nedenle Azure Redis Cache'e gelen giden Internet erişimini sağlama öncelikli olacağını ' dir.

ExpressRoute kullanarak bir şirket içi uygulamasından bir Azure Redis Cache örneğine bağlanma, performans nedenleriyle (en iyi performans Azure Redis Cache istemciler, Azure Redis Cache ile aynı bölgede olmalıdır) nedeniyle bir tipik kullanım senaryosu değil.

>[!IMPORTANT] 
>Bir UDR'de tanımlanan yollar **gerekir** ExpressRoute yapılandırması tarafından tanıtılan herhangi bir yoldan öncelikli olacak kadar spesifik olmalıdır. Aşağıdaki örnekte geniş 0.0.0.0/0 adres aralığı kullanır ve bu nedenle büyük olasılıkla yanlışlıkla daha spesifik adres aralıkları kullanan yol tanıtımları tarafından geçersiz kılınabilir.

>[!WARNING]  
>Azure Redis Cache ile ExpressRoute yapılandırmaları desteklenmez, **yanlış çapraz-yolları genel eşleme yolundan özel eşleme yoluna tanıtmak**. Genel eşlemesi yapılandırılmış, olan ExpressRoute yapılandırmaları çok sayıda Microsoft Azure IP adresi aralığı için Microsoft'tan yol tanıtımları alırsınız. Bu adres aralıkları yanlış çapraz-özel eşleme yolunda tanıtılırsa, giden tüm ağ paketleri Azure Redis Cache örneğinin alt ağdan hatalı bir müşterinin şirket içi ağ altyapısı için zorlamalı sonucudur . Bu ağ akışı, Azure Redis Cache keser. Bu sorunun çözümü, genel eşleme yolundan özel eşleme yoluna çapraz tanıtımını durdurmaktır yolları önlemektir.


Kullanıcı tanımlı yollar hakkında arka plan bilgileri bu kullanılabilir [genel bakış](../virtual-network/virtual-networks-udr-overview.md).

ExpressRoute hakkında daha fazla bilgi için bkz. [Expressroute'a teknik genel bakış](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerini kullanmayı öğrenin.

* [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

