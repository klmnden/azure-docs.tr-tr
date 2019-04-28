---
title: Azure ExpressRoute - App Service için ağ yapılandırma ayrıntıları
description: Yapılandırma ayrıntıları, bir Azure ExpressRoute bağlantı hattına bağlı sanal ağlarda bulunan PowerApps için App Service ortamı için ağ.
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
ms.custom: seodec18
ms.openlocfilehash: e0fa87facec73efdfff1a9908dcba92838215425
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130679"
---
# <a name="network-configuration-details-for-app-service-environment-for-powerapps-with-azure-expressroute"></a>Azure ExpressRoute ile PowerApps için App Service ortamı için ağ yapılandırma ayrıntıları

Müşteriler bağlanabilir bir [Azure ExpressRoute] [ ExpressRoute] bağlantı hattına sanal ağ altyapılarını kendi şirket içi ağı Azure'a genişletin. App Service ortamı alt ağı içinde oluşturulan [sanal ağ] [ virtualnetwork] altyapı. App Service ortamında çalışan uygulamalar, yalnızca ExpressRoute bağlantısı üzerinden erişilebilir olan arka uç kaynaklarına güvenli bağlantılar kurun.  

App Service ortamı bu senaryolarda oluşturulabilir:
- Azure Resource Manager sanal ağları.
- Klasik dağıtım modeli sanal ağları.
- Ortak adres aralıkları veya RFC1918 kullanan sanal ağlar alanları (diğer bir deyişle, özel adresler) adresi. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Gerekli ağ bağlantısı

App Service ortamı, başlangıçta Expressroute'a bağlı bir sanal ağdaki karşılanması değil ağ bağlantı gereksinimleri vardır.

App Service ortamı düzgün şekilde çalışabilmesi aşağıdaki ağ bağlantısı ayarları gerektirir:

* Giden ağ bağlantısını için Azure depolama uç noktaları dünya çapında bağlantı noktası 80 ve 443 numaralı bağlantı noktası. Bu uç noktaları, App Service ortamı ile aynı bölgede ve aynı zamanda diğer Azure bölgeleri içinde yer alır. Azure depolama uç noktaları aşağıdaki DNS etki alanları altında çözün: table.core.windows.net, blob.core.windows.net queue.core.windows.net ve file.core.windows.net.  

* Azure dosyaları hizmeti bağlantı noktası 445'in üzerinde giden ağ bağlantısı.

* App Service ortamı ile aynı bölgede bulunan Azure SQL veritabanı uç noktalarına giden ağ bağlantısını. SQL veritabanı uç noktaları 11000 11999 ve 14000 14999 1433 numaralı bağlantı noktalarına erişmesi açık database.windows.net etki alanı altında çözümleyin. SQL veritabanı V12 bağlantı noktası kullanımı hakkında daha fazla ayrıntı için bkz: [ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).

* Giden ağ bağlantısını Azure yönetim düzlemi Uç noktalara (Azure Klasik dağıtım modelini ve Azure Resource Manager uç noktaları). Bu uç noktalarına bağlantıyı management.core.windows.net ve management.azure.com etki alanlarını içerir. 

* Giden ağ bağlantısını ocsp.msocsp.com mscrl.microsoft.com ve crl.microsoft.com etki alanları. Bağlantı bu etki alanlarına SSL işlevselliği desteklemek için gereklidir.

* Sanal ağ için DNS yapılandırmasını tüm uç noktaları ve etki alanları bu makalede çözümlemek kurabilmesi gerekir. Uç noktaları çözümlenemezse, App Service ortamı oluşturma başarısız olur. Tüm mevcut App Service ortamı sağlıksız olarak işaretlenir.

* Giden erişim bağlantı noktası 53, DNS sunucuları ile iletişim için gereklidir.

* Özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki varsa, DNS sunucusu içeren App Service ortamı alt ağından erişilebilir olmalıdır. 

* Giden ağ yolu iç kurumsal proxy'leri seyahat olamaz ve şirket tünel zorla olamaz. Bu Eylemler, App Service ortamından giden ağ trafiğini etkili NAT adresini değiştirin. App Service ortamı giden ağ trafiği NAT adresini değişiklikler çoğu uç noktalarına bağlantısı hataları neden. App Service ortamı oluşturma başarısız olur. Tüm mevcut App Service ortamı sağlıksız olarak işaretlenir.

* App Service ortamı için gerekli bağlantı noktalarına gelen ağ erişimini izin verilmesi gerekir. Ayrıntılar için bkz [App Service ortamına gelen trafiği denetleme][requiredports].

DNS gereksinimlerini karşılamak için geçerli bir DNS altyapısının yapılandırılmış ve sanal ağ için tutulan emin olun. App Service ortamı oluşturduktan sonra DNS yapılandırması değiştiyse, geliştiricilerin yeni DNS yapılandırmasını seçmek için App Service ortamı zorlayabilirsiniz. Sıralı bir ortamı yeniden tetikleyebilirsiniz **yeniden** App Service ortamı yönetim kapsamında simgesi [Azure portalında][NewPortal]. Yeniden başlatma yeni DNS yapılandırmasını seçmek için ortamı neden olur.

Gelen ağ erişim gereksinimlerini karşılamak için yapılandırma bir [ağ güvenlik grubu (NSG)] [ NetworkSecurityGroups] App Service ortamı alt ağda. NSG gerekli erişimi sağlayan [App Service ortamına gelen trafiği denetleme][requiredports].

## <a name="outbound-network-connectivity"></a>Giden ağ bağlantısını

Varsayılan olarak, yeni oluşturulan bir ExpressRoute bağlantı hattı giden internet bağlantısı sağlayan bir varsayılan rota bildirir. App Service ortamı, diğer Azure Uç noktalara bağlanmak için bu yapılandırmayı kullanabilirsiniz.

Yaygın müşteri giden internet trafiğini şirket için akış zorlar, kendi varsayılan yolun (0.0.0.0/0) tanımlamak için bir yapılandırmadır. Bu trafik akışı, App Service ortamı neredeyse şaşmaz biçimde keser. Ya da engellenen şirket içi giden trafiği olduğundan veya tanınmayan bir artık çeşitli Azure uç noktaları ile çalışma adresleri kümesi NAT ister.

App Service ortamını içeren alt ağda bir (veya daha fazla) kullanıcı tanımlı yollar (Udr) tanımlamak için kullanılan çözümüdür. UDR yerine varsayılan yolu kabul özel alt ağ yollarını tanımlar.

Mümkün olduğunda, aşağıdaki yapılandırmayı kullanın:

* ExpressRoute yapılandırması 0.0.0.0/0 bildirir. Varsayılan olarak, tüm giden trafiği şirket içi yapılandırma zorla tünel oluşturur.
* App Service ortamını içeren alt ağa uygulanan UDR 0.0.0.0/0 sonraki atlama türü internet ile tanımlar. Bu yapılandırmanın bir örneği, bu makalenin sonraki bölümlerinde açıklanmıştır.

Bu yapılandırma etkilerini, alt düzey UDR ExpressRoute zorlamalı tüneli üzerinden öncelik kazanır ' dir. App Service ortamından giden internet erişimi sağlanır.

> [!IMPORTANT]
> Bir UDR'de tanımlanan yollar ExpressRoute yapılandırması tarafından tanıtılan tüm yollar öncelikli olacak kadar spesifik olmalıdır. Sonraki bölümde açıklanan örnekte geniş 0.0.0.0/0 adres aralığı kullanılır. Bu aralık, daha spesifik adres aralıkları kullanan yol tanıtımları tarafından yanlışlıkla geçersiz kılınabilir.
> 
> App Service ortamı, yollar genel eşleme yolundan özel eşleme yoluna çapraz olarak bildirmek ExpressRoute yapılandırmaları ile desteklenmez. Sahip genel eşlemesi yapılandırılmış ExpressRoute yapılandırmaları, Microsoft'tan yol tanıtımları çok sayıda Microsoft Azure IP adresi aralığı için alırsınız. Bu adres aralıkları özel eşleme yolunda çapraz olarak tanıtılan varsa, tüm giden ağ paketlerinin App Service ortamı alt ağından müşterinin şirket içi ağ altyapısına zorlamalı tünel uygulanır. Bu ağ akışı şu anda App Service ortamı ile desteklenmemektedir. Genel eşleme yolundan özel eşleme yoluna çapraz tanıtımını durdurmaktır yolları Durdur bir çözümdür.
> 
> 

Kullanıcı tanımlı yollar hakkında bilgi için bkz: [sanal ağ trafiği yönlendirme][UDROverview].  

Oluşturma ve kullanıcı tanımlı yolları yapılandırmanız hakkında bilgi edinmek için [PowerShell kullanarak bir yönlendirme tablosu ile ağ trafiğini yönlendirmek][UDRHowTo].

## <a name="udr-configuration"></a>UDR yapılandırma

Bu bölümde, App Service ortamı için örnek bir UDR yapılandırma gösterilmektedir.

### <a name="prerequisites"></a>Önkoşullar

* Azure Powershell'den yükleme [Azure indirmeler sayfasına][AzureDownloads]. Bir indirme Haziran 2015 veya sonraki bir tarihi seçin. Altında **komut satırı araçları** > **Windows PowerShell**seçin **yükleme** en son PowerShell cmdlet'lerini yüklemek için.

* App Service ortamı tarafından özel kullanım için benzersiz bir alt ağ oluşturun. Benzersiz bir alt ağ, alt ağ açık giden trafiği yalnızca App Service ortamı için uygulanan Udr'ler sağlar.

> [!IMPORTANT]
> Yapılandırma adımları tamamladıktan sonra yalnızca App Service ortamı dağıtın. Adımları, özel olarak App Service ortamı dağıtmayı denemeden önce giden ağ bağlantısını kullanılabilir olduğundan emin olun.

### <a name="step-1-create-a-route-table"></a>1. Adım: Yönlendirme tablosu oluşturma

Adlı bir yönlendirme tablosu oluşturma **DirectInternetRouteTable** Bu kod parçacığında gösterildiği gibi batı ABD Azure bölgesi:

`New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest`

### <a name="step-2-create-routes-in-the-table"></a>2. Adım: Yol tablosu oluşturma

Giden internet erişimi etkinleştirmek için yol tablosu yolları ekleyin.  

Giden internet erişimi yapılandırın. 0.0.0.0/0 için bir rota Bu kod parçacığında gösterildiği gibi tanımlayın:

`Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet`

0.0.0.0/0 geniş adres aralığıdır. Aralığın daha belirli olduğundan, Expressroute'un tanıtılan adres aralıklarını tarafından geçersiz kılındı. UDR 0.0.0.0/0 yol ile yalnızca 0.0.0.0/0 bildirir. bir ExpressRoute yapılandırması ile birlikte kullanılmalıdır. 

Alternatif olarak, Azure tarafından kullanılan CIDR aralıkları geçerli, kapsamlı bir listesini indirin. Tüm Azure IP adres aralıkları için XML dosyası kullanılabilir [Microsoft Download Center][DownloadCenterAddressRanges].  

> [!NOTE]
>
> Azure IP adresi aralıklarını zamanla değişir. Kullanıcı tanımlı yollar eşitlemek için el ile düzenli güncelleştirmeler gerekir.
>
> Tek bir UDR 100 yolların varsayılan bir üst sınırı vardır. "Azure IP adresi aralığı içinde 100-route sınırını aşmayacak şekilde özetlemek" gerekir. UDR tanımlı yollar, ExpressRoute bağlantınızı tarafından tanıtılan rotaları daha belirli gerekecektir.
> 

### <a name="step-3-associate-the-table-to-the-subnet"></a>3. Adım: Alt ağa tablosunu ilişkilendirme

Burada, App Service ortamı dağıtmak istediğiniz alt ağ için rota tablosu ilişkilendirin. Bu komut ilişkilendirir **DirectInternetRouteTable** tablo **ASESubnet** App Service ortamını içeren alt ağ.

`Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'`

### <a name="step-4-test-and-confirm-the-route"></a>4. Adım: Test ve rota onaylayın

Rota tablosunu alt ağa bağlandıktan sonra test ve rota onaylayın.

Alt ağa bir sanal makine dağıtma ve bu koşullar doğrulayın:

* Bu makalede açıklanan Azure dışı uç noktalar ve Azure'a giden trafik yok **değil** ExpressRoute bağlantı hattı aşağı akar. Varsa alt ağından giden trafiğe zorlamalı tünel şirket içinde her zaman App Service ortamı oluşturma başarısız olur.
* Tüm bu makalede açıklanan uç noktaları için DNS araması düzgün bir şekilde çözün. 

Yapılandırma adımları tamamlandıktan ve rota onaylayın sonra sanal makineyi silin. Alt ağ, App Service ortamı oluştururken "boş" olması gerekiyor.

App Service ortamı dağıtmak artık hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

PowerApps için App Service ortamı ile çalışmaya başlamak için bkz. [App Service Ortamı'na giriş][IntroToAppServiceEnvironment].

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/ 
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/ 
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md 
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ 
[UDROverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/ 
<!-- Old link -- [UDRHowTo]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/ -->

[UDRHowTo]: https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell 
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md 
[AzureDownloads]: https://azure.microsoft.com/downloads/ 
[DownloadCenterAddressRanges]: https://www.microsoft.com/download/details.aspx?id=41653 
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ 
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md 
[NewPortal]:  https://portal.azure.com 


<!-- IMAGES -->
