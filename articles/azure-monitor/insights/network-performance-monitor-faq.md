---
title: SSS - azure'da Ağ Performansı İzleyicisi çözüm | Microsoft Docs
description: Bu makalede, azure'da Ağ Performansı İzleyicisi hakkında sık sorulan sorular yakalar. Neredeyse gerçek zamanlı ağlarınızda performansını izlemek ve tespit edip bulmak Ağ Performansı İzleyicisi'ni (NPM) yardımcı olur, performans sorunlarını ağ.
services: log-analytics
documentationcenter: ''
author: vinynigam
manager: agummadi
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/12/2018
ms.author: vinynigam
ms.openlocfilehash: d573b7ad9edac6b1502744b61e85cba3402a6f68
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232663"
---
# <a name="network-performance-monitor-solution-faq"></a>Ağ Performansı İzleyicisi çözümü ile ilgili SSS

![Ağ Performansı İzleyicisi simgesi](media/network-performance-monitor-faq/npm-symbol.png)

Bu makalede sık sorulan sorular (SSS) azure'da Ağ Performansı İzleyicisi'ni (NPM) hakkında yakalar.

[Ağ Performansı İzleyicisi](/azure/networking/network-monitoring-overview) bulut tabanlı [hibrit ağ izleme](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md) yardımcı olan bir çözümü, ağ altyapınızın çeşitli noktalarında arasında ağ performansını izleme. Ayrıca, ağ bağlantısı izlemenize yardımcı olur [hizmet ve uygulama uç noktaları](../../azure-monitor/insights/network-performance-monitor-service-connectivity.md) ve [Azure ExpressRoute performansını izleme](../../azure-monitor/insights/network-performance-monitor-expressroute.md). 

Ağ Performansı İzleyicisi trafiği blackholing, yönlendirme hataları ve sorunları geleneksel ağ izleme yöntemleri algılayabilir gibi ağ sorunları algılar. Bir ağ bağlantısı için bir eşik ihlal edildiğinde, çözüm uyarılar oluşturur ve size bildirir. Ayrıca ağ performans sorunlarının zamanında tespit edilmesini sağlar ve sorunun kaynağını belirli bir ağ kesimine veya cihazına yerelleştirir. 

Tarafından desteklenen çeşitli özellikler hakkında daha fazla bilgi [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) çevrimiçi kullanılabilir.

## <a name="set-up-and-configure-agents"></a>Ayarlama ve aracıları yapılandırma

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM ile izleme için kullanılacak düğümleri için platform gereksinimleri nelerdir?
Aşağıda listelenen NPM'nin platform gereksinimlerini çeşitli özellikleri şunlardır:

- NPM Performans İzleyicisi ve hizmet bağlantı İzleyicisi özellikleri Windows server hem Windows Masaüstü/istemci işletim sistemlerini destekler. Desteklenen Windows server işletim sistemi sürümleri olan 2008 SP1 veya üzeri. Windows Masaüstü/istemci sürümleri desteklenir, Windows 10, Windows 8.1, Windows 8 ve Windows 7 ' dir. 
- NPM ExpressRoute İzleyicisi özelliğini destekleyen yalnızca Windows server (2008 SP1 veya üstü) işletim sistemi.

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>Linux makineleri düğümler NPM izleme olarak kullanabilir miyim?
Linux tabanlı düğümleri kullanarak ağları izleme yeteneği şu anda Önizleme aşamasındadır. Daha fazla bilgi edinmek için hesap yöneticinize ulaşın. Linux aracıları yalnızca NPM'nin Performans İzleyicisi özelliği için izleme olanağı sağlar ve için hizmet bağlantı İzleyicisi ve ExpressRoute İzleyicisi özellikleri kullanılabilir değil

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM ile izleme için kullanılacak düğümlerinin boyutu gereksinimleri nelerdir?
VM ağları izlemek için düğümde NPM çözümü çalıştırmak için en az 500 MB bellek ve bir çekirdek düğümleri olması gerekir. NPM çalıştırmak için ayrı düğüm kullanmanız gerekmez. Çözüm üzerinde çalışan iş yüklerine olan düğümleri çalıştırabilirsiniz. Çözüm, %5 CPU birden kullanılıyorsa izleme işlemini durdurmak için özellik sahiptir.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>NPM kullanmak için Bilgisayarım düğümleri doğrudan aracısı veya System Center Operations Manager üzerinden bağlanabilirim?
Performans İzleyicisi'ni hem de hizmet bağlantı İzleyicisi Özellikleri düğümleri desteklemek [bağlı doğrudan aracılar](../../azure-monitor/platform/agent-windows.md) ve [Operations Manager ile bağlı](../../azure-monitor/platform/om-agents.md).

ExpressRoute İzleyicisi özelliği için doğrudan aracılar yalnızca Azure düğümleri bağlanmalıdır. Operations Manager ile bağlı olan azure düğümleri desteklenmiyor. Şirket içi düğümleri için bir ExpressRoute bağlantı hattı izlemek için Operations Manager üzerinden doğrudan aracılar olarak bağlı düğümleri desteklenir.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>İzleme için TCP ve ICMP arasından hangi protokolün seçilmelidir?
Windows server tabanlı düğümleri kullanarak ağınızı izleme yapıyorsanız, çünkü daha büyük doğruluk sağlar İzleyici protokolü olarak TCP kullanmanızı öneririz. 

ICMP tabanlı işletim sistemi Windows Masaüstü/istemci düğümleri için önerilir. Bu platform does'nt NPM ağ topolojisini bulmak için kullandığı ham yuva gönderilecek TCP verileri sağlar.

Her Protokolü göreli avantajları hakkında daha fazla ayrıntıya ulaşabilirsiniz [burada](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md#choose-the-protocol).

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>Bir düğüm TCP protokolünü kullanılarak izleme destekleyecek şekilde nasıl yapılandırabilirim?
Düğüm TCP protokolünü kullanılarak izleme desteklemek: 
* Düğüm platformu, Windows Server olduğundan emin olun (2008 SP1 veya üzeri).
* Çalıştırma [EnableRules.ps1](https://aka.ms/npmpowershellscript) düğümde Powershell betiği. Bkz: [yönergeleri](../../azure-monitor/insights/network-performance-monitor.md#configure-log-analytics-agents-for-monitoring) daha fazla ayrıntı için.


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>İzleme için NPM tarafından kullanılan TCP bağlantı noktasını nasıl değiştirebilirim?
NPM tarafından çalıştırarak izlemek için kullanılan TCP bağlantı noktasını değiştirebilirsiniz [EnableRules.ps1](https://aka.ms/npmpowershellscript) betiği. Bir parametre olarak kullanmak istediğiniz bağlantı noktası numarası girmeniz. Örneğin, TCP bağlantı noktası 8060 etkinleştirmek için çalıştırın `EnableRules.ps1 8060`. İzleme için kullanılan tüm düğümlerde aynı TCP bağlantı noktasını kullandığınızdan emin olun.

Betik yalnızca Windows Güvenlik Duvarı yerel olarak yapılandırır. Ağ güvenlik duvarı veya ağ güvenlik grubu (NSG) kuralları varsa, NPM tarafından kullanılan TCP bağlantı noktasının giden trafiği izin verdiğinden emin olun.

### <a name="how-many-agents-should-i-use"></a>Aracı sayısı kullanmalıyım?
İzlemek istediğiniz her alt ağ için en az bir aracı kullanmanız gerekir.

### <a name="what-is-the-maximum-number-of-agents-i-can-use-or-i-see-error--youve-reached-your-configuration-limit"></a>Ne kullanabilirim aracıların sayısı ve hatası görüyorum "... Yapılandırma sınırınıza ulaştınız"?
NPM çalışma alanı başına 5000 IP'ler için IP'ler sayısını sınırlar. Hem IPv4 hem de IPv6 adreslerini bir düğüm varsa bu bu düğüm için 2 IP'ler sayılır. Bu nedenle, 5000 IP'lerin bu sınırı aracı sayısı üst sınırı karar vermeniz. Etkin aracılar, NPM düğümleri sekmesinden silebilirsiniz >> yapılandırın. NPM ayrıca tüm barındırma aracı ve her VM için hiç olmadığı kadar atanmış IP'ler, 5000 IP'ler sayısı üst sınırı için katkıda bulunan ayrı IP olarak sayılır geçmişini tutar. İçin ücretsiz çalışma alanınız için oluşturan Ip'lerini düğümleri sayfanın kullanımda olmayan IP'ler silmek için kullanabilirsiniz.

## <a name="monitoring"></a>İzleme

### <a name="how-are-loss-and-latency-calculated"></a>Kayıp ve gecikme süresi nasıl hesaplanır
(ICMP izlenmesi için protokol olarak seçilirse) kaynak aracıları ya da TCP SYN (TCP izleme için protokol olarak belirtildiyse) istekleri veya ICMP YANKI isteklerini hedef IP kaynak-hedef IP arasındaki tüm yolları emin olmak için düzenli aralıklarla gönderir birlikte ele alınmıştır. Alınan paketlerin ve paket gidiş dönüş süresi yüzdesi, kayıp ve gecikme süresi her yolunun hesaplamak için ölçülür. Bu veriler üzerinden belirli bir yoklama aralığı için IP birleşimi için toplanan değerler kayıp ve gecikme süresi almak için tüm yolları ve yoklama aralığı üzerinden toplanır.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>Hangi Frequency değeri ile kaynak aracı hedef izleme paketleri gönderir?
Performans İzleyicisi ve ExpressRoute İzleyicisi özellikleri için kaynak paketleri 5 saniyede gönderir ve ağ ölçümlerini kaydeder. Bu veri kaybı ve gecikme süresi, ortalama ve en üst seviyeye değerleri hesaplamak için 3 dakikalık yoklama aralığı içinde toplanır. Hizmet Bağlantı İzleyicisi özelliği için paketler için ağ ölçümünü gönderme sıklığı, test yapılandırılırken belirli test için kullanıcı tarafından girilen sıklığı tarafından belirlenir.

### <a name="how-many-packets-are-sent-for-monitoring"></a>İzleme için kaç paketler gönderilir?
Bir yoklama hedef kaynak aracı tarafından gönderilen paketlerin sayısını Uyarlamalı ve farklı ağ topolojileri için farklı olabilen algoritmamız tarafından belirlenir. Daha fazla kaynak-hedef IP bileşimi arasında ağ yolları daha fazla sayıda gönderilen paketlerinin sayısıdır. Sistem kaynak-hedef IP bileşimi arasında tüm yollar değinilmektedir sağlar.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>NPM, kaynak ve hedef arasındaki ağ topolojisi nasıl bulunsun mu?
NPM Traceroute alan özel bir algoritma tüm yolları ve kaynak ve hedef arasında atlama bulmak için kullanır.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>NPM, Yönlendirme ve geçiş düzeyi bilgileri sağlar mı 
NPM aracı kaynak ve hedef arasındaki olası tüm yolları algılayabilir rağmen belirli iş yükleri tarafından gönderilen paketler tarafından içine rota alındığı görünürlük sağlamaz. Çözüm, beklenenden daha fazla gecikme ekleme temel ağ atlamaları ve yolları belirlemenize yardımcı olabilir.

### <a name="why-are-some-of-the-paths-unhealthy"></a>Neden bazı yollar sağlıksız misiniz?
Kaynak ve hedef Ip'lerini arasında farklı ağ yollarını bulunabilir ve her yolu kayıp ve gecikme süresi, farklı bir değer olabilir. NPM bu yollar sağlıksız (kırmızı renkle başlar) işaretler kaybı ve/veya gecikme süresi değerleri olan izleme yapılandırma ilgili eşik değerinden büyük.

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>Ağ topolojisi haritası ' ne kırmızı renkli bir atlama geldiğiniz?
Bir atlama kırmızı ise, en az bir sağlıksız yol bir parçası olduğunu belirtir. NPM yalnızca yollar sağlıksız olarak işaretler, her bir yol sistem durumunu ayırmak değil. Sorunlu atlama tanımlamak için atlama atlama gecikme süresi görüntüleyebilir ve en fazla beklenen gecikme ekleme olanları ayırabilirsiniz.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>Performans İzleyicisi'nde hata yerelleştirme nasıl çalışır?
NPM, her ağ yolu, ağ kesimindeki hata olasılıklar atamak için olasılığa dayanan bir mekanizma kullanır ve bir parçası olduklarından sağlıksız yollar sayısına göre bağlı ağ atlamaları. Ağ kesimlerini ve atlama sağlıksız yollar daha fazla sayıda bir parçası haline geldiğinden, bunlarla ilişkili hata olasılığı artar. Bu hata olasılıklar hesaplamak için veri noktalarını arttıkça birbirine bağlı NPM aracısıyla birçok düğüm varsa, bu algoritmayı en iyi şekilde çalışır.

### <a name="how-can-i-create-alerts-in-npm"></a>' Nde NPM uyarıları nasıl oluşturabilirim?
Başvurmak [uyarıları belgeler bölümünde](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor#alerts) adım adım yönergeler için.

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>NPM yönlendiriciler ve sunucuları tek tek cihazlar olarak izleyebilir miyim?
NPM, yalnızca temel alınan arasındaki ağ atlamalarını (anahtarlar, yönlendiriciler, sunucular, vb.) kaynak ve hedef Ip'lerini IP ve ana bilgisayar adını tanımlar. Ayrıca, bu tanımlanan atlama arasında bir gecikme tanımlar. Bu temel alınan atlama ayrı ayrı izlemez.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>Azure ve AWS arasındaki ağ bağlantısını izlemek için NPM kullanılabilir mi?
Evet. Makalesine bakın [İzleyici Azure, AWS ve NPM kullanarak şirket içi ağları](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/) Ayrıntılar için.

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>ExpressRoute bant genişliği kullanımı, gelen veya giden mi?
Bant genişliği kullanımını gelen ve giden bant genişliği toplamıdır. Bit/sn cinsinden ifade edilir.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>Biz, ExpressRoute için gelen ve giden bant genişliği bilgi alabilir miyim?
Gelen ve giden hem birincil hem de ikincil bant genişliği değerlerini yakalanabilir.

Eşleme düzeyi bilgileri için belirtilen sorgu günlük araması'nda aşağıdaki

    NetworkMonitoring 
    | where SubType == "ExpressRoutePeeringUtilization"
    | project CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
  
Bağlantı hattı düzeyi bilgileri için kullanması aşağıda belirtilen sorgu 

    NetworkMonitoring 
    | where SubType == "ExpressRouteCircuitUtilization"
    | project CircuitName,PrimaryBytesInPerSecond, PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>NPM Performans İzleyicisi'ni hangi bölgeler desteklenir?
NPM birinde barındırılan bir çalışma alanından dünyanın herhangi bir bölümünde ağlar arasında bağlantı izleyebilirsiniz [desteklenen bölgeler](../../azure-monitor/insights/network-performance-monitor.md#supported-regions)

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>NPM Hizmeti Bağlantı İzleyicisi için hangi bölgeler desteklenir?
NPM izlemek için kullanabileceğiniz Bağlantı Hizmetleri dünyanın herhangi bir bölümünde birinde barındırılan bir çalışma alanından [desteklenen bölgeler](../../azure-monitor/insights/network-performance-monitor.md#supported-regions)

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>NPM ExpressRoute İzleyicisi için hangi bölgeler desteklenir?
NPM, dilediğiniz Azure bölgesinde bulunan, ExpressRoute devreleri izleyebilirsiniz. Birinde barındırılan bir Log Analytics çalışma alanı gerektirecektir npm için yerleşik, [desteklenen bölgeler](/azure/expressroute/how-to-npm)

## <a name="troubleshoot"></a>Sorun gider

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>Neden bazı ağ topolojisini görüntüleme tanımlanmamış olarak işaretlenmiş atlama misiniz?
NPM traceroute değiştirilmiş bir sürümünü hedef kaynak aracısından topolojiye bulmak için kullanır. Tanımlanmayan bir atlama ağ atlama kaynak aracının traceroute isteğine yanıt vermedi temsil eder. Üç ardışık ağ atlamaları aracının traceroute için yanıt vermezse, çözüm, yanıt vermeyen atlama tanımlanmamış olarak işaretler ve daha fazla atlama bulunacak denemez.

Bir atlama, bir veya daha fazlasında bir traceroute yanıtlayamayabilir senaryolar aşağıda:

* Yönlendiriciler, kimliklerini açığa çıkarmadığınızdan yapılandırıldı.
* Ağ aygıtlarının ICMP_TTL_EXCEEDED trafiğine izin vermiyor.
* Bir güvenlik duvarı, ağ aygıtı ICMP_TTL_EXCEEDED yanıttan engelliyor.

### <a name="why-does-my-link-show-unhealthy-but-the-topology-does-not"></a>Neden sağlıksız my bağlantı show ancak topoloji yok 
NPM, uçtan uca kaybı, gecikme süresi ve topoloji farklı aralıklarla izler. Kayıp ve gecikme süresi 5 saniyede bir kere hesaplanır ve (Performans İzleyicisi ve Express Route İzleyici) üç dakikada bir toplanan topolojisi her 10 dakikada traceroute kullanarak hesaplanırken. Örneğin, 3:44 ve 4:04 arasında topolojisi üç kez güncelleştirilebilir (3:44, 3:54, 4:04), ancak kayıp ve gecikme süresi yaklaşık yedi kat güncelleştirilir (3:44, 3:47 3:50 3: 53'ü 3:56, 3:59, 4:02). 3:54 oluşturulan topoloji kayıp ve gecikme süresi 3:56 hesaplanan işlenen 3:59 ve 4:02. 3:59 ER devreniz kötü bir uyarı alırsınız varsayalım. NPM için oturum açar ve topoloji saati 3:59 ayarlamaya çalışın. NPM 3:54 oluşturulan topoloji işlenir. Son bilinen ağınızın topolojisi anlamak için alanları TimeProcessed Karşılaştır (hangi kayıp ve gecikme süresi hesaplanmıştır adresindeki süresi) ve TracerouteCompletedTime (hangi topolojiyi hesaplandığında, saat). 

### <a name="what-is-the-difference-between-the-fields-e2emedianlatency-and-avghoplatencylist-in-the-networkmonitoring-table"></a>E2EMedianLatency ve AvgHopLatencyList NetworkMonitoring tabloda alanlar arasındaki fark nedir
E2EMedianLatency AvgHopLatencyList bilgileriyse tcp ping testlerin sonuçlarını toplayarak üzerinde traceroute göre her 10 dakikada güncelleştirdikten sonra üç dakikada bir güncelleştirilen gecikme olur. Başlangıçtan E2EMedianLatency hesaplandığı tam zamanı anlamak için alanın TimeProcessed kullanın. Hangi traceroute tamamlandı ve güncelleştirilmiş AvgHopLatencyList tam zamanında anlamak için alanın TracerouteCompletedTime kullanın.

### <a name="why-does-hop-by-hop-latency-numbers-differ-from-hoplatencyvalues"></a>Atlama atlama gecikme rakamları HopLatencyValues neden farkı nedir 
HopLatencyValues uç noktasına bir kaynaktır.
Örneğin: Hops - A,B,C. AvgHopLatency - 10,15,20. Bu kaynak için bir gecikme süresi anlamına gelir = 10, kaynak B gecikme = 15 ve kaynak C gecikme 20. Kullanıcı Arabirimi, A-B atlama gecikme süresi 5 topolojide olarak hesaplar

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>Çözüm % 100 kaybı gösterir, ancak kaynak ve hedef arasında bağlantısı
Konak güvenlik duvarı veya Ara Güvenlik Duvarı (ağ güvenlik duvarı ya da Azure NSG) tarafından NPM izleme için kullanılan bağlantı noktası üzerinden aracı kaynak ve hedef arasındaki iletişimi engelliyor bu durum meydana gelebilir (bağlantı noktası 8084'tür, sürece varsayılan olarak açıktır Müşteri bu dönüştürülmüştür).

* Konak güvenlik duvarı gerekli bağlantı noktasında iletişime engel olmadığından emin doğrulamak için aşağıdaki görünümü kaynak ve hedef düğümleri sistem durumunu görüntüleyin: Ağ Performansı İzleyicisi -> yapılandırma -> düğümleri. 
  Sağlıksız olmaları durumunda yönergeleri görüntüleyin ve düzeltici eylemlerde. Düğümler sağlıklı bulunuyorsa adıma b taşıyın. öğrenin.
* Üçüncü taraf PsPing yardımcı programını kullanarak bir ara ağ güvenlik duvarı ya da Azure NSG gerekli bağlantı noktasında iletişime engel olmadığından doğrulamak için kullanmak aşağıdaki yönergeleri:
  * psping programıdır indirilebilir [burada](https://technet.microsoft.com/sysinternals/psping.aspx) 
  * Komut kaynak düğümden çalıştırın.
    * psping -n 15 \<hedef düğüm IPADDRESS\>: BağlantıNoktasıNumarası NPM varsayılan 8084 bağlantı noktasını kullanır. Durumda, bu EnableRules.ps1 komut dosyası kullanarak açıkça değiştirildi, kullanmakta olduğunuz özel bir bağlantı noktası numarasını girin). Şirket içi Azure makineden ping budur.
* Ping başarılı olup olmadığını denetleyin. Aksi durumda, bu bağlantı noktası üzerinde trafiğe bir ara ağ güvenlik duvarı ya da Azure NSG engellediğini gösterir.
* Şimdi, kaynak düğümün IP'sini hedef düğümünden komutu çalıştırın.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>Bir düğümden B ancak A. B düğüme kaybı Neden?
Kayıp ve gecikme süresi için farklı değerler, A-B arasında ağ yolları a B arasında ağ yolları farklı olarak gösterilebilir.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>Neden tüm ExpressRoute devreleri ve bulunan eşleme bağlantılarını misiniz?
NPM artık ExpressRoute bağlantı hatları ve kullanıcı erişimi olan tüm abonelikler eşleme bağlantılarını bulur. Express Route kaynaklarınızın nerede bağlı olan ve bulunan her kaynak için izlemeyi etkinleştirin tüm abonelikleri seçin. NPM arar ve bir özel eşdüzey hizmet sağlama, keşfederken bağlantı nesneleri bu nedenle Lütfen onay bir VNET, eşleme ile ilişkili ise.

### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>ER izleme özelliği, bir tanılama iletisi "Trafik bir bağlantı hattı geçmiyor" vardır. Bu ne demektir?

Burada şirket içi arasında sağlıklı bir bağlantı yoktur ve Azure düğümleri ancak trafik düşülmemesini NPM tarafından izlenecek şekilde yapılandırılmış ExpressRoute bağlantı hattı üzerinden bir senaryo olabilir. 

Bu durum oluşabilir:

* ER devresini çalışmıyor.
* Rota filtreleri, hedeflenen ExpressRoute bağlantı hattı üzerinden bunlar diğer yollar (örneğin, bir VPN bağlantısı veya başka bir ExpressRoute bağlantı hattı) öncelik vermek şekilde yapılandırılır. 
* Şirket içi ve Azure düğümleri seçilen İzleme yapılandırmasını ExpressRoute bağlantı hattını izlemek için hedeflenen ExpressRoute bağlantı hattı üzerinden birbiriyle bağlantı yoktur. İzlemek istediğiniz ExpressRoute bağlantı hattı üzerinden birbirleriyle bağlantıya sahip doğru düğümler seçtiğiniz emin olun.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>ExpressRoute bağlantı hattımı izleme yapılandırılırken Azure düğümleri algılanmaz.
Operations Manager üzerinden Azure düğümleri bağlıysanız, bu durum oluşabilir. ExpressRoute İzleyicisi özelliği yalnızca doğrudan aracılar bağlı olan Azure düğümleri destekler.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>ExpressRoute devreleri OMS portalında tarafından bulunamıyor
NPM hem OMS portalında yanı sıra Azure portalı kullanılabilmesine rağmen Azure portalı üzerinden yalnızca ExpressRoute izleme özelliği bağlantı hattı bulmaya çalışır. Azure portalı üzerinden bağlantı hatlarının bulunan sonra ardından yeteneği iki Portal birini kullanabilirsiniz. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>Hizmet Bağlantı İzleyicisi özelliği kullanılamaz olarak hizmet yanıt süresi, Ağ kaybı ve gecikme süresi gösterilir
Bir veya daha fazla true ise bu durum oluşabilir:

* Hizmet çalışmıyor.
* Hizmet için ağ bağlantısını denetlemek için kullanılan düğümü çalışmıyor.
* Testi Yapılandırması'nda girilen hedef doğru değil.
* Düğüm, herhangi bir ağ bağlantısı yok.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>Hizmet Bağlantı İzleyicisi özelliği bir geçerli hizmet yanıt süresi gösterilir ancak gecikme süresi yanı sıra ağ kaybı kullanılamaz olarak gösterilir
 Bir veya daha fazla true ise bu durum oluşabilir:

* Bir Windows istemci makine hizmete ağ bağlantısını denetlemek için kullanılan düğümün ise hedef hizmet ICMP istekleri engellediğinde veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteği engelliyor.
* Testi Yapılandırması'nda gerçekleştirme ağ ölçülerini onay kutusu boştur.

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>Hizmet Bağlantı İzleyicisi özelliği NA hizmet yanıt süresi, ancak gecikme süresi yanı sıra ağ kaybı geçerlidir
Bu durum hedef hizmeti bir web uygulaması değil, ancak test yapılandırılmış bir Web testi olarak. Test yapılandırmasını düzenleyin ve ağ yerine Web test türü seçin.

## <a name="miscellaneous"></a>Çeşitli

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>İzleme için kullanılan düğümü üzerindeki performans etkisini mı?
NPM işlemi birden fazla %5 konak CPU kaynaklarını yararlanıyorsa durdurmak için yapılandırılır. Bu, düğümler, her zamanki iş yükleri için performansı etkilemeden kullanmaya devam edebilirsiniz, sağlamaktır.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>NPM izleme için güvenlik duvarı kurallarını Düzenle mu?
NPM EnableRules.ps1 Powershell betiğini aracılar birbiriyle belirtilen bağlantı noktasında TCP bağlantıları oluşturmak izin verecek şekilde çalıştığı düğümleri yalnızca yerel bir Windows Güvenlik duvarı kuralı oluşturur. Çözüm herhangi bir ağ güvenlik duvarı veya ağ güvenlik grubu (NSG) kuralları değiştirmez.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>Sistem durumunu izlemek için kullanılan düğümlerde nasıl kontrol edebilirim?
Aşağıdaki görünümden izlemek için kullanılan düğümlerde sistem durumunu görüntüleyebilirsiniz: Ağ Performansı İzleyicisi -> yapılandırma -> düğümleri. Bir düğüm sağlıksız durumda, hata ayrıntılarını görüntülemek ve önerilen eylemi gerçekleştirin.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>NPM mikrosaniye cinsinden gecikme rakamları bildirebilirsiniz?
NPM, kullanıcı arabiriminde ve milisaniye cinsinden gecikme rakamları yuvarlar. Aynı veriler, daha yüksek bir ayrıntı düzeyinde (bazen en fazla dört ondalık basamak) depolanır.

## <a name="next-steps"></a>Sonraki adımlar

- Ağ Performansı İzleyicisi hakkında daha fazla başvurarak bilgi [azure'da Ağ Performansı İzleyicisi çözüm](../../azure-monitor/insights/network-performance-monitor.md).
