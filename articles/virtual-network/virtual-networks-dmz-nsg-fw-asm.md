---
title: "DMZ örnek – yapı bir güvenlik duvarı ve Nsg'ler ile uygulamaları korumak için DMZ | Microsoft Docs"
description: "DMZ bir güvenlik duvarı ve ağ güvenlik grupları (NSG) ile derleme"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: cc0e8a3fa749eb2e6f65ef92c2d3cb404cfc8bc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Örnek 2 – bir güvenlik duvarı ve Nsg'ler ile uygulamaları korumak için DMZ derleme
[Güvenlik sınırı en iyi yöntemler sayfasına dön][HOME]

Bu örnek, bir güvenlik duvarı, dört windows sunucuları ve ağ güvenlik grupları ile DMZ oluşturur. Bu da her her adımın daha derin bir anlayış sağlamak için ilgili komutları yol gösterir. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryosu bölümü olan adım adım çevre savunma katmanlar arasında nasıl trafiği devam eder. Son olarak, içindeki başvuruların sınamak ve çeşitli senaryolarıyla denemeler için bu ortamı oluşturmak için yönerge ve tam bir kod bölümüdür. 

![NVA ve NSG gelen DMZ][1]

## <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, aşağıdakileri içeren bir abonelik vardır:

* İki bulut hizmetlerini: "FrontEnd001" ve "BackEnd001"
* Bir sanal ağ "CorpNetwork" iki alt ağ ile: "Ön uç" ve "Arka uç"
* Tek bir ağ güvenlik her iki alt ağlara uygulanan Grup
* Bu örnekte Barracuda NextGen Firewall bir ağ sanal gereç ön uç alt ağına bağlı
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama geri temsil eden iki windows sunucuları sunucuları ("AppVM01", "AppVM02") end
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu

> [!NOTE]
> Bu örnek Barracuda NextGen Firewall kullansa da, birçok farklı ağ sanal Gereçleri Bu örnek için kullanılabilir.
> 
> 

Başvurular bölümündeki yukarıda açıklanan ortamı çoğunu oluşturacak bir PowerShell komut dosyası yok. VM'ler ve sanal ağlar, derleme örnek komut dosyası tarafından yapılır rağmen değil açıklanmıştır bu belgede ayrıntılı.

Ortamı oluşturmak için:

1. References bölümünde bulunan ağ yapılandırma xml dosyasını kaydedin (adlar, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilir)
2. Kullanıcı değişkenleri karşı (Abonelikleri, hizmet adlarını, vb.) çalıştırılacak komut dosyasıdır ortamıyla eşleşecek şekilde güncelleştirin
3. PowerShell komut dosyası yürütme

**Not**: PowerShell Betiği miktarlara bölge ağ yapılandırması xml dosyasında miktarlara bölge ile eşleşmelidir.

Komut dosyası başarıyla çalıştıktan sonra aşağıdaki sonrası betik adımlar izlenebilir:

1. Güvenlik duvarı kuralları ayarlamak, bu başlıklı bölümde aşağıda ele alınmıştır: güvenlik duvarı kuralları.
2. İsteğe bağlı olarak web sunucusu ve uygulama sunucusu bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile ayarlamak için iki komut dosyası başvuruları bölümünde bulunur.

Sonraki bölümde ağ güvenlik grupları göre betikleri deyimleri çoğunu açıklar.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu yerleşik ve altı kurallarıyla yüklendi. 

> [!TIP]
> Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız gerekir ve daha genel "Deny" kuralları en son. Kurallar öncelik atanmış belirtir ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları her (alt ağ perspektifinden) gelen veya giden yönde uygulayabilirsiniz.
> 
> 

Bildirimli olarak, aşağıdaki kural gelen trafik için oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir
2. Her VM için RDP trafiğinin (3389 numaralı bağlantı noktası) Internet'ten izin verilir
3. NVA (Güvenlik Duvarı) için HTTP trafiğine (bağlantı noktası 80) Internet'ten izin verilir
4. Tüm trafiği (tüm bağlantı noktaları) IIS01 AppVM1 izin verilir
5. Tüm trafiği (tüm bağlantı noktaları) Internet'ten tüm VNet (her iki alt ağ) reddedildi
6. Tüm trafiği (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağa reddedildi

Bu kurallar ile bağlı her alt ağ için bir HTTP isteği hem kuralları 3, web sunucusu Internet'ten gelen ise (izin ver) ve 5 (uygulamak reddetme), ancak 3 kuralı, yalnızca geçerli olur ve kural 5 oyuna değil gelen daha yüksek öncelikli olduğundan. Bu nedenle HTTP isteği Güvenlik Duvarı'na izin. Bu aynı trafiği DNS01 sunucunun erişmeye, kural 5 (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmeniz izin verilmiyor. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafiği kurallarında 1 ve 4) Konuşmayı gelen ön uç alt ağ blokları, bu durumda arka uç ağ saldırgan ön uç web uygulamasında arka uç "korumalı" ağa (yalnızca AppVM01 sunucuda kullanıma sunulan kaynakları) erişimi sınırlı bir saldırganın güvenlik ihlalleri korur.

İnternet giden trafiğe izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Hem yönergeleri, kullanıcı tanımlı yönlendirme gereklidir, bulunabilir farklı bir örnekte keşfedilen bu trafiği kilitlemek için [ana güvenlik sınırı belge][HOME].

Yukarıda ele alındığı NSG kuralları NSG kuralları için çok benzer [örnek 1 - yapı Nsg'ler ile basit DMZ][Example1]. Lütfen her bir NSG kural ve onun özniteliklerini ayrıntılı bir bakış için bu belgedeki NSG tanımını gözden geçirin.

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Bir yönetim İstemcisi Güvenlik Duvarı'nı yönetmek ve gerekli yapılandırmaları oluşturmak için bir Bilgisayara yüklü gerekir. Belge, Güvenlik Duvarı (veya diğer NVA) satıcı cihaz yönetme konusuna bakın. Bu bölüm geri kalanı, güvenlik duvarı yapılandırmasını kendisini satıcılar Yönetimi istemcisi (yani Azure portal veya PowerShell) aracılığıyla anlatmaktadır.

İstemci Yükleme ve bu örnekte kullanılan Barracuda bağlanmak için yönergeler şurada bulunabilir: [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin)

Güvenlik Duvarı'nı kuralları iletme oluşturulması gerekir. Bu örnek yalnızca, Güvenlik Duvarı'nı ve ardından web sunucusuna bağlı Internet trafiğini yönlendiren olduğundan, yalnızca bir iletme NAT kuralı gereklidir. Barracuda NextGen Bu örnekte kullanılan Güvenlik Duvarı'nda kuralın hedef NAT kuralı ("Dst bu trafiği geçirmek için NAT") olacaktır.

Aşağıdaki kural oluşturma (veya var olan varsayılan kuralları doğrulamak için) Barracuda NG yönetici istemci panodan başlangıç yapılandırma sekmesine gidin, işletimsel yapılandırmasında bölüm Ruleset'ı tıklatın. Adlı bir kılavuz "Ana kuralları" Güvenlik Duvarı'nı varolan etkin ve devre dışı bırakılan kuralları gösterir. Bu kılavuz sağ üst köşesindeki olan küçük, yeşil "+" düğmesini tıklatın, yeni bir kural oluşturmak için burayı tıklatın (Not: "düğme"Kilitleme"olarak işaretlenmiş ve oluşturmak veya kuralları düzenlemek için"ruleset kilidini aç"ve düzenlenmesine olanak tanımak için bu düğmeye tıklayın yapamıyorsunuz görürseniz, güvenlik duvarı değişiklikler için kilitli olabilir"). Varolan bir kuralı düzenlemek istiyorsanız, o kural seçin, sağ tıklayın ve kuralını Düzenle seçin.

Yeni bir kural oluşturmak ve "WebTraffic" gibi bir ad sağlayın. 

Hedef NAT kuralı simgesi şöyle görünür: ![hedef NAT simgesi][2]

Kural şunun gibi görünür:

![Güvenlik Duvarı Kuralı][3]

Burada herhangi bir güvenlik duvarı isabetler adresi gelen HTTP (bağlantı noktası 80 veya HTTPS için 443) erişmeye güvenlik duvarının "DHCP1 yerel IP" arabirimin dışına gönderilen ve olması 10.0.1.5 IP adresi ile Web sunucusunu yeniden yönlendirildi. Trafiği bağlantı noktası 80 üzerinde gelen ve bağlantı noktası 80 üzerinde web sunucusuna giden bağlantı noktası değişikliğe gerekiyordu. Ancak, bu nedenle gelen bağlantı noktası 80 güvenlik duvarında gelen bağlantı noktası 8080 web sunucusu için çevirme 8080 bağlantı noktasından bizim Web sunucusu kulak varsa hedef listesi 10.0.1.5:8080 silinmiş.

Bağlantı yöntemi de, Internet'ten hedef kuralı için "Dinamik SNAT" en uygun olan miktarlara. 

Yalnızca bir kural oluşturuldu ancak önceliği doğru ayarlandığından emin önemlidir. Güvenlik Duvarı'nda tüm kuralların kılavuzunda (aşağıda "BLOCKALL" kuralı) altındaki Yeni kuralın ise, hiçbir zaman oyuna gelecektir. Yeni oluşturulan kuralın web trafiği için BLOCKALL kuralı olduğundan emin olun.

Kural oluşturulur, Güvenlik Duvarı'na gönderilir ve ardından etkinleştirilen gerekir, bu yapılmaz, kural değişikliği etkili olmaz. Anında iletme ve etkinleştirme işlemi bir sonraki bölümde açıklanmıştır.

## <a name="rule-activation"></a>Kural etkinleştirme
Bu kural eklemek için değişiklik ruleset ile ruleset Güvenlik Duvarı'na karşıya ve etkinleştirilmelidir.

![Güvenlik duvarı kuralını etkinleştirme][4]

Yönetim istemcisi üst sağ alt köşesindeki düğmeleri oluşan bir küme var. Değiştirilen kuralları Güvenlik Duvarı'na göndermek için "Değişiklikleri Gönder" düğmesini tıklatın, ardından "Etkinleştir" düğmesini tıklatın.

Güvenlik Duvarı ruleset etkinleştirme ile bu örnek ortamı yapı tamamlanır. Test etmek için bu ortam bir uygulama eklemek için başvurular bölümüne post yapı komut isteğe bağlı olarak çalıştırılabilir trafiği senaryolar aşağıda.

> [!IMPORTANT]
> Web sunucusu doğrudan karşılaşır değil, hayata geçirmek için önemlidir. Bir tarayıcı FrontEnd001.CloudApp.Net HTTP sayfası istediğinde, HTTP uç noktası (bağlantı noktası 80) web sunucusu Güvenlik Duvarı'na bu trafiği geçirir. Güvenlik Duvarı daha sonra kural göre oluşturulan yukarıda, Web sunucusuna istek NAT.
> 
> 

## <a name="traffic-scenarios"></a>Trafik senaryoları
#### <a name="allowed-web-to-web-server-through-firewall"></a>(İzin verilir) Web için Web sunucusu güvenlik duvarı üzerinden
1. Internet kullanıcı istekleri HTTP sayfasına FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti)
2. Bulut hizmeti geçiş trafiği bağlantı noktası 80 üzerinde güvenlik duvarı yerel arabirime 10.0.1.4:80 üzerinde açık uç noktası aracılığıyla
3. Ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kural 3 (Internet güvenlik duvarı için) geçerlidir, izin verilen, Dur kural işlenirken trafiğidir
4. İç IP adresi (10.0.1.4) Güvenlik Duvarı'nın trafik isabetler
5. Güvenlik Duvarı iletme kuralı bkz: Bu bağlantı noktası 80 trafiği, web sunucusuna IIS01 yönlendirir
6. IIS01 web trafiği için dinleme, bu isteği alır ve isteği işlemeye başlıyor
7. IIS01 SQL Server AppVM01 hakkında bilgi için sorar
8. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
9. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kural 3 (Internet güvenlik duvarı için) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak, trafiğine izin verilir, kural işleme Durdur
10. AppVM01 SQL sorgusu alır ve yanıtlar
11. Arka uç alt ağda hiçbir giden kuralları olduğundan yanıt izin verilir
12. Ön uç alt gelen kuralı işleme başlar:
    1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
    2. Alt ağlar arasında trafiğe izin varsayılan sistem kuralı, trafiğine izin bu trafiği olanak tanır.
13. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlar ve istek sahibine gönderir
14. Güvenlik Duvarı'ndan bir NAT oturumu olduğundan, yanıt hedef için Güvenlik Duvarı'nı (başlangıçta) kalır.
15. Güvenlik Duvarı Web sunucusundan yanıtı alır ve Internet kullanıcıya geri iletir
16. Olduğundan hiçbir giden kuralları yanıt ön uç alt ağda izin verilir ve istenen web sayfasının Internet kullanıcı alır.

#### <a name="allowed-rdp-to-backend"></a>(İzin verilir) Arka uç için RDP
1. Sunucu Yöneticisi internet üzerindeki AppVM01 xxxxx rastgele atanan bağlantı noktası numarası (atanan bağlantı noktası, Azure Portal veya PowerShell aracılığıyla bulunabilir) AppVM01 için RDP olduğu BackEnd001.CloudApp.Net:xxxxx üzerinde RDP oturumu istekleri
2. Güvenlik Duvarı'nı yalnızca FrontEnd001.CloudApp.Net adresini dinlemesini olduğundan, bu ile bu trafik akışı dahil değil
3. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) uygulamak için izin verilen, Dur kural işlenirken trafiğidir
4. Hiçbir giden kuralları, varsayılan kuralları uygula ve dönüş trafiğine izin verilir
5. RDP oturumu etkin
6. Kullanıcı adı ve parolasını AppVM01 ister

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) DNS sunucusundaki Web sunucusu DNS araması
1. Sunucu, IIS01, www.data.gov bir veri akışı gereksinimlerini ancak adresini çözümlemek için gereksinimlerini web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağda 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS isteği için DNS01 gönderir
3. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
4. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) uygulamak için izin verilen, Dur kural işlenirken trafiğidir
5. DNS sunucusu isteği alır
6. DNS sunucusu önbelleğe adresi yoksa ve bir kök DNS sunucusu internet'te sorar
7. Hiçbir giden kuralları arka uç alt ağdaki trafiğe izin verilir
8. Bu oturumda dahili olarak başlatıldı beri Internet DNS sunucusu yanıt, yanıt izin verilir
9. DNS sunucusu yanıtı önbelleğe alır ve geri IIS01 ilk isteğine yanıt verir
10. Hiçbir giden kuralları arka uç alt ağdaki trafiğe izin verilir
11. Ön uç alt gelen kuralı işleme başlar:
    1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
    2. Trafiğe izin için alt ağlar arasında trafiğe izin varsayılan sistem kuralı bu trafiği olanak tanır
12. IIS01 DNS01 yanıtı alır

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(İzin verilir) Web sunucusu erişimini dosyasını AppVM01 üzerinde
1. Bir dosyanın AppVM01 IIS01 sorar
2. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
3. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kural 3 (Internet güvenlik duvarı için) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak, trafiğine izin verilir, kural işleme Durdur
4. AppVM01 isteği alır ve (erişim yetkisi varsayılarak) dosyası ile yanıt verir
5. Arka uç alt ağda hiçbir giden kuralları olduğundan yanıt izin verilir
6. Ön uç alt gelen kuralı işleme başlar:
   1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
   2. Alt ağlar arasında trafiğe izin varsayılan sistem kuralı, trafiğine izin bu trafiği olanak tanır.
7. IIS sunucu dosya alır

#### <a name="denied-web-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Web
Web sunucusu, IIS01 ve güvenlik duvarı aynı bulut hizmeti olduğundan aynı genel kullanıma yönelik IP adresi paylaştıkları. Bu nedenle herhangi bir HTTP trafiğini Güvenlik Duvarı'na yönelik. İstek başarılı bir şekilde sunulması, ancak doğrudan Web sunucusuna gidemezsiniz, onu, güvenlik duvarı üzerinden ilk tasarlandığı gibi geçirildi. Bu bölümde trafik akışı için ilk senaryoda bakın.

#### <a name="denied-web-to-backend-server"></a>(Reddedildi) Arka uç sunucusuna Web
1. Internet kullanıcı BackEnd001.CloudApp.Net hizmeti aracılığıyla AppVM01 bir dosyaya erişmeyi dener
2. Dosya Paylaşımı için açık uç nokta yok olduğundan bu bulut hizmeti geçip geçmeyeceğini değil ve tarafından sunucuya ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kural 5 (sanal ağa Internet) bu trafiği engeller

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Reddedildi) DNS sunucusundaki Web DNS araması
1. Internet kullanıcı DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını arama dener
2. DNS için açık uç nokta yok olduğundan bu bulut hizmeti geçip geçmeyeceğini değil ve tarafından sunucuya ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kural 5 (sanal ağa Internet) bu trafiği engeller (Not: Bu kural 1 (DNS), iki nedenden dolayı geçerli değil, ilk kaynak adresi Internet, bu kural yalnızca kaynağı olarak yerel vnet'e uygulanır, hiçbir zaman trafiği reddetmeye böylece de bu bir izin verme kuralı)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Reddedildi) Güvenlik Duvarı üzerinden SQL erişmek için Web
1. Internet kullanıcı SQL verileri FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) ' ister.
2. SQL için açık uç nokta yok olduğundan bu bulut hizmeti geçip geçmeyeceğini değil ve güvenlik duvarı ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 2 (Internet güvenlik duvarı için) geçerlidir, izin verilen, Dur kural işlenirken trafiğidir
4. İç IP adresi (10.0.1.4) Güvenlik Duvarı'nın trafik isabetler
5. Güvenlik Duvarı için SQL iletme kuralları yok ve trafiği bırakır

## <a name="conclusion"></a>Sonuç
Bu bir güvenlik duvarı ile uygulamanızı koruma ve arka uç alt ağından gelen trafiği yalıtma göreceli olarak doğrudan ileriye doğru bir yoludur.

Daha fazla örnekler ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırması
Tam komut dosyasını bir PowerShell komut dosyası kaydedin. Ağ Yapılandırma "NetworkConf2.xml" adlı bir dosyaya kaydedin.
Kullanıcı tanımlı değişkenleri gerektiği gibi değiştirin. Komut dosyasını çalıştırın, ardından yukarıdaki güvenlik duvarı kuralı kurulum yönergeleri izleyin.

#### <a name="full-script"></a>Tam komut dosyası
Kullanıcı tanımlı değişkenleri esas alarak bu betiği olur:

1. Bir Azure aboneliğine Bağlanma
2. Yeni depolama hesabı oluşturma
3. Yeni bir VNet ve ağ yapılandırma dosyasında tanımlanan iki alt ağ oluşturma
4. 4 windows server Vm'lerinin oluşturma
5. NSG dahil olmak üzere yapılandırın:
   * Bir NSG oluşturma
   * Kuralları ile doldurma
   * NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bir bilgisayar veya sunucu, Internet'e yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırdığınızda, uyarı veya PowerShell'de pop diğer bilgilendirici iletileri olabilir. Yalnızca hata iletileri kırmızı sorunu nedeni edilir.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5

      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6

    #>

    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()

    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section

      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"

      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"

        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"

        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

    # Validation
    $FatalError = $false

    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *

        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Ağ yapılandırma dosyası
Güncelleştirilmiş konumla bu xml dosyasını kaydedin ve bu dosyaya yukarıdaki komut $NetworkConfigFile değişken bağlantı ekleyin.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Örnek uygulama komut dosyaları
Bu ve diğer çevre örnekleri için örnek bir uygulama yüklemek isterseniz, bir aşağıdaki bağlantıda sağlanmış: [örnek uygulama betiği][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "NSG ile giriş DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Hedef NAT simgesi"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Güvenlik Duvarı Kuralı"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Güvenlik duvarı kuralını etkinleştirme"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
