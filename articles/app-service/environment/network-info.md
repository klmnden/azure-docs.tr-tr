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
ms.date: 03/14/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 73175b326c25d5d9a78155d0d9d888b655da1bfd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61226861"
---
# <a name="networking-considerations-for-an-app-service-environment"></a>App Service ortamı ağ konuları #

## <a name="overview"></a>Genel Bakış ##

 Azure [App Service ortamı] [ Intro] bir Azure App Service, Azure sanal ağı (VNet) bir alt ağa dağıtımıdır. App Service ortamı (ASE) iki dağıtım türü vardır:

- **Dış ASE**: İnternet'ten erişilebilen bir IP adresi üzerindeki ASE barındırılan uygulamalar sunar. Daha fazla bilgi için [dış ASE oluşturma][MakeExternalASE].
- **ILB ASE**: ASE'de barındırılan uygulamaları üzerinde bir IP adresi, sanal ağ içinde kullanıma sunar. ILB ASE çağırdı neden olan bir iç yük dengeleyici (ILB) iç uç noktadır. Daha fazla bilgi için [oluşturma ve kullanma ILB ASE][MakeILBASE].

App Service ortamının iki sürümü vardır: ASEv1 ve ASEv2. ASEv1 hakkında daha fazla bilgi için bkz: [App Service ortamı v1 giriş][ASEv1Intro]. ASEv1, Klasik veya Resource Manager Vnet'i dağıtılabilir. ASEv2 yalnızca bir Resource Manager Vnet'e dağıtılabilir.

Bir ASE'yi internet'e gitmesini tüm çağrıları ASE için atanmış bir VIP arasında VNet bırakın. Bu VIP genel IP, kaynak IP ASE'yi internet'e gitmesini tüm çağrıları için gerçekleştirilir. Kaynak IP, ase'deki uygulamalar, VNet veya VPN kaynaklarına çağrıları yaparsanız, ASE'nizi tarafından kullanılan alt ağ IP'ler biridir. ASE sanal ağ içinde olduğundan, ek yapılandırma olmadan sanal ağ içindeki kaynaklara da erişebilirsiniz. Sanal ağ, şirket içi ağınıza bağlıysa, ase'deki uygulamalar var. ek yapılandırma olmadan kaynaklara erişim de.

![Dış ASE][1] 

Dış ASE varsa, genel VIP de ASE uygulamalarınız için çözümlenmesi uç noktadır:

* HTTP/S 
* FTP/S. 
* Web dağıtımı.
* Uzaktan hata ayıklama.

![ILB ASE][2]

ILB ASE, ILB adresini HTTP/S, FTP/S, web dağıtımı ve uzaktan hata ayıklama için uç noktasıdır.

Normal uygulama erişim bağlantı noktaları şunlardır:

| Kullanım | Başlangıç fiyatı | Alıcı |
|----------|---------|-------------|
|  HTTP/HTTPS  | Kullanıcı tarafından yapılandırılabilir |  80, 443 |
|  FTP/FTPS    | Kullanıcı tarafından yapılandırılabilir |  21, 990, 10001-10020 |
|  Visual Studio uzaktan hata ayıklama  |  Kullanıcı tarafından yapılandırılabilir |  4020, 4022, 4024 |
|  Web hizmeti dağıtma | Kullanıcı tarafından yapılandırılabilir | 8172 |

Bu, dış ASE veya ILB ASE kullanıyorsanız geçerlidir. Bir dış ASE'de kullanıyorsanız bu bağlantı noktaları genel VIP basın. ILB ASE'de kullanıyorsanız bu bağlantı noktalarına ILB basın. 443 numaralı bağlantı noktasından kilitlerseniz, portalda kullanıma sunulan bazı özellikler üzerinde bir etkisi olabilir. Daha fazla bilgi için [portalı bağımlılıkları](#portaldep).

## <a name="ase-subnet-size"></a>ASE alt ağ boyutu ##

Bir ASE barındırmak için kullanılan alt ağın boyutu ASE dağıtıldıktan sonra değiştirilemez.  ASE her altyapı rol için de her yalıtılmış App Service planı örneği için bir adres kullanır.  Ayrıca, Azure ağ tarafından oluşturulan her alt ağ için kullanılan 5 adresi vardır.  Bir uygulama oluşturmadan önce bir ASE'de App Service planı ile hiç 12 adreslerini kullanır.  ILB ASE ise bu ASE'de uygulama oluşturma önce ardından onu 13 adreslerini kullanır. ASE'nizi ölçeklendirilirken altyapısı rollerinin 15 ve 20 App Service planı örneklerinizin her birden çok eklenir.

   > [!NOTE]
   > Başka bir şey alt ancak ASE olabilir. Gelecekteki Büyümeye izin veren bir adres alanı seçtiğinizden emin olun. Bu ayar daha sonra değiştiremezsiniz. Boyutu öneririz `/24` 256 adreslerine sahip.

Ölçeği artırın veya azaltın, uygun boyutta yeni roller eklenir ve iş yükleriniz için hedef boyutu geçerli boyutunu sonra geçirilir. Uygulamalarınızı yalnızca geçirildikten sonra özgün sanal makinelere kaldırılır. Bu, 100 ASP örnekleri ile ASE olsaydı olacağını nokta, iki VM sayısını gereken anlamına gelir.  Kullanılmasını öneririz. Bu nedenle olan bir '/ 24' değişiklikleri gerekebilir uyum sağlamak için.  

## <a name="ase-dependencies"></a>ASE bağımlılıkları ##

### <a name="ase-inbound-dependencies"></a>ASE gelen bağımlılıklar ###

ASE gelen bağımlılıklar erişim:

| Kullanım | Başlangıç fiyatı | Alıcı |
|-----|------|----|
| Yönetim | App Service yönetim adresleri | ASE alt ağı: 454, 455 |
|  ASE iç iletişimi | ASE alt ağı: Tüm bağlantı noktaları | ASE alt ağı: Tüm bağlantı noktaları
|  Azure yük dengeleyici izin gelen | Azure yük dengeleyici | ASE alt ağı: Tüm bağlantı noktaları
|  Uygulama atanmış IP adresleri | Uygulama atanmış adresleri | ASE alt ağı: Tüm bağlantı noktaları

Gelen yönetim trafiğinin komut ve denetim sistemi İzleme ek olarak ase'nin sağlar. Bu trafiğe ait kaynak adresleri listelenen [ASE yönetim adresleri] [ ASEManagement] belge. Tüm IP'lere 454 ve 455 bağlantı noktalarında gelen erişime izin vermek ağ güvenlik yapılandırması gerekir. Bu adreslerden gelen erişimi engellerseniz, ASE'nizi kötü hale gelir ve ardından askıya haline gelir.

İç bileşen için kullanılan birçok bağlantı noktaları olduğundan ASE alt ağ içinde iletişim ve değiştirebilirsiniz.  Bu, tüm bağlantı noktaları ASE alt ASE alt ağdan erişilebilir olmasını gerektirir. 

Azure load balancer ile ASE alt arasındaki iletişimin açık olması gereken minimum bağlantı noktaları 454 ve 455 16001 içindir. 16001 bağlantı noktası, yük dengeleyiciden hem de ASE arasındaki Canlı canlı akış için kullanılır. ILB ASE kullanmakta olduğunuz sonra trafik 454, 455 16001 aşağı kilitleyebilirsiniz bağlantı noktaları.  Dış ASE kullanıyorsanız normal uygulama erişim bağlantı noktaları dikkate almanız gerekir.  Atanan uygulama adresleri kullanıyorsanız, tüm bağlantı noktalarını açmanız gerekir.  Belirli bir uygulama için bir adresi atandığında, yük dengeleyici, önceden ASE için HTTP ve HTTPS trafiğini göndermek için Bilinmeyen bağlantı noktalarını kullanın.

Uygulamayı atanmış uygulamalarınıza ASE alt ağına atanmış ıp'lerden trafiğine izin vermek için ihtiyacınız olan IP adresleri kullanıyorsa.

454 ve 455 bağlantı noktalarında gelen TCP trafiğine geri aynı belirtmediyseniz VIP'yi gider gerekir veya bir asimetrik yönlendirme sorununu olacaktır. 

### <a name="ase-outbound-dependencies"></a>ASE giden bağımlılıklar ###

Giden erişim için bir ASE birden çok dış sisteme bağlıdır. Bu sistem bağımlılıkların çoğu DNS adları ile tanımlanır ve sabit bir IP adresleri kümesini eşleme yok. Bu nedenle, ASE çeşitli bağlantı noktaları tüm dış IP'ler ASE alt ağından giden erişim gerektirir. 

Tam listesi, giden bağımlılıklar açıklayan belgede listelenen [App Service ortamı giden trafiği kilitleme](./firewall-integration.md). ASE bağımlılıklarını erişimi kaybederse, çalışmayı durdurur. Yeterince başardığınızda, ASE askıya alınır. 

### <a name="customer-dns"></a>Müşteri DNS ###

Kiracı iş yüklerini, sanal ağ, müşteri tanımlı bir DNS sunucusu ile yapılandırılmışsa, bunu kullanın. ASE, yönetim amaçları için Azure DNS ile iletişim kurmak hala gerekir. 

VNet DNS diğer tarafındaki VPN müşteri ile yapılandırılmışsa, DNS sunucusu ASE içeren alt ağdan erişilebilir olmalıdır.

Web uygulamanızdan çözümü test etmek için konsol komutunu kullanabilirsiniz *nameresolver*. Scm siteniz için uygulamanızı hata ayıklama penceresine gidin veya Portalı'nda uygulama gidin ve konsolu'nu seçin. Komut kabuğu istemiyle verebilir *nameresolver* aramak istediğiniz adres yanı sıra. Geri alma sonuç aynı arama yaparken uygulamanızı elde edebileceğiniz aynıdır. Nslookup kullanıyorsanız, bunun yerine Azure DNS kullanarak bir arama yapar.

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

Bir ILB ASE'yi kullanırken, SCM sitesine İnternet'e VNet dışından erişilebilir değil. Uygulamanızı bir ILB ASE üzerinde barındırıldığında, portaldan bazı özellikler çalışmaz.  

SCM sitesine bağımlı bu işlevlerin birçoğunu de doğrudan Kudu konsolunda mevcuttur. Dosyayı doğrudan yerine portalını kullanarak bağlanabilirsiniz. ILB ASE'de uygulama barındırılıyorsa, oturum açmak için yayımlama kimlik bilgilerinizi kullanın. ILB ASE'de barındırılan bir uygulamanın SCM sitesine erişmek için URL'si aşağıdaki biçime sahiptir: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

ILB ASE'NİZİN etki alanı adı ise *contoso.net* ve uygulama adınız *testapp*, uygulamayı en üst sınırına *testapp.contoso.net*. Bunun getirdiği SCM sitesine en üst sınırına *testapp.scm.contoso.net*.

## <a name="functions-and-web-jobs"></a>İşlevler ve Web işleri ##

Hem işlevler hem de Web işleri üzerinde SCM sitesine bağlıdır ancak tarayıcınız SCM sitesine ulaşabileceği sürece ILB ASE'de, uygulamalarınızı olsa bile, portalı kullanmak için desteklenir.  ILB ASE'nizi bir otomatik olarak imzalanan sertifika kullanıyorsanız, o sertifikaya güvenmek için tarayıcınızı etkinleştirmeniz gerekir.  IE ve sertifika anlamına gelir Microsoft Edge için bilgisayar güven deposunda olması gerekir.  Tarayıcıdaki sertifika daha önce büyük olasılıkla scm sitesine doğrudan tuşlarına basarak kabul ettiğiniz anlamına gelir Chrome kullanıyorsanız.  En iyi çözüm, tarayıcının güven zincirinde olan ticari bir sertifika kullanmaktır.  

## <a name="ase-ip-addresses"></a>ASE IP adresleri ##

Bir ASE, dikkat edilmesi gereken birkaç IP adresleri bulunur. Bunlar:

- **Gelen genel IP adresi**: Bir dış ase'de uygulama trafiğini ve yönetim trafiğinin dış ASE hem de bir ILB ASE için kullanılır.
- **Giden genel IP**: VPN yönlendirilen olmayan VNet terk giden bağlantılar ASE için "Kimden" IP kullanılır.
- **ILB IP adresi**: ILB ASE kullanıyorsanız.
- **Uygulama tarafından atanan IP tabanlı SSL adresleri**: Tek olası dış ASE ve ne zaman IP tabanlı SSL yapılandırılır.

Bu IP adresleri ASE kullanıcı Arabiriminden kolayca Azure portalında bir ASEv2'nda görülebilir. ILB ASE, ILB için IP listelenir.

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

Gelen ve giden gereksinimleri dikkate alındığında, Nsg'leri Bu örnekte gösterilen Nsg'ler benzemelidir. Sanal ağ adres aralığı _192.168.250.0/23_, ve ASE'nin bulunduğu alt ağ _192.168.251.128/25_.

Bu örnekte, listenin üst kısmındaki ASE işlevi için ilk iki gelen gereksinimlerini gösterilmektedir. Bunlar ASE yönetimini etkinleştirme ve ASE kendisi ile iletişim kurmasına izin verin. Diğer girişler tüm Kiracı yapılandırılabilir ve ASE barındırılan uygulamalar için ağ erişimi yönetebilir. 

![Gelen güvenlik kuralları][4]

Varsayılan kural, ASE alt ağına konuşmaya sanal IP'ler sağlar. Başka bir varsayılan kural, ASE ile iletişim kurmak için genel VIP olarak da bilinen yük dengeleyici sağlar. Varsayılan kuralları görmek için seçin **varsayılan kuralları** yanındaki **Ekle** simgesi. Diğer her şey kural gösterilen NSG kuralları sonra reddetme yerleştirirseniz, VIP ve ASE arasındaki trafiği engeller. Sanal ağ içinde gelen trafiği engellemek için izin veren kendi gelen kuralı ekleyin. Azureloadbalancer'a eşit olan bir kaynak hedefi ile kullanmak **herhangi** ve bir bağlantı noktası aralığını **\***. ASE alt ağa bir NSG kuralı uygulandığından, hedefte belirli olması gerekmez.

Uygulamanız için bir IP adresi atanmışsa, bağlantı noktalarının açık tutulması emin olun. Bağlantı noktalarını görmek için seçin **App Service ortamı** > **IP adresleri**.  

Aşağıdaki giden kurallar gösterilen tüm öğeler dışında son öğe gereklidir. Bunlar, bu makalenin önceki bölümlerinde belirtilenlerle ASE bağımlılıkları ağ erişimini etkinleştirin. Bunların hiçbirine engellerseniz ASE'niz çalışmayı durduruyor. Listedeki son öğeyi ASE'NİZİN sanal ağınızdaki diğer kaynaklarla iletişim sağlar.

![Giden güvenlik kuralları][5]

Nsg'lerinizi tanımlandıktan sonra ASE'nizi bulunduğu alt ağa atayın. ASE VNet veya alt hatırlamıyorsanız ASE portal sayfasında görebilirsiniz. NSG, alt ağa atamak için kullanıcı Arabirimi alt ağa gidin ve NSG seçin.

## <a name="routes"></a>Yollar ##

Giden trafiğin doğrudan internet ancak bir ExpressRoute ağ geçidiyle veya bir sanal Gereci gibi başka bir yere gitmek olmayan şekilde, yollar ağınızda zorlamalı tünel olur.  Böyle bir şekilde ASE'nizi yapılandırma sonra belgeyi okumaya devam etmeniz gerekiyorsa [App Service ortamınızı zorlamalı tünel ile yapılandırma][forcedtunnel].  ExpressRoute ve zorlamalı tünel ile çalışmak için kullanılabilir seçenekleri bu belgede size bildirir.

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

Bir kaynakta Hizmet Uç Noktalarını etkinleştirdiğinizde, diğer tüm yönlendiricilerden daha yüksek öncelikle oluşturulmuş rotalar vardır. ASE’ye zorlamalı tünel uygulanmış şekilde Hizmet Uç Noktalarını kullanırsanız, Azure SQL ve Azure Depolama yönetimi trafiğine zorlamalı tünel uygulanmaz. 

Azure SQL örneği içeren bir alt ağda Hizmet Uç Noktaları etkinleştirilirse, o alt ağa veya alt ağdan bağlanan tüm Azure SQL örnekleri için Hizmet Uç Noktaları etkinleştirilmiş olmalıdır. Aynı alt ağdan birden fazla Azure SQL örneğine erişmek istiyorsanız, tek bir Azure SQL örneğinde Hizmet Uç Noktalarını etkinleştirebilir, başka bir örnekte etkinleştiremezsiniz. Azure Depolama, Azure SQL ile aynı şekilde hareket etmez. Azure Depolama ile Hizmet Uç Noktalarını etkinleştirdiğinizde, alt ağınızdan o kaynağa erişimi kilitlersiniz, ancak Hizmet Uç Noktaları etkinleştirilmiş olmasa da diğer Azure Depolama hesaplarından erişmeye devam edebilirsiniz.  

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
