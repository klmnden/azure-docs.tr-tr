---
title: Sanal ağ yapılandırma için bir Premium Azure Redis önbelleğini | Microsoft Docs
description: Oluşturma ve Redis örneği için Premium katman Azure Cache için sanal ağ desteğini yönetme hakkında bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: yegu
ms.openlocfilehash: d4b8fd6ccb3fc7cb2627d4bd3e103239181e4d9d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831085"
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-cache-for-redis"></a>Sanal ağ desteği için bir Premium Azure önbelleği için Redis yapılandırma
Azure önbelleği için Redis önbellek boyutunu ve özelliklerini, kümeleme, Kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri dahil olmak üzere tercih ettiğiniz esneklik sağlayan farklı bir önbellek teklifleri sahiptir. Bir sanal ağ, bulutta özel bir ağdır. Bir Azure önbelleği için Redis örneği bir VNet ile yapılandırıldığında, genel olarak adreslenebilir değildir ve yalnızca sanal makineler ve sanal ağ içindeki uygulamalardan erişilebilir. Bu makalede, Redis örneği için bir premium Azure Cache için sanal ağ desteğini yapılandırma açıklanır.

> [!NOTE]
> Azure önbelleği için Redis destekleyen her iki Klasik ve Resource Manager sanal ağları.
> 
> 

Diğer premium önbellek özelliklerini hakkında daha fazla bilgi için bkz: [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Neden Vnet'e?
[Azure sanal ağı (VNet)](https://azure.microsoft.com/services/virtual-network/) geliştirilmiş güvenlik ve yalıtım, Azure önbelleği için Redis yanı sıra alt ağlar, erişim denetim ilkeleri için dağıtım sağlar ve erişimi daha da fazla diğer özellikleri.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Sanal ağ (VNet) desteği üzerinde yapılandırılmış **yeni Azure önbelleği için Redis** önbellek oluşturma sırasında dikey. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Premium fiyatlandırma katmanının seçtikten sonra aynı abonelik ve konumdaki önbelleğinizi olan bir sanal ağ'ı seçerek Redis VNet tümleştirmesi yapılandırabilirsiniz. Yeni bir sanal ağ kullanmak için ilk adımları izleyerek oluşturun [Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/manage-virtual-network.md#create-a-virtual-network) veya [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve ardından içindönüş**Yeni Azure önbelleği için Redis** oluşturma ve premium önbelleğinizi yapılandırma dikey penceresi.

VNet yeni önbelleğiniz için yapılandırmak için tıklayın **sanal ağ** üzerinde **yeni Azure önbelleği için Redis** dikey penceresinde ve aşağı açılan listesinden istediğiniz sanal ağ seçin.

![Sanal ağ][redis-cache-vnet]

İstenen alt ağdan seçin **alt** aşağı açılan liste ve istenen belirtin **statik IP adresi**. Klasik bir VNet kullanıyorsanız **statik IP adresi** alan isteğe bağlıdır ve belirtilmezse, seçilen alt ağından bir seçilir.

> [!IMPORTANT]
> Azure Cache için Resource Manager Vnet'i Redis'e dağıtırken, önbellek başka kaynaklar dışında Azure önbelleği için Redis örneği içeren ayrılmış alt ağında olmalıdır. Azure Cache için Resource Manager Vnet'i Redis'e dağıtmak için bir girişimde diğer kaynakları içeren bir alt ağa, dağıtım başarısız olur.
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

Bir sanal ağ kullanılırken, Azure önbelleği için Redis örneği bağlanmak için aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde önbelleğinizin konak adı belirtin:

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

## <a name="azure-cache-for-redis-vnet-faq"></a>Azure önbelleği için Redis sanal ağ hakkında SSS
Aşağıdaki liste, Redis ölçeklendirmeye yönelik Azure önbelleği hakkında sık sorulan soruların yanıtlarını içerir.

* Azure Cache, Redis ve sanal ağlar ile bazı yaygın hatalı yapılandırma sorunları nelerdir?
* [Önbelleğimin sanal ağ içinde çalıştığını nasıl doğrulayabilirim?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* Uzak sertifika geçersiz bildiren bir hata neden alıyorum bir VNET'te Azure Önbelleğimin bağlanmaya çalışırken Redis?
* [Sanal ağlar bir standart veya temel önbellek ile kullanabilir miyim?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* Neden Azure önbelleği için Redis oluşturma bazı alt ağlar, ancak diğerlerini başarısız oluyor?
* [Alt ağ adres alanı gereksinimleri nelerdir?](#what-are-the-subnet-address-space-requirements)
* [Tüm önbellek özellikleri, bir sanal ağ içindeki bir önbellek barındırırken çalışıyor mu?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

### <a name="what-are-some-common-misconfiguration-issues-with-azure-cache-for-redis-and-vnets"></a>Azure Cache, Redis ve sanal ağlar ile bazı yaygın hatalı yapılandırma sorunları nelerdir?
Azure önbelleği için Redis sanal ağ içinde barındırıldığında, bağlantı noktaları aşağıdaki tablolarda kullanılır. 

>[!IMPORTANT]
>Bağlantı noktalarını aşağıdaki tablolarda engellenirse, önbellek düzgün çalışmayabilir. Bir veya daha fazla engellenen Bu bağlantı noktalarına sahip en yaygın hatalı yapılandırma Azure Cache için sanal ağ, Redis kullanırken sorunudur.
> 
> 

- [Giden bağlantı noktası gereksinimleri](#outbound-port-requirements)
- [Gelen bağlantı noktası gereksinimleri](#inbound-port-requirements)

#### <a name="outbound-port-requirements"></a>Giden bağlantı noktası gereksinimleri

Yedi giden bağlantı noktası gereksinimleri vardır.

- Tüm giden internet bağlantıları üzerinden bir istemciye yapılabilmesi için kullanıcının şirket içi denetim cihaz.
- Üç bağlantı noktalarının Azure depolama ve Azure DNS hizmeti Azure uç noktaları için trafiği yönlendirme.
- Kalan bağlantı noktası aralıkları ve dahili Redis alt ağ iletişimi için. Hiçbir alt ağda NSG kuralları iç Redis alt ağ iletişimleri için gerekli değildir.

| Bağlantı noktaları | Direction | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
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

| Bağlantı noktaları | Direction | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
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

Bir sanal ağda başlangıçta karşılanıyor değil Redis Azure Cache için ağ bağlantı gereksinimleri vardır. Azure önbelleği için Redis bir sanal ağ içinde kullanıldığında düzgün şekilde çalışabilmesi için aşağıdaki tüm öğeleri gerektirir.

* Dünya çapındaki Azure depolama uç noktalarına giden ağ bağlantısını. Bu depolama uç noktaları yer yanı sıra, Azure önbelleği için Redis örneği ile aynı bölgede bulunan uç noktaları içerir **diğer** Azure bölgeleri. Azure depolama uç noktaları aşağıdaki DNS etki alanları altında çözün: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*ve *file.core.windows.net*. 
* Giden ağ bağlantısını *ocsp.msocsp.com*, *mscrl.microsoft.com*, ve *crl.microsoft.com*. Bu bağlantı, SSL işlevselliği desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve etki alanları önceki noktaların bahsedilen çözme özelliğine sahip olması gerekir. Bu DNS gereksinimleri, geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan sağlayarak karşılanabilir.
* Aşağıdaki DNS etki alanları altında çözümlemek, aşağıdaki Azure izleme uç noktaları, giden ağ bağlantısını: shoebox2-black.shoebox2.metrics.nsatc.net Kuzey-prod2.prod2.metrics.nsatc.net azglobal black.azglobal.metrics.nsatc.net , shoebox2-red.shoebox2.metrics.nsatc.net Doğu-prod2.prod2.metrics.nsatc.net azglobal red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Önbelleğimin sanal ağ içinde çalıştığını nasıl doğrulayabilirim?

>[!IMPORTANT]
>Bir Azure Cache için sanal ağ içinde barındırılan bir Redis örneğinden bağlanırken, önbellek istemcilerinize aynı sanal ağda veya bir sanal ağ içindeki etkin VNET eşlemesi ile olmalıdır. Bu, herhangi bir test uygulamaları veya ping işlemi tanılama araçları içerir. İstemcinin ağ trafiğini Redis örneği ulaşmasına izin verilir, istemci uygulamanın barındırıldığı bağımsız olarak, ağ güvenlik grupları yapılandırılması gerekir.
>
>

Bağlantı noktası gereksinimleri önceki bölümde açıklandığı gibi yapılandırıldıktan sonra aşağıdaki adımları uygulayarak önbelleğinizi çalışıp çalışmadığını doğrulayabilirsiniz.

- [Yeniden başlatma](cache-administration.md#reboot) tüm önbellek düğümleri. Tüm gerekli önbellek bağımlılıklarını ulaşılamıyor varsa (açıklandığı gibi [gelen bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#inbound-port-requirements) ve [giden bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#outbound-port-requirements)), önbellek başarıyla yeniden başlatmanız mümkün olmayacaktır.
- Önbellek düğümleri (Azure Portalı'nda önbellek durumu tarafından bildirilen gibi) yeniden başlattıktan sonra aşağıdaki testleri gerçekleştirebilirsiniz:
  - (bağlantı noktası 6380 kullanarak) önbellek uç noktası önbelleği olarak aynı VNET içindeki bir makineden ping işlemi kullanarak [Telnet](https://www.elifulkerson.com/projects/tcping.php). Örneğin:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Varsa `tcping` aracın bildirdiği bağlantı noktasının açık olduğundan, önbellek sanal ağ içindeki istemcilerden gelen bağlantı için kullanılabilir.

  - Test etmek için başka bir yolu, bir test önbellek istemcisi oluşturmaktır (basit olabilir StackExchange.Redis kullanarak bir konsol uygulaması), önbelleğe bağlanır ve ekler ve önbellekten bazı öğeleri alır. Örnek istemci uygulama, önbellek aynı sanal AĞA ve uygulamayı önbelleğe bağlantısını doğrulamak için bir VM üzerine yükleyin.


### <a name="when-trying-to-connect-to-my-azure-cache-for-redis-in-a-vnet-why-am-i-getting-an-error-stating-the-remote-certificate-is-invalid"></a>Uzak sertifika geçersiz bildiren bir hata neden alıyorum bir VNET'te Azure Önbelleğimin bağlanmaya çalışırken Redis?

Redis bir sanal ağ için bir Azure önbelleği için bağlanmaya çalışırken bunun gibi bir sertifika doğrulama hatası görürsünüz:

`{"No connection is available to service this operation: SET mykey; The remote certificate is invalid according to the validation procedure.; …"}`

IP adresine göre konağa bağlanırken neden olabilir. Ana bilgisayar adı kullanmanızı öneririz. Diğer bir deyişle, aşağıdakini kullanın:     

`[mycachename].redis.windows.net:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

Aşağıdaki bağlantı dizesi benzer bir IP adresi kullanmaktan kaçının:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False`

DNS adı çözümlenemiyor na erişemiyorsanız, bazı istemci kitaplıkları gibi yapılandırma seçeneklerini kapsar `sslHost` StackExchange.Redis istemcisi tarafından sağlanır. Bu sertifika doğrulama için kullanılan ana bilgisayar adı geçersiz kılmanıza da olanak sağlar. Örneğin:

`10.128.2.84:6380,password=xxxxxxxxxxxxxxxxxxxx,ssl=True,abortConnect=False;sslHost=[mycachename].redis.windows.net`

### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Sanal ağlar bir standart veya temel önbellek ile kullanabilir miyim?
Sanal ağlar, yalnızca premium önbellekler ile kullanılabilir.

### <a name="why-does-creating-an-azure-cache-for-redis-fail-in-some-subnets-but-not-others"></a>Neden Azure önbelleği için Redis oluşturma bazı alt ağlar, ancak diğerlerini başarısız oluyor?
Azure Cache için Resource Manager Vnet'i Redis'e dağıtıyorsanız, önbelleği başka bir kaynak türü içeren ayrılmış bir alt ağında olmalıdır. Bir Azure önbelleği için Redis bir Resource Manager Vnet'i alt ağa dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur. Yeni bir Azure önbelleği için Redis oluşturmadan önce alt ağ içinde mevcut kaynakları silmeniz gerekir.

Yeterli IP adresi kullanılabilir olduğu sürece, birden çok kaynak klasik bir sanal ağa dağıtabilirsiniz.

### <a name="what-are-the-subnet-address-space-requirements"></a>Alt ağ adres alanı gereksinimleri nelerdir?
Azure her alt ağ içinde bazı IP adreslerini ayırır ve bu adresi kullanılamaz. Alt ağların ilk ve son IP adresleri, Azure Hizmetleri için kullanılan üç daha fazla adres birlikte protokol uyumluluğu için ayrılmıştır. Daha fazla bilgi için [bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure sanal ağ altyapısı tarafından kullanılan IP adreslerinin yanı sıra, her Redis örneği parça başına alt ağı kullanan iki IP adresi ve yük dengeleyici için bir ek IP adresi. Kümelenmemiş bir önbellek bir parça olduğu kabul edilir.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Tüm önbellek özellikleri, bir sanal ağ içindeki bir önbellek barındırırken çalışıyor mu?
Önbelleğinizi bir sanal ağın parçası olduğunda, yalnızca sanal ağ istemcilerinde önbelleğe erişebilir. Sonuç olarak, aşağıdaki önbellek yönetim özellikleri, şu anda çalışmıyor.

* Redis Konsolu - Redis Konsolu sanal ağ dışında olan yerel tarayıcınızda çalıştığından önbelleğinize bağlantı kurulamıyor.


## <a name="use-expressroute-with-azure-cache-for-redis"></a>Kullanım ExpressRoute ile bir Azure önbelleği için Redis

Müşteriler bağlanabilir bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattına sanal ağ altyapılarını böylece kendi şirket içi ağı Azure'a genişletme. 

Varsayılan olarak, yeni oluşturulan bir ExpressRoute bağlantı hattı zorlamalı tünel gerçekleştirmez (varsayılan bir yol reklam 0.0.0.0/0) bir VNET üzerinde. Sonuç olarak, doğrudan sanal ağdan giden Internet bağlantısına izin verilir ve istemci uygulamaları Azure önbelleği için Redis dahil olmak üzere diğer Azure Uç noktalara bağlanabilmesi olanağına sahip olursunuz.

Ancak yaygın müşteri kullanmak için bir yapılandırmadır zorlamalı tünel (varsayılan rota tanıtma) bunun yerine şirket içi akış giden Internet trafiğini zorlar. Bu trafik akışını giden trafik ise Azure önbellek ile bağlantı için Redis keser. ardından Azure önbelleği için Redis örneği bağımlılıkları ile iletişim kurabildiğini değil, şirket içi engellendi.

Bir (veya daha fazla) kullanıcı tanımlı yollar (Udr) tanımlamak için Azure önbelleği için Redis içeren alt ağda çözümüdür. Varsayılan yol yerine getirilmez alt özel yollar UDR tanımlar.

Mümkün olduğunda, aşağıdaki yapılandırmayı kullanın önerilir:

* ExpressRoute yapılandırması 0.0.0.0/0 tanıtır ve varsayılan olarak, tüm giden trafiği şirket içi zorla tünel oluşturur.
* Azure önbelleği için Redis içeren alt ağa uygulanan UDR, genel internet'e TCP/IP'yi trafiği için bir çalışma rotayla 0.0.0.0/0 tanımlar; Örneğin sonraki atlama türü 'İnternet' olarak ayarlanıyor.

Bu adımların etkilerini, alt düzey UDR tünel, zorlamalı ExpressRoute üzerinden bu nedenle Azure önbellekten giden Internet erişimi için Redis sağlama öncelikli olacağını ' dir.

Bir Azure önbelleği için Redis örneği için bir şirket içi bağlanma ExpressRoute kullanarak uygulama performansla ilgili nedenlerle (en iyi performansı Azure önbelleği için Redis istemcileri Azure önbelleği için Redis ile aynı bölgede olmalıdır) nedeniyle bir tipik kullanım senaryosu değil .

>[!IMPORTANT] 
>Bir UDR'de tanımlanan yollar **gerekir** ExpressRoute yapılandırması tarafından tanıtılan herhangi bir yoldan öncelikli olacak kadar spesifik olmalıdır. Aşağıdaki örnekte geniş 0.0.0.0/0 adres aralığı kullanır ve bu nedenle büyük olasılıkla yanlışlıkla daha spesifik adres aralıkları kullanan yol tanıtımları tarafından geçersiz kılınabilir.

>[!WARNING]  
>Azure önbelleği için Redis ExpressRoute yapılandırmaları ile desteklenmez, **yanlış çapraz-yolları genel eşleme yolundan özel eşleme yoluna tanıtmak**. Genel eşlemesi yapılandırılmış, olan ExpressRoute yapılandırmaları çok sayıda Microsoft Azure IP adresi aralığı için Microsoft'tan yol tanıtımları alırsınız. Bu adres aralıkları yanlış çapraz-özel eşleme yolunda tanıtılırsa, Redis örneğinin alt ağ için Azure önbellek tüm giden ağ paketlerini yanlış bir müşterinin şirket içi ağ için zorlamalı sonucudur Altyapı. Bu ağ akışı, Azure önbelleği için Redis keser. Bu sorunun çözümü, genel eşleme yolundan özel eşleme yoluna çapraz tanıtımını durdurmaktır yolları önlemektir.


Kullanıcı tanımlı yollar hakkında arka plan bilgileri bu kullanılabilir [genel bakış](../virtual-network/virtual-networks-udr-overview.md).

ExpressRoute hakkında daha fazla bilgi için bkz. [Expressroute'a teknik genel bakış](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerini kullanmayı öğrenin.

* [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

