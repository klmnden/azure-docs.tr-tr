---
title: DMZ örnek – yapı ağ güvenlik duvarı, UDR ve NSG ile korunacak DMZ | Microsoft Docs
description: Bir güvenlik duvarı ile DMZ derleme, kullanıcı tanımlı yönlendirme (UDR) ve ağ güvenlik grupları (NSG)
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: dc01ccfb-27b0-4887-8f0b-2792f770ffff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2016
ms.author: jonor;sivae
ms.openlocfilehash: fdb3c5cbd3acee90386352c6f180a71aa81f54fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23885248"
---
# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Örnek 3 – bir çevre ağ güvenlik duvarı, UDR ve NSG ile korumak için derleme
[Güvenlik sınırı en iyi yöntemler sayfasına dön][HOME]

Bu örnek, bir güvenlik duvarı, dört windows sunucuları, kullanıcı tanımlı yönlendirme, IP iletimi ve ağ güvenlik grupları ile DMZ oluşturur. Bu da her her adımın daha derin bir anlayış sağlamak için ilgili komutları yol gösterir. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryosu bölümü olan adım adım çevre savunma katmanlar arasında nasıl trafiği devam eder. Son olarak, içindeki başvuruların sınamak ve çeşitli senaryolarıyla denemeler için bu ortamı oluşturmak için yönerge ve tam bir kod bölümüdür. 

![Çift yönlü DMZ NVA, NSG ve UDR][1]

## <a name="environment-setup"></a>Ortam Kurulumu
Bu örnekte, aşağıdakileri içeren bir abonelik vardır:

* Üç bulut hizmetlerini: "SecSvc001", "FrontEnd001" ve "BackEnd001"
* Bir sanal ağ "CorpNetwork" üç alt ağ ile: "SecNet", "Ön uç" ve "Arka uç"
* Bu örnekte bir güvenlik duvarı, bir ağ sanal gereç SecNet alt ağına bağlı
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama geri temsil eden iki windows sunucuları sunucuları ("AppVM01", "AppVM02") end
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu

Başvurular bölümündeki yukarıda açıklanan ortamı çoğunu oluşturacak bir PowerShell komut dosyası yok. VM'ler ve sanal ağlar, derleme örnek komut dosyası tarafından yapılır rağmen değil açıklanmıştır bu belgede ayrıntılı.

Ortamı oluşturmak için:

1. References bölümünde bulunan ağ yapılandırma xml dosyasını kaydedin (adlar, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilir)
2. Kullanıcı değişkenleri karşı (Abonelikleri, hizmet adlarını, vb.) çalıştırılacak komut dosyasıdır ortamıyla eşleşecek şekilde güncelleştirin
3. PowerShell komut dosyası yürütme

**Not**: PowerShell Betiği miktarlara bölge ağ yapılandırması xml dosyasında miktarlara bölge ile eşleşmelidir.

Komut dosyası başarıyla çalıştıktan sonra aşağıdaki sonrası betik adımlar izlenebilir:

1. Güvenlik duvarı kuralları ayarlamak, bu başlıklı bölümde aşağıda ele alınmıştır: güvenlik duvarı kuralı açıklaması.
2. İsteğe bağlı olarak web sunucusu ve uygulama sunucusu bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile ayarlamak için iki komut dosyası başvuruları bölümünde bulunur.

Komut dosyası kuralları tamamlanması gerekir Güvenlik Duvarı'nı başarıyla çalıştıktan sonra bu başlıklı bölümde ele alınmıştır: güvenlik duvarı kuralları.

## <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Varsayılan olarak, aşağıdaki sistem yolları gibi tanımlanır:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Her zaman VNETLocal vnet'in (IE, ağdan Vnet'e her belirli VNet nasıl tanımlandığına bağlı olarak değişir), belirli bir ağ için tanımlanan adres ön ' dir. Kalan sistem yolları statik ve yukarıdaki varsayılan.

Öncelik, ettirilmesi yollar en uzun ön ek eşleşmesi (LPM) yöntemiyle işlenir, böylece en özel bir rota tablosunda belirtilen hedef adresi için geçerli olur.

Bu nedenle, yerel ağ (10.0.0.0/16) trafiği (örneğin sunucuya DNS01, 10.0.2.4) 10.0.0.0/16 rota nedeniyle hedefine VNet arasında yönlendirilecektir. Diğer bir deyişle, 10.0.2.4 için 10.0.0.0/16 en özel bir rota 10.0.0.0/8 ve 0.0.0.0/0 de geçerli olabilir, ancak daha az olduğundan belirli kullanıcılar bu trafiği etkilemez olsa bile yoldur. Böylece 10.0.2.4 trafiği, yerel vnet'in sonraki atlama sahip ve yalnızca hedef yol.

Trafik tasarlanmıştır 10.1.1.1 için örneğin, 10.0.0.0/16 rota uygularsanız olmayacaktır, ancak bu 10.0.0.0/8 en belirgin olacaktır ve bu trafiği olacaktır ("siyah holed") sonraki atlama Null olduğundan bırakıldı. 

Hedef herhangi biri Null ön ekler veya VNETLocal önekleri uygulanmadı sonra en az belirli izleyin yönlendirmek, 0.0.0.0/0 ve Internet'e sonraki atlama olarak ve bu nedenle Azure'nın Internet kenar çıkışı yönlendirilmesi.

Rota tablosunda iki aynı önekleri varsa, yolları "kaynak" özniteliğine dayanarak tercih sırasına göre şudur:

1. "Değerinin VirtualAppliance" el ile tabloya eklenen kullanıcı tanımlanan rota =
2. "VPNGateway" dinamik rota (BGP), karma ağlarla kullanıldığında = dinamik Protokolü otomatik olarak eşlenmiş ağ değişiklikleri yansıtır gibi dinamik ağ protokolü tarafından eklenen, bu yollar zaman içinde değişebilir
3. "Varsayılan" rota yukarıdaki tabloda gösterildiği gibi sistem yolları, yerel VNet ve statik girdilerini =.

> [!NOTE]
> Giden ve gelen zorlamak için VPN ağ geçitleri trafiği bir ağ sanal gereç (NVA) yönlendirilmeden şirketler arası ve ExpressRoute ile kullanıcı tanımlı yönlendirme (UDR) artık kullanabilirsiniz.
> 
> 

#### <a name="creating-the-local-routes"></a>Yerel yollar oluşturma
Bu örnekte, iki yönlendirme tablolarını gereklidir, her biri ön uç ve arka uç alt ağlar için. Her tablo için belirli alt uygun statik yollar ile yüklenir. Bu örneğin amacı doğrultusunda, her tablo üç yol vardır:

1. Güvenlik Duvarı atlamak yerel alt ağ trafiğine izin vermek için yerel alt ağ trafiği hiçbir sonraki atlama ile tanımlanan
2. Bir sonraki güvenlik duvarı olarak tanımlanan atlama ile sanal ağ trafiği, bu doğrudan yönlendirmek yerel VNet trafiğe izin veren varsayılan kuralı geçersiz kılar
3. Bir sonraki Güvenlik Duvarı'nı tanımlanan atlama ile tüm kalan trafiği (0/0)

Yönlendirme tabloları oluşturulduktan sonra bunlar için alt ağlarını bağlıdır. Ön uç alt ağ için bir kez oluşturulur ve alt ağına bağlı yönlendirme tablosu aşağıdaki gibi görünmelidir:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Yol tablosu oluşturmak, bir kullanıcı tanımlı yolu ekleyin ve ardından yol tablosu bir alt ağa bağlamak için kullanılan bu örnek için aşağıdaki komutları (Not; dolar işareti ile başlayan altındaki öğeleri (örn: $BESubnet) kullanıcı tanımlı değişkenler başvuru bölümünde, bu belgenin betikten):

1. Temel yönlendirme tablosu oluşturulması gerekir. Bu kod parçacığında arka uç alt ağ için tablo oluşturulmasını gösterir. Komut dosyasında karşılık gelen bir tablo için ön uç alt da oluşturulur.
   
     AzureRouteTable yeni-ad $BERouteTableName '
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Yol tablosu oluşturulduktan sonra belirli kullanıcı tanımlı yollar eklenebilir. Bu konuda kırpılmış, tüm trafik (0.0.0.0/0) (bir değişken $VMIP [0] komut dosyasındaki sanal gereç oluşturulduğunda atanan IP adresi geçirmek için kullanılır) sanal gereç aracılığıyla yönlendirilir. Komut dosyasında karşılık gelen kuralı da ön uç tabloda oluşturulur.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. Yukarıdaki rota girişi, doğrudan hedef ve ağ sanal Gerece yönlendirmek için sanal ağ içindeki trafiği izin varsayılan "0.0.0.0/0" yol, ancak hala var olan varsayılan 10.0.0.0/16 kuralı geçersiz kılar. Doğru Bu davranış izleme kuralı eklenmelidir.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Bu noktada yapılacak bir seçenek yoktur. Yukarıdaki iki yollar değerlendirmesi, tek bir alt ağ içindeki bile trafiği için Güvenlik Duvarı'nda tüm trafik yönlendirilecek. Yerel Güvenlik Duvarı müdahalesi olmadan üçüncü yönlendirmek için bir alt ağ içindeki trafiği izin vermek için çok özel kural eklenebilir ancak bu, istenebilir. Bu yol var bildiren herhangi bir adresi destine yerel alt ağ yalnızca için rota doğrudan (NextHopType VNETLocal =).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Son olarak, oluşturulur ve kullanıcı tanımlı yollar ile doldurulur yönlendirme tablosu ile tablonun şimdi bir alt ağa bağlı olmalıdır. Komut dosyasında, ön uç yol tablosu ayrıca ön uç alt ağına bağlı. Burada, arka uç alt ağ için bağlama komut verilmiştir.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName '
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP İletimi
Bir yardımcı UDR için özelliktir IP iletimi. Gereci özellikle trafiği alabilmesine ve bu trafiğin ultimate hedefine iletmek imkan tanıyan bir sanal gereç bir ayar budur.

AppVM01 trafiğinden DNS01 sunucusuna bir istek yapıyorsa örnek olarak, UDR bu Güvenlik Duvarı'na rota. Etkin IP iletimi ile DNS01 hedef (10.0.2.4) trafiği (10.0.0.4) Gereci tarafından kabul ve olması ultimate hedefine (10.0.2.4) iletilir. Yol tablosu sonraki atlama olarak bir güvenlik duvarına sahip olsa bile Güvenlik Duvarı'nı etkin IP iletimi, trafiği Gereci tarafından kabul değil. 

> [!IMPORTANT]
> Kullanıcı tanımlı yönlendirme ile birlikte IP iletimi etkinleştirmeyi unutmayın önemlidir.
> 
> 

IP iletimi ayarlama tek bir komut ve VM oluşturma zamanında yapılabilir. Bu örnek akış için kod parçacığında komut dosyasının sonuna doğru olduğundan ve UDR komutları ile gruplandırılır:

1. Bu durumda, bir sanal gereç, güvenlik duvarı olan VM örneği çağırın ve IP iletimini etkinleştirme (Not; dolar işareti kırmızı başlayarak herhangi bir öğeye (örn: $VMName[0]), bu belgenin başvuru bölümdeki komut dosyasından bir kullanıcı tanımlı değişkendir. Köşeli ayraçlar [0], sıfır VM'ler değişiklik yapmadan çalışmak örnek komut dosyası için bir dizi ilk VM'yi temsil eder, Güvenlik Duvarı'nı (VM 0) ilk VM olması gerekir):
   
     Get-AzureVM-$ServiceName [0] [0] $VMName - ServiceName adı | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu yerleşik ve tek bir kural ile yüklenir. Bu grup, ardından yalnızca ön uç ve arka uç alt ağlara (SecNet değil) bağlıdır. Aşağıdaki kural oluşturulmakta olan bildirimli olarak:

1. Tüm trafiği (tüm bağlantı noktaları) Internet'ten (tüm alt ağlar) tüm sanal ağa reddedildi

Nsg'ler Bu örnekte kullanılan cihazın ana amacı el ile yetersizliğini karşı savunma ikincil bir katmanı olarak olsa da. Ön uç ya da internet'ten gelen tüm trafiği engellemek istiyoruz veya arka uç alt ağlar, trafiği yalnızca SecNet alt ağ güvenlik duvarı aracılığıyla akış (ve ardından IF, alt ağlar ön uç veya arka uç açın gerekli). Ayrıca, bir yerde, UDR kurallarla ön uç veya arka uç alt yaptı tüm trafik çıkışı Güvenlik Duvarı'nda (UDR) sayesinde yönlendirilmesi. Güvenlik Duvarı'nı bu asimetrik bir akış halinde görür ve giden trafik bırakma. Bu nedenle ön uç ve arka uç alt ağ korumaya güvenlik üç katmanı vardır; 1) açık uç nokta yok FrontEnd001 ve BackEnd001 bulut hizmetlerini, 2) Nsg'ler trafiğini Internet'ten, 3) güvenlik duvarı bırakma asimetrik trafiği engelleme.

Bir ilginç Bu örnekte ağ güvenlik grubu ile ilgili güvenlik alt ağ içerir tüm sanal ağ Internet trafiği engellemek için olan yalnızca bir kural, aşağıda gösterilen içerdiğini noktasıdır. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

NSG yalnızca ön uç ve arka uç alt ağa bağlı olduğundan, kuralın trafiğinde ancak işlenen değil gelen güvenlik alt ağ için. Sonuç olarak, NSG, hiçbir zaman güvenlik alt ağına bağlı olduğundan NSG kural VNet üzerinde herhangi bir adresi yok Internet trafiğini diyor olsa bile, trafik için güvenlik alt ağ akar.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Güvenlik Duvarı'nı kuralları iletme oluşturulması gerekir. Güvenlik duvarı engelleme veya iletme tüm gelen, giden ve içi-VNet trafiği olduğundan birçok güvenlik duvarı kuralları gerekir. Ayrıca, tüm gelen trafiği güvenlik hizmeti genel IP adresine (farklı bağlantı noktalarını), güvenlik duvarı tarafından işlenmek üzere karşılaşır. Alt ağlar ayarlamadan önce mantıksal akış diyagramı için en iyi uygulamadır ve daha sonra önlemek için güvenlik duvarı kuralları rework. Aşağıdaki şekilde, bu örnek için güvenlik duvarı kurallarının mantıksal bir görünümdür:

![Güvenlik duvarı kurallarını mantıksal görünümü][2]

> [!NOTE]
> Ağ sanal kullanılan Gereci bağlı olarak, yönetim bağlantı noktalarını değişir. Barracuda NextGen Firewall başvurulan Bu örnekte, bağlantı noktası 22, 801 ve 807 kullanır. Lütfen kullanılan aygıt yönetimi için kullanılan tam bağlantı noktalarını bulmak için Gereci satıcı belgelerine bakın.
> 
> 

### <a name="logical-rule-description"></a>Mantıksal kural açıklaması
Mantıksal Yukarıdaki diyagramda, güvenlik alt ağ güvenlik duvarı o alt ağdaki yalnızca kaynak ve bu diyagramda, güvenlik duvarı kuralları ve nasıl bunlar mantıksal olarak izin verme veya trafiği akışı ve gerçek yönlendirilmiş yol değil reddetme gösteren beri gösterilmez. Ayrıca, RDP trafiğine için daha yüksek seçilen dış bağlantı noktalarını bağlantı noktaları (8014 – 8026) aralıklı ve biraz daha kolay okunabilir olması için yerel IP adresi son iki sekizlisinin hizalamak için seçildi (örneğin yerel sunucu 10.0.1.4 dış bağlantı noktasıyla ilişkili adresidir 8014), ancak daha yüksek herhangi çakışmayan bağlantı noktaları kullanılabilir.

Bu örnek için 7 tür kuralların ihtiyacımız, bu kural türleri aşağıda açıklanmıştır:

* Dış kuralları (için gelen trafiği):
  1. Güvenlik Duvarı Yönetimi kuralı: Bu uygulama yeniden yönlendirme kuralı ağ sanal gereç yönetim bağlantı noktalarına geçirmek trafiğine izin verir.
  2. RDP kuralları (her windows server için): Bu dört kurallar (her sunucu için bir tane) RDP aracılığıyla tek sunucuların Yönetimi izin verir. Bu ayrıca ağ sanal kullanılan gereç özelliklerine bağlı olarak tek bir kural içine paketlenmiş.
  3. Uygulama trafiği kuralları: Ön uç web trafiği için ilk ve ikinci arka uç trafiği (örn. web sunucusu için veri katmanı) için iki uygulama trafiği kuralları vardır. Bu kurallar yapılandırmasını (burada, sunucularınızın yerleştirilir) ağ mimarisi bağımlı ve trafik akışları (hangi yönde trafik akışına ve hangi bağlantı noktaları kullanılır).
     * İlk kural, uygulama sunucusuna ulaşmaya gerçek uygulama trafiğine izin verir. Diğer kurallardan izin verirken, güvenlik, yönetim, vb. için uygulama ne harici kullanıcılar ya da hizmetleri uygulamaları erişmesine izin kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde tek bir web sunucusu yoktur, böylece bir tek uygulama güvenlik duvarını gelen trafiği web sunucuları iç IP adresi için harici IP yönlendirilecek. Yeniden yönlendirilen trafiği oturum NAT iç sunucuya sahip.
     * İkinci uygulama trafik kuralı AppVM01 sunucu (ancak değil AppVM02) için anlaşmak Web sunucusu herhangi bir bağlantı izin vermek için arka uç kuralıdır.
* İç kurallardan (içi-VNet trafiği)
  1. Internet kuralı giden: Bu kural seçilen ağlara geçirmek için herhangi bir ağ trafiğinden izin verir. Bu genellikle varsayılan bir kural zaten güvenlik duvarında ancak devre dışı durumdayken kuralıdır. Bu kural, bu örnek için etkinleştirilmelidir.
  2. DNS kuralı: Bu kural DNS sunucusuna iletmek yalnızca DNS (bağlantı noktası 53) trafiği sağlar. Çoğu trafiğinden ön uç arka uç için engellenen bu ortam için bu kural tüm yerel alt ağ üzerinden DNS özellikle sağlar.
  3. Alt ağ için alt kuralı: Bu kural, ön uç alt (ancak tersi) herhangi bir sunucuya bağlanmak için arka uç alt ağda herhangi bir sunucu izin vermektir.
* Yedek operatördür kuralı (yukarıdaki birini karşılamıyor trafiği):
  1. Tüm trafik kuralı Reddet: Bu her zaman son kural (bakımından öncelik) olmalıdır ve trafik akışlarına bu nedenle bu kural tarafından bırakılacak önceki kurallardan herhangi birinin eşleştirmek başarısız olur. Bu varsayılan bir kural ve genellikle etkin, değişikliğe genellikle gereklidir.

> [!TIP]
> İkinci uygulama trafik kuralı üzerinde herhangi bir bağlantı için en belirli bağlantı noktası gerçek bir senaryoda bu örneğin kolay izin verilir ve adres aralıkları, bu kural, saldırı yüzeyini azaltmak için kullanılmalıdır.
> 
> 

<br />

> [!IMPORTANT]
> Yukarıdaki kurallarda oluşturulduktan sonra trafiğine izin verilen veya istendiği gibi reddedilen emin olmak için her bir kural önceliğini gözden geçirilmesi önemlidir. Bu örnekte, öncelik sırasına göre kurallardır. Güvenlik Duvarı'nı yanlış sıralı kuralları nedeniyle dışında kilitlenmesi kolaydır. En azından, Yönetim güvenlik duvarı kendisi için her zaman mutlak en yüksek öncelik kuralı olduğundan emin olun.
> 
> 

### <a name="rule-prerequisites"></a>Kural önkoşulları
Bir Güvenlik Duvarı'nı çalıştıran sanal makine için önkoşuldur ortak uç noktaları. Trafiğini işlemek Güvenlik Duvarı için uygun ortak bitiş noktalarının açık olması gerekir. Bu örnekte trafiğin üç tür vardır; Windows sunucuları ve 3) uygulama trafiğini denetlemek için RDP trafiğine 1) yönetim trafiğinin Güvenlik Duvarı'nı Denetim ve güvenlik duvarı kuralları, 2). Bu üç sütunlar üst trafik türleri üzerinde güvenlik duvarı kuralları mantıksal görünümünü yarısı'dır.

> [!IMPORTANT]
> Burada anahtar takeway unutmayın vermektir **tüm** trafiğinin güvenlik duvarı üzerinden gelen. Bu nedenle IIS01 sunucuya Uzak Masaüstü için ön uç bulut hizmeti ve ön uç alt olmasına rağmen bu sunucuya erişmek için biz RDP için güvenlik duvarı bağlantı noktası 8014 gerekir ve RDP isteği IIS01 RDP Por için dahili olarak yönlendirmek Güvenlik Duvarı'nı izin ver t. Azure portal'ın "Bağlan" düğmesi olduğundan IIS01 için doğrudan bir RDP yol yok (portal görebilirsiniz uzaklığa kadar) çalışmaz. Bu, Internet'ten gelen tüm bağlantıları güvenlik hizmeti ve bir bağlantı noktası, örneğin secscv001.cloudapp.net:xxxx (başvuru harici bağlantı noktası, iç IP ve bağlantı noktası eşlemesi için yukarıdaki diyagramda) anlamına gelir.
> 
> 

Bir uç nokta ya da VM oluşturma zamanında açılabilir veya yapı örnek komut dosyasında yapılan ve aşağıda bu kod parçacığında gösterildiği gibi post (Not; herhangi bir dolar işareti öğesi başlayarak (örn: $VMName[$i]) olan bir kullanıcı tanımlı değişken başvuru ntüle komut Bu belgenin n. Köşeli ayraçlar "$i" [$i] VM'ler dizisi belirli bir VM'de dizi sayısını temsil eder):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Değişkenleri, ancak uç noktalar kullanımını, nedeniyle değil açıkça burada gösterilen ancak **yalnızca** güvenlik bulut Hizmeti'ndeki açılır. Tüm gelen trafiği yönetildiğinden emin olmak için budur (yönlendirilmiş NAT bırakılan) güvenlik duvarı tarafından.

Bir yönetim İstemcisi Güvenlik Duvarı'nı yönetmek ve gerekli yapılandırmaları oluşturmak için bir Bilgisayara yüklü gerekir. Belge, Güvenlik Duvarı (veya diğer NVA) satıcı cihaz yönetme konusuna bakın. Bu bölümde ve sonraki bölümde, güvenlik duvarı kuralları oluşturma, geri kalan güvenlik duvarı yapılandırmasını kendisini satıcılar Yönetimi istemcisi (yani Azure portal veya PowerShell) aracılığıyla anlatmaktadır.

İstemci Yükleme ve bu örnekte kullanılan Barracuda bağlanmak için yönergeler şurada bulunabilir: [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin)

Güvenlik Duvarı üzerine ancak güvenlik duvarı kuralları oluşturmadan önce oturum sonra kuralları daha kolay oluşturma yapabilirsiniz iki önkoşul nesne sınıfları vardır; Ağ & hizmeti nesneleri.

Bu örnekte, üç adlandırılmış ağ nesneleri tanımlı (ön uç alt ağ ve DNS sunucusunun IP adresi için de bir ağ nesnesi Backend alt ağı için bir) olmalıdır. Adlandırılmış bir ağ oluşturmak için; Barracuda NG yönetici istemci panodan başlayarak, yapılandırma sekmesine gidin, işletimsel yapılandırma bölümünde Ruleset, ardından "Ağlar" altında güvenlik duvarı nesneleri menüsünü tıklatın, ardından yeni Düzenle ağları menüsünde tıklatın. Ağ nesnesi artık, adını ve önekini ekleyerek oluşturulabilir:

![Bir ön uç ağ nesnesi oluşturun][3]

Bu ön uç alt ağ için bir adlandırılmış ağ oluşturur, arka uç alt ağ için de benzer bir nesne oluşturulmalıdır. Şimdi alt ağlar güvenlik duvarı kurallarında ada göre daha kolay başvurulabilir.

DNS sunucu nesnesi:

![Bir DNS sunucusu nesnesi oluşturun][4]

Bu tek IP adresi başvurusu DNS kuralı belgenin devamındaki kullanılmadan.

İkinci önkoşul nesneleri Hizmetleri nesneleridir. Bunlar, her sunucu için RDP bağlantı noktaları temsil eder. Varolan RDP hizmeti nesnesi varsayılan RDP bağlantı noktasına bağlı olduğundan, 3389, dış bağlantı noktalarını (8014-8026) trafiğine izin verecek şekilde yeni hizmetler oluşturulabilir. Yeni bağlantı noktaları, varolan RDP hizmeti eklenemedi, ancak tanıtım kolaylığı için her sunucu için tek bir kuralı oluşturulabilir. Bir sunucu için yeni bir RDP kuralı oluşturmak için; Barracuda NG yönetici istemci panodan başlayarak, yapılandırma sekmesine gidin, işletimsel yapılandırma bölümünde Ruleset, tıklayın ardından "Hizmetler" altında güvenlik duvarı nesneleri menüsünü tıklatın, hizmetlerin listesini kaydırın ve "RDP" hizmetini seçin. Sağ tıklayın ve kopyalayın, sonra sağ tıklatın ve Yapıştır seçin. Düzenlenebilir bir RDP Copy1 hizmet nesnesi artık yok. RDP Copy1 sağ tıklatın ve Düzenle, Düzenle hizmet penceresi aşağıda gösterildiği gibi kadar pop nesnesi seçin:

![Varsayılan RDP kuralı kopyası][5]

Değerleri, belirli bir sunucu için RDP hizmeti göstermek için sonra düzenlenebilir. AppVM01 için yukarıdaki varsayılan RDP kural yeni hizmet adı, açıklama ve dış RDP Şekil 8 diyagramda kullanılan bağlantı noktası yansıtacak şekilde değiştirilmesi gerekir (Not: bağlantı noktalarını belirli bu sunucu için kullanılan dış bağlantı noktası 3389 RDP varsayılandan değiştirilir söz konusu olduğunda AppVM01 8025 dış bağlantı noktasıdır) değiştirilmiş hizmeti aşağıda verilmiştir:

![AppVM01 kuralı][6]

Geri kalan sunucular için RDP hizmetleri oluşturmak için bu işlemin yinelenmesi gerekir; AppVM02, DNS01 ve IIS01. Bu hizmetler oluşturma kuralı oluşturmayı daha basit ve daha belirgin sonraki bölümde hale getirir.

> [!NOTE]
> Bir RDP Hizmeti Güvenlik Duvarı için iki nedenden dolayı gerekli değildir; 1) ilk VM Güvenlik Duvarı olduğundan Linux tabanlı görüntü SSH tanesi kullanılacak bağlantı noktası 22 RDP ve 2) bağlantı noktası 22 yerine VM yönetimi ve diğer yönetim bağlantı noktalarına iki Yönetim bağlantısı için aşağıda açıklanan ilk yönetim kuraldaki izin verilir.
> 
> 

### <a name="firewall-rules-creation"></a>Güvenlik duvarı kuralları oluşturma
Bu örnekte kullanılan güvenlik duvarı kuralları üç tür vardır, hepsinin ayrı simgeler vardır:

Uygulama yeniden yönlendirme kuralı: ![uygulama yeniden yönlendirme simgesi][7]

Hedef NAT kuralı: ![hedef NAT simgesi][8]

Geçişi kuralı: ![geçirmek simgesi][9]

Bu kural hakkında daha fazla bilgi Barracuda web sitesinde bulunabilir.

Aşağıdaki kuralları oluşturun (veya var olan varsayılan kuralları doğrulamak için), Barracuda NG yönetici istemci panodan başlangıç yapılandırma sekmesine gidin, işletimsel yapılandırmasında bölüm Ruleset tıklayın. Adlı bir kılavuz "Ana kuralları" varolan etkin ve devre dışı bırakılan kuralları bu Güvenlik Duvarı'nı gösterir. Bu kılavuz sağ üst köşesindeki olan küçük, yeşil "+" düğmesini tıklatın, yeni bir kural oluşturmak için burayı tıklatın (Not: güvenlik duvarını "değişiklikleri kilitlenebilir", görürseniz bir düğme "Kilit" olarak işaretlenmiş ve oluşturmak veya kuralları düzenlemek için "kural kümesi kilidini açmak için" Bu düğmeye tıklayın sorunu yaşıyor ve  düzenlenmesine izin verilir). Varolan bir kuralı düzenlemek istiyorsanız, o kural seçin, sağ tıklayın ve kuralını Düzenle seçin.

Kurallarınızı oluşturulan ve/veya değiştirilmiş, bunlar Güvenlik Duvarı'na gönderilen ve gerekir etkinleştirildiğinden, bu değil yapıldığında kural değişiklikler etkili olmaz. Anında iletme ve etkinleştirme sürecini ayrıntıları kural açıklamaları açıklanmıştır.

Bu örnek tamamlamak için gereken her bir kural ayrıntılarını aşağıda açıklanmıştır:

* **Güvenlik Duvarı yönetimi kural**: Bu uygulama yeniden yönlendirme kuralı trafiğin Bu örnekte ağ sanal gerecin yönetim noktalarına Barracuda NextGen Firewall geçmesine izin verir. Yönetim bağlantı noktalarını 801, 807 ve isteğe bağlı olarak 22 ' dir. İç ve dış bağlantı noktaları (yani hiçbir bağlantı noktası çevirisi) aynıdır. Bu kural, Kurulum MGMT erişim, varsayılan bir kural ve varsayılan (olarak Barracuda NextGen Firewall sürüm 6.1) etkinleştirilmiş.
  
    ![Güvenlik Duvarı Yönetimi kuralı][10]

> [!TIP]
> Bu kuralın kaynak adres alanı, olan Yönetim IP adres aralıklarını biliniyorsa, bu kapsamı azaltma ayrıca yönetim bağlantı noktalarına saldırı yüzeyini azaltmak.
> 
> 

* **RDP kuralları**: Bu hedef NAT kuralları izin ver RDP aracılığıyla tek sunucuların yönetimi.
  Bu kuralı oluşturmak için gereken dört önemli alanlar şunlardır:
  
  1. RDP yerden, başvuru "Herhangi bir" izin vermek için kaynak – kaynak alanında kullanılır.
  2. Dış bağlantı noktalarını sunucuları yerel IP adresi ve bağlantı noktası 3386 (varsayılan RDP bağlantı noktası) için yeniden yönlendirme, hizmeti – bu durumda "AppVM01 RDP" daha önce oluşturduğunuz uygun hizmet nesnesi kullanın. Bu belirli AppVM01 RDP erişim kuralıdır.
  3. Hedef – olmalıdır *Güvenlik Duvarı'nda yerel bağlantı noktası*, "DCHP 1 yerel IP" ya da statik IP kullanıyorsanız eth0. Sıra numarasını (eth0, eth1, vb.), ağ Gereci birden çok yerel arabirimi varsa, farklı olabilir. Bu güvenlik duvarı olduğundan göndermesinin giden bağlantı noktasıdır (alıcı bağlantı noktası ile aynı olabilir), gerçek yönlendirilmiş hedef hedef listesi alanıdır.
  4. Yeniden yönlendirme – bu bölümün sonunda bu trafiği yönlendirmek nereye sanal gereç bildirir. En basit yeniden yönlendirme IP ve bağlantı noktası (isteğe bağlı) hedef listesi alanında yerleştirmektir. Hiçbir bağlantı noktası kullanılıyorsa, hedef bağlantı noktası gelen istekte olacaktır (IE hiçbir çevirisi), bir bağlantı noktası bağlantı noktası belirlendiyse kullanılan de NAT yanı sıra IP adresi olur.
     
     ![Güvenlik Duvarı RDP kuralı][11]
     
     Dört RDP kuralları toplam oluşturulması gerekecektir: 
     
     | Kural Adı | Sunucu | Hizmet | Hedef listesi |
     | --- | --- | --- | --- |
     | RDP IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Kaynak ve hizmet alanların kapsamını daraltma saldırı yüzeyini azaltır. İşlevselliği sağlayacak en sınırlı kapsam kullanılmalıdır.
> 
> 

* **Uygulama trafik kurallarını**: ön uç web trafiği için ilk ve ikinci arka uç trafiği (örn. web sunucusu için veri katmanı) için iki uygulama trafiği kuralları vardır. Bu kurallar (burada, sunucularınızın yerleştirilir) ağ mimarisine göre değişir ve trafik akışları (hangi yönde trafik akışına ve hangi bağlantı noktaları kullanılır).
  
    Önce ele alınan web trafiği için ön uç kuralı gösterilmiştir:
  
    ![Güvenlik Duvarı Web kuralı][12]
  
    Bu hedef NAT kuralı uygulama sunucusuna ulaşmaya gerçek uygulama trafiğine izin verir. Diğer kurallardan izin verirken, güvenlik, yönetim, vb. için uygulama ne harici kullanıcılar ya da hizmetleri uygulamaları erişmesine izin kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde tek bir web sunucusu yok, bu nedenle tek uygulama güvenlik duvarını gelen trafiği web sunucuları iç IP adresi için harici IP yönlendirilecek.
  
    **Not**: hedef listesi alanına atanan bağlantı noktası yok, bu nedenle gelen bağlantı noktası 80 (veya seçili hizmeti için 443) web sunucusu yeniden yönlendirilmesini kullanılır. Web sunucusu farklı bir bağlantı noktasında dinleme yapıyorsanız, örneğin 8080 bağlantı noktası, 10.0.1.4:8080 de bağlantı noktası yönlendirmeye izin vermek için hedef liste alanı güncelleştirilemedi.
  
    Sonraki uygulama trafik kuralı AppVM01 sunucu (ancak değil AppVM02) için anlaşmak Web sunucusu herhangi bir hizmeti izin vermek için arka uç kuralı gösterilmiştir:
  
    ![Güvenlik Duvarı AppVM01 kuralı][13]
  
    Bu geçiş kuralı herhangi bir IIS sunucusuna AppVM01 ulaşmak için ön uç alt ağda verir (IP adresi 10.0.2.5) herhangi bir bağlantı üzerinde web uygulaması tarafından gerekli verilere erişmek için herhangi bir iletişim kuralı kullanarak.
  
    Bu ekran görüntüsü, bir "\<taşınmaya açık\>" 10.0.2.5 hedef olarak belirtmek için hedef alanındaki değer kullanılır. Bu şunlardan biri olabilir gösterildiği gibi veya açık adlı ağ (DNS sunucusu için Önkoşullar'da yapıldığı gibi) nesnesi. Bu toplayıcınızın yöntemi kullanılacak Güvenlik Duvarı'nın kadar yöneticisidir. Altına boş bir ilk satır 10.0.2.5 Explict Desitnation eklemek için çift \<taşınmaya açık\> ve açılır penceresinde adresini girin.
  
    Böylece bağlantı yöntemi "No SNAT" ayarlayabilirsiniz bu dahili trafiği olduğundan geçirmek bu kural, NAT gereklidir.
  
    **Not**: Bu kural kaynak ağında olan herhangi bir kaynak yalnızca olacaksa bir veya web sunucuları, bilinen belirli sayıda ön uç alt ağda, ağ nesnesi kaynak tüm ön uç alt ağa yerine tam bu IP adreslerine daha belirgin oluşturulabilir.

> [!TIP]
> Bu kural hizmet "Herhangi" örnek uygulama kullanması daha kolay hale getirmek için kullanır, bu da Icmpv4 izin verir (ping) tek bir kural. Ancak, önerilen bir uygulama değildir. Bağlantı noktalarını ve protokolleri ("Hizmetler") uygulama işlemi bu sınır saldırı yüzeyini küçültmek sağlayan en olası daraltıldığı.
> 
> 

<br />

> [!TIP]
> Bu kural kullanılan bir hedef açık başvuru gösterir, ancak tutarlı bir yaklaşım güvenlik duvarı yapılandırmasında kullanılmalıdır. Adlandırılmış ağ nesnesi boyunca daha kolay okunabilirlik ve desteklenebilirlik için kullanılması önerilir. Açık-taşınmaya burada yalnızca bir alternatif başvuru yöntemi göstermek için kullanılır ve (özellikle karmaşık yapılandırmalar için) genellikle önerilmez.
> 
> 

* **Internet kuralı giden**: Bu geçirmek kuralı izin ver seçili hedef ağlara geçirmek için herhangi bir kaynak ağ trafiği. Bu kural varsayılan bir kural zaten genellikle Barracuda NextGen Güvenlik Duvarı'nda olmakla birlikte, devre dışı durumda. Bu kurala sağ etkinleştirme kural komutu erişebilirsiniz. Burada gösterilen kural, bu kural kaynak özniteliği için bu belgenin önkoşul bölümüne başvurular olarak oluşturulan iki yerel alt ağlar eklemek için değiştirildi.
  
    ![Giden güvenlik duvarı kuralı][14]
* **DNS kuralı**: Bu geçirmek kural DNS sunucusuna iletmek yalnızca DNS (bağlantı noktası 53) trafiğine izin verir. Bu kural, çoğu trafiğinden ön uç arka uç için engellenen bu ortam için DNS özellikle sağlar.
  
    ![Güvenlik Duvarı DNS kuralı][15]
  
    **Not**: Bu ekran görüntüsü bağlantı yöntemi bulunur. Bu kural, iç IP adresi trafiği iç IP olduğundan, hiçbir NATing gerekli değildir, bu bağlantı yöntemi için bu geçişi kuralı "No SNAT" ayarlanır.
* **Alt ağ için alt kural**: Bu geçirmek etkinleştirildi ve ön uç alt ağda herhangi bir sunucuya bağlanmak için arka uç alt ağda herhangi bir sunucu izin vermek için değiştirilen varsayılan bir kural kuralıdır. Bu kural tüm iç trafiği olduğundan, bağlantı yöntemi için Hayır SNAT ayarlanabilir.
  
    ![Güvenlik Duvarı içi sanal ağ kuralı][16]
  
    **Not**: çift yönlü onay kutusu işaretli (veya, çoğu kurallarında işaretli değil), bu "tek yönlü" kural sağlar, bu kural için önemli budur, ön uç ağ ancak ters arka uç alt ağından bağlantı başlatılabilir. Bu onay kutusu işaretli değilse bu kural bizim mantıksal diyagramdan istenmediği çift yönlü trafiği etkinleştirir.
* **Reddetme tüm trafik kuralı**: Bu her zaman son bir kural (bakımından öncelik) olmalıdır ve trafik herhangi biriyle eşleşen başarısız akışlar, bu nedenle önceki onu bırakılacak bu kural tarafından kuralları. Bu varsayılan bir kural ve genellikle etkin, değişikliğe genellikle gereklidir. 
  
    ![Güvenlik duvarı kuralı Reddet][17]

> [!IMPORTANT]
> Yukarıdaki kurallarda oluşturulduktan sonra trafiğine izin verilen veya istendiği gibi reddedilen emin olmak için her bir kural önceliğini gözden geçirilmesi önemlidir. Bu örnekte, Barracuda Management istemcisi kurallarında iletme, ana kılavuzunda görünmesi gereken sırayla kurallardır.
> 
> 

## <a name="rule-activation"></a>Kural etkinleştirme
Mantığı diyagramı belirtimi değiştiren ruleset ile ruleset Güvenlik Duvarı'na karşıya ve sonra etkinleştirilmelidir.

![Güvenlik duvarı kuralını etkinleştirme][18]

Yönetim istemcisi üst sağ alt köşesindeki düğmeleri oluşan bir küme var. Değiştirilen kuralları Güvenlik Duvarı'na göndermek için "Değişiklikleri Gönder" düğmesini tıklatın, ardından "Etkinleştir" düğmesini tıklatın.

Güvenlik Duvarı ruleset etkinleştirme ile bu örnek ortamı yapı tamamlanır.

## <a name="traffic-scenarios"></a>Trafik senaryoları
> [!IMPORTANT]
> Bir anahtar takeway unutmayın vermektir **tüm** trafiğinin güvenlik duvarı üzerinden gelen. Bu nedenle IIS01 sunucuya Uzak Masaüstü için ön uç bulut hizmeti ve ön uç alt olmasına rağmen bu sunucuya erişmek için biz RDP için güvenlik duvarı bağlantı noktası 8014 gerekir ve RDP isteği IIS01 RDP Por için dahili olarak yönlendirmek Güvenlik Duvarı'nı izin ver t. Azure portal'ın "Bağlan" düğmesi olduğundan IIS01 için doğrudan bir RDP yol yok (portal görebilirsiniz uzaklığa kadar) çalışmaz. Bu, Internet'ten gelen tüm bağlantıları güvenlik hizmeti ve bağlantı noktası, örneğin secscv001.cloudapp.net:xxxx anlamına gelir.
> 
> 

Bu senaryolar için aşağıdaki güvenlik duvarı kuralları yerinde olmalıdır:

1. Güvenlik Duvarı Yönetimi
2. RDP IIS01 için
3. RDP DNS01 için
4. RDP AppVM01 için
5. RDP AppVM02 için
6. Web uygulaması trafiği
7. AppVM01 uygulama trafiği
8. Giden internet
9. DNS01 için ön uç
10. İçi alt ağ trafiği (yalnızca ön uç için arka uç)
11. Tümünü Reddet

Gerçek güvenlik duvarı ruleset büyük olasılıkla bu ek olarak birçok diğer kurallar vardır, belirli bir güvenlik duvarı kurallarında farklı öncelik numaraları burada listelenenlerden daha da sahip olur. Bu liste ve ilişkili numaralar on bu kurallar ve bunları arasında göreli öncelik arasında uygunluk sağlamak üzeresiniz. Diğer bir deyişle; Gerçek Güvenlik Duvarı'nı 5 kural sayısı olabilir, ancak "Güvenlik duvarı Yönetimi" kuralıdır ve "RDP için DNS01" kural amacıyla bu listeyi hizalayın sürece "RDP için IIS01" olabilir. Listenin içinde de yardımcı olur kısaltma; izin verme senaryolar aşağıda Örneğin "FW kuralı 9 (DNS)". Ayrıca okumanızdır dört RDP kuralları topluca çağrılır, "RDP kuralları" trafik senaryo olduğunda için RDP ilgisiz.

Ayrıca ağ güvenlik grupları yerinde geri çağırma ön uç ve arka uç ağlardaki gelen Internet trafiği için.

#### <a name="allowed-internet-to-web-server"></a>(İzin verilir) Web sunucusuna Internet
1. Internet kullanıcı istekleri HTTP sayfasına SecSvc001.CloudApp.Net (Internet'e yönelik bulut hizmeti)
2. Bulut hizmeti geçiş trafiği 80 numaralı bağlantı noktasında güvenlik duvarı arabiriminizi 10.0.0.4:80 üzerinde açık uç noktası aracılığıyla
3. Güvenlik alt ağına atanmış hiçbir NSG bunu sistem NSG kuralları güvenlik duvarı trafiğine izin verecek.
4. İç IP adresi (10.0.1.4) Güvenlik Duvarı'nın trafik isabetler
5. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralları 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
   3. FW kural 6 (uygulama: Web) uygulama trafiğine izin verilir, güvenlik duvarı NAT 10.0.1.4 (IIS01) için
6. Ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1'i (blok Internet) olmayan uygulama (Bu trafiği NAT oluştu güvenlik duvarı tarafından gerekir, böylece kaynak adresi şimdi güvenlik alt ağ üzerinde olan güvenlik duvarı ve ön uç alt ağa göre NSG "yerel" trafiği olarak görülen ve böylece izin), sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
7. IIS01 web trafiği için dinleme, bu isteği alır ve isteği işlemeye başlıyor
8. IIS01 girişimleri AppVM01 arka uç alt ağdaki bir FTP oturumu başlatır
9. Ön uç alt ağdaki UDR rota güvenlik duvarı sonraki atlama yapar.
10. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
11. Güvenlik duvarı kuralı işleme başlar:
    1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
    2. FW kuralı 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
    3. FW kural 6 (uygulama: Web) geçerli değil, sonraki kural taşıma
    4. FW kural 7 ' (uygulama: arka uç) uygulama trafiğine izin verilir, güvenlik duvarı 10.0.2.5 (AppVM01) trafiği iletir
12. Arka uç alt gelen kuralı işleme başlar:
    1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
13. AppVM01 isteği alır ve oturumu başlatır ve yanıtlar
14. Arka uç alt ağdaki UDR rota güvenlik duvarı sonraki atlama yapar.
15. Yanıt izin arka uç alt ağda giden hiçbir NSG kuralları olduğundan
16. Bu yerleşik bir oturum üzerinde trafiği döndürmektir çünkü güvenlik duvarı yanıtı web sunucusu (IIS01) geçirir.
17. Ön uç alt gelen kuralı işleme başlar:
    1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
18. IIS Sunucu yanıtı alır, AppVM01 işlemle tamamlandıktan ve HTTP yanıtı oluşturma tamamlandıktan, istek sahibine gönderilen bu HTTP yanıtı
19. Yanıt izin verilen ön uç alt ağda giden hiçbir NSG kuralları olduğundan
20. HTTP yanıtı güvenlik duvarı İsabetli ve bu yerleşik bir NAT oturum yanıtı olduğundan güvenlik duvarı tarafından kabul edilir
21. Güvenlik Duvarı yanıtı Internet kullanıcı ardından yönlendirir.
22. Olduğundan Hayır giden NSG kuralları veya yanıt izin verilen ön uç alt ağdaki UDR atlama ve Internet kullanıcının istenen web sayfasının alır.

#### <a name="allowed-internet-rdp-to-backend"></a>(İzin verilir) Internet arka uç için RDP
1. Sunucu Yöneticisi internet üzerindeki AppVM01 8025 "RDP için AppVM01" güvenlik duvarı kuralı kullanıcı tarafından atanan bağlantı noktası numarasını olduğu SecSvc001.CloudApp.Net:8025 üzerinden RDP oturumu istekleri
2. Bulut hizmeti 10.0.0.4:8025 güvenlik duvarı arabirimde 8025 bağlantı noktasında açık uç noktası üzerinden trafiğe geçirir
3. Güvenlik alt ağına atanmış hiçbir NSG bunu sistem NSG kuralları güvenlik duvarı trafiğine izin verecek.
4. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW Kural 2 (RDP IIS) değil, uygulamak için sonraki kural taşıma
   3. FW kural 3 (RDP DNS01) değil, uygulamak için sonraki kural taşıma
   4. FW kuralı 4 (RDP AppVM01) uygulamak için trafik verilir, güvenlik duvarı NAT 10.0.2.5:3386 kendisine (AppVM01 üzerinde RDP noktasına)
5. Arka uç alt gelen kuralı işleme başlar:
   1. Olmayan NSG kural 1'i (blok Internet) uygulama (Bu trafiği NAT oluştu güvenlik duvarı tarafından gerekir, böylece kaynak adresi şimdi güvenlik alt ağ üzerinde olan güvenlik duvarı ve arka uç alt ağa göre NSG "yerel" trafiği olarak görülen ve böylece izin verilir), sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
6. AppVM01 RDP trafiği için dinleme ve yanıtlar
7. Giden hiçbir NSG kuralları, varsayılan kuralları uygula ve dönüş trafiğine izin verilir
8. UDR Güvenlik Duvarı'na giden trafik, sonraki atlama olarak yönlendirir.
9. Bu yerleşik bir oturum üzerinde trafiği döndürmektir çünkü güvenlik duvarı yanıtı Internet kullanıcı geçirir.
10. RDP oturumu etkin
11. Kullanıcı adı ve parolasını AppVM01 ister

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) DNS sunucusundaki Web sunucusu DNS araması
1. Sunucu, IIS01, www.data.gov bir veri akışı gereksinimlerini ancak adresini çözümlemek için gereksinimlerini web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağda 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS isteği için DNS01 gönderir
3. UDR Güvenlik Duvarı'na giden trafik, sonraki atlama olarak yönlendirir.
4. Giden hiçbir NSG kuralları ön uç alt ağına bağlı olan, trafiğe izin verilir
5. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralı 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
   3. FW kuralları 6 ve 7 (uygulama kuralları) verme geçerli, sonraki kural taşıma
   4. FW kural 8 (Internet için) değil, uygulamak için sonraki kural taşıma
   5. FW kural 9 (DNS) geçerli, trafiğe izin verilir, güvenlik duvarı 10.0.2.4 (DNS01) trafiğini
6. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
7. DNS sunucusu isteği alır
8. DNS sunucusu önbelleğe adresi yoksa ve bir kök DNS sunucusu internet'te sorar
9. UDR Güvenlik Duvarı'na giden trafik, sonraki atlama olarak yönlendirir.
10. Giden hiçbir NSG kuralları arka uç alt ağdaki trafiğe izin verilir
11. Güvenlik duvarı kuralı işleme başlar:
    1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
    2. FW kuralı 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
    3. FW kuralları 6 ve 7 (uygulama kuralları) verme geçerli, sonraki kural taşıma
    4. FW kural 8 (Internet için) geçerlidir, trafiğe izin verilir, çıkışı SNAT Internet üzerindeki kök DNS sunucusuna oturumdur
12. Bu oturumda Güvenlik Duvarı'ndan başlatılan olduğundan, Internet DNS sunucusu yanıt, yanıt güvenlik duvarı tarafından kabul edilir
13. Yerleşik bir oturum olarak güvenlik duvarı yanıt DNS01 başlatma sunucusuna iletir.
14. Arka uç alt gelen kuralı işleme başlar:
    1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
15. DNS sunucusu alır ve yanıtı önbelleğe alır ve ardından geri IIS01 ilk isteğine yanıt verir
16. Arka uç alt ağdaki UDR rota güvenlik duvarı sonraki atlama yapar.
17. Giden hiçbir NSG kuralları arka uç alt ağda var, trafiğe izin verilir
18. Bu Güvenlik Duvarı'nda yerleşik bir oturum, yanıt IIS sunucuya geri güvenlik duvarı tarafından iletilir
19. Ön uç alt gelen kuralı işleme başlar:
    1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
    2. Trafiğe izin için alt ağlar arasında trafiğe izin varsayılan sistem kuralı bu trafiği olanak tanır
20. IIS01 DNS01 yanıtı alır

#### <a name="allowed-backend-server-to-frontend-server"></a>(İzin verilir) Ön uç sunucusu için arka uç sunucusu
1. RDP aracılığıyla AppVM02 için oturum açmış bir yönetici, doğrudan windows dosya Gezgini aracılığıyla IIS01 sunucusundan bir dosya istediğinde
2. Arka uç alt ağdaki UDR rota güvenlik duvarı sonraki atlama yapar.
3. Yanıt izin arka uç alt ağda giden hiçbir NSG kuralları olduğundan
4. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralı 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
   3. FW kuralları 6 ve 7 (uygulama kuralları) verme geçerli, sonraki kural taşıma
   4. FW kural 8 (Internet için) değil, uygulamak için sonraki kural taşıma
   5. FW kural 9 (DNS) değil, uygulamak için sonraki kural taşıma
   6. FW kural 10 (içi alt ağ) geçerli, trafiğe izin verilir, güvenlik duvarı trafiği 10.0.1.4 (IIS01) geçirir.
5. Ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
6. Uygun kimlik doğrulama ve yetkilendirme varsayıldığında, IIS01 isteği kabul eder ve yanıtlar
7. Ön uç alt ağdaki UDR rota güvenlik duvarı sonraki atlama yapar.
8. Yanıt izin verilen ön uç alt ağda giden hiçbir NSG kuralları olduğundan
9. Bu Güvenlik Duvarı'nda var olan bir oturum olarak bu yanıt izin verilir ve güvenlik duvarı AppVM02 yanıtı döndürür
10. Arka uç alt gelen kuralı işleme başlar:
    1. NSG kural 1'i (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin verme, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
11. AppVM02 yanıtı alır

#### <a name="denied-internet-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Internet
1. Internet kullanıcı FrontEnd001.CloudApp.Net hizmeti aracılığıyla IIS01, web sunucusu erişmeyi dener
2. HTTP trafiği için açık uç nokta yok olduğundan bu bulut hizmeti aracılığıyla geçecek değil ve sunucunun ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağdaki (blok Internet) NSG bu trafiği engeller
4. Son olarak, ön uç alt UDR rota tüm giden trafiği IIS01 Güvenlik Duvarı'nda sonraki atlama olarak gönderir ve Güvenlik Duvarı'nı bu asimetrik trafiği olarak görebilir ve giden yanıt en az üç bağımsız katmanlar arasında savunma vardır nedenle bırakma Internet ve yetkisiz/uygunsuz erişimi engelleme kendi bulut hizmeti aracılığıyla IIS01.

#### <a name="denied-internet-to-backend-server"></a>(Reddedildi) Arka uç sunucusuna Internet
1. Internet kullanıcı BackEnd001.CloudApp.Net hizmeti aracılığıyla AppVM01 bir dosyaya erişmeyi dener
2. Dosya Paylaşımı için açık uç nokta yok olduğundan bu bulut hizmeti geçip geçmeyeceğini değil ve tarafından sunucuya ulaşmak olmayacaktır
3. NSG (blok Internet) uç noktaları için herhangi bir nedenle açıksa, bu trafiği engeller
4. Son olarak, UDR rota tüm giden trafiği AppVM01 Güvenlik Duvarı'nda sonraki atlama olarak gönderir ve Güvenlik Duvarı'nı bu asimetrik trafiği olarak görebilir ve böylece savunma internet arasında en az üç bağımsız katmanı vardır giden yanıt bırakma ve Yetkisiz/uygunsuz erişimi engelleme kendi bulut hizmeti aracılığıyla AppVM01.

#### <a name="denied-frontend-server-to-backend-server"></a>(Reddedildi) Arka uç sunucusuna ön uç sunucusu
1. IIS01 aşılmış ve arka uç sunucuları alt için tararken kötü amaçlı kod çalıştığını varsayar.
2. Ön uç alt UDR rota tüm giden trafiği IIS01 Güvenlik Duvarı'na sonraki atlama olarak gönderebilir. Bu, güvenliği aşılan VM tarafından değiştirilebilir bir şey değildir.
3. İstek AppVM01 veya trafiğe Güvenlik Duvarı'nı (nedeniyle FW kuralları 7 ve 9) tarafından izin DNS aramaları için DNS sunucusu olduğunda güvenlik duvarının trafiğini işleyecek. Diğer tüm trafik FW kural 11 göre (tüm reddetme) engellenebilir.
4. Tehdit algılama Gelişmiş Güvenlik Duvarı'nı etkinleştirildi (da değil kapsanan bu belgede, Gelişmiş tehdit özellikleri, belirli bir ağ Gereci satıcı belgelerine bakın), hatta temel iletme kuralları tarafından izin trafiği Bu konuda ele alınan belge engelledi trafiği bilinen imzaları ya da bir Gelişmiş tehdit kuralı bayrak desenler içeriyorsa.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Reddedildi) DNS sunucusunda Internet DNS araması
1. Internet kullanıcı DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını arama dener 
2. DNS trafiği için açık uç nokta yok olduğundan bu bulut hizmeti aracılığıyla geçip geçmeyeceğini değil ve sunucunun ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağda NSG kuralının (blok Internet) bu trafiği engeller
4. Son olarak, arka uç alt UDR rota tüm giden trafiği DNS01 Güvenlik Duvarı'nda sonraki atlama olarak gönderir ve Güvenlik Duvarı'nı bu asimetrik trafiği olarak görebilir ve giden yanıt en az üç bağımsız katmanlar arasında savunma vardır nedenle bırakma Internet ve yetkisiz/uygunsuz erişimi engelleme kendi bulut hizmeti aracılığıyla DNS01.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Reddedildi) SQL erişimi güvenlik duvarı üzerinden İnternete
1. Internet kullanıcı SQL verileri SecSvc001.CloudApp.Net (Internet'e yönelik bulut hizmeti) ' ister.
2. SQL için açık uç nokta yok olduğundan bu bulut hizmeti geçip geçmeyeceğini değil ve güvenlik duvarı ulaşmak olmayacaktır
3. SQL uç noktaları için herhangi bir nedenle açıksa, güvenlik duvarı kuralı işlemeye başlamak:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralları 2-5 (RDP kurallar) yok, uygulamak için sonraki kural taşıma
   3. FW kural 6 ve 7 (uygulama kuralları) verme geçerli, sonraki kural taşıma
   4. FW kural 8 (Internet için) değil, uygulamak için sonraki kural taşıma
   5. FW kural 9 (DNS) değil, uygulamak için sonraki kural taşıma
   6. FW kural 10 (içi alt ağ) değil, uygulamak için sonraki kural taşıma
   7. FW kural 11 (Tümünü Reddet) geçerli, engellenmiş, Dur kural işlenirken trafiğidir

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırması
Tam komut dosyasını bir PowerShell komut dosyası kaydedin. Ağ Yapılandırma "NetworkConf2.xml" adlı bir dosyaya kaydedin.
Kullanıcı tanımlı değişkenleri gerektiği gibi değiştirin. Komut dosyasını çalıştırın, ardından yukarıdaki güvenlik duvarı kuralı kurulum yönergeleri izleyin.

#### <a name="full-script"></a>Tam komut dosyası
Kullanıcı tanımlı değişkenleri esas alarak bu betiği olur:

1. Bir Azure aboneliğine Bağlanma
2. Yeni depolama hesabı oluşturma
3. Yeni bir VNet ve ağ yapılandırma dosyasında tanımlanan üç alt ağlar oluşturun
4. Beş sanal makineler oluşturun; Güvenlik Duvarı'nı 1 ve 4 windows server Vm'leri
5. UDR dahil olmak üzere yapılandırın:
   1. İki yeni yönlendirme tabloları oluşturma
   2. Yollar tablolarına ekleme
   3. Tablolar için uygun alt ağları bağlama
6. NVA üstünde IP iletimini etkinleştirme
7. NSG dahil olmak üzere yapılandırın:
   1. Bir NSG oluşturma
   2. Bir kural ekleme
   3. NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bir bilgisayar veya sunucu, Internet'e yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırdığınızda, uyarı veya PowerShell'de pop diğer bilgilendirici iletileri olabilir. Yalnızca hata iletileri kırmızı sorunu nedeni edilir.
> 
> 

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)

     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA

      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).

     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.

      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4

      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4

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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"

      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"

      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"

      # NSG Details
        $NSGName = "MyVNetSG"

    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.

        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"

        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"

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

    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}

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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }

    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan

      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"

      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal

      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName

     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable

    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *

      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Çift yönlü DMZ NVA, NSG ve UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Güvenlik duvarı kurallarını mantıksal görünümü"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Bir ön uç ağ nesnesi oluşturun"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Bir DNS sunucusu nesnesi oluşturun"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Varsayılan RDP kuralı kopyası"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 kuralı"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Uygulama yeniden yönlendirme simgesi"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Hedef NAT simgesi"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Geçişi simgesi"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Güvenlik Duvarı Yönetimi kuralı"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Güvenlik Duvarı RDP kuralı"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Güvenlik Duvarı Web kuralı"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Güvenlik Duvarı AppVM01 kuralı"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Giden güvenlik duvarı kuralı"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Güvenlik Duvarı DNS kuralı"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Güvenlik Duvarı içi sanal ağ kuralı"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Güvenlik duvarı kuralı Reddet"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Güvenlik duvarı kuralını etkinleştirme"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
