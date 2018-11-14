---
title: Express Route ile çalışmak için ağ yapılandırma ayrıntıları
description: App Service ortamları çalıştıran bir sanal ağ için ağ yapılandırma ayrıntıları, bir ExpressRoute bağlantı hattına bağlı.
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
ms.openlocfilehash: 7873192e4a66cd2faed5a1a1255377139d33d750
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616070"
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>ExpressRoute Bağlantılı App Service Ortamları için Ağ Yapılandırması Ayrıntıları
## <a name="overview"></a>Genel Bakış
Müşteriler bağlanabilir bir [Azure ExpressRoute] [ ExpressRoute] bağlantı hattına sanal ağ altyapılarını böylece kendi şirket içi ağı Azure'a genişletme.  App Service ortamı bu alt ağda oluşturulabilir [sanal ağ] [ virtualnetwork] altyapı.  App Service ortamında çalışan uygulamalar, ardından yalnızca ExpressRoute bağlantısı üzerinden erişilebilir arka uç kaynaklarına güvenli bağlantılar kurabilirsiniz.  

App Service ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağı **veya** Klasik dağıtım modeli sanal ağı.  Haziran 2016'da yapılan son değişikliği ile ase artık genel adres aralıkları ya da RFC1918 adres alanları (yani özel adresler) kullanan sanal ağlara dağıtılabilir. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Gerekli ağ bağlantısı
Başlangıçta ExpressRoute bağlı bir sanal ağdaki karşılaşabileceğini değil App Service ortamları için ağ bağlantı gereksinimleri vardır.  App Service ortamları düzgün çalışması için şunların hepsini gerektirir:

* Azure depolama Uç noktalara dünya çapında her iki bağlantı noktası 80 ve 443 giden ağ bağlantısı.  Bu depolama uç noktaları yer yanı sıra, App Service ortamı ile aynı bölgede bulunan uç noktaları içerir **diğer** Azure bölgeleri.  Azure depolama uç noktaları aşağıdaki DNS etki alanları altında çözün: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* ve *file.core.windows.net*.  
* Azure dosyaları hizmeti bağlantı noktası 445'in üzerinde giden ağ bağlantısı.
* App Service ortamı ile aynı bölgede bulunan bir Sql DB uç noktalarına giden ağ bağlantısını.  SQL DB uç noktaları aşağıdaki etki alanı altında çözün: *database.windows.net*.  Bu bağlantı noktası 1433 11000 11999 ve 14000 14999 Access'i açıp gerektirir.  Daha fazla ayrıntı için bkz: [Sql veritabanı V12 bağlantı noktası kullanımıyla ilgili bu makale](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Giden ağ bağlantısını Azure yönetim düzlemi Uç noktalara (hem ASM hem de ARM uç noktaları).  Bu, hem de giden bağlantı içerir *management.core.windows.net* ve *management.azure.com*. 
* Giden ağ bağlantısını *ocsp.msocsp.com*, *mscrl.microsoft.com* ve *crl.microsoft.com*.  Bu, SSL işlevleri desteklemek için gereklidir.
* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve etki alanları önceki noktaların bahsedilen çözme özelliğine sahip olması gerekir.  Bu uç noktaları çözümlenemezse, App Service ortamı oluşturma denemesi başarısız olur ve var olan App Service ortamları sağlıksız olarak işaretlenir.
* Giden erişim bağlantı noktası 53, DNS sunucuları ile iletişim için gereklidir.
* Özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki varsa, DNS sunucusu içeren App Service ortamı alt ağından erişilebilir olmalıdır. 
* Giden ağ yolu iç kurumsal proxy'leri seyahat olamaz ya da şirket içi zorlamalı olabilir.  Bunun yapılması, App Service ortamından giden ağ trafiğini etkili NAT adresini değiştirir.  App Service ortamınızın giden ağ trafiği NAT adresini değiştirme-çok yukarıda listelenen uç noktası bağlantısı hataları neden olur.  Bu App Service ortamı oluşturma denemesi başarısız, yanı sıra daha önce sağlıklı App Service ortamları sistem durumu kötü olarak işaretlenmesi sonuçlanır.  
* App Service ortamları, bu konuda açıklandığı gibi izin gerekir için gerekli bağlantı noktaları için ağ erişimi gelen [makale][requiredports].

Geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan sağlayarak DNS gereksinimlerini karşılanabilir.  App Service ortamı oluşturulduktan sonra herhangi bir nedenle DNS yapılandırmasını değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını seçmek için bir App Service ortamı zorlayabilirsiniz.  App Service ortamı yönetim dikey pencerede üst kısmındaki "Yeniden Başlat" simgesini kullanarak sıralı bir ortamı yeniden harekete bulunan [Azure portalında] [ NewPortal] yeni DNS seçmek için ortamı neden olur yapılandırma.

Gelen ağ erişim gereksinimlerini yapılandırarak karşılanabiliyorsa bir [ağ güvenlik grubu] [ NetworkSecurityGroups] bu konuda açıklandığı gibi gerekli erişime izin vermek için App Service ortamının alt ağdaki [ makale][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>App Service ortamı için giden ağ bağlantısını etkinleştirme
Varsayılan olarak, yeni oluşturulan bir ExpressRoute bağlantı hattı giden Internet bağlantısı sağlayan bir varsayılan rota bildirir.  Bu yapılandırma ile bir App Service ortamı, diğer Azure Uç noktalara bağlanmak mümkün olacaktır.

Ancak yaygın müşteri giden Internet trafiğini şirket içi yerine akış zorlar, kendi varsayılan yolun (0.0.0.0/0) tanımlamak için bir yapılandırmadır.  Bu trafik akışı, App Service ortamları neredeyse şaşmaz biçimde ya da engellenen şirket içi giden trafiği olduğundan veya tanınmayan bir artık çeşitli Azure uç noktaları ile çalışma adresleri kümesi NAT istersiniz keser.

App Service ortamını içeren alt ağda bir (veya daha fazla) kullanıcı tanımlı yollar (Udr) tanımlamak için kullanılan çözümüdür.  Varsayılan yol yerine getirilmez alt özel yollar UDR tanımlar.

Mümkün olduğunda, aşağıdaki yapılandırmayı kullanın önerilir:

* ExpressRoute yapılandırması 0.0.0.0/0 tanıtır ve varsayılan olarak, tüm giden trafiği şirket içi zorla tünel oluşturur.
* App Service ortamını içeren alt ağa uygulanan UDR 0.0.0.0/0 sonraki atlama türü (buna örnek olarak bu makaledeki küçüldükleri aşağı) internet ile tanımlar.

Bu adımların etkilerini, alt düzey UDR öncelik böylece App Service ortamından giden Internet erişimini sağlama tünel, zorlamalı ExpressRoute üzerinden kazanır ' dir.

> [!IMPORTANT]
> Bir UDR'de tanımlanan yollar **gerekir** ExpressRoute yapılandırması tarafından tanıtılan herhangi bir yoldan öncelikli olacak kadar spesifik olmalıdır.  Aşağıdaki örnekte geniş 0.0.0.0/0 adres aralığı kullanır ve bu nedenle büyük olasılıkla yanlışlıkla daha spesifik adres aralıkları kullanan yol tanıtımları tarafından geçersiz kılınabilir.
> 
> App Service ortamları ile ExpressRoute yapılandırmaları desteklenmez, **yolları genel eşleme yolundan özel eşleme yoluna çapraz olarak bildirmek**.  Genel eşlemesi yapılandırılmış, olan ExpressRoute yapılandırmaları alacağı yol tanıtımları Microsoft çok sayıda Microsoft Azure IP adresi aralığı için.  Bu adres aralıkları özel eşleme yolunda çapraz olarak tanıtılan ise sonuç, tüm giden ağ paketlerinin App Service ortamının alt ağından bir müşterinin şirket içi ağ altyapısı için zorlamalı duruma geleceğidir.  Bu ağ akışı, App Service ortamları ile şu anda desteklenmiyor.  Bu sorunun bir çözümü, genel eşleme yolundan özel eşleme yoluna çapraz tanıtımını durdurmaktır yolları önlemektir.
> 
> 

Kullanıcı tanımlı yollar hakkında arka plan bilgileri bu kullanılabilir [genel bakış][UDROverview].  

Oluşturma ve kullanıcı tanımlı yollar yapılandırma ayrıntıları bu kullanılabilir [Kılavuzu nasıl][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>App Service ortamı için örnek UDR yapılandırma
**Ön koşullar**

1. Azure Powershell'den yükleme [Azure indirmeler sayfasına] [ AzureDownloads] (Haziran, 2015 veya üzeri tarihli).  "Komut satırı araçları altında" en son Powershell cmdlet'lerini yükler "Windows Powershell" altında bir "Yükle" bağlantısı yoktur.
2. Benzersiz bir alt ağ bir App Service ortamı tarafından özel kullanım için oluşturulan önerilir.  Bu App Service ortamı için giden trafik, alt ağa uygulanan Udr'ler yalnızca açılmasını sağlar.
3. **Önemli**: App Service ortamı kadar dağıtmayın **sonra** aşağıdaki yapılandırma adımlarını izlenir.  Bu, bir App Service ortamını dağıtmaya çalışmadan önce giden ağ bağlantısını kullanılabilir olmasını sağlar.

**1. adım: adlandırılmış yönlendirme tablosu oluşturma**

Aşağıdaki kod parçacığı Batı ABD Azure bölgesinde "DirectInternetRouteTable" adlı bir rota tablosu oluşturur:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**2. adım: bir veya daha fazla yol yol tablosu oluşturma**

Giden Internet erişimi etkinleştirmek için yönlendirme tablosunu bir veya daha fazla yol eklemeniz gerekecektir.  

Giden Internet erişimi yapılandırmak için önerilen yaklaşım, aşağıda gösterildiği gibi 0.0.0.0/0 için bir rota tanımlamaktır.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Bu 0.0.0.0/0 geniş adres aralığı ve bu nedenle daha spesifik adres aralıkları Expressroute'un tanıtılan tarafından geçersiz kılınır unutmayın.  Önceki öneri yeniden yinelemek için UDR 0.0.0.0/0 yol ile yalnızca 0.0.0.0/0 de bildirir. bir ExpressRoute yapılandırması ile birlikte kullanılmalıdır. 

Alternatif olarak, Azure tarafından kullanılan CIDR aralıkları kapsamlı ve güncelleştirilmiş bir listesini indirebilirsiniz.  Tüm Azure IP adresi aralıklarını içeren Xml dosyasını kullanılabilir [Microsoft Download Center][DownloadCenterAddressRanges].  

Ancak bu aralıklar böylece düzenli el ile güncelleştirmeleri eşitlemek için kullanıcı tanımlı yollar için araya zaman içinde değiştirmek unutmayın.  Tek bir UDR 100 yollar bir varsayılan üst sınır olduğundan, ayrıca, "Azure IP adresi aralıklarını 100 rota sınırı içinde uyacak şekilde özetlemek" ihtiyacınız olacak UDR yollar yollara kıyasla daha belirgin gerek tanımlanan göz önünde tutmak, ExpressRo tarafından tanıtılan alıştır.  

**3. adım: App Service ortamını içeren alt ağın yol tablosuna ilişkilendirme**

Son yapılandırma adımı alt ağına yönlendirme tablosunu ilişkilendirme App Service ortamı dağıtılacağı sağlamaktır.  Aşağıdaki komut, "App Service ortamı sonunda içerecek ASESubnet" için "DirectInternetRouteTable" ilişkilendirir.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**4. adım: Son adımlar**

Rota tablosunu alt ağa bağlandıktan sonra önce test ve hedeflenen etkisi onaylamak için önerilir.  Örneğin, alt ağa bir sanal makine dağıtma ve onaylayın:

* Bu makalede hem Azure hem de Azure dışı uç noktalarına giden trafik daha önce bahsedilen **değil** ExpressRoute bağlantı hattı akar.  Bu davranış, alt ağdan giden trafik ise şirket içi, App Service ortamı oluşturma her zaman başarısız olur hala zorlamalı tünel beri doğrulamak çok önemlidir. 
* Uç noktaları için DNS araması tüm düzgün bir şekilde çözme daha önce bahsedilen. 

Yukarıdaki adımları onaylandıktan sonra alt App Service ortamı oluşturulduğunda "boş" olması gerektiğinden, sanal makineyi silmeniz gerekir.

Ardından App Service ortamı oluşturma ile devam edin!

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [App Service Ortamı'na giriş][IntroToAppServiceEnvironment]

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
