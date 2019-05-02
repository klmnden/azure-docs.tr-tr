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
ms.date: 04/02/2019
ms.author:
- rimayber
- dgoddard
- stegag
- steveesp
- minale
- btalb
- prachank
ms.openlocfilehash: 31ca0ee666ff37afa37fb9636860c557d92a52c7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924785"
---
# <a name="tcpip-performance-tuning-for-azure-vms"></a>TCP/IP'yi performans Azure Vm'leri için ayarlama

Bu makalede, yaygın TCP/IP'yi performans ayarlama teknikleri ve bunları Azure'da çalışan sanal makineler için kullanırken dikkat etmeniz gerekenler açıklanmaktadır. Bu teknikler temel bir bakış sağlar ve bunların nasıl belirleyebileceğinizi keşfedin.

## <a name="common-tcpip-tuning-techniques"></a>Ortak TCP/IP'yi ayar teknikleri

### <a name="mtu-fragmentation-and-large-send-offload"></a>MTU parçalanma ve büyük gönderme boşaltması

#### <a name="mtu"></a>MTU

En fazla iletim birimi (MTU) bir ağ arabirimi üzerinden gönderilen bayt cinsinden belirtilen en büyük boyutunu (paket) çerçevesidir. MTU yapılandırılabilir bir ayardır. Varsayılan Azure Vm'lerinde MTU kullanıldığını ve 1.500 bayt küresel olarak çoğu ağ cihazları varsayılan ayarı.

#### <a name="fragmentation"></a>Parçalanma

Parçalanma, bir ağ arabiriminin MTU'su aşan bir paket gönderildiğinde gerçekleşir. TCP/IP yığınının paket bir arabirimin MTU için uygun daha küçük parçalara (parça) çalışmamasına neden olur. Parçalanma IP katmanında gerçekleşir ve temel alınan protokoldeki (örneğin, TCP) bağımsızdır. 2.000 baytlık bir paket 1.500 bir MTU ile bir ağ arabirimi üzerinden gönderildiğinde, paket 1.500 baytlık bir paket ve 500 baytlık bir paket halinde ayrılır.

Ağ cihazları arasındaki kaynak ve hedef yolda MTU ya da daha küçük parçalara paket parçalara ya da açılan paket olabilir.

#### <a name="the-dont-fragment-bit-in-an-ip-packet"></a>Bir IP paketi içinde bit parçalara ayırma

IP Protokolü üst bilgisindeki bir bayrak yoksa parça (DF) bittir. DF bit gönderen ve alıcı arasındaki yolu ağ cihazlarında paketin parçası olmayan gösterir. Bu bit çeşitli nedenlerle ayarlayabilirsiniz. (Bir örnek için bu makalenin "Yol MTU'su bulma" bölümüne bakın.) Bir ağ aygıtı parçalama bit kümesi içeren bir paket alır ve bu paket cihazın arabirimi MTU aşıyor, standart davranışlarını paket bırakmak için cihazdır. Cihaz, ICMP parçalanma gerekli iletisine paketin özgün kaynağına gönderir.

#### <a name="performance-implications-of-fragmentation"></a>Parçalanma performans etkileri

Parçalanma, olumsuz etkileri olabilir. CPU/bellek parçalanması etkisini ve paketlerin yeniden performans üzerindeki etkisi için temel nedenlerinden biridir. Bir ağ aygıtının bir paket gerektiğinde parçalanma gerçekleştirmek için CPU/bellek kaynakları tahsis gerekecektir.

Paket birleştirildiği aynı olur. Ağ aygıtı için onları özgün pakete yeniden birleştirmek alındığında kadar tüm parçaları depolamak vardır. Bu işlemi parçalanmasına ve yeniden gecikme da neden olabilir.

Diğer olası olumsuz olduğu çıkarımında parçalanma, parçalanmış paketlere sıralamaya gelebilir ' dir. Bazı ağ aygıtları türleri paketleri sıralamaya alındığında bunları bırakabilir. Bu durum oluştuğunda tüm paket da yeniden iletilemez gerekir.

Parça genellikle ağ güvenlik duvarları gibi güvenlik cihazlar tarafından bırakılan veya bir ağ cihazını alma zaman arabelleği tükendi. Bir ağ cihazını alma arabelleği tükendi, bir ağ aygıtı parçalanmış paketini yeniden birleştirmek çalışıyor, ancak paket reassume ve depolamak için bir kaynak yok.

Parçalanma farklı ağlar internet üzerinden bağlanırken gereklidir için parçalanma bir negatif işlemi ancak destek görülebilir.

#### <a name="benefits-and-consequences-of-modifying-the-mtu"></a>Avantajları ve sonuçları MTU değiştirme

Genel olarak bakıldığında, MTU artırarak daha verimli bir ağ oluşturabilirsiniz. Aktarılan her paketi, orijinal pakete eklenen üstbilgi bilgilerini içerir. Daha fazla paket parçalanmasını oluşturduğu zaman, yükü daha fazla üst bilgisi yoktur ve ağ daha az verimli sağlar.

Bir örnek aşağıda verilmiştir. Ethernet başlık boyutu 14 bayt artı çerçeve tutarlılığı sağlamak için bir 4 baytlık çerçeve onay dizisi ' dir. Bir bayt 2.000 paket gönderildiğinde, Ethernet yük 18 bayt ağda eklenir. Paket 1.500 baytlık bir paket ve 500 baytlık bir paket halinde parçalanmışsa, her paket Ethernet üstbilgisinin 36 bayt toplam 18 bayt olacaktır.

MTU artan daha verimli bir ağ mutlaka oluşturmaz unutmayın. Uygulamanın yalnızca 500 baytlık paketleri gönderirse, aynı üstbilgi yükü MTU 1.500 veya 9000 bayt olup sunulacaktır. Ağ yalnızca MTU tarafından etkilenen paket daha büyük boyutlar kullanıyorsa daha verimli olur.

#### <a name="azure-and-vm-mtu"></a>Azure ve VM MTU

Azure Vm'leri için MTU varsayılan 1500 bayttır. Azure sanal ağ yığınının paket 1,400 bayt parçalara dener. Ancak parçalama bit IP üstbilgisinde olarak ayarlandığında sanal ağ yığınının paket 2,006 bayt izin verir.

Vm'leri bir MTU 1.500 olsa bile paketler 1,400 bayt parçalarla ilgili olduğundan, sanal ağ yığınını kendiliğinden verimsiz olmadığını unutmayın. Ağ paketlerinin büyük bir yüzdesini 1,400 veya 1.500 bayttan çok daha küçük.

#### <a name="azure-and-fragmentation"></a>Azure ve parçalanma

Sanal ağ yığınının "sıralamaya parçaları," diğer bir deyişle, kendi özgün parçalanmış sırada ulşamasını yoksa parçalanmış paketlere bırakmak ayarlayın. Kasım FragmentSmack adlı 2018'de duyurulan çoğunlukla bir ağ güvenlik açığı nedeniyle bu paketler bırakılır.

Bir Linux çekirdeğinin parçalanmış IPv4 ve IPv6 paketlerini yeniden birleştirilmesinden işlenme üründe FragmentSmack olur. Bir uzak saldırgan bu zayıf hedef sistem üzerinde daha yüksek CPU ve hizmet reddine neden olabilir tetikleyici pahalı parça yeniden birleştirme işlemleri için kullanabilir.

#### <a name="tune-the-mtu"></a>MTU ayarlama

Diğer bir işletim sisteminde yaptığınız gibi bir Azure VM MTU yapılandırabilirsiniz. Ancak, yukarıda açıklanan azure'da oluşan parçalanma dikkate almanız gereken zaman, yapılandırmakta olduğunuz bir MTU.

VM MTU artırmak için müşterilerin öneririz yok. Bu tartışma, nasıl Azure MTU uygular ve parçalanma gerçekleştirir ayrıntılarını açıklamak için tasarlanmıştır.

> [!IMPORTANT]
>MTU artırılması, performansı artırmak için bilinen değildir ve uygulama performansı üzerinde olumsuz bir etkiye sahip olabilir.
>
>

#### <a name="large-send-offload"></a>Büyük Gönderme Boşaltması

Büyük Gönderme Boşaltması (LSO) ayrılmasını paketlerin Ethernet bağdaştırıcısına boşaltarak, ağ performansını iyileştirebilir. LSO etkinleştirildiğinde, TCP/IP yığınının bir TCP paket oluşturur ve Ethernet Bağdaştırıcısı iletmeden önce segment için gönderir. MTU için uygun ve bu işlem donanım gerçekleştirildiği Ethernet arabirimi yük boşaltma boyutları halinde paketler kesimlere gelen CPU boşaltabilirsiniz LSO faydası olur. LSO avantajları hakkında daha fazla bilgi için bkz: [destekleyen büyük gönderme boşaltması](https://docs.microsoft.com/windows-hardware/drivers/network/performance-in-network-adapters#supporting-large-send-offload-lso).

LSO Etkin olduğunda, paket yakalamaları gerçekleştirdikleri Azure müşterilerine büyük çerçeve boyutları görebilirsiniz. Bu büyük çerçeve boyutları parçalanma oluşan düşünme bazı müşteriler veya olmadığında büyük MTU kullanılmakta olduğunu neden olabilir. LSO ile daha büyük bir en büyük kesim boyutu (MSS) daha büyük bir TCP paketi oluşturmak için TCP/IP yığınının Ethernet bağdaştırıcısını tanıtabilirsiniz. Tüm bu segmentlere ayrılmamış çerçeve için Ethernet bağdaştırıcısını ardından iletilir ve VM üzerinde gerçekleştirilen bir paket yakalama içinde görünür olur. Ancak, paketi çok sayıda küçük çerçevelere Ethernet bağdaştırıcının MTU göre bir Ethernet bağdaştırıcısı tarafından ayrılır.

### <a name="tcp-mss-window-scaling-and-pmtud"></a>Pencere TCP MSS ölçeklendirme ve GEÇİŞİNİ

#### <a name="tcp-maximum-segment-size"></a>TCP en büyük kesim boyutu

TCP en büyük kesim boyutu (MSS) TCP paketlerinin parçalanma önler TCP kesimini boyutunu sınırlayan bir ayardır. İşletim sistemleri, genellikle MSS ayarlamak için bu formülü kullanır:

`MSS = MTU - (IP header size + TCP header size)`

IP üstbilgisi ve TCP üstbilgi 20 bayt veya 40 bayt toplam markalarıdır. Bu nedenle bir arabirim 1.500 bir MTU ile bir MSS 1,460, olacaktır. Ancak MSS yapılandırılabilir.

TCP oturumu bir kaynak ve hedef arasında ayarlandığında, bu ayar TCP üç yönlü anlaşma kabul edilmiş. Her iki tarafında da MSS değer gönderin ve iki alt TCP bağlantısı için kullanılır.

Kaynak ve hedef MTU MSS değeri belirlemek yalnızca faktörleri olmayan göz önünde bulundurun. Azure VPN Gateway de dahil olmak üzere VPN ağ geçitleri gibi ara ağ cihazları, en iyi ağ performansını sağlamak için kaynak ve hedef bağımsız olarak MTU ayarlayabilirsiniz.

#### <a name="path-mtu-discovery"></a>Yol MTU'su bulma

MSS görüştü, ancak kullanılabilir gerçek MSS belirtemez. Bu durum, diğer ağ cihazları arasındaki kaynak ve hedef yolda kaynak ve hedef daha küçük bir MTU değere sahip olabilir çünkü. Bu durumda, MTU paket küçük olan cihaz paket kaldıracağız. Cihaz, ICMP parçalanma gerekiyor (Tür 3, kod 4) kendi MTU içeren bir ileti geri gönderir. Kaynak ana bilgisayar, uygun şekilde kendi yol MTU azaltmak bu ICMP iletisini sağlar. İşleme, yol MTU'su bulma (GEÇİŞİNİ) denir.

GEÇİŞİNİ işlem verimsizdir ve ağ performansı etkiler. Paketler, ağ yolu MTU aşan gönderildiğinde paketleri ile daha düşük bir MSS da yeniden iletilemez gerekir. Gönderen parçalanma ICMP gerekiyor, belki de yolda bir ağ güvenlik duvarı nedeniyle iletisi değil, (genellikle olarak adlandırılan bir *GEÇİŞİNİ kara delik*), gönderen ihtiyaç duyduğu MSS düşürün ve sürekli olarak bilmez. paketi yeniden iletirken. Azure VM MTU artırma önermemekteyiz nedeni budur.

#### <a name="vpn-and-mtu"></a>VPN ve MTU

Kapsülleme (örneğin, IPSec VPN'ler) gerçekleştiren VM'ler kullanıyorsanız, paket boyutu ve MTU ile ilgili bazı ek hususlar vardır. Paket boyutunu artırır ve daha küçük bir MSS gerektirir VPN'ler paketleri, daha fazla üst bilgileri ekleyin.

Azure için TCP MSS 1.350 bayt clamping ayarlayın ve 1.400 MTU arabirimine tünel öneririz. Daha fazla bilgi için [VPN cihazları ve IPSec/IKE parametreleri sayfası](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

### <a name="latency-round-trip-time-and-tcp-window-scaling"></a>Gecikme süresi, gidiş-dönüş süresi ve TCP pencere ölçeklendirme

#### <a name="latency-and-round-trip-time"></a>Gecikme süresi ve gidiş dönüş süresi

Ağ gecikmesi ışık hızını fiber optik ağ üzerinden yönetilir. Ağ aktarım hızı TCP da verimli bir şekilde iki ağ cihazları arasındaki gidiş dönüş süresini (RTT) tabidir.

| | | | |
|-|-|-|-|
|**yol**|**uzaklık**|**Tek yönlü zaman**|**RTT**|
|New York to San Francisco|4,148 km|21 ms|42 ms|
|Londra'ya New York|5,585 km|28 ms|56 ms|
|Sidney için New York|15,993 km|80 ms|160 ms|

Bu tablo, iki konum arasında doğrusal uzaklığını gösterir. Ağlarda uzaklığı doğrusal uzaklık genellikle büyük/küçük harf uzundur. En düşük RTT ışık hızına göre yönetilen olarak hesaplamak için basit bir formül şu şekildedir:

`minimum RTT = 2 * (Distance in kilometers / Speed of propagation)`

200 yayma hızı için kullanabilirsiniz. Uzaklık ölçümleri, ışık 1 milisaniye cinsinden geçen budur.

New York San Francisco için örnek olarak alalım. Düz çizgi uzaklık 4,148 km ' dir. Bu değer eşitliğe takma, biz aşağıdaki alın:

`Minimum RTT = 2 * (4,148 / 200)`

Çıkış Denklemin milisaniye cinsindendir.

En iyi ağ performansı elde etmek istiyorsanız, bunlar arasında uzaklıklı hedefleri mantıksal seçeneği seçmektir. Ayrıca, trafiği yolunu iyileştirmek ve gecikme süresini azaltmak amacıyla sanal ağınızda tasarlamanız gerekir. Daha fazla bilgi için bu makalenin "Ağ tasarımları" bölümüne bakın.

#### <a name="latency-and-round-trip-time-effects-on-tcp"></a>TCP gecikme süresi ve gidiş dönüş süresi etkileri

Gidiş dönüş süresi en fazla TCP işleme doğrudan etkisi vardır. TCP protokolünde *pencere boyutunu* maksimum gönderen alıcıdan bildirim alması gereken önce bir TCP bağlantı üzerinden gönderilen trafik miktarı. TCP MSS 1,460 için ayarlanır ve TCP pencere boyutu için 65.535 gönderen alıcıdan bildirim almak sahip önce 45 paketleri gönderebilirsiniz. Bildirim gönderen katılmaz, verileri kurtarmasını. Formül şudur:

`TCP window size / TCP MSS = packets sent`

Bu örnekte, 65.535 / 1,460 yuvarlanır en çok 45.

Verileri güvenilir teslim emin olmak için bir mekanizma bu "için onay bekleniyor" durumu, TCP aktarım hızını etkilemek RTT ne neden olur. Uzun süre için bildirim gönderen bekler, artık daha fazla veri göndermeden önce beklemeniz gerekir.

Tek bir TCP bağlantı maksimum aktarım hızını hesapladığı için formül şudur:

`Window size / (RTT latency in milliseconds / 1,000) = maximum bytes/second`

Bu tabloda, en fazla megabayt gösterilmektedir / ikinci verimini tek bir TCP bağlantı başına. (Okunabilirlik açısından, megabayt kullanılır ölçü için.)

| | | | |
|-|-|-|-|
|**TCP pencere boyutu (bayt)**|**RTT gecikmesi (ms)**|**En fazla megabayt/saniye aktarım hızı**|**En fazla megabit/saniye aktarım hızı**|
|65,535|1|65.54|524.29|
|65,535|30|2.18|17.48|
|65,535|60|1.09|8.74|
|65,535|90|.73|5.83|
|65,535|120|.55|4.37|

Paket kayıpsa zaten gönderdiği verileri gönderen edinceye sırada en yüksek aktarım TCP bağlantısı indirim uygulanacaktır.

#### <a name="tcp-window-scaling"></a>TCP pencere ölçeklendirme

TCP pencere ölçeklendirme bir onay istenmeden önce gönderilecek daha fazla veri izin vermek için TCP pencere boyutunu dinamik olarak artıran bir tekniktir. Bir bildirim gerektirilmediği sırada önceki örnekte, 45 paketler gönderilir. Önce bir bildirim gerekli gönderilebilir paketlerin sayısını artırmak için bir gönderici TCP maksimum aktarım hızını artıran onayı bekliyor sayısını azaltma.

Bu tabloda, bu ilişkilerin gösterilmektedir:

| | | | |
|-|-|-|-|
|**TCP pencere boyutu (bayt)**|**RTT gecikmesi (ms)**|**En fazla megabayt/saniye aktarım hızı**|**En fazla megabit/saniye aktarım hızı**|
|65,535|30|2.18|17.48|
|131,070|30|4.37|34.95|
|262,140|30|8.74|69.91|
|524,280|30|17.48|139.81|

Ancak TCP pencere boyutu için TCP üstbilgi değeri yalnızca 2 bayt uzunluğunda 65.535 alma penceresi için en yüksek değer olduğu anlamına gelir. En fazla pencere boyutunu artırmak için TCP penceresi ölçek faktörü kullanıma sunulmuştur.

Ölçek faktörü Ayrıca, bir işletim sisteminde yapılandırabileceğiniz bir ayardır. TCP pencere boyutu ölçek Etkenler kullanarak hesaplama formülü aşağıdadır:

`TCP window size = TCP window size in bytes \* (2^scale factor)`

3 ve 65.535 pencere boyutu penceresi ölçek faktörü hesaplaması şu şekildedir:

`65,535 \* (2^3) = 262,140 bytes`

Bir ölçek faktörü 14 sonuçlarının bir TCP pencere boyutu 14 (izin verilen en büyük uzaklık). TCP pencere boyutu 1,073,725,440 bayt (8,5 Gigabit) olacaktır.

#### <a name="support-for-tcp-window-scaling"></a>TCP pencere ölçeklendirme desteği

Windows, farklı bağlantı türleri için farklı bir ölçekleme faktörü ayarlayabilirsiniz. (Veri merkezi, internet ve benzeri bağlantılarının sınıfları içerir.) Kullandığınız `Get-NetTCPConnection` PowerShell komutu, bağlantı türü ölçeklendirme penceresini görüntülemek için:

```powershell
Get-NetTCPConnection
```

Kullanabileceğiniz `Get-NetTCPSetting` PowerShell komutunu her sınıf değerlerini görüntülemek için:

```powershell
Get-NetTCPSetting
```

Kullanarak Windows ilk TCP pencere boyutu ve TCP Ölçeklendirme çarpanı ayarlayabilirsiniz `Set-NetTCPSetting` PowerShell komutu. Daha fazla bilgi için [kümesi NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps).

```powershell
Set-NetTCPSetting
```

Etkin TCP ayarlarını bunlar `AutoTuningLevel`:

| | | | |
|-|-|-|-|
|**AutoTuningLevel**|**Ölçeklendirme çarpanı**|**Ölçeklendirme çarpanı**|**Formülünü<br/>en fazla pencere boyutunu Hesapla**|
|Devre dışı|None|None|Pencere boyutu|
|Kısıtlı|4|2^4|Pencere boyutu * (2 ^ 4)|
|Yüksek oranda kısıtlanmış|2|2^2|Pencere boyutu * (2 ^ 2)|
|Normal|8|2^8|Pencere boyutu * (2 ^ 8)|
|Deneysel|14|2^14|Pencere boyutu * (2 ^ 14)|

Bu ayarlar, büyük olasılıkla TCP performans üzerindeki etkisi, ancak diğer birçok faktörü Azure, denetimin dışında kalan Internet üzerinden TCP performansını da etkileyebilir akılda tutulması yöneliktir.

#### <a name="increase-mtu-size"></a>MTU boyutunu artırın

Daha büyük bir MTU daha büyük bir MSS anlamına gelir çünkü MTU TCP performansını artırmak olup olmadığını merak ediyor. Olmayabilir. Artıları ve eksileri yalnızca TCP trafiği paket boyutu için vardır. Daha önce bahsedildiği gibi TCP işleme performansını etkileyen en önemli olan faktör TCP pencere boyutu, paket kaybı ve RTT ' dir.

> [!IMPORTANT]
> Azure müşterileri sanal makinelerde varsayılan MTU değer değiştirme önerilmemektedir.
>
>

### <a name="accelerated-networking-and-receive-side-scaling"></a>Hızlandırılmış ağ iletişimi ve Alma Tarafı Ölçeklendirmesi

#### <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi

Sanal makine ağ işlevleri, VM Konuk hem hiper yönetici/konak yoğun CPU geçmişte bırakılıyordu. Ana bilgisayar üzerinden transits her paket, CPU, tüm sanal ağ yalıtma ve kapsüllemeyi açma işlemi dahil olmak üzere ana bilgisayar tarafından yazılımda işlenir. Yüksek CPU, daha fazla trafik, ana bilgisayar üzerinden doğru şekilde yükleyin. Ve ana bilgisayar CPU diğer işlemleri ile meşgul ise, bu da ağ aktarım hızı ve gecikme süresini etkiler. Azure hızlandırılmış ağ ile bu sorunu giderir.

Hızlandırılmış ağ, şirket içi programlanabilir donanım Azure'dan ve SR-IOV gibi teknolojileri aracılığıyla tutarlı ultralow ağ gecikmesini sağlar. Hızlandırılmış ağ iletişimi çok Azure yazılım tanımlı ağ yığınının çoğunun FPGA tabanlı SmartNICs içine CPU'ları kapatıp taşır. Bu değişiklik, daha az yük değişimi ve tutarsızlık gecikme süresi azaltma, VM'de yerleştiren işlem döngüsünü geri kazanmak son kullanıcı uygulamaları sağlar. Diğer bir deyişle, performans, daha fazla belirlenebilen olabilir.

Hızlandırılmış ağ, Konuk VM konağı atlayıp bir konağın SmartNIC ile doğrudan bir datapath kurmak vererek performansını artırır. Hızlandırılmış ağ bazı avantajları şunlardır:

- **Daha düşük gecikme süresi / daha yüksek paket / saniye (pps)**: Sanal anahtar datapath kaldırılıyor paketleri içinde ana bilgisayar ilke işleme için harcadığınız zamanı ortadan kaldırır ve VM ile işlenebilen paketlerin sayısını artırır.

- **Değişimi azaltılmış**: Sanal anahtar işleme uygulanması gereken ilke miktarını ve işleme yapılıyor CPU iş yüküne bağlıdır. İlke zorlaması için donanım yük boşaltma, konak VM iletişim ve tüm yazılım kesmelerini ve bağlam anahtarları ortadan doğrudan sanal Makineye, paketleri sunarak bu değişkenlik kaldırır.

- **Düşük CPU kullanımı**: Konakta sanal anahtar atlayarak ağ trafiğini işlemek için daha az CPU kullanımına neden olur.

Hızlandırılmış ağ kullanmak için açık bir şekilde uygulanabilir her VM'de etkinleştirmeniz gerekir. Bkz: [hızlandırılmış ağ ile bir Linux sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) yönergeler için.

#### <a name="receive-side-scaling"></a>Alma Tarafı Ölçeklendirmesi

Alma Tarafı ölçeklendirme (RSS) dağıtarak, ağ trafiğinin alma daha verimli bir şekilde dağıtan bir ağ sürücüsü teknoloji alma işlemi birden çok CPU işlemcili bir sistemi olduğundan. Basit bir deyişle, RSS, tüm kullanılabilir CPU'ların yerine yalnızca bir tane kullandığından daha alınan trafiğini işlemek bir sistem sağlar. RSS daha fazla teknik bilgi için bkz. [Alma Tarafı Ölçeklendirmesi giriş](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

Bir VM'de hızlandırılmış ağ etkin olduğunda en iyi performansı elde etmek, RSS etkinleştirmeniz gerekir. RSS accelerated networking kullanmayan Vm'leri üzerinde avantajları da sağlayabilir. RSS etkin olup olmadığını belirleme ve nasıl etkinleştirileceği konusunda genel bakış için bkz: [Azure sanal makineleri için ağ verimini en iyi duruma getir](http://aka.ms/FastVM).

### <a name="tcp-timewait-and-timewait-assassination"></a>TCP TIME_WAIT ve TIME_WAIT assassination

TCP TIME_WAIT ağ ve uygulama performansını etkileyen başka bir ortak ayarıdır. Açma ve istemciler veya sunucular (kaynak IP:Source bağlantı noktası + hedef IP:Destination bağlantı noktası), TCP normal işlem sırasında olarak çok sayıda yuva kapatma meşgul Vm'leri üzerinde belirli bir yuva TIME_WAIT durumda uzun bir süredir kalabilirsiniz. TIME_WAIT durumunda bir yuvada kapatmadan önce teslim edilecek herhangi bir ek veriyi izin vermek için tasarlanmıştır. Bu nedenle TCP/IP'yi yığınları genellikle yeniden bir yuva sessizce istemcinin TCP SYN paketi bırakarak engelle.

Bir yuva TIME_WAIT içinde olduğu süre miktarını yapılandırılabilir. 30 saniyeden 240 saniye olarak değişiklik gösterebilir. Yuva sınırlı bir kaynaktır ve belirli bir zamanda kullanılabilir yuva sayısını yapılandırılabilir. (Kullanılabilir yuva genellikle yaklaşık 30.000 sayısıdır.) Kullanılabilir yuva tüketilen veya istemciler ve sunucular eşleşmeyen TIME_WAIT ayarları varsa ve bir VM TIME_WAIT durumundaki bir yuva yeniden dener, yeni bağlantılar, TCP SYN sessizce Bırakılan paketleri başarısız olur.

Giden yuva için bağlantı noktası aralığı değeri içinde bir işletim sisteminin TCP/IP yığınının genellikle yapılandırılabilir. Aynı şeyi TCP TIME_WAIT ayarları ve yuva yeniden kullanımı için geçerlidir. Bu numaraları değiştirme, potansiyel olarak ölçeklenebilirliği artırabilir. Ancak, duruma bağlı olarak bu değişiklikleri birlikte çalışabilirlik sorunlarına neden olabileceği. Bu değerleri değiştirirseniz dikkatli olmanız gerekir.

TIME_WAIT assassination ölçeklendirme bu sınırlama ele almak için kullanabilirsiniz. TIME_WAIT assassination gibi bazı durumlarda, yeni bağlantının IP paketi sıra numarasına önceki bağlantıdan son paket sırası sayısını aştığında kullanılabilmeleri yuva sağlar. Bu durumda, işletim sisteminin yeni bir bağlantı kurulması izin verir (yeni SYN/ACK kabul eder) ve zorla TIME_WAIT durumdaydı önceki bağlantıyı kapatın. Bu özellik, azure'da Windows sanal makinelerinde desteklenir. Diğer Vm'leri de desteği hakkında bilgi edinmek için işletim sistemi satıcıyla birlikte denetleyin.

TCP TIME_WAIT ayarları ve kaynak bağlantı noktası aralığı yapılandırma hakkında bilgi edinmek için [ağ performansını artırmak için değiştirilebilir ayarları](https://docs.microsoft.com/biztalk/technical-guides/settings-that-can-be-modified-to-improve-network-performance).

## <a name="virtual-network-factors-that-can-affect-performance"></a>Performansı etkileyen faktörleri sanal ağ

### <a name="vm-maximum-outbound-throughput"></a>VM en fazla giden işleme

Azure, çeşitli VM boyutları ve türleri, her performans özelliklerini farklı bir karışımını sağlar. Ağ aktarım hızı (veya bant genişliği), bu özelliklerin biri olan megabit (Mbps) cinsinden ölçülür. Paylaşılan donanımda barındırılan sanal makineler için ağ kapasitesi oldukça aynı donanımı kullanarak sanal makineler arasında paylaşılması gerekir. Büyük sanal makinelerin daha küçük sanal makineler daha fazla bant genişliği ayrılır.

Her sanal makineye ayrılan ağ bant genişliği, çıkış (giden) trafiğine, sanal makineden ölçülür. Tüm ağ trafiğini sanal makineyi bırakmak doğru ayrılmış sınır, hedef bağımsız olarak sayılır. Örneğin, bir sanal makine bir 1000 MB/sn sınırı varsa, aynı sanal ağda veya bir Azure dışındaki başka bir sanal makineye giden trafiği hedeflenen olup bu sınırı geçerlidir.

Giriş yok ölçülen veya doğrudan sınırlı. Ancak, gelen verileri işlemek için bir sanal makinenin yeteneğini etkileyebilir CPU ve depolama limitleri gibi diğer etkenler vardır.

Hızlandırılmış ağ gecikme süresi, aktarım hızı ve CPU kullanımı dahil olmak üzere ağ performansı artırmak için tasarlanmıştır. Hızlandırılmış ağ, sanal makinenin aktarım hızını artırabilir, ancak bunu, yalnızca sanal makinenin ayrılmış bant genişliği kadar yapabilirsiniz.

Azure sanal makineler, kendilerine iliştirilmiş en az bir ağ arabirimine sahiptir. Bunlar, çeşitli olabilir. Bir sanal makineye ayrılan bant genişliği giden tüm trafiği makineye bağlı tüm ağ arabirimleri arasında toplamıdır. Diğer bir deyişle, makineye bağlı ağ arabirimleri kaç bakılmaksızın her sanal makine olarak, bant genişliği ayrılır.

Giden beklenen aktarım hızıyla ve her sanal makine boyutu tarafından desteklenen ağ arabirimlerinin sayısı ayrıntılı olarak [boyutları için Windows azure'da sanal makineler](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). En yüksek aktarım görmek için bir tür gibi seçin **genel amaçlı**ve ardından (örneğin, "Dv2 serisi") elde edilen sayfa boyutu seriyle ilgili bölümü bulun. Her bir seri olarak adlandırılmıştır son sütunda ağ özellikleri sağlayan bir tablo yok "maks NIC / beklenen ağ bant genişliği (MB/sn)."

Aktarım hızı sınırı, sanal makine için geçerlidir. Aktarım hızı bu faktörlerden etkilenir değil:

- **Ağ arabirimleri sayısı**: Bant genişliği sınırını sanal makineden giden tüm trafiği toplamı için geçerlidir.

- **Hızlandırılmış**: Bu özellik yayımlanan sınırını elde etmeye yardımcı olabilir ancak bu sınırı değiştirmez.

- **Trafiği hedef**: Tüm hedeflere giden sınırında sayılır.

- **Protokol**: Tüm protokoller üzerinden giden tüm trafiği, sınırında sayılır.

Daha fazla bilgi için [sanal makine ağ bant genişliği](http://aka.ms/AzureBandwidth).

### <a name="internet-performance-considerations"></a>Internet performansla ilgili önemli noktalar

Bu makalede anlatıldığı gibi faktörleri İnternet'e ve Azure'nın denetimin dışında kalan ağ performansını etkileyebilir. Bu faktörlerin bazıları şunlardır:

- **Gecikme süresi**: İki hedefler arasında gidiş dönüş süresi Ara ağlardaki sorunları, uzaklık yolu "kısa" tepkilerden faydalanamamış olursunuz trafiği ve yetersiz eşleme yollarındaki tarafından etkilenebilir.

- **Paket kaybı**: Paket kaybı, Ağ Tıkanıklığı, fiziksel yol sorunlarını ve yeterli performansa sahip olmayan ağ cihazları tarafından kaynaklanabilir.

- **MTU boyutu/parçalanma**: Yol boyunca parçalanma paketlerin teslim etkileyebilecek gecikmeleri veri alma veya düzensiz, gelen paketlerin neden olabilir.

Traceroute, ağ performans özellikleri boyunca kaynak cihaz ile hedef aygıt arasındaki her ağ yolu (örneğin, paket kaybı ve gecikme) ölçmek için iyi bir araçtır.

### <a name="network-design-considerations"></a>Ağ tasarımı konuları

Bu makalede ele alınan konular yanı sıra, bir sanal ağ topolojisi ağ performansını etkileyebilir. Örneğin, veri toplama merkezleri genel olarak tek hub sanal ağa trafiği bir merkez ve uç tasarıma genel ağ performansı etkileyecek olan ağ gecikmesi başlatacaktır.

Ağ trafiğini geçtiği ağ aygıtlarının sayısını, toplam gecikme süresi olarak da etkileyebilir. Örneğin, internet'e ' i yapılandırmamız önce uç ağ sanal Gereci ve hub sanal Gereci aracılığıyla trafiği başarılı olursa bir merkez ve uç tasarımında, gecikme süresi ağ sanal Gereçleri çıkarabilir.

### <a name="azure-regions-virtual-networks-and-latency"></a>Azure bölgeleri, sanal ağlar ve gecikme süresi

Azure bölgeleri genel bir coğrafi bölge içinde bulunan birden çok veri merkezinde oluşur. Birbirinin yanına bu veri merkezleri fiziksel olarak olmayabilir. Bazı durumlarda kadar 10 kilometre tarafından ayrılmış. Azure fiziksel veri merkezi ağ üzerinde bir mantıksal katmana sanal ağdır. Bir sanal ağ, veri merkezindeki herhangi belirli ağ topolojisi kapsıyor değil.

Örneğin, aynı sanal ağ ve alt ağ olan iki sanal makine farklı raflar, satır veya hatta veri merkezleri içinde olabilir. Bunlar, veya kilometre fiber optik kablo, fiber optik kablo ayak tarafından ayrılmış. Bu farklılığa farklı sanal makineler arasında değişken gecikme (birkaç milisaniye fark) neden olabilirdi.

Coğrafi yerleşim VM'lerin yanı sıra, iki sanal makine arasındaki olası ortaya çıkan gecikme kullanılabilirlik alanları ve kullanılabilirlik kümelerini yapılandırma tarafından etkilenebilir. Ancak bölgeye özgü ve öncelikli olarak etkileyen bölgedeki veri merkezi topolojiye göre bir bölgedeki veri merkezleri arasındaki uzaklık.

### <a name="source-nat-port-exhaustion"></a>Kaynak NAT bağlantı noktası tükenmesi

Azure'da bir dağıtımı, Azure üzerinde genel internet ve/veya genel IP alanı içinde dışındaki uç noktaları ile iletişim kurabilir. Örneği bir giden bağlantı başlattığında, Azure genel IP adresi için özel IP adresini dinamik olarak eşler. Bu eşleme Azure oluşturduktan sonra giden kaynaklı akış için dönüş trafiği da özel IP adresini akışı geldiği ulaşabilirsiniz.

Giden her bağlantı için belirli bir süre için bu eşleme korumak Azure Load Balancer gerekir. Çok kiracılı Azure yapısı ile bu eşleme için her giden akış her VM için koruma kaynak yoğunluğu olabilir. Bu nedenle ayarlayın ve Azure sanal ağ yapılandırmasına bağlı olarak bir sınırı yoktur. Ya da daha kesin olarak söylemek için bir Azure sanal makinesi yalnızca giden bağlantılar belirli sayıda belirli bir zamanda yapabilir. Bu sınırlar ulaşıldığında, VM fazla giden bağlantı yapmanız mümkün olmayacaktır.

Ancak bu yapılandırılabilir, davranıştır. SNAT ve SNAT hakkında daha fazla bilgi için bağlantı noktası tükenmesi, bkz: [bu makalede](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections).

## <a name="measure-network-performance-on-azure"></a>Azure'da ölçü ağ performansı

Performans üst sınırlar bu makalede bir dizi ağ gecikme süresini ilişkili / gidiş dönüş süresini (RTT) iki VM arasında. Bu bölümde, gecikme süresi/RTT test etme ve TCP performans ve VM ağ performansını test etme için bazı öneriler sağlar. Ayar yapabilirsiniz ve performans testi Bu bölümde açıklanan teknikleri kullanarak daha önce açıklanan TCP/IP'yi ve ağ değerleri. Gecikme süresi, MTU, MSS ve pencere boyutunu değerlerini daha önce sağlanan hesaplamalar eklenebilecek ve test sırasında gözlemleyin gerçek değerlere teorik bir üst sınırlar karşılaştırın.

### <a name="measure-round-trip-time-and-packet-loss"></a>Ölçü gidiş dönüş süresi ve paket kaybı

TCP performans yoğun RTT ve paket kaybı kullanır. Windows ve Linux'taki PING yardımcı RTT ve paket kaybı ölçmek için en kolay yolunu sağlar. PING çıktısı, kaynak ve hedef arasında en düşük/en yüksek/ortalama gecikme süresi gösterilir. Paket kaybı de gösterilir. PING, varsayılan olarak ICMP protokolünü kullanır. PsPing TCP RTT test etmek için kullanabilirsiniz. Daha fazla bilgi için [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping).

### <a name="measure-actual-throughput-of-a-tcp-connection"></a>TCP bağlantısı gerçek aktarım hızı ölçümü

NTttcp, Linux veya Windows VM TCP performansını test etmek için kullanılan bir araçtır. Çeşitli TCP ayarlarını değiştirin ve ardından avantajları NTttcp kullanarak test. Daha fazla bilgi için şu kaynaklara bakın:

- [Bant genişliği/aktarım hızı (NTttcp) test etme](https://aka.ms/TestNetworkThroughput)

- [NTttcp yardımcı programı](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)

### <a name="measure-actual-bandwidth-of-a-virtual-machine"></a>Ölçü, bir sanal makinenin gerçek bant genişliği

Ağ hızlandırılmış farklı VM türleri performansını test etme ve böyle devam, iPerf adında bir araç kullanarak. iPerf ayrıca Linux ve Windows üzerinde kullanılabilir. iPerf, TCP veya UDP, genel ağ performansını test etmek için kullanabilirsiniz. iPerf TCP performans testleri (örneğin, gecikme süresi ve RTT) Bu makalede ele alınan faktörler tarafından etkilenir. Bu nedenle yalnızca en yüksek aktarım test etmek isterseniz UDP daha iyi sonuçlara neden.

Daha fazla bilgi için şu makalelere bakın:

- [Expressroute ağ performansıyla ilgili sorunları giderme](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-network-performance)

- [Sanal ağa yönelik VPN aktarım hızını doğrulama](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-validate-throughput-to-vnet)

### <a name="detect-inefficient-tcp-behaviors"></a>Verimsiz TCP davranışlarını algılayın

Paket yakalamaları TCP paketleri ağ performansı sorunlarını gösterebilecek olan TCP bayrakları (SACK DUP ACK, yeniden aktarım ve hızlı yeniden) ile Azure müşterileri görebilirsiniz. Bu paketler, paket kaybına neden ağ verimsizlikleri özellikle gösterir. Ancak paket kaybı Azure performans sorunlar nedeniyle olmak zorunda değildir. Performans sorunlarını, uygulama sorunlarını, işletim sistemi sorunları veya doğrudan Azure platformuna ilgili olabilir değil diğer sorunları sonucu olabilir.

Ayrıca, bazı aktarım ve yinelenen ack'lerini gösteriyor normal bir ağda olduğunu aklınızda bulundurun. TCP protokolleri, güvenilir olarak oluşturulmuştur. Aşırı oldukları sürece bu TCP paketleri paket yakalaması kanıtı mutlaka bir sistemle ilgili bir ağ sorunu göstermiyor.

Yine de bu paket TCP aktarım hızı bu makalenin diğer bölümlerinde açıklanan nedenlerle, en yüksek performansı elde etmek değil göstergelerden türleridir.

## <a name="next-steps"></a>Sonraki adımlar

TCP/IP'yi Azure Vm'leri için performans ayarlama hakkında öğrendiğinize göre diğer değerlendirmeler hakkında bilgi edinmek isteyebilirsiniz [sanal ağları planlama](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm) veya [bağlanma ve sanal ağları yapılandırma hakkında daha fazla bilgi edinin ](https://docs.microsoft.com/azure/virtual-network/).
