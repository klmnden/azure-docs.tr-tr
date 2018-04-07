---
title: Ağ Azure Performans İzleyicisi çözümde | Microsoft Docs
description: Ağ Performans İzleyicisi'nde Azure algılamak ve ağ performans sorunları bulmak için yakın gerçek zamanlı, ağlarınızı performansını izlemenize yardımcı olur.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 6e81f2cdd0cd3f62d93c85c1a073e0b9df542ec7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Performans İzleyicisi çözümde Azure ağ

![Ağ Performans İzleyici simgesi](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Ağ Performansı İzleyicisi, izleme, ağ altyapınızın çeşitli noktaları arasındaki ağ performansını izlemenize yardımcı olur çözümü karma bulut tabanlı bir ağdır. Ayrıca hizmet ve uygulama uç noktaları ve izlemek için ağ bağlantısı izlemenize yardımcı olur Azure ExpressRoute performansını. 

Ağ Performansı İzleyicisi trafiği blackholing, yönlendirme hataları ve geleneksel ağ izleme yöntemleri algılayabilir olmayan sorunlar gibi ağ sorunları algılar. Çözüm uyarılar oluşturur ve bir ağ bağlantısı için bir eşik aşıldığında size bildirir. Ayrıca, ağ performans sorunlarını zamanında algılanması sağlar ve belirli ağ kesimine veya cihaza sorunun kaynağını yerelletirilmesi. 

Ağ Performansı İzleyicisi üç geniş yetenekleri sunar: 

* [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md): Bulut dağıtımları ve şirket içi konumlara arasında ağ bağlantısı izleyebilirsiniz birden çok veri merkezleri ve şube ofislerinde ve kritik çok katmanlı uygulamalar veya mikro. Performans izleme ile kullanıcılar şikayetçi önce ağ sorunları algılayabilir.

* [Hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-service-endpoint.md): hizmetlere önem verdiğiniz, hangi altyapıya yolda olduğunu belirlemek ve ağ performans sorunlarını gerçekleştiği tanımlamak, kullanıcılardan bağlantı izleyebilirsiniz. Kullanıcılarınızın önce kesintileri hakkında bilmeniz ve ağ yol boyunca sorunları tam konumunu görebilirsiniz. 

    Bu özellik HTTP, HTTPS, TCP ve ICMP içinde yakın gerçek zamanlı veya geçmişte hizmetinizin yanıt süresi ve kullanılabilirliğini izlemek için göre testleri gerçekleştirmenize yardımcı olur. Paket kaybı ve gecikme süresi ağında katkı da izleyebilirsiniz. Ağ topolojisi haritası ile ağ yavaşlamalara ayırabilirsiniz. Ağ yol boyunca her atlama gecikme verileriyle hizmetine düğümden oluşan sorun noktalar tanımlayabilirsiniz. Yerleşik testleri ile Office 365 ve Dynamics CRM herhangi önceden yapılandırma olmadan ağ bağlantısı izleyebilirsiniz. Bu özellik ile Web siteleri, SaaS uygulamaları, PaaS uygulamaları ve SQL veritabanları gibi tüm TCP özellikli uç noktası için ağ bağlantısı izleyebilirsiniz.

* [ExpressRoute İzleyici](log-analytics-network-performance-monitor-expressroute.md): Azure ExpressRoute uçtan uca bağlantı ve şube ofislerinde ve Azure arasında performans izleme.  

Tarafından desteklenen çeşitli özellikleri hakkında daha fazla bilgi [Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) çevrimiçidir.
 
## <a name="supported-regions"></a>Desteklenen bölgeler
NPM aşağıdaki bölgelerinden birinde barındırılan bir çalışma alanından world herhangi bir kısmını uygulamalarda ağları arasında bağlantısı izleyebilirsiniz:
* Batı Avrupa
* Batı Orta ABD
* Doğu ABD
* Güneydoğu Asya
* Güney Doğu Avustralya
* Birleşik Krallık Güney
* ABD hükümeti Virginia

ExpressRoute İzleyici kullanılabilir listesini desteklenen bölgeler [belgelerine](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Ayarlama ve yapılandırma

### <a name="install-and-configure-agents"></a>Yükleme ve aracıları yapılandırma 

Temel işlemleri sırasında aracıları yüklemek için kullanmak [Azure günlük Analizi'ne bağlanmak Windows bilgisayarlara](log-analytics-windows-agents.md) ve [Operations Manager'a günlük analizi](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Aracıları yükleneceği yeri 

* **Performans İzleyicisi**: en az bir düğümde yüklemek Operations Management Suite aracıları bağlı istediğiniz diğer alt ağlara ağ bağlantısı izlemek her alt ağ.

    Bir ağ bağlantısı izlemek için o bağlantı üzerindeki her iki uç noktaları aracıları yükleyin. Ağınızın topolojisi hakkında emin değilseniz, aracıları sunucuları ağ performansını izlemek istediğiniz kritik iş yükleri ile yükleyin. Örneğin, bir web sunucusu ve SQL çalıştıran bir sunucu arasındaki ağ bağlantısını izlemek istiyorsanız, her iki sunucuda bir aracı yükleyin. Aracıları konakları, konaklar kendilerini arasında ağ bağlantısı (Bağlantılar) izleyin. 

* **Hizmet uç noktası İzleyicisi**: hizmet uç noktası için ağ bağlantısını izlemeniz istediğiniz her bir düğümde bir Operations Management Suite aracısı yükleyin. Ağ bağlantısı için Office 365 O1, O2 ve O3 etiketli office sitelerinden izlemek istediğiniz varsa bir örnektir. En az bir düğüm üzerindeki her O1, O2 ve O3 Operations Management Suite Aracısı'nı yükleyin. 

* **ExpressRoute İzleyici**: Azure sanal ağınızda en az bir Operations Management Suite aracısı yükleyin. Ayrıca ExpressRoute özel eşleme aracılığıyla bağlı, şirket içi alt ağ içinde en az bir aracı yükleyin.  

### <a name="configure-operations-management-suite-agents-for-monitoring"></a>İzleme için Operations Management Suite aracıları yapılandırma 

Ağ Performansı İzleyicisi yapay işlemler kaynak ve hedef aracılar arasındaki ağ performansını izlemek için kullanır. Performans İzleyicisi'ni ve hizmet uç noktası İzleyicisi özelliklerinden izleme için protokol olarak TCP ve ICMP arasında seçim yapabilirsiniz. TCP ExpressRoute izleme için kullanılır. Güvenlik Duvarı'nın seçtiğiniz protokol izlemesi için kullanılan Operations Management Suite aracılar arasındaki iletişimi izin verdiğinden emin olun. 

* **TCP Protokolü**: izleme için protokol olarak TCP seçerseniz, aracıları birbirlerine bağlanabildiğinden emin olmak için ağ Performans İzleyicisi'ni ve ExpressRoute İzleyicisi için kullanılan aracılar güvenlik duvarı bağlantı noktası açın. Bağlantı noktasını açmak için yönetici ayrıcalıklarıyla bir PowerShell penceresinde hiçbir parametre olmadan EnableRules.ps1 PowerShell komut dosyasını çalıştırın.

    Komut dosyası çözümü tarafından gerekli kayıt defteri anahtarları oluşturur. Ayrıca, TCP bağlantıları birbirleri ile oluşturmak aracıları izin vermek için Windows Güvenlik duvarı kuralları oluşturur. Komut dosyası tarafından oluşturulan kayıt defteri anahtarlarını oturum hata ayıklama günlükleri ve günlükleri dosyasının yolunu belirtin. Komut dosyası ayrıca iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarlar için değerleri otomatik olarak komut dosyası tarafından ayarlanır. Bu anahtarları el ile değiştirmeyin. Varsayılan portunu 8084 ' dir. Komut parametresi BağlantıNoktasıNumarası sağlayarak, özel bir bağlantı noktası kullanabilirsiniz. Komut dosyası çalıştırdığı tüm bilgisayarlarda aynı bağlantı noktasını kullanın. 

    >[!NOTE]
    > Komut dosyası, yalnızca Windows Güvenlik Duvarı'nı yerel olarak yapılandırır. Ağ güvenlik duvarı varsa, Ağ Performansı İzleyicisi tarafından kullanılan TCP bağlantı noktası için giden trafiğe izin verdiğinden emin olun.

    >[!NOTE]
    > Hizmet uç noktası İzleyicisi EnableRules.ps1 PowerShell betiğini çalıştırmak gerekmez.

    

* **ICMP Protokolü**: izleme için protokol olarak ICMP seçerseniz, ICMP güvenilir bir şekilde kullanmak aşağıdaki güvenlik duvarı kurallarını etkinleştirin:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>Çözümünüzü yapılandırma 

1. Ağ Performansı İzleyicisi çözüm alanınızdan ekleyin [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Açıklanan işlemi de kullanabilirsiniz [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md). 
2. Günlük analizi çalışma alanını açın ve seçin **genel bakış** döşeme. 
3. Seçin **Ağ Performansı İzleyicisi** döşeme iletiyle *çözüm ek yapılandırma gerektirir*.

   ![Ağ Performans İzleyicisi kutucuğu](media/log-analytics-network-performance-monitor/npm-config.png)

4. Üzerinde **Kurulum** sayfaya, Operations Management Suite aracıları yükleme ve izleme için aracıları yapılandırma seçeneğine bakın **ortak ayarları** görünümü. Operations Management Suite aracıları yükleyip, daha önce anlatıldığı olarak, seçin **Kurulum** özelliği yapılandırmak için kullanmak istediğiniz görünümü. 

   **Performans İzleyicisi'ni**: içinde yapay işlemler için kullanılacak protokolü seçin **varsayılan** Performans İzleyicisi Kuralı ' nı seçip **devam & Kaydet**. Bu protokol seçimini yalnızca sistem tarafından oluşturulan varsayılan kuralı için tutar. Performans İzleyicisi kural açıkça oluşturma her zaman Protokolü seçmeniz gerekir. Her zaman için taşıyabilirsiniz **varsayılan** kural ayarları **Performans İzleyicisi** (göründüğü gün-0 yapılandırmanızı tamamladıktan sonra) sekmesini ve daha sonra değişiklik protokolü. İzleme özelliği rPerfomance istemiyorsanız, varsayılan kuraldan devre dışı bırakabilirsiniz **varsayılan** kural ayarları **Performans İzleyicisi** sekmesi.

   ![Performans İzleyicisi görünümü](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
   **Hizmet uç noktası İzleyicisi**: Office 365 ve Dynamics 365 aracılarınızı ağ bağlantısını izlemek için yerleşik önceden yapılandırılmış testleri yeteneği sağlar. Bunları yanındaki onay kutularını işaretleyerek izlemek istediğiniz Office 365 ve Dynamics 365 hizmetleri seçin. İstediğiniz izlemek aracıları seçmek için Seç **eklemek aracıları**. Daha sonra ayarlamak için bu özelliği veya istediğiniz kullanmak istemiyorsanız, olmayan herhangi bir şey seçip **devam & Kaydet**.

   ![Hizmet uç noktası İzleyicisi görünümü](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

   **ExpressRoute İzleyici**: seçin **Şimdi Bul** Azure Abonelikteki sanal ağlara bağlanan özel eşlemeler, bu günlük analizi çalışma alanı ile bağlantılı tüm ExpressRoute bulmak için. 

   >[!NOTE] 
   > Çözüm şu anda yalnızca ExpressRoute özel eşlemeler bulur. 

   >[!NOTE] 
   > Yalnızca bu günlük analizi çalışma alanı ile bağlantılı abonelikle ilişkili sanal ağlara bağlanan özel eşlemeler bulunur. ExpressRoute, bu çalışma alanına bağlı abonelik dışında sanal ağlara bağlıysa, bu Aboneliklerde günlük analizi çalışma alanı oluşturun. Bu eşlemeler izlemek için ağ Performans İzleyicisi'ni kullanın.

   ![ExpressRoute İzleyicisi görünümü](media/log-analytics-network-performance-monitor/npm-express-route.png)

   Bulma işlemi tamamlandıktan sonra bulunan özel eşlemeleri bir tablodaki listelenir. 

   ![Ağ Performans İzleyicisi'ni yapılandırma sayfası](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
Bu eşlemeler için izleme başlangıçta bir devre dışı bırakılmış durumda. İzlemek istediğiniz her eşleme seçin ve Ayrıntılar görünümündeki sağdaki izlemesini bunlar için yapılandırın. Seçin **kaydetmek** yapılandırmayı kaydetmek için. Daha fazla bilgi için "Yapılandırma ExpressRoute izleme" makalesine bakın. 

Kurulum tamamlandıktan sonra verilerin doldurmak bir saat 30 dakika sürer. Çözüm, ağ üzerinden verileri toplar, ancak iletiyi görmek *çözüm ek yapılandırma gerektirir* ağ Performans İzleyicisi'ni üzerinde **genel bakış** döşeme. Veriler toplanır ve dizinli sonra **genel bakış** döşeme değiştirir ve bir Özet olarak, ağ durumu, size bildirir. Ardından, ortamınızdan bulunan alt ağlar yanı sıra hangi Operations Management Suite aracılarının yüklü olduğu düğümleri izlenmesini düzenleyebilirsiniz.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Alt ağları ve düğümler ilişkin izleme ayarlarını Düzenle 

En az bir aracısının yüklü olduğu tüm alt listelendiğini **ağlarla** yapılandırma sayfasında sekmesi. 


Etkinleştirmek veya belirli alt ağlar izleme devre dışı bırakmak için:

1. Seçin veya yanındaki onay kutusunu temizleyin **alt ağ kimliği**. Ardından olduğundan emin olun **kullanım izleme için** seçili veya temizlenmiş, uygun şekilde şeklindedir. Seçin veya birden çok alt ağı temizleyin. Devre dışı bırakıldığında, alt ağlar izlenen değil ve diğer aracıları ping işlemi durdurmak için aracıları güncelleştirilir. 
2. Belirli bir alt ağ içinde izlemek istediğiniz düğümleri seçin. Alt ağ listeden seçin ve gerekli düğümleri izlenmeyen ve izlenen düğümleri içeren listeleri arasında taşıyın. Alt ağ için özel bir açıklama ekleyebilirsiniz.
3. Seçin **kaydetmek** yapılandırmayı kaydetmek için. 

#### <a name="choose-nodes-to-monitor"></a>İzleme düğümü seçin

Bir aracısı yüklü olan tüm düğümleri listelendiğini **düğümleri** sekmesi. 

1. Seçin veya izlemek ya da izlemeyi durdurmak istediğiniz düğümleri temizleyin. 
2. Seçin **kullanım izleme için**, veya, uygun şekilde temizleyin. 
3. **Kaydet**’i seçin. 


İstediğiniz özellikleri yapılandırın:

- [Performans İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [ExpressRoute Monitor](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Veri toplama ayrıntıları
Kaybı ve gecikme bilgileri toplamak için protokol olarak TCP seçtiğinizde TCP Eşitlemeye SYNACK ACK el sıkışma paketleri ağ Performans İzleyicisi'ni kullanır. Protokol olarak ICMP seçtiğinizde ağ Performans İzleyicisi ICMP YANKI ICMP YANKI YANITI kullanır. İzleme yolu topoloji bilgilerini almak için de kullanılır.

Aşağıdaki tabloda, veri toplama yöntemleri ve Ağ Performansı İzleyicisi için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | System Center Operations Manager Aracısı | Azure Storage | Operations Manager gerekli? | Operations Manager Aracısı verilerinin yönetim grubu gönderilen | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Gönderilen TCP el sıkışmaları/ICMP YANKI iletileri 5 saniyede veri 3 dakikada bir |
 

 
Çözüm yapay işlemler ağ durumunu değerlendirmek için kullanır. Çeşitli yerlerinde ağ exchange TCP paketleri veya ICMP Yankı birbiriyle Operations Management Suite aracıları yüklü. Aracıları TCP paketleri veya ICMP Yankı kullanıp izleme için seçilen Protokolü bağlıdır. İşlem sırasında aracıları varsa gidiş dönüş süresi ve paket kaybı öğrenin. Düzenli olarak, her bir aracının bir izleme yolu tüm çeşitli yollar test edilmelidir ağda bulmak için diğer aracılara gerçekleştirir. Bu verileri kullanarak, aracıları ağ gecikme süresi ve paket kaybı rakamları türetme. Testleri beş saniyede yinelenir. Günlük analizi hizmeti karşıya önce veri yaklaşık üç dakika boyunca aracıları tarafından toplanır.



>[!NOTE]
> Aracıları sık birbirleriyle iletişim rağmen testleri yürütürken önemli ölçüde ağ trafiği üretme. Aracıları kaybı ve gecikme belirlemek için yalnızca TCP Eşitlemeye SYNACK ACK el sıkışma paketlerde kullanır. Hiçbir veri paketlerinin değiştirilir. Bu işlem sırasında aracılar birbiriyle yalnızca gerekli olduğunda iletişim kurar. Aracı iletişim topolojisinin ağ trafiğini azaltmak için optimize edilmiştir.

## <a name="use-the-solution"></a>Çözüm kullanın 

### <a name="network-performance-monitor-overview-tile"></a>Ağ Performans İzleyicisi'ne genel bakış kutucuğu 

Ağ Performansı İzleyicisi çözüm etkinleştirdikten sonra çözümü kutucuğunu **genel bakış** sayfası, ağ durumu hızlı bir genel bakış sağlar. 

 ![Ağ Performans İzleyicisi'ne genel bakış kutucuğu](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Ağ Performans İzleyicisi Panosu 

* **İlk ağ sistem durumu olayları**: olaylar etkin olduğu beri bu sayfayı listesini en son sistem durumu olayları ve Uyarıları sistem ve saat sağlar. İzleme kural için seçilen ölçüm (kaybı, gecikme, yanıt süresi veya bant genişliği kullanımını) değeri eşiği aştığında bir sistem durumu olayı ya da uyarı oluşturulur. 

* **ExpressRoute İzleyici**: Bu sayfa çeşitli ExpressRoute eşdüzey bağlantılar için sistem durumu özetlerini, çözümü sağlar. izleyiciler. **Topoloji** döşeme ağınızda ağ yollarını izlenen ExpressRoute bağlantı hatları aracılığıyla sayısını gösterir. Gitmek için bu kutucuğu seçin **topoloji** görünümü.

* **Hizmet uç noktası İzleyicisi**: Bu sayfa, oluşturduğunuz farklı testler için sistem durumu özetleri sağlar. **Topoloji** döşeme izlenen uç noktaların sayısını gösterir. Gitmek için bu kutucuğu seçin **topoloji** görünümü.

* **Performans İzleyicisi'ni**: Bu sayfa için durum özetleri sağlar **ağ** bağlantılar ve **alt ağ** çözümü izler bağlantılar. **Topoloji** döşeme ağınızda izlenen ağ yollarını sayısını gösterir. Gitmek için bu kutucuğu seçin **topoloji** görünümü. 

* **Genel sorgular**: Bu sayfa ham ağ verileri doğrudan izleme fetch arama sorguları bir dizi içerir. Bu sorgular, özelleştirilmiş raporlama için kendi sorguları oluşturmak için bir başlangıç noktası olarak kullanabilirsiniz. 

   ![Ağ Performans İzleyicisi Panosu](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>derinliği için detaya gitme 

Çözüm Panosu incelemek için çeşitli bağlantıları seçebilirsiniz ilgilendiğiniz herhangi bir alan içine daha derin aşağı. Örneğin, bir uyarı veya Panoda görünmesini bir sağlıksız ağ bağlantısı gördüğünüzde, daha fazla araştırmak için seçin. Bir sayfa belirli ağ bağlantısı için tüm alt ağ bağlantıları listeler. Kaybı, gecikme ve her alt ağ bağlantısı sistem durumunu görebilirsiniz. Hangi alt ağ bağlantısını sorunlara neden çıkışı hızlı bir şekilde bulabilirsiniz. Seçin **görüntülemek düğüm bağlantıları** sağlıksız alt ağ bağlantısı için tüm düğüm bağlantıları görmek için. Ardından, tek tek düğümü düğümü bağlantılara bakın ve sağlıksız düğüm bağlantıları bulabilirsiniz. 

Seçin **görünüm topoloji** kaynak ve hedef düğümleri arasında yolların atlama atlamalı topolojisini görüntülemek için. Sağlıksız yollar kırmızı olarak görünür. Hızlı ağ belirli bir kısmını sorunu belirleyebilir atlamalar tarafından katkıda bulunan gecikme görüntüleyebilirsiniz.

 

### <a name="network-state-recorder-control"></a>Ağ durumu Kaydedici denetimi

Her görünüm, ağ durumu görüntüsünü belirli bir noktada zamanında görüntüler. Varsayılan olarak, en son durum gösterilir. Sayfanın üstündeki çubuğu noktası durumu görüntülendiği süre gösterir. Önceki aynı anda bir anlık görüntüsünü ağ durumunu görüntülemek için seçin **Eylemler**. Ayrıca etkinleştirin veya en son durumunu görüntülerken herhangi bir sayfa için Otomatik yenilemeyi devre dışı bırakın. 

 ![Ağ durumu Kaydedicisi](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Eğilim grafikleri 

Ayrıntıya her düzeyde Geçerli ölçüm eğilimi görebilirsiniz. Kaybı, gecikme, yanıt süresi veya bant genişliği kullanımını olabilir. Eğilim için zaman aralığını değiştirmek için grafiğin üstünde zamanı denetimi kullanın. 

Eğilim grafikleri bir geçmiş açısından bir performans ölçümü performansını gösterir. Bazı ağ sorunları doğası gereği geçici ve ağın geçerli durumu yalnızca bakarak catch zordur. Sorunları hızla yüzey ve herkes, yalnızca daha sonraki bir noktada zamanında yeniden bildirimler önce kaybolur. Tür geçici sorunlar, ayrıca uygulama yöneticileri için zor olabilir. Sorunları genellikle uygulama yanıt süresini açıklanamayan artışlar olarak görünmesini bile, tüm uygulama bileşenleri sorunsuzca çalıştırması görünür. 

Bir eğilim Grafiği bakarak bu tür sorunları kolayca algılayabilir. Sorun, ağ gecikmesi veya paket kaybı ani bir depo görünür. Sorunu araştırmaya ağ anlık görüntü ve o noktadan topolojisini sorunu gerçekleştiği anda görüntülemek için ağ durumu Kaydedici denetimi kullanın.

 
![Eğilim grafikleri](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Topoloji Haritası 

Ağ Performansı İzleyicisi etkileşimli topoloji Haritası kaynak ve hedef noktadaki arasındaki yolları atlama atlamalı topolojisini gösterir. Topoloji Haritası görüntülemek için seçin **topoloji** döşeme çözüm panosunda. Öğesini de seçebilirsiniz **görünüm topoloji** ayrıntıya sayfalarında bağlantı. 

Kaynak ve hedef arasında ne kaç yollar, topoloji Haritası görüntüler yolları veri paketlerinin alın. Her ağ atlama tarafından katkıda gecikme süresi de görünür olur. Toplam yolu gecikme süresi (karşılık gelen izleme kuralında ayarlanır) eşiğin üstünde olduğu tüm yolları kırmızı olarak gösterilir. 

Bir düğüm veya vurgulu topoloji haritasında seçtiğinizde, FQDN ve IP adresi gibi düğüm özellikleri bakın. IP adresini görmek için bir atlama seçin. Sorunlu ağ atlama katkıda bulunduğu gecikme tercihinize göre tanımlayabilirsiniz. Belirli yollar filtrelemek için daraltılabilir Eylem Bölmesi'nde filtreleri kullanın. Ağ topolojisi basitleştirmek için eylem bölmesinde kaydırıcıyı kullanarak ara atlama gizleyin. Yakınlaştırma veya fare tekerleği kullanarak dışında topoloji Haritası Yakınlaştır. 

Haritada gösterilen topolojisi Katman 3 topolojisi ve Katman 2 aygıtlarını ve bağlantıları içermiyor. 

 
![Topoloji Haritası](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Günlük analizi arama 

Ağ Performansı İzleyicisi Pano üzerinden kullanıma sunulan grafik ve ayrıntıya sayfaları tüm veriler kullanılabilir da yerel olarak [günlük analizi arama](log-analytics-log-search-new.md). Etkileşimli veri deposunda çözümlemesi ve farklı kaynaklardaki verilerin bağıntısını. Ayrıca özel uyarılar ve görünümler oluşturma ve verileri Excel, Power BI veya paylaşılabilir bağlantı verin.  **Genel sorgular** panosunda alana sahip başlangıç noktası olarak kendi sorgular ve raporlar oluşturmak için kullanabileceğiniz bazı yararlı sorgular. 

##<a name="pricing"></a>Fiyatlandırma

Fiyatlandırma hakkında daha fazla bilgi edinilebilir [çevrimiçi](log-analytics-network-performance-monitor-pricing-faq.md).

## <a name="provide-feedback"></a>Geri bildirimde bulunma 

* **UserVoice:** fikirlerinizi bize üzerinde çalışmak istediğiniz ağ performansı İzleyicisi özellikleri için nakledebilirsiniz. Ziyaret [UserVoice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Bizim kohort Katıl:** her zaman yeni müşteriler bizim kohort katılma elde etmeyle ilgilenen çalışıyoruz. Bunun bir parçası olarak, yeni özellikleri ve ağ Performans İzleyicisi'ni geliştirmemize yardımcı olmak için bir fırsat erken erişim edinmek. Bağlama düşünüyorsanız, bu dolgu [hızlı anket](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Sonraki adımlar 
Daha fazla bilgi edinmek [Performans İzleyicisi'ni](log-analytics-network-performance-monitor-performance-monitor.md), [hizmet uç noktası İzleyicisi](log-analytics-network-performance-monitor-performance-monitor.md), ve [ExpressRoute İzleyici](log-analytics-network-performance-monitor-expressroute.md). 
