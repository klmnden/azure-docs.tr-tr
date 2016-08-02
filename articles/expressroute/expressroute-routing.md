<properties
   pageTitle="ExpressRoute için yönlendirme gereksinimleri | Microsoft Azure"
   description="Bu sayfada, ExpressRoute devreleri için yönlendirmeyi yapılandırma ve yönetmeye yönelik ayrıntılı gereksinimler verilmektedir."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/01/2016"
   ms.author="cherylmc"/>


# ExpressRoute yönlendirme gereksinimleri  

ExpressRoute kullanarak Microsoft bulut hizmetlerine bağlanmak için yönlendirmeyi ayarlamanız ve yönetmeniz gerekir. Bazı bağlantı sağlayıcıları yönlendirme ayarlama ve yönetimini yönetilen bir hizmet olarak sunar. Bu hizmetin sunulup sunulmadığını öğrenmek için bağlantı sağlayıcınıza başvurun. Bu hizmet sağlanmıyorsa aşağıda açıklanan gereksinimlere uymalısınız. 

Bağlantıyı kolaylaştırmak için ayarlanması gereken yönlendirme oturumlarının açıklaması için [Devreler ve yönlendirme etki alanları](expressroute-circuit-peerings.md) makalesine bakın.

**Not:** Microsoft, yüksek kullanılabilirlik yapılandırmaları için yönlendirici artıklık protokollerini (örn. HSRP, VRRP) desteklemez. Yüksek kullanılabilirlik için eşlik başına yedek bir BGP oturumları çifti kullanılır.

## Eşlikler için IP adresleri

Ağınız ile Microsoft'un Enterprise edge (MSEE) yönlendiricileri arasındaki yönlendirmeyi yapılandırmak üzere birkaç IP adresi bloğu ayırmanız gerekir. Bu bölümde gereksinimlerin bir listesi verilmekte ve bu IP adreslerinin nasıl elde edilip kullanılacağı hakkında kurallar açıklanmaktadır.

### Azure özel eşleme için IP adresleri

Eşlikleri yapılandırmak için özel IP adresleri veya ortak IP adresleri kullanabilirsiniz. Yolları yapılandırmak için kullanılan adres aralığı, Azure’da sanal ağlar oluşturmak için kullanılan adres aralıkları ile üst üste gelmemelidir. 

 - Arabirimleri yönlendirmek için bir /29 alt ağı veya iki /30 alt ağı ayırmanız gerekir.
 - Yönlendirme için kullanılan alt ağlar özel IP adresleri veya ortak IP adresleri olabilir.
 - Alt ağlar Microsoft bulutunda kullanılmak üzere müşteri tarafından ayrılan aralıkla çakışmamalıdır.
 - Bir /29 alt ağı kullanıldığında iki /30 alt ağına bölünür. 
     - Birinci /30 alt ağı birincil bağlantı ve ikinci /30 alt ağı ikincil bağlantı için kullanılır.
     - /30 alt ağın her biri için yönlendiriciniz üzerindeki /30 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu oluşturmak için /30 alt ağının ikinci IP adresini kullanır.
     - [Kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/)’sının geçerli olması için her iki BGP oturumunu da ayarlamanız gerekir.  

#### Özel eşleme örneği

Eşlik oluşturmak için a.b.c.d/29 kullanmayı seçerseniz iki /30 alt ağına bölünür. Aşağıdaki örnekte, a.b.c.d/29 alt ağının nasıl kullanıldığına bakılacaktır. 

a.b.c.d/29; a.b.c.d/30 ve a.b.c.d+4/30 olarak ayrılır ve sağlama API’leri yoluyla Microsoft’a geçirilir. a.b.c.d+1’i Birincil PE’nin VRF IP’si olarak kullanırsınız ve Microsoft birincil MSEE’nin VRF IP’si olarak a.b.c.d+2 kullanır. a.b.c.d+5’i ikincil PE’nin VRF IP’si olarak kullanırsınız ve Microsoft ikincil MSEE’nin VRF IP’si olarak a.b.c.d+6 kullanır.

Özel eşleme oluşturmak için 192.168.100.128/29’u seçtiğiniz bir durum düşünün. 192.168.100.128/29 adresi 192.168.100.128 ile 192.168.100.135 arasındaki adresleri içerir, bunlar arasında:

- 192.168.100.128/30 adresi bağlantı 1’e atanır, sağlayıcı 192.168.100.129 ve Microsoft 192.168.100.130 kullanır.
- 192.168.100.132/30 adresi bağlantı 2’ye atanır, sağlayıcı 192.168.100.133 ve Microsoft 192.168.100.134 kullanır.

### Azure genel ve Microsoft eşleme için IP adresleri

BGP oturumlarını ayarlamak için sahip olduğunuz ortak IP adreslerini kullanmanız gerekir. Microsoft, IP adreslerinin sahipliğini Routing Internet Registries ve Internet Routing Registries ile doğrulayabilmelidir. 

- Bir ExpressRoute devresindeki her eşleme (birden fazla varsa) için BGP eşliği oluşturmak üzere benzersiz bir /29 alt ağı veya iki /30 alt ağı kullanmanız gerekir. 
- Bir /29 alt ağı kullanıldığında iki /30 alt ağına bölünür. 
    - Birinci /30 alt ağı birincil bağlantı ve ikinci /30 alt ağı ikincil bağlantı için kullanılır.
    - /30 alt ağın her biri için yönlendiriciniz üzerindeki /30 al ağının birinci IP adresini kullanmanız gerekir. Microsoft bir BGP oturumu oluşturmak için /30 alt ağının ikinci IP adresini kullanır.
    - [Kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/)’sının geçerli olması için her iki BGP oturumunu da ayarlamanız gerekir.

IP adresi ve AS numarasının aşağıda listelenen kayıt defterlerinden birinde size kayıtlı olduğundan emin olun.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)


## Dinamik yönlendirme değişimi

Yönlendirme değişimi bir eBGP protokolü üzerinden olacaktır. EBGP oturumları MSEE’ler ile yönlendiricileriniz arasında oluşturulur. BGP oturumlarının kimlik doğrulaması zorunlu değildir. Gerekirse bir MD5 karması yapılandırılabilir. BGP oturumlarını yapılandırma hakkında bilgi için [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md) ve [Devre sağlama iş akışları ve devre durumları](expressroute-workflows.md) bölümlerine bakın.

## Otonom Sistem numaraları

Microsoft Azure genel, Azure özel ve Microsoft eşlemesi için AS 12076 kullanır. 65515 ile 65520 arasındaki ASN’ler şirket içi kullanım için ayrılmıştır. Hem 16 hem de 32 bit AS numaraları desteklenir.

Veri aktarımı simetrisi etrafında bir gereksinim yoktur. İleri ve geri dönüş yolları farklı yönlendirici çiftlerinden geçiş yapabilir. Aynı yollar size ait birden fazla devre çiftinin her iki tarafından tanıtılmalıdır. Yol ölçümlerinin aynı olması gerekmez.

## Yol toplama ve ön ek sınırları

Azure özel eşleme aracılığıyla bize tanıtılan 4000’e kadar ön eki destekliyoruz. ExpressRoute premium eklentisi etkinse bu sayı en fazla 10.000 ön eke kadar artırılabilir. Azure genel ve Microsoft eşlemesi için BGP oturumu başına en fazla 200 ön ek kabul edilmektedir. 

Ön ek sayısı bu sınırı aşarsa BGP oturumu düşürülür. Yalnızca özel eşleme bağlantısında varsayılan yollar kabul edilir. Sağlayıcının Azure genel ve Microsoft eşlemesi yollarından varsayılan yolu ve özel IP adreslerini (RFC 1918) filtrelemesi gerekir. 

## Geçiş yönlendirme ve çapraz bölge yönlendirme

ExpressRoute geçiş yönlendirici olarak yapılandırılamaz. Geçiş yönlendirme hizmetleri için bağlantı sağlayıcınızı kullanmanız gerekir.

## Varsayılan yolları tanıtma

Varsayılan yollar yalnızca Azure özel eşleme oturumlarında kullanılabilir. Böyle bir durumda, ilişkili sanal ağlardaki tüm trafik ağınıza yönlendirilecektir. Varsayılan yolların özel eşlemede tanıtılması Azure’daki Internet yolunun engellenmesiyle sonuçlanır. Azure içinde barındırılan hizmetler için internetten gelen ve giden trafiği yönlendirmek üzere kurumsal edge kullanmanız gerekir. 

 Diğer Azure hizmetleri ve altyapı hizmetleri ile bağlantıyı etkinleştirmek üzere aşağıdaki öğelerden birinin yerinde olduğundan emin olmanız gerekir:

 - Trafiği ortak uç noktalara yönlendirmek için Azure ortak eşleme etkindir
 - İnternet bağlantısı gerektiren her alt ağ için internet bağlantısına izin vermek üzere kullanıcı tanımlı yönlendirmeyi kullanıyorsunuz.

**Not:** Varsayılan yolların tanıtılması Windows ve diğer sanal makine lisans etkinleştirmelerini bozar. Bu sorunu çözmek için [buradaki](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx) yönergeleri izleyin.

## BGP toplulukları desteği (Önizleme)


Bu bölüm BGP toplulukların ExpressRoute ile nasıl kullanıldığına genel bir bakış sağlar. Microsoft genel ve Microsoft eşleme yollarındaki rotaları uygun topluluk değerleriyle etiketleyerek tanıtır. Bunu yapmanın gerekçesi ve topluluk değerlerine ilişkin ayrıntılar aşağıda açıklanmıştır. Ancak, Microsoft kendisine tanıtılan rotalara etiketlenmiş hiçbir topluluk değerini kabul etmez.

Microsoft’a ExpressRoute aracılığıyla jeopolitik bir bölgedeki herhangi bir eşleme konumundan bağlanıyorsanız jeopolitik sınır dahilindeki tüm bölgelerde bütün Microsoft bulut hizmetlerine erişiminiz olacaktır. 

Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlandıysanız Kuzey Avrupa ve Batı Avrupa’da barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. 

Jeopolitik bölgeler, ilişkili Azure bölgeleri ve karşılık gelen ExpressRoute eşleme konumlarını içeren ayrıntılı liste için [ExpressRoute iş ortakları ve eşleme konumları](expressroute-locations.md) sayfasına bakın.

Bir jeopolitik bölge için birden fazla ExpressRoute devresi satın alabilirsiniz. Birden fazla bağlantıya sahip olmanız coğrafi artıklık nedeniyle yüksek kullanılabilirliğe ilişkin önemli avantajlar sunar. Birden fazla ExpressRoute devrenizin olduğu durumlarda ortak eşleme ve Microsoft eşleme yollarında Microsoft’tan tanıtılan aynı ön eklerini alırsınız. Bu durum ağınız ile Microsoft arasında birden fazla yol olacağı anlamına gelir. Bu durum ağınızın içinde en iyi olmayan yönlendirme kararlarına neden olabilir. Sonuç olarak, farklı hizmetlerde en iyinin altında bağlantı deneyimleri yaşayabilirsiniz. 

Microsoft ortak eşleme ve Microsoft eşlemesi aracılığıyla tanıtılan ön eklere, ön eklerin barındırıldığı bölgeyi belirten uygun BGP topluluk değerlerini etiketler. [Müşteriler için en iyi yönlendirmeyi](expressroute-optimize-routing.md) sunmak üzere uygun yönlendirme kararlarını almak için topluluk değerlerini kullanabilirsiniz.

| **Jeopolitik Bölge** | **Microsoft Azure bölgesi** | **BGP topluluk değeri** |
|---|---|---|
| **Kuzey Amerika** |    |  |
|    | Doğu ABD | 12076:51004 |
|    | Doğu ABD 2 | 12076:51005 |
|    | Batı ABD | 12076:51006 |
|    | Orta Kuzey ABD | 12076:51007 |
|    | Orta Güney ABD | 12076:51008 |
|    | Orta ABD | 12076:51009 |
|    | Orta Kanada | 12076:51020 |
|    | Doğu Kanada | 12076:51021 |
| **Güney Amerika** |  |  |
|    | Güney Brezilya | 12076:51014 |
| **Avrupa** |    |  |
|    | Kuzey Avrupa | 12076:51003 |
|    | Batı Avrupa | 12076:51002 |
| **Asya Pasifik** |    |   |
|    | Doğu Asya | 12076:51010 |
|    | Güneydoğu Asya | 12076:51011 |
| **Japonya** |     |   |
|    | Japonya Doğu | 12076:51012 |
|    | Japonya Batı | 12076:51013 |
| **Avustralya** |    |   | 
|    | Avustralya Doğu | 12076:51015 |
|    | Avustralya Güneydoğu | 12076:51016 |
| **Hindistan** |    |   |
|    | Hindistan Güney | 12076:51019 |
|    | Hindistan Batı | 12076:51018 |
|    | Hindistan Orta | 12076:51017 |

Microsoft tarafından tanıtılan tüm yollar uygun topluluk değeriyle etiketlenecektir. 

>[AZURE.IMPORTANT] Genel ön ekler uygun bir topluluk değeri ile etiketlenecek ve yalnızca ExpressRoute premium eklentisi etkinleştirildiğinde tanıtılacaktır.


Yukarıdakilerin yanı sıra Microsoft, ön ekleri ait oldukları hizmet göre etiketleyecektir. Bu durum yalnızca Microsoft eşlemesi için geçerlidir. Aşağıdaki tabloda hizmetin BGP topluluk değeri ile eşleşmesi gösterilmektedir.

| **Hizmet** | **BGP topluluk değeri** |
|---|---|
| **Exchange** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype Kurumsal** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Diğer Office 365 Hizmetleri** | 12076:5100 |

>[AZURE.NOTE] Microsoft, Microsoft'a tanıtılan yollar üzerinde ayarladığınız hiçbir BGP topluluk değerini dikkate almaz.

## Sonraki adımlar

- ExpressRoute bağlantınızı yapılandırın.

    - [Klasik dağıtım modeli için ExpressRoute devresi oluşturma](expressroute-howto-circuit-classic.md) veya [Azure Resource Manager kullanarak ExpressRoute devresi oluşturma ya da değiştirme](expressroute-howto-circuit-arm.md)
    - [Klasik dağıtım modeli için yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md) veya [Resource Manager dağıtım modeli için yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
    - [Klasik VNet’i ExpressRoute devresine bağlama](expressroute-howto-linkvnet-classic.md) veya [Resource Manager VNet’i ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)





<!--HONumber=Jun16_HO2-->


