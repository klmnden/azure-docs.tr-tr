---
title: TCP/IP'yi Azure Vm'leri için performans ayarlama | Microsoft Docs
description: Çeşitli ortak TCP/IP'yi performans ayarlama teknikleri ve bunların Azure vm'lerine öğrenin.
services: virtual-network
documentationcenter: na
author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
manager: paragk
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/30/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 664c8b659152a370d7fb31907b6cdbcd414dce31
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905112"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP'yi performans Azure Vm'leri için ayarlama

Bu makalenin amacı, ortak TCP/IP'yi performans ayarlama teknikleri ve Microsoft Azure üzerinde çalışan sanal makineler için kendi konuları ele sağlamaktır. İlk kavramları temel bir anlayışa sahip ve bunların nasıl belirleyebileceğinizi'ı tartışmak önemlidir.

## <a name="common-tcpip-tuning-techniques"></a>Ortak TCP/IP'yi ayar teknikleri

### <a name="mtu-fragmentation-and-large-send-offload-lso"></a>MTU parçalanma ve Büyük Gönderme Boşaltması (LSO)

#### <a name="explanation-of-mtu"></a>MTU açıklaması

En fazla iletim birimi (MTU) bir ağ arabirimi üzerinden gönderilen bayt cinsinden belirtilen en büyük boyutunu (paket) çerçevesidir. MTU yapılandırılabilir bir ayar ve Azure Vm'lerinde MTU kullanılan varsayılan değer ve çoğu ağ cihazları varsayılan ayarı küresel olarak 1500 bayttır.

#### <a name="explanation-of-fragmentation"></a>Parçalanma açıklaması

Parçalanma, bir ağ arabiriminin MTU'su aşan bir paket gönderildiğinde gerçekleşir. TCP/IP yığınının paket bir MTU arabirimler için uygun daha küçük parçalara (parça) çalışmamasına neden olur. Parçalanma IP katmanında gerçekleşir ve temel alınan protokoldeki (örneğin, TCP) bağımsızdır. 2000 baytlık bir paket 1500 bir MTU ile bir ağ arabirimi üzerinden gönderildiğinde, ardından bunu bir 1500 baytlık bir paket ve 500 baytlık bir paket halinde ayrılır.

Ağ cihazları arasındaki kaynak ve hedef yolda MTU aşan paketler veya paket daha küçük parçalara parçalara seçeneğiniz vardır.

#### <a name="the-dont-fragment-df-bit-in-an-ip-packet"></a>"Yoksa parça (DF)" bit IP paketi

IP Protokolü üst bilgisindeki bir bayrak parçalama bittir. DF bit ayarlandığında, gönderen ve alıcı arasındaki yolu ara ağ cihazlarında paketin parçası olmayan gösterir. Neden bu bit ayarlanabilir birçok neden vardır (bir örnek için aşağıdaki yol bulma bölümüne bakın). Ne zaman bir ağ aygıtı parçalama bit kümesi içeren bir paket alır ve bu paket cihazları arabirimi MTU aşıyor, daha sonra cihazın paket bırakın ve bir "ICMP parçalanma gerekli" paketi özgün kaynağına göndermek standart davranıştır Paket.

#### <a name="performance-implications-of-fragmentation"></a>Parçalanma performans etkileri

Parçalanma, olumsuz etkileri olabilir. CPU/bellek parçalanması etkisini ve paketlerin yeniden performans etkisi için temel nedenlerinden biridir. Bir ağ aygıtının bir paket gerektiğinde parçalanma gerçekleştirmek için CPU/bellek kaynakları tahsis gerekecektir. Aynı durum, paket birleştirildiği oluşturulmadığında gerekir. Ağ aygıtı, onları özgün pakete yeniden birleştirmek şekilde alındığında kadar tüm parçaları depolamanız gerekir. Bu işlemi parçalanma/yeniden birleştirme gecikmesi parçalanma/yeniden birleştirme işlemi nedeniyle da neden olabilir.

Diğer olası olumsuz olduğu çıkarımında parçalanma, parçalanmış paketlere sıralamaya gelebilir ' dir. Çıkış sırası paketleri, belirli bir türdeki ağ cihazları daha sonra yeniden iletilmek için tüm paket gerektirir olan bozuk paketler - neden olabilir. Ağ güvenlik duvarları gibi güvenlik cihazları parçaları silmek için tipik senaryolar içerir veya bir ağ cihazını alma zaman arabelleği tükendi. Bir ağ cihazını alma arabelleği tükendi, bir ağ aygıtı parçalanmış paketini yeniden birleştirmek çalışıyor, ancak paket reassume ve depolamak için bir kaynak yok.

Parçalanma farklı ağlar Internet üzerinden bağlanmak için gerekli olduğu için parçalanma bir negatif işlemi ancak destek algılanan.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Avantajları ve sonuçları MTU değiştirme

Genel deyim olarak MTU artan daha verimli bir ağ oluşturabilirsiniz. Aktarılan her paketi, orijinal pakete eklenen ek üstbilgi bilgilerini içerir. Daha fazla paket yükü daha fazla üstbilgi anlamına gelir ve sonuç olarak ağ verimli değildir.

Örneğin, Ethernet başlık boyutu 14 bayt artı çerçeve tutarlılığını sağlamak için bir 4 baytlık çerçeve denetleme dizisi (FCS) ' dir. Bir 2000 baytlık paket gönderilir, ardından 18 bayt Ethernet yük eklenir ağda. Paket 1500 baytlık bir paket ve 500 baytlık bir paket halinde parçalanmışsa, her paket Ethernet üstbilgisinin - 18 bayt veya 36 bayt olacaktır. Tek 2000 baytlık bir paket, yalnızca bir Ethernet üstbilgisi 18 bayt gerekir ancak.

Kendi içinde MTU artırma mutlaka daha verimli bir ağ oluşturmayacağını dikkat edin önemlidir. Bir uygulamanın yalnızca 500 baytlık paketleri gönderirse, MTU 1500 veya 9000 bayt olup aynı üstbilgi yükü bulunur. Daha fazla olacak şekilde ağ için sırayla verimli, ardından bunu da MTU göre daha büyük paket boyutları kullanmanız gerekir.

#### <a name="azure-and-vm-mtu"></a>Azure ve VM MTU

Azure Vm'leri için MTU varsayılan 1500 bayttır. Azure sanal ağ yığınının paket 1400 bayt parçalara dener. Ancak, "Parçalara ayırma" bit IP üstbilgisinde olarak ayarlandığında Azure sanal ağ yığınını 2006 bayt paketler sağlar.

Bu parçalama Vm'leri bir MTU 1500 varken 1400 bayt paketlerini parçacık. çünkü Azure sanal ağ yığınını kendiliğinden verimsizdir yaptığından değil dikkat edin önemlidir. Gerçekte, ağ paketlerinin büyük bir yüzdesini 1400 veya 1500 bayt daha küçük olmasıdır.

#### <a name="azure-and-fragmentation"></a>Azure ve parçalanma

Azure'nın sanal ağ yığınını, Bugün "Out of sipariş parçaları" kendi özgün parçalanmış sırada ulşamasını yoksa parçalanmış paketlere anlamı - bırakmak için yapılandırılır. Bu paketler, öncelikli olarak Kasım FragmentStack adlı 2018'de duyurulan ağ güvenlik açığı nedeniyle bırakıldı.

Bir Linux çekirdeğinin parçalanmış IPv4 ve IPv6 paketlerini yeniden birleştirilmesinden işlenme üründe FragmentSmack olur. Bir uzak saldırgan bu zayıf hedef sistem üzerinde daha yüksek CPU ve hizmet reddine neden tetikleyici pahalı parça yeniden birleştirme işlemleri için kullanabilir.

#### <a name="tune-the-mtu"></a>MTU ayarlama

Azure sanal makineler, yalnızca başka bir işletim sistemi yapılandırılabilir MTU destekler. Ancak, Azure içinde oluşur ve yukarıdaki ayrıntılı parçalanma MTU yapılandırırken dikkate alınmalıdır.

Azure, müşterilerin kendi VM MTU artırmak için önerilir değil. Bu tartışma, nasıl Azure MTU uygular ve bugün parçalanma gerçekleştirir ayrıntılı olarak açıklayan yöneliktir.

> [!IMPORTANT]
>MTU artırma performansını artırmak için gösterilmez ve uygulama performansı üzerinde olumsuz bir etkiye sahip olabilir.
>
>

#### <a name="large-send-offload-lso"></a>Büyük Gönderme Boşaltması (LSO)

Büyük Gönderme Boşaltması (LSO) ayrılmasını paketlerin Ethernet bağdaştırıcısına boşaltarak, ağ performansını iyileştirebilir. Etkin LSO ile TCP/IP yığınının büyük bir TCP paket oluşturma ve ardından iletmeden önce segment için Ethernet bağdaştırıcısına gönderin. MTU için uygun ve bu işlem donanım gerçekleştirildiği Ethernet arabirimi yük boşaltma paketi boyutları halinde paketler kesimlere gelen CPU boşaltabilirsiniz LSO faydası olur. LSO avantajları hakkında daha fazla bilgi bulunabilir [performans Microsoft ağ bağdaştırıcısı belgelerinde](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

LSO etkinleştirildiğinde, Azure müşterilerine gerçekleştirme paket yakaladığında büyük çerçeve boyutları görebilirsiniz. Bu büyük çerçeve boyutları, bazı müşteriler bu parçalanma veya MTU olmadığında kullanılan bir jumbo neden olabilir. LSO ile daha büyük bir TCP paketi oluşturmak için TCP/IP'yi yığınına daha büyük bir MSS ethernet bağdaştırıcısını tanıtabilirsiniz. Tüm bu segmentlere ayrılmamış çerçeve için Ethernet bağdaştırıcısını ardından iletilir ve VM üzerinde gerçekleştirilen bir paket yakalama içinde görünür olur. Ancak, paketi çok sayıda küçük çerçevelere Ethernet bağdaştırıcının MTU göre Ethernet bağdaştırıcısı tarafından ayrılır.

### <a name="tcpmss-window-scaling-and-pmtud"></a>TCP/MSS pencere ölçeklendirme ve GEÇİŞİNİ

#### <a name="explanation-of-tcp-mss"></a>TCP MSS açıklaması

TCP en büyük kesim boyutu (MSS), TCP paketlerinin parçalanma önlemek için en çok TCP kesimini boyutunu ayarlamak için hedeflenen bir ayardır. İşletim sistemleri MSS MSS genellikle ayarlayacak MTU - = IP & TCP üstbilgi boyutu (her 20 bayt veya toplam 40 bayt). Bu nedenle bir arabirim 1500 bir MTU ile bir MSS 1460, olacaktır. MSS ancak yapılandırılabilir değildir.

TCP oturumu kaynak ve hedef arasında ayarlandığında, bu ayar TCP üç yönlü anlaşma kabul edilmiş. Her iki tarafında da MSS değer gönderin ve iki alt TCP bağlantısı için kullanılır.

Azure VPN Gateway de dahil olmak üzere VPN ağ geçitleri gibi ara ağ cihazları, en iyi ağ performansını sağlamak için kaynak ve hedef bağımsız MTU ayarlamak için sahipsiniz. Bu nedenle, tek başına bir hedef ve kaynak MTU tek faktör, gerçek MSS değeri değil unutulmamalıdır.

#### <a name="explanation-of-path-mtu-discovery-pmtud"></a>Yol MTU'su bulma (GEÇİŞİNİ) açıklaması

MSS görüştü, ancak diğer ağ cihazları arasındaki kaynak ve hedef yolda olarak kullanılan gerçek MSS kaynak ve hedef daha küçük bir MTU değere sahip olabilir göstermeyebilir. Bu durumda, MTU paket küçük olan cihaz paket bırakma ve geri kendi MTU içeren bir Internet Denetim İletisi Protokolü (ICMP) parçalanma (Tür 3, kod 4) gerekli iletisi gönderin. Kaynak ana bilgisayar, uygun şekilde kendi yol MTU azaltmak bu ICMP iletisini sağlar. İşleme, yol MTU'su bulma verilir.

GEÇİŞİNİ işlemi kendiliğinden verimsizdir ve ağ performansı için olası etkilere sahiptir. Ardından ağ yollarını MTU aşan paket gönderildiğinde, bu paketleri ile daha düşük bir MSS yeniden gerekir. Gönderen MSS düşürün ve sürekli olarak ihtiyacı gönderici bilmez sonra ICMP parçalanma gerekli paket, belki de bir ağ Güvenlik Duvarı'nda (genellikle GEÇİŞİNİ kara delik adlandırılır), yol nedeniyle almazsa, paketi yeniden iletirken. Bu nedenle, Azure VM MTU artırma önermemekteyiz.

#### <a name="vpn-considerations-with-mtu"></a>MTU ile VPN ilgili önemli noktalar

Kapsülleme (örneğin, IPSec VPN'ler) gerçekleştiren Vm'leri kullanan müşteriler, paket boyutu ve MTU başka etkileri olabilir. VPN'leri ek üst bilgilere böylece paketi boyutunu artırmayı ve daha küçük bir MSS gerektiren özgün pakete eklenecek ekleyin.

Geçerli Azure 1350 bayt ve 1400 tünel arabiriminde MTU TCP MSS clamping ayarlamak için önerilir. Daha fazla bilgi şu adreste bulunabilir: [VPN cihazları ve IPSec/IKE parametreleri sayfası](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Gecikme süresi, gidiş-dönüş süresi ve TCP pencere ölçeklendirme

#### <a name="latency-and-round-trip-time"></a>Gecikme süresi ve gidiş dönüş süresi

Ağ gecikmesi ışık hızını fiber optik ağ üzerinden yönetilir. Gerçeğidir, TCP ağ verimliliği etkili bir şekilde yönetilen (pratik üst sınırlar) ayrıca iki ağ cihazları gidiş dönüş süresi (RTT) nedeniyle olur.

| | | | |
|-|-|-|-|
|Yol|Uzaklık|Tek yönlü zaman|Gidiş dönüş süresini (RTT)|
|New York to San Francisco|4,148 km|21 ms|42 ms|
|Londra'ya New York|5,585 km|28 ms|56 ms|
|Sidney için New York|15,993 km|80 ms|160 ms|

Bu tabloda, iki konum arasında doğrusal uzaklık gösterilmektedir ancak ağlarda uzaklık genellikle doğrusal uzaklığı uzun olur. En düşük RTT ışık hızına göre yönetilen olarak hesaplamak için basit bir formül: en düşük RTT = 2 * (mesafeyi kilometre / yayılmasını hızlandırmak).

Standart bir değeri 200 yayma hızı için kullanılabilir - 1 milisaniye cinsinden uzaklık ölçümleri açık olarak geçen bir değerdir.

Örnekte New York için San Francisco, doğrusal 4,148 km mesafe bulunur. Minimum RTT = 2 * (4,148 / 20). Eşitlik çıktısı, milisaniye cinsinden belirtilir.

Aralarındaki en küçük uzaklığı ile hedefler seçmek için en mantıklı seçenektir gerekli, en yüksek ağ performansı, sabit bir gerçeklik fiziksel uzaklığı iki konum arasında olduğu gibi. Ürüne, tasarım kararlarını sanal ağ içinde trafiği yolunu iyileştirmek ve gecikme süresini azaltmak için yapılabilir. Bu sanal ağ konuları ağ tasarımı konuları bölümünde açıklanmıştır.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>TCP gecikme süresi ve gidiş dönüş süresi etkileri

Gidiş dönüş süresini (RTT) en yüksek TCP Aktarım doğrudan etkisi vardır. TCP protokolü pencere boyutunu kavramı vardır. Pencere boyutu en fazla gönderen alıcıdan bildirim almanız önce bir TCP bağlantı üzerinden gönderilen trafiği miktarıdır. Ardından alıcıdan alındı almanız önce TCP MSS 1460 ayarlanır ve TCP pencere boyutu 65535 ayarlanırsa gönderen 45 paketleri gönderebilirsiniz. Bildirim gönderen ileteceğini sonra alınmazsa. Bu örnekte, TCP pencere boyutu / TCP MSS gönderilen paketlerin =. Veya 65535 / 1460 yuvarlanır en çok 45.

Bu "için onay bekleniyor" durumu, bir güvenilir teslim verileri oluşturmak için bir mekanizma ne TCP aktarım hızını etkilemek RTT'ne etkili bir şekilde neden olur. Uzun süre için bildirim gönderen bekler, uzun, ayrıca daha fazla veri göndermeden önce beklemeniz gerekir.

En fazla aktarım hızı, tek bir TCP bağlantısı hesaplama formülü aşağıdaki gibidir: Pencere boyutu / (RTT, milisaniye cinsinden gecikme süresi / 1000) en fazla bayt/saniye =. Aşağıdaki tabloda megabayt cinsinden, okunabilmesi için biçimlendirilmiştir ve en fazla megabayt gösteren ikinci verimini tek bir TCP bağlantı başına /.

| | | | |
|-|-|-|-|
|TCP pencere boyutu bayt cinsinden|RTT gecikme süresi<br/>Milisaniye cinsinden|Maksimum<br/>İkinci işleme başına megabayt|Maksimum<br/> Megabit / ikinci aktarım hızı|
|65535|1|65.54|524.29|
|65535|30|2.18|17.48|
|65535|60|1.09|8.74|
|65535|90|.73|5.83|
|65535|120|.55|4.37|

Paket kaybı ise, zaten gönderdiği verileri gönderen edinceye sırada ardından, TCP bağlantısı en fazla üretilen iş azaltır.

#### <a name="explanation-of-tcp-window-scaling"></a>TCP pencere ölçeklendirme açıklaması

TCP pencere ölçeklendirme TCP penceresi daha fazla veri sağlayan bir onay istenmeden önce gönderilecek boyutunu dinamik olarak artıran bir kavramdır. Bir bildirim gerektirilmediği sırada önceki örneğimizde 45 paketler gönderilir. Sayı, önce bir bildirim artırılır, gönderilen paketlerin, daha sonra TCP en fazla aktarım hızı da bir gönderen için onayı bekliyor sayısını azaltarak artırılır.

TCP aktarım hızı, bir basit aşağıdaki tabloda gösterilmiştir:

| | | | |
|-|-|-|-|
|TCP pencere boyutu<br/>Bayt|Milisaniye cinsinden RTT gecikme süresi|Maksimum<br/>İkinci işleme başına megabayt|Maksimum<br/> Megabit / ikinci aktarım hızı|
|65535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Ancak, TCP üst bilgisi için TCP pencere boyutu yalnızca 2 bayt uzunluğunda 65535 alma penceresi için en yüksek değer olduğu anlamına gelir değerdir. En fazla pencere boyutunu artırmak için bir TCP penceresi ölçek faktörü kullanıma sunulmuştur.

Ölçek faktörü de bir işletim sisteminde yapılandırılmış bir ayardır. TCP pencere boyutu kullanarak ölçek Etkenler hesaplama formülü aşağıdaki gibidir: TCP pencere boyutu = TCP pencere boyutu bayt cinsinden \* (2 ^ Ölçeklendirme çarpanı). Pencere ölçek faktörü 3 ve pencere boyutunu 65535 ise, hesaplama aşağıdaki gibidir: 65535 \* (2 ^ 3) = 262,140 bayt. Bir ölçek faktörü 14, sonuçları bir TCP pencere boyutu 14 (izin verilen en büyük uzaklık) ve ardından TCP pencere boyutu 1,073,725,440 bayt (8,5 Gigabit) olacaktır.

#### <a name="support-for-tcp-window-scaling"></a>TCP pencere ölçeklendirme desteği

Windows üzerinde farklı bir ölçekleme faktörü ayarlama özelliğini sahip bir bağlantı türü ayrı - bağlantılar (veri merkezi, internet ve benzeri) birkaç sınıfı vardır. Get-NetTCPConnection powershell komutuyla penceresi ölçeklendirme bağlantı sınıflandırma görebilirsiniz.

```powershell
Get-NetTCPConnection
```

Get-NetTCPSetting powershell komutuyla her bir sınıfın değerleri görebilirsiniz.

```powershell
Get-NetTCPSetting
```

İlk TCP pencere boyutu ve TCP Ölçeklendirme çarpanı içinde Windows kümesi NetTCPSetting powershell komutu ayarlanabilir. Daha fazla bilgi şu adreste bulunabilir: [NetTCPSetting ayarlama sayfası](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

```powershell
Set-NetTCPSetting
```

AutoTuningLevel etkin TCP ayarlarını aşağıdaki gibidir.

| | | | |
|-|-|-|-|
|AutoTuningLevel|Ölçeklendirme çarpanı|Ölçeklendirme çarpanı|Formülü<br/>en fazla pencere boyutunu Hesapla|
|Devre dışı|None|None|Pencere boyutu|
|Kısıtlı|4|2^4|Pencere boyutu * (2 ^ 4)|
|Yüksek oranda kısıtlanmış|2|2^2|Pencere boyutu * (2 ^ 2)|
|Normal|8|2^8|Pencere boyutu * (2 ^ 8)|
|Deneysel|14|2^14|Pencere boyutu * (2 ^ 14)|

Bu ayarlar büyük olasılıkla TCP performans üzerindeki etkisi olsa da, Azure, denetimin dışında kalan Internet üzerinden diğer birçok faktörü TCP performansını da etkileyebilir unutulmamalıdır.

#### <a name="increase-mtu-size"></a>MTU boyutunu artırın

"Daha büyük bir MTU daha büyük bir MSS oluşturucusunu MTU artırma TCP performansını artırabilir" mantıksal bir soru mu? Basit çözüm değildir büyük olasılıkla –. Açıklandığı gibi Artıları ve eksileri paket boyutu için yalnızca TCP trafiği için geçerli olan vardır. Etkileyen en önemli olan faktör, yukarıda açıklandığı gibi TCP aktarım hızı performansı TCP pencere boyutu, paket kaybı ve RTT ' dir.

> [!IMPORTANT]
> Azure, Azure müşterilerine sanal makinelerde varsayılan MTU değer değiştirme önermez.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Hızlandırılmış ağ iletişimi ve Alma Tarafı Ölçeklendirmesi

#### <a name="accelerated-networking"></a>Hızlandırılmış Ağ

Sanal makine ağ işlevleri, VM Konuk hem hiper yönetici/konak yoğun CPU geçmişte bırakılıyordu. Ana bilgisayar üzerinden transits her paket, CPU - tüm sanal ağ kapsülleme/de-capsulation dahil olmak üzere ana bilgisayar tarafından yazılımda işlenir. Bu nedenle, daha fazla trafik, konak ve ardından yüksek CPU yükü gider. Ve ana bilgisayar CPU diğer işlemleri yapılması meşgul ise, ardından, ayrıca ağ aktarım hızı ve gecikme süresini etkiler. Hızlandırılmış ağ ile bu sorun giderilmiştir.

Hızlandırılmış ağ, Azure'nın şirket içi programlanabilir donanım ve SR-IOV gibi teknolojiler aracılığıyla tutarlı çok düşük ağ gecikme süresi sağlar. Taşıyarak Azure'nın yazılım tanımlı ağ yığınının çoğunun kapalı CPU ve döngüleri, daha az yük VM'de koyma, değişim ve tutarsızlık gecikme süresi azaltma son kullanıcı uygulamaları tarafından kazanılır FPGA tabanlı SmartNICs işlem. Diğer bir deyişle, performans, daha fazla belirlenebilen olabilir.

Hızlandırılmış ağ performans geliştirmeleri, Konuk VM konağı atlayıp bir konağın SmartNIC ile doğrudan bir datapath kurmak vererek ulaşır. Hızlandırılmış ağ avantajları şunlardır:

- **Daha düşük gecikme süresi / daha yüksek paket / saniye (pps)**: Sanal anahtar datapath kaldırılıyor ana bilgisayar ilke işleme için paketler için harcadığınız zamanı kaldırır ve sanal makine içinde işlenebilecek paketlerin sayısını artırır.

- **Değişimi azaltılmış**: Sanal anahtar işleme uygulanması gereken ilke miktarını ve işleme yapılıyor CPU iş yüküne bağlıdır. İlke zorlaması için donanım yük boşaltma, konak VM iletişim ve tüm yazılım kesmelerini ve bağlam anahtarları kaldırılıyor doğrudan VM paketleri sunarak bu değişkenlik kaldırır.

- **Düşük CPU kullanımı**: Konakta sanal anahtar atlayarak ağ trafiğini işlemek için daha az CPU kullanımına neden olur.

Hızlandırılmış ağ, VM başına temelinde açıkça etkinleştirilmelidir. Hızlandırılmış ağ bir VM'de etkinleştirmek için yönergeleri [sayfası hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

#### <a name="receive-side-scaling-rss"></a>Alma Tarafı Ölçeklendirmesi (RSS)

Alma Tarafı Ölçeklendirmesi dağıtarak, ağ trafiğinin alma daha verimli bir şekilde dağıtan bir ağ sürücüsü teknoloji alma işlemi birden çok CPU'lar arasında birden çok işlemcili sistemde olduğundan. Basit bir deyişle, RSS, tüm kullanılabilir CPU'ların yerine yalnızca bir tane kullandığından alınan trafik büyük bir miktarını işlemek bir sistem sağlar. RSS daha teknik bir tartışma şu yolda bulunabilir: [giriş sayfasına Alma Tarafı Ölçeklendirmesi](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

RSS, bir VM'de hızlandırılmış ağ etkin olduğunda en yüksek performansı elde etmek için gereklidir. Olabilir avantajları hızlandırılmış ağ etkin olmayan VM'ler üzerinde RSS kullanma. RSS etkin ve bunu etkinleştirmek için yapılandırma bulunabilir belirlemek nasıl bir genel bakış [İyileştir ağ aktarım hızı için Azure sanal makineler sayfasındaki](http://aka.ms/FastVM).

### <a name="tcp-time-wait-and-time-wait-assassination"></a>Assassination TCP bekleme süresi ve saat bekleyin

Ağ ve uygulama performansını etkileyen başka bir yaygın sorun TCP bekleme süresi bir ayardır. Açma ve kapatma ya da bir istemci veya sunucu (kaynak IP:Source bağlantı noktası + hedef IP:Destination bağlantı noktası), TCP, normal işlem sırasında olarak çok sayıda yuva meşgul Vm'leri üzerinde belirli bir yuva bir süre bekleme durumunda önemli bir süre boyunca kalabilirsiniz. Bu "bekleme süresi" durumu kapatmadan önce bir yuvada teslim edilecek herhangi bir ek veriyi izin vermek için yöneliktir. Bu nedenle, TCP/IP'yi yığınları genellikle yeniden bir yuva sessizce istemcilerin TCP SYN paketi bırakarak engelle.

Bu zaman bir yuva bekleme durumuna yapılandırılabilir, ancak 30 saniyeden 240 saniye olarak değişiklik gösterebilir zaman miktarıdır. Yuva sınırlı bir kaynak olan ve belirli bir zamanda kullanılabilir yuva sayısını yapılandırılabilir (genellikle kaynaklandığını yaklaşık 30.000 olası yuva sayısı). Sessizce bırakılan TCP SYN paketleri gibi bu sayı bitmeye yaklaştıkça, veya istemciler ve sunucular eşleşmeyen bir saat bekleyin ayarları varsa ve bir VM zaman bekleme durumuna yuvası yeniden dener, yeni bağlantılar başarısız olur.

Genellikle bağlantı noktası aralığı giden yuva hem de TCP bekleme süresi ayarları ve yuva yeniden kullanmak için değeri içinde TCP/IP yığınının bir işletim sisteminin yapılandırılabilir. Bu numaraları değiştirme potansiyel olarak ölçeklenebilirliği artırabilirsiniz ancak senaryoya bağlı olarak birlikte çalışabilirlik sorunlarını neden olabilirdi ve dikkatli değiştirilmelidir.

Saat bekleyin Assassination adlı bir özellik sınırlaması ölçeklendirme bunu ele almak için kullanıma sunulmuştur. Yeni bağlantının IP paketi sıra numarasına önceki bağlantıdan son paket sırası sayısını aştığında gibi belirli senaryolar altında kullanılabilmeleri yuva saat bekleyin Assassination sağlar. Bu durumda, işletim sisteminin yeni bir bağlantı kurulması izin verir (yeni SYN ACK kabul eder) ve zorla süre bekleme durumu önceki bağlantıyı kapatın. Bu özellik azure'daki Windows sanal makinelerinde bugün desteklenir ve diğer VM'lerin içinde desteği ile ilgili işletim sistemi satıcıları Azure müşterileri tarafından incelenmesi gerekiyor.

TCP bekleme süresi ayarları ve kaynak bağlantı noktası aralığı yapılandırma belgeler şu adreste [ağ performansını artırma sayfasına değiştirilebilir ayarları](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Sanal ağ faktörleri performansını etkileyebilir

### <a name="vm-maximum-outbound-throughput"></a>VM en fazla giden işleme

Azure, çeşitli VM boyutları ve türleri, her performans özelliklerini farklı bir karışımını sunar. Ağ aktarım hızı (veya bant genişliği) megabit (Mbps) cinsinden bir performans özelliği var. Paylaşılan donanımda barındırılan sanal makineler için ağ kapasitesi oldukça aynı donanımı paylaşımı sanal makineler arasında paylaşılması gerekir. Büyük sanal makinelerin daha küçük sanal makineler görece daha fazla bant genişliği ayrılır.

Her sanal makineye ayrılan ağ bant genişliği, çıkış (giden) trafiğine, sanal makineden ölçülür. Tüm ağ trafiğini sanal makineyi bırakmak doğru ayrılmış sınır, hedef bağımsız olarak sayılır. Örneğin, bir sanal makine bir 1000 MB/sn sınırı varsa, aynı sanal ağda ya da Azure dışındaki başka bir sanal makineye giden trafiği hedeflenen olup bu sınırı geçerlidir.
Giriş yok ölçülen veya doğrudan sınırlı. Ancak, gelen verileri işlemek için bir sanal makinenin becerisini etkileyebilir, CPU ve depolama limitleri gibi diğer faktörlere de vardır.

Hızlandırılmış ağ gecikme süresi, aktarım hızı ve CPU kullanımı gibi ağ performansını iyileştirmek için tasarlanmış bir özelliktir. Hızlandırılmış ağ, sanal makinenin aktarım hızını artırabilir, ancak sanal makinenin en fazla bant genişliği ayrılan yalnızca, bunu yapabilirsiniz.

Azure sanal makineleri, Mayıs ancak birkaç, ağ arabirimleri bağlı olması gerekir. Bir sanal makineye ayrılan bant genişliği giden tüm trafiği bir sanal makineye bağlı tüm ağ arabirimleri arasında toplamıdır. Diğer bir deyişle, kaç ağ arabirimleri için sanal makineye bağlı bağımsız olarak sanal makine başına ayrılan bant olur.
 
Giden beklenen aktarım hızıyla ve her sanal makine boyutu tarafından desteklenen ağ arabirimlerinin sayısını ayrıntılı burada. En yüksek aktarım görmek için genel amaçlı gibi bir türü seçin sonra boyutu serisi Dv2 serisi gibi elde edilen sayfasında seçin. Her bir seri olan bir tablo, belirtimleri başlıklı, en fazla NIC son sütunda ağ ile / beklenen ağ performansı (Mbps).

Aktarım hızı sınırı, sanal makine için geçerlidir. Aktarım hızı, aşağıdaki faktörlerden etkilenmez:

- **Ağ arabirimleri sayısı**: Bant genişliği sınırı, sanal makineden giden tüm trafik toplu.

- **Hızlandırılmış**: Özellik yayımlanan sınırını elde etmeye yardımcı olabilir ancak sınırı değiştirmez.

- **Trafiği hedef**: Tüm hedeflere giden sınırında sayılır.

- **Protokol**: Tüm protokoller üzerinden giden tüm trafiği, sınırında sayılır.

A [VM türü başına en yüksek bant genişliği, tablo, bu sayfasını ziyaret ederek bulunabilir](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ve üzerinde kendi VM türü. Her türü sayfasında, bir tablo en fazla NIC ve beklenen maksimum ağ bant genişliğini gösterir.

VM ağ bant genişliği hakkında daha fazla bilgi şu adreste bulunabilir: [sanal makine ağ bant genişliği](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Internet performansla ilgili önemli noktalar

Bu makalede anlatıldığı gibi faktörleri İnternet'e ve Azure'nın denetimin dışında kalan ağ performansını etkileyebilir. Bu faktörler şunlardır:

- **Gecikme süresi**: İki hedefler arasında gidiş dönüş süresi Ara ağlardaki trafiği "kısa" uzaklık yolu mümkün ve yetersiz eşleme yollarındaki alma değil sorunları etkilenebilir

- **Paket kaybı**: Paket kaybı oluşabilir, Ağ Tıkanıklığı, fiziksel yol sorunlarını ve ağ cihazları altında gerçekleştirme

- **MTU boyutu/parçalanma**: Yol boyunca parçalanma için veri alma veya düzensiz, gelen paketlerin teslim paketlerinin etkileyebilir gecikmelere neden olabilir

Traceroute, ağ performans özellikleri boyunca her bir kaynak ve hedef aygıt arasında ağ yolu (örneğin, paket kaybı ve gecikme) ölçmek için iyi bir araçtır.

### <a name="network-design-considerations"></a>Ağ tasarımı konuları

Yukarıdaki konuları yanı sıra, bir sanal ağ topolojisini sanal ağ performansını etkileyebilir. Veri toplama merkezleri trafiği küresel olarak tek bir hub sanal ağ için ağ gecikme süresi sunar ve böylece genel ağ performansı efekt Örneğin, bir merkez ve uç tasarım. Benzer şekilde, ağ trafiğini geçtiği ağ cihazların sayısını, toplam gecikme süresi etkileyebilir. Trafiği bir bileşen ağ sanal Gereci ve Hub sanal gereç Internet'i yapılandırmamız önce geçen, örneğin, hub ve bağlı bileşen tasarımında, daha sonra gecikme süresi ağ sanal Gereçleri ile tanıtılabilir.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure bölgeleri, sanal ağlar ve gecikme süresi

Azure bölgeleri genel bir coğrafi bölge içinde bulunan birden çok veri merkezinde oluşur. Bu veri merkezleri, diğer her yanındaki fiziksel olarak olmayabilir ve bazı durumlarda kadar 10 kilometre tarafından ayrılmış. Azure'nın fiziksel veri merkezi ağı üzerinde bir mantıksal katmana sanal ağdır ve sanal ağ, veri merkezinde herhangi belirli ağ topolojisi göstermez. Örneğin, bir VM ve VM B aynı sanal ağ ve alt olan ancak farklı raflar içinde satır veya hatta veri merkezleri olabilir. Bunlar, fiber optik kablo ayak veya kilometre fiber optik kablo ayrılmış. Bu gerçekte, farklı sanal makineler arasında değişken gecikme (birkaç milisaniye fark) neden olabilir.

Bu coğrafi yerleştirme ve bu nedenle iki VM arasında gecikme süresi ile kullanılabilirlik kümeleri ve kullanılabilirlik bölgeleri yapılandırmasını etkileyen, ancak, bölgeye özgü ve genellikle etkileyen bir bölgedeki veri merkezleri arasında daha fazla mesafe Veri Merkezi bölgesi topolojisinde.

### <a name="source-nat-port-exhaustion"></a>Kaynak NAT bağlantı noktası tükenmesi

Azure'da bir dağıtımı, Azure genel Internet ve/veya genel IP alanı dışında uç noktaları ile iletişim kurabilir. Örneği bu giden bağlantı başlattığında, Azure genel IP adresi için özel IP adresini dinamik olarak eşler. Bu eşleme oluşturulduktan sonra dönüş trafiği giden bu kaynaklı akış için Ayrıca özel IP adresini akışı geldiği ulaşabilirsiniz.

Giden her bağlantı için Azure Load Balancer Bu eşleme belirli bir süre için sürdürmeniz gerekir. Azure ile çok kiracılı yapısı, bu eşleme için her giden akış her VM için koruma, kaynak kullanımı yoğun olabilir. Bu nedenle, ayarlayın ve Azure sanal ağ yapılandırmasına bağlı olarak bir sınırı yoktur. Veya daha kesin olarak - belirtilen bir Azure sanal makinesi yalnızca belirli sayıda giden bağlantılar belirli bir zamanda yapabilirsiniz. Bu sınırları tükendi, Azure VM yapmasını herhangi bir ek giden bağlantı engellenir.

Bu, ancak yapılandırılabilir, davranıştır. Hakkında daha fazla bilgi için [SNAT ve SNAT bağlantı noktası tükenmesi], bkz: [bu makalede](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>Azure'da ölçü ağ performansı

Performans üst sınırlar bu makalede bir dizi ağ gecikmesine ilişkili / gidiş dönüş süresini (RTT) iki VM arasında. Bu bölümde, gecikme süresi/RTT, hem de TCP performans ve VM ağ performansını test etme için bazı öneriler sağlar. Yukarıda açıklanan TCP/IP'yi & ağ değerler ayarlanmış ve aşağıda açıklanan teknikleri kullanarak performansı test. Gecikme süresi, MTU MSS ve pencere boyutunu değerlerini yukarıda listelenen hesaplamalarında kullanılabilir ve teorik bir üst sınırlar test sırasında gözlemlenen gerçek değerleri karşılaştırılabilir.

### <a name="measure-round-trip-time-and-packet-loss"></a>Ölçü gidiş dönüş süresi ve paket kaybı

TCP performans yoğun RTT ve paket kaybı kullanır. Ölçüm RTT ve paket kaybı için en basit yolu, Windows ve Linux'taki ping yardımcı kullanıyor. Ping çıktısını bir kaynak ve hedef olarak paket kaybı arasındaki en düşük/en yüksek/ortalama gecikme süresi gösterilir. Ping, varsayılan olarak ICMP protokolünü kullanır. TCP RTT test etmek için ardından PsPing kullanılabilir. PsPing hakkında daha fazla bilgi edinilebilir [bu bağlantıyı](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>TCP bağlantısı gerçek aktarım hızı ölçümü

NTttcp bir Linux veya Windows VM TCP performansını test etmek için kullanılan bir araçtır. Çeşitli TCP ayarları tweaked ve avantajları NTttcp kullanarak test. Aşağıdaki bağlantılarda NTttcp hakkında daha fazla bilgi bulunabilir.

- [Bant genişliği/aktarım hızı (NTttcp) test etme](https://aka.ms/TestNetworkThroughput)

- [NTttcp yardımcı programı](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Ölçü, bir sanal makinenin gerçek bant genişliği

Farklı VM türleri, hızlandırılmış ağ ve benzeri, performans testi, Linux ve Windows üzerinde de kullanılabilir olan Iperf adında bir araç kullanılarak sınanabilir. Iperf, TCP veya UDP, genel ağ performansını test etmek için kullanabilirsiniz. Iperf kullanarak TCP performans testleri (gecikme süresi, RTT ve benzeri) Bu makalede ele alınan faktörler tarafından etkilenir. Bu nedenle, UDP, yalnızca en yüksek aktarım hızı testi için daha iyi sonuçlara neden olabilir.

Aşağıdaki ek bilgiler bulunabilir:

- [Expressroute ağ performansıyla ilgili sorunları giderme](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Bir sanal ağa yönelik VPN aktarım hızını doğrulama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Verimsiz TCP davranışlarını algılayın

Azure müşterileri, TCP paketleri TCP bayrakları (SACK DUP ACK, yeniden aktarım ve hızlı yeniden) ile ağ performansı sorunlarını gösterebilecek paket yakalamaları görebilirsiniz. Bu paketleri, özellikle ağ verimsizlikleri paket kaybı sonucu olarak belirtin. Ancak paket kaybı Azure performans sorunları nedeniyle olmak zorunda değildir. Performans sorunlarını, uygulama, işletim sistemi veya doğrudan Azure platformuna ilgili olabilir değil diğer sorunlar olabilir. Ayrıca, bazı aktarım veya bir ağ üzerinde yinelenen ack'lerini gösteriyor normal – TCP protokolleri güvenilir olarak oluşturulmuş dikkat etmek önemlidir. Ayrıca, aşırı olmadıkları sürece bu TCP paketleri paket yakalaması kanıtı mutlaka bir sistemle ilgili ağ sorun olduğu anlamına gelmez.

Ancak, bu açıkça bu paket türlerinin göstergelerden TCP aktarım hızı – diğer bölümlerinde açıklanan nedenlerle en yüksek performansı elde değil olduğundan belirtilen.
