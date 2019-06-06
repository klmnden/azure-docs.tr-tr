---
title: App Service ortamı - Azure ile ilgili ağ konuları
description: ASE ağ trafiği ve Nsg'ler ve Udr ASE'nizi ile nasıl ayarlanacağı açıklanır
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: b29dec76fb6b1f9883c5c594d4719c9f3032089e
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514633"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>App Service ortamı ağ konuları #

## <a name="overview"></a>Genel Bakış ##

 Azure [App Service ortamı] [ Intro] bir Azure App Service, Azure sanal ağı (VNet) bir alt ağa dağıtımıdır. App Service ortamı (ASE) iki dağıtım türü vardır:

- **Dış ASE**: İnternet'ten erişilebilen bir IP adresi üzerindeki ASE barındırılan uygulamalar sunar. Daha fazla bilgi için [dış ASE oluşturma][MakeExternalASE].
- **ILB ASE**: ASE'de barındırılan uygulamaları üzerinde bir IP adresi, sanal ağ içinde kullanıma sunar. ILB ASE çağırdı neden olan bir iç yük dengeleyici (ILB) iç uç noktadır. Daha fazla bilgi için [oluşturma ve kullanma ILB ASE][MakeILBASE].

Ase'ler, dış ve ILB, tüm gelen yönetim trafiği ve olarak kullanılan genel bir VIP sahip internet ASE çağrıları yapılırken adresinden. İnternet'e gitmesini ASE çağrılarından ASE için atanan VIP ile VNet bırakın. Bu VIP genel IP, kaynak IP ASE'yi internet'e gitmesini tüm çağrıları için gerçekleştirilir. Kaynak IP, ase'deki uygulamalar, VNet veya VPN kaynaklarına çağrıları yaparsanız, ASE'nizi tarafından kullanılan alt ağ IP'ler biridir. ASE sanal ağ içinde olduğundan, ek yapılandırma olmadan sanal ağ içindeki kaynaklara da erişebilirsiniz. Sanal ağ, şirket içi ağınıza bağlıysa, ase'deki uygulamalar var. ek yapılandırma olmadan kaynaklara erişim de.

![Dış ASE][1] 

Dış ASE varsa, genel VIP de ASE uygulamalarınız için çözümlenmesi uç noktadır:

* HTTP/S 
* FTP/S
* Web dağıtımı
* Uzaktan hata ayıklama

![ILB ASE][2]

ILB ASE, ILB adresini HTTP/S, FTP/S, web dağıtımı ve uzaktan hata ayıklama için uç nokta adresidir.

## <a name="ase-subnet-size"></a>ASE alt ağ boyutu ##

Bir ASE barındırmak için kullanılan alt ağ boyutunu, ASE dağıtıldıktan sonra değiştirilemez.  ASE her altyapı rol için de her yalıtılmış App Service planı örneği için bir adres kullanır.  Ayrıca, Azure ağ tarafından oluşturulan her alt ağ için kullanılan beş adresi vardır.  Bir uygulama oluşturmadan önce bir ASE'de App Service planı ile hiç 12 adreslerini kullanır.  ILB ASE etkinleştirilmişse, bu ASE'de uygulama oluşturma önce ardından onu 13 adreslerini kullanır. ASE'nizi ölçeklendirilirken altyapısı rollerinin 15 ve 20 App Service planı örneklerinizin her birden çok eklenir.

   > [!NOTE]
   > Başka bir şey alt ancak ASE olabilir. Gelecekteki Büyümeye izin veren bir adres alanı seçtiğinizden emin olun. Bu ayar daha sonra değiştiremezsiniz. Boyutu öneririz `/24` 256 adreslerine sahip.

Ölçeği artırın veya azaltın, uygun boyutta yeni roller eklenir ve iş yükleriniz için hedef boyutu geçerli boyutunu sonra geçirilir. İş yükleri yalnızca geçirdikten sonra özgün VM'lerin kaldırıldı. 100 ASP örnekleri ile ASE olsaydı olacaktır nokta, iki VM sayısını gereken.  Kullanılmasını öneririz. Bu nedenle olan bir '/ 24' değişiklikleri gerekebilir uyum sağlamak için.  

## <a name="ase-dependencies"></a>ASE bağımlılıkları ##

### <a name="ase-inbound-dependencies"></a>ASE gelen bağımlılıklar ###

ASE gelen bağımlılıklar erişim:

| Kullanım | Başlangıç | Bitiş |
|-----|------|----|
| Yönetim | App Service yönetim adresleri | ASE alt ağı: 454, 455 |
|  ASE iç iletişimi | ASE alt ağı: Tüm bağlantı noktaları | ASE alt ağı: Tüm bağlantı noktaları
|  Azure yük dengeleyici izin gelen | Azure yük dengeleyici | ASE alt ağı: Tüm bağlantı noktaları
|  Uygulama atanmış IP adresleri | Uygulama atanmış adresleri | ASE alt ağı: Tüm bağlantı noktaları

Gelen yönetim trafiğinin komut ve denetim sistemi İzleme ek olarak ase'nin sağlar. Bu trafiğe ait kaynak adresleri listelenen [ASE yönetim adresleri] [ ASEManagement] belge. Tüm IP'lere 454 ve 455 bağlantı noktalarında gelen erişime izin vermek ağ güvenlik yapılandırması gerekir. Bu adreslerden gelen erişimi engellerseniz, ASE'nizi kötü hale gelir ve ardından askıya haline gelir.

ASE alt ağ içinde iç bileşen iletişim için kullanılan birçok bağlantı noktaları vardır ve bunlar değiştirebilirsiniz. Bu, tüm bağlantı noktaları ASE alt ASE alt ağdan erişilebilir olmasını gerektirir. 

Azure load balancer ile ASE alt arasındaki iletişimin açık olması gereken minimum bağlantı noktaları 454 ve 455 16001 içindir. 16001 bağlantı noktası, yük dengeleyiciden hem de ASE arasındaki Canlı canlı akış için kullanılır. ILB ASE kullanmakta olduğunuz sonra trafik 454, 455 16001 aşağı kilitleyebilirsiniz bağlantı noktaları.  Dış ASE kullanıyorsanız, normal uygulama erişim bağlantı noktaları dikkate almanız gerekir.  Uygulama atanmış adresleri kullanıyorsanız, tüm bağlantı noktalarını açmanız gerekir.  Belirli bir uygulama için bir adresi atandığında, yük dengeleyici, önceden ASE için HTTP ve HTTPS trafiğini göndermek için Bilinmeyen bağlantı noktalarını kullanın.

Uygulama atanmış IP adresleri kullanıyorsanız, uygulamalarınıza ASE alt ağına atanmış ıp'lerden trafiği izin vermeniz gerekir.

454 ve 455 bağlantı noktalarında gelen TCP trafiğine geri aynı belirtmediyseniz VIP'yi gider gerekir veya bir asimetrik yönlendirme sorununu olacaktır. 

### <a name="ase-outbound-dependencies"></a>ASE giden bağımlılıklar ###

Giden erişim için bir ASE birden çok dış sisteme bağlıdır. Bu sistem bağımlılıkların çoğu DNS adları ile tanımlanır ve sabit bir IP adresleri kümesini eşleme yok. Bu nedenle, ASE çeşitli bağlantı noktaları tüm dış IP'ler ASE alt ağından giden erişim gerektirir. 

ASE'yi internet erişilebilir adreslerine aşağıdaki bağlantı noktalarını kullanıma iletişim kurar:

| Port | Kullanımlar |
|-----|------|
| 53 | DNS |
| 123 | NTP |
| 80/443 | CRL Windows güncelleştirmeleri, Linux bağımlılıkları, Azure Hizmetleri |
| 1433 | Azure SQL | 
| 12000 | İzleme |

Tam listesi, giden bağımlılıklar açıklayan belgede listelenen [App Service ortamı giden trafiği kilitleme](./firewall-integration.md). ASE bağımlılıklarını erişimi kaybederse, çalışmayı durdurur. Yeterince başardığınızda, ASE askıya alınır. 

### <a name="customer-dns"></a>Müşteri DNS ###

Kiracı iş yüklerini, sanal ağ, müşteri tanımlı bir DNS sunucusu ile yapılandırılmışsa, bunu kullanın. ASE Azure DNS, yönetim amaçları için kullanır. VNet müşteri tarafından seçilen bir DNS sunucusu ile yapılandırılmışsa, DNS sunucusu ASE içeren alt ağından erişilebilir olmalıdır.

Web uygulamanızdan DNS çözümlemesi test etmek için konsol komutunu kullanabilirsiniz *nameresolver*. Scm siteniz için uygulamanızı hata ayıklama penceresine gidin veya Portalı'nda uygulama gidin ve konsolu'nu seçin. Komut kabuğu istemiyle verebilir *nameresolver* aramak istediğiniz DNS adı ile birlikte. Geri alma sonuç aynı arama yaparken uygulamanızı elde edebileceğiniz aynıdır. Nslookup kullanıyorsanız, bunun yerine Azure DNS kullanarak bir arama yapar.

ASE'NİZİN olan sanal ağın DNS ayarı değiştirirseniz, ASE'nizi yeniden başlatmanız gerekir. ASE'NİZİN yeniden başlatılmasını önlemek için ASE'nizi oluşturmak için sanal ağınıza DNS ayarlarınızı yapılandırmak önerilir.  

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portal bağımlılıkları ##

ASE işlevsel bağımlılıkları ek olarak, portal deneyiminizle ilgili bazı ek öğeler vardır. Bazı özellikleri Azure portalında doğrudan erişim bağımlı _SCM sitesine_. Azure App Service'te her uygulama için iki URL vardır. İlk URL, uygulamaya erişmek için kullanılır. İkinci olarak da anılır SCM sitesine erişmek için URL'dir _Kudu konsolunu_. SCM sitesine kullanan özellikler şunlardır:

-   Web işleri
-   İşlevler
-   Günlük akışı
-   Kudu
-   Uzantılar
-   İşlem Gezgini
-   Konsol

Bir ILB ASE'yi kullanırken, SCM sitesine VNet dışından erişilebilir değil. Bunlar, bir uygulamanın SCM sitesine erişim gerektirdiğinden uygulama portaldan bazı özellikler çalışmaz. SCM sitesine portalı yerine doğrudan bağlanabilirsiniz. 

ILB ASE'NİZİN etki alanı adı ise *contoso.appserviceenvironnment.net* ve uygulama adınız *testapp*, uygulamayı en üst sınırına *testapp.contoso.appserviceenvironment.net*. Bunun getirdiği SCM sitesine en üst sınırına *testapp.scm.contoso.appserviceenvironment.net*.

## <a name="ase-ip-addresses"></a>ASE IP adresleri ##

Bir ASE, dikkat edilmesi gereken birkaç IP adresleri bulunur. Bunlar:

- **Gelen genel IP adresi**: Bir dış ase'de uygulama trafiğini ve yönetim trafiğinin dış ASE hem de bir ILB ASE için kullanılır.
- **Giden genel IP**: VPN yönlendirilen olmayan VNet terk giden bağlantılar ASE için "Kimden" IP kullanılır.
- **ILB IP adresi**: ILB ASE ILB IP adresi yalnızca var.
- **Uygulama tarafından atanan IP tabanlı SSL adresleri**: Tek olası dış ASE ve ne zaman IP tabanlı SSL yapılandırılır.

Bu IP adresleri, ASE kullanıcı Arabiriminden Azure Portalı'nda görülebilir. ILB ASE, ILB için IP listelenir.

   > [!NOTE]
   > ASE'NİZİN çalışmaya kalır sürece bu IP adresleri değiştirmez.  ASE'NİZİN askıya alındı ve geri olur, ASE'nizi tarafından kullanılan adres değişir. Normal bir ASE askıya gelen yönetim erişimi engellemeye veya ASE bağımlılık erişimi engelleme nedeni. 

![IP adresleri][3]

### <a name="app-assigned-ip-addresses"></a>Uygulama tarafından atanan IP adresleri ###

Dış ASE ile tek tek uygulamalar için IP adresleri atayabilirsiniz. Bir ILB ASE ile bunu yapamazsınız. Kendi IP adresi sağlamak için uygulamanızı yapılandırma hakkında daha fazla bilgi için bkz. [mevcut bir özel SSL sertifikasını Azure App Service'e bağlama](../app-service-web-tutorial-custom-ssl.md).

ASE, uygulama kendi IP tabanlı SSL adresi sahip olduğunda, bu IP adresine eşlemek için iki bağlantı noktası ayırır. Bir bağlantı noktası HTTP trafiği için ve HTTPS için diğer bağlantı noktasıdır. Bu bağlantı noktalarını ASE kullanıcı Arabirimine IP adresleri bölümünde listelenir. Trafik Bu bağlantı noktalarını VIP'yi ulaşabilir veya uygulamaları erişilemez. Bu gereksinim, ağ güvenlik grupları (Nsg'ler) yapılandırdığınızda unutmamak önemlidir.

## <a name="network-security-groups"></a>Ağ Güvenlik Grupları ##

[Ağ güvenlik grupları] [ NSGs] sanal ağ içindeki ağ erişimi denetleme olanağı sağlar. Portalı kullandığınızda, her şeyi reddetmek için en düşük öncelikte bir örtük izin verme kuralı yok. Hangi derleme olan, kuralları izin verir.

Bir ASE'de, ASE'nin barındırmak için kullanılan sanal makineleri için erişiminiz yok. Bunlar, Microsoft tarafından yönetilen bir abonelikte hedeflenmiştir. ASE üzerinde uygulamalara erişimi kısıtlamak istiyorsanız, ASE alt ağda Nsg ayarlayın. Bunu yaparken dikkatli ASE bağımlılık dikkat edin. Herhangi bir bağımlılığın engellerseniz, ASE çalışmayı durduruyor.

Nsg'ler, Azure portalı üzerinden veya PowerShell aracılığıyla yapılandırılabilir. Buradaki bilgiler, Azure portalında gösterir. Oluşturma ve altında en üst düzey bir kaynak olarak portalda Nsg'leri yönetme **ağ**.

İşlevi, bir ASE için bir NSG gerekli girişleri trafiğine izin verecek şekilde şunlardır:

**Gelen**
* IP etiketi AppServiceManagement 454,455 bağlantı noktalarında hizmet
* 16001 numaralı bağlantı noktasında yük dengeleyiciden
* ASE alt ağına tüm bağlantı noktalarındaki ASE alt ağdan

**Giden**
* için bağlantı noktası 123 tüm IP'ler
* için bağlantı noktası 80, 443 üzerindeki tüm IP'ler
* IP etiketi AzureSQL 1433 numaralı bağlantı noktalarında hizmet
* 12000 numaralı bağlantı noktasındaki tüm IP'ler için
* ASE alt ağına tüm bağlantı noktaları

DNS trafiği, NSG kuralları tarafından etkilenmez olarak eklenecek DNS bağlantı noktası gerekmez. Bu bağlantı noktaları, başarılı kullanılmak üzere uygulamalarınızın gerektirdiği bağlantı noktalarını içermez. Normal uygulama erişim bağlantı noktaları şunlardır:

| Kullanım | Başlangıç | Bitiş |
|----------|---------|-------------|
|  HTTP/HTTPS  | Kullanıcı tarafından yapılandırılabilir |  80, 443 |
|  FTP/FTPS    | Kullanıcı tarafından yapılandırılabilir |  21, 990, 10001-10020 |
|  Visual Studio uzaktan hata ayıklama  |  Kullanıcı tarafından yapılandırılabilir |  4020, 4022, 4024 |
|  Web hizmeti dağıtma | Kullanıcı tarafından yapılandırılabilir | 8172 |

Gelen ve giden gereksinimleri dikkate alındığında, Nsg'leri Bu örnekte gösterilen Nsg'ler benzemelidir. 

![Gelen güvenlik kuralları][4]

Varsayılan kural, ASE alt ağına konuşmaya sanal IP'ler sağlar. Başka bir varsayılan kural, ASE ile iletişim kurmak için genel VIP olarak da bilinen yük dengeleyici sağlar. Varsayılan kuralları görmek için seçin **varsayılan kuralları** yanındaki **Ekle** simgesi. Diğer her şey kural önce varsayılan kuralları reddetme yerleştirirseniz, VIP ve ASE arasındaki trafiği engeller. Sanal ağ içinde gelen trafiği engellemek için izin veren kendi gelen kuralı ekleyin. Azureloadbalancer'a eşit olan bir kaynak hedefi ile kullanmak **herhangi** ve bir bağlantı noktası aralığını **\*** . ASE alt ağa bir NSG kuralı uygulandığından, hedefte belirli olması gerekmez.

Uygulamanız için bir IP adresi atanmışsa, bağlantı noktalarının açık tutulması emin olun. Bağlantı noktalarını görmek için seçin **App Service ortamı** > **IP adresleri**.  

Aşağıdaki giden kurallar gösterilen tüm öğeler dışında son öğe gereklidir. Bunlar, bu makalenin önceki bölümlerinde belirtilenlerle ASE bağımlılıkları ağ erişimini etkinleştirin. Bunların hiçbirine engellerseniz ASE'niz çalışmayı durduruyor. Listedeki son öğeyi ASE'NİZİN sanal ağınızdaki diğer kaynaklarla iletişim sağlar.

![Giden güvenlik kuralları][5]

Nsg'lerinizi tanımlandıktan sonra ASE'nizi bulunduğu alt ağa atayın. ASE VNet veya alt hatırlamıyorsanız ASE portal sayfasında görebilirsiniz. NSG, alt ağa atamak için kullanıcı Arabirimi alt ağa gidin ve NSG seçin.

## <a name="routes"></a>Yollar ##

Giden trafiğin doğrudan internet ancak bir ExpressRoute ağ geçidiyle veya bir sanal Gereci gibi başka bir yere gitmek olmayan şekilde, yollar ağınızda zorlamalı tünel olur.  ASE'nizi bir şekilde yapılandırmanız gerekiyorsa, belgeyi okumaya devam [App Service ortamınızı zorlamalı tünel ile yapılandırma][forcedtunnel].  ExpressRoute ve zorlamalı tünel ile çalışmak için kullanılabilir seçenekleri bu belgede size bildirir.

Portalda bir ASE oluşturduğunuzda da rota tabloları kümesi ASE ile oluşturulan alt ağdaki oluştururuz.  Doğrudan internet'e giden trafik göndermek olan yollar yalnızca varsayalım.  
Aynı yollar el ile oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalına gidin. Seçin **ağ** > **rota tabloları**.

2. Sanal ağınızı aynı bölgede yeni bir yol tablosu oluşturun.

3. Seçin, kullanıcı Arabirimi, rota tablosu içindeki **yollar** > **Ekle**.

4. Ayarlama **sonraki atlama türü** için **Internet** ve **adres ön eki** için **0.0.0.0/0**. **Kaydet**’i seçin.

    Ardından aşağıdaki gibi bir şey görürsünüz:

    ![İşlevsel yolları][6]

5. Yeni bir yol tablosu oluşturduktan sonra ASE'nizi içeren alt ağa gidin. Yol tablonuz portalındaki listeden seçin. Değişiklikleri kaydettikten sonra alt ağ ile belirtilen yollar ve Nsg'ler ardından görmeniz gerekir.

    ![Nsg'leri ve yolları][7]

## <a name="service-endpoints"></a>Hizmet Uç Noktaları ##

Hizmet Uç Noktaları, çok kiracılı hizmetlere erişimi bir dizi Azure sanal ağı ve alt ağı ile kısıtlamanızı sağlar. [Sanal Ağ Hizmet Uç Noktaları][serviceendpoints] belgelerinde Hizmet Uç Noktaları hakkında daha fazla bilgi edinebilirsiniz. 

Bir kaynakta Hizmet Uç Noktalarını etkinleştirdiğinizde, diğer tüm yönlendiricilerden daha yüksek öncelikle oluşturulmuş rotalar vardır. Hizmet uç noktaları ile zorlamalı tünel ASE, herhangi bir Azure hizmeti kullanırsanız, bu hizmetleri trafiği değil zorlanır tünel. 

Azure SQL örneği içeren bir alt ağda Hizmet Uç Noktaları etkinleştirilirse, o alt ağa veya alt ağdan bağlanan tüm Azure SQL örnekleri için Hizmet Uç Noktaları etkinleştirilmiş olmalıdır. Aynı alt ağdan birden fazla Azure SQL örneğine erişmek istiyorsanız, tek bir Azure SQL örneğinde Hizmet Uç Noktalarını etkinleştirebilir, başka bir örnekte etkinleştiremezsiniz. Diğer Azure Hizmetleri, hizmet uç noktaları ile ilgili Azure SQL gibi davranır. Azure Depolama ile Hizmet Uç Noktalarını etkinleştirdiğinizde, alt ağınızdan o kaynağa erişimi kilitlersiniz, ancak Hizmet Uç Noktaları etkinleştirilmiş olmasa da diğer Azure Depolama hesaplarından erişmeye devam edebilirsiniz.  

![Hizmet Uç Noktaları][8]

<!--Image references-->
[1]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow.png
[2]: ./media/network_considerations_with_an_app_service_environment/networkase-overflow2.png
[3]: ./media/network_considerations_with_an_app_service_environment/networkase-ipaddresses.png
[4]: ./media/network_considerations_with_an_app_service_environment/networkase-inboundnsg.png
[5]: ./media/network_considerations_with_an_app_service_environment/networkase-outboundnsg.png
[6]: ./media/network_considerations_with_an_app_service_environment/networkase-udr.png
[7]: ./media/network_considerations_with_an_app_service_environment/networkase-subnet.png
[8]: ./media/network_considerations_with_an_app_service_environment/serviceendpoint.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[forcedtunnel]: ./forced-tunnel-support.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
