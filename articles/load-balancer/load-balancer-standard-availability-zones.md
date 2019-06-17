---
title: Azure standart Load Balancer ve kullanılabilirlik bölgeleri
titlesuffix: Azure Load Balancer
description: Standard Load Balancer ve Kullanılabilirlik Bölgeleri
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: kumud
ms.openlocfilehash: 6f33be6e418366f57d243f578035b5c87079c99e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60734465"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standard Load Balancer ve Kullanılabilirlik Bölgeleri

Azure Load Balancer'ın standart SKU destekler [kullanılabilirlik](../availability-zones/az-overview.md) senaryoları. Bazı yeni kavramlar, uçtan uca senaryo kullanılabilirlik alanları ile kaynakları hizalama ve dilimlerinde dağıtmadan iyileştirmenize izin Standard Load Balancer ile kullanılabilir.  Gözden geçirme [kullanılabilirlik](../availability-zones/az-overview.md) kullanılabilirlik alanları nedir ile ilgili yönergeler için hangi bölgeler şu anda kullanılabilirlik alanları ve diğer destek kavramları ve ürünleri ilgili. Standard Load Balancer ile kullanılabilirlik alanına birlikte birçok farklı senaryo oluşturabileceğiniz bir korunmalarını ve esnek bir özellik kümesidir.  Bunlar anlamak için bu belgeyi gözden [kavramları](#concepts) ve temel senaryo [tasarım kılavuzunu](#design).

>[!IMPORTANT]
>Gözden geçirme [kullanılabilirlik](../availability-zones/az-overview.md) ilgili konular için bölge belirli bilgilere dahil.

## <a name="concepts"></a> Yük Dengeleyici için uygulanan kullanılabilirlik kavramları

Yük Dengeleyici kaynakları ve gerçek altyapınız arasında doğrudan bir ilişki yoktur; bir yük dengeleyici oluşturmaya örneğini oluşturmaz. Yük Dengeleyici kaynaklarının içinde Azure oluşturmak istediğiniz senaryoyu ulaşmak için önceden oluşturulmuş çok kiracılı altyapısını nasıl program ifade edebilirsiniz nesneleridir.  Bölgesel olarak yedekli bir hizmet, bir müşteri açısından bakıldığında bir kaynak olarak görünür ancak tek bir yük dengeleyici kaynağı programlama birden çok kullanılabilirlik bölgelerinde altyapının denetleyebildiğinden kullanılabilirlik bağlamında önemli budur.

Bir yük dengeleyici kaynağın işlevleri, bir ön uç, bir kural, bir durum araştırması ve arka uç havuzu tanımı ifade edilir.

Kullanılabilirlik bağlamında, bir yük dengeleyici kaynak özelliklerini ve davranışını bölgesel olarak yedekli ya da bölgesel olarak açıklanmıştır.  Bölgesel olarak yedekli ve bölgesel bir özelliğin zonality açıklanmaktadır.  Yük Dengeleyici bağlamında, her zaman bölgesel olarak yedekli anlamına gelir *birden çok bölge* ve hizmetle yalıtma bölgesel bir *tek bölge*.

Hem genel hem de iç Load Balancer, bölgesel olarak yedekli ve bölgesel senaryoları desteklemek ve her ikisi de trafiği dilimlerinde gerektiği şekilde yönlendirebilir (*bölgeler arası Yük Dengeleme*).

Bir yük dengeleyici kaynağı, Bölgesel ve hiçbir zaman bölgesel ' dir.  Ve Bölgesel ve hiçbir zaman bölgesel bir sanal ağ ve alt ağ her zaman.

### <a name="frontend"></a>Ön uç

Bir yük dengeleyici ön ucuna veya bir genel IP adresi kaynağı, hem de bir sanal ağ kaynağı alt ağ içinde özel bir IP adresi başvuran bir ön uç IP yapılandırması var.  Yük dengeli uç nokta, hizmetinizin Burada sunulan oluşturur.

Bir yük dengeleyici kaynağıyla aynı anda Bölgesel ve bölgesel olarak yedekli ön uçlar içerebilir. 

Bir genel IP kaynağı bölgeye garanti edilen zaman zonality (veya yapanın olmaması) değişebilir değildir.  Değiştirmek veya bir genel IP ön ucu, zonality atlamak istiyorsanız, uygun bölgesinde genel IP yeniden oluşturmanız gerekir.  

Bir iç Load Balancer'ın bir ön uç, zonality kaldırma ve ön uç yeniden oluşturma, değiştirme veya zonality atlama değiştirebilirsiniz.

Birden çok ön uç kullanırken gözden [Load Balancer için birden çok ön uç](load-balancer-multivip-overview.md) önemli noktalar için.

#### <a name="zone-redundant-by-default"></a>Bölge varsayılan olarak yedekli

>[!IMPORTANT]
>Gözden geçirme [kullanılabilirlik](../availability-zones/az-overview.md) ilgili konular için bölge belirli bilgilere dahil.

Kullanılabilirlik alanları ile bir bölgede standart yük dengeleyici ön uç, bölgesel olarak yedekli varsayılan olarak.  Tek bir ön uç IP adresi bölge hatası hayatta kalamaz ve bölge ne olursa olsun tüm arka uç havuzu üyelerine erişmek için kullanılabilir. Bu hitless veri yolu gelmez, ancak herhangi bir yeniden deneme veya reestablishment başarılı olur. DNS yedeklilik düzenleri gerekli değildir. Ön uç'ın tek bir IP adresi, aynı anda birden fazla kullanılabilirlik alanına birden çok bağımsız altyapı dağıtımı tarafından sunulur.  Bölgesel olarak yedekli tüm gelen veya giden akışlar aynı anda tek bir IP adresi kullanarak bir bölgede birden fazla kullanılabilirlik tarafından sunulan anlamına gelir.

Bir veya daha fazla kullanılabilirlik başarısız olabilir ve veri yolu olduğu sürece bir bölgede bölge kalır sağlıklı devam eder. Bölgesel olarak yedekli yapılandırma varsayılandır ve hiçbir ek eylem gerektirir.  

İç standart Load Balancer'ınız için bir bölgesel olarak yedekli genel IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bölümüne bu şablonları.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

İç standart Load Balancer'ınız için bir bölgesel olarak yedekli ön uç IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bölümüne bu şablonları.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

#### <a name="optional-zone-isolation"></a>İsteğe bağlı bölge yalıtım

Tek bir bölge için garantili bir ön ucu olarak da bilinen sahip olmadığınıza bir *bölgesel ön uç*.  Bu, bir bölgedeki tek bir bölge tarafından sunulan herhangi bir gelen veya giden akış anlamına gelir.  Ön uç sunucularınızın kader bölge durumunu paylaşır.  Veri yolu, burada garanti dışındaki bölgelerde hataları tarafından etkilenmez. Bölgesel ön uçlar, bir IP adresi kullanılabilirlik alanı başına kullanıma sunmak için kullanabilirsiniz.  Ayrıca, bölgesel ön uçlar doğrudan kullanmak veya kullanabilirsiniz, ön uç genel IP adreslerini oluşuyorsa bunları gibi yük dengeleyici DNS ürün tümleştirin [Traffic Manager](../traffic-manager/traffic-manager-overview.md) ve bir istemci için çözümler tek bir DNS adı kullanın birden çok bölgesel IP adresi.  Ayrıca bu tek tek her bölge izlemek için bölge yükü dengelenmiş Uç noktalara kullanıma sunmak için kullanabilirsiniz.  Bu kavramlar (Bölgesel olarak yedekli ve aynı arka uç için bölgesel) blend isterseniz, gözden [Azure Load Balancer için birden çok ön uç](load-balancer-multivip-overview.md).

Bir genel yük dengeleyiciye ön uç için eklediğiniz bir *bölgeleri* parametre ön uç IP yapılandırması tarafından başvurulan genel IP için.  

Bir iç yük dengeleyici ön uç için ekleme bir *bölgeleri* iç yük dengeleyici ön uç IP yapılandırması için parametre. Bölgesel ön uç, belirli bir bölge için bir alt ağdaki bir IP adresi sağlamak yük dengeleyici neden olur.

Kullanılabilirlik bölge 1'de bölgesel bir standart genel IP adresi oluşturmak için aşağıdaki betiği kullanın. Yapılandırmanızda mevcut Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bölümüne bu şablonları.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Kullanılabilirlik bölge 1'de bir iç standart yük dengeleyici ön ucu oluşturmak için aşağıdaki betiği kullanın.

Yapılandırmanızda mevcut Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bölümüne bu şablonları. Ayrıca tanımlayan **bölgeleri** ön uç IP yapılandırması alt kaynak için bir özellik.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

### <a name="cross-zone-load-balancing"></a>Bölgeler arası Yük Dengeleme

Bölgeler arası Yük Dengeleme, Load Balancer'ın herhangi bir bölgedeki bir arka uç noktaya ulaşabilmelidir olanağı ve ön uç ve kendi zonality bağımsızdır.

Hizalama ve dağıtımınız tek bir bölge içinde garanti istiyorsanız, bölgesel ön uç ve aynı bölge bölgesel bir arka uç kaynaklarına hizalayın. Başka bir eylem gerekli değildir.

### <a name="backend"></a>Arka uç

Yük Dengeleyici sanal makineleri ile çalışır.  Tek bir sanal ağ içindeki herhangi bir VM olup olmadığı bir bölgeye garanti veya hangi bölge için garanti edilen bağımsız olarak arka uç havuzunun parçası olabilir.

Yalnızca hizalama ve ön uç ve arka uç ile tek bir bölge garanti istiyorsanız, sanal makineleri kendi arka uç havuzuna aynı bölge içinde yerleştirin.

Yalnızca birden çok bölge arasında sanal makineleri adresi istiyorsanız, VM'ler aynı arka uç havuzuna birden çok bölgelerinden yerleştirin.  Sanal makine ölçek kümeleri kullanırken, bir veya daha fazla sanal makine ölçek kümeleri aynı arka uç havuzuna yerleştirebilirsiniz.  Ve her bu sanal makine ölçek kümeleri, tek bir veya birden çok bölge olabilir.

### <a name="outbound-connections"></a>Giden bağlantılar

[Giden bağlantılar](load-balancer-outbound-connections.md) tüm alanlar tarafından sunulan ve bir sanal makine bölgesel olarak yedekli bir ön uç ile genel Load Balancer ile ilişkili olduğunda kullanılabilirlik alanları ile bir bölgede otomatik olarak bölgesel olarak yedekli.  Giden bağlantı SNAT bağlantı noktası ayırmalar bölge hatalara karşı koruma sağlayacak.  

Buna karşılık, VM bölgesel bir ön uç ile genel Load Balancer ile ilişkili ise, tek bir bölge tarafından sunulacak giden bağlantılar sağlanır.  Giden bağlantılar kader ilgili bölgenin durumu ile paylaşır.

Algoritma ve SNAT bağlantı noktası ön tahsis aynıdır içeren veya içermeyen bölgeleri.

### <a name="health-probes"></a>Sistem durumu araştırmaları

Kullanılabilirlik alanları oldukları gibi mevcut sistem durumu araştırması tanımlarınızı kalır.  Ancak biz altyapı düzeyinde bir sistem durumu modeli genişletilmiş. 

Bölgesel olarak yedekli kullanırken ön uçlar, yük dengeleyici bağımsız olarak her kullanılabilirlik alanı bir VM'den erişilebilirliğini araştırma ve müşteri müdahalesi olmadan başarısız olan alanları genelinde yolları'ni kapatmak için kendi iç sistem durumu modeli genişletir.  Belirli bir yol başka bir bölgedeki bir VM için kullanılabilir bir bölgeye yük dengeleyici altyapıdan değilse, yük dengeleyici algılayabilir ve bu hatadan kaçınmak. Bu VM ulaşabileceği diğer bölgeler, kendi ilgili ön uçlar VM'den hizmet devam edebilirsiniz.  Sonuç olarak, hata olayları sırasında her bölge uçtan uca hizmetinizin genel sistem durumu korurken biraz farklı akış dağıtımları olabilir mümkündür.

## <a name="design"></a> Tasarım konuları

Yük Dengeleyiciyi kullanılabilirlik bağlamında bilerek esnektir. Bölgesel olarak yedekli olmasını seçebilirsiniz veya bölgelere hizalamak seçebilirsiniz.  Yüksek kullanılabilirlik artan karmaşıklık fiyattan gelebilir ve kullanılabilirlik için en iyi performans için tasarlamanız gerekir.  Bazı önemli tasarım konuları bir göz atalım.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge artıklığı

Yük Dengeleyici bölgesel olarak yedekli bir ön ucu olarak tek bir IP basitleştirir. Bölgesel olarak yedekli bir IP adresi herhangi bir bölgedeki bölgesel bir kaynak güvenli bir şekilde görebilir ve bir bölgeyi bölge içinde sağlıklı kaldığı sürece bir veya daha fazla bölge hataları hayatta kalamaz. Buna karşılık, bölgesel bir ön uç hizmetinin tek bir bölge ve paylaşımları kader ilgili bölge ile azaltma ' dir.

Bölge artıklığı hitless datapath ya da Denetim düzlemi anlamına gelmez;  Bu, veri düzlemi açıkça olur. Bölgesel olarak yedekli akışlar hiçbir bölge kullanabilirsiniz ve bir müşterinin akışlar bir bölgede tüm sağlıklı bölgeleri kullanır. Bölge hatası durumunda, o noktasında sağlıklı bölgeleri kullanarak trafik akışları etkilenmez.  Bir bölgeyi bölge hata sırasındaki kullanarak trafik akışları etkilenmiş olabilir ancak uygulamaları kurtarabilir ve Azure bölgesi hata etrafında hiper yakınsama sonra bu akışlar aktarım veya reestablishment bölge içinde kalan sağlıklı bölgelerinde devam edebilirsiniz.

### <a name="xzonedesign"></a> Çapraz bölge sınırları

Herhangi bir zamanda bir uçtan uca hizmet bölgeleri aştığında, kader olmayan bir dilimi ancak potansiyel olarak birden fazla bölge ile paylaşma anlamak önemlidir.  Sonuç olarak, uçtan uca hizmetinizi herhangi bir kullanılabilirlik bölgesel olmayan dağıtımlar elde etmiştir değil.

Kullanılabilirlik kazançlar kullanılabilirlik alanları kullanılırken silinmez istenmeyen bölgeler arası bağımlılıklar oluşturmaktan kaçının.  Uygulamanızı birden çok bileşenden oluşur ve bölge hatalarına karşı dayanıklı olmasını istediğiniz zaman, yeterli kritik bileşenleri acil ihtiyaç bölge başarısız olması durumunda emin olmak için ilgileniriz gerekir.  Örneğin, uygulamanız için tek bir kritik bileşeni yalnızca kalan bölgeleri dışındaki bir bölgedeki varsa tüm uygulamanızın etkileyebilir.  Ayrıca, bölge geri yükleme ve uygulamanızın nasıl yakınsanır ayrıca düşünün. Şimdi bazı önemli noktaları inceleyin ve kendi senaryonuza düşündüğünüz gibi sorular için ilham kullanabilirsiniz.

- Uygulamanızı bir IP adresi ve yönetilen disk ile VM gibi iki bileşenden oluşur ve bölge 2, bölge 1, uçtan uca hizmetinize başarısız olduğunda değil titanik'ten bölge 1 garanti bölge 1 başarısız olduğunda.  Potansiyel olarak tehlikeli hata modu oluşturmakta olduğunuz tam olarak anlamak sürece bölgeler arası yok.

- İki bileşenden uygulamanız varsa, bir IP adresi ve bir sanal disk, yönetilen ve bölgesel olarak yedekli ve sırasıyla 1 bölge garanti, uçtan uca hizmetinizi bölge 2, bölge hatası titanik'ten gibi bölge 1 başarısız oldu sürece 3 veya her ikisini de bölge.  Ancak, tüm görüyorsanız ise ön uç erişilebilirliğini nedeni, hizmet durumu hakkında bazı özelliğini kaybedersiniz.  Daha kapsamlı bir sistem durumu ve kapasite modeli geliştirmeyi düşünün.  İçgörü ve yönetilebilirlik genişletmek için bölgesel olarak yedekli ve bölgesel kavramları birlikte kullanabilirsiniz.

- Uygulamanızın üç bölgelerinde bölgesel olarak yedekli bir yük dengeleyici ön uç ve bölgeler arası sanal makine ölçek kümesi gibi iki bileşenleri varsa, kaynaklarınızı hatasından etkilenmeyenler bölgelerinde kullanıma sunulacaktır ancak uçtan uca hizmet kapasitenizi düzeyi düşürülmüş olabilir Bölge sırasında hata oluştu. Bir altyapı açısından dağıtımınızı bir veya daha fazla bölge hatalara ve bu aşağıdaki soruları başlatır:
  - Uygulamanız bu tür hataları ve azaltılmış kapasitesi hakkında nasıl neden biliyor musunuz?
  - Bir bölge çiftine gerekirse bir yük devretmeye zorlamak için hizmetinizde tedbirler gerekiyor mu?
  - Nasıl, izlemek, algılamak ve böyle bir senaryo azaltmak? Standard Load Balancer Tanılama, izleme, uçtan uca hizmet performansını artırmak için kullanmanız mümkün olabilir. Kullanılabilir nedir ve ne için eksiksiz bir resim güçlendirme gerekebilir göz önünde bulundurun.

- Bölgeler, daha kolay anlaşılan ve bulunan hataları yapabilirsiniz.  Ancak, zaman aşımı, yeniden denemeler ve geri alma algoritmaları gibi kavramları söz konusu olduğunda bölge hatası diğer hatalarından farklı değildir. Azure Load Balancer bölgesel olarak yedekli yollar sağlar ve gerçek zamanlı bir paket düzeyinde hızlı bir şekilde, kurtarılır çalışır halde yeniden iletimleri üst sınırı veya reestablishments başladıkları sırasında bir hata ortaya çıkabilir ve nasıl ile uygulamanızı copes anlaşılması önemlidir hataları. Yük Dengeleme düzeninizi kalmaya devam eder, ancak aşağıdakiler için planlama yapmanız:
  - Bir bölge başarısız olduğunda, uçtan uca hizmetiniz bu anlıyor mu ve durumu kaybolursa, nasıl, kurtarılır?
  - Bir bölge geri döndüğünde, uygulamanızın nasıl güvenli biçimde anlıyor mu?

### <a name="zonalityguidance"></a> Bölgesel olarak yedekli bölgesel karşılaştırması

>[!IMPORTANT]
>Gözden geçirme [kullanılabilirlik](../availability-zones/az-overview.md) ilgili konular için bölge belirli bilgilere dahil.

Bölgesel olarak yedekli bir bölge belirsiz sağlayabilir veya hizmet için aynı zaman dayanıklı seçeneğinde tek bir IP adresi.  Sırayla Bu karmaşıklığı azaltabilir.  Bölgesel olarak yedekli ayrıca mobility dilimlerinde vardır ve herhangi bir bölge içinde kaynaklara güvenli bir şekilde kullanılabilir.  Ayrıca, bir bölge kullanılabilirlik elde edin sonra gerekli değişiklikleri sınırlayabilirsiniz kullanılabilirlik alanları olmayan bölgelerde geleceğe vardır.  Kullanılabilirlik alanları olmadan dahil olmak üzere herhangi bir bölgedeki bir bölgesel olarak yedekli IP adresi veya ön uç yapılandırması sözdizimi başarılı.

Bölgesel kader bölge durumunu paylaşımı açık bir garanti, bir bölgeye sağlayabilir. Özellikle, ekli olan kaynak aynı bölgedeki bölgesel bir sanal makine ise ilişkilendirme bir bölgesel IP adresi veya bölgesel yük dengeleyici ön ucuna arzu ya da makul bir öznitelik olabilir.  Veya belki de kaynak bulunur hangi bölge açık bilgisine uygulamanızın gerektirdiği ve farklı bölgelerde kullanılabilirliği hakkında açıkça neden istiyor.  Birden fazla bölgesel ön uçlar için alanları genelinde dağıtılmış bir uçtan uca hizmeti kullanıma sunmak seçebilirsiniz (diğer bir deyişle, her bölge için birden fazla bölgesel sanal makine ölçek bölgesel ön uçlar ayarlar).  Bölgesel, ön uçlar genel IP adresleri, bu Bölgesel birden çok ön uç hizmetinizle açığa çıkarmak için kullanabileceğiniz [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Veya bölgesel birden çok ön uç, üçüncü taraf izleme çözümleri aracılığıyla bölge durum ve performans öngörüleri başına kazanmak ve genel hizmeti bölgesel olarak yedekli bir ön uç ile göstermek için kullanabilirsiniz. Yalnızca aynı bölgeye hizalı bölgesel ön uçlar ile bölgesel kaynaklar hizmet ve bölgesel kaynaklar için zararlı bölgeler arası senaryoları kaçınmak gerekir.  Bölgesel kaynaklar yalnızca kullanılabilirlik bulunduğu bölgede mevcut.

Diğer hizmet mimarisi bilerek olmadan daha iyi bir seçim biridir genel bir yönerge yoktur.

## <a name="limitations"></a>Sınırlamalar

- While veri düzlemi tamamen bölgesel olarak yedekli (Bölgesel garantisi belirtilmediyse), Denetim düzlemi işlemleri tam olarak bölgesel olarak yedekli değildir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [kullanılabilirlik alanları](../availability-zones/az-overview.md)
- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin
- Bilgi edinmek için nasıl [Yük Dengelemesi Vm'leri bölgesel bir ön uç ile standart yük dengeleyici kullanarak bir bölge içerisindeki](load-balancer-standard-public-zonal-cli.md)
- Bilgi edinmek için nasıl [bölgesel olarak yedekli bir ön uç ile standart yük dengeleyici kullanarak bölgeler arasında Yük Dengelemesi Vm'leri](load-balancer-standard-public-zone-redundant-cli.md)
