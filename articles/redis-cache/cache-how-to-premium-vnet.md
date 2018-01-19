---
title: "Premium Azure Redis önbelleği için sanal ağ yapılandırma | Microsoft Docs"
description: "Oluşturma ve Premium katmanı Azure Redis önbelleği örnekleri için sanal ağ desteğini yönetme hakkında bilgi edinin"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: wesmc
ms.openlocfilehash: 74ec104bebec2004a8b7116865c2394c02b12638
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Premium Azure Redis önbelleği için sanal ağ desteğini yapılandırma
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini, kümeleme, sürdürme ve sanal ağ desteği gibi Premium katmanı özellikleri dahil olmak üzere seçimi esneklik sağlayan farklı önbellek teklifleri vardır. Bir sanal ağ bulutta özel bir ağdır. Bir Azure Redis önbelleği örneğine sahip bir VNet yapılandırıldığında, genel olarak adreslenebilir değildir ve yalnızca sanal makineler ve sanal ağ içinden uygulamalardan erişilebilir. Bu makalede, premium Azure Redis önbelleği örneği için sanal ağ desteğini yapılandırma açıklar.

> [!NOTE]
> Azure Redis önbelleği destekleyen her iki Klasik ve Resource Manager sanal ağlar.
> 
> 

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Neden VNet?
[Azure sanal ağı (VNet)](https://azure.microsoft.com/services/virtual-network/) Gelişmiş Güvenlik ve yalıtım, Azure Redis önbelleği yanı sıra alt ağlar, erişim denetimi ilkeleri için dağıtım sağlar ve diğer özellikleri daha da fazla erişimi kısıtlayabilirsiniz.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Sanal ağ (VNet) desteği üzerinde yapılandırılmış **yeni Redis önbelleği** dikey önbellek oluşturma sırasında. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Premium fiyatlandırma katmanına seçtikten sonra aynı abonelik ve konum önbelleğiniz olarak bir sanal ağı seçerek Redis VNet tümleştirme yapılandırabilirsiniz. Yeni bir VNet kullanmak için önce içindeki adımları izleyerek oluşturun [Azure portalını kullanarak bir sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) veya [Azure portalını kullanarak bir sanal ağ (Klasik) oluşturmak](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) ve daha sonra geri dönüp **yeni Redis önbelleği** oluşturmak ve premium önbelleğiniz yapılandırmak için dikey.

VNet yeni önbelleğiniz için yapılandırmak için tıklatın **sanal ağ** üzerinde **yeni Redis önbelleği** dikey ve istenen VNet aşağı açılan listeden seçin.

![Sanal ağ][redis-cache-vnet]

İstenen alt ağ üzerinden seçin **alt** aşağı açılan listesinde ve istenen belirtin **statik IP adresi**. Klasik bir VNet kullanıyorsanız **statik IP adresi** alan isteğe bağlıdır ve belirtilmemişse, bir seçilen alt ağından seçilir.

> [!IMPORTANT]
> Bir Azure Redis önbelleği için Resource Manager Vnet'i dağıtırken, önbellek Azure Redis önbelleği örnekleri dışında başka kaynaklar içeren özel bir alt ağda olmalıdır. Bir Azure Redis önbelleği bir alt ağı için Resource Manager Vnet'i dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur.
> 
> 

![Sanal ağ][redis-cache-vnet-ip]

> [!IMPORTANT]
> Her alt ağ içindeki bazı IP adreslerini Azure ayırır ve bu adresleri kullanılamaz. Alt ağlar ilk ve son IP adreslerini Azure Hizmetleri için kullanılan üç daha fazla adres birlikte Protokolü uyum için ayrılmıştır. Daha fazla bilgi için bkz: [bu alt ağ içindeki IP adresleri kullanma kısıtlamaları vardır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> Azure sanal ağ altyapısı tarafından kullanılan IP adreslerinin yanı sıra, her Redis örnek parça her alt ağ kullanan iki IP adresi ve yük dengeleyici için bir ek IP adresi. Kümelenmemiş bir önbellek, bir parça olduğu kabul edilir.
> 
> 

Önbellek oluşturulduktan sonra sanal ağ yapılandırması tıklayarak görüntüleyebilirsiniz **sanal ağ** gelen **kaynak menü**.

![Sanal ağ][redis-cache-vnet-info]

Bir sanal ağ kullanırken, Azure Redis önbelleği örneğine bağlanmak için aşağıdaki örnekte gösterildiği gibi bağlantı dizesindeki önbelleğiniz ana bilgisayar adını belirtin:

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

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis önbelleği sanal ağ ile ilgili SSS
Aşağıdaki listede, Azure Redis önbelleği ölçeklendirme hakkında sık sorulan soruların yanıtlarını içerir.

* [Azure Redis önbelleği ve Vnet ile ilgili bazı ortak yetersizliğini sorunları nelerdir?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [My önbelleği sanal ağ içinde çalıştığını nasıl doğrulayabilirsiniz?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Sanal ağlar ile standart ya da temel bir önbellek kullanabilir miyim?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Neden Redis önbelleği oluşturma bazı alt ağların ancak diğer başarısız oluyor?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Alt ağ adres alanı gereksinimleri nelerdir?](#what-are-the-subnet-address-space-requirements)
* [Tüm önbellek özellikleri, bir VNET önbelleğinde barındırdığında çalışıyor mu?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Azure Redis önbelleği ve Vnet ile ilgili bazı ortak yetersizliğini sorunları nelerdir?
Azure Redis önbelleği sanal ağ içinde barındırıldığında, aşağıdaki tablolarda bağlantı noktaları kullanılır. 

>[!IMPORTANT]
>Aşağıdaki tablolarda bağlantı noktaları engellenirse, önbellek düzgün çalışmayabilir. Bir veya daha fazla engellenen Bu bağlantı noktalarına sahip en yaygın yetersizliğini Azure Redis Önbelleği'nde bir sanal ağ kullanırken bir sorundur.
> 
> 

- [Giden bağlantı noktası gereksinimleri](#outbound-port-requirements)
- [Gelen bağlantı noktası gereksinimleri](#inbound-port-requirements)

### <a name="outbound-port-requirements"></a>Giden bağlantı noktası gereksinimleri

Yedi giden bağlantı noktası gereksinimleri vardır.

- İstenen, tüm giden bağlantıları Internet üzerinden yapılan, istemcinin denetim cihaz şirket içi.
- Üç bağlantı noktalarının Azure Storage ve Azure DNS hizmeti Azure uç noktaları için trafiği yönlendirmek.
- Kalan bağlantı noktası aralıklarını ve iç Redis alt ağ iletişimleri için. Hiçbir alt ağ NSG kuralları iç Redis alt ağ iletişimleri için gerekli değildir.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Giden |TCP |Bağımlılıklar üzerinde Azure depolama/PKI (Internet) redis | (Alt ağ redis) |* |
| 53 |Giden |TCP/UDP |DNS (Internet/VNet) bağımlılıkları redis | (Alt ağ redis) |* |
| 8443 |Giden |TCP |Redis iç iletişiminde | (Alt ağ redis) | (Alt ağ redis) |
| 10221-10231 |Giden |TCP |Redis iç iletişiminde | (Alt ağ redis) | (Alt ağ redis) |
| 20226 |Giden |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Alt ağ redis) |
| 13000-13999 |Giden |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Alt ağ redis) |
| 15000-15999 |Giden |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Alt ağ redis) |


### <a name="inbound-port-requirements"></a>Gelen bağlantı noktası gereksinimleri

Sekiz gelen bağlantı noktası aralığı gereksinimi yoktur. Bu aralıklardaki gelen istekleri aynı VNET içinde barındırılan diğer hizmetlerden gelen veya Redis alt ağ iletişimleri için iç.

| Bağlantı noktaları | Yön | Aktarım Protokolü | Amaç | Yerel IP | Uzak IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Gelen |TCP |İstemci iletişimi için Redis, Azure Yük Dengeleme | (Alt ağ redis) |Sanal ağ, Azure yük dengeleyici |
| 8443 |Gelen |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Alt ağ redis) |
| 8500 |Gelen |TCP/UDP |Azure Yük Dengeleme | (Alt ağ redis) |Azure Load Balancer |
| 10221-10231 |Gelen |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Redis. alt ağ), Azure yük dengeleyici |
| 13000-13999 |Gelen |TCP |İstemci iletişimi Redis kümelerine, Azure Yük Dengeleme | (Alt ağ redis) |Sanal ağ, Azure yük dengeleyici |
| 15000-15999 |Gelen |TCP |İstemci iletişimi Redis kümeleri için Azure Yük Dengeleme | (Alt ağ redis) |Sanal ağ, Azure yük dengeleyici |
| 16001 |Gelen |TCP/UDP |Azure Yük Dengeleme | (Alt ağ redis) |Azure Load Balancer |
| 20226 |Gelen |TCP |Redis iç iletişiminde | (Alt ağ redis) |(Alt ağ redis) |

### <a name="additional-vnet-network-connectivity-requirements"></a>Ek sanal ağ bağlantı gereksinimleri

Azure Redis önbelleği sanal bir ağa başlangıçta karşılanır değil için ağ bağlantı gereksinimleri vardır. Azure Redis önbelleği sanal ağ içindeki kullanıldığında düzgün çalışması için tüm aşağıdaki öğeleri gerektirir.

* Azure depolama uç noktaları dünya çapında giden ağ bağlantısı. Bu depolama uç noktaları bulunan yanı sıra, Azure Redis önbelleği örneği aynı bölgede bulunan uç noktaları içerir **diğer** Azure bölgeleri. Azure depolama uç noktaları çözmek aşağıdaki DNS etki alanı altında: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*, ve *file.core.windows.net*. 
* Giden ağ bağlantısı *ocsp.msocsp.com*, *mscrl.microsoft.com*, ve *crl.microsoft.com*. Bu bağlantılar, SSL işlevleri desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve önceki noktaların belirtilen etki alanları çözme yeteneğinin olması gerekir. Geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan sağlayarak bu DNS gereksinimleri karşılanabilir.
* Aşağıdaki DNS etki alanı altında çözmek aşağıdaki Azure izleme uç noktaları, giden ağ bağlantısı: shoebox2-black.shoebox2.metrics.nsatc.net, Kuzey-prod2.prod2.metrics.nsatc.net azglobal-black.azglobal.metrics.nsatc.net , shoebox2-red.shoebox2.metrics.nsatc.net, Doğu-prod2.prod2.metrics.nsatc.net azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>My önbelleği sanal ağ içinde çalıştığını nasıl doğrulayabilirsiniz?

>[!IMPORTANT]
>Sanal ağ içinde barındırılan bir Azure Redis önbelleği örneğine bağlanırken, önbellek istemcileri herhangi bir sınama uygulamalarını veya tanılama ping araçları dahil olmak üzere aynı VNET'i olması gerekir.
>
>

Bağlantı noktası gereksinimleri önceki bölümde açıklandığı gibi yapılandırıldıktan sonra önbelleğiniz aşağıdaki adımları gerçekleştirerek çalışıp çalışmadığını doğrulayabilirsiniz.

- [Yeniden başlatma](cache-administration.md#reboot) tüm önbellek düğümleri. Tüm gerekli önbellek bağımlılıklarını ulaşılamıyor varsa (açıklandığı gibi [gelen bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#inbound-port-requirements) ve [giden bağlantı noktası gereksinimleri](cache-how-to-premium-vnet.md#outbound-port-requirements)), Önbelleği başarıyla yeniden yükleyemezsiniz.
- Ön bellek düğümleri (Azure Portalı'nda önbellek durumu tarafından belirlendiği şekilde) yeniden başlattıktan sonra aşağıdaki testleri gerçekleştirebilirsiniz:
  - Önbellek uç noktası (bağlantı noktası 6380 kullanarak) önbelleği olarak aynı sanal ağ içindeki bir makineden ping kullanarak [tcping](https://www.elifulkerson.com/projects/tcping.php). Örneğin:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Varsa `tcping` aracı raporları bağlantı noktasının açık olduğundan, önbellek VNET içindeki istemcilerinden gelen bağlantı için kullanılabilir.

  - Test etmek için başka bir test önbellek istemcisi oluşturmak için yoldur (basit bir olabilir konsol uygulaması StackExchange.Redis kullanarak), önbelleğe bağlanır ve ekler ve bazı öğeler önbellekten alır. Örnek istemci uygulaması önbelleği olarak aynı sanal ağda ve çalıştırın, önbellek bağlantısını doğrulamak için bir VM üzerine yükleyin.


### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Sanal ağlar ile standart ya da temel bir önbellek kullanabilir miyim?
Sanal ağlar yalnızca premium önbelleklere ile kullanılabilir.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Neden Redis önbelleği oluşturma bazı alt ağların ancak diğer başarısız oluyor?
Bir Azure Redis önbelleği için Resource Manager Vnet'i dağıtıyorsanız, önbelleği başka bir kaynak türünü içeren özel bir alt ağda olmalıdır. Bir Resource Manager Vnet'i alt ağa bir Azure Redis önbelleği dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur. Yeni Redis önbelleği oluşturmadan önce alt ağ içindeki mevcut kaynakların silmeniz gerekir.

Kullanılabilir yeterli IP adresine sahip olduğu sürece, birden çok tür kaynak klasik bir Vnet'e dağıtabilirsiniz.

### <a name="what-are-the-subnet-address-space-requirements"></a>Alt ağ adres alanı gereksinimleri nelerdir?
Her alt ağ içindeki bazı IP adreslerini Azure ayırır ve bu adresleri kullanılamaz. Alt ağlar ilk ve son IP adreslerini Azure Hizmetleri için kullanılan üç daha fazla adres birlikte Protokolü uyum için ayrılmıştır. Daha fazla bilgi için bkz: [bu alt ağ içindeki IP adresleri kullanma kısıtlamaları vardır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure sanal ağ altyapısı tarafından kullanılan IP adreslerinin yanı sıra, her Redis örnek parça her alt ağ kullanan iki IP adresi ve yük dengeleyici için bir ek IP adresi. Kümelenmemiş bir önbellek, bir parça olduğu kabul edilir.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Tüm önbellek özellikleri, bir VNET önbelleğinde barındırdığında çalışıyor mu?
Önbelleğinizi VNET parçası olduğunda, yalnızca sanal ağ istemcilerinde önbelleğe erişebilir. Sonuç olarak, aşağıdaki önbellek yönetim özellikleri şu anda çalışmıyor.

* Konsol redis - Redis konsol sanal ağ dışında olan yerel tarayıcınızda çalıştığından, önbelleğiniz bağlanamıyor.

## <a name="use-expressroute-with-azure-redis-cache"></a>ExpressRoute ile Azure Redis önbelleğini kullanma
Müşteriler bağlanabileceği bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) böylece Azure için kendi şirket içi ağ genişletme sanal ağ altyapılarını devresine. 

Varsayılan olarak, yeni oluşturulan bir expressroute bağlantı hattı zorlamalı tünel gerçekleştirmez (varsayılan yol tanıtım 0.0.0.0/0) bir VNET üzerinde. Sonuç olarak, giden Internet bağlantısı sanal ağdan doğrudan izin verilir ve istemci uygulamaları Azure Redis önbelleği dahil olmak üzere diğer Azure uç noktaları için bağlantı kuramaz.

Ancak ortak bir müşteri yapılandırması kullanmaktır zorlanan tünel (varsayılan rota tanıtma) yerine şirket içi giden Internet akışına zorlar. Bu trafik akışı giden trafik ise, Azure Redis önbelleği ile bağlantısını keser. ardından Azure Redis önbelleği örneği bağımlılıklarını ile iletişim kurabildiğinden değil, şirket içi engellendi.

Azure Redis önbelleği içeren alt ağda bir (veya daha fazla) kullanıcı tanımlı yollar (Udr'ler) tanımlamak için kullanılan çözümüdür. Bir UDR yerine varsayılan yol uyulacaktır alt özel yollar tanımlar.

Mümkünse, aşağıdaki yapılandırmayı kullanmak için önerilir:

* ExpressRoute yapılandırma 0.0.0.0/0 tanıtır ve varsayılan olarak tüm giden trafiği şirket içi zorlamalı tünel oluşturur.
* Azure Redis önbelleği içeren alt ağa uygulanan UDR genel internet'e TCP/IP'yi trafiği için bir çalışma rotayla 0.0.0.0/0 tanımlar; Örneğin sonraki atlama türü 'İnternet' olarak ayarlanıyor.

Bu adımları etkilerini, alt düzey UDR tünel, zorunlu ExpressRoute böylece Azure Redis Önbelleği'nden giden Internet erişimi sağlama öncelikli olacağını ' dir.

ExpressRoute kullanarak bir şirket içi uygulamasından Azure Redis önbelleği örneğine bağlanmayı performans nedenleriyle (en iyi performans Azure Redis önbelleği istemcileri Azure Redis önbelleği ile aynı bölgede olması gerekir) nedeniyle tipik kullanım bir senaryo değildir.

>[!IMPORTANT] 
>Bir UDR tanımlanan rotalar **gerekir** ExpressRoute yapılandırma tarafından tanıtılan rotaları önceliklidir için belirli olabilir. Aşağıdaki örnek geniş 0.0.0.0/0 adres aralığını kullanır ve bu nedenle olası yanlışlıkla daha belirli adres aralıkları kullanarak Yol tanıtımlarını tarafından geçersiz kılınabilir.

>[!WARNING]  
>Azure Redis önbelleği ile ExpressRoute yapılandırmaları desteklenmez, **yanlış arası-özel eşleme yoluna ortak eşleme yolu yolları tanıtma**. Yapılandırılmış, ortak eşleme sahip ExpressRoute yapılandırmaları çok sayıda Microsoft Azure IP adres aralıkları için Microsoft'tan Yol tanıtımlarını alırsınız. Bu adres aralıklarını yanlış cross-özel eşleme yoluna üzerinde tanıtılan varsa, tüm giden ağ paketlerinin Azure Redis önbelleği örneğinin alt ağdan hatalı zorla bir müşterinin şirket içi ağ altyapısına tünelli sonucudur . Bu ağ akışı Azure Redis önbelleği keser. Bu sorun için çözüm ortak eşleme yolu arası reklam yolları özel eşleme yoluna önlemektir.


Kullanıcı tanımlı yollar hakkında arka plan bilgileri kullanılabilir bu [genel bakış](../virtual-network/virtual-networks-udr-overview.md).

ExpressRoute hakkında daha fazla bilgi için bkz: [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerinin nasıl kullanılacağını öğrenin.

* [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

