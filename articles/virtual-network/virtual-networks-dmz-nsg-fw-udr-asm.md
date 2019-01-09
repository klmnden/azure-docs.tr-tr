---
title: DMZ örnek – ağları bir güvenlik duvarı, UDR ve NSG ile korunacak bir DMZ oluşturmak | Microsoft Docs
description: Çevre ağı ile bir güvenlik duvarı oluşturma, kullanıcı tanımlı yönlendirme (UDR) ve ağ güvenlik grupları (NSG)
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
ms.openlocfilehash: 9c2ebcfc376456f63896ebae8331136aff0cdb99
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119450"
---
# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Örnek 3: ağları bir güvenlik duvarı, UDR ve NSG ile korunacak bir DMZ oluşturma
[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

Bu örnek, bir güvenlik duvarı, dört windows sunucuları, kullanıcı tanımlı yönlendirme, IP iletimi ve ağ güvenlik grupları ile DMZ oluşturma. Bu da her her adımda daha derin bir anlayış sağlamak için ilgili komutları yol gösterir. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryo bölümü yoktur adım adım dmz'deki savunma katmanları ile nasıl trafiği devam eder. Son olarak, test ve deneme ile çeşitli senaryolar için bu ortamı oluşturmak için yönerge ve tam kod başvuruları bölüm yöneliktir. 

![UDR NVA ve NSG ile iki yönlü DMZ][1]

## <a name="environment-setup"></a>Ortam Kurulumu
Bu örnekte aşağıdakileri içeren bir aboneliği vardır:

* Üç bulut Hizmetleri: "SecSvc001", "FrontEnd001" ve "BackEnd001"
* "CorpNetwork" üç alt ağ ile sanal ağ: "SecNet", "Ön uç" ve "Arka uç"
* Bu örnekte bir güvenlik duvarı bir ağ sanal Gereci SecNet alt ağa bağlı
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows Server
* Uygulama geri temsil eden iki windows sunucuları sunucuları ("AppVM01", "AppVM02") sona
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server

Başvurular bölümdeki çoğu yukarıda açıklanan ortam oluşturacak bir PowerShell Betiği yoktur. VM'ler ve sanal ağlar oluşturma örnek komut dosyası tarafından yapılır ancak değil açıklanmıştır ayrıntılı bu belgedeki.

Ortamı oluşturmak için:

1. References bölümünde dahil ağ yapılandırma xml dosyasını kaydedin (adları, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilmiş)
2. Kullanıcı değişkenleri betiğinde betiğidir karşı (abonelik, hizmet adlarını, vb.) çalıştırılması için ortam eşleşecek şekilde güncelleştirin
3. PowerShell Betiği yürütün

**Not**: PowerShell betik miktarlara bölge ağ yapılandırma xml dosyasında miktarlara bölge eşleşmelidir.

Betik başarıyla çalıştırıldıktan sonra aşağıdaki betik sonrası adımlar izlenebilir:

1. Güvenlik duvarı kuralları ayarlamak, bu başlıklı aşağıdaki bölümde ele alınmıştır: Güvenlik duvarı kuralı açıklaması.
2. İsteğe bağlı olarak bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile uygulama sunucusu ve web sunucusu kurmak için iki komut dosyası başvuruları bölümünde bulunur.

Komut kuralları tamamlanması gerekir güvenlik duvarı başarıyla çalıştırıldıktan sonra bunu başlıklı bölümde ele alınmıştır: Güvenlik duvarı kuralları.

## <a name="user-defined-routing-udr"></a>Kullanıcı tanımlı yönlendirme (UDR)
Varsayılan olarak, aşağıdaki sistem yolları olarak tanımlanır:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal her zaman sanal ağın (IE, sanal ağdan sanal ağa belirli her VNet nasıl tanımlandığını bağlı olarak değişir), belirli bir ağ için tanımlanan adres ön olur. Kalan sistem rotaları, statik ve yukarıdaki varsayılan.

Öncelik için yollar en uzun ön ek eşleşmesi (LPM) temel yöntem işlenir, bu nedenle en belirli bir yol tablosundaki belirli hedef adresi için uygulanacak.

Bu nedenle 10.0.0.0/16 rota nedeniyle hedefine sanal ağ arasında trafiği (örneğin sunucuya DNS01 10.0.2.4) yerel ağ (10.0.0.0/16) yönlendirilirdi. Diğer bir deyişle, 10.0.2.4 için 10.0.0.0/16 en belirli bir yol 10.0.0.0/8 ve 0.0.0.0/0 de uygulayabilirsiniz, ancak daha az olduğundan belirli kullanıcılar bu trafiği etkilemez olsa bile yoldur. Bu nedenle 10.0.2.4 trafik, yerel sanal ağ'ın bir sonraki atlamaya sahip ve yalnızca hedef yol.

Trafik tasarlandıysa 10.1.1.1 için örneğin, 10.0.0.0/16 rota uygularsanız mıydı, ancak 10.0.0.0/8 en belirgin olur ve bu trafiği olacaktır ("siyah holed") sonraki atlama Null olduğundan bırakıldı. 

Hedef herhangi bir Null ön ekleri veya VNETLocal önekleri uygulanmadı sonra en az belirli izleyin route 0.0.0.0/0 ve Internet'e sonraki atlama olarak ve bu nedenle Azure'nın internet ucuna out yönlendirilmesini.

Rota tablosunda iki özdeş bir önek varsa, yolları "kaynak" özniteliğine dayanarak tercih sırasına göre verilmiştir:

1. "VirtualAppliance" el ile tabloya eklenen kullanıcı tanımlı yol =
2. "VPNGateway" (BGP) ile karma ağ kullanıldığında, bir dinamik yönlendirme = dinamik Protokolü otomatik olarak eşlenen ağ değişiklikleri yansıtan gibi dinamik ağ protokolü tarafından eklenen, bu yollar zaman içinde değişebilir
3. "Varsayılan" rota yukarıdaki tabloda gösterildiği gibi sistem yolları, yerel sanal ağ ve statik girdilerini =.

> [!NOTE]
> Giden ve gelen zorlamak için VPN ağ geçitleri trafik bir ağ sanal Gereci (NVA) yönlendirilmesini şirketler arası ve ExpressRoute ile artık kullanıcı tanımlı yönlendirme (UDR) kullanabilirsiniz.
> 
> 

#### <a name="creating-the-local-routes"></a>Yerel yollar oluşturma
Bu örnekte, iki yönlendirme tablolarını gereklidir, her biri ön uç ve arka uç alt ağları için. Her tabloda belirtilen alt ağ için uygun statik yollar ile yüklenir. Bu örneğin amacı doğrultusunda, her tablo üç yol vardır:

1. Yerel alt ağ trafiğinin güvenlik duvarını geçmesine izin vermek için yerel alt ağ trafiği hiçbir sonraki atlama ile tanımlanan
2. Bir sonraki güvenlik duvarı tanımlanan atlama ile sanal ağ trafiği, bu doğrudan yönlendirmek yerel sanal ağ trafiğine izin veren varsayılan kuralı geçersiz kılar
3. Bir sonraki güvenlik duvarı olarak tanımlanan atlama ile tüm kalan trafiği (0/0)

Yönlendirme tablolarını oluşturulduktan sonra kendi alt ağlara bağlanır. Ön uç alt ağ için bir kez oluşturulur ve alt ağa bağlı yönlendirme tablosu şu şekilde görünmelidir:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Bu örnek için aşağıdaki komutları yol tablosu oluşturun, bir kullanıcı tanımlı yol ekleyin ve sonra yönlendirme tablosunu bir alt ağa bağlamak için kullanılır (Not; aşağıda bir dolar işareti ile başlayan herhangi bir öğe (örn: $BESubnet) komut dosyası kullanıcı tanımlı değişkenler Bu belgenin başvurusu bölümü):

1. İlk temel yönlendirme tablosunun oluşturulması gerekir. Bu kod parçacığı, arka uç alt ağı için bir tablo oluşturulmasını gösterir. Betikte, karşılık gelen bir tablo için ön uç alt ağı da oluşturulur.
   
     Yeni AzureRouteTable-ad $BERouteTableName '
   
         -Location $DeploymentLocation `
         -Label "Route table for $BESubnet subnet"
2. Yol tablosu oluşturulduktan sonra belirli kullanıcı tanımlı yollar eklenebilir. Bu konuda ölçeklendirmesinden, tüm trafik (0.0.0.0/0) (bir değişken $VMIP [0], komut dosyasında sanal gerece oluşturulduğunda atanan IP adresini geçirmek için kullanılır) sanal Gereci aracılığıyla yönlendirilir. Betikte, karşılık gelen bir kural ön uç tabloda da oluşturulur.
   
     Get-AzureRouteTable $BERouteTableName | `
   
         Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
         -NextHopType VirtualAppliance `
         -NextHopIpAddress $VMIP[0]
3. Yukarıdaki rota girişi, trafiği ağ sanal Gerecinin değil de, doğrudan hedefine yönlendirmek için sanal ağ içindeki izin varsayılan "0.0.0.0/0" yol, ancak hala mevcut varsayılan 10.0.0.0/16 kuralını geçersiz kılar. Doğru izleme kuralı bu davranışı eklenmesi gerekir.
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
4. Bu noktada yapılacak bir seçenek yoktur. Yukarıdaki iki rotalarla değerlendirmesi, tek bir alt ağ içinde bile trafiği için Güvenlik Duvarı tüm trafiği yönlendirir. Yerel Güvenlik Duvarı müdahalesi olmadan üçüncü yönlendirmek için bir alt ağ trafiğine izin verecek şekilde belirli bir kural eklenebilir ancak bu, istenebilir. Bu rota var. yerel alt ağ yalnızca için herhangi bir adresi destine durumları rota doğrudan (NextHopType VNETLocal =).
   
        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
5. Son olarak, oluşturduğunuz ve bir kullanıcı tanımlı yollar ile doldurulmuş yönlendirme tablosu ile tablo artık bir alt ağa bağlı olmalıdır. Betikte, ön uç yol tablosu ayrıca ön uç alt ağına bağlanır. Arka uç alt ağı için bağlama betiği aşağıda verilmiştir.
   
     Set-AzureSubnetRouteTable - VirtualNetworkName $VNetName '
   
        -SubnetName $BESubnet `
        -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP İletimi
UDR, bir yardımcı özelliği olan IP iletimi. Bu, özellikle gerecine gönderilmiş olan trafiği almasını ve ardından bu trafiğin ultimate hedefine iletmek sağlayan bir sanal gereç üzerinde bir ayardır.

AppVM01 gelen trafiği DNS01 sunucusuna bir istek yapıyorsa örnek olarak, UDR bu güvenlik duvarı route. Etkin IP iletimi ile DNS01 hedef (10.0.2.4) trafiği (10.0.0.4) Gereci tarafından kabul edilen ve ardından kendi (10.0.2.4) ultimate hedef iletilen. Güvenlik Duvarı'sonraki atlama olarak yol tablosuna sahip olsa da güvenlik duvarını etkin IP iletimi, trafiği gereç tarafından kabul edilebiliyordu değil. 

> [!IMPORTANT]
> Kullanıcı tanımlı yönlendirme ile birlikte IP iletimini etkinleştirmeniz unutmamak önemlidir.
> 
> 

IP iletimi ayarlama, tek bir komuttur ve VM oluşturma sırasında yapılabilir. Bu örnekte akış için kod parçacığı kodun sonuna doğru olduğundan ve UDR komutları ile gruplandırılmış:

1. Bu durumda, bir sanal gereç, güvenlik duvarı olan VM örneği çağırın ve IP iletimini etkinleştirmeniz (Not; bir dolar işareti olan kırmızı başlayan herhangi bir öğeyi (örneğin: $VMName[0]) olan bir kullanıcı tanımlı değişken başvuru bölümünde, bu belgenin komut dosyası. "[0], parantez içine sıfır değişiklik olmadan çalışmaya örnek betiği sanal makinelerin dizideki ilk VM temsil eder, güvenlik duvarı ilk sanal makine (VM 0) olması gerekir):
   
     Get-AzureVM-$VMName [0] - ServiceName $ServiceName [0] adı | `
   
        Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu oluşturulan ve ardından tek bir kural ile yüklendi. Bu grup, ardından yalnızca ön uç ve arka uç alt ağlara (SecNet değil) bağlıdır. Aşağıdaki kural bildirimli olarak üretiliyor:

1. Tüm trafik (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağa (tüm alt ağlar) reddedildi

Nsg'leri Bu örnekte kullanılan olsa da, cihazın ana amacı el ile gerçekleştirilen bir yanlış yapılandırma karşı savunma ikincil bir katmanı gibidir. Ön uç ya da internet'ten gelen tüm trafiği engellemek istediğimiz veya arka uç alt ağları, trafiği yalnızca SecNet alt ağ güvenlik duvarı üzerinden akışını (ve sonra ise, alt ağ ön uç veya arka uç hizmetine uygun). Ayrıca, UDR kurallarıyla bir yerde ön uç veya arka uç alt ağına yaptı tüm trafik kullanıma Güvenlik Duvarı (UDR) sayesinde yönlendirilmesi. Güvenlik Duvarı bu asimetrik bir akış görür ve giden trafiği bırakmaya. Bu nedenle ön uç ve arka uç alt ağları koruma güvenlik üç katmanı vardır; (1) açık uç nokta FrontEnd001 ve BackEnd001 bulut Hizmetleri, 2) Nsg'ler trafik Internet'ten 3) güvenlik duvarı bırakılıyor asimetrik trafiği engelleme.

Bu örnekte ağ güvenlik grubu ile ilgili bir ilgi çekici nokta, yalnızca bir kural, aşağıda gösterilen güvenlik alt dahil tüm sanal ağ için internet trafiği reddetmeye yönelik olduğu içermesidir. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

NSG yalnızca ön uç ve arka uç alt ağa bağlı olduğundan, kuralın trafiğinde ancak işlenen değil güvenlik alt ağına gelen. Sonuç olarak, NSG, hiçbir zaman güvenlik alt ağa bağlı olduğundan NSG kuralı VNet üzerinde herhangi bir adresi yok Internet trafiğini diyor olsa da, trafiği güvenlik alt ağa akacak.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Güvenlik Duvarı Kuralları
Güvenlik Duvarı'nı, kuralları iletme oluşturulması gerekir. Güvenlik duvarı engelleme veya ileten tüm gelen, giden ve içi-VNet trafiği olduğundan çok sayıda güvenlik duvarı kuralları gerekir. Ayrıca, tüm gelen trafiği, güvenlik hizmeti genel IP adresini (farklı bağlantı noktaları üzerinde) güvenlik duvarı tarafından işlenecek ulaşırsınız. Alt ağlar ' ayarlamadan önce mantıksal akışlarda diyagram için en iyi uygulamadır ve önlemek için güvenlik duvarı kuralları daha sonra yeniden. Aşağıdaki şekil, bu örnek için güvenlik duvarı kurallarını mantıksal bir görünümüdür:

![Güvenlik duvarı kurallarını mantıksal görünümü][2]

> [!NOTE]
> Ağ sanal kullanılan Gerecinde bağlı olarak, yönetim bağlantı noktalarına değişir. Barracuda NextGen güvenlik duvarı başvurulan Bu örnekte, bağlantı noktası 22 ve 801 807 kullanır. Lütfen kullanılan cihaz yönetimi için kullanılan bağlantı noktaları bulmak için gereç satıcı belgelerine bakın.
> 
> 

### <a name="logical-rule-description"></a>Mantıksal kural açıklaması
Yukarıdaki mantıksal diyagramda, güvenlik alt ağ, Güvenlik Duvarı bu alt ağda yalnızca bir kaynaktır ve bu diyagramda, güvenlik duvarı kuralları ve nasıl mantıksal olarak izin ver veya trafik akışları ve gerçek yönlendirilmiş yol değil Reddet gösteren gösterilmez. Ayrıca, RDP trafiği için daha yüksek seçilen dış bağlantı noktaları aralıklı bağlantı noktaları (8014 – 8026) ve biraz daha kolay okunabilmesi için yerel IP adresi, son iki sekizlik tabanda hizalamak için seçilmiş olan (örneğin yerel sunucu adresi 10.0.1.4 dış bağlantı noktasıyla ilişkilendirildiğinden 8014), ancak daha yüksek bir çakışma olmayan bağlantı noktaları kullanılabilir.

Bu örnekte, kural 7 türleri ihtiyacımız, bu kural türleri aşağıda açıklanmıştır:

* Dış kuralları (gelen trafik için):
  1. Güvenlik Duvarı Yönetimi kuralı: Bu uygulama yeniden yönlendirme kuralı trafiği ağ sanal Gereci yönetim bağlantı noktalarına geçirilecek sağlar.
  2. RDP kuralları (her windows server için): Bu dört kural (her sunucu için bir tane) RDP aracılığıyla tek tek sunucuların yönetimini sağlar. Bu da ağ sanal Gereci'ı kullanılan özelliklerine bağlı olarak tek bir kural halinde paketlenmiş.
  3. Uygulama trafik kuralları: Ön uç web trafiği için ilk ve ikinci (ör. web sunucusu için veri katmanı) arka uç trafiği için iki uygulama trafik kuralları vardır. Bu kurallar yapılandırmasını (burada sunucularınızın yerleştirilir) ağ mimarisine bağlı ve trafik akışları (hangi yönde trafik akışı ve bağlantı noktalarını kullanılır).
     * İlk kural, uygulama tarafından sunucuya ulaşmak gerçek uygulama trafiği izin verir. Diğer kurallardan izin verirken, güvenlik, yönetim, vb. için uygulamanın hangi dış kullanıcıları veya hizmetler uygulamaları erişmeye izin kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde bir tek bir web sunucusu olduğundan, bu nedenle bir tek uygulama güvenlik duvarını gelen trafiğe web sunucuları iç IP adresi için dış IP yönlendirilecek. Yeniden yönlendirilen trafik oturumu NAT iç sunucuya vardı.
     * İkinci uygulama trafiği kuralı AppVM01 sunucu (ancak değil AppVM02) konuşmak Web sunucusu herhangi bir bağlantı noktası izin vermek için arka uç kuralıdır.
* (İçi-VNet trafiği) iç kurallar
  1. Giden Internet kuralı için: Bu kural, seçili ağlar için geçirilecek herhangi bir ağa gelen trafiğe izin verir. Bu kural, genellikle varsayılan bir kural zaten güvenlik duvarı, ancak devre dışı durumuna içindir. Bu kural, bu örnek için etkinleştirilmesi gerekir.
  2. DNS kuralı: Bu kural yalnızca DNS sunucusuna geçirilecek DNS (bağlantı noktası 53) trafiğine izin verir. Arka uca ön uç çoğu trafiği engellenir ve bu ortam için bu kural, DNS özellikle yerel bir alt ağdan sağlar.
  3. Alt ağ için alt ağ kuralı: Bu kural, ön uç alt (ancak tersine) herhangi bir sunucuya bağlanmak için arka uç alt ağdaki herhangi bir sunucu sağlamaktır.
* Emniyet kuralı (yukarıdakilerden herhangi biri karşılamıyor trafiği):
  1. Tüm trafik kuralı Reddet: Bu her zaman son kuralı (öncelik) açısından olmalıdır ve trafik akışları, bu nedenle bu kural tarafından bırakılır önceki kurallardan herhangi birinin eşleştirmek başarısız olur. Bu varsayılan kural ve genellikle etkin, herhangi bir değişiklik genellikle gereklidir.

> [!TIP]
> İkinci uygulama trafik kuralını ve herhangi bir bağlantı noktası en belirli bağlantı noktası bu örnekte gerçek bir senaryo, kolay izin verilir ve adres aralıkları, bu kural saldırı yüzeyini azaltmak için kullanılmalıdır.
> 
> 

<br />

> [!IMPORTANT]
> Yukarıdaki kurallarının tümü oluşturulduktan sonra trafiğe izin veya istediğiniz gibi reddedildi emin olmak için her bir kural önceliklerini gözden geçirilmesi önemlidir. Bu örnekte, öncelik sırasına göre kurallardır. Güvenlik Duvarı yanlış sıralı kuralları nedeniyle dışında kilitlenmesi kolay bir işlemdir. En azından, özel olarak Yönetim güvenlik duvarı kendisi için her zaman mutlak en yüksek öncelikli kuralı olduğundan emin olun.
> 
> 

### <a name="rule-prerequisites"></a>Kural önkoşulları
Güvenlik Duvarı'nı çalıştıran sanal makine için bir önkoşul ortak uç noktalar şunlardır. Trafiğini işlemek Güvenlik Duvarı için uygun bir ortak Uç noktalara açık olması gerekir. Bu örnekte trafiğin üç tür vardır; Windows sunucuları ve 3) uygulama trafiği denetlemek için RDP trafiği 1) yönetim trafiğini denetimi güvenlik duvarı ve güvenlik duvarı kuralları, 2). Bu üç üst trafik türlerinin güvenlik duvarı kurallarına mantıksal görünümünü yarısını sütunlarıdır.

> [!IMPORTANT]
> Burada bir anahtar takeway unutmayın sağlamaktır **tüm** trafiği, güvenlik duvarı üzerinden gelen. Bu nedenle IIS01 sunucuya Uzak Masaüstü için ön uç bulut hizmeti ve ön uç alt olmasına rağmen bu sunucuya erişmek için biz RDP için güvenlik duvarında bağlantı noktası 8014 gerekir ve ardından RDP isteği IIS01 RDP Por için dahili olarak yönlendirmek Güvenlik Duvarı'nı izin t. Azure portalının "Bağlan" düğmesi olduğundan IIS01 için doğrudan RDP yol yok (portal görebilirsiniz kadar) işe yaramaz. Başka bir deyişle, İnternet'ten gelen tüm bağlantıları, güvenlik ve bağlantı noktası, örneğin secscv001.cloudapp.net:xxxx (başvurusu dış bağlantı noktası, iç IP ve bağlantı noktası eşlemesi için yukarıdaki diyagramda) olacaktır.
> 
> 

Bir uç nokta ya da VM oluşturma zamanında açılabilir veya örnek komut dosyasında yapılan ve aşağıda bu kod parçacığında gösterildiği gibi derleme sonrası (Not; herhangi bir dolar işareti öğesi başlayarak (örn: $VMName[$i]) olan bir kullanıcı tanımlı değişken başvuru _Böl içinde komut dosyası Bu belgenin n. Parantez içine "$i" [$i] belirli bir VM'ye sanal dizide dizi sayısını temsil eder):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Değişkenleri, ancak uç noktalar kullanımı nedeniyle değil açıkça burada gösterilen ancak **yalnızca** güvenlik bulut hizmetinde açılır. Tüm gelen trafiği yönetildiğinden emin olmak için budur (yönlendirilmiş NAT bırakılan içeren) bir güvenlik duvarı tarafından.

Bir yönetim istemcisi bir güvenlik duvarını yönetmeyi ve gerekli yapılandırmaları oluşturmak için bir bilgisayarda yüklü olması gerekir. Cihazı yönetmek, Güvenlik Duvarı (veya diğer NVA) belgelerinden satıcı bakın. Bu bölümde ve sonraki bölümde, güvenlik duvarı kuralları oluşturma, geri kalanında, güvenlik duvarı yapılandırmasını kendisini satıcıları Yönetimi istemcisi (yani Azure portal veya PowerShell) üzerinden anlatmaktadır.

İstemci Yükleme ve bu örnekte kullanılan Barracuda bağlanmak için yönergeler burada bulunabilir: [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin)

Güvenlik Duvarı üzerine ancak güvenlik duvarı kuralları oluşturmadan önce açtıktan sonra daha kolay kurallar oluşturma yapabileceğiniz iki önkoşul nesne sınıfları vardır; & Ağ hizmeti nesneleri.

Bu örnekte, üç adlandırılmış ağ nesnelerini tanımlanmış (ön uç alt ağını ve arka uç alt ağı, ayrıca bir DNS sunucusunun IP adresi için ağ nesne için bir) olmalıdır. Adlandırılmış bir ağ oluşturmak için; Barracuda NG yönetici istemci panodan başlayarak, yapılandırma sekmesine gidin, işletimsel yapılandırma bölümünde Ruleset tıklayın, ardından güvenlik duvarı nesneleri menüsü altındaki "Ağ"'a tıklayın ve ardından yeni ağlar düzenleme menüsünü. Ağ nesnesini artık, adı ve önek ekleyerek oluşturulabilir:

![Bir ön uç ağ nesnesi oluşturun][3]

Bu adlandırılmış FrontEnd alt ağı oluşturur, arka uç alt ağı için de benzer bir nesne oluşturulmalıdır. Artık alt ağların güvenlik duvarı kuralları ada göre daha kolay başvurulabilir.

DNS sunucu nesnesi için:

![Bir DNS sunucusu nesnesi oluşturun][4]

Bu tek IP adresi başvurusu, bir DNS kuralında belgenin ilerleyen bölümlerinde kullanılır.

İkinci önkoşul nesneleri Hizmetleri nesneleridir. Bu, her sunucu için RDP bağlantı noktaları temsil eder. Mevcut RDP hizmet nesnesi varsayılan RDP bağlantı noktasına bağlı olduğundan 3389, dış bağlantı noktaları (8026 8014) gelen trafiğe izin verecek şekilde yeni hizmetler oluşturulabilir. Yeni bağlantı noktaları, RDP hizmetiniz eklenebilir, ancak tanıtım kolaylığı için her sunucu için tek bir kural oluşturulabilir. Bir sunucu için yeni bir RDP kuralı oluşturmak için; Barracuda NG yönetici istemci panodan başlayarak, yapılandırma sekmesine gidin, kural kümesi, işlem yapılandırma bölümünde tıklayın sonra "Hizmetler" altında güvenlik duvarı nesneleri menüsünü, hizmetlerin listesini kaydırın ve "RDP" hizmetini seçin. Sağ tıklayın ve kopyalama,'ı seçin, sonra sağ tıklayın ve Yapıştır seçin. Düzenlenebilir bir RDP Copy1 hizmet nesnesi artık yoktur. RDP Copy1 sağ tıklayın ve Düzenle, Düzenle hizmet penceresi aşağıda gösterildiği gibi en üstten nesnesi seçin:

![Varsayılan RDP kuralının kopyalama][5]

Değerleri, ardından belirli bir sunucu için RDP hizmeti göstermek için düzenlenebilir. AppVM01 için yeni hizmet adı, açıklama ve dış RDP Şekil 8 şemada kullanılan bağlantı noktasını yansıtmak için yukarıdaki varsayılan RDP kuralının değiştirilmesi gerekir (Not: Bu belirli bir sunucu için kullanılan dış bağlantı noktasına 3389 RDP varsayılan bağlantı noktalarını değiştirilir söz konusu olduğunda AppVM01 8025 dış bağlantı noktası olan) değiştirilen hizmetin aşağıda gösterilmiştir:

![AppVM01 kuralı][6]

Geri kalan sunucular için RDP hizmetleri oluşturmak için bu işlemin yinelenmesi gerekir; AppVM02 DNS01 ve IIS01. Bu hizmetler oluşturulmasını kural oluşturma sonraki bölümde daha basit ve daha belirgin yapar.

> [!NOTE]
> İki nedenden dolayı bir RDP Hizmeti Güvenlik Duvarı için gerekli değildir; (1) ilk VM Güvenlik duvarı olup Linux tabanlı bir görüntü SSH kullanılabilir bağlantı noktası 22 için VM Yönetimi yerine RDP ve (2) bağlantı noktası 22 ve diğer yönetim bağlantı noktalarına iki için yönetim bağlantısına izin vermek için aşağıda açıklanan ilk yönetim kuralında izin verilir.
> 
> 

### <a name="firewall-rules-creation"></a>Güvenlik duvarı kuralları oluşturma
Bu örnekte kullanılan güvenlik duvarı kuralları üç tür vardır, hepsi farklı simgeleri olur:

Uygulama yeniden yönlendirme kuralı: ![Uygulama yeniden yönlendirme simgesi][7]

Hedef NAT kuralı: ![Hedef NAT simgesi][8]

Geçişi kuralı: ![Geçişi simgesi][9]

Bu kurallar hakkında daha fazla bilgi Barracuda web sitesinde bulunabilir.

Aşağıdaki kurallar oluşturun (veya var olan varsayılan kuralları doğrulamak için), Barracuda NG yönetici istemci panodan başlangıç yapılandırma sekmesine gidin, işletimsel yapılandırmada bölümü Ruleset'a tıklayın. Adlı bir kılavuz için "Ana kuralları" Bu Güvenlik Duvarı'nı mevcut etkin ve devre dışı bırakılan kuralları gösterir. Bu kılavuz sağ üst köşesinde küçük yeşil olduğundan "+" düğmesi, yeni bir kural oluşturmak için tıklayın (Not: güvenlik duvarınızı "değişiklikleri kilitlenebilir", görürseniz bir düğme "Kilitle" olarak işaretlenmiş ve oluşturma veya kuralları düzenlemek için "kural kümesi kilidini açmak için" Bu düğmeye tıklayın belirleyemiyoruz ve  düzenlemeye izin ver). Mevcut bir kuralı düzenlemek istiyorsanız, bu kuralı seçin, sağ tıklayın ve kuralı Düzenle'yi seçin.

Sonra kurallarınızı oluşturulan ve/veya değiştirilmiş, bunlar gerekir güvenlik duvarı gönderilmesine ve etkinleştirildiğinden, Bu yapılmazsa, kural değişikliklerini etkili olmaz. Anında iletme ve etkinleştirme işlemi ayrıntıları kuralı açıklamaları açıklanmıştır.

Bu örnekte tamamlamak için gereken her bir kural ayrıntılarını aşağıda açıklanmıştır:

* **Güvenlik duvarı kuralı Yönetim**: Bu uygulama yeniden yönlendirme kuralı, bu örnekte bir Barracuda NextGen güvenlik duvarı ağ sanal Gereci yönetim bağlantı noktalarını geçirilecek trafiğine izin verir. Yönetim bağlantı noktalarına 801, 807 ve isteğe bağlı olarak 22'dir. Dış ve iç bağlantı noktalarını (yani hiçbir bağlantı noktası çeviri) aynıdır. Bu kural, Kurulum-MGMT-erişim, bir varsayılan kural ve (Barracuda NextGen güvenlik duvarı sürüm 6.1 içinde) varsayılan olarak etkin.
  
    ![Güvenlik Duvarı Yönetimi][10]

> [!TIP]
> Varsa, bu kuralın kaynak adres alanında olduğundan yönetim IP adresi aralıklarını bilinmiyorsa, bu kapsam azaltma da saldırı yüzeyini yönetim bağlantı noktalarına azaltır.
> 
> 

* **RDP kuralları**:  Bu hedef NAT kuralları, RDP aracılığıyla tek tek sunucuların yönetimini sağlar.
  Bu kuralı oluşturmak için gereken dört önemli alanlar şunlardır:
  
  1. RDP yerden başvuru "Tümü" izin vermek için kaynak – kaynak alanında kullanılır.
  2. Dış bağlantı noktalarını sunucuları yerel IP adresi ve bağlantı noktası (varsayılan RDP bağlantı noktası) 3386 için yeniden yönlendirme, hizmeti –, bu durumda "AppVM01 RDP" daha önce oluşturduğunuz uygun hizmet nesnesi kullanın. Bu kuralı AppVM01 RDP erişimi içindir.
  3. Hedef – olmalıdır *yerel bağlantı noktası için güvenlik duvarında*, "DHCP 1 yerel IP" ya da statik IP kullanırken, eth0. Bir sıra numarası (eth0, eth1 vb.) ağ aletiniz birden çok yerel arabirimi varsa farklı olabilir. Bu güvenlik duvarı olup göndermeden gelen bağlantı noktasıdır (alıcı bağlantı noktası aynı olabilir), gerçek yönlendirilmiş hedef hedef liste alandır.
  4. Yeniden yönlendirme – Bu bölümde, sanal gerecin sonuçta bu trafiğin yönlendirileceği söyler. Basit Yönlendirme IP ve bağlantı noktası (isteğe bağlı) hedef listesinin alanında yerleştirmektir. Hiçbir bağlantı noktası kullanılırsa, hedef bağlantı noktası gelen istekte olacaktır (IE çevirisi yok), bir bağlantı noktası belirtilen bağlantı noktası, kullanılan NAT yanı sıra IP adresi de olacaktır.
     
     ![RDP Güvenlik Duvarı][11]
     
     Toplam dört RDP kuralları oluşturulması gerekir: 
     
     | Kural Adı | Sunucu | Hizmet | Hedef listesi |
     | --- | --- | --- | --- |
     | RDP IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
     | RDP DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
     | RDP AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
     | RDP AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

> [!TIP]
> Kaynak ve hizmet alanların kapsamını daraltmak daraltma saldırı yüzeyini de azaltır. İşlevselliği sağlayacak en sınırlı kapsamı kullanılmalıdır.
> 
> 

* **Uygulama trafik kuralları**: Ön uç web trafiği için ilk ve ikinci (ör. web sunucusu için veri katmanı) arka uç trafiği için iki uygulama trafik kuralları vardır. Bu kurallar ağ mimarisine (burada sunucularınızın yerleştirilir) bağlıdır ve trafik akışları (hangi yönde trafik akışı ve bağlantı noktalarını kullanılır).
  
    Önce ele alınan web trafiği için ön uç kuralıdır:
  
    ![Web güvenlik duvarı][12]
  
    Bu hedef NAT kuralı, uygulama tarafından sunucuya ulaşmak gerçek uygulama trafiğine izin verir. Diğer kurallardan izin verirken, güvenlik, yönetim, vb. için uygulamanın hangi dış kullanıcıları veya hizmetler uygulamaları erişmeye izin kurallardır. Bu örnekte, bağlantı noktası 80 üzerinde bir tek bir web sunucusu olduğundan, bu nedenle tek uygulama güvenlik duvarını gelen trafiğe web sunucuları iç IP adresi için dış IP yönlendirilecek.
  
    **Not**: hedef listesinin alanına atanan bağlantı noktası yok, bu nedenle yeniden yönlendirmesi web sunucusunun gelen bağlantı noktası 80'i (veya seçili hizmeti için 443) kullanılacak. Web sunucusu farklı bir bağlantı noktasında dinliyorsa, örneğin 8080 bağlantı noktası, 10.0.1.4:8080 de bağlantı noktası yönlendirmeye izin vermek için hedef listesi alanı güncelleştirilemedi.
  
    Sonraki uygulama trafik kuralı AppVM01 sunucu (ancak değil AppVM02) konuşmak Web sunucusu herhangi bir hizmeti izin vermek için arka uç kuralı şöyledir:
  
    ![Güvenlik Duvarı AppVM01 kuralı][13]
  
    Bu geçişi kural her IIS sunucusu AppVM01 ulaşmak için ön uç alt ağda sağlar (IP adresi 10.0.2.5) herhangi bir bağlantı üzerinde web uygulaması tarafından gerekli verilere herhangi bir protokolünü kullanarak.
  
    Bu ekran görüntüsünde, bir "\<dest açık\>" hedef alanı 10.0.2.5 hedef olarak belirtmek için kullanılır. Bu şunlardan biri olabilir gösterildiği veya açık adlı ağ (DNS sunucusu için Önkoşullar'da yapıldığı gibi) nesne. Hangi yöntemi kullanılacak Güvenlik Duvarı'nın yöneticisine kalmıştır budur. İlk boş satırda altında 10.0.2.5 bir işinizi Desitnation eklemek için çift \<dest açık\> ve açılır pencerede adresini girin.
  
    Bağlantı yöntemi "No SNAT" için ayarlanabilir bu nedenle bu iç trafik olduğundan geçirmek bu kural, NAT gereklidir.
  
    **Not**: Bu kural kaynak ağdan herhangi bir kaynağıdır yalnızca olacaksa bir veya web sunucuları, bilinen belirli sayıda ön uç alt ağını bir ağ nesnesine kaynak tüm ön uç alt ağını yerine tam bu IP adreslerini daha özel olarak oluşturulabilir.

> [!TIP]
> Bu kural hizmeti "Tüm" örnek uygulama kurulumu ve kullanımı kolay hale getirmek için kullanır, bu da Icmpv4 izin verir (ping) içinde tek bir kural. Ancak, bu önerilen bir yöntem değildir. Bağlantı noktaları ve protokoller ("Hizmetler") bu sınırında saldırı yüzeyini azaltmak, uygulama işlemi sağlayan en olası daraltıldığı.
> 
> 

<br />

> [!TIP]
> Bu kural bir hedef açık başvuru kullanılan gösterir, ancak tutarlı bir yaklaşım güvenlik duvarı yapılandırmasında kullanılması gerekir. Adlandırılmış ağ nesnesi boyunca daha kolay okunabilirlik ve desteklenebilirlik için kullanılması önerilir. Açık-dest burada yalnızca bir alternatif başvuru yöntemi göstermek için kullanılır ve (özellikle karmaşık yapılandırmalar için) genellikle önerilmez.
> 
> 

* **Internet kuralı için giden**: Bu geçişi kural seçilen hedef ağlara geçirilecek herhangi bir kaynak ağa gelen trafiğe izin verir. Bu kural, Barracuda NextGen güvenlik duvarı genellikle zaten varsayılan kuralı, ancak devre dışı durumda. Bu kurala sağ tıklayın, etkinleştirme kuralı komut erişebilirsiniz. Burada gösterilen kural, bu kural kaynak özniteliği için bu belgenin önkoşul bölümüne başvurular olarak oluşturulan iki yerel alt ağ eklemek için değiştirildi.
  
    ![Giden güvenlik duvarı kuralı][14]
* **DNS kuralı**: Bu geçişi kural yalnızca DNS sunucusuna geçirilecek DNS (bağlantı noktası 53) trafiğine izin verir. Bu kural, arka uca ön uç çoğu trafiği engellenir ve bu ortam için DNS özellikle sağlar.
  
    ![DNS güvenlik duvarı][15]
  
    **Not**: Bu ekranda görüntüsü bağlantı yöntemini dahil edilir. Bu kural, iç IP adresi trafik için iç IP için olduğundan, hiçbir NATing gerekli değildir, bu bağlantı yöntemi için bu geçişi kuralı "No SNAT" için ayarlanır.
* **Alt ağ için alt kural**: Etkin ve herhangi bir sunucu ön uç alt ağda bulunan herhangi bir sunucuya bağlanmak için arka uç alt ağında izin vermek için değiştirilmiş bir varsayılan kural bu geçişi kuralıdır. Bu kural tüm iç trafik olduğundan bağlantı yöntemi için Hayır SNAT ayarlanabilir.
  
    ![Güvenlik Duvarı içi-sanal ağ kuralı][16]
  
    **Not**: İki yönlü onay kutusunu işaretli (veya çoğu kurallarında işaretli değil), bu "tek yönlü" kural sağlar, bu kural için önemli budur, ön uç ağ ancak tersine arka uç alt ağından bir bağlantı başlatılabilir. Bu kural, bu onay kutusunu işaretlediyseniz istenmiyorsa, bizim mantıksal diyagramı yönlü trafik etkinleştirebilirsiniz.
* **Reddetme tüm trafik kuralı**: Bu her zaman son kuralı (öncelik) açısından olmalıdır ve trafik akışları, bu nedenle bu kural tarafından bırakılır önceki kurallardan herhangi birinin eşleştirmek başarısız olur. Bu varsayılan kural ve genellikle etkin, herhangi bir değişiklik genellikle gereklidir. 
  
    ![Güvenlik duvarı kuralı Reddet][17]

> [!IMPORTANT]
> Yukarıdaki kurallarının tümü oluşturulduktan sonra trafiğe izin veya istediğiniz gibi reddedildi emin olmak için her bir kural önceliklerini gözden geçirilmesi önemlidir. Bu örnekte, Barracuda Yönetimi istemcisi'nde kuralları iletme, ana kılavuzunda görünmelidir sırada kurallardır.
> 
> 

## <a name="rule-activation"></a>Kural etkinleştirme
Mantıksal diyagramı belirtimi değiştiren ruleset ile ruleset karşıya için Güvenlik Duvarı'nı ve ardından etkinleştirilmelidir.

![Güvenlik duvarı kuralını etkinleştirme][18]

Üst sağ köşesindeki management istemcisi içinde düğme kümelerdir. Güvenlik Duvarı değiştirilmiş kuralları göndermek için "Değişiklikleri Gönder" düğmesine tıklayın ve ardından "Etkinleştir" düğmesine tıklayın.

Bu örnek ortam derleme güvenlik duvarı kural kümesi etkinleştirme'yle tamamlanmıştır.

## <a name="traffic-scenarios"></a>Trafik senaryoları
> [!IMPORTANT]
> Bir anahtar takeway unutmayın olmaktır **tüm** trafiği, güvenlik duvarı üzerinden gelen. Bu nedenle IIS01 sunucuya Uzak Masaüstü için ön uç bulut hizmeti ve ön uç alt olmasına rağmen bu sunucuya erişmek için biz RDP için güvenlik duvarında bağlantı noktası 8014 gerekir ve ardından RDP isteği IIS01 RDP Por için dahili olarak yönlendirmek Güvenlik Duvarı'nı izin t. Azure portalının "Bağlan" düğmesi olduğundan IIS01 için doğrudan RDP yol yok (portal görebilirsiniz kadar) işe yaramaz. Başka bir deyişle, İnternet'ten gelen tüm bağlantıları, güvenlik hizmetleri ve bağlantı noktası, örneğin secscv001.cloudapp.net:xxxx olacaktır.
> 
> 

Bu senaryolar için aşağıdaki güvenlik duvarı kuralları koşulların karşılanması:

1. Güvenlik Duvarı Yönetimi
2. RDP için IIS01
3. RDP için DNS01
4. RDP için AppVM01
5. RDP için AppVM02
6. Web uygulaması trafiğini
7. AppVM01 uygulama trafiği
8. İnternet'e giden
9. DNS01 ön ucu
10. İçi alt ağ trafiği (ön uca arka uç)
11. Tümünü Reddet

Gerçek güvenlik duvarı kural kümesi, büyük olasılıkla bunlara ek olarak birçok diğer kurallar vardır, verilen herhangi bir güvenlik duvarı kuralları burada listelenenlerden farklı önceliği sayılar da sahip olursunuz. On bu kurallar ve göreli önceliğini bunları arasından arasında ilgi düzeyi sağlamak için bu listeyi ve ilişkili sayılardır. Diğer bir deyişle; Kural numarası 5 olabilir, ancak "Güvenlik duvarı Yönetimi" kuralıdır ve "RDP için DNS01" kuralı amacıyla yapılıyorsa bu liste hizalayın sürece, gerçek güvenlik duvarında "RDP için IIS01" olabilir. Listenin içinde de yardımcı olur aşağıdaki senaryoları kısaltma; izin verme Örneğin "FW kuralı 9 (DNS)". Ayrıca konuyu uzatmamak amacıyla, dört RDP kuralları topluca çağrılır, "RDP kuralları" trafiği senaryo olduğunda RDP'den ilgisiz.

Ayrıca, ağ güvenlik grupları yerinde geri çağırma ön uç ve arka uç alt ağlarda gelen internet trafiği için.

#### <a name="allowed-internet-to-web-server"></a>(İzin verilir) Web sunucusuna Internet
1. Internet kullanıcı istekleri HTTP sayfasına SecSvc001.CloudApp.Net (Internet'e yönelik bulut hizmeti)
2. Bulut hizmeti geçiş trafiği 80 numaralı bağlantı noktasında güvenlik duvarı arabirimine 10.0.0.4:80 açık uç noktası
3. Güvenlik alt ağına atanmış hiçbir NSG, bunu sistemi NSG kuralları, güvenlik duvarı trafiğe izin vermek
4. Trafik (10.0.1.4) Güvenlik Duvarı'nın iç IP adresi ile denk gelir.
5. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralları 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
   3. FW kural 6 (uygulama: Web) uygulamak için trafiğe izin verilir, güvenlik duvarı NAT 10.0.1.4 (IIS01) için
6. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kural 1'i (blok Internet) geçerli değildir (Bu trafiği olan NAT güvenlik duvarı tarafından gerekir, böylece kaynak adresi artık güvenlik alt ağda bulunan güvenlik duvarı ve "yerel" trafiği olarak ön uç alt ağ NSG'SİNDE tarafından görülen ve bu nedenle izin), sonraki kural Taşı
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
7. IIS01 web trafiği, bu isteği alır ve isteği işlemeye başlar.
8. IIS01 girişimleri AppVM01 arka uç alt ağı üzerinde bir FTP oturumu başlatır
9. Sonraki atlama güvenlik duvarı, UDR rotaya ön uç alt ağını yapar
10. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
11. Güvenlik duvarı kuralı işleme başlar:
    1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
    2. FW Kural 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
    3. FW kural 6 (uygulama: Web) değil, uygulamak için sonraki kural Taşı
    4. FW kural 7 (uygulama: Arka uç) geçerli, güvenlik duvarı 10.0.2.5 (AppVM01) trafiğini trafiğe izin verilir
12. Arka uç alt ağı gelen kuralı işleme başlar:
    1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
13. AppVM01 isteği alır ve oturumu başlatır ve yanıtlar
14. Sonraki atlama güvenlik duvarı, UDR rotaya arka uç alt ağı yapar
15. Yanıta izin arka uç alt ağı giden hiçbir NSG kuralları olduğundan
16. Bu yerleşik bir oturum üzerinde trafiği döndürüyor olduğundan güvenlik yanıtı web sunucusu (IIS01) geçer.
17. Ön uç alt ağını gelen kuralı işleme başlar:
    1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
18. IIS sunucusu yanıtı alır, AppVM01 ile hareketi tamamlar ve ardından HTTP yanıtı oluşturma tamamlandıktan, bu HTTP yanıtı istek sahibine gönderilir
19. Yanıta izin verilen ön uç alt ağını giden hiçbir NSG kuralları olduğundan
20. HTTP yanıtı İsabetleri güvenlik duvarı ve güvenlik duvarı tarafından kabul edilir, çünkü bu yerleşik bir NAT oturum yanıtı
21. Güvenlik Duvarı yanıt Internet kullanıcı ardından yönlendirir.
22. Olduğundan Hayır giden NSG kuralları veya UDR atlama yanıta izin verilen ön uç alt ağda bulunan ve Internet kullanıcı web sayfasının alır.

#### <a name="allowed-internet-rdp-to-backend"></a>(İzin verilir) Internet'i arka uca RDP
1. İnternet üzerinde Sunucu Yöneticisi aracılığıyla SecSvc001.CloudApp.Net:8025 8025 "RDP için AppVM01" güvenlik duvarı kuralı kullanıcı tarafından atanan bağlantı noktası numarasını olduğu, AppVM01 RDP oturumu istekleri
2. Bulut hizmeti güvenlik duvarı 10.0.0.4:8025 arabirimdeki 8025 bağlantı noktasında trafiği üzerinden açık uç noktasını geçirir
3. Güvenlik alt ağına atanmış hiçbir NSG, bunu sistemi NSG kuralları, güvenlik duvarı trafiğe izin vermek
4. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW Kural 2'de (RDP IIS) değil, uygulamak için sonraki kural taşıma
   3. FW kural 3 (RDP DNS01) değil, uygulamak için sonraki kural taşıma
   4. FW kural 4 (RDP AppVM01) uygulamak için trafiğe izin verilir, güvenlik duvarı NAT 10.0.2.5:3386 kendisine (AppVM01 üzerinde RDP noktasına)
5. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kural 1'i (blok Internet) geçerli değildir (Bu trafiği olan NAT güvenlik duvarı tarafından gerekir, böylece kaynak adresi artık güvenlik alt ağda bulunan güvenlik duvarı ve "yerel" trafiği olarak arka uç alt ağ NSG'SİNDE tarafından görülen ve bu nedenle izin), sonraki kural Taşı
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
6. AppVM01 RDP trafiği için dinleme ve yanıtlar
7. Giden hiçbir NSG kuralları ile varsayılan kuralları uygulanır ve dönüş trafiğine izin verilir
8. UDR güvenlik duvarı giden trafiği, sonraki atlama olarak yönlendirir.
9. Bu yerleşik bir oturum üzerinde trafiği döndürüyor olduğundan güvenlik yanıtı internet kullanıcı geçer.
10. RDP oturumu etkin
11. AppVM01 kullanıcı adı ve parolasını ister.

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) DNS sunucusu üzerinde Web sunucusu DNS araması
1. Sunucu, IIS01 www.data.gov bir veri akışı gereksinimlerini, ancak adresini çözümlemek için gereksinimleri web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağında 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS01 için DNS isteği gönderir
3. UDR güvenlik duvarı giden trafiği, sonraki atlama olarak yönlendirir.
4. Giden hiçbir NSG kuralları ön uç alt ağına bağlı olan, trafiğe izin verilir
5. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW Kural 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
   3. FW kuralları 6 ve 7 (uygulama kuralları) yoksa uygulama, sonraki kural Taşı
   4. FW kural 8 (Internet için) değil, uygulama sonraki kural Taşı
   5. FW kural 9 (DNS) geçerli, güvenlik duvarı 10.0.2.4 (DNS01) trafiğini trafiğe izin verilir
6. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
7. DNS sunucusu isteği alır
8. DNS sunucusu önbelleğe alınmış adresleriniz değil ve bir kök DNS sunucusu internet'te sorar
9. UDR güvenlik duvarı giden trafiği, sonraki atlama olarak yönlendirir.
10. Giden hiçbir NSG kuralları arka uç alt ağı üzerinde trafiğe izin verilir
11. Güvenlik duvarı kuralı işleme başlar:
    1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
    2. FW Kural 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
    3. FW kuralları 6 ve 7 (uygulama kuralları) yoksa uygulama, sonraki kural Taşı
    4. FW kural 8 (Internet için) geçerlidir, trafiğe izin verilir, oturumu kök DNS sunucusu Internet'teki kullanıma SNAT kullanmaktır
12. Bu oturum, Güvenlik Duvarı'ndan başlatıldıktan sonra Internet DNS sunucusu yanıt, yanıt güvenlik duvarı tarafından kabul edilir
13. Bu yerleşik bir oturum olduğundan, güvenlik duvarı yanıt DNS01 başlatma sunucusuna iletir.
14. Arka uç alt ağı gelen kuralı işleme başlar:
    1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
15. DNS sunucusu alır ve yanıtı önbelleğe alır ve ardından geri IIS01 ilk isteğine yanıt verir
16. Sonraki atlama güvenlik duvarı, UDR rotaya arka uç alt ağı yapar
17. Arka uç alt ağda mevcut giden hiçbir NSG kuralları trafiğe izin verilir
18. Bu yerleşik bir oturum için güvenlik duvarında, yanıt, güvenlik duvarı IIS sunucuya geri iletilir
19. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
    2. Trafiğe izin verilmesi bu trafiğe izin alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı
20. IIS01 DNS01 yanıtı alır

#### <a name="allowed-backend-server-to-frontend-server"></a>(İzin verilir) Ön uç sunucusu için arka uç sunucusu
1. RDP aracılığıyla AppVM02 oturum açmış bir yönetici, doğrudan windows dosya Gezgini aracılığıyla IIS01 sunucusundan bir dosyayı ister.
2. Sonraki atlama güvenlik duvarı, UDR rotaya arka uç alt ağı yapar
3. Yanıta izin arka uç alt ağı giden hiçbir NSG kuralları olduğundan
4. Güvenlik duvarı kuralı işleme başlar:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW Kural 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
   3. FW kuralları 6 ve 7 (uygulama kuralları) yoksa uygulama, sonraki kural Taşı
   4. FW kural 8 (Internet için) değil, uygulama sonraki kural Taşı
   5. FW kural 9 (DNS) değil, uygulamak için sonraki kural taşıma
   6. FW kural 10 (içi alt ağ) geçerli, trafiğe izin verilir, güvenlik duvarı trafiği için 10.0.1.4 (IIS01) geçirir.
5. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
   2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
6. Uygun kimlik doğrulaması ve yetkilendirme varsayıldığında, IIS01 isteği kabul eder ve yanıt verir
7. Sonraki atlama güvenlik duvarı, UDR rotaya ön uç alt ağını yapar
8. Yanıta izin verilen ön uç alt ağını giden hiçbir NSG kuralları olduğundan
9. Şu güvenlik duvarı var olan bir oturum olduğundan, bu yanıt izin ve güvenlik duvarı AppVM02 yanıtı döndürür
10. Arka uç alt ağı gelen kuralı işleme başlar:
    1. NSG kuralı 1 (blok Internet) değil, uygulamak için sonraki kural taşıma
    2. Varsayılan NSG kuralları alt ağ için alt ağ trafiğine izin, trafiğe izin verilir, NSG kuralının işlenmesi Durdur
11. AppVM02 yanıtı alır

#### <a name="denied-internet-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Internet
1. Internet kullanıcı FrontEnd001.CloudApp.Net hizmet aracılığıyla IIS01, web sunucusuna erişmeye çalışır
2. Açık HTTP trafiği için uç nokta olduğundan bu bulut hizmeti aracılığıyla geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağda NSG (blok Internet) bu trafiği engeller
4. Son olarak, ön uç alt ağı UDR yol, sonraki atlama olarak güvenlik duvarı, IIS01 tüm giden trafiği gönderir ve Güvenlik Duvarı bu asimetrik trafiği görebilir ve giden yanıt savunma hattı en az üç bağımsız Katmanlar vardır Thus bırak İnternet'e ve yetkisiz/uygunsuz erişimi engelleme, bulut hizmeti aracılığıyla IIS01.

#### <a name="denied-internet-to-backend-server"></a>(Reddedildi) Internet'e arka uç sunucusu
1. Internet kullanıcı BackEnd001.CloudApp.Net hizmet aracılığıyla AppVM01 bir dosyaya erişmeye çalışır
2. Dosya Paylaşımı için açık uç nokta olduğundan bu bulut hizmeti geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, bu trafiğin NSG (blok Internet) engeller
4. Son olarak, UDR yol, sonraki atlama olarak güvenlik duvarı, AppVM01 tüm giden trafiği gönderir ve Güvenlik Duvarı bu asimetrik trafiği görebilir ve böylece savunma hattı internet arasındaki en az üç bağımsız Katmanlar vardır giden yanıt bırak ve AppVM01 yetkisiz/uygunsuz erişimi engelleme, bulut hizmeti aracılığıyla.

#### <a name="denied-frontend-server-to-backend-server"></a>(Reddedildi) Arka uç sunucusu için ön uç sunucusu
1. IIS01 güvenliğinin aşıldığını ve arka uç alt ağı sunucuları için tararken kötü amaçlı bir kodun çalıştığını varsayar.
2. Ön uç alt ağı UDR rota tüm giden trafiği IIS01 güvenlik duvarı sonraki atlama olarak gönderir. Bu tehlikeye giren bir VM tarafından değiştirilebilecek bir sorun değildir.
3. İstek AppVM01 veya trafiğe güvenlik duvarı (FW kuralları 7 ve 9) nedeniyle izin verilmesi DNS araması için DNS sunucusu varsa güvenlik duvarının trafiği işlem. FW kural 11 göre (tüm Reddet) diğer tüm trafik engellenir.
4. Gelişmiş tehdit algılama, Güvenlik Duvarı'nı etkinleştirildi (da değil kapsanan bu belgede, Gelişmiş tehdit özellikleri belirli ağ aletiniz için satıcının belgelerine bakın), hatta temel iletme kuralı tarafından izin verilecek trafiği Bu konuda bahsedilen belge engelleyen trafiği bilinen imza veya Gelişmiş tehdit kural bayrak desenleri içeriyorsa.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Reddedildi) DNS sunucusundaki Internet DNS Arama
1. Internet kullanıcı DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını arama dener 
2. DNS trafiği için açık uç nokta olduğundan bu bulut hizmeti aracılığıyla geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağda NSG kuralı (blok Internet) bu trafiği engeller
4. Son olarak, arka uç alt ağı UDR yol, sonraki atlama olarak güvenlik duvarı, DNS01 tüm giden trafiği gönderir ve Güvenlik Duvarı bu asimetrik trafiği görebilir ve giden yanıt savunma hattı en az üç bağımsız Katmanlar vardır Thus bırak internet ve yetkisiz/uygunsuz erişimi engelleme, bulut hizmeti aracılığıyla DNS01.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Reddedildi) Internet için güvenlik duvarı üzerinden SQL erişimi
1. Internet kullanıcı SQL veri SecSvc001.CloudApp.Net (Internet'e yönelik bulut hizmeti) istekleri
2. SQL için açık uç nokta olduğundan bu bulut hizmeti geçecekse değildir ve güvenlik duvarı ulaşın mıydı
3. Bazı nedenlerden dolayı SQL uç noktaları açma, güvenlik duvarı kuralı işlemeye başlamak:
   1. FW kural 1'i (FW Mgmt) değil, uygulamak için sonraki kural taşıma
   2. FW kuralları 2-5 (RDP kuralları) yok, uygulamak için sonraki kural taşıma
   3. FW kural 6 ve 7 (uygulama kuralları) yoksa uygulama, sonraki kural Taşı
   4. FW kural 8 (Internet için) değil, uygulama sonraki kural Taşı
   5. FW kural 9 (DNS) değil, uygulamak için sonraki kural taşıma
   6. FW kural 10 (içi alt ağ) değil, uygulamak için sonraki kural Taşı
   7. FW kural 11 (Tümünü Reddet) geçerli, trafik engellenmiş, Dur kural işleme

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırma
Bir PowerShell komut dosyasında tam komut dosyasını kaydedin. Ağ Yapılandırması "NetworkConf2.xml" adlı bir dosyaya kaydedin.
Kullanıcı tanımlı değişkenler, gerektiği gibi değiştirin. Betiği çalıştırın ve ardından yukarıdaki güvenlik duvarı kuralı kurulum yönergeleri izleyin.

#### <a name="full-script"></a>Tam betik
Bu betik, kullanıcı tanımlı değişkenleri esas alarak olur:

1. Bir Azure aboneliğine Bağlanma
2. Yeni depolama hesabı oluşturma
3. Yeni bir sanal ağ ve ağ yapılandırma dosyasında tanımlanan üç alt ağ oluşturma
4. Beş sanal makine oluşturun; Güvenlik Duvarı'nı 1 ve 4 windows server Vm'leri
5. UDR dahil olmak üzere yapılandırın:
   1. İki yeni rota tabloları oluşturma
   2. Yol tablolarına ekleme
   3. Tablolar için uygun alt ağları bağlama
6. NVA üzerindeki IP iletmeyi etkinleştirin
7. NSG dahil olmak üzere yapılandırın:
   1. Bir NSG oluşturma
   2. Bir kural ekleme
   3. NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bilgisayar veya sunucu bir İnternet'e bağlı yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu betik çalıştırıldığında, uyarı veya PowerShell'de pop diğer bilgilendirme iletileri olabilir. Yalnızca hata iletileri kırmızı endişeye neden olan.
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
        Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
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
Güncelleştirilmiş konumu ile bu xml dosyasını kaydedin ve bağlantıyı yukarıdaki betik $NetworkConfigFile değişkeninde bu dosyaya ekleyin.

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
Bu ve diğer DMZ örnekleri için örnek uygulamayı yüklemek istiyorsanız, aşağıdaki bağlantıda bir sağlanmıştır: [Örnek uygulama betiği][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "UDR NVA ve NSG ile iki yönlü DMZ"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Güvenlik duvarı kurallarını mantıksal görünümü"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Bir ön uç ağ nesnesi oluşturun"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Bir DNS sunucusu nesnesi oluşturun"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Varsayılan RDP kuralının kopyalama"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 kuralı"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Uygulama yeniden yönlendirme simgesi"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Hedef NAT simgesi"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Geçişi simgesi"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Güvenlik Duvarı Yönetimi"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "RDP Güvenlik Duvarı"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Web güvenlik duvarı"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Güvenlik Duvarı AppVM01 kuralı"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Giden güvenlik duvarı kuralı"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "DNS güvenlik duvarı"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Güvenlik Duvarı içi-sanal ağ kuralı"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Güvenlik duvarı kuralı Reddet"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Güvenlik duvarı kuralını etkinleştirme"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
