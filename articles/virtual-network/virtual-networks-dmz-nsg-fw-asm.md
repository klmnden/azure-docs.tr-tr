---
title: DMZ örnek – bir güvenlik duvarı ve Nsg'ler ile uygulamaları korumak için bir DMZ oluşturmak | Microsoft Docs
description: Bir güvenlik duvarı ve ağ güvenlik grupları (NSG) ile bir çevre ağı oluşturma
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: c78491c7-54ac-4469-851c-b35bfed0f528
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: 31d945f64cccd0c811d4dc45163583224102fb8a
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965248"
---
# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Örnek 2: bir güvenlik duvarı ve Nsg'ler ile uygulamaları korumak için bir DMZ oluşturma
[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

Bu örnek, bir güvenlik duvarı, dört windows sunucularını ve ağ güvenlik grupları ile DMZ oluşturma. Bu da her her adımda daha derin bir anlayış sağlamak için ilgili komutları yol gösterir. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryo bölümü yoktur adım adım dmz'deki savunma katmanları ile nasıl trafiği devam eder. Son olarak, test ve deneme ile çeşitli senaryolar için bu ortamı oluşturmak için yönerge ve tam kod başvuruları bölüm yöneliktir. 

![Gelen NVA ve NSG ile DMZ][1]

## <a name="environment-description"></a>Ortam tanımı
Bu örnekte aşağıdakileri içeren bir aboneliği vardır:

* İki bulut Hizmetleri: "FrontEnd001" ve "BackEnd001"
* "CorpNetwork" iki alt ağlı sanal ağ: "FrontEnd" ve "Arka uç"
* Tek bir ağ güvenlik her iki alt ağa uygulanan Grup
* Bu örnekte bir Barracuda NextGen Güvenlik Duvarı bir ağ sanal Gereci ön uç alt ağına bağlı
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows Server
* Uygulama geri temsil eden iki windows sunucuları sunucuları ("AppVM01", "AppVM02") sona
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server

> [!NOTE]
> Bu örnek bir Barracuda NextGen güvenlik duvarı kullansa da, birçok farklı ağ sanal Gereçleri Bu örnek için kullanılabilir.
> 
> 

Başvurular bölümdeki çoğu yukarıda açıklanan ortam oluşturacak bir PowerShell Betiği yoktur. VM'ler ve sanal ağlar oluşturma örnek komut dosyası tarafından yapılır ancak değil açıklanmıştır ayrıntılı bu belgedeki.

Ortamı oluşturmak için:

1. References bölümünde dahil ağ yapılandırma xml dosyasını kaydedin (adları, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilmiş)
2. Kullanıcı değişkenleri betiğinde betiğidir karşı (abonelik, hizmet adlarını, vb.) çalıştırılması için ortam eşleşecek şekilde güncelleştirin
3. PowerShell Betiği yürütün

**Not**: PowerShell betik miktarlara bölge ağ yapılandırma xml dosyasında miktarlara bölge eşleşmelidir.

Betik başarıyla çalıştırıldıktan sonra aşağıdaki betik sonrası adımlar izlenebilir:

1. Güvenlik duvarı kuralları ayarlamak, bu başlıklı aşağıdaki bölümde ele alınmıştır: Güvenlik duvarı kuralları.
2. İsteğe bağlı olarak bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile uygulama sunucusu ve web sunucusu kurmak için iki komut dosyası başvuruları bölümünde bulunur.

Sonraki bölümde, ağ güvenlik grupları betikleri ifadelerine çoğunu açıklar.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi. 

> [!TIP]
> Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız ve ardından daha genel "Reddet" kuralları son. Kurallar öncelik atanmış belirtir, ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları, gelen veya giden yönde (alt ağ perspektifinden) ya da uygulayabilirsiniz.
> 
> 

Bildirimli olarak, aşağıdaki kuralları için gelen trafiği oluşturulmakta:

1. İç DNS trafiği (bağlantı noktası 53) izin verilir
2. Herhangi bir VM için RDP trafiğinin (3389 bağlantı noktası) Internet'ten izin verilir
3. Nva'nın (Güvenlik Duvarı) İnternet'ten gelen HTTP trafiğine (bağlantı noktası 80) izin verilir
4. Tüm trafiğe (tüm bağlantı noktaları) IIS01 AppVM1 için izin verilir
5. Tüm trafiği (tüm bağlantı noktaları) İnternet'ten gelen tüm VNet (her iki alt ağ) reddedildi
6. Tüm trafik (tüm bağlantı noktaları) Frontend alt ağından arka uç alt ağına reddedildi

Bu kurallar ile hem kuralları 3 web sunucusuna Internet'ten gelen HTTP isteği, her alt ağa bağlı (izin ver) ve 5 (Reddet. geçerli), ancak yalnızca geçerli ve kural 5 oyuna değil gelir kural 3 daha yüksek bir önceliğe sahip olduğundan. Bu nedenle HTTP isteği için Güvenlik Duvarı izin verilir. Bu aynı trafiği DNS01 sunucunun erişmeye kural 5 (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmek için izin verilecek değil. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafik kuralları 1 ve 4) Konuşmayı gelen ön uç alt ağını engeller, bir saldırganın güvenlik ihlalleri saldırgan ön uç web uygulamasının erişimi sınırlı durumda bu arka uç ağını korur. arka uç "korumalı" ağa (yalnızca AppVM01 sunucu üzerinde kullanıma sunulan kaynakları).

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Hem yönergeleri, kullanıcı tanımlı yönlendirme gereklidir, bulunabilir farklı bir örnekte araştırılan bu trafiği kilitlemek için [ana güvenlik sınırı belge][HOME].

Yukarıda ele alındığı NSG kuralları NSG kuralları için oldukça benzerdir [örnek 1 - derleme Nsg'ler ile basit DMZ][Example1]. Lütfen her bir NSG kuralı ve onun özniteliklerini ayrıntılı bilgi için bu belgedeki NSG açıklamayı gözden geçirin.

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Bir yönetim istemcisi bir güvenlik duvarını yönetmeyi ve gerekli yapılandırmaları oluşturmak için bir bilgisayarda yüklü olması gerekir. Cihazı yönetmek, Güvenlik Duvarı (veya diğer NVA) belgelerinden satıcı bakın. Bu bölümün geri kalanında, güvenlik duvarı yapılandırmasını kendisini satıcıları Yönetimi istemcisi (yani Azure portal veya PowerShell) üzerinden anlatmaktadır.

İstemci Yükleme ve bu örnekte kullanılan Barracuda bağlanmak için yönergeler burada bulunabilir: [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin)

Güvenlik Duvarı'nı, kuralları iletme oluşturulması gerekir. Bu örnek yalnızca, Güvenlik Duvarı'na, ardından web sunucusuna internet trafiği yönlendiren olduğundan, yalnızca bir adet iletme NAT kuralı gereklidir. Barracuda NextGen Güvenlik Duvarı bu örnekte kullanılan üzerinde kuralın hedef NAT kuralı ("Dst bu trafiği geçirmek için NAT") olacaktır.

Aşağıdaki kuralı oluşturun (veya var olan varsayılan kuralları doğrulamak için), Barracuda NG yönetici istemci panodan başlangıç yapılandırma sekmesine gidin, işletimsel yapılandırmada bölümü Ruleset'a tıklayın. Adlı bir kılavuz için "Ana kuralları" Güvenlik Duvarı'nı mevcut etkin ve devre dışı bırakılan kuralları gösterir. Bu kılavuz sağ üst köşesinde küçük yeşil olduğundan "+" düğmesi, yeni bir kural oluşturmak için tıklayın (Not: "düğme"Kilitle"olarak işaretlenmiş ve oluşturma veya kuralları düzenlemek için"ruleset kilidini açmak için "Bu düğmeye tıklayın belirleyemiyoruz görürseniz, güvenlik duvarı değişiklikleri için kilitlenebilir" ve düzenlemeye izin ver). Mevcut bir kuralı düzenlemek istiyorsanız, bu kuralı seçin, sağ tıklayın ve kuralı Düzenle'yi seçin.

Yeni bir kural oluşturun ve "WebTraffic" gibi bir ad sağlayın. 

Hedef NAT kuralı simgesi şöyle görünür: ![Hedef NAT simgesi][2]

Kural, şunun gibi görünür:

![Güvenlik Duvarı Kuralı][3]

Burada herhangi bir güvenlik duvarı İsabetleri adresi gelen HTTP (bağlantı noktası 80 veya HTTPS için 443'tür) erişmeye güvenlik duvarının "DHCP1'in yerel IP" arabirimi gönderilmesine ve 10.0.1.5'i IP adresi ile Web sunucusuna yönlendirildi. Trafiği 80 numaralı bağlantı noktasında gelen ve 80 numaralı bağlantı noktasında web sunucusuna giden bağlantı noktası değişiklik gerekiyordu. Ancak, bu nedenle gelen bağlantı noktası 80 için güvenlik duvarında gelen bağlantı noktası 8080 web sunucusu çevirme 8080 numaralı bağlantı noktasında Web Sunucumuz dinledik hedef listesinin 10.0.1.5:8080 silinmiş.

Bağlantı yöntemi ayrıca, hedef Kuralı Internet'ten için "Dinamik SNAT" en uygun olan miktarlara. 

Yalnızca bir kural oluşturuldu ancak önceliği doğru şekilde ayarlandığını önemlidir. Tüm güvenlik duvarı kurallarında kılavuzunda alt (altında "BLOCKALL" kuralı) bu yeni kural ise, hiçbir zaman oyuna gelir. Web trafiği için yeni oluşturulan kuralın BLOCKALL kuralı olduğundan emin olun.

Sonra kural oluşturulur güvenlik duvarı gönderildi ve ardından etkinleştirilen gerekir, Bu yapılmazsa, kural değişikliği etkili olmaz. Anında iletme ve etkinleştirme işlemi, sonraki bölümde açıklanmıştır.

## <a name="rule-activation"></a>Kural etkinleştirme
Bu kural eklemek için değişiklik ruleset ile kural kümesi için Güvenlik Duvarı'nı karşıya ve etkinleştirilmelidir.

![Güvenlik duvarı kuralını etkinleştirme][4]

Üst sağ köşesindeki management istemcisi içinde düğme kümelerdir. Güvenlik Duvarı değiştirilmiş kuralları göndermek için "Değişiklikleri Gönder" düğmesine tıklayın ve ardından "Etkinleştir" düğmesine tıklayın.

Bu örnek ortam derleme güvenlik duvarı kural kümesi etkinleştirme'yle tamamlanmıştır. Bu ortamın test etmek için bir uygulama eklemek için başvurular bölümüne post derleme betiklerde isteğe bağlı olarak, çalıştırılabilir trafiği senaryolar aşağıda.

> [!IMPORTANT]
> Web sunucusuna doğrudan ulaşırsınız değil gerektiğini bilmeniz önemlidir. Bir tarayıcı FrontEnd001.CloudApp.Net HTTP sayfa istediğinde, HTTP uç noktasına (bağlantı noktası 80) bu trafiği web sunucusuna değil güvenlik duvarı geçirir. Güvenlik duvarının ardından kuralına göre oluşturulan yukarıda, Web sunucusuna istek NAT.
> 
> 

## <a name="traffic-scenarios"></a>Trafik senaryoları
#### <a name="allowed-web-to-web-server-through-firewall"></a>(İzin verilir) Web için Web sunucusu güvenlik duvarı üzerinden
1. Internet kullanıcı istekleri HTTP sayfasına FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti)
2. Bulut hizmeti geçiş trafiği 80 numaralı bağlantı noktasında güvenlik duvarı yerel arabirime 10.0.1.4:80 açık uç noktası
3. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet için Güvenlik Duvarı) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafik (10.0.1.4) Güvenlik Duvarı'nın iç IP adresi ile denk gelir.
5. Güvenlik Duvarı iletme kuralı bkz bu trafiği 80 numaralı bağlantı noktasını, web sunucusuna IIS01 yönlendirir
6. IIS01 web trafiği, bu isteği alır ve isteği işlemeye başlar.
7. IIS01 SQL Server AppVM01 hakkında bilgi için sorar.
8. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
9. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet için Güvenlik Duvarı) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak için trafiğe izin verilir, kuralı işlemeyi durdur
10. AppVM01 SQL sorgusunu alır ve bunlara yanıt verir
11. Arka uç alt ağda hiçbir giden kuralları olduğundan yanıta izin verilir
12. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
    2. Alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı, trafiğe izin verilmesi bu trafiğe izin.
13. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlandıktan ve istek sahibine gönderir
14. Bu bir güvenlik duvarı NAT oturumundan yanıt hedef (başlangıçta) için güvenlik duvarını olduğundan
15. Güvenlik Duvarı, Web sunucusundan bir yanıt alır ve Internet kullanıcıya iletir
16. Olduğundan hiçbir giden kuralları yanıt ön uç alt ağında izin verilir ve istediğiniz web sayfası Internet kullanıcı alır.

#### <a name="allowed-rdp-to-backend"></a>(İzin verilir) RDP için arka uç
1. İnternet üzerinde Sunucu Yöneticisi AppVM01 xxxxx RDP AppVM01 (atanan bağlantı noktası, Azure Portal veya PowerShell aracılığıyla bulunabilir) için rastgele atanan bağlantı noktası numarasını olduğu BackEnd001.CloudApp.Net:xxxxx üzerinde RDP oturumu isteği
2. Güvenlik Duvarı yalnızca FrontEnd001.CloudApp.Net adresinde dinlemeye olduğundan, bu trafik akışı ile sürecine dahil değildir
3. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) geçerli, trafiğin izin verilen, Dur kural işleme
4. Hiçbir giden kuralları, varsayılan kuralları uygulanır ve dönüş trafiğine izin verilir
5. RDP oturumu etkin
6. AppVM01 kullanıcı adı ve parolasını ister.

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) DNS sunucusu üzerinde Web sunucusu DNS araması
1. Sunucu, IIS01 www.data.gov bir veri akışı gereksinimlerini, ancak adresini çözümlemek için gereksinimleri web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağında 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS01 için DNS isteği gönderir
3. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
4. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli, trafiğin izin verilen, Dur kural işleme
5. DNS sunucusu isteği alır
6. DNS sunucusu önbelleğe alınmış adresleriniz değil ve bir kök DNS sunucusu internet'te sorar
7. Hiçbir giden kuralları arka uç alt ağı üzerinde trafiğe izin verilir
8. Bu oturum dahili olarak başlatıldıktan sonra Internet DNS sunucusu yanıt, yanıt izin verilir
9. DNS sunucusu yanıtı önbelleğe alır ve geri IIS01 ilk isteğine yanıt verir
10. Hiçbir giden kuralları arka uç alt ağı üzerinde trafiğe izin verilir
11. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
    2. Trafiğe izin verilmesi bu trafiğe izin alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı
12. IIS01 DNS01 yanıtı alır

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(İzin verilir) Web sunucu erişim dosyasında AppVM01
1. Bir dosyanın AppVM01 IIS01 sorar
2. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
3. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet için Güvenlik Duvarı) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak için trafiğe izin verilir, kuralı işlemeyi durdur
4. AppVM01 isteği alır ve (erişim yetkisi varsayılarak) dosyası ile yanıt verir
5. Arka uç alt ağda hiçbir giden kuralları olduğundan yanıta izin verilir
6. Ön uç alt ağını gelen kuralı işleme başlar:
   1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
   2. Alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı, trafiğe izin verilmesi bu trafiğe izin.
7. IIS sunucusu dosyayı alır

#### <a name="denied-web-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Web
Web sunucusu, IIS01 ve güvenlik duvarı aynı bulut hizmetinde olduğu aynı genel kullanıma yönelik IP adresi paylaşırlar. Bu nedenle tüm HTTP trafiğini güvenlik duvarı yönlendirilmesi. İstek başarıyla hizmet, ancak doğrudan Web sunucusuna gidilemiyor,, güvenlik duvarı üzerinden ilk tasarlandığı gibi başarılı. Bu bölümde trafik akışı için ilk senaryoda bakın.

#### <a name="denied-web-to-backend-server"></a>(Reddedildi) Arka uç sunucusuna Web
1. Internet kullanıcı BackEnd001.CloudApp.Net hizmet aracılığıyla AppVM01 bir dosyaya erişmeye çalışır
2. Dosya Paylaşımı için açık uç nokta olduğundan bu bulut hizmeti geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. NSG kuralı 5 (Internet'e VNet) uç noktaları için herhangi bir nedenle açıksa, bu trafiği engeller

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Reddedildi) DNS sunucusu Web DNS araması
1. Internet kullanıcı DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını arama dener
2. DNS için açık uç nokta olduğundan bu bulut hizmeti geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, bu trafiğin NSG kuralı 5 (Internet'e VNet) engellenecekse (Not: Bu kural 1'in (DNS), iki nedenden dolayı uygulanamaz ilk kaynak adresi internet, bu kural yerel sanal ağ kaynağı olarak yalnızca uygular. hiçbir zaman trafiği reddetmeye da bu bir izin verme kuralı olduğundan)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Reddedildi) Web güvenlik duvarı üzerinden SQL erişimi
1. Internet kullanıcı SQL veri FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) istekleri
2. SQL için açık uç nokta olduğundan bu bulut hizmeti geçecekse değildir ve güvenlik duvarı ulaşın mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG Kural 2'de (Internet için Güvenlik Duvarı) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafik (10.0.1.4) Güvenlik Duvarı'nın iç IP adresi ile denk gelir.
5. Güvenlik Duvarı için SQL iletme kuralları yok ve trafiği bırakır.

## <a name="conclusion"></a>Sonuç
Bu, uygulamanızı bir güvenlik duvarı ile koruma ve arka uç alt ağından gelen trafiği yalıtma göreceli olarak doğrudan ileriye doğru bir yoludur.

Daha fazla örnek ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırma
Bir PowerShell komut dosyasında tam komut dosyasını kaydedin. Ağ Yapılandırması "NetworkConf2.xml" adlı bir dosyaya kaydedin.
Kullanıcı tanımlı değişkenler, gerektiği gibi değiştirin. Betiği çalıştırın ve ardından yukarıdaki güvenlik duvarı kuralı kurulum yönergeleri izleyin.

#### <a name="full-script"></a>Tam betik
Bu betik, kullanıcı tanımlı değişkenleri esas alarak olur:

1. Bir Azure aboneliğine Bağlanma
2. Yeni depolama hesabı oluşturma
3. Yeni bir sanal ağ ve ağ yapılandırma dosyasında tanımlanan iki alt ağ oluşturma
4. 4 windows server sanal makineleri oluşturma
5. NSG dahil olmak üzere yapılandırın:
   * Bir NSG oluşturma
   * Kuralları ile doldurma
   * NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bilgisayar veya sunucu bir İnternet'e bağlı yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu betik çalıştırıldığında, uyarı veya PowerShell'de pop diğer bilgilendirme iletileri olabilir. Yalnızca hata iletileri kırmızı endişeye neden olan.
> 
> 

```powershell
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

        # VM 2 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"

        # VM 3 - The Second Application Server
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
    # No User Defined Variables or   #
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
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

    If ($FatalError) {
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
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
```

#### <a name="network-config-file"></a>Ağ yapılandırma dosyası
Güncelleştirilmiş konumu ile bu xml dosyasını kaydedin ve bağlantıyı yukarıdaki betik $NetworkConfigFile değişkeninde bu dosyaya ekleyin.

```xml
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
```

#### <a name="sample-application-scripts"></a>Örnek uygulama komut dosyaları
Bu ve diğer DMZ örnekleri için örnek uygulamayı yüklemek istiyorsanız, aşağıdaki bağlantıda bir sağlanmıştır: [Örnek uygulama betiği][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Gelen NSG ile DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Hedef NAT simgesi"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Güvenlik Duvarı Kuralı"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Güvenlik duvarı kuralını etkinleştirme"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
