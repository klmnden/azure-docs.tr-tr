---
title: Azure’da ağ güvenlik grupları | Microsoft Docs
description: Azure'daki dağıtılmış güvenlik duvarını kullanan sanal ağlarınızdaki trafik akışını yalıtmak ve denetlemek için Ağ Güvenlik Gruplarının nasıl kullanılacağı konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 87ca0a1cd9766d3ad76d0fe5dd29a34ec40ea276
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="filter-network-traffic-with-network-security-groups"></a>Ağ güvenlik grupları ile ağ trafiğini filtreleme

Ağ güvenlik grubu (NSG), Azure Sanal Ağlara (VNet) bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. Ağ güvenlik grupları (NSG’ler), alt ağlarla, ayrı ayrı VM’lerle (klasik) veya VM’lere bağlı ağ arabirimleri ile ilişkilendirilebilir (Resource Manager). Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Bir NSG’nin bir VM veya ağ arabirimi ile ilişkilendirilmesi yoluyla da trafik kısıtlanabilir.
 
> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale her iki modelin de nasıl kullanıldığını kapsıyor olsa da, Microsoft en yeni dağıtımların Resource Manager modelini kullanmasını önermektedir.

## <a name="nsg-resource"></a>NSG kaynağı
NSG'ler aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Kısıtlamalar | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Adı |NSG'nin adı |Bölge içinde benzersiz olmalıdır.<br/>Harf, sayı, alt çizgi, nokta ve kısa çizgi içerebilir.<br/>Bir harf veya sayı ile başlamalıdır.<br/>Bir harf, sayı veya alt çizgi ile bitmelidir.<br/>80 karakterden uzun olamaz. |Birden fazla NSG oluşturmanız gerekebileceği için NSG'lerinizin işlevini tanımlamanızı kolaylaştıran bir adlandırma kuralınızın bulunduğundan emin olun. |
| Bölge |NSG'nin oluşturulduğu Azure [bölgesi](https://azure.microsoft.com/regions). |NSG’ler yalnızca NSG ile aynı bölgede bulunan kaynaklarla ilişkilendirilebilir. |Bir bölgede kaç tane NSG’ye sahip olabileceğiniz hakkında bilgi almak için [Azure limitleri](../azure-subscription-service-limits.md#virtual-networking-limits-classic) makalesini okuyun.|
| Kaynak grubu |NSG'nin mevcut olduğu [kaynak grubu](../azure-resource-manager/resource-group-overview.md#resource-groups). |Bir NSG bir kaynak grubunda mevcut olsa da, kaynağın NSG'nin ait olduğu Azure bölgesinin bir parçası olması koşuluyla, NSG herhangi bir kaynak grubuyla ilişkilendirilebilir. |Kaynak grupları, birden fazla kaynak grubunun birlikte bir dağıtım birimi olarak yönetilmesi için kullanılır.<br/>NSG'yi ilişkili olduğu kaynaklarla gruplandırmayı değerlendirebilirsiniz. |
| Kurallar |Hangi trafiklere izin verildiğini veya reddedildiğini tanımlayan gelen veya giden kuralları. | |Bu makalenin [NSG kuralları](#Nsg-rules) bölümüne bakın. |

> [!NOTE]
> Uç nokta tabanlı ACL'ler ve ağ güvenlik grupları, aynı VM örneğinde desteklenmez. Bir NSG'yi kullanmak istiyorsanız ve bir uç nokta ACL'si zaten kullanılıyorsa öncelikle uç nokta ACL'sini kaldırın. ACL’yi kaldırma hakkında bilgi için bkz. [PowerShell kullanarak Uç Noktalar için Erişim Denetim Listelerini (ACL’ler) yönetme](virtual-networks-acl-powershell.md).
> 

### <a name="nsg-rules"></a>NSG kuralları
NSG kuralları aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Kısıtlamalar | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| **Ad** |Kuralın adı. |Bölge içinde benzersiz olmalıdır.<br/>Harf, sayı, alt çizgi, nokta ve kısa çizgi içerebilir.<br/>Bir harf veya sayı ile başlamalıdır.<br/>Bir harf, sayı veya alt çizgi ile bitmelidir.<br/>80 karakterden uzun olamaz. |Bir NSG içinde çeşitli kurallara sahip olabilirsiniz, bu nedenle kuralınızın işlevini tanımlayan bir adlandırma kuralını uyguladığınızdan emin olun. |
| **Protokol** |Kural ile eşleştirilecek protokol. |TCP, UDP veya * |Protokol olarak * kullanmak ICMP'yi (yalnızca Doğu-Batı trafiği), aynı zamanda UDP'yi ve TCP'yi içerir ve ihtiyacınız olan kuralların sayısını azaltabilir.<br/>Bununla birlikte, * kullanmak çok geniş bir yaklaşım olabilir, bu nedenle yalnızca gerçekten gerekli olduğu zaman * kullandığınızdan emin olun. |
| **Kaynak bağlantı noktası aralığı** |Kural ile eşleştirilecek kaynak bağlantı noktası aralığı. |1 - 65535 aralığındaki tek bağlantı noktası numarası, bağlantı noktası aralığı (yani 1 - 65535) veya * (tüm bağlantı noktaları için). |Kaynak bağlantı noktaları kısa ömürlü olabilir. İstemci programınız belirli bir bağlantı noktasını kullanmadığı sürece, çoğu durum için * kullanın.<br/>Birden çok kurala ihtiyaç duyulmasını önlemek için mümkün olduğunca bağlantı noktası aralıklarını kullanmaya çalışın.<br/>Birden çok bağlantı noktası veya bağlantı noktası aralığı virgülle birleştirilemez. |
| **Hedef bağlantı noktası aralığı** |Kural ile eşleştirilecek hedef bağlantı noktası aralığı. |1'den 65535'e kadar olan tek bağlantı noktası, bağlantı noktası aralığı (yani 1-65535) veya \* (tüm bağlantı noktaları için). |Birden çok kurala ihtiyaç duyulmasını önlemek için mümkün olduğunca bağlantı noktası aralıklarını kullanmaya çalışın.<br/>Birden çok bağlantı noktası veya bağlantı noktası aralığı virgülle birleştirilemez. |
| **Kaynak adres ön eki** |Kural ile eşleştirilecek kaynak adres ön eki veya etiketi. |Tek IP adresi (örnek: 10.10.10.10), IP alt ağı (örnek: 192.168.1.0/24), [hizmet etiketi](#service-tags) veya * (tüm adresler için). |Kuralların sayısını azaltmak için aralıklar, hizmet etiketleri ve * kullanmayı düşünün. |
| **Hedef adres ön eki** |Kural ile eşleştirilecek hedef adres ön eki veya etiketi. | tek IP adresi (örnek: 10.10.10.10), IP alt ağı (örnek: 192.168.1.0/24), [varsayılan etiket](#service-tags) veya * (tüm adresler için). |Kuralların sayısını azaltmak için aralıklar, hizmet etiketleri ve * kullanmayı düşünün. |
| **Yön** |Kural için eşleştirilecek trafik yönü. |Gelen veya giden. |Gelen veya giden kuralları, yöne bağlı olarak ayrı ayrı işlenir. |
| **Öncelik** |Kurallar öncelik sırasına göre denetlenir. Bir kural uygulandığı zaman eşleştirme için başka hiçbir kural test edilmez. | 100 ile 4096 arasında bir sayı. | Gelecekte oluşturabileceğiniz yeni kurallara alan bırakmak amacıyla, her kural için öncelikleri 100'lü adımlarla atlayarak kuralları oluşturmayı düşünün. |
| **Erişim** |Kuralın eşleşmesi durumunda uygulanacak erişim türü. | İzin ver veya reddet. | Bir paket için izin verme kuralı bulunmazsa paketin bırakılacağını göz önünde bulundurun. |

NSG'ler iki kural kümesi içerir: Gelen ve giden. Bir kurala ait öncelik her küme içinde benzersiz olmalıdır. 

![NSG kuralının işlenmesi](./media/virtual-network-nsg-overview/figure3.png) 

Önceki resimde NSG kurallarının nasıl işlendiği gösterilmektedir.

### <a name="default-tags"></a>Sistem etiketleri

Hizmet etiketleri, bir IP adresi kategorisini belirtmek için sistem tarafından sağlanan tanımlayıcılardır. Herhangi bir güvenlik kuralının **kaynak adres ön eki** ve **hedef adres ön eki** özelliklerinde hizmet etiketlerini kullanabilirsiniz. [Hizmet etiketleri](security-overview.md#service-tags) hakkında daha fazla bilgi edinin.

### <a name="default-rules"></a>Varsayılan güvenlik kuralları

Tüm NSG'ler bir varsayılan güvenlik kuralları kümesi içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler. [Varsayılan güvenlik kuralları](security-overview.md#default-security-rules) hakkında daha fazla bilgi edinin.

## <a name="associating-nsgs"></a>NSG'leri ilişkilendirme
Kullandığınız dağıtım modeline bağlı olarak, bir NSG'yi VM'lerle, ağ arabirimleriyle ve alt ağlarla aşağıdaki gibi ilişkilendirebilirsiniz:

* **VM (yalnızca klasik):** Güvenlik kuralları VM’ye/VM’den tüm trafiğe uygulanır. 
* **Ağ arabirimi (yalnızca Resource Manager):** Güvenlik kuralları, NSG’nin ilişkili olduğu ağ arabirimine gelen ve buradan giden trafiğin tamamına uygulanır. Birden çok ağ arabirimi içeren sanal makinelerde, her ağ arabirimine farklı NSG uygulayabileceğiniz gibi her birine aynı NSG’yi uygulayabilirsiniz. 
* **Alt ağ (Resource Manager ve klasik):** Güvenlik kuralları, alt ağa bağlı kaynaklara/kaynaklardan tüm trafiğe uygulanır.

Bir VM (veya dağıtım modeline bağlı olarak, ağ arabirimi) ve bu VM'nin (veya ağ arabiriminin) bağlı olduğu alt ağ ile farklı NSG’ler ilişkilendirebilirsiniz. Her NSG'deki öncelik temel alınarak, güvenlik kuralları aşağıdaki sırayla trafiğe uygulanır:

- **Gelen trafik**

  1. **Alt ağa uygulanan NSG:** Alt ağ NSG'sinde trafiği reddetmeye yönelik bir eşleştirme kuralı varsa paket bırakılır.

  2. **NSG’yi Ağ arabirimine (Resource Manager) veya VM’ye (klasik) uygulama**: VM’nin/ağ arabiriminin NSG'sinde trafiği reddetmeye yönelik bir eşleşme kuralı varsa, alt ağ NSG'sinde trafiğe izin vermeye yönelik bir eşleşme kuralı olsa bile paketler VM’de/ağ arabiriminde bırakılır.

- **Giden trafik**

  1. **NSG’yi ağ arabirimine (Resource Manager) veya VM’ye (klasik) uygulama**: VM’nin/ağ arabiriminin NSG’sinde trafiği reddetmeye yönelik bir eşleşme kuralı varsa, paketler bırakılır.

  2. **NSG’yi alt ağa uygulama:** Bir alt ağ NSG’sinde trafiği engelleyen bir eşleşme kuralı varsa, VM’nin/ağ arabiriminin NSG’sinde trafiğe izin veren bir eşleşme kuralı olsa bile paketler bırakılır.

> [!NOTE]
> Tek bir NSG'yi, yalnızca bir alt ağ, VM veya ağ arabirimi ile ilişkilendirebilirsiniz. Ancak aynı NSG'yi istediğiniz sayıda kaynak ile ilişkilendirebilirsiniz.
>

## <a name="implementation"></a>Uygulama
Aşağıdaki araçları kullanarak NSG’leri Resource Manager veya klasik dağıtım modellerine uygulayabilirsiniz:

| Dağıtım aracı | Klasik | Resource Manager |
| --- | --- | --- |
| Azure portalına   | Yes | [Evet](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Evet](virtual-networks-create-nsg-classic-ps.md) | [Evet](tutorial-filter-network-traffic.md) |
| Azure CLI **V1**   | [Evet](virtual-networks-create-nsg-classic-cli.md) | [Evet](tutorial-filter-network-traffic-cli.md) |
| Azure CLI **V2**   | Hayır | [Evet](tutorial-filter-network-traffic-cli.md) |
| Azure Resource Manager şablonu   | Hayır  | [Evet](template-samples.md) |

## <a name="planning"></a>Planlama
NSG'leri uygulamadan önce aşağıdaki soruları yanıtlamanız gerekir:

1. Hangi tür kaynakların gelen veya giden trafiğini filtrelemek istersiniz? Ağ arabirimleri (Resource Manager), VM’ler (klasik), Cloud Services, Uygulama Hizmeti Ortamları ve VM Ölçek Kümeleri gibi kaynakları bağlayabilirsiniz. 
2. Gelen/giden trafiği filtrelemek istediğiniz kaynaklar, mevcut sanal ağlardaki alt ağlara mı bağlı?

Azure'da ağ güvenliği planlaması konusunda daha fazla bilgi için [Bulut hizmetleri ve ağ güvenliği](../best-practices-network-security.md) makalesini okuyun. 

## <a name="design-considerations"></a>Tasarım konusunda dikkat edilmesi gerekenler
[Planlama](#Planning) bölümündeki soruların yanıtlarını öğrendiğiniz zaman, NSG'lerinizi tanımlamadan önce aşağıdaki bölümleri gözden geçirin:

### <a name="limits"></a>Sınırlar
Bir abonelikte sahip olabileceğiniz NSG sayısı ve NSG başına kural sayısı sınırlıdır. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

### <a name="vnet-and-subnet-design"></a>Sanal ağ ve alt ağ tasarımı
NSG'ler alt ağlara uygulanabildiğinden kaynaklarınızı alt ağa göre gruplayıp NSG'leri alt ağlara uygulayarak NSG sayısını en aza indirebilirsiniz.  NSG'leri alt ağlara uygulamaya karar verirseniz var olan sanal ağlarınızın ve alt ağlarınızın NSG'ler göz önüne alınmadan tanımlanmış olduğunu fark edebilirsiniz. NSG tasarımınızı destekleyen yeni sanal ağlar ile alt ağlar tanımlamanız ve yeni kaynaklarınızı yeni alt ağlarınıza dağıtmanız gerekebilir. Bu işlemlerden sonra var olan kaynaklarınızı yeni alt ağlara taşımak için bir geçiş stratejisi tanımlayabilirsiniz. 

### <a name="special-rules"></a>Özel kurallar
Aşağıdaki kuralların izin verdiği trafiği engellerseniz, altyapınız temel Azure hizmetleriyle iletişim kuramaz:

* **Ana bilgisayar düğümünün sanal IP'si:** DHCP, DNS ve sistem durumunu izleme gibi temel altyapı hizmetleri, 168.63.129.16 numaralı sanallaştırılmış ana bilgisayar IP adresi yoluyla sağlanır. Bu genel IP adresi Microsoft'a aittir ve tüm bölgelerde bu amaç için kullanılan tek sanallaştırılmış IP adresi olarak kullanılır. Bu IP adresi, VM’yi barındıran sunucu makinesinin (ana bilgisayar düğümü) fiziksel IP adresiyle eşleşir. Ana bilgisayar düğümü, yük dengeleyici durum araştırması ve makine durumu araştırması için araştırma kaynağı, DNS özyinelemeli çözümleyici ve DHCP geçişi olarak görev yapar. Bu IP adresi ile iletişim bir saldırı değildir.
* **Lisanslama (Anahtar Yönetimi Hizmeti):** VM’lerde çalışan Windows görüntülerinin lisanslanması gerekir. Lisanslama için, lisans isteği sorgularını işleyen Anahtar Yönetimi Hizmeti ana bilgisayar sunucularına bir lisans isteği gönderilir. İstek, bağlantı noktası 1688 üzerinden gönderilir.

### <a name="icmp-traffic"></a>ICMP trafiği
Geçerli NSG kuralları yalnızca *TCP* veya *UDP* protokollerine izin verir. *ICMP* için belirli bir etiket bulunmaz. Ancak, sanal ağ içindeki herhangi bir bağlantı noktası ve protokolün gelen ve giden trafiğine izin veren AllowVNetInBound varsayılan kuralı tarafından bir sanal ağ içinde ICMP trafiğine izin verilir.

### <a name="subnets"></a>Alt ağlar
* İş yükünüzün gerektirdiği katmanların sayısını göz önünde bulundurun. Her katman bir alt ağ kullanılarak yalıtılabilir, bunun için alt ağa bir NSG uygulanır. 
* Bir VPN ağ geçidi veya ExpressRoute bağlantı hattı için bir alt ağ uygulamanız gerekiyorsa bu alt ağa bir NSG **uygulamayın**. Aksi halde sanal ağlar arası veya şirket içi ve dışı karma bağlantılar çalışmayabilir. 
* Bir ağ sanal gereci (NVA) uygulamanız gerekirse, NVA’yı kendi alt ağına bağlayın ve NVA’ya/NVA’dan kullanıcı tanımlı yollar (UDR) oluşturun. Bu alt ağa gelen ve giden trafiği filtrelemek için alt ağ düzeyinde bir NSG uygulayabilirsiniz. UDR’ler hakkında daha fazla bilgi için [Kullanıcı tanımlı yollar](virtual-networks-udr-overview.md) makalesini okuyun.

### <a name="load-balancers"></a>Yük dengeleyiciler
* İş yükleriniz tarafından kullanılan her bir yük dengeleyicisi için yük dengeleme ve ağ adresi çevirisi (NAT) kurallarını göz önünde bulundurun. NAT kuralları, ağ arabirimini (Resource Manager) veya VM/Cloud Services rol örneklerini (klasik) içeren bir arka uç havuzuna bağlanır. Yalnızca yük dengeleyicilerde uygulanan kurallar yoluyla eşlenen trafiğe izin vermek üzere, her arka uç havuzu için bir NSG oluşturmayı düşünün. Her bir arka uç havuzu için bir NSG oluşturulması, arka uç havuzuna doğrudan (yük dengeleyici üzerinden değil) gelen trafiğin de filtrelenmesini garanti eder.
* Klasik dağıtımlarda, bir yük dengeleyicideki bağlantı noktalarını VM'lerinizdeki veya rol örneklerinizdeki bağlantı noktalarına eşleyen uç noktalar oluşturursunuz. Resource Manager ile genel kullanıma yönelik bireysel yük dengeleyicinizi de oluşturabilirsiniz. Gelen trafik için hedef bağlantı noktası, yük dengeleyici tarafından kullanıma sunulan bağlantı noktası değil, VM veya rol örneğindeki gerçek bağlantı noktasıdır. VM'ye gelen bağlantıya ait kaynak bağlantı noktası ve adresi yük dengeleyici tarafından kullanıma sunulan bağlantı noktası ve adresi değil, İnternet'teki uzak bilgisayar üzerindeki bir bağlantı noktası ve adresidir.
* Azure Load Balancer üzerinden gelen trafiği filtrelemek üzere NSG’ler oluşturduğunuzda, uygulanan kaynak bağlantı noktası ve adres aralığı yük dengeleyici ön ucundan değil, kaynak bilgisayardan gelir. Hedef bağlantı noktası ve adres aralığı, yük dengeleyici ön ucuna değil, hedef bilgisayara aittir.
* AzureLoadBalancer etiketini engellerseniz Azure Load Balancer’dan gelen sistem durumu araştırmaları başarısız olur ve hizmetiniz etkilenebilir.

### <a name="other"></a>Diğer
* Uç nokta tabanlı access control listeleri (ACL) ve NSG'ler, aynı VM örneğinde desteklenmez. Bir NSG'yi kullanmak istiyorsanız ve bir uç nokta ACL'si zaten kullanılıyorsa öncelikle uç nokta ACL'sini kaldırın. Bir uç nokta ACL’yi kaldırma hakkında bilgi için [Uç nokta ACL’leri yönetme](virtual-networks-acl-powershell.md) makalesine bakın.
* Resource Manager’da birden çok ağ arabirimi içeren VM'ler için, bir ağ arabirimi ile ilişkilendirilmiş NSG kullanarak ağ arabirimi temelinde yönetimi (uzaktan erişim) etkinleştirebilirsiniz. Her bir ağ arabirimi ile benzersiz NSG’lerin ilişkilendirilmesi, ağ arabirimleri arasında trafik türlerinin ayılmasını sağlar.
* Yük dengeleyicilerin kullanımına benzer şekilde, diğer sanal ağlardan gelen trafiği filtrelerken sanal ağları bağlayan ağ geçidini değil, uzak bilgisayarın kaynak adres aralığını kullanmanız gerekir.
* Çoğu Azure hizmeti sanal ağlara bağlanamaz. Bir Azure kaynağı bir sanal ağa bağlı değilse, kaynağa giden trafiği filtrelemek için bir NSG kullanabilirsiniz.  Kullandığınız hizmetlerin sanal ağa bağlanıp bağlanamayacaklarını belirlemek için bu hizmetlerin belgelerini okuyun.

## <a name="sample-deployment"></a>Örnek dağıtımı
Bu makaledeki bilgilerin uygulanmasına ilişkin bir örnek görmek üzere, aşağıdaki resimde gösterilen iki katmanlı uygulamayla yaygın bir senaryo düşünün:

![NSG'ler](./media/virtual-network-nsg-overview/figure1.png)

Diyagramda gösterildiği gibi, *Web1* ile *Web2* VM'leri *FrontEnd* alt ağına ve *DB1* ile *DB2* VM'leri *BackEnd* alt ağına bağlanır.  Her iki alt ağ da *TestVNet* sanal ağının parçasıdır. Uygulama bileşenlerinin her biri, sanal ağa bağlı bir Azure VM içinde çalışır. Senaryo aşağıdaki gereksinimlere sahiptir:

1. WEB ve DB sunucuları arasındaki trafiğin ayrılması.
2. Trafiği 80 numaralı bağlantı noktasındaki tüm web sunucularına yük dengeleyiciden ileten yük dengeleme kuralları.
3. Yük dengeleyici NAT kuralları, bağlantı noktası 50001 üzerinden yük dengeleyiciye gelen trafiği WEB1 VM üzerindeki bağlantı noktası 3389’a iletir.
4. 2 ve 3 numaralı gereksinimler dışında İnternet'ten ön uç veya arka uç VM'lerine erişim olmaması.
5. WEB veya DB sunucularından giden İnternet erişimi olmaması.
6. Herhangi bir web sunucusunun 3389 numaralı bağlantı noktasına FrontEnd alt ağından erişime izin verilir.
7. Herhangi bir DB sunucusunun 3389 numaralı bağlantı noktasına FrontEnd alt ağından erişime izin verilir.
8. Tüm DB sunucularının 1433 numaralı bağlantı noktasına FrontEnd alt ağından erişime izin verilir.
9. DB sunucularındaki farklı ağ arabirimlerinde yönetim trafiğinin (3389 numaralı bağlantı noktası) ve veritabanı trafiğinin (1433) ayrılması.

1-6 gereksinimlerinin tümü (3 ve 4 gereksinimleri hariç) alt ağ alanlarıyla sınırlandırılmıştır. Aşağıdaki NSG'ler önceki gereksinimleri karşılarken, gerekli NSG sayısını en aza indirir:

### <a name="frontend"></a>FrontEnd
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-HTTP-Internet | İzin Ver | 100 | Internet | * | * | 80 | TCP |
| Allow-Inbound-RDP-Internet | İzin Ver | 200 | Internet | * | * | 3389 | TCP |
| Deny-Inbound-All | Reddet | 300 | Internet | * | * | * | TCP |

**Giden kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All |Reddet |100 | * | * | Internet | * | * |

### <a name="backend"></a>BackEnd
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Reddet | 100 | Internet | * | * | * | * |

**Giden kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Reddet | 100 | * | * | Internet | * | * |

Aşağıdaki NSG'ler oluşturulur ve aşağıdaki VM'ler içinde ağ arabirimleri ile ilişkilendirilir:

### <a name="web1"></a>WEB1
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Internet | İzin Ver | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | İzin Ver | 200 | Internet | * | * | 80 | TCP |

> [!NOTE]
> Önceki kuralların kaynak adres aralığı, yük dengeleyicinin sanal IP adresi değil, **Internet**’tir. Kaynak bağlantı noktası 500001 değil, * şeklindedir. Yük dengeleyiciler için NAT kuralları, NSG güvenlik kurallarıyla aynı değildir. NSG güvenlik kuralları her zaman için trafiğin orijinal kaynağı ve son hedefi ile ilgilidir, ikisi arasındaki yük dengeleyicisiyle **değil**. Azure Load Balancer, kaynak IP adresini ve bağlantı noktasını her zaman korur.
> 
> 

### <a name="web2"></a>WEB2
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Inbound-RDP-Internet | Reddet | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | İzin Ver | 200 | Internet | * | * | 80 | TCP |

### <a name="db-servers-management-nic"></a>DB sunucuları (Yönetim ağ arabirimi)
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Front-end | İzin Ver | 100 | 192.168.1.0/24 | * | * | 3389 | TCP |

### <a name="db-servers-database-traffic-nic"></a>DB sunucuları (Veritabanı trafiği ağ arabirimi)
**Gelen kuralları**

| Kural | Access | Öncelik | Kaynak adres aralığı | Kaynak bağlantı noktası | Hedef adres aralığı | Hedef bağlantı noktası | Protokol |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-SQL-Front-end | İzin Ver | 100 | 192.168.1.0/24 | * | * | 1433 | TCP |

Bazı NSG’ler ayrı ayrı ağ arabirimleri ile ilişkili olduğundan, kurallar Resource Manager aracılığıyla dağıtılan kaynaklar için geçerlidir. Nasıl ilişkilendirildiklerine bağlı olarak, kurallar alt ağ ve ağ arabirimi için birleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
* [NSG Dağıtma (Resource Manager)](virtual-networks-create-nsg-arm-pportal.md).
* [NSG Dağıtma (klasik)](virtual-networks-create-nsg-classic-ps.md).
* [NSG günlüklerini yönetin](virtual-network-nsg-manage-log.md).
* [NSG sorunlarını giderme](virtual-network-nsg-troubleshoot-portal.md)
