---
title: Çevre ağ örnek – çevre ağına bir güvenlik duvarı, UDR ve Nsg'ler oluşan koruma ağlarla | Microsoft Docs
description: Çevre ağı (DMZ olarak da bilinir) bir güvenlik duvarı, kullanıcı tanımlı yönlendirme (UDR) ve ağ güvenlik grupları (Nsg'ler) ile oluşturun.
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
ms.openlocfilehash: 0a7927868a9a4bebc80ec995baefbae4c45d747f
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410470"
---
# <a name="example-3-build-a-perimeter-network-to-protect-networks-with-a-firewall-udr-and-nsgs"></a>Örnek 3: Ağları bir güvenlik duvarı, UDR ve Nsg'ler ile korunacak bir çevre ağı oluşturma

[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

Bu örnekte, oluşturduğunuz bir çevre ağında (DMZ, sivil bölge ve denetimli alt ağ da bilinir). Örneğin, bir güvenlik duvarı, dört Windows sunucuları, kullanıcı tanımlı yönlendirme (UDR), IP iletme ve ağ güvenlik grupları (Nsg'ler) uygular. Bu makalede her her adımda daha derin bir anlayış sağlamak için ilgili komutları gösterilmektedir. Trafik senaryo bölümünde de trafiği çevre ağındaki savunma katmanları ile nasıl devam eder ayrıntılı olarak açıklanmaktadır. Son olarak, başvurular bölümüne, tüm kod ve bu ortamı test edin ve çeşitli senaryolarıyla denemeler oluşturmak için yönergeler içerir.

![UDR NVA ve NSG ile iki yönlü çevre ağı][1]

## <a name="environment-setup"></a>Ortam kurulumu

Bu örnekte bir abonelik aşağıdaki bileşenleri içerir:

* Üç bulut Hizmetleri: SecSvc001 FrontEnd001 ve BackEnd001
* Üç alt ağ ile sanal ağ (CorpNetwork): SecNet, ön uç ve arka uç
* Bir ağ sanal Gereci: bir güvenlik duvarı SecNet alt ağa bağlı
* Uygulama web sunucusunu temsil eden bir Windows sunucusu: IIS01
* Uygulama arka uç sunucuları temsil eden iki Windows sunucuları: AppVM01, AppVM02
* Bir DNS sunucusunu temsil eden bir Windows sunucusu: DNS01

[Başvurduğu bölüm](#references) burada açıklanan ortam çoğunu oluşturan bir PowerShell Betiği içerir. Bu makalede, aksi halde sanal ağlar ve sanal makineleri (VM'ler) oluşturmaya yönelik ayrıntılı yönergeler vermez.

Ortamı oluşturmak için:

1. Dahil ağ yapılandırma XML dosyasını kaydedin [başvuru bölümüne](#references). Ad, konum ve eşleşen verilen senaryo için IP adresi ile güncelleştirmeniz gerekir.
1. Kullanıcı değişkenleri, belirli bir ortama (örneğin, abonelik, hizmet adlarını ve benzeri) eşleşmesi için tam komut güncelleştirin.
1. PowerShell Betiği yürütün.

> [!NOTE]
> PowerShell komut dosyasında belirtilen bölge ağ yapılandırma XML dosyasında belirtilen bölge eşleşmelidir.

Betik başarıyla çalıştırıldıktan sonra aşağıdaki adımları uygulayın:

1. Güvenlik duvarı kurallarını ayarlayın. Bkz: [güvenlik duvarı kuralları](#firewall-rules) bölümü.
1. İsteğe bağlı olarak, bu DMZ yapılandırmanın testi izin vermek için bir web uygulamasını oluşturan uygulama sunucusu ve web sunucusu için başvurular bölümünde iki betiklerini kullanın.

## <a name="user-defined-routing"></a>Kullanıcı tanımlı yönlendirme

Varsayılan olarak, aşağıdaki sistem yolları olarak tanımlanır:

    Effective routes :
     Address Prefix    Next hop type    Next hop IP address Status   Source
     --------------    -------------    ------------------- ------   ------
     {10.0.0.0/16}     VNETLocal                            Active   Default
     {0.0.0.0/0}       internet                             Active   Default
     {10.0.0.0/8}      Null                                 Active   Default
     {100.64.0.0/10}   Null                                 Active   Default
     {172.16.0.0/12}   Null                                 Active   Default
     {192.168.0.0/16}  Null                                 Active   Default

VNETLocal her zaman belirli bir sanal ağ için tanımlanan adres ön şeklindedir. Örneğin, belirli bir sanal ağın nasıl tanımlandığını bağlı olarak sanal ağ için sanal ağdan değişecektir. Kalan sistem yolları statiktir ve varsayılan olarak görüntülenir.

Öncelikli olduğu gibi en uzun ön ek eşleşmesi (LPM) yöntemiyle yollar işlenir. Bu nedenle en belirli bir yol tablosundaki belirli hedef adresi için geçerlidir.

Bu nedenle, bir sunucu gibi DNS01 yönelik trafiği (10.0.2.4) yerel ağ (10.0.0.0/16) nedeniyle 10.0.0.0/16 rota sanal ağ üzerinden yönlendirilir.  10.0.2.4 için 10.0.0.0/16 yol en belirgin yoldur. Bu kural, 0.0.0.0/0 ve 10.0.0.0/8 de ilgili olsa bile geçerlidir. Ancak, daha az belirli kullanıcılar bu trafiği etkilemez. 10.0.2.4 trafiğini yerel sanal ağ için hedefe yönlendirilir, sonraki atlama olarak sahiptir.

Örneğin, 10.0.0.0/16 rota 10.1.1.1 için hedeflenen trafiği için geçerli değildir. Sonraki atlama Null olduğundan 10.0.0.0/8 sistem yolu en belirgin trafik bırakıldı ya da "siyah holed" olur.

Hedef herhangi bir Null ön ekleri veya VNETLocal ön ekler için geçerli değilse, trafiğin az belirli bir yol (0.0.0.0/0) izler. İnternet'e sonraki atlama olarak ve Azure'nın internet ucuna dışında yönlendirilmeden.

Rota tablosunda iki özdeş bir önek varsa, tercih sırasına göre rotanın kaynak öznitelikte dayanır:

1. VirtualAppliance: Bir kullanıcı tanımlı yol tablosu için el ile eklenen.
1. VPNGateway: Bir dinamik yol (hibrit ağlar ile kullanıldığında BGP) dinamik ağ protokolü tarafından eklendi. Dinamik iletişim kuralı otomatik olarak eşlenen ağ değişiklikleri yansıtan gibi bu yolları zaman içinde değişebilir.
1. Varsayılan: Sistem yolları, yerel sanal ağ ve statik girdilerini yukarıdaki rota tablosunda gösterilen şekilde.

> [!NOTE]
> Artık kullanıcı tanımlı yönlendirme (UDR) ExpressRoute ve VPN ağ geçitleri ile giden ve gelen şirketler arası trafik bir ağ sanal Gereci (NVA) yönlendirilmesini zorlamak için kullanabilirsiniz.

### <a name="create-local-routes"></a>Yerel yollar oluşturma

Bu örnek, her biri ön uç ve arka uç alt ağları için iki yönlendirme tablolarını kullanır. Her tabloda belirtilen alt ağ için uygun statik yollar ile yüklenir. Bu örneğin amacı doğrultusunda, her tablo üç yol vardır:

1. Yerel alt ağ trafiği ile tanımlanan herhangi bir sonraki atlama. Bu yol, yerel alt ağ trafiği, güvenlik duvarını geçmesine izin verir.
2. Güvenlik Duvarı tanımlanan bir sonraki atlama ile sanal ağ trafiği. Bu yol, doğrudan yönlendirmek yerel sanal ağ trafiğine izin veren varsayılan kuralı geçersiz kılar.
3. Tüm kalan trafiği (0/0) güvenlik duvarı olarak tanımlanan bir sonraki atlama.

Yönlendirme tablolarını oluşturulduktan sonra kendi alt ağlara bağlanır. Ön uç alt ağına yönlendirme tabloda gibi görünmelidir:

    Effective routes :
     Address Prefix    Next hop type       Next hop IP address  Status   Source
     --------------    ------------------  -------------------  ------   ------
     {10.0.1.0/24}     VNETLocal                                Active
     {10.0.0.0/16}     VirtualAppliance    10.0.0.4             Active
     {0.0.0.0/0}       VirtualAppliance    10.0.0.4             Active

Bu örnekte, rota tablosunu, kullanıcı tanımlı bir yol ekleyin ve sonra yönlendirme tablosunu bir alt ağa bağlamak için aşağıdaki komutları kullanır. İle başlayan öğeleri `$`, gibi `$BESubnet`, komut dosyası başvurusu bölümündeki kullanıcı tanımlı değişkenler.

1. İlk olarak, temel bir yönlendirme tablosu oluşturun. Aşağıdaki kod parçacığı, arka uç alt ağı için bir tablo oluşturur. Tam komut dosyası ayrıca için ön uç alt ağına karşılık gelen bir tablo oluşturur.

   ```powershell
   New-AzureRouteTable -Name $BERouteTableName `
       -Location $DeploymentLocation `
       -Label "Route table for $BESubnet subnet"
   ```

1. Rota tablosunu oluşturduktan sonra belirli kullanıcı tanımlı yollar ekleyebilirsiniz. Aşağıdaki kod parçacığını (0.0.0.0/0) tüm trafiği sanal gereç aracılığıyla yönlendirilir belirtir. Bir değişken `$VMIP[0]` sanal gereç komut dosyasındaki oluşturulduğunda atanan IP adresini geçirmek için kullanılır. Tam betik, ön uç tabloda karşılık gelen kuralı da oluşturur.

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. Önceki rota girişi varsayılan "0.0.0.0/0" yolu geçersiz kılar ancak varsayılan 10.0.0.0/16 kural doğrudan hedef ve ağ sanal gerecinin yönlendirmek için sanal ağ içindeki trafik yine de sağlar. Bu davranışı düzeltmek için aşağıdaki kuralı eklemeniz gerekir:

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
       -NextHopType VirtualAppliance `
       -NextHopIpAddress $VMIP[0]
   ```

1. Bu noktada, bir seçim yapmanız gerekir. İki önceki kurallar, tek bir alt ağ içinde trafiği de dahil olmak üzere, değerlendirme için Güvenlik Duvarı tüm trafiği yönlendirme. Bu davranış isteyebilirsiniz. Ancak, aksi takdirde, güvenlik duvarı katılımı yerel olarak yönlendirmek için bir alt ağ içinde trafiği izin verebilirsiniz. Üçüncü, doğrudan yerel alt ağ için hedeflenen herhangi bir adresine yönlendiren kuralı ekleyin (NextHopType VNETLocal =).

   ```powershell
   Get-AzureRouteTable $BERouteTableName | `
       Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
           -NextHopType VNETLocal
   ```

1. Son olarak, yönlendirme tablosu oluşturulur ve kullanıcı tanımlı yollar ile doldurulmuş sonra tabloyu bir alt ağa bağlamak gerekir. Aşağıdaki kod parçacığı, arka uç alt ağı için tablo bağlar. Tam betik, ayrıca ön uç alt ağına ön uç yol tablosuna bağlar.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
       -SubnetName $BESubnet `
       -RouteTableName $BERouteTableName
   ```

## <a name="ip-forwarding"></a>IP iletme

IP iletimi UDR için yardımcı özelliğidir. Bu ayar bir sanal gereç üzerinde gerecine gönderilmeyen trafiği alma ve ardından bu trafiğin ultimate hedefine iletecek şekilde sağlar.

Örneğin, UDR AppVM01 gelen trafiği DNS01 sunucuya istekte, güvenlik duvarı trafiği yönlendirir. Etkin IP iletimi ile DNS01 hedef (10.0.2.4) trafiği kabul güvenlik duvarı Gereci (10.0.0.4) ve ardından kendi (10.0.2.4) ultimate hedef iletilen. Güvenlik Duvarı'sonraki atlama olarak yol tablosuna sahip olsa da güvenlik duvarını etkin IP iletme olmadan gereç tarafından trafiği kabul edilmiyor.

> [!IMPORTANT]
> Kullanıcı tanımlı yönlendirme ile birlikte IP iletimini etkinleştirmeniz unutmayın.

IP iletimi tek bir komutla VM oluşturma sırasında etkinleştirilebilir. Güvenlik Duvarı sanal Gereci ve IP iletimini etkinleştirir VM örneği çağırırsınız. Şununla kırmızı öğeleri göz önünde bulundurun `$`gibi `$VMName[0]`, kullanıcı tanımlı değişkenler komut dosyası başvurusu bölümünde bu belgenin. Parantez içine sıfır `[0]`, VM'lerin dizideki ilk VM temsil eder. Örnek betiği değişiklik yapılmadan çalışmak güvenlik duvarı ilk sanal makine (VM 0) olmalıdır. Tam komut dosyasında sonlarında UDR komutlarla ilgili kod parçacığını gruplandırılır.

```powershell
Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
    Set-AzureIPForwarding -Enable
```

## <a name="network-security-groups"></a>Ağ güvenlik grupları

Bu örnekte, bir ağ güvenlik grubu (NSG) oluşturun ve ardından tek bir kural ile yükler. Örnek, ardından yalnızca ön uç ve arka uç alt ağlara (SecNet değil) NSG'ye bağlar. NSG içinde yük kural aşağıdaki gibidir:

* Tüm trafiği (tüm bağlantı noktaları) İnternet'ten gelen tüm sanal ağ (tüm alt ağlar) reddedildi

Nsg'leri Bu örnekte kullanılan olsa da, kendi ana amacı el ile gerçekleştirilen bir yanlış yapılandırma karşı savunma ikincil bir katmanı gibidir. İnternet'ten ön uç veya arka uç alt ağları için tüm gelen trafiği engellemek istiyorsunuz. Trafiği yalnızca SecNet alt ağa güvenlik duvarı aracılığıyla sonra yalnızca uygun trafiği ön uç veya arka uç alt ağları açın yönlendirilir akışı. Ayrıca, UDR kuralları ön uç veya arka uç alt ağları güvenlik duvarına ulaştığında tüm trafiği yönlendirme. Güvenlik Duvarı, asimetrik bir akış görür ve giden trafiğini bırakır.

Üç güvenlik katmanları, ön uç ve arka uç alt ağları koruma:

1. Bulut Hizmetleri ve FrontEnd001 BackEnd001 açık uç nokta yok
1. Nsg'ler trafik internet'ten Reddet
1. Güvenlik Duvarı asimetrik trafiğini bırakır

Bu örnekte NSG hakkında ilginç bir noktanın yalnızca bir kural, aşağıda gösterilen içermesidir. Bu kural güvenlik alt ağ da dahil olmak üzere tüm sanal ağ, internet trafiğini reddeder.

```powershell
Get-AzureNetworkSecurityGroup -Name $NSGName | `
    Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
    from the internet" `
    -Type Inbound -Priority 100 -Action Deny `
    -SourceAddressPrefix INTERNET -SourcePortRange '*' `
    -DestinationAddressPrefix VIRTUAL_NETWORK `
    -DestinationPortRange '*' `
    -Protocol *
```

NSG yalnızca ön uç ve arka uç alt ağa bağlı olduğundan, kuralın güvenlik alt ağa gelen trafik için uygulanmaz. Sonuç olarak, güvenlik alt ağa trafiği akar.

```powershell
Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $FESubnet -VirtualNetworkName $VNetName

Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
    -SubnetName $BESubnet -VirtualNetworkName $VNetName
```

## <a name="firewall-rules"></a>Güvenlik duvarı kuralları

Güvenlik Duvarı'nı iletme kurallarını oluşturmanız gerekir. Güvenlik Duvarı engeller veya tüm gelen, giden ve intra virtual ağ trafiğini nedeniyle çok sayıda güvenlik duvarı kuralları gerekir. Ayrıca güvenlik hizmeti genel IP adresini (temel, farklı bağlantı noktaları için) gelen tüm trafiği işlemek güvenlik duvarı sahiptir. Daha sonra kurtulabilir için alt ağları ve güvenlik duvarı kurallarını ayarlama önce mantıksal akışlarda diyagram tarafından en iyi izleyin. Aşağıdaki şekil, bu örnek için güvenlik duvarı kurallarını mantıksal bir görünümüdür:

![Güvenlik duvarı kurallarını mantıksal görünümü][2]

> [!NOTE]
> Yönetim bağlantı noktalarını ağ sanal gerecinin bağlı olarak değişir. Bu örnekte, bağlantı noktası 22 ve 801 807 kullanan bir Barracuda NextGen Firewall gösterilmektedir. Lütfen cihazı yönetmek için tam bağlantı noktalarını bulmak için gereç satıcı belgelerine bakın.

### <a name="logical-rule-description"></a>Mantıksal kural açıklaması

Güvenlik Duvarı bu alt ağda yalnızca kaynak olduğundan, yukarıdaki mantıksal diyagramda güvenlik alt göstermez. Güvenlik duvarı kuralları, bu diyagramda gösterilmiştir nasıl mantıksal olarak izin ver veya Reddet trafik akışları, ancak değil gerçek yol yönlendirilir. Ayrıca, son iki sekizlik tabanda yerel IP adresi ile hizalamak daha kolay okunabilmesi için seçilen daha yüksek aralıklı bağlantı (8014 – 8026) için Uzak Masaüstü Protokolü (RDP) trafiğine seçili dış bağlantı noktaları noktalarıdır. Örneğin, yerel sunucu adresi 10.0.1.4 8014 dış bağlantı noktası ile ilişkilidir. Ancak, daha yüksek çakışmayan bağlantı noktalarını kullanabilirsiniz.

Aşağıdaki kural türleri bu örnek için şunlar gereklidir:

* Gelen trafik için dış kuralları:
  1. Güvenlik Duvarı Yönetimi: trafiği ağ sanal Gereci yönetim bağlantı noktalarına geçmesine izin verir.
  2. Her windows server için RDP kuralları: RDP aracılığıyla tek tek sunucuların yönetilmesine izin verir.  Ağ sanal Gereci özelliklerine bağlı olarak, bir alana kurallar paket mümkün olabilir.
  3. Uygulama trafik kuralları: biri ön uç web trafiği, diğeri arka uca trafik (örneğin, veri katmanı için web sunucusu). Bu kurallar yapılandırmasını ağ mimarisi ve trafiği bağlıdır akışlar.

     * İlk kural, uygulama tarafından sunucuya ulaşmak gerçek uygulama trafiğine izin verir. Güvenlik, yönetim ve benzeri kurallarından farklı olarak, uygulama kuralları dış kullanıcıları veya hizmetler uygulamaların erişmesine izin verir. Bu örnekte bunun yerine web sunucusunun iç IP adresine yönlendirmek dış IP adresi için giden trafiği yönlendirmek tek bir güvenlik duvarı uygulama kuralı sağlayan bir tek bir web sunucusu bağlantı noktası 80 ' var. Yeniden yönlendirilen trafik oturumu NAT tarafından iç sunucuya eşlenir.
     * İkinci uygulama trafiği kural, web sunucusunu AppVM01 sunucuya, ancak AppVM02 sunucuya trafiği yönlendirmek için herhangi bir bağlantı noktası kullanacak şekilde izin vermek için arka uç kuralıdır.

* İç kuralları intra virtual ağ trafiği için:
  1. Giden internet kuralı: Seçili ağlar için geçirilecek herhangi bir ağa gelen trafiğe izin verir. Bu kural, genellikle varsayılan bir güvenlik duvarı, ancak devre dışı durumuna değerdir. Bu kural Bu örnek için etkinleştirin.
  2. DNS kuralı: yalnızca DNS sunucusuna geçirilecek DNS (bağlantı noktası 53) trafiğine izin verir. Bu ortam için ön uçtan arka uç çoğu trafik engellenir. Bu kural, tüm yerel alt ağından DNS özellikle sağlar.
  3. Alt ağ ve alt ağ kuralı: ön uç alt ağı, ancak tersine herhangi bir sunucuya bağlanmak için arka uç alt ağdaki herhangi bir sunucu sağlar.

* Eğer yukarıdaki ölçütlerden birini karşılamıyor trafiği için emniyet kuralı:
  1. Reddetme tüm trafik kuralı: öncelik açısından her zaman son kuralı. Önceki kurallardan herhangi birinin eşleştirilecek trafik akışı başarısız olursa bu kural, engeller. Varsayılan bir kuraldır. Yaygın olarak etkinleştirilmiş olduğundan, herhangi bir değişiklik gereklidir.

> [!TIP]
> İkinci uygulama trafiği kuralda herhangi bir bağlantı, bu örneği basit tutmak izin verilmez. Gerçek bir senaryoda, bu kural saldırı yüzeyini azaltmak için belirli bir bağlantı noktası ve adres aralıklarının kullanmalısınız.


> [!IMPORTANT]
> Kuralları oluşturduktan sonra trafiğe izin veya istediğiniz gibi reddedildi emin olmak için her bir kural önceliklerini gözden geçirmeniz önemlidir. Bu örnekte, öncelik sırasına göre kurallardır. Dışında bir güvenlik duvarı kuralları hatalı sıralanmış olması durumunda saldırdığında daha kolaydır. Güvenlik duvarını yönetim mutlak en yüksek öncelikli ayarladığınızdan emin olun.

### <a name="rule-prerequisites"></a>Kural önkoşulları

Ortak Uç noktalara Güvenlik Duvarı'nı çalıştıran sanal makine için gerekli değildir. Güvenlik duvarının trafiği işleyebilir, böylece bu ortak Uç noktalara açık olması gerekir. Bu örnekte trafiğin üç tür vardır:

1. Yönetim trafiği güvenlik duvarı ve güvenlik duvarı kuralları kontrol etmek için
1. Windows sunucuları denetlemek için RDP trafiği
1. Uygulama trafiği

Trafik türlerini güvenlik duvarı kuralları mantıksal Yukarıdaki diyagramda üst yarısında görüntülenir.

> [!IMPORTANT]
> Unutmayın *tüm* trafiği güvenlik duvarı üzerinden sunulur. IIS01 sunucuya Uzak Masaüstü için güvenlik duvarında bağlantı noktası 8014 bağlanmak ve ardından RDP isteği IIS01 RDP bağlantı noktası için dahili olarak yönlendirmek güvenlik duvarı izin gerekir. Azure portalının **Connect** düğme olduğu için doğrudan RDP yol yok IIS01 portalı görebilirsiniz kadar çalışmaz. Güvenlik hizmeti ve bağlantı noktası (örneğin, secscv001.cloudapp.net:xxxx) için İnternet'ten gelen tüm bağlantılardır. Yukarıdaki diyagramda dış bağlantı noktası ve iç IP ve bağlantı noktası eşlemesi için başvuru.

Bir uç nokta, bir VM oluşturmak veya derlemeden sonra zaman açabilirsiniz. VM oluşturulduktan sonra örnek komut dosyası ve aşağıdaki kod parçacığı bir uç nokta açın.

Unutmayın, bu öğeler ile başlayan `$`, gibi `$VMName[$i]`, komut dosyası başvurusu bölümündeki kullanıcı tanımlı değişkenler. `[$i]` VM'lerin bir dizideki belirli bir VM'ye dizi sayısını temsil eder.

```powershell
Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
    -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
    Update-AzureVM
```

Değil açıkça değişkenler nedeniyle burada gösterilmeden olsa da, yalnızca güvenlik bulut hizmeti bitiş noktası açmanız gerekir. Bu önlem, özel olarak, yönlendirilmiş, NAT tarafından uri'lerini veya bırakılan olup olmadığını Güvenlik Duvarı tüm gelen trafiği işleyen sağlamaya yardımcı olur.

Bir yönetim istemcisi bir güvenlik duvarını yönetmeyi ve gerekli yapılandırmaları oluşturmak için bir Bilgisayara yükleyin. Güvenlik duvarınızı veya diğer NVA yönetme hakkında daha fazla bilgi için satıcının belgelerine başvurun. Bu bölümün geri kalanında yanı sıra **güvenlik duvarı kuralları oluşturma** bölümü, güvenlik duvarı yapılandırmasını açıklar. Satıcının yönetim istemci değil Azure portalı veya PowerShell kullanın.

Git [Barracuda NG yönetici](https://techlib.barracuda.com/NG61/NGAdmin) yönetim istemcisini indirin ve Barracuda güvenlik duvarı bağlantı kurmayı öğrenin.

Güvenlik Duvarı açtığınız sonra güvenlik duvarı kuralları oluşturmadan önce ağ ve hizmet nesneleri tanımlar. Bu iki önkoşul nesne sınıfları, kuralları daha kolay oluşturma yapabilirsiniz.

Bu örnekte, üç adlandırılmış ağ nesnelerinin her biri için tanımlayın:

* Ön uç alt ağı
* Arka uç alt ağı
* DNS sunucusunun IP adresi

Barracuda NG yönetici istemci panodan adlandırılmış bir ağ oluşturmak için:

1. Git **yapılandırma sekmesinde**.
1. Seçin **Ruleset** içinde **işletimsel yapılandırma** bölümü
1. Seçin **ağları** altında **güvenlik duvarı nesneleri** menüsü.
1. Seçin **yeni** içinde **Düzenle ağları** menüsü.
1. Ad ve önek ekleyerek ağ nesnesini oluşturun:

   ![Bir ön ed ağ nesnesi oluşturun][3]

Önceki adımlarda, ön uç alt ağı için adlandırılmış bir ağ oluşturun. Benzer bir nesne için de arka uç alt ağı oluşturun. Artık alt ağların güvenlik duvarı kuralları ada göre daha kolay başvurulabilir.

DNS sunucu nesnesi için:

![Bir DNS sunucusu nesnesi oluşturun][4]

Bu tek IP adresi başvurusu bir DNS kuralı belgenin ilerleyen bölümlerinde kullanılır.

Bir nesne sınıfı, her sunucu için RDP bağlantı noktaları gösteren Hizmetleri nesneleri içerir. Mevcut RDP hizmet nesnesi 3389 varsayılan RDP bağlantı noktasına bağlıdır. Bu nedenle, dış bağlantı noktaları (8026 8014) gelen trafiğe izin verecek şekilde yeni hizmetler oluşturabilirsiniz. Ayrıca, RDP hizmetiniz için yeni bağlantı noktaları ekleyebilirsiniz. Ancak, tanıtım kolaylığı için her sunucu için tek bir kuralı yapabilirsiniz. Barracuda NG yönetici istemci panodan bir sunucu için yeni bir RDP kuralı oluşturmak için:

1. Git **yapılandırma sekmesinde**.
1. Seçin **Ruleset** içinde **işletimsel yapılandırma** bölümü.
1. Seçin **Hizmetleri** altında **güvenlik duvarı nesneleri** menüsü.
1. Hizmetleri ve seçim listesini aşağı kaydırın **RDP**.
1. Sağ tıklatın ve Kopyala'yı seçin, sonra sağ tıklayıp yapıştırın.
1. Düzenlenebilir bir RDP Copy1 hizmet nesnesi artık yoktur. Sağ **RDP Copy1** seçip **Düzenle**.
1. **Hizmet nesnesi Düzenle** burada gösterildiği gibi kadar penceresi açılır:

   ![Varsayılan RDP kuralının kopyalama][5]

1. Belirli bir sunucu için RDP hizmeti göstermek için değerleri düzenleyin. Varsayılan RDP kuralının AppVM01 için yeni bir hizmet yansıtacak şekilde değiştirilmelidir **adı**, **açıklama**ve dış RDP Şekil 8 şemada kullanılan bağlantı noktası. Bu belirli bir sunucu için dış bağlantı noktasına bağlantı noktası 3389 RDP varsayılan değiştirilir aklınızda bulundurun. Örneğin, dış bağlantı AppVM01 için 8025 noktasıdır. Değiştirilen hizmetin kural aşağıda gösterilmiştir:

   ![AppVM01 kuralı][6]

Geri kalan sunucular için RDP hizmetleri oluşturmak için bu işlemi yineleyin: AppVM02 DNS01 ve IIS01. Bu hizmetler kuralları oluşturmak daha basit ve daha belirgin sonraki bölümde hale getirir.

> [!NOTE]
> RDP yerine VM yönetimi için kullanılan SSH bağlantı noktası 22 için VM Güvenlik Duvarı Linux tabanlı bir görüntü olduğu için bir RDP Hizmeti Güvenlik Duvarı için gerekli değildir. Ayrıca 22 numaralı bağlantı noktası ve diğer iki bağlantı noktası yönetim bağlantısını için izin verilir. Bkz: **Yönetimi Güvenlik Duvarı** sonraki bölümde.

### <a name="firewall-rules-creation"></a>Güvenlik duvarı kuralları oluşturma

Bu örnekte, tüm farklı simgeleri ile kullanılan güvenlik duvarı kuralları üç tür vardır:

Uygulama yeniden yönlendirme kuralı: ![Uygulama yeniden yönlendirme simgesi][7]

Hedef NAT kuralı: ![Hedef NAT simgesi][8]

Geçişi kuralı: ![Geçişi simgesi][9]

Bu kurallar hakkında daha fazla bilgi Barracuda Web sitesinde bulunabilir.

Aşağıdaki kurallar oluşturabilir veya var olan varsayılan kuralları doğrulamak için:

1. Barracuda NG yönetici istemci panodan Git **yapılandırma** sekmesi.
1. İçinde **işletimsel yapılandırma** bölümünden **Ruleset**.
1. **Ana kuralları** kılavuz var olan active gösterir ve bu güvenlik duvarı kurallarını devre dışı bırakıldı. Yeşili **+** yeni bir kural oluşturmak üzere sağ üst köşedeki içinde. Güvenlik duvarınızı değişikliklere karşı kilitlenmiş olarak işaretlenmiş bir düğme görürsünüz **kilit** ve olamaz kuralları oluşturun veya düzenleyin. Seçin **kilit** düğmesine kural kümesi kilidini açmak ve düzenlenmesine izin verilir. İstediğiniz düzen ve seçmek için bir kurala sağ tıklayın **kuralını Düzenle**.

Tüm kuralları oluşturduğunuzda veya sonra bunları gönderin güvenlik duvarı ve bunları etkinleştirmek. Aksi takdirde, kural değişiklikler etkili olmayacaktır. Anında iletme ve etkinleştirme işlemi açıklanan [kuralını etkinleştirme](#rule-activation).

Bu örnekte tamamlamak için gereken her bir kural ayrıntılarını şunlardır:

* **Güvenlik Duvarı Yönetimi**: Bu uygulama yeniden yönlendirme kuralı, bu örnekte bir Barracuda NextGen güvenlik duvarı ağ sanal Gereci yönetim bağlantı noktalarını geçirilecek trafiğine izin verir. Yönetim bağlantı noktalarına 801 807 ve isteğe bağlı olarak 22 ' dir. İç ve dış bağlantı noktası, bağlantı noktası çeviri aynıdır. Bu kural, Kurulum-MGMT-erişim denir. Bu varsayılan kural ve varsayılan olarak, Barracuda NextGen güvenlik duvarı, sürüm 6.1 etkin.
  
    ![Güvenlik Duvarı Yönetimi][10]

  > [!TIP]
  > Bu kuralın kaynak adres alanında olduğundan **herhangi**. Yönetim IP adresi aralıklarını bilinmiyorsa, bu kapsam azaltma yönetim bağlantı noktalarına saldırı yüzeyini de azaltır.

* **RDP kuralları**:  Bu hedef NAT kuralları, RDP aracılığıyla tek tek sunucuların yönetimini sağlar. Bu kurallar için önemli alanlar şunlardır:
  * Kaynağı. RDP yerden izin vermek için başvuru kullanın **herhangi** kaynağı alanında.
  * Hizmeti. Daha önce oluşturduğunuz RDP hizmet nesnesi kullanın: **AppVM01 RDP**. Dış bağlantı noktaları, sunucunun yerel IP adresi ve varsayılan RDP bağlantı noktası 3386 yönlendirin. Bu kuralı AppVM01 RDP erişimi içindir.
  * Hedef. Yerel bağlantı noktası Güvenlik Duvarı'nı kullanın: **DHCP 1 yerel IP** veya **eth0** statik IP kullanıyorsanız. Bir sıra numarası (eth0, eth1 vb.) ağ aletiniz birden çok yerel arabirimi varsa farklı olabilir. Güvenlik Duvarı göndermek için bu bağlantı noktasını kullanır ve alma bağlantı noktası aynı olabilir. Gerçek yönlendirilmiş hedef bulunduğu **hedef liste** alan.
  * Yeniden yönlendirme. Sonuç olarak bu trafiğin yönlendirileceği sanal gerece bildirmek için yapılandırın. Basit Yönlendirme IP hedef liste alanında yerleştirmektir. Bağlantı noktası hem de IP adresi NAT yeniden yönlendirmeler ve bağlantı noktası da belirtebilirsiniz. Bir bağlantı noktası belirtmediyseniz, sanal gerecin gelen istek hedef bağlantı noktası kullanır.

    ![RDP Güvenlik Duvarı][11]

    Dört RDP kurallarını oluşturun:

    | Kural Adı | Sunucu  | Hizmet | Hedef listesi |
    | --- | --- | --- | --- |
    | RDP IIS01 |IIS01 |IIS01 RDP |10.0.1.4:3389 |
    | RDP DNS01 |DNS01 |DNS01 RDP |10.0.2.4:3389 |
    | RDP AppVM01 |AppVM01 |AppVM01 RDP |10.0.2.5:3389 |
    | RDP AppVM02 |AppVM02 |AppVm02 RDP |10.0.2.6:3389 |

  > [!TIP]
  > Kaynak ve hizmet alanları kapsamını daraltma saldırı yüzeyini de azaltır. İşlevsellik sağlayan en sınırlı kapsamı kullanın.

* **Uygulama trafik kuralları**: İki uygulama trafik kuralları vardır. Ön uç web trafiği biridir. Diğer veri katmanı için web sunucusu gibi arka uca trafik kapsar. Bu kurallar, ağ mimarisine bağlı ve trafik akışları.
  
  * Web trafiği için ön uç kuralı:
  
    ![Web güvenlik duvarı][12]
  
    Bu hedef NAT kuralı, uygulama tarafından sunucuya ulaşmak gerçek uygulama trafiğine izin verir. Güvenlik, yönetim ve etcetera kurallarından farklı olarak, uygulama kuralları dış kullanıcıları veya hizmetler uygulamaların erişmesine izin verir. Bu örnekte bunun yerine web sunucusunun iç IP adresine yönlendirmek dış IP adresi için giden trafiği yönlendirmek tek bir güvenlik duvarı uygulama kuralı sağlayan bir tek bir web sunucusu bağlantı noktası 80 ' var. Yeniden yönlendirilen trafik oturumu NAT tarafından iç sunucuya eşlenir.

    > [!NOTE]
    > Atanan bağlantı noktası yok **hedef liste** alan. Bu nedenle, gelen bağlantı noktası 80'i (veya seçili hizmeti için 443) web sunucusu yeniden yönlendirmesi kullanılır. Web sunucusu, 8080 gibi farklı bir bağlantı noktasında dinliyorsa, 10.0.1.4:8080 de bağlantı noktası yönlendirmeye izin vermek için hedef listesi alanın güncelleştirebilirsiniz.
  
  * Arka uç kuralı AppVM01 sunucu ancak değil AppVM02 aracılığıyla konuşmak web sunucusu sağlar **herhangi** hizmeti:
  
    ![Güvenlik Duvarı AppVM01 kuralı][13]
  
    Bu geçişi kural her IIS sunucusu AppVM01 ulaşmak için ön uç alt ağda sağlar (10.0.2.5) veri web uygulaması tarafından erişilebilen herhangi bir protokol kullanarak herhangi bir bağlantı noktasında.
  
    Bu ekran görüntüsünde `<explicit-dest>` kullanılır **hedef** 10.0.2.5 hedef olarak belirtmek için alan. IP adresi aşağıdaki ekran görüntüsünde gösterildiği gibi açıkça belirtebilirsiniz. DNS sunucusu için Önkoşullar gibi adlandırılmış bir ağ nesnesine de kullanabilirsiniz. Güvenlik Duvarı yönetici hangi yöntemin kullanılacağını seçebilirsiniz. İlk boş satırda altında 10.0.2.5 açık bir hedef eklemek için çift `<explicit-dest>` ve açılan iletişim kutusunda adresini girin.
  
    İç trafiği işleyen çünkü bu geçişi kuralla NAT gereklidir. Ayarlayabileceğiniz **bağlantı yöntemini** için `No SNAT`.
  
    > [!NOTE]
    > Varsa yalnızca bu kuralda kaynak ağ ön uç alt ağdaki herhangi bir kaynak olabilir. Mimarinizi web sunucuları, bilinen birtakım belirtiyorsa, tüm ön uç alt ağına yerine tam bu IP adreslerini daha özel olarak bir ağ nesnesine kaynak oluşturabilirsiniz.

    > [!TIP]
    > Bu kural hizmetin kullandığı **herhangi** örnek uygulama kurulumu ve kullanımı kolay hale getirmek için. Icmpv4 sağlar (ping) içinde tek bir kural. Ancak, saldırı azaltmak için bu sınırı arasında yüzey bağlantı noktaları ve protokoller Hizmetleri uygulama işlemi sağlayan en küçük olası sınırlama öneririz.

    > [!TIP]
    > Bu örnekteki kural kullansa `<explicit-dest>` başvuru, güvenlik duvarı yapılandırması genelinde tutarlı bir yaklaşım kullanmanız gerekir. Adlandırılmış bir ağ nesnesine daha kolay okunabilirlik ve desteklenebilirlik için kullanılması önerilir. `<explicit-dest>` Burada yalnızca bir alternatif başvuru yöntemi gösterecek şekilde gösterilmektedir. Yoksa genellikle önerilir, özellikle karmaşık yapılandırmalar için.

* **Giden internet kuralı**: Bu geçişi kural seçilen hedef ağlara geçirilecek herhangi bir kaynak ağa gelen trafiğe izin verir. Barracuda NextGen güvenlik duvarı genellikle bu kural "on" varsayılan olarak, ancak devre dışı durumuna sahiptir. Bu kurala erişmek için sağ **etkinleştirme kuralı** komutu. Bu kural kaynak özniteliği için arka uç ve ön uç alt ağları için ağ nesneleri eklemek için ekran görüntüsünde gösterilen kuralı değiştirin. Bu ağ nesneler, bu makalenin önkoşul bölümünde oluşturduğunuz.
  
    ![Giden güvenlik duvarı kuralı][14]

* **DNS kuralı**: Bu geçişi kural yalnızca DNS sunucusuna geçirilecek DNS (bağlantı noktası 53) trafiğine izin verir. Bu kural, özellikle DNS trafiğe izin veren şekilde bu ortam için ön uçtan arka uç çoğu trafik engellenir.
  
    ![DNS güvenlik duvarı][15]
  
    > [!NOTE]
    > **Bağlantı yöntemini** ayarlanır `No SNAT` iç IP iç IP adresi trafiği için bu kural olduğundan ve hiçbir NAT yeniden yönlendirilmesine gereklidir.

* **Alt ağ için alt kural**: Bu varsayılan geçişi kural etkinleştirildi ve ön uç alt ağına herhangi bir sunucuya bağlanmak için arka uç alt ağdaki herhangi bir sunucu izin verecek şekilde değiştirilmelidir. Bu kural yalnızca iç trafik coves böylece **bağlantı yöntemini** ayarlanabilir `No SNAT`.

    ![Güvenlik Duvarı içi-sanal ağ kuralı][16]
  
    > [!NOTE]
    > **Yönlü** onay kutusu seçilmez burada bu kural yalnızca bir yöne uygular. Bir bağlantı, ön uç ağı, ancak tersine arka uç alt ağından başlatılabilir. Bu kural, onay kutusu seçildiyse, bizim mantıksal diyagramda istenmeyen olarak belirttiğiniz yönlü trafik, etkinleştirebilirsiniz.

* **Reddetme tüm trafik kuralı**: Bu kural her zaman öncelikli açısından son bir kural olmalıdır. Trafik akışını önceki kurallardan herhangi birinin eşleşmiyorsa, kuralın kesilmesine neden olur. Herhangi bir değişiklik gerektiği şekilde kural genellikle varsayılan olarak etkinleştirilir.
  
    ![Güvenlik duvarı kuralı Reddet][17]

> [!IMPORTANT]
> Tüm önceki kurallar oluşturulduktan sonra trafiğe izin verilir veya trafik reddedilir istediğiniz gibi emin olmak için her bir kural önceliklerini gözden geçirin. Bu örnekte, kuralları iletme kuralları, Barracuda yönetim istemcinin ana kılavuz halinde görünmesi gereken sırayla listelenir.

## <a name="rule-activation"></a>Kural etkinleştirme

Kural mantığı diyagramın belirtimlere için kümesi değiştirdiğiniz sonra güvenlik duvarı kural kümesini yüklemek ve etkinleştirmek gerekir.

![Güvenlik duvarı kuralını etkinleştirme][18]

Yönetim istemcisi pencerenin sağ üst köşesinde arayın ve seçin **değişiklikleri göndermek** değiştirilmiş kuralları için Güvenlik Duvarı'nı yüklemek için. Seçin **etkinleştirme**.

Güvenlik duvarı kural kümesi etkinleştirdiğinizde, bu örnek ortam tamamlanmıştır.

## <a name="traffic-scenarios"></a>Trafik senaryoları

> [!IMPORTANT]
> Unutmayın *tüm* trafiği güvenlik duvarı üzerinden sunulur. IIS01 sunucuya Uzak Masaüstü için güvenlik duvarında bağlantı noktası 8014 bağlanmak ve ardından RDP isteği IIS01 RDP bağlantı noktası için dahili olarak yönlendirmek güvenlik duvarı izin gerekir. Azure portalının **Connect** düğme olduğu için doğrudan RDP yol yok IIS01 portalı görebilirsiniz kadar çalışmaz. Güvenlik hizmeti ve bağlantı noktası (örneğin, secscv001.cloudapp.net:xxxx) için İnternet'ten gelen tüm bağlantılardır. Yukarıdaki diyagramda dış bağlantı noktası ve iç IP ve bağlantı noktası eşlemesi için başvuru.

Bu senaryolar için aşağıdaki güvenlik duvarı kuralları koşulların karşılanması:

1. Güvenlik Duvarı Yönetimi (FW Mgmt)
2. RDP için IIS01
3. RDP için DNS01
4. RDP için AppVM01
5. RDP için AppVM02
6. Web uygulaması trafiğini
7. AppVM01 uygulama trafiği
8. İnternet'e giden
9. DNS01 için ön uç
10. İçi alt ağ trafiği (ön uca arka uç)
11. Tümünü Reddet

Bu örnekte olandan daha fazla kural büyük olasılıkla, gerçek güvenlik duvarı kural kümesi gerektirir. Bunlar büyük olasılıkla farklı öncelik numarasına sahip. Bu liste ve ilişkili numaralar birbirlerine göreli önceliklerine için başvurmanız gerekir. Örneğin, "RDP için IIS01" kural numarası 5 ancak onun aşağıdaki "güvenlik duvarı Yönetimi" sürece gerçek güvenlik duvarı kuralı olabilir ve "RDP için DNS01" kuralı, belirlenen amacıyla yapılıyorsa bu liste hizalar. Bu liste, izleyen senaryolar için yönergeler basitleştirmek da yardımcı olur. Örneğin, "güvenlik duvarı kuralı 9 (DNS)." Dört RDP kural "RDP kuralları" toplu olarak adlandırılır unutmayın trafiği senaryo olduğunda RDP'den ilgisiz.

Ayrıca ağ güvenlik grupları (Nsg'ler) gelen internet trafiği ön uç ve arka uç alt ağlardaki yürürlükte olduğunu hatırlayın.

### <a name="allowed-internet-to-web-server"></a>(İzin verilir) Web sunucusuna Internet

1. İnternet kullanıcı SecSvc001.CloudApp.Net (internet'e yönelik bulut hizmeti) istekler HTTP sayfası.
1. Bulut hizmeti trafiği üzerinden açık bir uç nokta bağlantı noktası 80 üzerinde 10.0.0.4:80 güvenlik duvarı arabirimde geçirir.
1. Sistem NSG kuralları güvenlik duvarı izin verecek şekilde hiçbir NSG güvenlik alt ağa atanır.
1. Trafiği bir dahili IP adresine (10.0.1.4) Güvenlik Duvarı'nın denk gelir.
1. Güvenlik duvarı kuralı işlemeyi gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 2-5 (RDP kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 6 (uygulama: Geçerli Web). Trafiğe izin verilir. Güvenlik Duvarı aracılığıyla NAT 10.0.1.4 (IIS01) trafiği yeniden yönlendirmeler.
1. Ön uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
   1. NSG kuralı 1 (blok internet) geçerli değildir. Kaynak adresi artık güvenlik duvarı, bu nedenle NAT aracılığıyla bu trafiğe güvenlik duvarı yönlendirdi. Güvenlik Duvarı güvenlik alt ağda bulunan ve ön uç alt ağına NSG yerel trafiği olarak görünür olduğundan, trafiğe izin verilir. Sonraki kural taşıyın.
   1. Bu trafiğe izin verilmesi varsayılan NSG kuralları alt ağ ve alt ağ trafiği izin verir. NSG kuralının işlenmesi durdurun.
1. Web trafiği için IIS01 dinliyor. Bu isteği alır ve isteği işlemeye başlar.
1. IIS01 AppVM01 arka uç alt ağı üzerinde bir FTP oturumu başlatmak çalışır.
1. Ön uç alt ağına UDR rotaya güvenlik duvarı, sonraki atlama yapar.
1. Giden kural yok ön uç alt ağı üzerinde trafiğe izin verilmesi.
1. Güvenlik duvarı kuralı işleme başlar:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 2-5 (RDP kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 6 (uygulama: Web için) geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 7 (uygulama: arka uç) geçerlidir. Trafiğe izin verilir. Güvenlik Duvarı 10.0.2.5 (AppVM01) trafiği iletir.
1. Arka uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
    1. NSG kuralı 1 (blok internet) geçerli değildir. Sonraki kural taşıyın.
    1. Varsayılan NSG kuralları alt ağ ve alt ağ trafiğine izin verin. Trafiğe izin verilir. NSG kuralının işlenmesi durdurun.
1. AppVM01 isteği aldığında, oturumu başlatır ve yanıt verir.
1. Arka uç alt ağı UDR rotaya güvenlik duvarı, sonraki atlama yapar.
1. Giden hiçbir NSG kuralları arka uç alt ağında olduğundan yanıta izin verilmesi.
1. Yerleşik bir oturum üzerinde trafiği döndürmektir olduğundan, güvenlik yanıtı web sunucusu (IIS01) geçer.
1. Ön uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
    1. NSG kuralı 1 (blok internet) geçerli değildir. Sonraki kural taşıyın.
    1. Bu trafiğe izin verilmesi varsayılan NSG kuralları alt ağ ve alt ağ trafiğine izin. NSG kuralının işlenmesi durdurun.
1. IIS Sunucu yanıtı alır ve AppVM01 ile hareketi tamamlar. Ardından sunucu HTTP yanıtı oluşturma tamamlandıktan ve istek sahibine gönderir.
1. Giden hiçbir NSG kuralları ön uç alt ağında olduğundan yanıta izin verilmesi.
1. HTTP yanıtı, güvenlik duvarı denk gelir. Yerleşik bir NAT oturum yanıt olduğundan, Güvenlik Duvarı'nı da kabul eder.
1. Güvenlik Duvarı yanıt internet kullanıcı yönlendirir.
1. Giden NSG kuralları veya yok ön uç alt ağında UDR atlama yanıta izin verilmesi. İnternet kullanıcı web sayfasının alır.

### <a name="allowed-internet-rdp-to-back-end"></a>(İzin verilir) Arka uca Internet RDP

1. İnternet üzerindeki bir sunucu yöneticisi AppVM01 SecSvc001.CloudApp.Net:8025 aracılığıyla bir RDP oturumu ister. 8025 4 (RDP AppVM01) güvenlik duvarı kuralı kullanıcı tarafından atanan bağlantı noktası numarasıdır.
1. Bulut hizmeti açık uç nokta üzerinden geçen trafik 8025 bağlantı noktasında 10.0.0.4:8025 güvenlik duvarı arabirimde geçirir.
1. NSG kuralları sistem güvenlik duvarı trafiğe izin vermek için hiçbir NSG güvenlik alt ağa atanır.
1. Güvenlik duvarı kuralı işlemeyi gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. 2 (RDP IIS) güvenlik duvarı kuralı geçerli değildir. Sonraki kural taşıyın.
   1. 3 (RDP DNS01) güvenlik duvarı kuralı geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 4 (AppVM01 RDP) trafiğine izin verilmesi geçerli. Güvenlik Duvarı, NAT 10.0.2.5:3386 için yeniden yönlendirmeler (AppVM01 üzerinde RDP noktasına).
1. Arka uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
   1. NSG kuralı 1 (blok internet) geçerli değildir. Kaynak adresi artık güvenlik alt ağda güvenlik duvarı, bu nedenle bu trafiği aracılığıyla NAT güvenlik duvarı yönlendirdi. Bu arka uç alt ağ NSG'SİNDE tarafından yerel trafiği olarak görülür ve izin verilir. Sonraki kural taşıyın.
   1. Bu trafiğe izin verilmesi varsayılan NSG kuralları alt ağ ve alt ağ trafiği izin verir. NSG kuralının işlenmesi durdurun.
1. AppVM01, RDP trafiği için dinleme ve yanıt verir.
1. Varsayılan kuralları uygulamak için giden hiçbir NSG kuralları vardır. Dönüş trafiğe izin verilir.
1. UDR, güvenlik duvarı sonraki atlama olarak giden trafiği yönlendirir.
1. Yerleşik bir oturum üzerinde trafiği döndürmektir olduğundan, güvenlik yanıtı internet kullanıcı geçer.
1. RDP oturumu etkin.
1. AppVM01 kullanıcı adı ve parolasını ister.

### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(İzin verilir) Web sunucusu DNS sunucusu DNS araması

1. Web sunucusu IIS01 veri akışı http isteklerini\:\/\/www.data.gov ancak adresini çözümlemek gerekiyor.
1. Ağ yapılandırması için sanal ağ listeleri DNS01 (arka uç alt ağında 10.0.2.4) birincil DNS sunucusu. IIS01 DNS01 için DNS isteği gönderir.
1. UDR, güvenlik duvarı sonraki atlama olarak giden trafiği yönlendirir.
1. Giden hiçbir NSG kuralları ön uç alt ağına bağlanır. Trafiğe izin verilir.
1. Güvenlik duvarı kuralı işlemeyi gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. 2-5 (RDP kuralları) güvenlik duvarı kuralı uygulanmaz. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 6 ve 7 (uygulama kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 8 (internet için) geçerli değildir. Sonraki kural taşıyın.
   1. Geçerli güvenlik duvarı kuralı 9 (DNS). Trafiğe izin verilir. Güvenlik Duvarı 10.0.2.4 (DNS01) trafiği iletir.
1. Arka uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
   1. NSG kuralı 1 (blok internet) geçerli değildir. Sonraki kural taşıyın.
   1. Varsayılan NSG kuralları alt ağ ve alt ağ trafiğine izin verin. Trafiğe izin verilir. NSG kuralının işlenmesi durdurun.
1. DNS sunucusu isteği alır.
1. DNS sunucusu, önbelleğe alınmış adresleriniz değil ve bir kök DNS sunucusu internet'te sorar.
1. UDR, güvenlik duvarı giden trafik sonraki atlama olarak yönlendirir.
1. Var olan trafik için arka uç alt ağdaki giden hiçbir NSG kuralları izin verilir.
1. Güvenlik duvarı kuralı işlemeyi gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. 2-5 (RDP kuralları) güvenlik duvarı kuralı uygulanmaz. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 6 ve 7 (uygulama kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 8 (internet için) geçerlidir. Trafiğe izin verilir. Oturum SNAT internet kök DNS sunucusuna yönlendirdi.
1. İnternet DNS sunucusu yanıt verir. Yanıt güvenlik duvarı tarafından kabul edilen biçimde Güvenlik Duvarı'ndan bu oturumu başlatıldı.
1. Güvenlik Duvarı yanıt DNS01 başlatma sunucusuna iletir. Bu nedenle bu oturumu zaten oluşturulur.
1. Arka uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
    1. NSG kuralı 1 (blok internet) geçerli değildir. Sonraki kural taşıyın.
    1. Varsayılan NSG kuralları alt ağ ve alt ağ trafiği bu trafiğe izin verin. NSG kuralının işlenmesi durdurun.
1. DNS sunucusu alır ve yanıtı önbelleğe alır ve ardından geri IIS01 ilk isteğine yanıt verir.
1. Arka uç alt ağı UDR rotaya güvenlik duvarı, sonraki atlama yapar.
1. Trafiğe izin verilmesi giden hiçbir NSG kuralları arka uç alt ağda mevcut.
1. Güvenlik Duvarı yanıt IIS sunucusunu yeniden yönlendirmeler şekilde bu oturumu zaten Güvenlik Duvarı'nı oluşturulur.
1. Ön uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
    1. Hiçbir NSG kurallarını uygulamak için arka uç alt ağından gelen trafiği ön uç alt ağı için NSG kural yoktur.
    1. Varsayılan sistem kuralı alt ağlar arasındaki trafiği sağlar. Trafiğe izin verilir.
1. IIS01 DNS01 yanıtı alır.

### <a name="allowed-back-end-server-to-front-end-server"></a>(İzin verilir) Ön uç sunucusu için arka uç sunucu

1. RDP aracılığıyla AppVM02 oturum açmış bir yönetici, doğrudan windows dosya Gezgini aracılığıyla IIS01 sunucusundan bir dosyayı ister.
1. Arka uç alt ağı UDR rotaya güvenlik duvarı, sonraki atlama yapar.
1. Giden hiçbir NSG kuralları arka uç alt ağında olduğundan yanıta izin verilmesi.
1. Güvenlik duvarı kuralı işlemeyi gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. 2-5 (RDP kuralları) güvenlik duvarı kuralı uygulanmaz. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 6 ve 7 (uygulama kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 8 (internet için) geçerli değildir. Sonraki kural taşıyın.
   1. 9 (DNS) güvenlik duvarı kuralı geçerli değildir. Sonraki kural taşıyın.
   1. Geçerli güvenlik duvarı kuralı 10 (içi alt ağ). Trafiğe izin verilir. Güvenlik Duvarı (IIS01) 10.0.1.4 için trafiği geçirir.
1. Ön uç alt ağına gelen kuralı işleme başlar:
   1. NSG kuralı 1 (blok internet) değil, uygulamak için sonraki kural taşıma
   1. Varsayılan NSG kuralları trafiğe izin verilmesi alt ağ ve alt ağ trafiği izin verin. NSG kuralının işlenmesi durdurun.
1. Uygun kimlik doğrulaması ve yetkilendirme varsayıldığında, IIS01 isteği kabul eder ve yanıt verir.
1. Ön uç alt ağına UDR rotaya güvenlik duvarı, sonraki atlama yapar.
1. Giden hiçbir NSG kuralları ön uç alt ağında olduğundan yanıta izin verilmesi.
1. Bu yanıt izin verilmesi Güvenlik Duvarı'nı bu oturumu zaten mevcut. Güvenlik Duvarı AppVM02 yanıtı döndürür.
1. Arka uç alt ağına gelen kuralı işlemeyi gerçekleştirir:
    1. NSG kuralı 1 (blok internet) geçerli değildir. Sonraki kural taşıyın.
    2. Varsayılan NSG kuralları trafiğe izin verilmesi alt ağ ve alt ağ trafiği izin verin. NSG kuralının işlenmesi durdurun.
1. AppVM02 yanıtı alır.

### <a name="denied-internet-direct-to-web-server"></a>(Reddedildi) Web sunucusuna doğrudan Internet

1. İnternet kullanıcı FrontEnd001.CloudApp.Net hizmet aracılığıyla IIS01 web sunucusuna erişmeye çalışır.
1. Vardır; uç nokta açık HTTP trafiği için bu trafik bulut hizmeti aracılığıyla geçmiyor. Bu nedenle. Trafik tarafından sunucuya ulaşmak değil.
1. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağda NSG (blok internet) bu trafiği engeller.
1. Son olarak, ön uç alt ağına UDR rota tüm giden trafiği IIS01 Güvenlik Duvarı için sonraki atlama olarak gönderir. Güvenlik Duvarı, asimetrik trafiği olarak görür ve giden yanıt bırakır.

>Bu nedenle, savunma hattı IIS01 ve internet arasında en az üç bağımsız Katmanlar vardır. Bulut hizmeti yetkisiz veya uygunsuz erişimi engeller.

### <a name="denied-internet-to-back-end-server"></a>(Reddedildi) Arka uç sunucusuna Internet

1. Bir dosya çubuğunda AppVM01 BackEnd001.CloudApp.Net hizmet aracılığıyla erişmek bir internet kullanıcı çalışır.
2. Bulut hizmeti bu isteği geçmiyor için dosya paylaşımı için açık uç nokta yok. Trafik tarafından sunucuya ulaşmak değil.
3. Uç noktaları için herhangi bir nedenle açıksa, bu trafiğin NSG (blok internet) engeller.
4. Son olarak, UDR rota tüm giden trafiği AppVM01 Güvenlik Duvarı için sonraki atlama olarak gönderir. Güvenlik Duvarı, asimetrik trafiği olarak görür ve giden yanıt bırakır.

> Bu nedenle, savunma hattı AppVM01 ve internet arasında en az üç bağımsız Katmanlar vardır. Bulut hizmeti yetkisiz veya uygunsuz erişimi engeller.

### <a name="denied-front-end-server-to-back-end-server"></a>(Reddedildi) Arka uç sunucusu için ön uç sunucusu

1. IIS01 güvenliği aşıldığında ve arka uç alt ağı sunucuları için tararken kötü amaçlı kod çalışıyor.
1. Ön uç alt ağına UDR yol sonraki atlama olarak güvenlik duvarı IIS01 tüm giden trafiği gönderir. Bu yönlendirme güvenliği aşılmış VM değiştirilemiyor.
1. Güvenlik duvarının trafiği işler. İstek AppVM01 veya DNS sunucusu DNS araması için ise güvenlik duvarı 7 ve 9 güvenlik duvarı kuralları nedeniyle trafik potansiyel olarak sağlayabilir. Diğer tüm trafiği, güvenlik duvarı kuralı 11 (Tümünü Reddet) tarafından engellenir.
1. Gelişmiş tehdit algılama için güvenlik duvarında etkinleştirirseniz, bilinen imzaları veya Gelişmiş tehdit kural bayrak desenleri içeren trafiği engelleyen. Bu makalede ele alınan temel yönlendirme kurallarına göre trafiğe izin olsa bile bu ölçüyü çalışabilir. Gelişmiş tehdit algılama, bu belgenin kapsamında değildir. Gelişmiş tehdit özellikleri belirli ağ aletiniz için satıcının belgelerine bakın.

### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Reddedildi) DNS sunucusundaki Internet DNS Arama

1. DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını aramak bir internet kullanıcı çalışır.
1. DNS trafiği için açık uç nokta olduğundan, bu trafik bulut hizmeti aracılığıyla geçmiyor. Bu sunucunun ulaşmaz.
1. Ön uç alt ağına NSG kuralı (blok Internet), uç noktaları için herhangi bir nedenle açıksa, bu trafiği engeller.
1. Son olarak, arka uç alt ağı UDR rota tüm giden trafiği DNS01 Güvenlik Duvarı için sonraki atlama olarak gönderir. Güvenlik Duvarı bu asimetrik trafiği olarak görür ve giden yanıt bırakır.

> Bu nedenle, savunma hattı DNS01 ve internet arasında en az üç bağımsız Katmanlar vardır. Bulut hizmeti yetkisiz veya uygunsuz erişimi engeller.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Reddedildi) Internet için güvenlik duvarı üzerinden SQL erişimi

1. İnternet kullanıcı SecSvc001.CloudApp.Net internet'e yönelik bulut hizmetinden SQL verileri ister.
1. Uç nokta için SQL açık var. Bu trafik, bulut hizmeti geçmiyor şekilde. Güvenlik Duvarı ulaşmak değil.
1. Bazı nedenlerden dolayı SQL uç noktaları açık olması durumunda, güvenlik duvarı kuralı işleme gerçekleştirir:
   1. Güvenlik Duvarı (FW Mgmt) 1 geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 2-5 (RDP kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralları 6 ve 7 (uygulama kuralları) için geçerli değildir. Sonraki kural taşıyın.
   1. Güvenlik duvarı kuralı 8 (internet için) geçerli değildir. Sonraki kural taşıyın.
   1. 9 (DNS) güvenlik duvarı kuralı geçerli değildir. Sonraki kural taşıyın.
   1. 10 (içi alt ağ) güvenlik duvarı kuralı geçerli değildir. Sonraki kural taşıyın.
   1. Geçerli güvenlik duvarı kuralı 11 (Tümünü Reddet). Trafik engellenir. Kuralı işlemeyi durdur.

## <a name="references"></a>Başvurular

Bu bölüm, aşağıdaki öğeleri içerir:

* Tam komut dosyası. Bir PowerShell komut dosyası kaydedin.
* Ağ yapılandırması. NetworkConf2.xml adlı bir dosyaya kaydedin.

Kullanıcı tanımlı değişkenler dosyaları gerektiği gibi değiştirin. Betiği çalıştırın ve ardından bu makalenin önceki bölümlerinde listelenen güvenlik duvarı kuralı ayarlama yönergeleri izleyin.

### <a name="full-script"></a>Tam betik

Kullanıcı tanımlı değişkenler ayarladıktan sonra bu komut dosyasını çalıştırın:

1. Bir Azure aboneliğine Bağlanma
1. Yeni depolama hesabı oluştur
1. Yeni bir sanal ağ ve ağ yapılandırma dosyasında tanımlanan üç alt ağ oluşturma
1. Beş sanal makine oluştur: bir güvenlik duvarı ve dört Windows Server Vm'leri
1. UDR yapılandırın:
   1. İki yeni rota tabloları oluşturma
   1. Yol tablolarına ekleme
   1. Tablolar için uygun alt ağları bağlama
1. NVA üzerindeki IP iletmeyi etkinleştirin
1. NSG yapılandırın:
   1. Bir NSG oluşturma
   1. Kural ekle
   1. NSG için uygun alt ağları bağlama

Bu PowerShell çalıştırma betiği yerel olarak bir internet üzerindeki bilgisayar veya sunucu bağlı.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırdığınızda, uyarı veya bilgilendirme iletileri diğer PowerShell'de açılır. Yalnızca kırmızı hata iletileri endişeye neden olan.

```powershell
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
       - IP Forwarding from the FireWall out to the internet
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
            Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
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

      # Associate the Route Tables with the Subnets
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
        # since the NSG bound to the FrontEnd and BackEnd subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install BackEnd resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
```

### <a name="network-config-file"></a>Ağ yapılandırma dosyası

Bu XML dosyasını güncelleştirilmiş konumu ile kaydedin. Değişiklik `$NetworkConfigFile` kaydedilen ağ yapılandırma dosyasına bağlanmak için tam komut Yukarıdaki değişken.

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
```

## <a name="next-steps"></a>Sonraki adımlar

Bu çevre ağ örneğiyle yardımcı olmak için örnek bir uygulama yükleyebilirsiniz.

> [!div class="nextstepaction"]
> [Örnek uygulama betiği](./virtual-networks-sample-app.md)

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
