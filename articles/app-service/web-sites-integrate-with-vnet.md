---
title: Bir Azure sanal ağı ile bir uygulamayı tümleştirin
description: Azure uygulama Hizmeti'nde bir uygulamanın bir yeni veya var olan Azure sanal ağa bağlama işlemi gösterilmektedir
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: 5eab09d5dffe16517e8c18eb0281716618ca0286
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166251"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure sanal ağı ile tümleştirme
Bu belge, Azure App Service sanal ağ tümleştirme özelliğini açıklar ve uygulamalar ile ayarlama işlemi gösterilmektedir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Azure sanal ağları (Vnet) ile alışkın değilseniz, bu birçok Azure kaynaklarınızın erişimini denetleyen bir ağdaki internet olmayan routeable yerleştirmenize olanak sağlayan bir özelliktir. Bu ağlar ardından çeşitli teknolojiler VPN kullanarak şirket içi ağa bağlanabilir. Azure sanal ağları hakkında daha fazla bilgi edinmek için buradaki bilgileri ile Başlat: [Azure sanal ağa genel bakış][VNETOverview]. 

Azure App Service iki biçimi vardır. 

1. Fiyatlandırma planı tam aralığını destekleyen çok kiracılı sistemleri
2. Vnet'te dağıtılır. App Service ortamı (ASE) premium özelliği. 

Bu belge, VNet tümleştirmesi ve App Service ortamı gider. ASE özelliği hakkında daha fazla bilgi edinmek istiyorsanız, buradaki bilgileri ile Başlat: [App Service ortamı giriş][ASEintro].

VNet tümleştirmesi, web uygulaması sanal ağınızdaki kaynaklara erişmenizi sağlar, ancak sanal ağdan web uygulamanıza özel erişim sağlamaz. Özel site erişimi, uygulamanızı yalnızca özel ağdan gibi bir Azure sanal ağı içinde erişilebilir hale getirmek için ifade eder. Özel site erişimi yalnızca bir iç yük dengeleyici (ILB) ile yapılandırılmış bir ASE ile kullanılabilir. ILB ASE kullanma hakkında daha fazla ayrıntı için buraya makale ile başlayın: [oluşturma ve ILB ASE kullanarak][ILBASE]. 

Burada VNet tümleştirmesi kullanmak yaygın bir senaryo, bir veritabanı veya Azure sanal ağınızdaki bir sanal makinede çalışan bir web hizmeti web uygulamanızdan erişim etkileştirir. VNet Tümleştirmesi ile VM ancak can uygulamaları özel dışı Internet yönlendirilebilir adreslerini kullanmanız için genel bir uç nokta kullanıma sunmak gerekmez. 

VNet tümleştirmesi özelliği:

* Standart, Premium veya yalıtılmış fiyatlandırma planı gerektirir 
* Klasik veya Resource Manager sanal ağı ile çalışır 
* TCP ve UDP destekler
* Web, mobil, API uygulamalarıyla çalışır ve işlev uygulamaları
* aynı anda yalnızca 1 sanal ağa bağlanmak bir uygulama sağlar
* bir App Service planında tümleştirilmesini ile beş adede kadar sanal ağlar sağlar 
* bir App Service planı içinde birden fazla uygulama tarafından kullanılmak üzere aynı sanal ağda izin verir
* VNet ağ geçidinde nedeniyle SLA'sı % 99,9 SLA destekler

VNet tümleştirmesi dahil olmak üzere desteklemeyen bazı şeyler vardır:

* bir sürücü bağlama
* AD tümleştirmesi 
* NetBIOS
* Özel site erişimi

### <a name="getting-started"></a>Başlarken
Aşağıda, web uygulamanızı bir sanal ağa bağlamadan önce göz önünde bulundurmanız gereken bazı hususlar verilmiştir:

* Sanal ağ tümleştirmesi yalnızca çalışır Apps bir **standart**, **Premium**, veya **yalıtılmış** fiyatlandırma planı. Varsa özelliği etkinleştirmek ve ardından App Service planınızın uygulamalarınızı kullanmakta oldukları sanal ağlar için bağlantıları kesilir desteklenmeyen fiyatlandırma planı ölçeklendirin. 
* Hedef sanal ağınız zaten varsa, noktadan siteye VPN dinamik yönlendirme ağ geçidi ile bir uygulama için bağlı önce etkinleştirilmiş olması gerekir. Statik yönlendirme ağ geçidi yapılandırılmışsa, noktadan siteye sanal özel ağ (VPN) etkinleştiremezsiniz.
* Sanal ağ, App Service Plan(ASP) ile aynı abonelikte olması gerekir.
* Ağ geçidi-etkin siteye ile zaten varsa ve temel SKU'da değil, IKEv2 noktadan siteye yapılandırmanızda devre dışı bırakılmalıdır.
* Bir sanal ağ ile tümleştirilen uygulamalar, o VNet için belirtilen DNS kullanın.
* Varsayılan olarak tümleştirme uygulamalarınızı trafiği sanal ağınızda tanımlanmış rotalar dayalı olarak yalnızca rota. 

## <a name="enabling-vnet-integration"></a>VNet tümleştirmesi etkinleştiriliyor

Uygulamanızı yeni veya mevcut bir sanal ağa bağlamak için bir seçenekleri var. Yeni bir ağ tümleştirmenizi bir parçası olarak oluşturursanız, yalnızca sanal ağ oluşturmaya ek olarak, dinamik yönlendirme ağ geçidi sizin için önceden yapılandırılmıştır ve noktadan siteye VPN etkinleştirilmiş. 

> [!NOTE]
> Yeni bir sanal ağ tümleştirmesi yapılandırılıyor, birkaç dakika sürebilir. 
> 
> 

VNet tümleştirmesi etkinleştirmek için uygulama ayarlarınızı açın ve ağ'ı seçin. Açılan kullanıcı Arabirimi, üç ağ seçenekleri sunar. Bu kılavuz yalnızca sanal ağ tümleştirme ancak karma bağlantılar geçiyor ve App Service ortamları bu belgenin sonraki bölümlerinde ele alınmıştır. 

Uygulamanızın doğru fiyatlandırma planını olduğu değil, kullanıcı Arabirimi, planınız için daha yüksek bir fiyatlandırma planı, tercih ettiğiniz şekilde ölçeklendirmenize olanak sağlar.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Önceden var olan bir sanal ağ ile sanal ağ tümleştirmesi etkinleştiriliyor
VNet tümleştirmesi kullanıcı Arabirimi, sanal ağlar listesinden seçmenizi sağlar. Klasik sanal ağlar oldukları gibi VNet adının yanında parantez içinde "Klasik" sözcüğüyle olduğunu gösterir. Liste, Resource Manager sanal ağlarına ilk sırada olacağı şekilde sıralanır. Aşağıda gösterilen resimde yalnızca bir VNet seçilebilir görebilirsiniz. Bir sanal ağ da dahil olmak üzere kullanıma gri, birden çok nedeni vardır:

* hesabınıza erişimi olan başka bir Abonelikteki sanal ağ olan
* sanal ağa noktadan siteye etkin yok
* Sanal Ağ Dinamik yönlendirme ağ geçidi yok

![][2]

Tümleştirmeyi etkinleştirmek için ile tümleştirmek istediğiniz sanal ağa tıklayın. Sanal Ağ'ı seçtikten sonra değişikliklerin etkili olması için uygulamanızı otomatik olarak yeniden başlatılır. 

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Etkinleştir-siteye klasik bir sanal ağ

Sanal ağınızı bir ağ geçidi yok ya da Noktadan siteye sahip, ilk kümesi gerekir. Klasik bir VNet için bir ağ geçidi yapılandırmak için portala gidin ve sanal Networks(classic) listesinde taşıyın. İle tümleştirmek istediğiniz ağı seçin. VPN bağlantıları seçin. Buradan, VPN sitesi ve bir ağ geçidi oluşturmanız bile sağlamak için noktanız oluşturabilirsiniz. Ağ geçidi görünmesi yaklaşık 30 dakika sürer.

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Noktadan siteye bir Resource Manager sanal ağı içinde etkinleştirme
Resource Manager Vnet'i bir ağ geçidi ve noktadan siteye ile yapılandırmak için ya da PowerShell belgelendiği gibi burada kullanabileceğiniz [PowerShell kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma] [ V2VNETP2S] veya burada açıklandığı gibi Azure portalını kullanma [Azure portalını kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma][V2VNETPortal]. Bu özellik gerçekleştirmek için kullanıcı Arabirimi henüz kullanılamıyor. Noktadan siteye yapılandırması için sertifikalar oluşturmanız gerekmez. Uygulamanız portalı kullanarak sanal ağa bağlandığında, sertifikalar otomatik olarak oluşturulur. 

### <a name="creating-a-pre-configured-vnet"></a>Önceden yapılandırılmış bir sanal ağ oluşturma
Ardından bir ağ geçidiyle yapılandırılmış yeni bir sanal ağ ve siteden siteye noktası oluşturmak istiyorsanız, UI ağ App Service, ancak yalnızca Resource Manager Vnet'i için bunu yeteneğine sahiptir. Bir ağ geçidi ve noktadan siteye ile klasik sanal ağ oluşturmak istiyorsanız, bu ağ kullanıcı arabirimi aracılığıyla el ile yapmanız gerekir. 

VNet tümleştirmesi kullanıcı Arabirimi aracılığıyla bir Resource Manager sanal ağı oluşturmak için Seç **yeni sanal ağ oluştur** ve sağlayın:

* Sanal ağ adı
* Sanal Ağ Adresi Bloğu
* Alt Ağ Adı
* Alt Ağ Adres Bloğu
* Ağ geçidi adres bloğu
* Noktadan Siteye Adres Bloğu

Başka bir ağa bağlanmak için bu sanal ağ istiyorsanız, bu ağlarla örtüşüyor IP adres alanı çekme kaçınmanız gerekir. 

> [!NOTE]
> Bir ağ geçidi ile Resource Manager sanal ağı oluşturma işlemi yaklaşık 30 dakika sürer ve şu anda sanal ağ, uygulama ile tümleştirilmezse. Sanal ağ geçidi ile oluşturulduktan sonra VNet tümleştirmesi UI uygulamanıza geri dönün ve yeni VNet gerekir.
> 
> 

![][3]

Azure sanal ağ içindeki özel ağ adresleri normalde oluşturulur. VNet tümleştirmesi varsayılan özellik sanal ağınızdaki bu IP adresi aralıklarını hedefleyen tüm trafiği yönlendirir. Özel IP adresi aralıklarını şunlardır:

* -Bu, 10.0.0.0 ile aynı - 10.0.0.0/8 10.255.255.255
* -Bu, 172.16.0.0 ile aynı - 172.16.0.0/12 172.31.255.255 
* -Bu, 192.168.0.0 ile aynı - 192.168.0.0/16 192.168.255.255

VNet adres alanı CIDR gösteriminde belirtilmelidir. CIDR gösterimiyle alışkın değilseniz, bir IP adresi ve ağ maskesi temsil eden bir tamsayı kullanarak adres bloklarını belirtmek için bir yöntem var. Hızlı başvuru olarak 10.1.0.0/24 256 adresleri olacaktır ve 10.1.0.0/25 128 adres olacağını göz önünde bulundurun. Bir IPv4 adresi bir özelliğini/32 ile yalnızca 1 adresi olacaktır. 

DNS sunucusu bilgileri buraya ayarlarsanız, sanal ağınıza ait ayarlanır. VNet oluşturulduktan sonra VNet kullanıcı deneyimleri bu bilgileri düzenleyebilirsiniz. Sanal ağın DNS değiştirirseniz, bir eşitleme ağ işlemi gerçekleştirmeniz gerekir.

Klasik bir VNet tümleştirmesi kullanıcı arabirimini kullanarak sanal ağ oluşturduğunuzda, uygulamanızla aynı kaynak grubunda bir sanal ağ oluşturur. 

## <a name="how-the-system-works"></a>Sistemin nasıl çalışır?
Arka planda bu özellik, uygulamanızı sanal ağınıza bağlanmak için noktadan siteye VPN teknolojisi üzerine oluşturur. Sanal makineler ile olduğu gibi doğrudan bir sanal ağ içindeki bir uygulama sağlama önleyen bir çok kiracılı sistem mimarisi, uygulamanız Azure App Service'te. Noktadan siteye teknolojisine oluşturarak, biz yalnızca uygulamayı barındıran sanal makine ağ erişimi sınırlayın. Uygulamalarınıza erişmek için bunları yapılandırmanız ağları yalnızca erişebilmesi için ağ erişimi daha fazla uygulama konaklarda sınırlıdır. 

![][4]

Sanal ağınız ile bir DNS sunucusu yapılandırmadıysanız, uygulamanızı sanal ağ içindeki kaynak ulaşmak için IP adreslerini kullanmanız gerekir. IP adresleri kullanırken, bu özelliğin önemli avantajlarından özel ağınızın içinden özel adreslerini kullanır, sağlar olduğunu unutmayın. Uygulamanızı ayarlarsanız kullanacak Vm'lerinizi biri için genel IP adresleri, sonra VNet tümleştirme özelliğini kullanmıyorsanız ve internet üzerinden iletişim.

## <a name="managing-the-vnet-integrations"></a>VNet tümleştirmeler yönetme
Bağlanmak ve bir sanal ağa bağlantısını kesmek için bir uygulama düzeyinde yeteneğidir. Birden fazla uygulama arasında VNet tümleştirmesi etkileyen işlemleri bir ASP düzeyindedir. Uygulama düzeyinde gösterilen Arabiriminden sanal ağınızda ayrıntılarını alabilirsiniz. Aynı bilgilerin çoğu da ASP düzeyinde gösterilir. 

![][5]

Ağ özelliği durumu sayfasından uygulamanızı sanal ağınıza bağlı olup olmadığını görebilirsiniz. Vnet'in VNet ağ geçidinizi aşağı herhangi bir nedenle, ardından bu not bağlı olarak gösterir. 

Artık uygulama düzeyi VNet tümleştirmesi UI ASP'den alma hakkında ayrıntılı bilgi ile aynıdır, kullanılabilir olan bilgiler. Bu öğeler şunlardır:

* VNet adı: Bu bağlantıyı Azure sanal ağı kullanıcı arabirimini açar
* Konumu - bu, sanal ağınızın konumunu yansıtır. Başka bir konumda bir VNet ile tümleştirmek mümkündür.
* Sertifika durumu - uygulama ve sanal ağ arasında VPN bağlantısı güvenliğini sağlamak için kullanılan sertifikaları vardır. Bu, eşitlenmiş olduklarından emin olmak için bir test yansıtır.
* Ağ geçidi durumu - ağ geçitlerinizi herhangi bir nedenle kapalı olmalıdır sonra uygulamanızı sanal ağ içindeki kaynaklara erişemez. 
* VNet adres alanının - sanal ağınızdaki IP adres alanı budur. 
* Noktadan siteye adres alanı - bu noktasıdır sitesine IP adres alanı sanal ağınıza ait. Uygulama olarak, bu adres alanı IP'ler birinden gelen iletişimi gösterir. 
* Site adres alanına - site sanal ağınızı şirket içi kaynaklarınıza veya diğer sanal ağa bağlanmak için siteden siteye VPN'ler kullanabilirsiniz. VPN bağlantısı burada gösterildiğinden emin IP aralıkları ile tanımlanan ardından yapılandırılmış olması gerekir.
* DNS sunucuları - DNS, bir VNet ile yapılandırılan sunucular varsa burada listelenir.
* Bir listesi, sanal ağ için tanımlanan yönlendirme olan IP adresleri ve bu adresleri show - burada Vnet'e yönlendirilmiş IP var. 

VNet tümleştirmenizi uygulama görünümünü gerçekleştirebileceğiniz tek işlem uygulamanız şu anda bağlı olduğu sanal ağ bağlantısını kesmeyi sağlamaktır. Uygulamanızı bir sanal ağdan bağlantısını kesmek için üst kısmında bağlantısını kesin. Bu eylem, sanal ağınıza değiştirmez. Sanal ağ ve ağ geçitleri de dahil olmak üzere yapılandırmasını değişmeden kalır. Ardından sanal ağınıza silmek istiyorsanız, önce ağ geçitleri de dahil olmak üzere içindeki kaynakları silmek gerekir. 

App Service planı görünümü, birkaç ek işlem vardır. Bu ayrıca farklı daha uygulamadan erişilebilir. ASP ağ UI ulaşmak için aşağı kaydırma ve ASP UI'ı açın. "Ağ özelliği ağ özelliği durumu kullanıcı arabirimini açmak için durumu" seçin. Bu ASP VNet tümleştirmeler listesine "yönetmek için buraya tıklayın"'i seçin.

![][6]

ASP konumu ile tümleştirme vnet'in konumda ararken unutmayın iyi olur. Sanal ağ başka bir konumda olduğunda, gecikme sorunlarını görmek çok daha yüksektir. 

Sanal ağlar ile tümleşik uygulamalarınız ile bu ASP ve kaç tane sağlayabilirsiniz tümleşik olduğu bir anımsatıcı kaç sanal ağlar üzerinde değil. 

Her VNet üzerinde ek ayrıntıları görmek için yalnızca ilgilendiğiniz sanal ağa tıklayın. Daha önce Not ayrıntıları ek olarak, VNet kullanarak bu ASP uygulamalarında listesini görebilirsiniz. 

Eylemler ile ilgili iki birincil eylemler vardır. İlk Vnet'inizi uygulamanızı trafiğe sürücü yollar ekleme yeteneğidir. Sertifikalar ve ağ bilgilerini eşitlemek yeteneği ikinci eylemdir.

![][7]

**Yönlendirme** ağınızda tanımlanan yollar, uygulamanızı ağınızdan içine trafiği yönlendiren için kullandığınız kadar daha önce belirtildiği gibi. Müşteriler ek giden trafik bir uygulamadan VNet ve bunlar için bu özelliği göndermek istediğiniz sağlanan olsa bazı kullanımlarını vardır. Trafiği, müşteri kendi sanal nasıl yapılandırdığını kadar sonraki ne olur. 

**Sertifikaları** sertifika durumu VPN bağlantısı için kullanıyoruz sertifikaların iyi olduğunu doğrulamak için App Service tarafından gerçekleştirilen bir denetimi gösterir. VNet tümleştirmesi etkin olduğunda, ardından bu ilk tümleştirme bu Vnet'e tüm uygulamalarda bu ASP ise var. gerekli bir bağlantının güvenliğini sağlamak için sertifika alışverişi Sertifikaları yanı sıra DNS yapılandırmasını, yollar ve ağ açıklayan başka benzer bir şeyler alın.
Bu sertifikalar veya ağ bilgileri değiştirilirse, "eşitleme ağı" gerekir. **Not**: kısa bir kesinti uygulamanızı sanal ağınıza arasında bağlantı neden "Ağı Eşitle" tıkladığınızda. Uygulamanızı yeniden başlatılmaz, ancak bağlantı kaybı düzgün sitenize çalışmamasına neden olabilir. 

## <a name="accessing-on-premises-resources"></a>Erişim şirket içi kaynaklara
Uygulamalarınızı şirket içi kaynaklarınıza uygulamanızdan erişebilirsiniz ağınıza bir siteden siteye VPN ile şirket içi ağınıza bağlıysa VNet tümleştirmesi özelliğin avantajlarından biri olmasıdır. Bunu, şirket içi VPN ağ geçidinize noktadan siteye IP'niz rotalarla güncelleştirme gerekebilir ancak çalışması aralık. Siteden siteye VPN ilk kurulduğunda yapılandırmak için kullanılan komut rotaları, noktadan siteye VPN dahil olmak üzere ayarlamanız gerekir. Noktadan siteye VPN, siteden siteye VPN oluşturduktan sonra eklerseniz, yolların el ile güncelleştirmeniz gerekir. Bunu nasıl yapacağınız hakkında ayrıntılı bilgi, ağ geçidi değişir ve burada açıklanmamaktadır. 

> [!NOTE]
> VNet tümleştirme özelliğini bir uygulama, bir ExpressRoute ağ geçidi olan bir VNet ile tümleşmez. ExpressRoute ağ geçidi olarak yapılandırılmış olsa bile [bir arada bulunma modu] [ VPNERCoex] VNet tümleştirmesi çalışmaz. Bir ExpressRoute bağlantısı üzerinden kaynaklara erişmeye ihtiyacınız sonra kullanabileceğiniz bir [App Service ortamı][ASE], ağınızda çalışır.
> 
> 

## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Birkaç VNet tümleştirme özelliğini kullanırken bilmeniz gereken farklılıklarına fiyatlandırma. Bu özelliğin kullanımı için 3 ilgili ücretler vardır:

* ASP fiyatlandırma katmanı gereksinimleri
* Veri aktarım maliyetleri
* VPN ağ geçidi maliyet.

Uygulamalarınızı bu özelliği kullanabilmek, bunların bir standart veya Premium App Service planı içinde olması gerekir. Burada bu maliyetlerinden daha fazla bilgi görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 

Sanal ağ aynı veri merkezinde olsa bile, noktadan siteye VPN'lerde nasıl işleneceğini nedeniyle her zaman giden veri için ücret, VNet tümleştirmesi bağlantınız. Bu ücretler nelerdir görmek için buraya bakın: [veri aktarma fiyatlandırma ayrıntıları][DataPricing]. 

Son öğeyi VNet ağ geçitleriniz maliyetidir. Ağ geçitleri siteden siteye VPN'ler gibi başka bir şey için gereksinim duymuyorsanız ağ geçitleri için VNet tümleştirme özelliğini desteklemek ücret ödersiniz. Burada bu maliyetleri ayrıntıları vardır: [VPN Gateway fiyatlandırması][VNETPricing]. 

## <a name="troubleshooting"></a>Sorun giderme
Özellik ayarlamak kolay olsa da, bu deneyiminiz sorun ücretsiz olacaktır anlamına gelmez. Karşılaşmamanız gerekir, istenen uç noktası var. erişme uygulama konsolundan bağlantıyı test etmek için kullanabileceğiniz bazı yardımcı programlar sorunlardır. Kullanabileceğiniz iki konsol deneyimleri vardır. Kudu konsolunu biridir ve diğer Azure portalında erişebileceğiniz konsoludur. Araçlar Git Kudu konsoluna uygulamanızdan almak için Kudu ->. Bu [siteadı] için giderek ile aynı olur. scm.azurewebsites.net. Açıldıktan sonra hata ayıklama Konsolu sekmesine gidin. Git araçları için Azure portal barındırılan konsola ardından uygulamanızdan almak için konsolu ->. 

#### <a name="tools"></a>Araçlar
Araçlar **ping**, **nslookup** ve **tracert** güvenlik kısıtlamaları nedeniyle konsolu üzerinden çalışmaz. Doldurmak için void orada olduğu iki ayrı Araçlar eklendi. DNS işlevselliğini test etmek için nameresolver.exe adlı bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Kullanabileceğiniz **nameresolver** uygulamanızın bağımlı ana bilgisayar adları kontrol etmek için. Herhangi bir şey yanlış DNS ile yapılandırılmış olması ya da belki de, DNS sunucusuna erişiminiz yoksa, bu şekilde, test edebilirsiniz.

Sonraki aracı, bir konak ve bağlantı noktası birleşimi TCP bağlantısını test etmek sağlar. Bu aracı adlı **tcpping** ve söz dizimi şu şekildedir:

    tcpping.exe hostname [optional: port]

**Tcpping** yardımcı programı bildirir belirli ana bilgisayarı ve bağlantı noktası ulaşması durumunda. Yalnızca başarı gösterebilirsiniz varsa: konak ve bağlantı noktası birleşimini dinleyen bir uygulamanın yoktur ve belirtilen konak ve bağlantı noktası uygulamanızdan ağ erişimi yoktur.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Kaynakları barındıran sanal ağdan sanal ağa erişim hata ayıklama
Uygulamanız belirli ana bilgisayarı ve bağlantı noktası ulaşmasını önleyen etmenizi vardır. Çoğu zaman bu üç şeylerden biri değil:

* **Biçiminde bir güvenlik duvarı olup** biçiminde bir güvenlik duvarı varsa, TCP zaman aşımı ulaşırsınız. Bu durumda, 21 saniye olur. Kullanım **tcpping** bağlantısını test etmek için aracı. TCP zaman aşımı, güvenlik duvarları ötesinde pek çok nedeni olabilir ancak kavramla başlayacağız. 
* **DNS erişilebilir değil** DNS zaman aşımı olan üç saniye başına DNS sunucusu. İki DNS sunucusu varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Ağınız ile yapılandırılmış DNS kullanmaz olarak nslookup kullanamazsınız unutmayın.
* **Geçersiz P2S IP aralığı** site IP aralığı noktasına RFC 1918 özel IP aralığı olması gerekir (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Aralığın dışında IP'ler kullanıyorsa şeyler işe yaramaz. 

Bu öğeleri sorununuzu alamıyorsanız, gibi basit şeyler için ilk bakış: 

* Ağ geçidini oluşturan portalda olacak şekilde gösteriyor mu?
* Sertifikalar eşit olacak şekilde gösteriyor?
* Etkilenen ASP'ler içinde "eşitleme ağ" yapmadan herkes ağ yapılandırması değişti? 

Ağ geçidiniz kapalı ise, getirmeniz yedekleyin. Sertifikalarınızı eşitleme dışı ise, VNet tümleştirmesi ASP görünümüne gidin ve "Ağı Eşitle" isabet. İle sanal ağ yapılandırması için yapılan bir değişikliği yapılmış yükselmiyor ve eşitleme bölümdü gerekir, ASP sonra sanal ağ tümleştirmesi ASP görünümüne gidin ve "Ağı Eşitle" sadece bir anımsatıcı olarak isabet, bu sanal ağ bağlantınızı ve uygulamalarınız ile kısa bir kesintiye neden. 

Tüm bu durumda, iyi biraz daha ayrıntılı incelemek gerekir:

* Aynı sanal ağ kaynaklarına ulaşmak için VNet Tümleştirmesi'ni kullanarak diğer tüm uygulamalar var mı? 
* Uygulama konsoluna gidin yapabilir ve ağınızdaki diğer bir kaynaklarına ulaşmak için tcpping kullanabilirsiniz? 

Yukarıdaki doğruysa, ardından, sanal ağ tümleştirmesi uygundur ve başka bir yerde bir sorundur. Burada bir konak: bağlantı noktası neden ulaşılamıyor görmek için basit bir yolu olmadığından daha zor olmasını alır budur. Nedenlerden bazıları şunlardır:

* bir güvenlik duvarı erişim noktanız site IP aralığı uygulama bağlantı noktasına engelleyen konağınızdaki sahip. Çapraz alt ağlar genellikle ortak erişim gerektirir.
* Hedef ana bilgisayarınızda çalışmıyor
* Uygulamanızı kapalı
* yanlış IP veya ana bilgisayar adı olan
* Uygulamanızın beklediğiniz değerinden farklı bir bağlantı noktasında dinliyor. Bu, ana bilgisayar gidip ve "netstat - aon" kullanarak komut istemi'nden denetleyebilirsiniz. Bu, kimliği hangi bağlantı noktası üzerinde dinleme işlemi gösterilmektedir. 
* Bunlar erişim bağlantı noktası ve uygulama ana bilgisayarı, noktadan siteye IP aralığına engelleyecek şekilde yapılandırılmış, ağ güvenlik grupları

Noktadan siteye IP aralığındaki tüm aralığından erişim izin vermeniz gerekir böylece uygulamanızın kullanacağı hangi IP bilmiyorum unutmayın. 

Ek hata ayıklama adımları içerir:

* Sanal ağınızdaki bir sanal makineye bağlanın ve, kaynak konak: bağlantı noktası buradan ulaşmaya çalışır. Erişim için TCP test etmek için şu PowerShell komutunu kullanın **test-netconnection**. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* VM ve test erişim uygulama söz konusu konakta ve bağlantı noktası uygulamanızdan konsolundan getirecek

#### <a name="on-premises-resources"></a>Şirket içi kaynaklara ####

Uygulamanızı, şirket içi kaynak ulaşamazsa, ağınızdan ulaşabileceği kaynak denetleyin. Kullanım **test-netconnection** Bunu yapmak için PowerShell komutu. Ardından, şirket içi kaynak Makinenizin ulaşamazsa, siteden siteye VPN bağlantınızın çalıştığından emin olun. Ardından çalışıyorsa, durumu ve şirket içi ağ geçidi yapılandırması yanı sıra daha önce Not aynı şey olup olmadığını denetleyin. 

Vnet'inizi barındırıyorsa, VM, şirket içi sisteminizi ulaşabilir ancak nedeni büyük olasılıkla aşağıdakilerden birini sonra uygulamanızı yapamaz:

* yollarınızı noktanızın site IP aralıkları, şirket içi ağ geçidi için yapılandırılmadı
* Ağ güvenlik gruplarınızda noktadan siteye IP aralığınızı erişimi engelliyor
* Şirket içi güvenlik duvarlarınızdan noktadan siteye IP aralığınızı gelen trafiği engelliyor
* Noktadan siteye bağlı trafiğiniz şirket içi ağınıza erişmesini engelleyen sanal ağınızdaki bir kullanıcı tanımlı Route(UDR) sahip

## <a name="powershell-automation"></a>PowerShell Otomasyonu

App Service, bir Azure PowerShell kullanarak sanal ağ ile tümleştirebilirsiniz. Çalıştırılmaya hazır bir betik için bkz: [Azure uygulama Hizmeti'nde bir uygulamanın bir Azure sanal ağına bağlama](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Karma bağlantılar ve App Service ortamları
Barındırılan bir sanal ağ kaynaklarına erişimi etkinleştirme üç özellik vardır. Bunlar:

* VNet tümleştirmesi
* Karma Bağlantılar
* App Service ortamları

Karma bağlantılar, karma bağlantı Manager(HCM) ağınızdaki adlı bir geçiş aracısı yüklemek gerektirir. HCM, Azure ve uygulamanızı bağlanabilmesi gerekir. Bu gibi şirket içi ağınıza uzak bir ağ üzerinden özellikle harika bir çözümdür veya internet erişilebilir uç nokta gerektirmediğinden, hatta başka bir bulut ağ barındırılabilir. HCM, yalnızca Windows üzerinde çalışır ve yüksek kullanılabilirlik sağlamak için çalışan en fazla beş örnek olabilir. Karma bağlantılar yalnızca destekler TCP olsa ve her HC uç noktası için bir özel konak: bağlantı noktası bileşimi ile eşleştirmek. 

App Service ortamı Özelliği Azure App Service örneği ağınızda çalıştırmanıza olanak sağlar. Bu, uygulamaları herhangi bir ekstra adım olmadan sanal ağınızdaki kaynaklara erişmek sağlar. Bir App Service ortamı avantajlarını en fazla 14 GB RAM ile Dv2 bağlı çalışanları kullanabileceğiniz bazılarıdır. Sistem gereksinimlerinizi karşılamak için ölçeği, başka bir avantajdır. Çok kiracılı ortamlarının aksine, ASP 20 örneklerine sınırlı olduğu bir ASE'de, en fazla 100 ASP örneklerinin ölçeklendirebilirsiniz. ASE ile VNet tümleştirmesi yok kullanmaya şeyler bir App Service ortamı ile bir ExpressRoute VPN çalışabilir biridir. 

Olsa bazı büyük/küçük harf çakışma kullanır, bu özelliklerin hiçbiri diğerlerinden değiştirebilirsiniz. Bilerek hangi özelliği kullanacağınıza gereksinimlerinize bağlıdır. Örneğin:

* Bir geliştirici olan ve Azure'da bir site çalıştırmak ve bu iş istasyonunda masanızın altında veritabanına erişmek istiyorsanız, kullanılacak kolay şey karma bağlantılar olur. 
* Çok sayıda yerleştirmek ister büyük bir kuruluş ise web özellikleri genel Bulut ve kendi ağınızda yönetmek ve ardından App Service ortamı ile gitmek istiyorsanız. 
* Sanal ağınızdaki kaynaklara erişmek için gereken birden fazla uygulama varsa, daha sonra VNet tümleştirmesi Git yoludur. 

Sanal ağınızı şirket içi ağınıza bağlıysa, ardından sanal ağ tümleştirmesi veya App Service Ortamı'nı kullanarak şirket içi kaynakları kullanmasını kolay bir yoludur. Ardından sanal ağınızı şirket içi ağınıza bağlı değilse, bunu bir siteden siteye VPN HCM yükleme ile karşılaştırıldığında, sanal ağ ile ayarlamak için çok daha fazla getirdiği yüktür. 

İşlevsel farklılıklar dışında burada da fiyatlandırma farklarını göz. Bir Premium App Service ortamı özelliği olan hizmet teklifi ancak en iyi ağ diğer harika özelliklerin yanı sıra yapılandırma olanakları sunar. VNet tümleştirmesi standart veya Premium ASP'ler ile kullanılabilir ve güvenli bir şekilde ağınızdan çok kiracılı App Service kaynakları kullanan için idealdir. Karma bağlantılar şu anda bağlı olarak gereksinim duyduğunuz fiyatlandırması, ücretsiz olarak başlayın ve ardından aşamalı olarak daha pahalı düzeylerine olan hesap temel BizTalk. Ancak birçok ağlarda çalışmaya söz konusu olduğunda, 100'den de ayrı ağlar kaynaklara erişmek etkinleştirebilirsiniz karma bağlantılar gibi diğer özellik yoktur. 

## <a name="new-vnet-integration"></a>Yeni VNet tümleştirmesi ##

Noktadan siteye VPN teknolojisine bağımlı değil VNet tümleştirme özelliğini yeni bir sürümü var. Bu yeni sürümü Önizleme aşamasındadır. Yeni VNet tümleştirme özelliği, aşağıdaki özelliklere sahiptir.

- Yeni özellik, Resource Manager Vnet'i kullanılmayan bir alt ağ gerektirir
- Bir adres, her bir App Service planı örneği için kullanılır. Alt ağ boyutu atamasından sonra değiştirilemez olduğundan, birden fazla maksimum ölçek boyutunuzu ele bir alt ağ kullanın. 32 adreslerine sahip bir/27, 20 örnek için ölçeklendirilebilir bir App Service planı kapsayan önerilen boyutu aynıdır.
- Hizmet uç yeni VNet tümleştirme özelliğini kullanarak güvenli hale getirilmiş kaynaklara kullanabilir. Hizmet uç noktaları ile uygulamanızı yapılandırmak için uygulamanıza atanan alt ağından erişime olanak
- Herhangi bir ek yapılandırma olmadan ExpressRoute bağlantısı üzerinden kaynaklara erişebilir
- Ağ geçidi yok yeni VNet tümleştirme özelliğini kullanmak için gereklidir
- App Service planınızın standart, Premium veya PremiumV2 planına olmalıdır
- Yeni özelliği yalnızca yeni Azure App Service ölçek birimleri kullanılabilir. Uygulamanızı yeni VNet tümleştirme özelliğini kullanırsanız, portal size bildirir. 
- Uygulama ve sanal ağ aynı bölgede olması gerekir

Yeni VNet tümleştirme özelliğini başlangıçta yalnızca Kuzey Avrupa ve Doğu ABD bölgelerinde kullanılabilir.


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
