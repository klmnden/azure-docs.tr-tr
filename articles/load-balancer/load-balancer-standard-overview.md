---
title: "Azure yük dengeleyici standart genel bakış | Microsoft Docs"
description: "Azure yük dengeleyici standart özelliklerine genel bakış"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2017
ms.author: kumud
ms.openlocfilehash: 08e4e22ae7e5d6f6efad458b4240a6d57090e865
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="azure-load-balancer-standard-overview-preview"></a>Azure yük dengeleyici standart genel bakış (Önizleme)

Azure yük dengeleyici standart SKU ve ortak IP standart SKU birlikte düzeyde ölçeklenebilir ve güvenilir mimarileri oluşturmanıza olanak sağlar. Yük Dengeleyici standart kullanan uygulamaları yeni özelliklerinden yararlanabilir. Düşük gecikme, yüksek verimlilik ve ölçek milyonlarca akışları tüm TCP ve UDP uygulamaları için kullanılabilir.

>[!NOTE]
> Yük Dengeleyici standart SKU şu anda önizlemede değil. Genel kullanılabilirlik özellikleri serbest olarak Önizleme sırasında aynı düzeyde kullanılabilirlik ve güvenilirlik özelliği sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Genel olarak kullanılabilir kullanmak [yük dengeleyici temel SKU](load-balancer-overview.md) üretim hizmetleriniz için. Bu önizleme ile ilişkili özellikler [kullanılabilirlik bölgeleri](https://aka.ms/availabilityzones), ve [HA bağlantı noktaları](https://aka.ms/haports), şu anda kaydolma ayrı gerektirir. İlgili yönergeleri için yük dengeleyici için kaydolan ek olarak, bu özellik için kaydolma [Standard Önizleme](#preview-sign-up).

## <a name="why-use-load-balancer-standard"></a>Neden yük dengeleyici standart kullanılsın mı?

Sanal veri merkezleri tam aralığının yük dengeleyici standart kullanabilirsiniz. Büyük ve karmaşık çok bölge mimarileri için küçük ölçekli dağıtımlarından aşağıdaki özelliklerinden yararlanmak için yük dengeleyici standart kullanın:

- [Kurumsal ölçek](#enterprisescale) yük dengeleyici standart ile gerçekleştirilebilir. Bu özellik, 1.000 VM örneğine kadar bir sanal ağ içinde herhangi bir sanal makine (VM) örneği ile kullanılabilir.

- [Yeni tanılama Öngörüler](#diagnosticinsights) anlamak, yönetmek ve sanal veri merkezinizin önemli bu bileşen gidermenize yardımcı olmak kullanılabilir. Göster, filtre ve sürekli veri yolu sistem durumu ölçümleri yeni çok boyutlu ölçümler Grup Azure İzleyicisi'ni (Önizleme) kullanın. Verilerinizden ön uç VM, TCP bağlantı girişimleri için uç nokta sistem durumu araştırmalarının ve giden bağlantıları izleyin.

- [Ağ güvenlik grupları](#nsg) olan yük dengeleyici standart SKU'ları veya ortak IP standart SKU'ları ile ilişkili herhangi bir VM örneğine için artık gerekli. Ağ güvenlik grupları (Nsg'ler) senaryonuz için Gelişmiş güvenlik sağlar.

- [Yüksek kullanılabilirlik (HA) bağlantı noktalarını sağlayan yüksek güvenilirlik](#highreliability) ve ağ sanal Gereçleri (NVAs) için ölçek ve diğer uygulama senaryoları. Tüm bağlantı noktaları üzerinde bir Azure iç yük dengeleyici (ILB) VM örnekleri havuzuna ön uç HA bağlantı noktalarını yükünü dengeleyin.

- [Giden bağlantılar](#outboundconnections) artık daha fazla esneklik ve ölçek sağlayan yeni bir kaynak ağ adresi çevirisi (SNAT) bağlantı noktası ayırma modelini kullanır.

- [Yük Dengeleyici standardı kullanılabilirlik bölgeleri ile](#availabilityzones) bölge olarak yedekli ve zonal mimarileri oluşturmak için kullanılabilir. Hem de bu mimariler arası bölge Yük Dengeleme içerebilir. DNS kayıtlarını bağımlılığını olmadan bölge artıklık elde edebilirsiniz. Tek bir IP adresi bölge-varsayılan olarak gereksizdir.  Tek bir IP adresi, tüm kullanılabilirlik bölgeler arasında olan bir bölge içindeki bir sanal ağdaki tüm VM ulaşabilirsiniz.


Yük Dengeleyici standart bir genel veya iç yapılandırmasında aşağıdaki temel senaryoları desteklemek için kullanabilirsiniz:

- Yük Dengelemesi sağlıklı uç örneklerine gelen trafiği.
- Bağlantı noktası iletme gelen trafiği tek bir arka uç örnek.
- Bir genel IP adresine giden trafiği sanal ağ içinde özel bir IP adresinden çevir.

### <a name = "enterprisescale"></a>Kurumsal ölçeklendirme

 Yüksek performanslı sanal veri merkezinizin tasarlamak ve herhangi bir TCP veya UDP uygulamayı desteklemek için yük dengeleyici standart kullanın. Tek başına VM örnekleri kullanın veya bir arka uç havuzundaki sanal makine ölçek 1.000 örneğine kadar ayarlar. Düşük iletme gecikme, yüksek verimlilik performansı ve ölçeği akışları milyonlarca tam olarak yönetilen bir Azure hizmetinde kullanmak için devam edin.
 
Yük Dengeleyici standart bir sanal ağdaki bir bölgedeki herhangi bir VM örneğine trafik gönderebilir. Arka uç havuzu boyutları aşağıdaki VM senaryoları herhangi bir bileşimini en çok 1.000 örnekleriyle olabilir:

- Kullanılabilirlik kümeleri olmadan tek başına VM'ler
- Kullanılabilirlik kümeleri ile tek başına VM'ler
- Sanal makine ölçek kümeleri, en çok 1.000 örnekleri
- Birden çok sanal makine ölçek ayarlar
- Karışımlar VM'ler ve sanal makine ölçekleme kümeleri

Artık gereksinimi olduğunda kullanılabilirlik kümeleri için. Kullanılabilirlik kümeleri sağladıkları başka avantajları için kullanmayı da tercih edebilirsiniz.

### <a name = "diagnosticinsights"></a>Tanılama Öngörüler

Yük Dengeleyici standart ortak ve iç yük dengeleyici yapılandırmalarının için yeni çok boyutlu tanılama yetenekleri sağlar. Bu yeni ölçümler Azure İzleyicisi (Önizleme) yoluyla sağlanır ve tüm aşağı akış tüketicileri ile tümleştirmek için özelliği de dahil olmak üzere ilgili yeteneklerini kullanma.

| Ölçüm | Açıklama |
| --- | --- |
| VIP kullanılabilirliği | Yük Dengeleyici standart sürekli veri yolundan bir bölgedeki tüm VM destekleyen SDN yığını için ön uç yük dengeleyiciye uygular. Sağlıklı örnekleri kaldığı sürece ölçüm uygulamanızın yükü dengelenmiş trafiğinin aynı yol izler. Müşteriler tarafından kullanılan veri yolu ayrıca doğrulanır. Ölçüm uygulamanıza görünmez olur ve diğer işlemlerle engellemez.|
| DIP kullanılabilirliği | Yük Dengeleyici standart uygulama uç noktanın yapılandırma ayarlarınıza göre izler hizmeti yoklama dağıtılmış bir sistem durumu kullanır. Bu ölçüm bitiş noktası filtre görünümünde yük dengeleyici her bağımsız örnek uç başına havuzu veya bir toplama sağlar.  Yük Dengeleyici sistem durumu araştırma yapılandırmanızı tarafından belirtildiği gibi uygulamanızın nasıl görünümleri görebilirsiniz.
| Eşitlemeye paketleri | Yük Dengeleyici standart olmayan TCP bağlantılarını sonlandırma veya TCP veya UDP paket akışları ile etkileşim. Akışlar ve bunların el sıkışmaları her zaman kaynak ve VM örneği arasında olur. Daha iyi TCP protokolü senaryolarınızı gidermek için Eşitlemeye kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paketler çalışır hale getirilir. Ölçüm alınan TCP Eşitlemeye paketlerin sayısını raporlar. Ölçüm hizmetiniz için bir bağlantı kurmayı deneyin istemcileri de gösterebilir.|
| SNAT bağlantıları | Yük Dengeleyici standart ön uç genel IP adresine verdiğinizi giden bağlantı sayısını raporlar. SNAT bağlantı noktalarını exhaustible bir kaynaktır. Bu ölçüm nasıl yoğun bir şekilde uygulamanızın üzerinde SNAT kaynaklı giden bağlantılar için bağlı bir gösterge verebilirsiniz.|
| Bayt sayaçları | Yük Dengeleyici standart ön uç başına işlenen veri bildirir.|
| Paket sayaçları | Yük Dengeleyici standart başına ön uç işlenen paketleri bildirir.|

### <a name = "highreliability"></a>Yüksek güvenilirlik

Yük Dengeleme, uygulama ölçek yapmak ve yüksek oranda güvenilir için kuralları yapılandırın. Tek tek bağlantı noktaları için kuralları yapılandırın veya TCP veya UDP bağlantı noktası numarası bağımsız olarak tüm trafiği dengelemek için HA bağlantı noktalarını kullanabilirsiniz.  

Çeşitli senaryolarda, yüksek kullanılabilirlik ve ölçek için iç NVAs de dahil olmak üzere kilidini açmak için yeni HA bağlantı noktalarını özelliğini kullanabilirsiniz. Özellik pratik ya da tek tek bağlantı noktalarını belirlemek için istenmeyen olduğu diğer senaryolar için kullanışlıdır. HA bağlantı noktaları, gerektiği kadar örnekleri sağlayarak artıklık ve ölçek sağlar. Yapılandırmanızı artık etkin/pasif senaryoları için sınırlı değildir. Sistem durumu araştırma yapılandırmalarınızı, yalnızca sağlıklı örneklerine trafiği ileterek hizmetinizi koruyun.

NVA satıcılar müşterileri için tam olarak satıcı tarafından desteklenen, esnek senaryolar sağlayabilir. Tek hata noktası kaldırılır ve birden fazla etkin örnekler için ölçek desteklenir. Aygıtınızın özelliklerine bağlı olarak, iki veya daha fazla örneklerine ölçeklendirebilirsiniz. Bu senaryolar için ek yönergeler için NVA satıcınıza başvurun.

### <a name = "availabilityzones"></a>Kullanılabilirlik bölgeleri

[!INCLUDE [availability-zones-preview-statement](../../includes/availability-zones-preview-statement.md)]

Desteklenen bölgeleri kullanılabilirlik bölgeleri kullanarak uygulamanızın dayanıklılık ilerleyin. Kullanılabilirlik bölgeler şu anda önizlemede belirli bölgelerdeki ve ek katılımı gerektirir.

### <a name="automatic-zone-redundancy"></a>Otomatik bölge artıklık

Yük Dengeleyici bölge olarak yedekli veya zonal ön uç, uygulamaların her biri için sağlamalıdır olup olmadığını seçebilirsiniz. Bölge artıklık yük dengeleyici standart ile oluşturmak kolaydır. Tek bir ön uç IP adresi otomatik olarak bölge olarak yedekli ' dir. Bir bölge olarak yedekli ön uç bir bölgedeki tüm kullanılabilirlik bölgeler tarafından eşzamanlı olarak sunulur. Bölge olarak yedekli veri yolu, gelen ve giden bağlantılar için oluşturulur. Azure bölgesi artıklık birden çok IP adreslerini ve DNS kayıtlarının gerektirmez. 

Bölge artıklık genel veya iç ön uç için kullanılabilir. Genel IP adresi ve ön uç özel IP iç yük dengeleyici için bölge olarak yedekli olabilir.

İç yük dengeleyici için bir bölge olarak yedekli genel IP adresi oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

Bölge olarak yedekli bir ön uç IP, iç yük dengeleyici için oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

Ön uç genel IP bölge olarak yedekli ise VM örneklerinden otomatik olarak yapılır giden bağlantılar bölge olarak yedekli haline gelir. Ön uç bölge arızasına karşı korunur. SNAT bağlantı noktası ayırma de bölge hatası devam eder.

#### <a name="cross-zone-load-balancing"></a>Çapraz bölge Yük Dengeleme

Çapraz bölge yük dengeleyici arka uç havuzu için bir bölge içinde kullanılabilir ve VM örnekleri için en büyük esnekliği sağlar. Bir ön uç, VM örneği kullanılabilirlik bölgesi bağımsız olarak sanal ağdaki tüm VM akışları sunar.

Veri yolu ve belirli bir bölgenin kaynaklarla hizalamak için ön uç ve arka uç örneklerinizi, belirli bir bölgenin de belirtebilirsiniz.

Sanal ağlar ve alt ağları hiçbir zaman bir bölge tarafından kısıtlanmıştır. İstenen VM örnekleri olan bir arka uç havuzu tanımlamak ve yapılandırmanızı tamamlanır.

#### <a name="zonal-deployments"></a>Zonal dağıtımları

Bir seçenek olarak, belirli bir bölgenin ön uç, yük dengeleyici bir zonal tanımlayarak Hizala ön uç. Bir zonal belirlenen tek kullanılabilirlik bölgesi tarafından yalnızca ön uç sunulur. Ön uç zonal VM örnekleri ile birleştirildiğinde, belirli bölgeler kaynaklara hizalanmasını sağlayabilirsiniz.

Belirli bir bölgenin her zaman oluşturulan bir genel IP adresi yalnızca bu bölgede bulunmaktadır. Bir ortak IP adresi alanı değiştirmek mümkün değil. Birden çok bölgelerdeki kaynaklara bağlı bir ortak IP adresi için bölge olarak yedekli bir genel IP oluşturun.

Kullanılabilirlik bölge 1'de zonal bir genel IP adresi oluşturmak için aşağıdaki komut dosyasını kullanın. Yapılandırmanızda varolan Resource Manager şablonları kullanıyorsanız, ekleyin **sku** bu şablonları bölümüne.

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

Kullanılabilirlik bölge 1'e bir iç yük dengeleyici ön uç oluşturmak için aşağıdaki komut dosyasını kullanın.

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

Arka uç havuzu için bir sanal ağ havuza bulunan VM örneklerinizi koyarak çapraz bölge dengelemesini ekleyin.

Yük Dengeleyici standart her zaman burada kullanılabilirlik bölgeleri desteklenir bölge ve bölge olarak yedekli kaynaktır. Bir ortak IP adresi veya iç yük atanmış bir bölgeyi herhangi bir bölgede olmayan dengeleyici ön uç dağıtabilirsiniz. Kullanılabilirlik bölgeler için destek dağıtım özelliği etkilemez. Bir bölge daha sonra kullanılabilirlik bölgeleri sağlarsa, genel IP'ler veya iç yük dengeleyici ön otomatik olarak bölge olarak yedekli hale uç daha önce Dağıtılmış. Bölge olarak yedekli veri yolu %0 paket kaybı göstermez.

### <a name = "nsg"></a>Ağ güvenlik grupları

Dengeleyici standart ve ortak IP standart tam yerleşik ağ güvenlik grupları (Nsg'ler) kullanılmasını gerektiren bir sanal ağ yükleyin. Nsg'ler, beyaz liste trafik akışı için mümkün kılar. Dağıtımınız için trafiği üzerinde tam denetim kazanmak için Nsg'ler kullanabilirsiniz. Artık, diğer trafik akışına tamamlanmasını beklemek zorunda.

Nsg'ler alt ağları veya arka uç havuzu VM örnekleri ağ arabirimlerine (NIC'ler) ile ilişkilendirin. Örnek düzeyinde ortak IP kullanıldığında, bu yapılandırma yük dengeleyici standart ve ortak IP standart ile kullanın. NSG açıkça beyaz liste sırada o trafiğe izin vermek istediğiniz trafiği gerekir.

Nsg'ler ve senaryonuz için uygulama hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-nsg.md).

### <a name ="outboundconnections"></a>Giden bağlantılar

Yük Dengeleyici standart bir yük dengeleyici bağlantı noktası maskelemeyi SNAT kullandığında sanal ağında olan VM'ler için giden bağlantılar sağlar. Bağlantı noktası maskelemeyi SNAT algoritması Artan sağlamlık ve ölçek sağlar.

Ortak bir yük dengeleyici kaynak VM örnekleriyle ilişkili olduğunda, her giden bağlantı kaynağı yeniden yazılmıştır. Kaynak sanal ağ özel IP adres alanından yük dengeleyici ön uç genel IP adresi yeniden yazılmıştır.

Giden bağlantılar kullanıldığında bir bölge olarak yedekli ile ön uç, bağlantıları da bölge olarak yedekli ve SNAT bağlantı noktası ayırma varlığını sürdürmesini bölge hatası.

Yük Dengeleyici standart yeni algoritması SNAT her VM NIC bağlantı noktalarına preallocates. Bir NIC havuza eklendiğinde, SNAT bağlantı noktaları havuz boyutuna göre önceden ayrılmış. Aşağıdaki tabloda, arka uç havuzu boyutlarda altı katmanları için bağlantı noktası preallocations gösterilmektedir:

| Havuz boyutu (VM örnekleri) | Ön tahsis SNAT bağlantı noktası |
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

SNAT bağlantı noktalarını doğrudan giden bağlantı sayısını Çevir yok. SNAT bağlantı noktası benzersiz birden çok varış yeri için yeniden kullanılabilir. Ayrıntılar için gözden [giden bağlantılar](load-balancer-outbound-connections.md) makalesi.

Arka uç havuzu boyutu artar ve daha yüksek bir katmanı geçiş durumunda, ayrılmış bağlantı noktaları yarısını geri kazanılır. Bağlantı noktası iadesi zaman aşımı ile ilişkili olan ve kurulmaları gerekir bağlantıları. Yeni bağlantı girişimleri hemen başarılı. Arka uç havuzu boyutunu azaltır ve daha düşük bir katman geçişleri, kullanılabilir SNAT bağlantı noktalarının sayısını artırır. Bu durumda, var olan bağlantıların etkilenmez.

Yük Dengeleyici standart bir kural başına temelinde kullanılabilecek ek yapılandırma seçeneği vardır. Birden çok ön uç kullanılabilir olduğunda ön uç bağlantı noktası maskelemeyi SNAT için kullanıldığı kontrol edebilirsiniz.

Yalnızca Yük Dengeleyici standart VM örnekleri hizmet, giden SNAT bağlantılar kullanılamaz. VM örnekleri aynı zamanda bir genel yük dengeleyiciye atayarak bu yeteneği açıkça geri yükleyebilirsiniz. Örnek düzeyinde genel IP'ler her VM örneğine olarak genel IP'ler doğrudan da atayabilirsiniz. Bu yapılandırma seçeneği bazı işletim sistemi ve uygulama senaryoları için gerekli olabilir. 

### <a name="port-forwarding"></a>Bağlantı noktası iletme

Temel ve standart yük dengeleyici ön uç bağlantı noktası ayrı bir arka uç örneğine eşlemek için gelen NAT kuralları yapılandırmanıza olanak sağlar. Bu kuralları yapılandırarak, Uzak Masaüstü Protokolü uç noktaları ve SSH uç noktaları kullanıma sunmak veya diğer uygulama senaryoları gerçekleştirin.

Gelen NAT kuralları aracılığıyla bağlantı noktası iletme yeteneği sağlamak yük dengeleyici standart sürdürür. Bölge olarak yedekli ön uç ile kullanıldığında, gelen NAT kuralları bölge olarak yedekli olur ve bölge hatası bitiminden sonra da geçerlidir.

### <a name="multiple-front-ends"></a>Birden çok ön uç

Uygulamalar, TLS Web siteleri veya SQL AlwaysOn Kullanılabilirlik grubu uç noktaları gibi açığa çıkarılması birden çok tek tek IP adresleri gerektirdiğinde birden çok ön uç tasarım esnekliği için yapılandırın. 

Yük Dengeleyici standart birden çok ön uç sağlamak benzersiz bir IP adresi belirli uygulama noktadaki kullanıma sunmak gereken burada devam eder.

Birden çok ön uç IP yapılandırma hakkında daha fazla bilgi için bkz: [birden çok IP yapılandırması](load-balancer-multivip-overview.md).

## <a name = "sku"></a>SKU'ları hakkında

SKU'ları yalnızca Azure Resource Manager dağıtım modelinde kullanılabilir. Bu önizleme yük dengeleyici ve genel IP kaynakları için iki SKU'ları sunar: temel ve standart. SKU'ları yeteneklerini, performans özellikleri, sınırlamalar ve bazı iç davranışı farklı. Sanal makineler ya da SKU ile kullanılabilir. Yük Dengeleyici ve genel IP kaynakları için isteğe bağlı öznitelik SKU'ları kalır. SKU'ları bir senaryo tanımı göz ardı edilir, yapılandırma temel SKU kullanılmasıdır.

>[!IMPORTANT]
>Bir kaynağın SKU değişebilir değil. Mevcut bir kaynağı SKU'su değiştiremeyebilir.  

### <a name="load-balancer"></a>Load Balancer

[Var olan yük dengeleyici kaynak](load-balancer-overview.md) temel SKU olur ve genellikle kullanılabilir ve değişmeden kalır.

Yük Dengeleyici standart SKU yeni ve şu anda önizlemede. 1 Ağustos 2017, Microsoft.Network/loadBalancers için API sürümü ekler **sku** özelliği için kaynak tanımı:

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```
Yük Dengeleyici standart kullanılabilirlik bölgeleri teklif bölgelerde otomatik olarak bölge-esnektir. Yük Dengeleyici zonal bildirilmiş, ardından otomatik olarak bölge esnek değil.

### <a name="public-ip"></a>Genel IP

[Mevcut genel IP kaynağı](../virtual-network/virtual-network-ip-addresses-overview-arm.md) temel SKU olur ve genel olarak kullanılabilir tüm yetenekleri, performans özellikleri ve sınırlamaları ile kalır.

Ortak IP standart SKU yeni ve şu anda önizlemede. 1 Ağustos 2017, Microsoft.Network/publicIPAddresses için API sürümü ekler **sku** özelliği için kaynak tanımı:

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

Ortak IP birden çok ayırma yöntemi sunan temel, ortak IP standart her zaman statik ayırma kullanır.

Ortak IP standart kullanılabilirlik bölgeleri teklif bölgelerde otomatik olarak bölge esnek değildir. Genel IP zonal bildirilmiş, ardından otomatik olarak bölge esnek değil. Zonal bir genel IP bir bölgesinden diğerine değiştirilemez.

## <a name="migration-between-skus"></a>SKU'ları arasında geçiş

SKU'ları değişebilir değildir. SKU bir kaynak grubundan diğerine taşımak için bu bölümdeki adımları izleyin.

### <a name="migrate-from-basic-to-standard-sku"></a>Standart SKU Basic'ten geçirme

1. Yeni bir standart kaynağı (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kurallarınızı oluşturun ve tanımları araştırma.

2. Temel SKU kaynakları (yük dengeleyici ve geçerli genel IP'ler) tüm VM örnekleri kaldırın. Ayrıca bir kullanılabilirlik kümesi tüm VM örnekleri kaldırdığınızdan emin olun.

3. Tüm VM örnekleri için yeni standart SKU kaynaklar bağlayın.

### <a name="migrate-from-standard-to-basic-sku"></a>Temel SKU standart geçirme

1. Yeni bir temel kaynak (yük dengeleyici ve genel gerektiğinde IP'ler) oluşturun. Kurallarınızı oluşturun ve tanımları araştırma. 

2. Standart SKU kaynakları (yük dengeleyici ve geçerli genel IP'ler) tüm VM örnekleri kaldırın. Ayrıca bir kullanılabilirlik kümesi tüm VM örnekleri kaldırdığınızdan emin olun.

3. Tüm VM örnekleri için yeni temel SKU kaynaklar bağlayın.

>[!IMPORTANT]
>
>Temel ve standart SKU'ları kullanımıyla ilgili sınırlamalar vardır.
>
>HA bağlantı noktalarını ve standart SKU tanılama yalnızca standart SKU kullanılabilir. Standart SKU temel SKU geçirmek ve ayrıca bu özellikleri korur.
>
>SKU'ları eşleşen yük dengeleyici ve genel IP kaynakları için kullanılması gerekir. Temel SKU ve standart SKU kaynaklarının karışımına sahip olamaz. VM, bir kullanılabilirlik kümesindeki sanal makineleri eklenemiyor veya her iki SKU'ları için aynı anda bir sanal makine ölçek kümesi.
>

## <a name="region-availability"></a>Bölge kullanılabilirliği

Yük Dengeleyici standart bu bölgelerde şu anda kullanılabilir değil:
- Doğu ABD 2
- Orta ABD
- Kuzey Avrupa
- Batı Orta ABD
- Batı Avrupa
- Güneydoğu Asya

## <a name="sku-service-limits-and-abilities"></a>SKU hizmet sınırları ve yetenekleri

Azure [ağ iletişimi için hizmet sınırları](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits#networking-limits) her Abonelikteki bölge başına uygulayın. 

Aşağıdaki tabloda sınırları ve yük dengeleyici temel ve standart SKU'ları yeteneklerini karşılaştırılır:

| Load Balancer | Temel | Standart |
| --- | --- | --- |
| Arka uç havuzu boyutu | en fazla 100 | en çok 1.000 |
| Arka uç havuzu sınır | Kullanılabilirlik Kümesi | sanal ağ, bölge |
| Arka uç havuzu tasarım | Kullanılabilirlik kümesi, sanal makine ölçek kullanılabilirlik set Vm'lerde | Sanal ağda herhangi bir VM örneğine |
| HA bağlantı noktaları | Desteklenmiyor | Kullanılabilir |
| Tanılama | , Yalnızca genel sınırlı | Kullanılabilir |
| VIP kullanılabilirliği  | Desteklenmiyor | Kullanılabilir |
| Hızlı IP hareketlilik | Desteklenmiyor | Kullanılabilir |
|Kullanılabilirlik bölgeleri senaryoları | Yalnızca Zonal | Zonal, bölge olarak yedekli, çapraz bölge Yük Dengeleme |
| Giden SNAT algoritması | İsteğe bağlı | Önceden ayrılmış |
| Giden SNAT ön uç seçimi | Yapılandırılamaz, birden çok adayları | Aday azaltmak için isteğe bağlı yapılandırma |
| Ağ güvenlik grubu | NIC/alt ağdaki isteğe bağlı | Gerekli |

Aşağıdaki tabloda sınırları ve ortak IP temel ve standart SKU'ları yeteneklerini karşılaştırılır:

| Genel IP | Temel | Standart |
| --- | --- | --- |
| Kullanılabilirlik bölgeleri senaryoları | Yalnızca Zonal | Bölge olarak yedekli (varsayılan), zonal (isteğe bağlı) | 
| Hızlı IP hareketlilik | Desteklenmiyor | Kullanılabilir |
| VIP kullanılabilirliği | Desteklenmiyor | Kullanılabilir |
| Sayaçları | Desteklenmiyor | Kullanılabilir |
| Ağ güvenlik grubu | NIC üzerinde isteğe bağlı | Gerekli |


## <a name="preview-sign-up"></a>Önizleme kaydolma

Yük Dengeleyici standart SKU ve ortak IP standart SKU Yardımcısı için Önizleme'na katılmak için aboneliğinizi kaydedin.  PowerShell veya Azure CLI 2.0, abonelik erişmenizi kaydediliyor. Kaydetmek için aşağıdaki adımları gerçekleştirin:

>[!NOTE]
>Yük Dengeleyici standart özelliğinin kayıt saate genel geçerlilik kazanacağını kadar sürebilir. Yük Dengeleyici standart ile kullanmak istiyorsanız, [kullanılabilirlik bölgeleri](https://aka.ms/availabilityzones) ve [HA bağlantı noktaları](https://aka.ms/haports), bu Önizleme için kaydolma ayrı bir gereklidir. İlgili yönergeleri için bu özellikleri kaydolun.

### <a name="sign-up-by-using-azure-cli-20"></a>Azure CLI 2.0 kullanarak kaydolun

1. Bu özellik sağlayıcı ile Kaydet:

    ```cli
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. İşlemi tamamlamak için 10 dakika sürebilir. Aşağıdaki komutla işlemin durumunu denetleyebilirsiniz:

    ```cli
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```
    
    Özellik kayıt durumu 'Kayıtlı' döndürdüğünde sonraki adıma devam edin:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowLBPreview",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Önizleme kayıt kaynak sağlayıcısı aboneliğinizle yeniden kaydederek tamamlayın:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-by-using-powershell"></a>PowerShell kullanarak kaydolun

1. Bu özellik sağlayıcı ile Kaydet:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. İşlemi tamamlamak için 10 dakika sürebilir. Aşağıdaki komutla işlemin durumunu denetleyebilirsiniz:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Özellik kayıt durumu 'Kayıtlı' döndürdüğünde sonraki adıma devam edin:
   
    ```
    FeatureName      ProviderName        RegistrationState
    -----------      ------------        -----------------
    AllowLBPreview   Microsoft.Network   Registered
    ```
    
3. Önizleme kayıt kaynak sağlayıcısı aboneliğinizle yeniden kaydederek tamamlayın:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```
 
## <a name="pricing"></a>Fiyatlandırma

Yük Dengeleyici standart SKU faturalama yapılandırılmış kuralları ve işlenen verilerin dayanır. Hiçbir ücret Önizleme dönemi boyunca ücrete. Daha fazla bilgi için gözden [yük dengeleyici](https://aka.ms/lbpreviewpricing) ve [genel IP](https://aka.ms/lbpreviewpippricing) sayfaları fiyatlandırma.

Müşteriler, yük dengeleyici temel SKU hiçbir ücret ödemeden keyfini çıkarın devam edin.

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki sınırlamalar Önizleme aynı anda uygulamak ve değiştirilebilir:

- Yük Dengeleyici arka uç örnekleri şu anda eşlenen sanal ağlarda yer alamaz. Arka uç hepsinin aynı bölgede olması gerekir.
- SKU'ları değişebilir değildir. Mevcut bir kaynağı SKU'su değiştiremeyebilir.
- Her iki SKU'ları olabilir bir tek başına VM kullanıldığında, VM örnekleri bir kullanılabilirlik kümesi içinde veya bir sanal makine ölçek kümesi. VM birleşimleri hem de SKU'ları ile aynı anda kullanılamaz. SKU'ları bir karışımını içeren bir yapılandırma izin verilmez.
- VM örneği (veya bir kullanılabilirlik kümesi'nin herhangi bir parçası) ile bir iç yük dengeleyici standart kullanarak devre dışı bırakır [varsayılan SNAT giden bağlantılar](load-balancer-outbound-connections.md). Bu tek başına VM yeteneği, bir kullanılabilirlik kümesine ya da bir sanal makine ölçek kümesi VM örnekleri döndürebilirsiniz. Ayrıca, giden bağlantılar yapma yeteneği geri yükleyebilirsiniz. Bu yetenekler geri yüklemek için aynı anda bir genel yük dengeleyiciye standart ya da ortak IP standart olarak aynı VM örneği için bir örnek düzeyinde ortak IP atayın. Atama tamamlandıktan sonra bağlantı noktası maskelemeyi SNAT bir genel IP adresine yeniden sağlanır.
- VM örnekleri tam arka uç havuzu ölçeği elde etmek için kullanılabilirlik kümeleri halinde gruplandırılır gerekebilir. En fazla 150 kullanılabilirlik kümeleri ve tek başına VM'ler tek bir arka uç havuzu yerleştirilebilir.
- IPv6 desteklenmez.
- Kullanılabilirlik bölgeleri bağlamında bir ön uç için bölge olarak yedekli veya tersi gelen zonal değişebilir değil. Bir ön uç bölge olarak yedekli oluşturulduktan sonra bölge olarak yedekli kalır. Bir ön uç olarak zonal oluşturulduktan sonra zonal kalır.
- Kullanılabilirlik bölgeleri bağlamında zonal bir genel IP adresi bir bölgesinden diğerine taşınamaz.


## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [yük dengeleyici temel](load-balancer-overview.md).
- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](../availability-zones/az-overview.md).
- Başka bir anahtar bazıları hakkında bilgi edinin [ağı yetenekleri](../networking/networking-overview.md) azure'da.

