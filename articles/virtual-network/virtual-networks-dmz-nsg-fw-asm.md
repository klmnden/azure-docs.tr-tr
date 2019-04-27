---
title: Çevre ağ örnek – uygulamaları bir güvenlik duvarı ile korunacak derleme bir çevre ağına ve Nsg'ler | Microsoft Docs
description: Çevre ağı ile bir güvenlik duvarı ve ağ güvenlik grupları (Nsg'ler) oluşturun
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
ms.openlocfilehash: e0271c9212b093bd803518ebeaa4b7d9682cc773
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60868374"
---
# <a name="example-2-build-a-perimeter-network-to-protect-applications-with-a-firewall-and-nsgs"></a>Örnek 2: Bir güvenlik duvarı ve Nsg'ler ile uygulamaları korumak için bir çevre ağı oluşturma
[Microsoft bulut Hizmetleri ve ağ güvenlik Sayfası'na dönmek][HOME]

Bu örnek, bir çevre ağı oluşturma işlemi gösterilmektedir (olarak da bilinen *DMZ*, *sivil bölge*, ve *denetimli alt ağ*) bir güvenlik duvarı, dört Windows Server bilgisayarları ve ağ güvenlik grupları (Nsg'ler). Bu, her adımda daha derin bir anlayış sağlamak için ilgili komutların her birine hakkındaki ayrıntıları içerir. "Trafiği senaryoları" Bölüm trafiği çevre ağındaki savunma katmanları ile nasıl devam eder, adım adım bir açıklama sağlar. Son olarak, "Başvuru" bölümünde tam kod ve test ve deneme ile farklı senaryolar için bu ortamı oluşturmak yönergeler sağlar.

![NVA ve Nsg'ler ile gelen çevre ağı][1]

## <a name="environment"></a>Ortam 
Bu örnek bir senaryo bu öğeleri içeren bir Azure aboneliği ile temel alır:

* İki bulut Hizmetleri: FrontEnd001 ve BackEnd001.
* İki alt ağa sahip CorpNetwork adlı bir sanal ağ: Ön uç ve arka uç.
* Her iki alt ağa uygulanan tek bir NSG.
* Bir ağ sanal Gereci: Barracuda NextGen güvenlik duvarı ön uç alt ağına bağlı.
* Bir uygulamanın web sunucusunu temsil eder bir Windows Server bilgisayarı: IIS01.
* Uygulama arka uç sunucuları temsil eden iki Windows Server bilgisayarları: AppVM01 ve AppVM02.
* Windows Server bilgisayarı bir DNS sunucusunu temsil eder: DNS01.

> [!NOTE]
> Bu örnek bir Barracuda NextGen güvenlik duvarı kullansa da, birçok ağ sanal Gereçleri kullanılabilir.
> 
> 

Bu makalede "Başvuru" bölümündeki PowerShell betiğini çoğunu burada açıklanan ortamı oluşturur. VM'ler ve sanal ağlar komut dosyası tarafından oluşturulur, ancak bu işlemleri ayrıntılı bu belgede açıklanan değildir.

Ortamı oluşturmak için:

1. "Başvuru" bölümünde yer alan ağ yapılandırma XML dosyasını kaydedin (adları, konumları ve IP adresleri senaryonuzla eşleşecek şekilde güncelleştirilir).
2. Kullanıcı tanımlı değişkenler komut dosyası (abonelik, hizmet adlarını ve benzeri karşı) çalıştırılır ortamınızla eşleşen için PowerShell betiğini güncelleştirin.
3. PowerShell Betiği çalıştırın.

> [!NOTE]
> PowerShell komut dosyasında belirtilen bölge ağ yapılandırma XML dosyasında belirtilen bölge eşleşmelidir.
>
>

Betik başarıyla çalıştırıldıktan sonra bu adımları tamamlayın:

1. Güvenlik duvarı kurallarını ayarlayın. Bu makalenin "güvenlik duvarı kurallarını" bölümüne bakın.
2. İsterseniz, web sunucusu ve uygulama sunucusu çevre ağ yapılandırmasıyla sınama izin vermek için basit bir web uygulaması ile ayarlayabilirsiniz. "Başvuru" bölümünde sağlanan iki uygulama komut dosyalarını kullanabilirsiniz.

İlişkili Nsg'ler için betiğin deyimlerinin çoğu sonraki bölümde açıklanmaktadır.

## <a name="nsgs"></a>NSG'ler
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi.

> [!TIP]
> Genel olarak, belirli "İzin ver" kurallarınızı önce oluşturmanız ve daha genel "Reddet" kuralları son. Kurallar öncelik atanmış denetimleri ilk değerlendirilir. Trafiği belirli bir kural için uygulanan bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları gelen veya giden yön (alt ağ perspektifinden) uygulayabilirsiniz.
> 
> 

Bildirimli olarak, bu kurallar, gelen trafik için oluşturulur:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir.
2. Herhangi bir VM için RDP trafiğinin (3389 bağlantı noktası) internet'ten izin verilir.
3. Nva'nın (Güvenlik Duvarı) İnternet'ten gelen HTTP trafiğine (bağlantı noktası 80) izin verilir.
4. Giden tüm trafiği (tüm bağlantı noktaları) IIS01 AppVM01 izin verilir.
5. Tüm trafiği (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağ (her iki alt ağ) reddedildi.
6. Tüm trafiği (tüm bağlantı noktaları) FrontEnd alt ağından arka uç alt ağına reddedildi.

İlişkili kurallar ile bir HTTP isteği web sunucusuna internet'ten gelen, her alt ağ, hem de kural 3 (izin ver) ve kural 5 (Reddet) uygulama görünüyor, ancak yalnızca kural 3 daha yüksek bir önceliğe sahip olduğundan uygulanır. Kural 5 oyuna gelen olmaz. Bu nedenle HTTP isteği için Güvenlik Duvarı izin verilir.

Bu aynı trafiği DNS01 sunucunun erişmeye, 5 kural (Reddet) sunucuya geçirmek için trafiğe izin verilmesi mıydı uygulamak için ilk olması. Kural 6 (Reddet) arka uç alt ağı (hariç, trafik kuralları 1 ve 4'e izin) ile iletişim kurmasını ön uç alt ağını engeller. Ön uç web uygulamasında bir saldırganın etkilediğinde durumunda bu arka uç ağını korur. Bu durumda, saldırganın korunan arka uç"" Ağ erişimi sınırlı. (Saldırgan AppVM01 sunucu üzerinde kullanıma sunulan kaynaklara erişmek mümkün olacaktır.)

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gerekir. Bu teknikte farklı bir örnekte hakkında bilgi edinebilirsiniz [ana güvenlik sınırı belge][HOME].

Burada açıklanan NSG kuralları NSG kuralları benzerdir [örnek 1 - derleme Nsg'ler ile basit DMZ][Example1]. Her bir NSG kuralı ve onun özniteliklerini ayrıntılı bilgi için bu makaledeki NSG açıklamayı gözden geçirin.

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları
Bir yönetim istemcisi bir güvenlik duvarını yönetmeyi ve gerekli yapılandırmaları oluşturmak için bir bilgisayara yüklemeniz gerekir. Cihaz yönetme hakkında belgeleri, Güvenlik Duvarı (veya diğer NVA) bkz. Bu bölümün geri kalanında, güvenlik duvarı yapılandırmasını kendisi, satıcının yönetim istemci (Azure portalı veya PowerShell değil) aracılığıyla açıklar.

Bkz: [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin) için istemci indirme ve bu örnekte kullanılan Barracuda güvenlik duvarı bağlanmak için yönergeler.

Güvenlik Duvarı'nı iletme kurallarını oluşturmak için ihtiyacınız. Bu örnek senaryoda, yalnızca internet trafiği yönlendirir güvenlik duvarı gelen ve ardından web sunucusu için yalnızca bir adet iletme NAT kuralı gerekir. Bu örnekte kullanılan Barracuda Güvenlik Duvarı'nı, bir hedef NAT kuralı (Dst bu trafiği geçirmek için NAT) kuralı olacaktır.

Aşağıdaki kural oluşturmak için (veya var olan varsayılan kuralları doğrulamak için), şu adımları tamamlayın:
1. Barracuda NG yönetici istemci panosunda, yapılandırma sekmesinde, içinde **işletimsel yapılandırma** bölümünden **Ruleset**. 

   Bir kılavuz olarak adlandırılan **ana kuralları** mevcut etkin gösterir ve güvenlik duvarı kurallarını devre dışı bırakıldı.

2. Yeni bir kural oluşturmak için küçük yeşil seçin **+** bu kılavuz sağ üst köşesindeki düğme. (Güvenlik duvarınızı kilitli olabilir. İşaretli bir düğme görürseniz **kilit** ve oluşturma veya kuralları düzenlemek, ruleset kilidini açmak ve düzenlenmesine olanak tanımak için düğmeyi seçin.)
  
3. Mevcut bir kuralı düzenlemek için kuralı, sağ tıklayın ve ardından seçin **kuralını Düzenle**.

Aşağıdakine benzer adlı yeni bir kural oluşturmak **WebTraffic.** 

Hedef NAT kuralı simgesi şöyle görünür: ![Hedef NAT simgesi][2]

Kural bu şekilde görünür:

![Güvenlik Duvarı Kuralı][3]

HTTP (bağlantı noktası 80 ve HTTPS için 443'tür) erişmeye güvenlik duvarı İsabetleri herhangi bir gelen adresi güvenlik duvarının dışında DHCP1'in yerel IP arabirimi gönderilen ve IP adresiyle 10.0.1.5'i web sunucusuna yönlendirildi. Trafiği 80 numaralı bağlantı noktasında gelen ve 80 numaralı bağlantı noktasında web sunucusuna giden olduğundan, hiçbir bağlantı noktası değişikliği gerekli değildir. Ancak, gelen bağlantı noktası 80 için güvenlik duvarında gelen bağlantı noktası 8080 web sunucusuna çevirir 8080 bağlantı noktasında web sunucusu dinledik, hedef listesinin 10.0.1.5:8080 silinmiş.

Bağlantı yöntemi de belirtmeniz. İnternet'ten hedef kuralı için dinamik SNAT en uygun yöntemdir.

Bir kural yalnızca oluşturduğunuz olsa da, önceliği doğru şekilde ayarlanması önemlidir. Bu yeni kural alt (altında BLOCKALL kuralı) ise tüm güvenlik duvarı kurallarında kılavuzunda, hiçbir zaman oyuna gelir. Yeni kuralı web trafiği için BLOCKALL kuralı olduğundan emin olun.

Kural oluşturulduktan sonra güvenlik duvarı gönderme ve onu etkinleştirmeniz gerekir. Bu adımlar yoksa, kural değişikliği uygulanmayacak. Sonraki bölümde anında iletme ve etkinleştirme işlemi açıklanır.

## <a name="rule-activation"></a>Kural etkinleştirme
Kural için bir kural kümesi eklenir, güvenlik duvarı kural kümesi yüklemek ve etkinleştirmek gerekir.

![Güvenlik duvarı kuralını etkinleştirme][4]

Yönetim istemci sağ üst köşesinde bulunan bir grup düğmesi görürsünüz. Seçin **değişiklikleri göndermek** değiştirilmiş ruleset güvenlik duvarı gönderin ve ardından **etkinleştirme**.

Güvenlik duvarı kural kümesi etkinleştirildikten sonra tam bir ortamdır. İsterseniz, ortama bir uygulama eklemek için "Başvuru" bölümündeki örnek uygulama komut dosyalarını çalıştırabilirsiniz. Uygulama eklerseniz, sonraki bölümde açıklanan trafiği senaryoları test edebilirsiniz.

> [!IMPORTANT]
> Web sunucusu doğrudan yakalayamazsınız dikkate almalısınız. Bir tarayıcı FrontEnd001.CloudApp.Net HTTP sayfa istediğinde, HTTP uç noktasına (bağlantı noktası 80) trafiği geçirir güvenlik duvarı, web sunucusuna değil. Güvenlik Duvarı NAT daha önce oluşturduğunuz kural nedeniyle, isteği web sunucusuna eşleştirmek için daha sonra kullanır.
> 
> 

## <a name="traffic-scenarios"></a>Trafik senaryoları
#### <a name="allowed-web-to-web-server-through-the-firewall"></a>(İzin verilir) Web sunucusu güvenlik duvarı üzerinden Web
1. İnternet kullanıcı FrontEnd001.CloudApp.Net (internet'e yönelik bulut hizmeti) istekler bir HTTP sayfası.
2. Bulut hizmeti 80 numaralı bağlantı noktasında 10.0.1.4:80 yerel arabirimdeki güvenlik duvarının açık bir uç nokta üzerinden geçen trafik geçirir.
3. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli değildir. Sonraki kuralın taşıyın.
   2. NSG kuralı 2 (RDP) geçerli değildir. Sonraki kuralın taşıyın.
   3. NSG kuralı 3 (internet için Güvenlik Duvarı) uygulayın. Trafiğine izin verin. Kuralı işlemeyi durdur.
4. Trafik (10.0.1.4) Güvenlik Duvarı'nın iç IP adresi denk gelir.
5. Bu bağlantı noktası 80 trafiği ve web sunucusuna IIS01 yönlendiren iletme kuralı bir güvenlik duvarı belirler.
6. IIS01 web trafiği için dinleme isteği alır ve isteği işlemeye başlar.
7. IIS01 AppVM01 üzerinde SQL Server örneğinden bilgi ister.
8. Giden kural yok ön uç alt ağı trafiğine izin verilmesi.
9. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli değildir. Sonraki kuralın taşıyın.
   2. NSG kuralı 2 (RDP) geçerli değildir. Sonraki kuralın taşıyın.
   3. NSG kuralı 3 (internet için Güvenlik Duvarı) geçerli değildir. Sonraki kuralın taşıyın.
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulayın. Trafiğine izin verin. Kuralı işlemeyi durdur.
10. SQL Server örneği üzerinde AppVM01 isteği alır ve yanıt verir.
11. Arka uç alt ağda bir giden kuralı yok olduğundan, yanıt izin verilir.
12. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Geçerli NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği gelen.
    2. Alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı, trafiğine izin verilmesi bu trafiği sağlar.
13. IIS01 AppVM01 yanıtı alır, HTTP yanıtı tamamlanır ve istek sahibine gönderir.
14. Bu Güvenlik Duvarı'ndan bir NAT oturumu olduğundan yanıt hedefi için güvenlik duvarını başlangıçta durumundadır.
15. Güvenlik Duvarı, web sunucusundan yanıt alır ve internet kullanıcıya geri iletir.
16. Ön uç alt ağda bir giden kuralı yok olduğundan, yanıt izin verilir ve web sayfası Internet kullanıcı alır.

#### <a name="allowed-rdp-to-backend"></a>(İzin verilir) RDP için arka uç
1. İnternet üzerindeki bir sunucu yöneticisi istekleri AppVM01 BackEnd001.CloudApp.Net üzerinde bir RDP oturumu:*xxxxx*burada *xxxxx* AppVM01 yönelik RDP için rastgele atanan bağlantı noktası numarasıdır. (Atanmış bağlantı Azure portalında veya PowerShell kullanarak bulabilirsiniz.)
2. Güvenlik Duvarı yalnızca FrontEnd001.CloudApp.Net adresinde dinlediğinden, bu trafik akışı dahil değildir.
3. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli değildir. Sonraki kuralın taşıyın.
   2. NSG kuralı 2 (RDP) uygulayın. Trafiğine izin verin. Kuralı işlemeyi durdur.
4. Giden kural olduğundan, varsayılan kuralları uygulamak ve trafiğe izin döndürür.
5. RDP oturumu etkin.
6. AppVM01 bir kullanıcı adı ve parola bilgilerini ister.

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) Web sunucusu DNS sunucusu DNS araması
1. Veri akışı www.data.gov ancak adresini çözümlemek gerekli web sunucusu, IIS01, gerekiyor.
2. Ağ yapılandırması için sanal ağ gösterir DNS01 (arka uç alt ağında 10.0.2.4) birincil DNS sunucusu. IIS01 DNS01 için DNS isteği gönderir.
3. Ön uç alt ağda bir giden kuralı yok olduğundan, trafiğe izin verilir.
4. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) uygular. Trafiğine izin verin. Kuralı işlemeyi durdur.
5. DNS sunucusu isteği alır.
6. DNS sunucusu, önbelleğe alınmış adresleriniz değil ve internet üzerindeki bir kök DNS sunucusunu sorgular.
7. Arka uç alt ağda bir giden kuralı yok olduğundan, trafiğe izin verilir.
8. İnternet DNS sunucusu yanıt verir. Yanıt, oturum dahili olarak başlatıldığından izin verilir.
9. DNS sunucusu yanıtı önbelleğe alır ve IIS01 isteğine yanıt verir.
10. Arka uç alt ağda bir giden kuralı yok olduğundan, trafiğe izin verilir.
11. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Geçerli NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği gelen.
    2. Trafiğe izin verilmesi alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı bu trafiğe izin verir.
12. IIS01 DNS01 yanıtı alır.

#### <a name="allowed-web-server-file-access-on-appvm01"></a>(İzin verilir) Web sunucusu AppVM01 dosya erişimi
1. Bir dosya çubuğunda AppVM01 IIS01 ister.
2. Ön uç alt ağda bir giden kuralı yok olduğundan, trafiğe izin verilir.
3. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli değildir. Sonraki kuralın taşıyın.
   2. NSG kuralı 2 (RDP) geçerli değildir. Sonraki kuralın taşıyın.
   3. NSG kuralı 3 (internet için Güvenlik Duvarı) geçerli değildir. Sonraki kuralın taşıyın.
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulayın. Trafiğine izin verin. Kuralı işlemeyi durdur.
4. AppVM01 isteği alır ve (erişim yetkisi varsayılarak) dosyası ile yanıt verir.
5. Arka uç alt ağda bir giden kuralı yok olduğundan, yanıt izin verilir.
6. Ön uç alt ağını gelen kuralı işleme başlar:
   1. Geçerli NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği gelen.
   2. Trafiğe izin verilmesi alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı bu trafiğe izin verir.
7. IIS01 dosyayı alır.

#### <a name="denied-web-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Web
Web sunucusu IIS01 ve güvenlik duvarı aynı bulut hizmetinde olduğu için aynı genel kullanıma yönelik IP adresi paylaşırlar. Bu nedenle tüm HTTP trafiğini güvenlik duvarı yönlendirilir. İstek başarıyla hizmet, ancak doğrudan web sunucusuna devam edemezsiniz. Bu, güvenlik duvarı üzerinden ilk tasarlandığı gibi da geçirir. Bu bölümde trafik akışı için ilk senaryoda bakın.

#### <a name="denied-web-to-backend-server"></a>(Reddedildi) Arka uç sunucusuna Web
1. Bir dosya çubuğunda AppVM01 BackEnd001.CloudApp.Net hizmet aracılığıyla erişmek bir internet kullanıcı çalışır.
2. Dosya Paylaşımı için açık uç nokta olmadığından bu bulut hizmeti başarılı olmaz ve tarafından sunucuya ulaşmak olmaz.
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kuralı 5 (sanal ağdan sanal ağa internet) trafiği engeller.

#### <a name="denied-web-dns-lookup-on-the-dns-server"></a>(Reddedildi) Web DNS sunucusuna DNS araması
1. DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını aramak bir internet kullanıcı çalışır.
2. Uç nokta için DNS açık olduğundan, bu bulut hizmeti başarılı olmaz ve tarafından sunucuya ulaşmak olmaz.
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kuralı 5 (sanal ağdan sanal ağa internet) trafiği engeller. (Kural 1 [DNS] iki nedenden dolayı geçerli değildir. İlk olarak, kaynak adresi internet ve yalnızca yerel sanal ağ kaynağı olduğunda bu kural uygular. Hiçbir zaman trafiği engelleyen ikinci olarak, bu kural bir izin verme kuralı olduğundan.)

#### <a name="denied-web-to-sql-access-through-the-firewall"></a>(Reddedildi) Web güvenlik duvarı üzerinden SQL erişimi
1. İnternet kullanıcı FrontEnd001.CloudApp.Net (internet'e yönelik bulut hizmeti) SQL verileri ister.
2. SQL için açık uç nokta olmadığından bu bulut hizmeti başarılı olmaz ve güvenlik duvarı ulaşın olmaz.
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) geçerli değildir. Sonraki kuralın taşıyın.
   2. NSG kuralı 2 (RDP) geçerli değildir. Sonraki kuralın taşıyın.
   3. NSG kuralı 3 (internet için Güvenlik Duvarı) uygulayın. Trafiğine izin verin. Kuralı işlemeyi durdur.
4. Trafik (10.0.1.4) Güvenlik Duvarı'nın iç IP adresi denk gelir.
5. Güvenlik Duvarı için SQL iletme kuralları yok ve trafiği bırakır.

## <a name="conclusion"></a>Sonuç
Bu örnek, bir güvenlik duvarı ile uygulamanızı korumak ve arka uç alt ağından gelen trafiği yalıtmak için görece basit bir yol gösterir.

Güvenlik sınırları daha fazla örnek ve ağ genel bir bakış bulabilirsiniz [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="full-script-and-network-config"></a>Tam komut ve ağ yapılandırma
Bir PowerShell komut dosyasında tam komut dosyasını kaydedin. Ağ yapılandırma betiği NetworkConf2.xml adlı bir dosyaya kaydedin.
Kullanıcı tanımlı-değişkenleri gerektiği gibi değiştirin. Betiği çalıştırın ve ardından bu makalenin "güvenlik duvarı kuralları" bölümünde yer alan yönergeleri izleyin.

#### <a name="full-script"></a>Tam betik
Kullanıcı tanımlı değişkenleri esas alarak, bu betik, aşağıdaki adımları tamamlayacaksınız:

1. Bir Azure aboneliğine bağlanın.
2. Depolama hesabı oluşturma.
3. Bir sanal ağ ve iki alt ağ yapılandırma dosyasında tanımlanan oluşturun.
4. Dört Windows Server sanal makineleri oluşturun.
5. NSG yapılandırın. Yapılandırma adımları gerçekleştirir:
   * Bir NSG oluşturur.
   * NSG kuralları ile doldurur.
   * NSG, uygun bir alt ağa bağlar.

Bu PowerShell Betiği, bir İnternet'e bağlı bir bilgisayara veya sunucuya yerel olarak çalıştırmalısınız.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırdığınızda, uyarılar ve diğer bilgi iletilerini PowerShell'de görünebilir. Kırmızı hata iletileri hakkında önceliğiniz olması yeterlidir.
> 
> 

```powershell
    <# 
     .SYNOPSIS
      Example of a perimeter network and Network Security Groups in an isolated network. (Azure only. No hybrid connections.)

     .DESCRIPTION
      This script will build out a sample perimeter network setup containing:
       - A default storage account for VM disks.
       - Two new cloud services.
       - Two subnets (the FrontEnd and BackEnd subnets).
       - A network virtual appliance (NVA): a Barracuda NextGen Firewall.
       - One server on the FrontEnd subnet (plus the NVA on the FrontEnd subnet).
       - Three servers on the BackEnd subnet.
       - Network Security Groups to allow/deny traffic patterns as declared.

      Before running the script, ensure the network configuration file is created in
      the directory referenced by the $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Security requirements are different for each use case and can be addressed in many ways. Be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that you can use, but it should not be used for all scenarios. You
      are responsible for assessing your security needs and the appropriate protections
      and then effectively implementing those protections.

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

    # User-Defined Global Variables
      # These should be changed to reflect your subscription and services.
      # Invalid options will fail in the validation section.

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

    # User-Defined VM-Specific Config
        # To ensure proper NSG rule creation later in this script:
        #       - The web server must be VM 1.
        #       - The AppVM1 server must be VM 2.
        #       - The DNS server must be VM 4.
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG rule IP addresses
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
    # No user-defined variables or   #
    # configuration past this point #
    # ----------------------------- #

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

      # Create storage account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

      # Update subscription pointer to new storage account
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

    # Create virtual network
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

    # Create services
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
                # Set up all the endpoints we'll need once we're up and running.
                # Note: Web traffic goes through the firewall, so we'll need to set up an HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: An SSH endpoint is automatically created on port 22 when the appliance is created.
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

      # Add NSG rules
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

        # Assign the NSG to the subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-Script Manual Configuration
      # Configure firewall
      # Install test web app (run post-build script on the IIS server)
      # Install back-end resources (run post-build script on AppVM01)
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
Güncelleştirilmiş konumlarla bu XML dosyasını kaydedin ve daha sonra bu dosyayı önceki komut $NetworkConfigFile değişkeninde bir bağlantı ekleyin.

```xml
    <NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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
Bu ve diğer çevre ağ örnekleri için örnek uygulamayı yüklemek istiyorsanız, bkz. [örnek uygulama komut dosyası][SampleApp].

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Gelen NSG ile DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Hedef NAT simgesi"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Güvenlik Duvarı Kuralı"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Güvenlik duvarı kuralını etkinleştirme"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
