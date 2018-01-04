---
title: "Bir Azure sanal ağı ile bir uygulamayı tümleştirin"
description: "Bir uygulamayı Azure App Service'te bir yeni veya var olan Azure sanal ağa bağlanmak nasıl gösterir"
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: cephalin
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: ccompy
<<<<<<< HEAD
ms.openlocfilehash: 72ff0c13319218f8ef91aff9208772fcb0fd9459
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: d285e63e64d8f4a260c45143f0ae3f7fddd4a2b6
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı Azure sanal ağı ile tümleştirme
Bu belgede Azure App Service sanal ağ tümleştirme özelliğini açıklar ve uygulamalar ile ayarlanması gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Azure sanal ağlar (Vnet'ler) ile bilginiz yoksa, bu Azure kaynaklarınızı çoğunu erişimi denetlemek Internet olmayan routeable ağ yerleştirmek izin veren bir yetenektir. Bu ağlar sonra VPN teknolojileri çeşitli kullanarak, şirket içi ağlara bağlanabilir. Azure sanal ağlar hakkında daha fazla bilgi edinmek için burada bilgilerle Başlat: [Azure Virtual Network'e genel bakış][VNETOverview]. 

Azure uygulama hizmeti iki tür vardır. 

1. Fiyatlandırma planı tam kapsamlı destekleyen çok kiracılı sistemleri
2. Ağınızı dağıtır uygulama hizmeti ortamı (ana) premium özelliği. 

Bu belge, VNet tümleştirme ve uygulama hizmeti ortamı gider. ANA özelliği hakkında daha fazla bilgi edinmek istiyorsanız, buradaki bilgiler ile Başlat: [uygulama hizmeti ortamı giriş][ASEintro].

VNet tümleştirme, web app sanal ağınızdaki kaynaklara erişmenizi sağlar ancak özel erişim, web uygulamanızın sanal ağdan vermez. Özel site erişimi uygulamanızı özel ağdan gibi bir Azure sanal ağı içinde yalnızca erişilebilir hale getirmek için ifade eder. Özel site yalnızca bir dahili yük dengeleyici (ILB) ile yapılandırılmış bir ana ile erişilebilir. Bir ILB ana kullanılması hakkında daha fazla ayrıntı için buraya makale ile başlayın: [oluşturma ve bir ILB ana kullanarak][ILBASE]. 

VNet tümleştirme nerede kullanacağınız yaygın bir senaryo bir veritabanı veya Azure sanal ağınızda bir sanal makinede çalışan bir web hizmeti, web uygulamasından erişimi etkinleştirme. VNet tümleşme VM ancak can uygulamaları özel dışındaki Internet yönlendirilebilir adresleri kullanmanız için genel bir uç nokta kullanıma gerekmez. 

VNet tümleştirme özelliği:

* Standart, Premium ya da planı fiyatlandırma Isolated gerektirir 
* Klasik veya Resource Manager Vnet'i ile çalışır 
* TCP ve UDP destekler
* çalışır ile Web, mobil, API uygulamaları ve işlev uygulamalarının
* aynı anda yalnızca 1 Vnet'e bağlanmak bir uygulama sağlar
* bir uygulama hizmeti planı'nda tümleştirilmesini ile en fazla beş sanal ağlar sağlar 
* bir uygulama hizmeti planında birden çok uygulamalar tarafından kullanılmak üzere aynı Vnet'i sağlar
* sanal ağ geçidinde SLA nedeniyle % 99,9 SLA destekler

VNet tümleştirme de dahil olmak üzere desteklemiyor bazı şeyler vardır:

* bir sürücü bağlama
* AD tümleştirmesi 
* NetBIOS
* Özel site erişimi

### <a name="getting-started"></a>Başlarken
Web uygulamanızı bir sanal ağa bağlanmadan önce göz önünde bulundurmanız gereken bazı şeyler şunlardır:

* VNet tümleştirme yalnızca çalışır uygulamaları ile bir **standart**, **Premium**, veya **Isolated** planı fiyatlandırma. Özelliği etkinleştirmek ve uygulama hizmet planınızdaki uygulamalarınızı kullanmakta olduğunuz sanal ağlara bağlantıları kesilir desteklenmeyen fiyatlandırma planı ölçeklendirmek istiyorsanız. 
* Hedef sanal ağınız zaten varsa, noktadan siteye VPN ile dinamik yönlendirme ağ geçidi etkin bir uygulama için bağlı önce olmalıdır. Ağ geçidi, statik yönlendirme ile yapılandırılmışsa, noktadan siteye sanal özel ağ (VPN) etkinleştiremezsiniz.
* Sanal ağ, App Service Plan(ASP) ile aynı abonelikte olması gerekir. 
* Bir VNet ile tümleştirmek uygulamalar, bu sanal ağ için belirtilen DNS kullanır.
* Varsayılan olarak tümleştirme uygulamalarınızı trafiği, sanal ağınızda tanımlanan rotaları göre sanal ağınızı içine yalnızca rota. 

## <a name="enabling-vnet-integration"></a>VNet Integration etkinleştirme

Uygulamanızı yeni veya var olan bir sanal ağa bağlanmak için seçeneğiniz vardır. Yeni bir ağ tümleştirmenize bir parçası olarak oluşturursanız, yalnızca sanal ağ oluşturmaya ek olarak, dinamik yönlendirme ağ geçidi sizin için önceden yapılandırılmıştır ve Site VPN noktasına etkindir. 

> [!NOTE]
> Yeni bir sanal ağ tümleştirme yapılandırması birkaç dakika sürebilir. 
> 
> 

VNet tümleştirmeyi etkinleştirmek için uygulamanızın ayarlarını açın ve ardından ağ seçin. Açılan UI üç ağ seçenek sunar. Bu kılavuz yalnızca VNet tümleştirmeye ancak karma bağlantılar geçiyor ve App Service ortamları bu belgenin sonraki bölümlerinde ele alınmaktadır. 

Uygulamanız doğru durumda planı fiyatlandırma olduğunu değil, kullanıcı arabirimini daha yüksek bir fiyatlandırma Planı tercih ettiğiniz planınıza ölçeklendirmenizi sağlar.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Önceden var olan bir VNet ile VNet tümleştirme etkinleştirme
VNet tümleştirme UI, sanal ağlar listesinden seçmenize olanak sağlar. Klasik sanal ağlar oldukları gibi VNet adının yanında parantez içinde "Klasik" sözcüğüyle olduğunu gösterir. Resource Manager sanal ağlar ilk listelenen şekilde liste sıralanır. Aşağıda gösterilen görüntüdeki yalnızca bir VNet seçilebilir görebilirsiniz. Bir sanal ağ dahil olmak üzere çıkışı gri olduğunu birden çok nedeni vardır:

* hesabınıza erişimi olan başka bir abonelik sanal ağ kullanılıyor
* sanal ağ sitesine etkin noktası yok
* VNet dinamik yönlendirme ağ geçidi yok

![][2]

Etkinleştirmek için tümleştirme tıklamanız yeterlidir, ile tümleştirmek istediğiniz VNet üzerinde. VNet seçtikten sonra değişikliklerin etkili olması için uygulamanızı otomatik olarak yeniden başlatılır. 

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Klasik bir VNet sitede noktasının etkinleştir
Sanal ağınızı bir ağ geçidi yok veya siteye noktası sahip değilse, ilk ayarlamanız gerekir. Klasik bir VNet için bunu yapmak için şu adrese gidin [Azure portal] [ AzurePortal] ve sanal Networks(classic) listesi ayarlamak duruma getirin. Buradan, tümleştirileceğini ve VPN bağlantıları adlı Essentials altındaki büyük kutuya tıklayın istediğiniz ağı tıklayın. Buradan, VPN site ve bir ağ geçidi oluşturmak bile sağlamak için noktanız oluşturabilirsiniz. Site ağ geçidi oluşturma deneyimi ile noktasından gidin sonra yaklaşık 30 dakika önce hazır olur. 

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Resource Manager Vnet'i sitede noktasına etkinleştirme
Bir ağ geçidi ve Site noktasına sahip bir Resource Manager Vnet'i yapılandırmak için her iki PowerShell belirtildiği gibi burada kullanabilirsiniz [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma] [ V2VNETP2S] veya burada açıklandığı gibi Azure portalını kullanma [Azure portalını kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma][V2VNETPortal]. Bu özellik gerçekleştirmek için kullanıcı Arabirimi henüz kullanılabilir değil. Site Yapılandırması noktasına için sertifikaları oluşturmanız gerekmez unutmayın. Vnet'e, WebApp bağlandığında otomatik olarak yapılandırılır. 

### <a name="creating-a-pre-configured-vnet"></a>Önceden yapılandırılmış bir sanal ağ oluşturma
Bir ağ geçidi ile yapılandırılmış yeni bir VNet ve noktadan siteye oluşturmak istiyorsanız, uygulama kullanıcı Arabirimi ağ hizmeti yalnızca Resource Manager Vnet'i için bunu yapmak için yeteneğine sahiptir. Bir ağ geçidi ve noktadan siteye klasik bir VNet oluşturmak istiyorsanız, bu ağ kullanıcı arabirimi aracılığıyla el ile yapmanız gerekir. 

Resource Manager Vnet'i Vnet'e tümleştirme kullanıcı Arabirimi aracılığıyla oluşturmak için seçmeniz yeterlidir **yeni sanal ağ oluştur** ve sağlar:

* Sanal ağ adı
* Sanal Ağ Adresi Bloğu
* Alt Ağ Adı
* Alt Ağ Adres Bloğu
* Ağ geçidi adres bloğu
* Noktadan Siteye Adres Bloğu

Başka bir ağa bağlanmak için bu VNet istiyorsanız, bu ağ ile çakışıyor IP adresi alanı çekme kaçınmanız gerekir. 

> [!NOTE]
> Bir ağ geçidi ile Resource Manager Vnet'i oluşturulması yaklaşık 30 dakika sürer ve şu anda VNet uygulamanızla tümleştirin değil. Sanal ağınızın ağ geçidi ile oluşturulduktan sonra uygulamaya VNet tümleştirme UI geri dönün ve yeni sanal ağınızı seçmek gerekir.
> 
> 

![][3]

Azure sanal özel ağ adresleri içinde normal olarak oluşturulur. Varsayılan olarak VNet tümleştirme özelliği bu IP adresi aralıklarını sanal ağınızı hedefleyen trafik yönlendirir. Özel IP adres aralıklarını şunlardır:

* -Bu aynıdır 10.0.0.0 - 10.0.0.0/8 10.255.255.255
* -Bu aynıdır 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
* -Bu aynıdır 192.168.0.0 - 192.168.0.0/16 192.168.255.255

VNet adres alanı CIDR gösteriminde belirtilmelidir. İle CIDR gösteriminde bilginiz yoksa, bu adres blokları IP adresi ve ağ maskesi temsil eden bir tamsayı kullanarak belirtmek için bir yöntemdir. Hızlı başvuru olarak 10.1.0.0/24 256 adresleri olacaktır ve 10.1.0.0/25 128 adresleri olacağını göz önünde bulundurun. Bir /32 bir IPv4 adresiyle yalnızca 1 adresi olacaktır. 

DNS sunucusu bilgileri burada ayarlarsanız, ağınız için ayarlanır. VNet oluşturulduktan sonra VNet kullanıcı deneyimleri bu bilgileri düzenleyebilirsiniz. VNet DNS değiştirirseniz, bir eşitleme ağ işlemi gerçekleştirmek için gerekir.

Klasik bir VNet tümleştirme kullanıcı arabirimini kullanarak VNet oluşturduğunuzda, uygulamanız ile aynı kaynak grubunda bir sanal ağ oluşturur. 

## <a name="how-the-system-works"></a>Sistem nasıl çalışır?
Uygulamanızı Vnet'iniz bağlanmak için noktadan siteye VPN teknolojisi üstünde bu özellik perde arkasında oluşturur. Azure uygulama hizmetinde uygulamaları bir uygulamada doğrudan bir sanal ağ sanal makinelerle gerçekleştirilir gibi sağlama önleyen bir çok kiracılı sistem mimarisi vardır. Noktadan siteye teknolojisine oluşturarak, biz yalnızca uygulamayı barındıran sanal makine ağ erişimi sınırlayın. Uygulamalarınızı erişmeleri için yapılandırdığınız ağlar yalnızca erişebilmesi için ağ erişimi daha fazla bu uygulama konaklarda sınırlıdır. 

![][4]

Bir DNS sunucusu ile sanal ağınızı yapılandırmadıysanız, uygulamanızın VNet kaynak ulaşmak için IP adreslerini kullanmak gerekir. IP adresleri kullanırken, bu özellik önemli faydası, özel ağınızdaki özel adresler kullanmanızı sağlar olduğunu unutmayın. Uygulamanızı ayarlarsanız kullanacak Vm'leriniz biri için ortak IP adresleri, sonra VNet tümleştirme özelliği kullanmadığınız ve Internet üzerinden iletişim kuran.

## <a name="managing-the-vnet-integrations"></a>VNet tümleştirmeler yönetme
Bağlanmak ve bir sanal ağa bağlantısını kesmek için bir uygulama düzeyinde yeteneğidir. Birden çok uygulama arasında VNet tümleştirme etkileyen bir ASP düzeyinde işlemleridir. Uygulama düzeyinde gösterilen UI, sanal ağınızda ayrıntılarını alabilirsiniz. Aynı bilgilerin çoğunu ASP düzeyinde da gösterilmektedir. 

![][5]

Ağ özellik durumu sayfasından uygulamanızı Vnet'iniz bağlı olup olmadığını görebilirsiniz. Sanal ağ geçidiniz için herhangi bir nedenle çalışmıyorsa, ardından bu değil bağlı olarak gösterir. 

Şimdi de uygulama düzeyinde VNet tümleştirme UI ASP alma hakkında ayrıntılı bilgi ile aynıdır elinizdeki bilgileri. Bu öğeler şunlardır:

* Azure sanal ağı UI VNet adı - bu bağlantıyı açar
* Konumu - bu ağınızı konumunu yansıtır. Başka bir konumda bir VNet ile tümleştirmek mümkündür.
* Sertifika durumu - VPN bağlantısı uygulama ve sanal ağ arasında güvenli hale getirmek için kullanılan sertifikaları vardır. Bu eşitlenmiş olduklarından emin olmak için bir test yansıtır.
* Ağ geçidi durumu -, ağ geçitleri için herhangi bir nedenle aşağı olmalıdır sonra uygulamanızı VNet içindeki kaynaklara erişemez. 
* VNet adres alanı - Vnet'inizi için IP adresi alanını budur. 
* Noktası Site adres alanı - site IP adres alanı sanal ağınızı noktasına budur. Uygulamanız bu adres alanındaki IP birinden gelen olarak iletişimi gösterir. 
* Site adres alanı - siteye ağınızı şirket içi kaynaklarınıza veya diğer sanal ağa bağlanmak için siteden siteye VPN'ler kullanabilirsiniz. VPN bağlantısı burada gösterir IP aralıkları ile tanımlanmış sonra yapılandırılmış olması gerekir.
* DNS sunucuları - burada listelenen sonra VNet ile yapılandırılmış DNS sunucuları varsa.
* Vnet'e - orada yönlendirilmiş IP de Vnet'inizi için tanımlanan yönlendirme olan IP adresleri ve bu adresleri göster listesi. 

Uygulamanız şu anda bağlı olduğu sanal ağ bağlantısını kesmek için sanal ağ tümleştirmesinin uygulama görünümünde gerçekleştirebileceğiniz tek işlem var. Yapmak için bu tıklamanız yeterlidir bağlantıyı kes en üstünde. Bu eylem, sanal ağınızı değiştirmez. Sanal ağ ve ağ geçitleri de dahil olmak üzere yapılandırmasıyla değişmeden kalır. Ardından sanal ağınızı silmek istiyorsanız, önce ağ geçitleri de dahil olmak üzere kaynakları silmeniz gerekir. 

Uygulama hizmeti planı görünümün birkaç ek işlem vardır. Bu aynı zamanda farklı daha uygulamadan erişilebilir. Ulaşmak için ağ ASP UI açmanız yeterlidir ASP UI ve kaydırma kapalı. Ağ özellik durumu adlı bir kullanıcı Arabirimi öğesi yok. VNet tümleştirme geçici küçük bazı ayrıntılar sağlar. Bu kullanıcı Arabirimi üzerindeki tıklatarak Ağ özellik durumu kullanıcı arabirimini açar. "Yönetmek için üzerinde burayı tıklatın"'i tıklatırsanız bu ASP VNet tümleştirmeler listeler kullanıcı arabirimini açar.

![][6]

ASP ile tümleştirme Vnet konumlarında bakarken anımsaması iyi konumudur. VNet başka bir konumda olduğunda ne kadar büyük olasılıkla gecikmesi sorunlarını görürsünüz. 

Sanal ağlar ile tümleşik uygulamalarınızı ile bu ASP ve kaç tane sağlayabilirsiniz tümleşik olduğu bir anımsatıcı kaç tane sanal ağlar üzerinde değil. 

Her VNet üzerinde eklenen ayrıntıları görmek için yalnızca ilgilendiğiniz VNet tıklayın. Daha önce not ettiğiniz Ayrıntılar ek olarak, bu ASP, VNet kullanarak uygulamaların bir listesini görebilirsiniz. 

Eylemler göre iki birincil eylemler vardır. İlk sanal ağınızı uygulamanızı trafiğe sürücü yollar ekleme yeteneğidir. Sertifikaları ve ağ bilgilerini eşitleme becerisi ikinci eylemdir.

![][7]

**Yönlendirme** , sanal ağınızda tanımlanan yollar, ne sanal ağınızı uygulamanızdan içine trafiği yönlendirerek için kullanılan daha önce belirtildiği gibi. Müşteriler ek giden trafiği bir uygulamadan VNet içine ve bunlar için bu özelliği göndermek istediğiniz yere sağlanan karşın bazı kullanımları vardır. Müşteri kendi sanal nasıl yapılandırır kadar olan sonra ne trafiği olur. 

**Sertifikaları** VPN bağlantısı için kullanıyoruz sertifikaların iyi olduğunu doğrulamak için App Service tarafından gerçekleştirilen bir onay sertifika durumunu yansıtır. VNet tümleştirme etkinleştirildiğinde, ardından bu ilk tümleştirme bu Vnet'e bu ASP tüm uygulamalardan ise var. gerekli bir bağlantının güvenliğini sağlamak için sertifika alışverişi Sertifikalar ile birlikte DNS yapılandırmasını, yollar ve ağ açıklayan benzer başka şeyler alın.
Bu sertifikalar veya ağ bilgilerini değiştirdiyseniz "Eşitleme ağ" tıklatın gerekir. **Not**: kısa bir kesinti uygulamanızı ve ağınızı arasında Bağlantısı'nda neden "Eşitleme ağ" tıklattığınızda. Uygulamanızı yeniden başlatılmamış durumdayken bağlantı kaybı düzgün sitenize çalışmamasına neden olabilir. 

## <a name="accessing-on-premises-resources"></a>Erişme şirket içi kaynakları
Uygulamalarınızı şirket içi kaynaklarınıza uygulamanızdan erişebileceğini sonra sanal ağınızı şirket içi ağınıza bir siteden siteye VPN ile bağlıysa, VNet tümleştirme özelliği avantajlarından biri olmasıdır. Bunu şirket içi VPN ağ geçidinizi noktanızın Site IP için yollar güncelleştirmeniz gerekebilir olsa çalışması aralık. Siteden siteye VPN ilk olarak ayarlandığında, daha sonra yapılandırmak için kullanılan komut noktanızın Site VPN dahil olmak üzere yollar ayarlamanız gerekir. Siteden siteye VPN oluşturduktan sonra Site VPN noktası eklerseniz, yollarını el ile güncelleştirmeniz gerekir. Bunun nasıl yapılacağını ayrıntıları her geçidi değişir ve burada açıklanmamaktadır. 

> [!NOTE]
> VNet tümleştirme özelliği, bir ExpressRoute ağ geçidi sahip bir VNet ile birlikte bir uygulamayı tümleştirin değil. ExpressRoute ağ geçidi olarak yapılandırılmış olsa bile [bir arada bulunma modu] [ VPNERCoex] VNet tümleştirme çalışmıyor. ExpressRoute bağlantısı kaynaklara erişmeye ihtiyacınız sonra kullanabileceğiniz bir [uygulama hizmeti ortamı][ASE], sanal ağınızda çalışır.
> 
> 

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Birkaç vardır VNet tümleştirme özelliği kullanırken bilmeniz gereken nüanslarını fiyatlandırma. Bu özelliğin kullanımının 3 ilişkili giderleri vardır:

* ASP fiyatlandırma katmanı gereksinimleri
* Veri aktarım maliyetleri
* VPN ağ geçidi maliyetleri.

Bu özelliği kullanabilmek, uygulamalarınız için bunlar standart veya Premium App Service planı içinde olması gerekir. Burada bu maliyetlerini hakkında daha fazla ayrıntı görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 

Site VPN'ler noktası şekilde kaynaklanır VNet aynı veri merkezinde olsa bile işlenmiş, her zaman giden veriler için bir ücret VNet tümleştirme bağlantınızı sahip. Bu giderleri nelerdir görmek için buraya bakın: [veri aktarım fiyatlandırma ayrıntıları][DataPricing]. 

Son öğeyi VNet ağ geçitleri maliyetidir. Siteden siteye VPN gibi başka bir şey için ağ geçitleri gerek duymuyorsanız, VNet tümleştirme özelliği desteklemek ağ geçitleri için ödeme yapıyorsanız. Burada bu maliyetlerinde ayrıntıları vardır: [VPN ağ geçidi fiyatlandırma][VNETPricing]. 

## <a name="troubleshooting"></a>Sorun giderme
Özelliği ayarlamak kolay olmakla birlikte, deneyiminizi sorun boş olacaktır anlamına gelmez. Karşılaşmamanız gerekir istenen uç noktanızı var. erişme uygulama konsol bağlantısını test etme için kullanabileceğiniz bazı yardımcı programları sorunlardır. Kullanabileceğiniz iki konsol deneyimleri vardır. Kudu konsolundan biridir ve diğer Azure portalında ulaşabileceği konsoludur. Kudu konsola uygulamanızdan almak için Araçlar -> Kudu. Bu [sitename] giderek ile aynı olur. scm.azurewebsites.net. Yalnızca açılır sonra hata ayıklama Konsolu sekmesine gidin. Azure portal barındırılan konsola sonra uygulamanızdan almak için Araçlar -> konsol. 

#### <a name="tools"></a>Araçlar
Araçlar ping, nslookup ve tracert güvenlik kısıtlamaları nedeniyle konsolu üzerinden çalışmaz. Doldurmak için void orada olmuştur iki ayrı araçları eklendi. DNS işlevselliğini test etmek için nameresolver.exe adında bir aracı eklediğimiz. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Uygulamanızı bağlıdır ana bilgisayar adları denetlemek için nameresolver kullanabilirsiniz. DNS ile yanlış yapılandırılmış herhangi bir şey varsa veya belki de, DNS sunucusuna erişiminiz yoksa bu şekilde, test edebilirsiniz.

Sonraki aracı ana bilgisayarı ve bağlantı noktası bileşimi için TCP bağlantısı için test etmenizi sağlar. Bu araç tcpping.exe olarak adlandırılır ve sözdizimi aşağıdaki gibidir:

    tcpping.exe hostname [optional: port]

Tcpping yardımcı programı, belirli bir ana bilgisayarı ve bağlantı noktası ulaşmak varsa bildirir. Yalnızca başarı gösterebilir,: ana bilgisayar ve bağlantı noktası bileşimi dinleyen bir uygulamanın yoktur ve bağlantı noktası ve belirtilen konak uygulamanızdan ağ erişimi yoktur.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Barındırılan kaynaklara erişim vnet'e hata ayıklama
Uygulamanızı belirli bir ana bilgisayarı ve bağlantı noktası ulaşmasını engellemeniz şey vardır. Çoğu zaman üç şey biridir:

* **Biçiminde bir güvenlik duvarı olduğundan** biçiminde bir güvenlik duvarınız varsa, TCP zaman aşımı karşılaşır. 21 saniye bu durumda olur. Bağlantıyı sınamak için tcpping Aracı'nı kullanın. TCP zaman aşımı, güvenlik duvarları ötesinde pek çok nedeni olabilir ancak var. başlatın. 
* **DNS erişilebilir değil** DNS zaman aşımı olan üç saniyede bir DNS sunucusu. İki DNS sunucusu varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Sanal ağınızı ile yapılandırılmış DNS kullanmaz nslookup kullanamazsınız unutmayın.
* **Geçersiz P2S IP aralığı** site IP aralığı noktasına RFC 1918 özel IP aralıkları olması gerekir (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Aralık dışında IP'leri kullanıyorsa şeyler çalışmaz. 

Bu öğeleri sorununuzu alamıyorsanız, gibi basit şeyleri için ilk bakış: 

* Ağ geçidi portalda yukarı olduğu gösteriyor mu?
* Sertifikaları eşit olacak şekilde gösteriyor?
* Bir "eşitleme ağında" Etkilenen ASP yapmadan herkes ağ yapılandırmasını değiştirmek mi? 

Ağ geçidi kapalı ise, getirmeniz yedekleyin. Sertifikalarınızı eşitlenmemiş varsa, VNet tümleştirme ASP görünümüne gidin ve "Eşitleme ağ" isabet. VNet yapılandırmanızı yapılan bir değişikliği açıldı şüpheleniyorsanız ve eşitleme değildi ile yaptığınız, ASP sonra VNet tümleştirme ASP görünümüne gidin ve "Eşitleme ağ" sadece bir anımsatıcı olarak isabet, bu kısa bir kesinti VNet bağlantınızı ve uygulamalarınızı neden olur. 

Tümünü ise sorun, biraz daha derin derinliklerine gerekir:

* Aynı sanal kaynaklara ulaşmak için VNet tümleştirme kullanan diğer uygulamalar var mı? 
* Uygulama konsoluna gidin ve ağınızı alanındaki herhangi bir kaynağa erişmek için tcpping kullanılsın mı? 

Yukarıdaki biri doğruysa, sonra VNet tümleştirme uygundur ve başka bir yere bir sorundur. Burada ana bilgisayar: bağlantı noktası neden ulaşamıyor görmek için basit bir yolu olduğundan daha zor olarak alır budur. Bazı nedenler şunlardır:

* bir güvenlik duvarı erişim noktanızın site IP aralığına uygulama bağlantı noktasına engelleyen ana bilgisayarınızda çalışır duruma getirdikten. Çapraz alt ağlar genellikle ortak erişim gerektirir.
* Hedef ana bilgisayar kullanılamıyor
* Uygulamanızı çalışmıyor
* yanlış IP veya ana bilgisayar adı vardı
* Uygulamanızı beklediğiniz daha farklı bir bağlantı noktasında dinliyor. Bunu barındıran giderek ve "netstat - aon" kullanarak komut isteminden denetleyebilirsiniz. Bu kimliği hangi bağlantı noktası üzerinde dinleme hangi işlemin gösterir. 
* Ağ güvenlik grupları bunlar erişim uygulama ana bilgisayarı ve bağlantı noktasına noktanızın site IP aralığına engellemek, şekilde yapılandırılır

Hangi IP noktanız tüm aralığı erişime izin verecek şekilde gerekir böylece uygulamanız kullanacağı Site IP aralığına tanımadığınız olduğunu unutmayın. 

Ek hata ayıklama adımları içerir:

* Sanal ağınızı başka bir VM'de oturum ve kaynak konak: bağlantı noktanızın buradan ulaşmaya çalışır. Bazı TCP ping yardımcı programları vardır bu amaçla kullanabileceğiniz veya telnet değilse bile kullanabilirsiniz olması gerekir. Burada yalnızca bağlantısı bu bir sanal makineden olup olmadığını belirlemek için amaçtır. 
* bir uygulamayı başka bir VM ve test erişim o ana bilgisayar ve bağlantı noktası uygulamanızdan konsolundan Getir

#### <a name="on-premises-resources"></a>Şirket içi kaynakları ####
Uygulamanızı kaynakları şirket erişemiyorsa, denetlemeniz gereken ilk şey, sanal ağınızda ulaşabileceği bir kaynak olur. VM sanal ağ ile şirket içi uygulamasından erişmek, çalışıyorsa, deneyin. Telnet veya TCP ping yardımcı programını kullanabilirsiniz. VM, şirket içi kaynağa bağlanamazsa, siteden siteye VPN bağlantınızın çalıştığından emin olun. Çalışıyorsa, şirket içi ağ geçidi yapılandırma ve durum yanı sıra daha önce not ettiğiniz aynı şeyleri denetleyin. 

Şimdi sanal ağınızı barındırılıyorsa VM şirket içi sisteminizi ulaşabilir ancak nedeni büyük olasılıkla aşağıdakilerden birini sonra uygulamanızı olamaz:

* yollarınızı noktanızın site IP aralıkları şirket içi ağ geçidiniz için yapılandırılmamış
* Ağ güvenlik grupları noktanızın Site IP aralığına için erişimi engelliyor
* Şirket içi güvenlik duvarlarında Site IP aralığı noktanız trafiğini engelliyor
* Şirket içi ağınıza ulaşmasını noktanızın Site göre trafiği engelleyen ağınızı kullanıcı tanımlı bir Route(UDR) var

## <a name="hybrid-connections-and-app-service-environments"></a>Karma bağlantılar ve uygulama hizmeti ortamları
Barındırılan sanal ağ kaynaklarına erişimi etkinleştirme üç özellikler vardır. Bunlar:

* VNet tümleştirme
* Karma Bağlantılar
* App Service ortamları

Karma bağlantılar, ağınızda karma bağlantı Manager(HCM) adlı bir geçiş aracısı yüklemenizi gerektirir. HCM Azure ve uygulamanızı bağlanabilmesi gerekir. Bu şirket içi ağınız gibi uzak bir ağdan özellikle harika bir çözümdür veya bir internet erişilebilir uç nokta gerektirmediğinden bile başka bir bulut ağ barındırılabilir. HCM yalnızca Windows çalıştıran ve en fazla beş örnek yüksek kullanılabilirlik sağlamak için çalışıyor olabilir. Karma bağlantılar yalnızca destekler TCP ancak ve belirli ana bilgisayar: bağlantı noktası bileşimi için her HC bitiş eşleşmelidir. 

Uygulama hizmeti ortamı özelliği, sanal ağınızda Azure App Service örneği çalıştırmanıza olanak sağlar. Bu uygulamalar erişim kaynaklarınızı ağınızı ek adımlar olmadan sağlar. Bir uygulama hizmeti ortamı'nın diğer yararları 14 GB ile RAM Dv2 bağlı çalışanları kullanabileceğiniz bazılarıdır. Başka bir avantajı, sistem gereksinimlerinizi karşılayacak şekilde ölçeklendirebilirsiniz ' dir. ASP 20 örneklerine sınırlı olduğu çok kiracılı ortamlarının aksine, ASE'de en fazla 100 ASP Örnekleri ölçeklendirebilirsiniz. VNet tümleşme olmayan bir ana ile alma şeyleri bir uygulama hizmeti ortamı ExpressRoute VPN ile çalışabilirsiniz biridir. 

Varken bazı servis talebi çakışma kullanır, bu özellik hiçbiri diğerlerinden değiştirebilirsiniz. Hangi özelliği kullanacağınıza gereksinimlerinize bağlı bilerek. Örneğin:

* Geliştirici misiniz ve yalnızca bir site ile Azure çalıştırmak ve bu iş istasyonunda masanızın altında veritabanına erişmek istiyorsanız daha sonra kullanmak için kolay karma bağlantılar şeydir. 
* Çok sayıda yerleştirilecek isteyen büyük bir kuruluş ise ortak web özelliklerinde bulut, kendi ağ yönetmek ve sonra uygulama hizmeti ortamı ile gitmek ister. 
* Uygulama hizmeti sayıda varsa uygulamalar barındırılan ve kullanarak sanal ağınızdaki kaynaklara erişmek istiyorsanız, sonra VNet tümleştirme gitmek için bir yoludur. 

Kullanım örnekleri bazı Basitlik vardır ilgili yönü. Sanal ağınızı şirket içi ağınıza zaten bağlıysa, VNet tümleştirme ya da bir uygulama hizmeti ortamı kullanarak şirket içi kaynakları kullanmak için kolay bir yoludur. Sanal ağınızı şirket içi ağınıza bağlı değilse diğer yandan, sonra onu HCM yükleme ile karşılaştırıldığında sanal ağınızı ile siteden siteye VPN ayarlamak için çok daha fazla getirdiği yüktür. 

İşlevsel farklılıklar dışında var. Ayrıca farklar fiyatlandırma. Premium uygulama hizmeti ortamı özelliktir hizmet teklifi ancak en ağ diğer harika özellikler ek yapılandırma olanaklar sunar. VNet tümleştirme standart veya Premium ASP kullanılabilir ve güvenli bir şekilde uygulama hizmeti çok kiracılı ağınızdan kaynaklarında tüketen için mükemmeldir. Karma bağlantılar şu anda bağlı olan, ücretsiz başlayıp aşamalı olarak daha pahalı alma düzeyleri fiyatlandırma olan hesap ihtiyacınız miktarına bağlı BizTalk. Ancak birçok ağlarda çalışmaya geldiğinde, 100'den de ayrı ağlarda, kaynaklara erişmek etkinleştirebilirsiniz karma bağlantılar gibi diğer hiçbir özellik yok. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
