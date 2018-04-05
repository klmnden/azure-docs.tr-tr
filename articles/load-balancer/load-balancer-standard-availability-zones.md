---
title: Azure standart yük dengeleyici ve kullanılabilirlik bölgeleri | Microsoft Docs
description: Standart yük dengeleyici ve kullanılabilirlik bölgeleri
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: kumud
ms.openlocfilehash: f5d46fda6bdb32c1a5000883c6aedb2da15e796a
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standart yük dengeleyici ve kullanılabilirlik bölgeleri

Azure yük dengeleyicinin standart SKU destekleyen [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) senaryoları. Bazı yeni kavramlar yanı sıra uçtan uca senaryonuz kullanılabilirlik bölgeleri kaynaklarla hizalayarak en iyi duruma dilimlerinde dağıtmak izin standart yük dengeleyici ile kullanılabilir.  Gözden geçirme [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) kullanılabilirlik bölgeleri nelerdir ile ilgili yönergeler için hangi bölgelerin kullanılabilirlik bölgeleri ve diğer desteklemekte kavramları ve ürünleri ilgili. Standart yük dengeleyici ile birlikte kullanılabilirlik bölgeleri birçok farklı senaryolar oluşturabilirsiniz korunmalarını ve esnek özellik kümesidir.  Bunlar anlamak için bu belgeyi gözden [kavramları](#concepts) ve temel senaryo [Tasarım Kılavuzu](#design).

>[!NOTE]
>Gözden geçirme [kullanılabilirlik bölgeleri](https://aka.ms/availabilityzones) diğer için İlgili Konular. 

## <a name="concepts"></a> Yük Dengeleyici ile uygulanan kullanılabilirlik bölgeleri kavramları

Yük Dengeleyici kaynakları ve gerçek altyapısı arasında doğrudan ilişkisi yoktur; bir yük dengeleyici oluşturma örneğini oluşturmaz. Yük Dengeleyici kaynaklar içerisinde, nasıl Azure oluşturmak istediğiniz senaryo elde etmek için önceden oluşturulmuş çok kiracılı altyapı program ifade edebilirsiniz nesneleridir.  Bölge olarak yedekli hizmet bir müşteri açısından bakıldığında bir kaynak olarak görüntülenirken tek bir yük dengeleyici kaynak altyapısının birden çok kullanılabilirlik bölgelerde programlama denetleyebilirsiniz kullanılabilirlik bölgeleri bağlamında önemli olmasıdır.

Bir yük dengeleyici kaynağın işlevleri, bir ön uç, bir kural, bir sistem durumu araştırması ve arka uç havuzu tanımını ifade edilir.

Kullanılabilirlik bölgeleri bağlamında, bir yük dengeleyici kaynak özelliklerini ve davranış bölge olarak yedekli veya zonal olarak açıklanmaktadır.  Bölge olarak yedekli ve zonal bir özellik zonality açıklanmaktadır.  Yük Dengeleyici bağlamında, her zaman bölge olarak yedekli anlamına gelir *tüm bölgeler* ve hizmete güvence altına almak zonal anlamına gelir bir *tek bir bölge*.

Hem genel hem de iç yük dengeleyici bölge olarak yedekli ve zonal senaryoları destekler ve her ikisi de trafiği dilimlerinde gerektiği şekilde yönlendirebilir (*çapraz bölge Yük Dengeleme*).

Bir yük dengeleyici kaynak Bölgesel ve hiçbir zaman zonal değildir.  Ve her zaman bir VNet ve alt ağ Bölgesel ve hiçbir zaman zonal.

### <a name="frontend"></a>Ön uç

Bir yük dengeleyici ön başvuran genel bir IP adresi kaynağı veya bir sanal ağ kaynağın alt ağ içindeki özel bir IP adresi ön uç IP bir yapılandırmadır.  Yük dengeli uç nokta hizmetinizi Burada sunulan oluşturur.

Bir yük dengeleyici kaynak zonal ve bölge olarak yedekli ön uçlar aynı anda içerebilir. 

Genel IP kaynağı bölgeye garanti, zonality (veya bunların olmaması) değişebilir değil.  Değiştirme veya bir ortak IP ön zonality atlamak istiyorsanız, uygun bölge'deki ortak IP yeniden oluşturmanız gerekecek.  

Bir iç yük dengeleyici ön uç zonality kaldırma ve ön uç yeniden oluşturma, değiştirme veya zonality atlama tarafından değiştirebilirsiniz.

Birden çok ön uçlar kullanırken, gözden [yük dengeleyici için birden çok ön uçlar](load-balancer-multivip-overview.md) önemli konular için.

#### <a name="zone-redundant-by-default"></a>Bölge varsayılan olarak yedekli

Kullanılabilirlik bölgeleri bir bölgede bir standart yük dengeleyici ön bölge-varsayılan olarak gereksizdir.  Tek bir ön uç IP adresi bölge hatası varlığını sürdürmesini ve tüm arka uç havuzu üyeleri bölge yedeklemiş erişmek için kullanılan. Bu hitless veri yolu gelmez, ancak herhangi bir yeniden deneme veya reestablishment başarılı olur. DNS artıklık düzenleri gerekli değildir. Ön uç'ın tek bir IP adresi aynı anda bağımsız altyapısı dağıtımlarında her kullanılabilirlik bölgesinde tarafından sunulur.  Bölge olarak yedekli tüm gelen veya giden akışlar aynı anda tek bir IP adresi kullanarak bir bölgedeki tüm kullanılabilirlik bölgeler tarafından sunulan anlamına gelir.

Bir veya daha fazla kullanılabilirlik bölgeleri başarısız olabilir ve veri yolu bölge kalır bir bölgede sürece sağlıklı devam eder. Bölge olarak yedekli yapılandırma varsayılandır ve başka bir eylem gerektirir.  Bir bölge kullanılabilirliği bölgeleri destekleme özelliği elde edince, var olan bir ön uç otomatik olarak bölge olarak yedekli olur.

İç, standart yük dengeleyici için bir bölge olarak yedekli genel IP adresi oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

İç, standart yük dengeleyici için bir bölge olarak yedekli ön uç IP adresi oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

#### <a name="optional-zone-guarantee"></a>İsteğe bağlı bölge garantisi

Tek bir bölge için kesin bir ön uç olarak bilinen sahip olmayı seçebilirsiniz bir *zonal ön uç*.  Bu, tüm gelen veya giden akış bir bölgedeki tek bir bölge tarafından sunulan anlamına gelir.  Ön uç kader bölge durumunu paylaşır.  Veri yolu burada garanti dışındaki bölgelerde hataları tarafından etkilenmez. Zonal ön uçlar bir IP adresi kullanılabilirliği bölge başına kullanıma sunmak için kullanabilirsiniz.  Ayrıca, zonal ön uçlar doğrudan kullanabilir veya kullanabilirsiniz, ön uç ortak IP adreslerinden oluşuyorsa bunları DNS Yük Dengeleme gibi ürün tümleştirileceğini [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md) ve bir istemci için çözümleyecek tek bir DNS adı kullanın birden çok zonal IP adresi.  Ayrıca bu tek tek her bölge izlemek için bölge yük dengeli uç nokta kullanıma sunmak için kullanabilirsiniz.  Bu kavramlar (bölge olarak yedekli ve aynı arka uç için zonal) blend isterseniz, gözden [Azure yük dengeleyici için birden çok ön uçlar](/load-balancer-multivip-overview.md).

Ortak bir yük dengeleyici ön eklediğiniz bir *bölgeleri* ön uç IP yapılandırması tarafından başvurulan genel IP parametresi.  

Bir iç yük dengeleyicisi, ön uç için ekleme bir *bölgeleri* iç yük dengeleyici ön uç IP yapılandırmasını parametresi. Zonal ön uç, belirli bir bölgenin bir alt ağda bir IP adresi güvence altına almak yük dengeleyici neden olur.

Kullanılabilirlik bölge 1'de bir zonal standart genel IP adresi oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

Bir iç standart yük dengeleyici ön uç kullanılabilirlik bölge 1'de oluşturmak için aşağıdaki komut dosyasını kullanın.

Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne. Ayrıca, tanımlama **bölgeleri** alt kaynak ön uç IP yapılandırmasını özelliği.

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

### <a name="cross-zone-load-balancing"></a>Çapraz bölge Yük Dengeleme

Çapraz bölge Yük Dengeleme herhangi bir bölgedeki bir arka uç noktası ulaşmak için yük dengeleyici özelliğidir ve ön uç ve kendi zonality bağımsızdır.

Hizalama ve dağıtımınız tek bir bölge içinde garanti etmek istiyorsanız, zonal ön uç ve aynı bölge zonal arka uç kaynaklarına hizalayın. Başka bir eylem gerekli değildir.

### <a name="backend"></a>Arka uç

Yük Dengeleyici sanal makineleri ile çalışır.  Tek bir VNet içindeki herhangi bir VM bir bölgeye desteklemediğini garanti veya hangi bölgede garanti ne olursa olsun arka uç havuzunun parçası olabilir.

Yalnızca hizalama ve ön uç ve arka uç tek bir bölge ile garanti etmek istiyorsanız, sanal makineleri aynı bölge içinde ilgili arka uç havuzuna yerleştirin.

Yalnızca adres VM'ler arasında birden fazla bölge isterseniz, sanal makineleri aynı arka uç havuzuna birden çok bölgelerinden yerleştirin.  Sanal makine ölçek kullanarak ayarladığında, bir veya daha fazla sanal makine ölçek kümesi aynı arka uç havuzuna yerleştirebilirsiniz.  Ve her bu sanal makine ölçek kümesini tek bir veya birden fazla bölge olabilir.

### <a name="outbound-connections"></a>Giden bağlantılar

[Giden bağlantılar](load-balancer-outbound-connections.md) tüm bölgeler tarafından sunulan ve bir sanal makine bir genel yük dengeleyiciye ve bölge olarak yedekli bir ön uç ile ilişkili olduğunda kullanılabilirlik bölgeleri içeren bir bölgede otomatik olarak bölge olarak yedekli.  Giden bağlantı SNAT bağlantı noktası ayırma bölge hataları bitiminden sonra da geçerlidir.  

Buna karşılık, VM ortak bir yük dengeleyici ve zonal bir ön uç ile ilişkili ise, giden bağlantılar tarafından tek bir bölge sunulması sağlanır.  Giden bağlantılar kader ilgili bölgenin health ile paylaşır.

SNAT bağlantı noktası ön tahsisi ve algoritması olan veya olmayan bölgeleri'de aynı olur.

### <a name="health-probes"></a>Sistem durumu araştırmaları

Kullanılabilirlik bölge olmadan oldukları gibi var olan sistem durumu araştırma tanımlarını kalır.  Ancak biz altyapı düzeyinde bir sistem durumu modeli genişletilmiş. 

Bölge olarak yedekli kullanırken ön uçlar, yük dengeleyici bağımsız olarak her kullanılabilirlik bölgeden VM ulaşılabilirlik araştırma ve müşteri müdahalesi olmadan başarısız olmuş bölgeler arasında yolları kapatmak için kendi iç sistem durumu modeli genişletir.  Belirli bir yol bir bölgeye yük dengeleyici altyapısından başka bir bölgedeki bir VM kullanılamıyorsa, yük dengeleyici algılayabilir ve bu hatanın kaçının. Bu VM ulaşabilir diğer bölgelerdeki kendi ilgili ön uçlar VM'den sunmaya devam edebilirsiniz.  Sonuç olarak, hata olayları sırasında her bölge uçtan uca hizmetinizi genel durumunu korurken biraz farklı akış dağıtımları olabilir mümkündür.

## <a name="design"></a> Tasarım konuları

Yük Dengeleyici kullanılabilirlik bölgeleri bağlamında bilerek esnektir. Bölge olarak yedekli olmasını seçebilirsiniz veya bölgelere hizalamak seçebilirsiniz.  Yüksek kullanılabilirlik artan karmaşıklık fiyattan gelebilir ve en iyi performans için kullanılabilirlik tasarlamanız gerekir.  Bazı önemli tasarım konuları göz atalım.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge artıklık

Yük Dengeleyici bölge olarak yedekli bir ön uç olarak tek bir IP sahip basit hale getirir. Bölge olarak yedekli bir IP adresi herhangi bir bölgedeki zonal kaynak güvenle görebilir ve bir bölgeyi bölge içinde sağlıklı kaldığı sürece bir veya daha fazla bölge hataları varlığını sürdürmesini. Buna karşılık, zonal ön uç hizmetinin ilgili bölge ile tek bir bölge ve paylaşımları kader azaltma ' dir.

Bölge artıklık hitless datapath ya da Denetim düzlemi anlamına gelmez;  Bu veri düzlemi açıkça olur. Bölge olarak yedekli akışları hiçbir bölge kullanabilirsiniz ve Müşteri'nin akışlarının bir bölgede tüm sağlıklı bölgeleri kullanır. Bölge hatası durumunda, trafik akışlarının zamandaki o noktada sağlıklı bölgeleri kullanılarak etkilenmez.  Bir bölgeyi bölge hata sırasındaki kullanarak trafik akışına etkilenebilir ancak uygulamaları kurtarabilir ve Azure bölgesi hatayı çözebilirsiniz Yakınsanan sonra bu akışlar bölge aktarım veya reestablishment bağlı kalan sağlıklı bölgelerde devam edebilirsiniz.

### <a name="xzonedesign"></a> Bölge arası

Herhangi bir zamanda bir uçtan uca hizmeti bölgeleri kestiği, olmayan bir bölge ancak büyük olasılıkla birden fazla bölge kader paylaşmak anlamak önemlidir.  Sonuç olarak, uçtan uca hizmetinizi tüm kullanılabilirlik zonal olmayan dağıtımlar elde etmiştir değil.

Kullanılabilirlik kazançlar kullanılabilirlik bölgeler kullanılırken geçersiz istenmeyen çapraz bölge bağımlılıklarının Tanıtımı kaçının.  Uygulamanız birden çok bileşenden oluşur ve bölge hatalarına karşı dayanıklı olmasını istediğiniz zaman, yeterli kritik bileşenleri hayatta bölge başarısız olması durumunda emin olmak için dikkatli olun gerekir.  Örneğin, yalnızca kalan bölgelerini dışında bir bölgedeki varsa, uygulamanız için tek bir kritik bileşen tüm uygulamanızı etkileyebilir.  Ayrıca, aynı zamanda bölge geri yükleme ve uygulamanızı nasıl birleşir göz önünde bulundurun. Şimdi bazı önemli noktaları inceleyin ve size özel senaryonun düşündüğünüz esin sorular için bunları kullanabilirsiniz.

- Uygulamanızı bir IP adresi ve bir VM yönetilen diskle gibi iki bileşenden oluşur ve 1 bölgesinde garanti ve bölge uçtan uca hizmetinizi bölge 1 başarısız olduğunda 2 değil varlığını sürdürmesini ne zaman dilimi 1 başarısız olur.  Zararlı hatası modu oluşturmakta olduğunuz tam olarak anlamadan bölgeler arası yok.

- İki bileşenden uygulamanız varsa, bir IP adresi ve bir VM ile disk, yönetilen ve 1 sırasıyla bölge ve bölge olarak yedekli olması garanti, uçtan uca hizmetinizi 2 bölgesinin bölge hatası varlığını sürdürmesini gibi bölge 1 başarısız oldu sürece 3 ya da her ikisini de bölge.  Ancak, tüm Gözlemleme ise, ön uç ulaşılabilirlik nedeni, hizmet durumu hakkında bazı özelliğini kaybedersiniz.  Daha kapsamlı bir sistem durumu ve kapasite modeli geliştirmeyi düşünün.  Bölge olarak yedekli ve zonal kavramları hakkında bilgi ve yönetilebilirlik genişletmek için birlikte kullanmak mümkün olabilir.

- Uygulamanızı üç bölgelerinde bölge olarak yedekli bir yük dengeleyici ön uç ve çapraz bölge sanal makine ölçek kümesi gibi iki bileşen varsa, kaynaklarınızın hatasından etkilenen değil bölgelerde kullanılabilir durumda olur ancak uçtan uca hizmetinizi cinsinden düşebilir Bölge hatası sırasında kapasitesi. Bir altyapı açısından dağıtımınızı bir veya daha fazla bölge hataları varlığını sürdürmesini ve bu aşağıdaki soruları başlatır:
  - Uygulamanız bu tür hataları ve düşürülmüş kapasitesi hakkında nasıl nedenleri biliyor musunuz?
  - Hizmetinizde zorlamak için bir bölge çifti gerekirse yük devri korumaları olması gerekiyor mu?
  - Nasıl, izlemek, algılar ve böyle bir senaryo azaltmak? Uçtan uca hizmet performansını izleme büyütmek için standart yük dengeleyici tanılama kullanmak mümkün olabilir. Kullanılabilir ve ne için tam bir resim augmentation'a gerekebilir göz önünde bulundurun.

- Bölgeleri daha kolay anlaşılmasını ve içerdiği hataları yapabilirsiniz.  Ancak, zaman aşımları, yeniden deneme ve geri Çekilme algoritmalar gibi kavramlarına geldiğinde bölge hatası diğer hataları farklı değildir. Azure yük dengeleyici bölge olarak yedekli yollar sağlar ve hızlı bir şekilde, gerçek zamanlı, paket düzeyinde kurtarmak çalışır halde yeniden iletimleri veya reestablishments hata onset sırasında ortaya çıkabilir ve nasıl ile uygulamanızı copes anlamak önemlidir hataları. Yük Dengeleme düzeni varlığını sürdürmesini, ancak aşağıdaki için planlamanız gerekir:
  - Bir bölge başarısız olduğunda, uçtan uca hizmetiniz bu anlıyor mu ve durumu kaybolursa, nasıl, kurtarır?
  - Bir bölge geri döndüğünde, uygulamanızın nasıl güvenli biçimde anlıyor mu?

### <a name="zonalityguidance"></a> Bölge olarak yedekli zonal karşılaştırması

Bölge olarak yedekli bölge belirsiz sağlayın ve hizmet için aynı zaman dayanıklı seçeneği ile tek bir IP adresi.  Karmaşıklığını sırayla azaltabilirsiniz.  Bölge olarak yedekli ayrıca mobility dilimlerinde vardır ve herhangi bir bölgedeki kaynakları güvenli bir şekilde kullanılabilir.  Ayrıca, bir bölge kullanılabilirliği bölgeleri elde sonra gerekli değişiklikleri sınırlayabilirsiniz kullanılabilirlik bölgeler olmadan bölgelerdeki gelecekteki kanıt vardır.  Bölge olarak yedekli IP adresi veya ön uç yapılandırma sözdizimi kullanılabilirlik bölgeler olmadan dahil olmak üzere herhangi bir bölgede başarılı olur.

Zonal açık bir garanti bir bölge için bölge durumunu kader paylaşımı sağlar. Özellikle, ekli olan kaynak aynı bölgedeki zonal bir VM ise ilişkilendirme zonal bir IP adresi veya zonal yük dengeleyici ön uç arzu veya makul bir öznitelik olabilir.  Veya belki de uygulamanız hakkında hangi bölgede kaynak bulunan açık bilgi gerektirir ve ayrı bölgelerde kullanılabilirliği hakkında açıkça neden istiyor.  Birden çok zonal ön uçlar dilimlerinde dağıtılmış bir uçtan uca hizmet için kullanıma sunmak seçebilirsiniz (diğer bir deyişle, her bölge için birden çok zonal sanal makine ölçek zonal ön uçlar ayarlar).  Ve zonal, ön uçlar genel IP adresi varsa, bu birden çok zonal ön uçlar hizmetiniz ile gösterme için kullanabileceğiniz [trafik Yöneticisi](../traffic-manager/traffic-manager-overview.md).  Veya üçüncü taraf çözümleri izleme aracılığıyla bölge sağlık ve performans Öngörüler başına kazanmak ve bölge olarak yedekli bir ön uç genel hizmetiyle kullanıma sunmak için birden çok zonal ön uçlar kullanabilirsiniz. Yalnızca aynı bölgeye hizalı zonal ön uçlar ile zonal kaynakları sunar ve zonal kaynaklar için zararlı çapraz bölge senaryolarını önlemek gerekir.  Zonal kaynakları yalnızca kullanılabilirlik bölgeleri var olduğu bölgelerde mevcut.

Diğer hizmet mimarisi bilerek olmadan daha iyi bir seçim biridir genel bir yönerge yoktur.

## <a name="limitations"></a>Sınırlamalar

- While veri düzlemi tam yedekli bölge (zonal garantisi belirtilmedikçe), Denetim düzlemi işlemleri tam yedekli bölgesi değildir.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md)
- Daha fazla bilgi edinmek [standart yük dengeleyici](load-balancer-standard-overview.md)
- Bilgi edinmek için nasıl [zonal bir ön uç ile standart bir yük dengeleyici kullanarak bir bölgedeki Yük Dengelemesi VM'ler](load-balancer-standard-public-zonal-cli.md)
- Bilgi edinmek için nasıl [bölge olarak yedekli bir ön uç ile standart bir yük dengeleyici kullanarak bölgeler arasında Yük Dengelemesi VM'ler](load-balancer-standard-public-zone-redundant-cli.md)
