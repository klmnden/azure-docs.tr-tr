---
title: "Ağ Azure Performans İzleyicisi çözümde | Microsoft Docs"
description: "Ağ Performans İzleyicisi'nde Azure ağları eklentinizi gerçek zaman için yakın performansını izlemenizi sağlar algılamak ve ağ performans sorunları bulun."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 399fe552d5c7d9a96cdabc2a1dfafe99635d4a61
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Performans İzleyicisi çözümde Azure ağ

![Ağ Performans İzleyici simgesi](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Ağ Performans İzleyicisi'ni (NPM) olan bir bulut tabanlı karma Ağ İzleyicisi ağ bağlantısı hizmeti/uygulama uç noktaları, ağ altyapınızın çeşitli noktaları arasındaki ağ performansını izlemenize yardımcı olur çözüm izleme ve Azure ExpressRoute performansını izleyin.  

NPM trafiği blackholing, yönlendirme hataları ve geleneksel ağ izleme yöntemleri algılayabilir olmayan sorunlar gibi ağ sorunları algılar. Çözüm uyarılar oluşturur, bir eşiği için bir ağ bağlantı ihlal, ağ performans sorunlarını zamanında algılanması sağlar ve belirli ağ kesimine veya cihaza sorunun kaynağını yerelletirilmesi bildirir. 

NPM üç geniş yetenekleri sunar: 

[Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md): Bulut dağıtımları ve şirket içi konumlarına, birden çok veri merkezi ve şube arasında ağ bağlantısı izleme, kritik çok katmanlı uygulamalar/mikro-services görev. Performans izleme ile kullanıcılarınız şikayetçi önce ağ sorunları algılayabilir.  

[Hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-service-endpoint.md): bağlantı izleyebilirsiniz, kullanıcılarınızın önem verdiğiniz yolunda hangi altyapıya belirlemek ve burada Ağ Hizmetleri performans sorunları ortaya çıkan. Kullanıcılarınızın önce kesintileri hakkında bilgi edinmek ve sorunları, ağ yolu boyunca tam konumunu görebilirsiniz. 

Bu özellik http gerçekleştirmenize yardımcı olur, HTTPS, TCP ve ICMP de yakın gerçek zamanlı veya geçmişte kullanılabilirlik ve hizmetinizin yanıt süresi ve paket kaybı ve gecikme süresi ağında katkı izlemek için testleri göre. Ağ topolojisi haritası ile ağ yol boyunca her atlama gecikme verileriyle hizmetine düğümden oluşan sorun noktalar belirleyerek ağ yavaşlamalara ayırabilirsiniz. Yerleşik testleriyle Office365 ve Dynamics CRM herhangi bir ön yapılandırma olmadan ağ bağlantısını izleyin. Bu özellik ile ağ bağlantısı izleyebilirsiniz Web siteleri, SaaS, PaaS uygulamaları, veritabanları gibi tüm TCP özellikli uç vb.  

[ExpressRoute İzleyici](log-analytics-network-performance-monitor-expressroute.md): Azure ExpressRoute uçtan uca bağlantı ve şube ofislerinde ve Azure arasında performans izleme.  
 

## <a name="set-up-and-configure"></a>Ayarlama ve yapılandırma

### <a name="install-and-configure-agents"></a>Yükleme ve aracıları yapılandırma 

Temel işlemleri sırasında aracıları yüklemek için kullanmak [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agents.md) ve [Operations Manager'a günlük analizi](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Aracıları yükleneceği yeri 

**Performans İzleyicisi:** yüklemek istediğiniz diğer alt ağlara ağ bağlantısı izlemek her alt ağ OMS Aracısı en az bir düğümde bağlı.  

Bir ağ bağlantısı izlemek için o bağlantı üzerindeki her iki uç noktaları aracıları yüklemeniz gerekir.  Ağınızın topolojisi hakkında emin değilseniz, aracıları sunucuları ağ performansını izlemek istediğiniz kritik iş yükleri ile yükleyin. Örneğin, bir Web sunucusu ve SQL çalıştıran bir sunucu arasında ağ bağlantısı izlemek istiyorsanız, her iki sunucuda bir aracı yükleyin. Aracıları konakları kendilerini ana bilgisayarlar arasında ağ bağlantısı (Bağlantılar) izleyin. 

**Hizmet uç noktası izleme:** yüklemek istediğiniz hizmet uç noktası için ağ bağlantısı izlemek her bir düğümde OMS Aracısı. Ağ bağlantısı için Office365 office sitenizden O1 izlemek istiyorsanız, örneğin, O2 ve O3, ardından OMS Aracısı en az bir düğüm üzerindeki her O1, O2 ve O3 yükleyin. 

**ExpressRoute izleme:** ExpressRoute özel eşleme bağlı, şirket içi alt ağ, Azure sanal ağınızın içinde en az bir OMS aracısı ve en az bir aracı yükleyin.  

### <a name="configure-oms-agents-for-monitoring"></a>İzleme için OMS aracıları yapılandırma  

NPM, kaynak ve hedef aracılar arasındaki ağ performansını izlemek için yapay işlemler kullanır. TCP için ExpressRoute İzleyicisi kullanılırken çözüm Performans İzleyicisi'ni ve hizmet uç noktası izleme yeteneği, izleme için protokol olarak TCP ve ICMP arasında bir seçim sunar. Güvenlik Duvarı İzleme için seçtiğiniz Protokolü izleme için kullanılan OMS Aracısı arasındaki iletişim verdiğinden emin olun.  

**TCP protokolü:** izleme için protokol olarak TCP seçtiyseniz, güvenlik duvarı bağlantı noktası için Performans İzleyicisi'ni ve ExpressRoute izleme olanakları, aracıları birbirine bağlanabilir sağlamak için kullanılan aracılar açın. Bunu yapmak için yönetici ayrıcalıklarına sahip bir power shell penceresinde hiçbir parametre olmadan EnableRules.ps1 PowerShell komut dosyasını çalıştırın.  

Komut dosyası çözümü tarafından gerekli kayıt defteri anahtarları ve aracılar birbiriyle TCP bağlantıları oluşturmak izin vermek için Windows Güvenlik duvarı kuralları oluşturur. Komut dosyası tarafından oluşturulan kayıt defteri anahtarlarını da oturum hata ayıklama günlükleri ve günlükleri dosyasının yolunu belirtin. Ayrıca, iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarları el ile değiştirmemelisiniz şekilde bu anahtarları için değerleri otomatik olarak komut dosyası tarafından ayarlanır. Varsayılan portunu 8084 ' dir. Komut parametresi BağlantıNoktasıNumarası sağlayarak, özel bir bağlantı noktası kullanabilirsiniz. Ancak, aynı bağlantı noktasını tüm bilgisayarlarda betiğin çalıştırıldığı kullanılması gerekir. 

>[!NOTE]
> Komut dosyası, yalnızca windows Güvenlik Duvarı'nı yerel olarak yapılandırır. Ağ güvenlik duvarı varsa, NPM tarafından kullanılan TCP bağlantı noktası için giden trafiğe izin verdiğinden emin olmalısınız 

>[!NOTE]
> Hizmet uç noktası İzleyicisi EnableRules.ps1 PowerShell betiğini çalıştırmak gerekmez 

 

**ICMP Protokolü** - İzleme Protokolü etkinleştirmek gibi güvenilir bir şekilde ICMP kullanılarak aşağıdaki güvenlik duvarı kuralları ICMP seçtiyseniz: 

 
```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```
 

### <a name="configure-the-solution"></a>Çözümünüzü yapılandırma 

1. Ağ Performansı İzleyicisi çözüm alanınızdan ekleyin [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). 
2. Günlük analizi çalışma alanını açın ve tıklayın **genel bakış** döşeme.  
3. Başlıklı kutucuğa tıklayın **Ağ Performansı İzleyicisi** iletiyle *çözüm ek yapılandırma gerektirir*.

    ![NPM döşeme](media/log-analytics-network-performance-monitor/npm-config.png)

3. Üzerinde **Kurulum** sayfaya, OMS aracıları yükleme ve izleme için aracıları yapılandırma seçeneğine bakın **ortak ayarları** görünümü. Önceden yüklenmiş ve yapılandırılmış OMS Aracısı, yukarıda açıklandığı gibi tıklayın **Kurulum** kullanmayla ilgilenen yetenek yapılandırma görünümü.  

    **Performans İzleyicisi görünümünü** -hangi protokolü varsayılan yapay işlemler için kullanılması gereken seçin Performans İzleyicisi kuralı ve kaydederken ve devam et'ı tıklatın. Bu protokol seçimini yalnızca sistem tarafından oluşturulan varsayılan kuralı için tutar ve Performans İzleyicisi kural açıkça oluşturma her zaman Protokolü seçmeniz gerekir. Her zaman varsayılan kural ayarları (Bu gün-0 yapılandırmanızı tamamladıktan sonra görünür) Performans İzleyicisi sekmesinde taşıyın ve protokol daha sonra değiştirin. RPerfomance izleme yeteneği ilgilenen olmayan olasılığına Performans İzleyicisi sekmesindeki varsayılan kural ayarlarından varsayılan kuralı devre dışı bırakabilirsiniz. 

    ![NPM yapılandırma](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
    **Hizmet uç noktası izleme görünümü** -aracılarınızı Office365 ve Dynamcis365 ağ bağlantısını izlemek için yerleşik önceden yapılandırılmış testleri yeteneği sağlar. Bunları yanındaki onay kutusunu işaretleyerek izlemede ilgilendiğiniz Office365 ve Dynamcis365 hizmetleri seçin. Aracıları Ekle düğmesine tıklayarak, izlemek istediğiniz aracıları seçin. Bu özelliği kullanın veya daha sonra ayarlamak istediğiniz istemiyorsanız, bu atlayıp doğrudan tıklayın seçebileceğiniz **kaydetmek** ve **devam** herhangi bir şey seçme olmadan.  

    ![NPM yapılandırma](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

    **ExpressRoute İzleyicisi görünümü** -tıklayın **Şimdi Bul** Azure Abonelikteki sanal ağlar için bağlı özel eşlemeler, bu günlük analizi çalışma alanı ile bağlantılı tüm ExpressRoute Bul düğmesi.  


    >[!NOTE] 
    > Çözüm şu anda yalnızca ExpressRoute özel eşlemeler bulur. 

    >[!NOTE] 
    > Yalnızca bu özel eşlemeler, bu günlük analizi çalışma alanı ile bağlantılı abonelikle ilişkili sanal ağlar için bağlandığı bulunur. Bu çalışma alanına bağlı abonelik dışında ExpressRoute sanal ağlara bağlı ise, bu Aboneliklerde günlük analizi çalışma alanı oluşturmak ve bu eşlemeler izlemek için NPM kullanmak gerekir. 

    ![NPM yapılandırma](media/log-analytics-network-performance-monitor/npm-express-route.png)

    Bulma tamamlandıktan sonra bulunan özel eşlemeleri bir tablodaki listelenir.  

    ![NPM yapılandırma](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
    Bu eşlemeler için izleme olan başlangıçta devre dışı durumda. Her eşlemesi üzerinde izleme İlgilendiğiniz ve bunlar için sağ taraftaki (RHS) Ayrıntıları görünümünden izlemeyi yapılandırma'ı tıklatın.  Yapılandırmayı kaydetmek için düğmesine tıklayın. Bkz: [yapılandırma ExpressRoute izleme]() daha fazla bilgi için.  

    Kurulum tamamlandıktan sonra verilerin doldurmak bir saat 30 dakika sürer. Çözüm ağınızdan veri toplama sırasında gördüğünüz *çözüm ek yapılandırma gerektirir* NPM genel bakış kutucuğu üzerinde. Veriler toplanır ve dizine sonra genel bakış değişiklikleri döşeme ve ağınızın sistem durumu özetini size bildirir. Ardından düğümler üzerinde OMS Aracısı, ortamınızdan bulunan alt ağlar yanı sıra yüklü izleme Düzenle seçebilirsiniz 

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Alt ağları ve düğümler ilişkin izleme ayarlarını Düzenle 

En az bir aracısının yüklü olduğu tüm alt ağlar yapılandırma sayfasını ağlarla sekmesinde listelenir. 


Etkinleştirme veya belirli alt ağlar izlemeyi devre dışı bırakma 

1. Seçin veya kutunun işaretini **alt ağ kimliği** ve emin olun **kullanım izleme için** seçili veya temizlenmiş, uygun şekilde şeklindedir. Seçin veya birden çok alt ağı temizleyin. Diğer aracıları ping işlemi durdurmak için aracıları güncelleştirildikçe devre dışı bırakıldığında, alt ağlar izlenmeyen. 
2. Belirli bir alt ağ alt ağ listeden seçerek ve gerekli düğümleri izlenmeyen ve izlenen düğümleri içeren liste arasında taşıma izlemek istediğiniz düğümleri seçin. Ekleyebileceğiniz bir **özel açıklama** alt ağ. 
3. tıklatın **kaydetmek** yapılandırmayı kaydetmek için. 

#### <a name="choose-nodes-to-monitor"></a>İzleme düğümü seçin

Bir aracısı yüklü olan tüm düğümleri listelenen **düğümleri** sekmesi. 

1. Seçin veya izlemek ya da izlemeyi durdurmak istediğiniz düğümleri temizleyin. 
2. Tıklatın **kullanım izleme için**, veya, uygun şekilde temizleyin. 
3. **Kaydet**’e tıklayın. 


İlgilendiğiniz capability(s) yapılandırın: 
- Yapılandırma [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Yapılandırma [hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Yapılandırma [ExpressRoute İzleyicisi](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Veri toplama ayrıntıları
ICMP kaybı ve gecikme bilgileri toplamak için protokol olarak seçildiğinde TCP seçildiğinde TCP Eşitlemeye SYNACK ACK el sıkışma paketleri ve ICMP YANKI ICMP YANKI YANITI ağ Performans İzleyicisi'ni kullanır. İzleme yolu topoloji bilgilerini almak için de kullanılır.

Aşağıdaki tabloda, veri toplama yöntemleri ve Ağ Performansı İzleyicisi için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Gönderilen TCP el sıkışmaları/ICMP YANKI iletileri 5 saniyede veri 3 dakikada bir |
 

 
Çözüm yapay işlemler ağ durumunu değerlendirmek için kullanır. Çeşitli yerlerinde ağ exchange TCP paketleri veya ICMP Yankı (izleme için seçilen Protokolü) bağlı olarak birbirleriyle yüklü OMS aracılar. İşlem sırasında aracıları varsa gidiş dönüş süresi ve paket kaybı öğrenin. Düzenli olarak, her bir aracının bir izleme yolu tüm çeşitli yollar test edilmelidir ağda bulmak için diğer aracılara gerçekleştirir. Bu verileri kullanarak, aracıları ağ gecikme süresi ve paket kaybı rakamları türetme. Testler, her beş saniyede ve verileri toplanır üç dakika boyunca aracıları tarafından için günlük analizi hizmeti yüklemeden önce yinelenir. 



>[!NOTE]
> Aracıları sık birbirleriyle iletişim karşın, bunların önemli ölçüde ağ trafiği testleri yürütürken oluşturmaz. Aracıları kaybı ve gecikme--paketler arasında alınıp verilen veri belirlemek için yalnızca TCP Eşitlemeye SYNACK ACK el sıkışma paketlerde kullanır. Bu işlem sırasında aracılar birbiriyle yalnızca gerekli olduğunda iletişim ve aracı iletişim topolojisinin ağ trafiğini azaltmak için optimize edilmiştir.

## <a name="using-the-solution"></a>Çözümü kullanma 

### <a name="npm-overview-tile"></a>NPM genel bakış kutucuğu 

Ağ Performansı İzleyicisi çözüm etkinleştirdikten sonra genel bakış sayfasında çözüm kutucuğu ağ durumunu hızlı bir genel bakış sağlar. 

 ![NPM genel bakış kutucuğu](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Ağ Performans İzleyicisi Panosu 

**Üst ağ sistem durumu olayları** sayfa listesini sağlar en son sistem durumu olayları ve Uyarıları sistem ve saat olay etkin edildiğinden. İzleme kural için seçilen ölçüm (kaybı, gecikme, yanıt süresi veya bant genişliği kullanımını) değeri eşiği aştığında bir sistem durumu olayı ya da uyarı oluşturulur. 

 **Performans İzleyicisi** sayfa ağ bağlantılarının durumunu bir özetini sunar ve alt ağ bağlantıları çözümü tarafından izleniyor. Topoloji döşeme ağınızda izlenmekte olan ağ yollarını sayısı bildirir. Bu kutucuğa doğrudan tıklandığında topoloji görünümü gider. 

 **Hizmet uç noktası İzleyicisi** sayfasında oluşturduğunuz farklı testleri sistem durumu özetini sağlar. Topoloji döşeme izlenmekte olan uç noktaların sayısını bildirir. Bu kutucuğa doğrudan tıklandığında topoloji görünümü gider.

 **ExpressRoute İzleyici** sayfası çözümü tarafından izlenmekte olan çeşitli ExpressRoute eşleme bağlantıların sistem durumu özetini sağlar. Topoloji döşeme ağınızda izlenmekte olan ExpressRoute circuit(s) aracılığıyla ağ yollarını sayısı bildirir. Bu kutucuğa doğrudan tıklandığında topoloji görünümü gider.

 **Genel sorgular** sayfa ham ağ verileri doğrudan izleme fetch arama sorguları kümesi içerir. Özelleştirilmiş raporlama için kendi sorguları oluşturmak için bu sorguları bir başlangıç noktası olarak kullanabilirsiniz. 

![NPM Panosu](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>derinliği için detaya gitme 

İncelemek için çözüm panosunda çeşitli bağlantıları tıklatabilirsiniz ilgilendiğiniz herhangi bir alan içine daha derin aşağı. Örneğin, bir uyarı veya Panoda görünmesini bir sağlıksız ağ bağlantısı gördüğünüzde, daha fazla araştırmak için tıklatabilirsiniz. Belirli bir ağ bağlantısı için tüm alt ağ bağlantıları listeleyen bir sayfaya yönlendirilirsiniz. Her alt ağ bağlantı kaybı, gecikme ve sistem durumunu görebilmek için ve hangi alt ağ bağlantıları bulma sorunu neden hızlı bir şekilde. Daha sonra tıklatabilirsiniz **görüntülemek düğüm bağlantıları** sağlıksız alt ağ bağlantısı için tüm düğüm bağlantıları görmek için. Ardından, tek tek düğümü düğümü bağlantılara bakın ve sağlıksız düğüm bağlantıları bulabilirsiniz. 

Tıklayabilirsiniz **görünüm topoloji** kaynak ve hedef düğümleri arasında yolların atlama atlamalı topolojisini görüntülemek için. Sağlıksız yollar kırmızı olarak gösterilir ve hızlı bir şekilde ağ belirli bir kısmını sorunu belirleyebilir atlamalar tarafından katkıda bulunan gecikme görüntüleyebilirsiniz. 

 

### <a name="network-state-recorder"></a>Ağ durumu Kaydedicisi 

Her görünüm, ağ durumu görüntüsünü belirli bir noktada zamanında görüntüler. Varsayılan olarak, en son durum gösterilir. Sayfanın üstündeki çubuğu noktası durumu görüntülenmektedir zamanında gösterir. Zamanında geri dönün ve Eylemler çubuğunda tıklatarak, ağ durumu görüntüsünü görüntülemek seçebilirsiniz. Etkinleştirmek veya en son durumunu görüntülerken herhangi bir sayfa için Otomatik yenilemeyi devre dışı bırakmak seçebilirsiniz. 

 ![Ağ durumu Kaydedicisi](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Eğilim grafikleri 

Ayrıntıya her düzeyde Geçerli ölçüm – kaybı, gecikme, yanıt süresi, bant genişliği kullanımı eğilimi görebilirsiniz. Grafik üstünde zamanı denetimi kullanarak eğilim için zaman aralığını değiştirebilirsiniz. 

Eğilim grafikleri bir geçmiş açısından bir performans ölçümü performansını gösterir. Bazı ağ sorunları doğası gereği geçici ve yalnızca geçerli durumunu ağ bakarak catch zor olabilir. Sorunları hızla yüzey ve herkes, yalnızca daha sonraki bir noktada zamanında yeniden bildirimler önce kayboluyor olmasıdır. Bu sorunlar nedeniyle genellikle yüzeyini uygulama yanıt süresini, tüm uygulama bileşenleri sorunsuzca çalıştırması göründüğünde bile açıklanamayan artışlar olarak tür geçici sorunlar Ayrıca uygulama yöneticileri için zor olabilir. 

Bu tür sorunları sorun ani bir depo ağ gecikmesi veya paket kaybı göründüğü eğilim Grafiği bakarak kolayca algılayabilir. Ardından, sorunun oluştuğu zamanında ağ anlık görüntü ve o noktadan topolojisini görüntülemek için ağ durumu kaydedicisini kullanarak sorunu araştırabilirsiniz. 

 
![Eğilim grafikleri](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topoloji Haritası 

NPM, etkileşimli topoloji haritaya kaynak ve hedef uç nokta arasındaki yolları atlama atlamalı topolojisini gösterir. Topoloji Haritası tıklayarak görüntüleyebilirsiniz **topoloji** kutucuğuna tıklayarak çözüm Panosu açıp **görünüm topoloji** ayrıntıya sayfalarında bağlantı.  

Kaynak ve hedef arasında ne kaç yollar, topoloji Haritası görüntüler yolları veri paketlerinin alın. Her ağ atlama tarafından katkıda gecikme süresi de görünür olur. Toplam yolu gecikme süresi (karşılık gelen izleme kuralında ayarlanır) eşiğin üstünde olduğu tüm yolları kırmızı olarak gösterilir.  

Bir düğüm veya vurgulu topoloji haritasında tıklattığınızda, FQDN ve IP adresi gibi düğümünün özelliklerine bakın. IP adresini görmek için bir atlama'ı tıklatın. Sorunlu ağ atlama tarafından katkısı gecikme tercihinize göre tanımlayabilirsiniz. Belirli yollar daraltılabilir Eylem Bölmesi'nde filtreleri kullanarak filtreleme seçebilirsiniz. Eylem Bölmesi'nde kaydırıcıyı kullanarak ara atlama gizleme tarafından Ayrıca ağ topolojileri basitleştirebilirsiniz. Yakınlaştırma veya için fare tekerleği kullanarak topoloji haritasını out. 

Haritada gösterilen topolojisi Katman 3 topolojisi ve Katman 2 aygıtlarını ve bağlantıları içermiyor unutmayın. 

 
![Topoloji Haritası](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Log Analytics Search 

NPM Pano üzerinden kullanıma sunulan grafik ve ayrıntıya sayfaları tüm veriler kullanılabilir da yerel olarak [günlük analizi arama](log-analytics-log-search-new.md). Etkileşimli veri deposunda çözümlemesi, farklı kaynaklardaki verilerin bağıntısını, özel uyarılar oluşturun, özel görünümlerini oluşturma ve verileri Excel, Powerbı veya paylaşılabilir bağlantı verin. Pano genel sorgular alanında kendi sorgular ve raporlar oluşturmak için başlangıç noktası olarak kullanabileceğiniz bazı yararlı sorgular var. 

 

## <a name="provide-feedback"></a>Geri bildirimde bulunma 

**UserVoice** -fikirlerinizi bize üzerinde çalışmak istediğiniz ağ performansı İzleyicisi özellikleri için nakledebilirsiniz. Ziyaret bizim [UserVoice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

**Bizim kohort katılma** -her zaman yeni müşteriler bizim kohort katılma elde etmeyle ilgilenen çalışıyoruz. Parçası olarak bu, yeni özellikler erken erişim edinmek ve ağ Performans İzleyicisi'ni iyileştirmemize yardımcı olun. Bağlama düşünüyorsanız dolgu bu genişletme [hızlı anket](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Sonraki adımlar 
- Daha fazla bilgi edinmek [Performans İzleyicisi'ni](log-analytics-network-performance-monitor-performance-monitor.md), [hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md), ve [ExpressRoute İzleyici](log-analytics-network-performance-monitor-expressroute.md). 
