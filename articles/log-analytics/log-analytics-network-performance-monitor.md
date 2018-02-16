---
title: "Ağ Azure günlük analizi Performans İzleyicisi çözümde | Microsoft Docs"
description: "Ağ Performans İzleyicisi'nde Azure günlük analizi algılamaya ağları eklentinizi real-zamanlı yeri yakın performansını izlemenize yardımcı olur ve ağ performans sorunları bulun."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: magoedte
ms.openlocfilehash: 5fc2477e566fdea76294b62a738c0e18facbe629
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="network-performance-monitor-solution-in-log-analytics"></a>Günlük analizi Performans İzleyicisi çözümde ağ

![Ağ Performans İzleyici simgesi](./media/log-analytics-network-performance-monitor/npm-symbol.png)

Bu belge, ayarlama ve ağlar eklentinizi real-zamanlı yeri yakın performansını izlemenize yardımcı olan günlük analizi çözümde algılamak ağ Performans İzleyicisi'ni kullanın ve ağ performans sorunları bulmak açıklar. Ağ Performansı İzleyicisi çözümüyle kaybına ve alt ağları veya sunucuları iki ağ arasında gecikme izleyebilirsiniz. Ağ Performansı İzleyicisi trafiği blackholing, yönlendirme hataları ve geleneksel ağ izleme yöntemleri algılayabilir olmayan sorunlar gibi ağ sorunları algılar. Ağ Performansı İzleyicisi uyarılar oluşturur ve olarak ve bir ağ bağlantısı için bir eşik aşıldığında bildirir. Bu eşikler sistem tarafından otomatik olarak öğrenilebilecek veya bunları özel uyarı kuralları kullanmak üzere yapılandırabilirsiniz. Ağ Performansı İzleyicisi ağ performans sorunlarını zamanında algılanması sağlar ve belirli ağ kesimine veya cihaza sorunun kaynağını yerelletirilmesi.

Çözüm Panosu ağ sorunları algılayabilir. Son ağ sistem durumu olayları, sağlıksız ağ bağlantıları ve yüksek paket kaybı ve gecikme süresi karşılıklı alt ağ bağlantıları dahil olmak üzere ağınız hakkında özet bilgileri görüntüler. Alt ağ bağlantıları ve bunun yanı sıra düğümü düğümü bağlantılarını geçerli sistem durumunu görüntülemek için ağ bağlantısını ayrıntıya girebilirsiniz. Geçmiş eğilim kaybı ve gecikme ağ, alt ağ ve düğümü düğümü düzeyinde de görüntüleyebilirsiniz. Paket kaybı ve gecikme süresi geçmiş eğilim grafiklerde görüntüleyerek geçici ağ sorunları algılar ve bir topoloji Haritası üzerinde ağ engelleri bulun. Etkileşimli topoloji grafik atlama atlamalı ağ yollarını görselleştirmek ve sorunun kaynağını belirlemek sağlar. Diğer çözümleri gibi ağ performansı İzleyicisi tarafından toplanan verileri temel alan özel raporlar oluşturmak için günlük arama çeşitli analytics gereksinimleri için kullanabilirsiniz.

Çözüm yapay işlemler, ağ hataları algılamak için birincil mekanizması olarak kullanır. Bu nedenle, belirli ağ cihazın satıcı veya model için bakmadan kullanabilirsiniz. Şirket içi, bulut (Iaas) ve karma ortamlar arasında çalışır. Çözüm, ağ topolojisini ve ağınızdaki çeşitli yolları otomatik olarak bulur.

Tipik ağ izleme ürünleri ağ aygıtı (yönlendiriciler, anahtarlar vb.) sistem durumu izleme odaklanmak ancak ağ performansı İzleyicisi mu iki nokta arasında ağ bağlantısı gerçek kalitesini Öngörüler sağlamaz.

### <a name="using-the-solution-standalone"></a>Çözüm tek başına kullanma
Ağ bağlantıları, kritik iş yüklerini arasında kalitesini izlemek istiyorsanız, ağları, veri merkezleri veya office siteleri sonra Ağ Performansı İzleyicisi çözüm tek başına arasındaki bağlantı durumunu izlemek için kullanabilirsiniz:

* bir genel veya özel ağ kullanarak bağlanan birden çok veri merkezi veya office siteler
* İş kolu satır uygulama çalıştıran kritik iş yükleri
* Genel bulut Hizmetleri, Microsoft Azure veya Amazon Web Hizmetleri (AWS) ve şirket içi ağlarda, Iaas (VM) kullanılabilir varsa ve iletişime izin verecek şekilde yapılandırılmış ağ geçitleri gibi şirket içi ağlar ve bulut ağları arasında
* Hızlı rota kullandığınızda azure ve şirket içi ağlar

### <a name="using-the-solution-with-other-networking-tools"></a>Çözümü ile ağ diğer araçları kullanarak
Bir iş kolu satır uygulama izlemek istiyorsanız, diğer ağ araçları Yardımcısı çözüm olarak Ağ Performansı İzleyicisi çözüm kullanabilirsiniz. Yavaş bir ağ yavaş uygulamalara açabilir ve Ağ Performansı İzleyicisi tarafından temel ağ sorunları nedeniyle uygulama performans sorunlarını araştırmanıza yardımcı olabilir. Çözüm ağ aygıtları için herhangi bir erişim gerektirmediğinden, uygulama Yöneticisi ağ uygulamaları nasıl etkilediğini hakkında bilgi sağlamak için ağ takım Bel gerekmez.

İzleme Araçları diğer ağ zaten yatırım, çoğu geleneksel ağ izleme çözümleri uçtan uca ağ performans ölçümleri kaybı ve gecikme gibi Öngörüler sağlamadığı için Ayrıca, ardından çözümü bu araçlara tamamlayabilir.  Ağ Performansı İzleyicisi çözümü bu boşluğu doldurmak yardımcı olabilir.

## <a name="installing-and-configuring-agents-for-the-solution"></a>Çözüm için aracıları yükleme ve yapılandırma
Temel işlemleri sırasında aracıları yüklemek için kullanmak [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agent.md) ve [Operations Manager'a günlük analizi](log-analytics-om-agents.md).

> [!NOTE]
> Bul ve ağ kaynaklarınıza izlemek için yeterli veri olması için en az 2 aracıları yüklemeniz gerekir. Aksi durumda, ek aracıları yükleyip kadar Çözüm yapılandırılırken bir durumda kalır.
>
>

### <a name="where-to-install-the-agents"></a>Aracıları yükleneceği yeri
Aracıları yüklemeden önce ağınızı ve izlemek istediğiniz ağ hangi kısımlarının topolojisini dikkate alın. İzlemek istediğiniz her alt ağ için birden fazla aracı yüklemenizi öneririz. Diğer bir deyişle, izlemek istediğiniz her bir alt, iki veya daha fazla sunucular veya VM'ler seçin ve aracı yükler.

Ağınızın topolojisi hakkında emin değilseniz, ağ performansını izlemek istediğiniz kritik iş yükleri ile sunucularda aracıları yükleyin. Örneğin, bir Web sunucusu ve SQL Server çalıştıran bir sunucu arasında ağ bağlantısı izlemek isteyebilirsiniz. Bu örnekte, bir aracı sunucularda hem de yüklenir.

Aracıları konakları--konakları kendilerini arasında ağ bağlantısı (Bağlantılar) izleyin. Bu nedenle, bir ağ bağlantısı izlemek için o bağlantı üzerindeki her iki uç noktaları aracıları yüklemeniz gerekir.

### <a name="configure-agents"></a>Aracıları yapılandırın

Yapay işlemler için ICMP Protokolü kullanmayı düşünüyorsanız, güvenilir bir şekilde ICMP kullanan için aşağıdaki güvenlik duvarı kurallarını etkinleştirmeniz gerekir:

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow
```


TCP protokolü kullanmak istiyorsanız, aracıları iletişim kurabildiğinden emin olmak bu bilgisayarlar için güvenlik duvarı bağlantı noktalarını açmanız gerekir. İndirin ve çalıştırın [EnableRules.ps1 PowerShell Betiği](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) yönetici ayrıcalıklarıyla bir PowerShell penceresinde herhangi bir parametre olmadan.

Komut dosyası tarafından ağ Performans İzleyicisi'ni gerekli kayıt defteri anahtarları ve aracılar birbiriyle TCP bağlantıları oluşturmak izin vermek için Windows Güvenlik duvarı kuralları oluşturur. Komut dosyası tarafından oluşturulan kayıt defteri anahtarlarını da oturum hata ayıklama günlükleri ve günlükleri dosyasının yolunu belirtin. Ayrıca, iletişim için kullanılan Aracısı TCP bağlantı noktasını tanımlar. Bu anahtarları el ile değiştirmemelisiniz şekilde bu anahtarları için değerleri otomatik olarak komut dosyası tarafından ayarlanır.

Varsayılan portunu 8084 ' dir. Özel bir bağlantı noktası parametresi sağlayarak kullanabileceğiniz `portNumber` komut dosyası. Ancak, aynı bağlantı noktasını tüm bilgisayarlarda betiğin çalıştırıldığı kullanılması gerekir.

> [!NOTE]
> EnableRules.ps1 komut dosyası komut çalıştırıldığı bilgisayarda Windows Güvenlik duvarı kuralları yapılandırır. Ağ güvenlik duvarı varsa, Ağ Performansı İzleyicisi tarafından kullanılan TCP bağlantı noktası için giden trafiğe izin verdiğinden emin olun.
>
>

## <a name="configuring-the-solution"></a>Çözüm yapılandırılıyor
Yüklemek ve çözüm yapılandırmak için aşağıdaki bilgileri kullanın.

1. Ağ Performansı İzleyicisi çözüm Windows Server 2008 SP 1 veya sonrasını çalıştıran bilgisayarlar veya Windows 7 SP1 verileri alır veya daha sonra hangi gereksinimleri olarak Microsoft İzleme Aracısı'nı (MMA) aynı değil. NPM aracıları, Windows Masaüstü/istemci işletim sistemlerinde (Windows 10, Windows 8.1, Windows 8 ve Windows 7) olarak da çalıştırabilirsiniz.
    >[!NOTE]
    >Windows server işletim sistemleri için aracıları olarak yapay işlem protokolleri TCP ve ICMP destekler. Ancak, Windows istemci işletim sistemleri için aracıları yalnızca ICMP yapay işlem protokolü olarak destekler.

2. Ağ Performansı İzleyicisi çözüm alanınızdan ekleyin [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).<br><br> ![Ağ Performans İzleyici simgesi](./media/log-analytics-network-performance-monitor/npm-symbol.png)  
3. OMS portalında gördüğünüz başlıklı yeni bir kutucuk **Ağ Performansı İzleyicisi** iletiyle *çözüm ek yapılandırma gerektirir*. Gitmek için olan kutucuğuna tıklayın **dağıtım** sekmesinde ve ağ izleme için yapay işlemler yapmak için kullanılacak protokolü seçin.  Gözden geçirme [sağ Protokolü ICMP veya TCP seçin](#choose-the-right-protocol-icmp-or-tcp) doğru protokolü seçmenize yardımcı olmak için ağınız için uygundur.<br><br> ![Çözüm protokol seçimini gerektirir](media/log-analytics-network-performance-monitor/log-analytics-netmon-perf-welcome.png)<br><br>

4. Protokol seçtikten sonra yönlendirilirsiniz **OMS genel bakış** sayfası. Çözüm, ağ üzerinden verileri toplar, Ağ Performansı İzleyicisi genel bakış kutucuğu belirten iletisi görüntüler *veri toplama işlemi sürüyor*.<br><br> ![Çözüm verileri toplama](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-01.png)<br><br>
5. Veriler toplanır ve dizine, ek yapılandırma gerçekleştirmeniz gereken belirtmek için genel bakış kutucuğu değişiklikleri sonra.<br><br> ![Çözüm döşeme ek yapılandırma gerektirir](media/log-analytics-network-performance-monitor/log-analytics-netmon-tile-status-02.png)<br><br>
6. Kutucuğuna tıklayın ve aşağıdaki adımları kullanarak Çözüm yapılandırılırken başlatın.

### <a name="create-new-networks"></a>Yeni ağ oluşturma
Ağ Performans İzleyicisi'nde bir ağ, alt ağlar için mantıksal bir kapsayıcısıdır. Bir kolay adla bir ağ oluşturun ve alt ağlarını iş mantığınızı göre ekleyin. Örneğin, adlı bir ağ oluşturabilirsiniz *Londra* ve tüm alt ağlar, Londra datacenter veya adlı bir ağ eklemek *ContosoFrontEnd* ve tüm alt ağlar Contoso adlı, uygulamanızın ön uç hizmet veren ekleyin Bu ağa.
Yapılandırma sayfasında, gördüğünüz adlı bir ağ **varsayılan** ağlar sekmesinde. Tüm otomatik olarak bulunan alt ağlara oluşturmadıysanız, varsayılan ağ yerleştirilir.
Her bir ağ oluşturduğunuzda, bir alt ağ ekleyin ve bu alt ağ varsayılan ağdan kaldırılır. Bir ağ silerseniz, tüm alt varsayılan ağa otomatik olarak döndürülür.
Bu nedenle, varsayılan ağ herhangi bir kullanıcı tarafından tanımlanan ağ içinde yer almayan tüm alt ağlar için kapsayıcı görevi görür. Düzenleyemez veya varsayılan ağ silin. Her zaman, sistemde kalır. Ancak, gereksinim duyduğunuz kadar çok özel ağlar oluşturabilirsiniz.
Çoğu durumda, alt ağlar, kuruluşunuzda birden fazla ağ düzenlenir ve ağlarınızı iş mantığınızı göre gruplandırmak için bir veya daha fazla ağ oluşturmanız gerekir

#### <a name="to-create-a-new-network"></a>Yeni bir ağ oluşturmak için
1. Tıklatın **Ekle ağ** ve ağ adını ve açıklamasını yazın.
2. Bir veya daha fazla alt ağ seçin ve ardından **Ekle**.
3. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.<br><br> ![add network](./media/log-analytics-network-performance-monitor/npm-add-network.png)

### <a name="wait-for-data-aggregation"></a>Veri toplama bekle
Yapılandırmanın ilk kez kaydettikten sonra çözümü aracılarını yüklendiği düğümler arasında ağ paket kaybı ve gecikme bilgileri toplanıyor başlatır. Bu işlem, bazen zaman 30 dakika. Bu aşamasında, Ağ Performansı İzleyicisi döşemenin genel bakış sayfasında belirten bir ileti görüntüler *veri toplama işleminde*.

![veri toplama işlemi sürüyor](./media/log-analytics-network-performance-monitor/npm-aggregation.png)

Verileri karşıya, Ağ Performansı İzleyicisi kutucuğu verilerini gösteren güncelleştirilir.

![Ağ Performans İzleyicisi kutucuğu](./media/log-analytics-network-performance-monitor/npm-tile.png)

Ağ Performansı İzleyicisi Panoyu görebilmek için kutucuğa tıklayın.

![Ağ Performans İzleyicisi Panosu](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Alt ağlar için izleme ayarlarını Düzenle
En az bir Aracısı yüklendiği tüm alt listelendiğini **ağlarla** yapılandırma sayfası sekmesindedir.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Etkinleştirme veya belirli alt ağlar için izlemeyi devre dışı bırakma
1. Seçin veya kutunun işaretini **alt ağ kimliği** ve emin olun **kullanım izleme için** seçili veya temizlenmiş, uygun şekilde şeklindedir. Seçin veya birden çok alt ağı temizleyin. Diğer aracıları ping işlemi durdurmak için aracıları güncelleştirildikçe devre dışı bırakıldığında, alt ağlar izlenmeyen.
2. İzlemek için belirli bir alt ağ alt ağı listeden seçerek ve gerekli düğümleri izlenmeyen ve izlenen düğümleri içeren listeler arasında taşımak istediğiniz düğümü seçin.
   Özel bir ekleyebilirsiniz **açıklama** alt ağ için.
3. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.<br><br> ![alt ağ Düzenle](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>İzleme düğümü seçin
Bir aracısı yüklü olan tüm düğümleri listelenen **düğümleri** sekmesi.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Etkinleştirme veya düğümler için izlemeyi devre dışı bırakma
1. Seçin veya izlemek ya da izlemeyi durdurmak istediğiniz düğümleri temizleyin.
2. Tıklatın **kullanım izleme için**, veya, uygun şekilde temizleyin.
3. **Kaydet**’e tıklayın.<br><br> ![düğüm izlemeyi etkinleştir](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)

### <a name="set-monitoring-rules"></a>İzleme kurallarını ayarlama
Ağ Performansı İzleyicisi iki ağ arasında veya iki alt ağlar arasında ağ bağlantıları performans eşiği aşıldığında sistem durumu olayları oluşturur. Bu eşikler sistem tarafından otomatik olarak öğrenilebilecek veya özel eşikler sağlayabilir.

Sistem varsayılan kuralı otomatik olarak oluşturur. Kural kaybı veya herhangi bir ağ/alt ağ bağlantıları çifti arasındaki gecikme süresi sistem öğrenilen eşik breaches her bir sistem durumu olayı oluşturur. Bu, tüm izleme kurallarını açıkça oluşturmadınız kadar ağ altyapınızı izlemek çözüm yardımcı olur. Varsayılan kural etkinleştirilirse, tüm düğümlere yapay işlemler izleme için etkinleştirilmiş olan tüm diğer düğümlere gönderin. Varsayılan kural küçük ağlar için yararlıdır. Örneğin, bir mikro hizmet ve çalıştıran sunucular az sayıda sahip olduğu bir senaryoda tüm sunucular birbirleriyle bağlantı olduğundan emin olmak istersiniz.

>[!NOTE]
>Varsayılan kural devre dışı bırakın ve özellikle çok sayıda düğümü izleme için kullandığınız büyük ağlarda durumunda özel izleme kurallar oluşturmanız önerilir. Bu çözüm ve ağınızı izleme düzenlemenize yardım tarafından oluşturulan trafiğin azaltır.

İş mantığınızın göre özel izleme kurallarını oluşturun. Genel merkez iki office sitelere ağ bağlantısını performansını izlemek isterseniz, örneğin, ardından office site1 O1 ağındaki tüm alt ağlar, office site2 O2 ağındaki tüm alt ağlar ve headquarter H. ağındaki tüm alt ağlar gruplandırma İki kuralları-O1 ve H ve diğer O2 H. arasındaki arasında bir izleme oluşturun


#### <a name="to-create-custom-monitoring-rules"></a>Özel İzleme kuralları oluşturmak için
1. Tıklatın **Kuralı Ekle** içinde **İzleyici** sekmesinde ve kural ad ve açıklama girin.
2. Listelerden izlemek için ağ veya alt ağ bağlantıları çifti seçin.
3. İlk ilgi ilk alt ağ/s yer alan ağ ağ aşağı açılır listeden seçtikten sonra alt ağ/s karşılık gelen alt ağ aşağı açılır listeden seçin.
   Seçin **tüm alt ağlar** bir ağ bağlantısı tüm alt ağlar izlemek istiyorsanız. Benzer şekilde, diğer alt ağ/s ilgi seçin. Ve tıklayabilirsiniz **özel durum ekleyin** yaptığınız seçimden belirli alt ağ bağlantıları için izleme dışlanacak.
4. [ICMP ve TCP protokollerini arasında seçin](#choose-the-right-protocol-icmp-or-tcp) yapay işlemleri yürütmek.
5. Seçtiğiniz, ardından öğeleri için sistem durumu olayları Temizle oluşturmak istemiyorsanız, **bu kural tarafından kapsanan bağlantılarında sistem durumu izlemeyi etkinleştir**.
6. İzleme koşulları seçin.
   Eşik değerleri yazarak, sistem olay oluşturma için özel eşikler ayarlayabilirsiniz. Seçilen ağ/alt ağ çifti için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur.
7. tıklatın **kaydetmek** yapılandırmayı kaydetmek için.<br><br> ![Özel İzleme kuralı oluşturma](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)

Bir izleme kuralı kaydettikten sonra bu kural uyarı yönetimi ile tıklayarak tümleştirebilirsiniz **oluşturma uyarı**. Bir uyarı kuralı arama sorgusu ve diğer gerekli ile otomatik olarak oluşturulan otomatik olarak doldurulmuş parametreler. Bir uyarı kuralı kullanarak NPM içinde var olan uyarılar ek olarak e-posta tabanlı uyarılar alabilir. Uyarılar ayrıca eylemlerden runbook'larla tetikleyebilir veya Web kancalarını kullanarak mevcut hizmet yönetimi çözümleriyle tümleştirilebilir. Tıklayabilirsiniz **yönetmek uyarı** uyarı ayarlarını düzenlemek için.

### <a name="choose-the-right-protocol-icmp-or-tcp"></a>Sağ Protokolü ICMP veya TCP seçin

Ağ Performans İzleyicisi'ni (NPM) yapay işlemler paket kaybı ve bağlantı gecikme süresi gibi ağ performans ölçümlerini hesaplamak için kullanır. Bu daha iyi anlamak için bağlı bir NPM aracı göz önünde bulundurun. bir ağ bağlantısı ucunu. Bu NPM aracı başka bir ağ sonuna bağlı ikinci bir NPM aracı araştırma paketleri gönderir. İkinci aracı yanıt paketleri ile yanıtlar. Bu işlem birkaç kez yinelenir. Yanıtlar ve her yanıt almak için geçen süre sayısını ölçme, birinci NPM aracı bağlantı gecikme değerlendirir ve paket bırakma.

Biçim, boyut ve bu paketlerin dizisini izleme kurallarını oluştururken seçtiğiniz protokolü tarafından belirlenir. Paketlerin protokolünü temel, ara ağ aygıtlarını (yönlendiriciler, anahtarlar vb.) bu paketleri farklı işlem. Sonuç olarak, Protokolü tercih ettiğiniz sonuçları doğruluğunu etkiler. Ve protokolü tercih ettiğiniz de NPM çözümü dağıttıktan sonra herhangi bir el ile adımlar atmanız gerekir olup olmadığını belirler.

NPM yapay işlemleri yürütmek ICMP ve TCP protokollerini arasında seçim sunar.
Yapay işlem kuralı oluşturduğunuzda, ICMP seçerseniz, NPM aracıları ağ gecikme süresi ve paket kaybı hesaplamak için ICMP YANKI iletisi kullanın. ICMP YANKI geleneksel Ping yardımcı programı tarafından gönderilen aynı iletiyi kullanır. Protokol olarak TCP'yi kullandığınızda NPM aracıları ağ üzerinden TCP Eşitlemeye paket gönderin. Bu bir TCP anlaşması tarafından izlenir tamamlama ve RST paketlerini kullanarak bağlantı kaldırılıyor.

#### <a name="points-to-consider-before-choosing-the-protocol"></a>Protokol seçmeden önce dikkate alınacak noktalar
Kullanılacak protokol seçmeden önce aşağıdaki bilgileri göz önünde bulundurun:

##### <a name="discovering-multiple-network-routes"></a>Birden çok ağ yollarını keşfetme
Birden çok yol keşfederken TCP daha doğru olduğunu ve her alt ağda daha az aracıları gerekiyor. Örneğin, TCP kullanarak bir veya iki aracılarını alt ağlar arasındaki tüm yedekli yollar bulabilir. Ancak, benzer sonuçlar elde etmek için ICMP kullanarak birkaç aracılarını gerekir. ICMP, varsa kullanarak *N* 5'ten fazla ihtiyacınız iki alt ağ arasındaki yolları sayısı*N* kaynak veya hedef alt ağdaki aracıları.

##### <a name="accuracy-of-results"></a>Sonuçları doğruluğunu
Yönlendiriciler ve anahtarlar ICMP YANKI paketleri TCP paketleri karşılaştırıldığında daha düşük öncelikli atamak eğilimindedir. Ağ aygıtlarını yoğun olarak yüklendiğinde, belirli durumlarda daha yakından TCP tarafından alınan veri kaybı ve gecikme uygulamalar tarafından yaşadı yansıtır. Bu uygulama trafiğini çoğunu akışlar için TCP üzerinden oluşur. Böyle durumlarda, ICMP TCP'ye kıyasla daha az doğru sonuçlar sağlar.

##### <a name="firewall-configuration"></a>Güvenlik duvarı yapılandırması
TCP protokolü, bir hedef bağlantı noktasına gönderilen TCP paket gerektirir. Aracıları yapılandırdığınızda bu değiştirebilirsiniz ancak NPM aracıları tarafından kullanılan varsayılan bağlantı noktası 8084, ' dir. Bu nedenle, ağ güvenlik duvarları veya NSG kuralları (azure'da) bağlantı noktası üzerinde trafiğe izin emin olmak için gerekir. Aracıları yüklü bilgisayarda yerel güvenlik duvarı Bu bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırıldığından emin olmanız gerekir.

Ağ güvenlik duvarı el ile yapılandırmanız gerekiyor ancak Windows çalıştıran bilgisayarlarınızda güvenlik duvarı kurallarını yapılandırmak için PowerShell komut dosyalarını kullanabilirsiniz.

Buna karşılık, ICMP bağlantı noktası kullanılarak çalışmaz. Çoğu Kurumsal senaryoda, ICMP trafiği güvenlik duvarları üzerinden Ping yardımcı programı gibi ağ tanılama araçları kullanmanıza izin verilir. Başka bir makineden Ping, bu nedenle, daha sonra ICMP Protokolü güvenlik duvarları el ile yapılandırmak zorunda kalmadan kullanabilirsiniz.

> [!NOTE]
> Bazı güvenlik duvarları çok fazla sayıda olayı, güvenlik, bilgi ve olay yönetim sisteminde kaynaklanan aktarım açabilir ICMP engelleyebilir. Seçtiğiniz Protokolü engellenmemiş olduğundan emin olun ağ güvenlik duvarı/NSG, tarafından aksi NPM ağ kesimine izlemek mümkün olmaz.  Bu nedenle, izleme için TCP kullanan önerilir.
> Burada, TCP, gibi ne zaman kullanmanız mümkün olmayan senaryolarda ICMP kullanmanız gerekir:
> * TCP ham yuva Windows istemcisinde izin verilmiyor beri Windows istemci tabanlı düğümleri, kullanmakta olduğunuz
> * Ağ güvenlik duvarı/NSG TCP engeller


#### <a name="how-to-switch-the-protocol"></a>Protokol geçiş yapma

Dağıtım sırasında ICMP kullanmayı seçerseniz, varsayılan kuralı izleme düzenleyerek TCP için herhangi bir zamanda geçiş yapabilirsiniz.

##### <a name="to-edit-the-default-monitoring-rule"></a>Varsayılan kural izleme düzenlemek için
1.  Gidin **ağ performansı** > **İzleyici** > **yapılandırma** > **İzleyici** ve ardından **varsayılan kuralı**.
2.  Kaydırma **Protokolü** bölümünde ve kullanmak istediğiniz protokolü seçin.
3.  Tıklatın **kaydetmek** ayarı uygulamak için.

Varsayılan kural belirli bir protokol kullanıyor olsa bile, farklı bir protokol yeni kurallar oluşturabilirsiniz. Burada bazı kuralların ICMP kullanır ve başka bir TCP kullandığı kuralları bir karışımını da oluşturabilirsiniz.


## <a name="data-collection-details"></a>Veri toplama ayrıntıları
ICMP kaybı ve gecikme bilgileri toplamak için protokol olarak seçildiğinde TCP seçildiğinde TCP Eşitlemeye SYNACK ACK el sıkışma paketleri ve ICMP YANKI YANITI ağ Performans İzleyicisi'ni kullanır. İzleme yolu topoloji bilgilerini almak için de kullanılır.

Aşağıdaki tabloda, veri toplama yöntemleri ve Ağ Performansı İzleyicisi için verileri nasıl toplanır ilgili diğer ayrıntıları gösterir.

| Platform | Doğrudan Aracısı | SCOM Aracısı | Azure Storage | SCOM gerekli? | Yönetim grubu gönderilen SCOM Aracısı verileri | Toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Gönderilen TCP el sıkışmaları/ICMP YANKI iletileri 5 saniyede veri 3 dakikada bir |

Çözüm yapay işlemler ağ durumunu değerlendirmek için kullanır. Çeşitli yerlerinde ağ exchange TCP paketleri veya ICMP Yankı (izleme için seçilen Protokolü) bağlı olarak birbirleriyle yüklü OMS aracılar. İşlem sırasında aracıları varsa gidiş dönüş süresi ve paket kaybı öğrenin. Düzenli olarak, her bir aracının bir izleme yolu tüm çeşitli yollar test edilmelidir ağda bulmak için diğer aracılara gerçekleştirir. Bu verileri kullanarak, aracıları ağ gecikme süresi ve paket kaybı rakamları türetme. Testler, her beş saniyede ve verileri toplanır üç dakika boyunca aracıları tarafından için günlük analizi hizmeti yüklemeden önce yinelenir.

> [!NOTE]
> Aracıları sık birbirleriyle iletişim olsa da, bunlar çok miktarda ağ trafiği testleri yürütürken oluşturmaz. Aracıları kaybı ve gecikme--paketler arasında alınıp verilen veri belirlemek için yalnızca TCP Eşitlemeye SYNACK ACK el sıkışma paketlerde kullanır. Bu işlem sırasında aracılar birbiriyle yalnızca gerekli olduğunda iletişim ve aracı iletişim topolojisinin ağ trafiğini azaltmak için optimize edilmiştir.
>
>

## <a name="using-the-solution"></a>Çözümü kullanma
Bu bölümde, tüm Pano açıklanmaktadır işlevleri ve nasıl kullanıldıklarını.

### <a name="solution-overview-tile"></a>Çözüm genel bakış kutucuğu
Ağ Performansı İzleyicisi çözüm etkinleştirdikten sonra OMS genel bakış sayfasında çözüm kutucuğu ağ durumunu hızlı bir genel bakış sağlar. Sağlıklı ve sağlıksız alt ağ bağlantıları sayısını gösteren bir halka grafiği görüntüler. Kutucuğa tıkladığınızda, çözüm panosu açılır.

![Ağ Performans İzleyicisi kutucuğu](./media/log-analytics-network-performance-monitor/npm-tile.png)

### <a name="network-performance-monitor-solution-dashboard"></a>Ağ Performans İzleyicisi çözüm Panosu
**Ağ Özet** alan ağları göreli boyutlarına birlikte özetini gösterir. Bu, ağ bağlantıları, alt ağ bağlantıları ve yolları toplam sayısı (bir yolu aracıları ile iki ana ve aralarındaki tüm atlama IP adreslerini oluşur) sisteminde gösteren döşeme tarafından izlenir.

**Üst ağ sistem durumu olayları** alan listesini sağlar en son sistem durumu olayları ve Uyarıları sistem ve saat olay etkin edildiğinden. Paket kaybı veya bir ağ veya alt ağ bağlantısının gecikme eşiği aştığında bir sistem durumu olayı ya da uyarı oluşturulur.

**Üst sağlıksız ağ bağlantıları** alanı sağlıksız ağ bağlantıları listesini gösterir. Bir veya daha fazla olumsuz sistem durumu olayları kendileri için şu anda sahip ağ bağlantıları bunlar.

**Üst alt ağ bağlantıları en kaybı ile** ve **en gecikme süresi ile alt ağ bağlantıları** alanları paket kaybı tarafından üst alt ağ bağlantıları göstermek ve alt ağ bağlantıları gecikmesine sırasıyla üst. Belirli ağ bağlantıları bekleniyordu yüksek gecikme veya miktar paket kaybı. Bu gibi bağlantılar üst on listelerinde görünür ancak sağlıksız olarak işaretlenmez.

**Genel sorgular** alanı ham ağ verileri doğrudan izleme fetch arama sorguları kümesi içerir. Özelleştirilmiş raporlama için kendi sorguları oluşturmak için bu sorguları bir başlangıç noktası olarak kullanabilirsiniz.

![Ağ Performans İzleyicisi Panosu](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="drill-down-for-depth"></a>derinliği için detaya gitme
İncelemek için çözüm panosunda çeşitli bağlantıları tıklatabilirsiniz ilgilendiğiniz herhangi bir alan içine daha derin aşağı. Örneğin, bir uyarı veya Panoda görünmesini bir sağlıksız ağ bağlantısı gördüğünüzde, daha fazla araştırmak için tıklatabilirsiniz. Belirli bir ağ bağlantısı için tüm alt ağ bağlantıları listeleyen bir sayfaya yönlendirilirsiniz. Her alt ağ bağlantı kaybı, gecikme ve sistem durumunu görebilmek için ve hangi alt ağ bağlantıları bulma sorunu neden hızlı bir şekilde. Daha sonra tıklatabilirsiniz **görüntülemek düğüm bağlantıları** sağlıksız alt ağ bağlantısı için tüm düğüm bağlantıları görmek için. Ardından, tek tek düğümü düğümü bağlantılara bakın ve sağlıksız düğüm bağlantıları bulabilirsiniz.

Tıklayabilirsiniz **görünüm topoloji** kaynak ve hedef düğümleri arasında yolların atlama atlamalı topolojisini görüntülemek için. Hızlı ağ belirli bir kısmını sorunu belirleyebilir sağlıksız yolları veya atlama kırmızı olarak gösterilir.

![Veri ayrıntıya](./media/log-analytics-network-performance-monitor/npm-drill.png)

### <a name="network-state-recorder"></a>Ağ durumu Kaydedicisi

Her görünüm, ağ durumu görüntüsünü belirli bir noktada zamanında görüntüler. Varsayılan olarak, en son durum gösterilir. Sayfanın üstündeki çubuğu noktası durumu görüntülenmektedir zamanında gösterir. Zamanında geri dönün ve çubuğunda tıklayarak, ağ durumunu görüntülemek seçebileceğiniz **Eylemler**. Etkinleştirmek veya en son durumunu görüntülerken herhangi bir sayfa için Otomatik yenilemeyi devre dışı bırakmak seçebilirsiniz.

![ağ durumu](./media/log-analytics-network-performance-monitor/network-state.png)

#### <a name="trend-charts"></a>Eğilim grafikleri
Ayrıntıya her düzeyde kaybı ve gecikme süresi bir ağ bağlantısının eğilimi görebilirsiniz. Eğilim grafikleri de alt ağ ve düğüm bağlantıları için kullanılabilir. Grafik üstünde zamanı denetimi kullanarak çizmek için grafiği için zaman aralığını değiştirebilirsiniz.

Eğilim grafikleri, geçmiş bir perspektif bir ağ bağlantısı performansını gösterir. Bazı ağ sorunları doğası gereği geçici ve yalnızca geçerli durumunu ağ bakarak catch zor olabilir. Sorunları hızla yüzey ve herkes, yalnızca daha sonraki bir noktada zamanında yeniden bildirimler önce kayboluyor olmasıdır. Bu sorunlar nedeniyle genellikle yüzeyini uygulama yanıt süresini, tüm uygulama bileşenleri sorunsuzca çalıştırması göründüğünde bile açıklanamayan artışlar olarak tür geçici sorunlar Ayrıca uygulama yöneticileri için zor olabilir.

Burada sorunu ani bir depo ağ gecikmesi veya paket kaybı görünür bir eğilim Grafiği bakarak bu tür sorunları kolayca algılayabilir.

![Eğilim grafiği](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>atlama atlamalı topoloji Haritası
Ağ Performansı İzleyicisi etkileşimli topoloji Haritası üzerinde iki düğüm arasında rotaları atlama atlamalı topolojisini gösterir. Bir düğüm bağlantısını seçerek ve ardından topoloji Haritası görüntüleyebilirsiniz **görünüm topoloji**. Ayrıca, topoloji Haritası tıklatarak görüntüleyebileceğiniz **yolları** döşeme Panoda. Tıkladığınızda **yolları** sol panelden kaynak ve hedef düğümleri seçin ve ardından zorunda Panoda **çizim** iki düğüm arasında rotaları çizmek için.

İki düğüm arasında ne kaç yollar, topoloji Haritası görüntüler yolları veri paketlerinin alın. Ağ performans sorunları topoloji harita üzerinde kırmızı işaretlenir. Hatalı ağ bağlantısı veya hatalı ağ aygıtı topoloji haritasında kırmızı renkli öğeler arayarak bulabilirsiniz.

Bir düğüm veya vurgulu topoloji haritasında tıklattığınızda, FQDN ve IP adresi gibi düğümünün özelliklerine bakın. IP adresini görmek için bir atlama'ı tıklatın. Belirli yollar daraltılabilir Eylem Bölmesi'nde filtreleri kullanarak filtreleme seçebilirsiniz. Ve eylem bölmesinde kaydırıcıyı kullanarak ara atlama gizleme tarafından ağ topolojileri basitleştirebilirsiniz. Yakınlaştırma veya için fare tekerleği kullanarak topoloji haritasını out.

Haritada gösterilen topolojisi Katman 3 topolojisi ve Katman 2 aygıtlarını ve bağlantıları içermiyor unutmayın.

![atlama atlamalı topoloji Haritası](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Hataya yerelleştirme
Ağ Performansı İzleyicisi ağ cihazlarına bağlanmadan ağ performans sorunlarını bulamıyor. Ağ ve ağ grafikte gelişmiş algoritmalar uygulayarak toplayan verileri bağlı olarak, Ağ Performansı İzleyicisi sorunun kaynağını olasılığı en yüksek ağ bölümlerinin probabilistic tahmin yapar.

Bu yaklaşım, yönlendiriciler veya anahtarlar gibi ağ aygıtlarını toplanması herhangi bir veri gerektirmediğinden atlama erişimi kullanılamadığında ağ performans sorunlarını belirlemek kullanışlıdır. Bu da iki düğüm arasında atlamanın yönetimsel denetiminde olmadığında yararlıdır. Örneğin, ISS yönlendirici atlamaları olabilir.

### <a name="log-analytics-search"></a>Günlük analizi arama
Grafik sayfaları detaya gitme ve Ağ Performansı İzleyicisi Pano aracılığıyla sunulan tüm verileri de yerel olarak günlük analizi aramada mevcut değil. Arama sorgu dili kullanarak verileri sorgulamak ve Excel veya Power BI için verileri dışa aktararak özel raporlar oluşturun. **Genel sorgular** panosunda alana sahip kendi sorgular ve raporlar oluşturmak için başlangıç noktası olarak kullanabileceğiniz bazı yararlı sorgular.

![arama sorguları](./media/log-analytics-network-performance-monitor/npm-queries.png)

## <a name="investigate-the-root-cause-of-a-health-alert"></a>Bir sistem durumu uyarısı kök nedeni araştırın
Ağ Performansı İzleyicisi hakkında okuduğunuza göre bir sistem durumu olayı kök nedeni basit araştırmasını bakalım.

1. Genel bakış sayfasında, ağınızın durumunu hızlı bir anlık görüntü izlenerek elde **Ağ Performansı İzleyicisi** döşeme. İzlenmekte olan 6 alt ağ bağlantıları dışında 2 sağlıksız olduğuna dikkat edin. Bu araştırma gerektirir. Çözüm panosunu görüntülemek için kutucuğa tıklayın.<br><br> ![Ağ Performans İzleyicisi kutucuğu](./media/log-analytics-network-performance-monitor/npm-investigation01.png)  
2. Aşağıdaki görüntüde, düzgün çalışmayan bir ağ bağlantısı bir sistem durumu olayı olduğuna dikkat edin. Sorunu araştırmanıza ve tıklayın karar **DMZ2 DMZ1** sorun kökündeki bulmak için ağ bağlantısı.<br><br> ![sağlıksız ağ bağlantısı örneği](./media/log-analytics-network-performance-monitor/npm-investigation02.png)  
3. Sayfa detaya içindeki tüm alt ağ bağlantıları gösterir **DMZ2 DMZ1** ağ bağlantısı. Her iki alt ağ bağlantıları için ağ bağlantısı bozan eşik gecikmesi taşını dikkat edin. Her iki alt ağ bağlantıları gecikme eğilimleri de görebilirsiniz. Saat seçimini kullanabilirsiniz gerekli zaman aralığına odaklanmak için grafiği denetiminde. Gecikme süresi, yoğun zaman ulaştı günün saatini görebilirsiniz. Sorunu araştırmaya bu zaman aralığı için günlükleri daha sonra arama yapabilirsiniz. Tıklatın **görüntülemek düğüm bağlantıları** daha fazla ayrıntıya için.<br><br> ![sağlıksız alt ağ bağlantıları örneği](./media/log-analytics-network-performance-monitor/npm-investigation03.png)
4. Önceki sayfaya benzer, sayfa detaya belirli alt ağ bağlantısı için kendi bağlı düğüm bağlantıları listeler. Benzer eylemler gerçekleştirebilir önceki adımda yaptığınız gibi burada. Tıklatın **görünüm topoloji** 2 düğümler arasında topolojisini görüntülemek için.<br><br> ![sağlıksız düğüm bağlantıları örneği](./media/log-analytics-network-performance-monitor/npm-investigation04.png)  
5. 2 seçili düğümler arasındaki tüm yolları topoloji haritasında çizilir. Topoloji harita üzerinde iki düğüm arasında yolların atlama atlamalı topoloji görselleştirebilirsiniz. Kaç yollar iki düğüm arasında ne mevcut NET bir resim sunan veri paketlerinin yapmakta yolları. Ağ performans sorunları kırmızı renk işaretlenir. Hatalı ağ bağlantısı veya hatalı ağ aygıtı topoloji haritasında kırmızı renkli öğeler arayarak bulabilirsiniz.<br><br> ![sağlıksız topoloji görünümü örneği](./media/log-analytics-network-performance-monitor/npm-investigation05.png)  
6. İçinde kaybı, gecikme ve her yolundaki durak sayısını incelenebilecek **eylemi** bölmesi. Scrollbar sağlıksız bu yollar ayrıntılarını görüntülemek için kullanın.  Yalnızca seçilen yollar için topoloji çizilen sağlıksız atlama ile yolları seçmek için filtreleri kullanın. İçinde veya dışında topoloji Haritası yakınlaştırma için fare tekerleğinin kullanabilirsiniz.

   İçindeki görüntü açıkça ağ belirli bölümüne sorunlu alanları kök nedenini yolları ve kırmızı renk atlama bakarak görebilirsiniz. Bir topoloji Haritası düğümünde tıklayarak FQDN dahil olmak üzere düğümü özelliklerini gösterir ve IP adresi. Atlama tıklatarak atlamanın IP adresi gösterir.<br><br> ![sağlıksız topolojisi - yolu örnek ayrıntıları](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="provide-feedback"></a>Geri bildirimde bulunma

- **UserVoice** -fikirlerinizi bize üzerinde çalışmak istediğiniz ağ performansı İzleyicisi özellikleri için nakledebilirsiniz. Ziyaret bizim [UserVoice sayfa](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring).
- **Bizim kohort katılma** -her zaman yeni müşteriler bizim kohort katılma elde etmeyle ilgilenen çalışıyoruz. Parçası olarak bu, yeni özellikler erken erişim edinmek ve ağ Performans İzleyicisi'ni iyileştirmemize yardımcı olun. Bağlama düşünüyorsanız dolgu bu genişletme [hızlı anket](https://aka.ms/npmcohort).

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
