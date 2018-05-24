---
title: Azure uygulama hizmeti ortamı ile ağ konuları
description: Ana ağ trafiği ve Nsg'ler ve Udr'ler, ana ile nasıl ayarlanacağı açıklanır
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
ms.date: 03/20/2018
ms.author: ccompy
ms.openlocfilehash: d099163cdc34624afd8f01b8f1978c5ee902d1ff
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="networking-considerations-for-an-app-service-environment"></a>Bir uygulama hizmeti ortamı için ağ konuları #

## <a name="overview"></a>Genel Bakış ##

 Azure [uygulama hizmeti ortamı] [ Intro] bir Azure App Service, Azure sanal ağınıza (VNet) bir alt ağ içinde dağıtımıdır. Uygulama hizmeti ortamı (ana) için iki dağıtım türleri şunlardır:

- **Dış ana**: internet'ten erişilebilen bir IP adresi üzerinde ana barındırılan uygulamalar kullanıma sunar. Daha fazla bilgi için bkz: [bir dış ana oluşturma][MakeExternalASE].
- **ILB ana**: ana barındırılan uygulamalar üzerinde bir IP adresi sanal ağınızın içinde kullanıma sunar. Bir ILB ana çağırdı neden olan bir iç yük dengeleyici (ILB) iç uç noktadır. Daha fazla bilgi için bkz: [oluşturma ve kullanma bir ILB ana][MakeILBASE].

Artık uygulama hizmeti ortamı iki sürümü vardır: ASEv1 ve ASEv2. ASEv1 hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı v1 giriş][ASEv1Intro]. ASEv1 Klasik veya Resource Manager Vnet'i dağıtılabilir. ASEv2 yalnızca Resource Manager Vnet'i dağıtılabilir.

İnternet'e giden tüm çağrılarından bir ana sanal ağ için ana atanmış bir VIP aracılığıyla bırakın. Bu VIP genel IP ana gelen internet'e gidin tüm çağrıları için kaynak IP ise. Kaynak IP, ana uygulamalar sanal ağınızın içinde veya bir VPN kaynaklara çağrıları yaparsanız, ana tarafından kullanılan alt ağdaki IP biridir. Ana sanal ağ içinde olduğundan, ayrıca herhangi bir ek yapılandırma olmadan sanal ağ içindeki kaynaklara erişebilir. VNet şirket içi ağınıza bağlanırsa, ana uygulamalarında kaynaklarına erişimi de vardır'e sahip. Her ana ya da uygulamanızı daha fazla yapılandırmak gerekmez.

![Dış ana][1] 

Bir dış ana varsa, genel VIP ana uygulamalarınız için çözümlemek uç nokta da şöyledir:

* HTTP/S'DİR. 
* FTP/S'DİR. 
* Web dağıtımı.
* Uzaktan hata ayıklama.

![ILB ANA][2]

Bir ILB ana varsa, ILB'nin IP adresini HTTP/S, FTP/sn, web dağıtımı ve uzaktan hata ayıklama için uç noktadır.

Normal uygulama erişim bağlantı noktaları şunlardır:

| Kullanım | Kimden | Alıcı |
|----------|---------|-------------|
|  HTTP/HTTPS  | Kullanıcı tarafından yapılandırılabilir |  80, 443 |
|  FTP/FTPS    | Kullanıcı tarafından yapılandırılabilir |  21, 990, 10001-10020 |
|  Visual Studio uzaktan hata ayıklama  |  Kullanıcı tarafından yapılandırılabilir |  4016, 4018, 4020, 4022 |

Bu, bir dış ana ya da bir ILB ana iseniz geçerlidir. Bir dış ana üzerinde değilseniz, bu bağlantı noktalarına genel VIP ulaştı. Bir ILB ana değilseniz, bu bağlantı noktalarına ILB ulaştı. Bağlantı noktası 443 kilitlemek portalda kullanıma bazı özellikler üzerinde bir etkisi olabilir. Daha fazla bilgi için bkz: [Portal bağımlılıkları](#portaldep).

## <a name="ase-subnet-size"></a>ANA alt ağ boyutu ##

Ana dağıtıldıktan sonra bir ana barındırmak için kullanılan alt ağ boyutunu değiştirilemez.  Ana her altyapı rolü için de her yalıtılmış uygulama hizmeti plan örneği için bir adres kullanır.  Ayrıca, Azure ağ tarafından oluşturulan her alt ağ için kullanılan 5 adresleri vardır.  Bir uygulamayı oluşturmadan önce bir ana uygulama hizmeti planı ile hiç 12 adreslerini kullanır.  Bir ILB ana ise, o ana bir uygulamayı oluşturmadan önce sonra onu 13 adreslerini kullanır. Uygulama hizmeti planlarınızı ölçeklendirirken her ön eklenen uç için ek adresleri gerektirecektir.  Varsayılan olarak, her 15 toplam uygulama hizmeti planı örnek için ön uç sunucularına eklenir. 

   > [!NOTE]
   > Hiçbir şey alt ancak ana olabilir. Gelecekteki büyümeyi de izin veren bir adres alanı seçtiğinizden emin olun. Bu ayarı daha sonra değiştiremezsiniz. Dosya boyutu öneririz `/25` 128 adreslerine sahip.

## <a name="ase-dependencies"></a>Ana bağımlılıkları ##

Bir ana gelen erişim bağımlılık:

| Kullanım | Kimden | Alıcı |
|-----|------|----|
| Yönetim | App Service management adresleri | ANA alt: 454, 455 |
|  ANA iç iletişim | ANA alt: tüm bağlantı noktaları | ANA alt: tüm bağlantı noktaları
|  Azure yük dengeleyici izin gelen | Azure yük dengeleyici | ANA alt: tüm bağlantı noktaları
|  Uygulama atanmış IP adresleri | Atanmış adresleri uygulama | ANA alt: tüm bağlantı noktaları

Gelen trafik komut ve sistem izleme yanı sıra ana denetim sağlar. Bu trafik kaynağı IP'leri içinde listelenen [adresleri ana Yönetimi] [ ASEManagement] belge. Tüm IP'ler 454 ve 455 bağlantı noktalarında gelen erişime izin verecek şekilde ağ güvenliği yapılandırması gerekir.

İç bileşeni için kullanılan pek çok bağlantı noktaları olduğundan ana alt ağ içindeki iletişim ve bunlar değiştirebilirsiniz.  Bu, tüm bağlantı noktaları ana alt ağdaki ana alt ağ üzerinden erişilebilir olmasını gerektirir. 

Azure yük dengeleyici ve ana alt ağ arasındaki iletişimin açık olması gereken minimum bağlantı noktaları 454 ve 455 16001 içindir. 16001 bağlantı noktası, yük dengeleyici ve ana arasındaki Koru canlı akış için kullanılır. Bir ILB ana kullandığınız sonra yalnızca 454, 455, 16001 aşağıya doğru trafiği kilitleyebilirsiniz bağlantı noktaları.  Bir dış ana kullanıyorsanız, normal uygulama erişim bağlantı noktaları dikkate almanız gerekir.  Atanan uygulamayı adresleri kullanıyorsanız, tüm bağlantı noktalarını açmanız gerekir.  Belirli bir uygulamayı bir adresi atandığında, yük dengeleyici, önceden HTTP ve HTTPS trafiği için ana göndermek için Bilinmeyen bağlantı noktalarını kullanır.

Atanmış IP adresleri uygulamalarınızı ana alt ağa atanan IP gelen trafiğe izin verecek şekilde gereken uygulama kullanıyorsanız.

Giden erişim için bir ana birden çok dış sistemlerde bağlıdır. Bu sistem bağımlılıkların DNS adları ile tanımlanır ve sabit bir IP adresleri kümesini eşleme yok. Bu nedenle, ana çeşitli bağlantı noktaları tüm dış IP ana alt ağdan giden erişim gerektirir. Bir ana aşağıdaki giden bağımlılıklara sahiptir:

| Kullanım | Kimden | Alıcı |
|-----|------|----|
| Azure Storage | ANA alt ağ | Table.Core.Windows.NET, blob.core.windows.net, queue.core.windows.net, file.core.windows.net: 80, 443, 445 (445 yalnızca gereklidir ASEv1 için.) |
| Azure SQL Database | ANA alt ağ | Database.Windows.NET: 1433 11000 11999, 14000 14999 (daha fazla bilgi için bkz: [SQL Database V12 bağlantı noktası kullanımı](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).)|
| Azure Yönetimi | ANA alt ağ | Management.Core.Windows.NET, management.azure.com: 443 
| SSL sertifika doğrulama |  ANA alt ağ            |  OCSP.msocsp.com, mscrl.microsoft.com, crl.microsoft.com: 443
| Azure Active Directory        | ANA alt ağ            |  Internet: 443
| Uygulama Hizmeti Yönetimi        | ANA alt ağ            |  Internet: 443
| Azure DNS                     | ANA alt ağ            |  Internet: 53
| ANA iç iletişim    | ANA alt: tüm bağlantı noktaları |  ANA alt: tüm bağlantı noktaları

Ana Bu bağımlılıklar erişimi kaybederse, çalışmayı durdurur. Bu yetecek kadar uzun süre durum oluştuğunda ana askıya alındı.

### <a name="customer-dns"></a>Müşteri DNS ###

VNet müşteri tanımlı bir DNS sunucusu ile yapılandırılmışsa, Kiracı İş yükleri bunu kullanın. Ana hala yönetim amacıyla Azure DNS ile iletişim kurması gerekiyor. 

VNet DNS diğer tarafındaki bir VPN müşteri ile yapılandırılmışsa, DNS sunucusu ana içeren alt ağ üzerinden erişilebilir olması gerekir.

<a name="portaldep"></a>

## <a name="portal-dependencies"></a>Portal bağımlılıkları ##

ANA işlevsel bağımlılıkları ek olarak, portal deneyimiyle ilgili birkaç ek öğeler vardır. Bazı Azure portalında özelliklerini doğrudan erişim bağımlı _SCM site_. Azure App Service'te her uygulama için iki URL'leri vardır. İlk uygulamanızı erişmek için URL'dir. İkinci olarak da adlandırılır SCM sitesine erişmek için URL'dir _Kudu konsol_. SCM site kullanan özellikler şunlardır:

-   Web işleri
-   İşlevler
-   Günlük akışı
-   Kudu
-   Uzantılar
-   İşlem Gezgini
-   Konsol

Bir ILB ana kullandığınızda, SCM site internet sanal ağ dışında erişilebilir değildir. Uygulamanızı bir ILB ana barındırıldığında, bazı özellikleri portaldan çalışmaz.  

Birçok SCM site bağımlı bu yetenekleri de doğrudan Kudu konsolunda kullanılabilir. Bunu doğrudan yerine portalını kullanarak bağlanabilir. Uygulamanızı ILB ASE'de barındırılıyorsa, oturum açmak için yayımlama kimlik bilgilerinizi kullanın. ILB ASE'de barındırılan bir uygulamanın SCM sitesine erişmek için URL'si aşağıdaki biçime sahiptir: 

```
<appname>.scm.<domain name the ILB ASE was created with> 
```

ILB ana etki alanı adı ise *contoso.net* ve uygulama adınız *testapp*, uygulamanın en üst sınırına *testapp.contoso.net*. İle gidiyor SCM sitenin en üst sınırına *testapp.scm.contoso.net*.

## <a name="functions-and-web-jobs"></a>İşlevler ve Web işleri ##

İşlevler ve Web işleri SCM siteye bağımlı olan ancak tarayıcınız SCM sitenin ulaşabileceği sürece, uygulamalarınızı ILB ASE'de olsa bile, portalı kullanmak için desteklenir.  İle ILB ana kendinden imzalı bir sertifika kullanıyorsanız, bu sertifika güven için tarayıcınızı etkinleştirmeniz gerekir.  IE ve sertifika anlamına gelir kenar için bilgisayar güven deposunda olması gerekir.  Tarayıcıda sertifika önceki büyük olasılıkla scm sitenin doğrudan basarsa tarafından kabul ettiğiniz anlamına gelir sonra Chrome kullanıyorsanız.  En iyi çözüm, güven tarayıcı zinciri ticari bir sertifika kullanmaktır.  

## <a name="ase-ip-addresses"></a>ANA IP adresleri ##

Bir ana dikkat edilmesi gereken birkaç IP adresi vardır. Bunlar:

- **Gelen ortak IP adresi**: bir dış ana uygulama trafiği ve bir dış ana ve bir ILB ana yönetim trafiği için kullanılır.
- **Giden genel IP**: hangi VPN yönlendirilen değil, VNet bırakın giden bağlantılar ana "Kimden" IP olarak kullanılacak.
- **ILB IP adresi**: bir ILB ana kullanıyorsanız.
- **Uygulama tarafından atanan IP tabanlı SSL adresleri**: bir dış ana ve IP tabanlı SSL yapılandırıldığında mümkün.

Bu IP adreslerine ana Arabiriminden kolayca Azure portalında bir ASEv2 içinde görülebilir. Bir ILB ana varsa, ILB'nin IP listelenir.

![IP adresleri][3]

### <a name="app-assigned-ip-addresses"></a>Uygulama tarafından atanan IP adresleri ###

Bir dış ana ile tek tek uygulamalar için IP adresleri atayabilirsiniz. Bir ILB ana ile yapamazsınız. Kendi IP adresini için uygulamanızı yapılandırma hakkında daha fazla bilgi için bkz: [Azure web uygulamaları için var olan özel bir SSL sertifikası bağlama](../app-service-web-tutorial-custom-ssl.md).

Uygulama kendi IP tabanlı SSL adresine sahipse, ana o IP adresine eşlemek için iki bağlantı noktası ayırır. Bir bağlantı noktası HTTP trafiği için ve diğer bağlantı noktası için HTTPS şeklindedir. Bu bağlantı noktaları ana kullanıcı arabiriminde IP adresleri bölümünde listelenir. Uygulamaları erişilemeyen veya trafiği bağlantı noktalarından VIP ulaşabilir olmalıdır. Bu gereksinim, ağ güvenlik grupları (Nsg'ler) yapılandırdığınızda unutmayın.

## <a name="network-security-groups"></a>Ağ Güvenlik Grupları ##

[Ağ güvenlik grupları] [ NSGs] bir sanal ağ içindeki ağ erişimi denetleme olanağı sağlar. Portalı'nı kullandığınızda, her şeyi reddetmek için en düşük öncelikli bir örtük izin verme kuralı yoktur. Derleme olan, kuralları izin verir.

ASE'de, ana barındırmak için kullanılan sanal makineleri erişimi yok. Bunlar, Microsoft tarafından yönetilen bir abonelikte demektir. Ana uygulamaları için erişimi kısıtlamak istiyorsanız, ana alt ağda Nsg'ler ayarlayın. Bunu yaparken dikkatli ana bağımlılıkları dikkat edin. Bağımlılıkları engellerseniz, ana çalışmayı durdurur.

Nsg'ler, Azure portal veya PowerShell aracılığıyla yapılandırılabilir. Burada yer alan bilgiler Azure Portalı'nı gösterir. Oluşturma ve Nsg'ler altında en üst düzey bir kaynak olarak portalda yönetme **ağ**.

Gelen ve giden gereksinimlerini dikkate alınır, Nsg'ler Bu örnekte gösterilen Nsg'ler benzemelidir. VNet adres aralığı _192.168.250.0/23_, ve ana bulunduğu alt ağ _192.168.251.128/25_.

Bu örnekte liste üstündeki ana işlevi ilk iki gelen gereksinimlerini gösterilmektedir. Bunlar ana yönetimini etkinleştirme ve ana kendisiyle iletişim kurmasına izin verin. Diğer girişler tüm Kiracı yapılandırılabilir ve ana barındırılan uygulamalar için ağ erişimi yönetebilirler. 

![Gelen güvenlik kuralları][4]

Varsayılan kural ana alt ağına anlaşmak için sanal ağ içindeki IP sağlar. Başka bir varsayılan kural ana ile iletişim kurmak için ortak VIP olarak da bilinen yük dengeleyici sağlar. Varsayılan kuralları görmek için seçin **varsayılan kuralları** yanına **Ekle** simgesi. NSG gösterilen kuralları sonra şey kural reddetme put ise, VIP ve ana arasındaki trafiği engeller. VNet içinde gelen trafiği engellemek için izin veren kendi gelen kuralı ekleyin. Bir kaynak için AzureLoadBalancer eşit hedefi ile kullanmak **herhangi** ve bir bağlantı noktası aralığı **\***. NSG kuralının ana alt ağına uygulandığından hedef belirli olması gerekmez.

Uygulamanız için bir IP adresi atanmışsa, bağlantı noktalarını açık tutmak emin olun. Bağlantı noktaları görmek için seçin **uygulama hizmeti ortamı** > **IP adreslerini**.  

Aşağıdaki giden kurallar gösterilen tüm öğeler, son öğenin dışında gereklidir. Bunlar, bu makalenin önceki bölümlerinde belirtilenlerle ana bağımlılıkları ağ erişimini etkinleştirir. Bunlardan herhangi birinin engellerseniz, ana çalışmayı durdurur. Listesindeki son öğeyi kullanarak sanal ağınızdaki diğer kaynakları ile iletişim kurmak, ana sağlar.

![Giden güvenlik kuralları][5]

Nsg'lerinizi tanımlandıktan sonra ana açıktır alt ağa atayın. Ana sanal ağ veya alt ağ hatırlamıyorsanız ana portal sayfasından görebilirsiniz. Alt ağınız NSG atamak için kullanıcı Arabirimi alt ağa gidin ve NSG seçin.

## <a name="routes"></a>Yollar ##

Giden trafiği doğrudan internet ancak bir ExpressRoute ağ geçidi veya bir sanal gereç gibi başka bir yere gidin olmayan şekilde, yollar, sanal ağınızda zorlamalı tünel olur.  Bu tür bir şekilde, ana yapılandırın ardından belgeyi okumaya devam gerekiyorsa [zorlamalı tüneli ile uygulama hizmeti ortamınızı yapılandırma][forcedtunnel].  ExpressRoute ve zorlamalı tünel çalışmak için kullanılabilir seçenekleri bu belge size bildirir.

Bir ana portalında oluşturduğunuzda da yönlendirme tabloları kümesi ile ana oluşturulan alt ağdaki oluşturuyoruz.  Bu yollar yalnızca doğrudan internet'e giden trafiği göndermesine izin söyleyin.  
Aynı yollar el ile oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalına gidin. Seçin **ağ** > **yol tablosu**.

2. Sanal ağınızı aynı bölgede yeni bir yol tablosu oluşturun.

3. Kullanıcı Arabirimi, rota tablosu içindeki seçin **yollar** > **Ekle**.

4. Ayarlama **sonraki atlama türü** için **Internet** ve **adres ön eki** için **0.0.0.0/0**. **Kaydet**’i seçin.

    Ardından aşağıdaki gibi bir şey görürsünüz:

    ![İşlev yolları][6]

5. Yeni yol tablosu oluşturduktan sonra ana içeren alt ağa gidin. Yol tablosu portal listeden seçin. Değişiklik kaydettikten sonra alt ağ ile belirtilen yollar ve Nsg'ler sonra görmeniz gerekir.

    ![Nsg'ler ve yolları][7]

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
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ASEManagement]: ./management-addresses.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[forcedtunnel]: ./forced-tunnel-support.md
[serviceendpoints]: ../../virtual-network/virtual-network-service-endpoints-overview.md
