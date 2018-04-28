---
title: Hızlı rota ile çalışmak için ağ yapılandırma ayrıntıları
description: Uygulama hizmeti ortamları sanal ağlarda çalıştırmak için ağ yapılandırma ayrıntıları bir expressroute bağlantı hattı bağlı.
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: fcb9fa9004039205fa49f63c50d5907a8029a079
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>ExpressRoute Bağlantılı App Service Ortamları için Ağ Yapılandırması Ayrıntıları
## <a name="overview"></a>Genel Bakış
Müşteriler bağlanabileceği bir [Azure ExpressRoute] [ ExpressRoute] böylece Azure için kendi şirket içi ağ genişletme sanal ağ altyapılarını devresine.  Bu alt ağda bir uygulama hizmeti ortamı oluşturulabilir [sanal ağ] [ virtualnetwork] altyapı.  Uygulama hizmeti ortamı üzerinde çalışan uygulamalar, ardından yalnızca ExpressRoute bağlantısı üzerinden erişilebilir arka uç kaynaklarına güvenli bağlantı kurabilirsiniz.  

Bir uygulama hizmeti ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağ **veya** Klasik dağıtım modeli sanal ağı.  Haziran 2016'da yapılan son bir değişiklikle ASEs de artık ortak adres aralıkları veya RFC1918 adres alanları (özel adresleri) kullanan sanal ağları içinde dağıtılabilir. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Gerekli ağ bağlantısı
Başlangıçta bir Expressroute'a bağlı bir sanal ağdaki karşılaşabileceğini değil, App Service ortamları için ağ bağlantı gereksinimleri vardır.  Uygulama hizmeti ortamları düzgün çalışması için aşağıdakilerin tümü gerektirir:

* Azure depolama üzerindeki uç noktaları dünya çapında her iki bağlantı noktası 80 ve 443 giden ağ bağlantısı.  Bu uygulama hizmeti ortamı ile aynı bölgede bulunan uç noktalar olarak bulunan depolama uç noktaları içerir **diğer** Azure bölgeleri.  Azure depolama uç noktaları çözmek aşağıdaki DNS etki alanı altında: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* ve *file.core.windows.net*.  
* Azure dosyaları hizmet bağlantı noktası 445 giden ağ bağlantısı.
* Uygulama hizmeti ortamı ile aynı bölgede bulunan Sql DB uç noktalarına giden ağ bağlantısı.  SQL DB uç noktaları altında aşağıdaki etki alanı çözümlenemiyor: *database.windows.net*.  Bu bağlantı noktası 1433'tür 11000 11999 ve 14000 14999 erişimi açma gerektirir.  Daha fazla bilgi için bkz [bu makalede Sql Database V12 bağlantı noktası kullanımı](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Azure yönetim düzlemi uç noktaları (ASM ve ARM uç noktaları) giden ağ bağlantısı.  Bu, hem de giden bağlantı içerir *management.core.windows.net* ve *management.azure.com*. 
* Giden ağ bağlantısı *ocsp.msocsp.com*, *mscrl.microsoft.com* ve *crl.microsoft.com*.  Bu, SSL işlevleri desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve önceki noktaların belirtilen etki alanları çözme yeteneğinin olması gerekir.  Bu uç noktalar çözümlenemezse, uygulama hizmeti ortamı oluşturma denemeleri başarısız olur ve var olan uygulama hizmeti ortamları gibi sağlıksız olarak işaretlenecek.
* DNS sunucuları ile iletişim için bağlantı noktası 53 giden erişim gereklidir.
* Özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki varsa, DNS sunucusu uygulama hizmeti ortamı içeren alt ağ üzerinden erişilebilir olması gerekir. 
* Giden ağ yolu iç kurumsal proxy'leri seyahat olamaz ve şirket içi tünelli zorla olabilir.  Bunun yapılması uygulama hizmeti ortamındaki giden ağ trafiğini etkili NAT adresini değiştirir.  Uygulama hizmeti ortamı'nın giden ağ trafiğini NAT adresini değiştirmek yukarıda listelenen uç noktaları çoğuna bağlantısı hataları neden olacak.  Bu, başarısız uygulama hizmeti ortamı oluşturma girişimi, aynı zamanda daha önce sağlıklı App Service ortamları sorunlu olarak işaretlenen sonuçlanır.  
* Uygulama hizmeti ortamları, bu konuda açıklandığı gibi izin verilmelidir için ağ erişimi için gereken bağlantı noktalarını gelen [makale][requiredports].

Geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan sağlayarak DNS gereksinimleri karşılanabilir.  Bir uygulama hizmeti ortamı oluşturulduktan sonra DNS yapılandırmasını herhangi bir nedenle değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını almak için bir uygulama hizmeti ortamı zorlayabilirsiniz.  "Yeniden Başlat" simgesini kullanarak çalışırken bir ortamı yeniden başlatma tetikleme bulunan uygulama hizmeti ortamı yönetim dikey penceresinde üstündeki [Azure portal] [ NewPortal] yeni DNS araması seçmek ortam neden olur yapılandırma.

Gelen ağ erişim gereksinimlerini yapılandırarak karşılanır bir [ağ güvenlik grubu] [ NetworkSecurityGroups] bu konuda açıklandığı gibi gerekli erişime izin vermek için uygulama hizmeti ortamı'nın alt ağdaki [ makale][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Uygulama hizmeti ortamı için etkinleştirme giden ağ bağlantısı
Varsayılan olarak, yeni oluşturulan bir expressroute bağlantı hattı giden Internet bağlantısı sağlayan bir varsayılan yol tanıtır.  Bu yapılandırma ile bir uygulama hizmeti ortamı Azure diğer Uç noktalara bağlanmak mümkün olacaktır.

Ancak bir ortak müşteri bunun yerine şirket içi giden Internet akışına zorlar kendi varsayılan yol (0.0.0.0/0) tanımlamak için yapılandırmadır.  Bu trafik akışı App Service ortamları ya da engellenen şirket içi giden trafik olduğundan veya tanınmayan bir artık çeşitli Azure uç noktaları ile çalışma adresleri kümesini NAT ister neredeyse şaşmaz biçimde keser.

Uygulama hizmeti ortamı içeren alt ağda bir (veya daha fazla) kullanıcı tanımlı yollar (Udr'ler) tanımlamak için kullanılan çözümüdür.  Bir UDR yerine varsayılan yol uyulacaktır alt özel yollar tanımlar.

Mümkünse, aşağıdaki yapılandırmayı kullanmak için önerilir:

* ExpressRoute yapılandırma 0.0.0.0/0 tanıtır ve varsayılan olarak tüm giden trafiği şirket içi zorlamalı tünel oluşturur.
* Uygulama hizmeti ortamı içeren alt ağa uygulanan UDR (buna örnek olarak bu makaledeki uzağına aşağı) Internet sonraki atlama türü olan 0.0.0.0/0 tanımlar.

Bu adımları etkilerini, alt düzey UDR öncelik böylece uygulama hizmeti ortamındaki giden Internet erişimi sağlama tünel, zorunlu ExpressRoute kazanır ' dir.

> [!IMPORTANT]
> Bir UDR tanımlanan rotalar **gerekir** ExpressRoute yapılandırma tarafından tanıtılan rotaları önceliklidir için belirli olabilir.  Aşağıdaki örnek geniş 0.0.0.0/0 adres aralığını kullanır ve bu nedenle olası yanlışlıkla daha belirli adres aralıkları kullanarak Yol tanıtımlarını tarafından geçersiz kılınabilir.
> 
> Uygulama hizmeti ortamları ExpressRoute yapılandırmalarla desteklenmiyor, **ortak eşleme yolu yolları özel eşleme yoluna arası tanıtma**.  Yapılandırılmış, ortak eşleme sahip ExpressRoute yapılandırmaları alacağı Yol tanıtımlarını Microsoft'tan çok sayıda Microsoft Azure IP adres aralıkları için.  Özel eşleme yoluna üzerinde çapraz tanıtılan bu adres aralıkları, sonuç tüm giden ağ paketlerinin uygulama hizmeti ortamı'nın alt ağdan bir müşterinin şirket içi ağ altyapısına zorlamalı tünel olacağını olur.  Bu ağ akışı ile uygulama hizmeti ortamları şu anda desteklenmiyor.  Bu sorun için bir çözüm, ortak eşleme yolu arası reklam yolları özel eşleme yoluna önlemektir.
> 
> 

Kullanıcı tanımlı yollar hakkında arka plan bilgileri kullanılabilir bu [genel bakış][UDROverview].  

Oluşturma ve kullanıcı tanımlı yollar yapılandırma ile ilgili ayrıntılar bu konuda kullanılabilir [kılavuzuna nasıl][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Uygulama hizmeti ortamı için örnek UDR yapılandırma
**Ön koşullar**

1. Azure Powershell gelen yükleme [Azure indirmeler sayfası] [ AzureDownloads] (Haziran 2015 veya sonraki tarihli).  "Altında komut satırı araçları" son Powershell cmdlet'lerini yükler "Windows Powershell" altında "Yükle" bağlantısı yoktur.
2. Benzersiz bir alt ağ bir uygulama hizmeti ortamı tarafından özel kullanım için oluşturulan önerilir.  Bu uygulama hizmeti ortamı için giden trafik, alt ağa uygulanan Udr'ler yalnızca açılır sağlar.
3. **Önemli**: uygulama hizmeti ortamı kadar dağıtmayın **sonra** aşağıdaki yapılandırma adımlarını izlenir.  Bu uygulama hizmeti ortamını dağıtmaya çalışmadan önce giden ağ bağlantısı kullanılabilir olmasını sağlar.

**1. adım: bir adlandırılmış yol tablosu oluşturma**

Aşağıdaki kod parçacığında Batı ABD Azure bölgesinde "DirectInternetRouteTable" adında bir yol tablosu oluşturur:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**2. adım: bir veya daha fazla yol rota tablosunda oluşturma**

Giden Internet erişimi etkinleştirmek için yol tablosu bir veya daha fazla yol eklemeniz gerekir.  

Giden Internet erişimi yapılandırmak için önerilen yaklaşım, aşağıda gösterildiği gibi 0.0.0.0/0 için bir rota tanımlamaktır.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Bu 0.0.0.0/0 geniş adres aralığı ve bu nedenle daha belirli adres aralıkları ExpressRoute tarafından tanıtılan tarafından geçersiz kılınacaktır unutmayın.  Önceki öneri yeniden yinelemek için UDR 0.0.0.0/0 yol ile yalnızca 0.0.0.0/0 de tanıtır ExressRoute yapılandırma ile birlikte kullanılmalıdır. 

Alternatif olarak, Azure tarafından kullanılıyor CIDR aralıkları, kapsamlı ve güncelleştirilmiş listesini indirebilirsiniz.  Tüm Azure IP adres aralıklarını içeren Xml dosyasını kullanılabilir [Microsoft Download Center][DownloadCenterAddressRanges].  

Bu aralıklar böylece düzenli el ile eşitlenmiş tutmak için kullanıcı tanımlı yollar yapılan güncelleştirmeler araya zaman içinde değişip unutmayın.  Varsayılan üst sınır tek UDR 100 yolların olduğundan, ayrıca, "100 rota sınırı içinde uyacak şekilde Azure IP adres aralıklarını özetlemek" gerekir UDR yollar yolları ayrıntılı olması gerekiyor tanımlanan aklınızda tutarak, ExpressRo tarafından tanıtılan ute.  

**3. adım: uygulama hizmeti ortamı içeren alt ağ için yol tablosu ilişkilendirme**

Son yapılandırma adımı alt ağ için yol tablosu ilişkilendirmek için uygulama hizmeti ortamı dağıtılacağı ' dir.  Aşağıdaki komut, "Bir uygulama hizmeti ortamı sonunda içerecek ASESubnet" için "DirectInternetRouteTable" ilişkilendirir.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**4. adım: Son adımlar**

Yol tablosu alt ağına bağlandıktan sonra önce test ve hedeflenen etkisi onaylamak için önerilir.  Örneğin, alt ağ bir sanal makine dağıtabilir ve onaylayın:

* Bu makalede hem Azure hem de Azure olmayan uç noktaları giden trafik daha önce bahsedilen **değil** expressroute bağlantı hattı akar.  Bu davranış, alt ağdan giden trafiği ise şirket içi, uygulama hizmeti ortamı oluşturma her zaman başarısız olur hala zorlamalı tünel beri doğrulamak çok önemlidir. 
* Uç noktaları için DNS araması tüm düzgün çözme daha önce bahsedilen. 

Yukarıdaki adımları onaylandıktan sonra alt ağ uygulama hizmeti ortamı oluşturulduğunda "boş" olması gerektiğinden sanal makine silmeniz gerekir.

Bir uygulama hizmeti ortamı oluşturma ile devam edin!

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamı giriş][IntroToAppServiceEnvironment]

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[AzureDownloads]: http://azure.microsoft.com/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
