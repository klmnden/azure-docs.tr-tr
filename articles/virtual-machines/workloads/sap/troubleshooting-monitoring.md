---
title: "Sorun giderme ve Azure (büyük örnekler) üzerinde SAP HANA izleme | Microsoft Docs"
description: "Sorun giderme ve Azure (büyük örnekler) üzerinde SAP HANA izleyin."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5583f3d1949614dbba4d2f91d72e4ac6b4d03d1c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-troubleshoot-and-monitor-sap-hana-large-instances-on-azure"></a>Sorun giderme ve Azure üzerinde SAP HANA (büyük örnekler) izlemek nasıl


## <a name="monitoring-in-sap-hana-on-azure-large-instances"></a>SAP HANA (büyük örnekler) azure'da izleme

SAP HANA Azure (büyük örnekler) üzerinde diğer bir Iaas dağıtımından farklı — işletim sistemi ve uygulama yaptıklarını ve bunları nasıl bunlar aşağıdaki kaynaklara izlemeniz gerekir:

- CPU
- Bellek
- Ağ bant genişliği
- Disk alanı

Azure sanal makinelerle, yukarıda adı kaynak sınıfları yeterli olup, veya olup bunlar tükendiğinde dağıtamadığını bulmak gerektiği. İşte daha fazla ayrıntı her farklı sınıflar:

**CPU kaynak tüketimi:** SAP HANA karşı belirli iş yükü için tanımlanan oranı bellekte depolanan verilerde çalışmak kullanılabilir yeterli CPU kaynaklarını olmalıdır emin olmak için uygulanır. Bununla birlikte, burada HANA CPU yürütülen sorguları eksik dizinler veya benzer sorunlar nedeniyle çok tüketen durumlar olabilir. Başka bir deyişle HANA büyük örneği birim yanı sıra belirli HANA Hizmetleri tarafından tüketilen CPU kaynaklarının CPU kaynak tüketimini izlemeniz gerekir.

**Bellek tüketimi:** gelen biriminde HANA içinde yanı sıra HANA dışında izlemek önemlidir. HANA içinde verileri SAP gerekli boyutlandırma yönergeleri içinde kalmak için bellek tahsis HANA nasıl tüketme izleyin. Ayrıca, ek yüklü olmayan-HANA yazılım değil çok fazla bellek kullanmasına ve bu nedenle için bellek HANA ile rekabet emin olmak için büyük örnek düzeyinde bellek tüketimi izlemek istersiniz.

**Ağ bant genişliği:** Azure sanal ağ geçidi Azure ne kadar yakın Azure ağ geçidi SKU'su seçtiğiniz sınırları için olduğunuzu bulmak için bir sanal ağ içindeki tüm Azure VM'ler tarafından alınan veri izlemek yardımcı olacak şekilde Vnet'in taşınmasını verilerin bant genişliği sınırlandırılmıştır. HANA büyük örneği biriminde algılama da gelen ve giden ağ trafiğini izlemek ve zaman içinde işlenir birimleri izlemek için yapar.

**Disk alanı:** Disk alanı tüketimini genellikle zamanla artar. Bunun pek çok nedeni vardır, ancak tüm çoğu: veri birimi artırır, işlem günlüğü yedeklemeleri, izleme dosyaları depolamak ve depolama anlık görüntüleri gerçekleştirme yürütülmesini. Bu nedenle, disk alanı kullanımı izlemek ve HANA büyük örneği birimiyle ilişkili disk alanını yönetmek önemlidir.

İçin **türü II SKU'ları** HANA büyük örneklerini sunucu önceden sistem tanılama araçları ile birlikte gelir. Sistem durumu denetimi gerçekleştirmek için bu tanılama araçlarını kullanabilir. /Var/log/health_check sistem durumu denetimi günlük dosyasını oluşturmak için aşağıdaki komutu çalıştırın.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Bir sorunu gidermek için Microsoft Support ekibi ile çalışırken, siz de bu Tanılama aracını kullanarak günlük dosyalarını sağlamak için istenebilir. Aşağıdaki komutu kullanarak dosya zip.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```


## <a name="monitoring-and-troubleshooting-from-hana-side"></a>İzleme ve HANA taraftan sorun giderme

Etkili bir şekilde Azure (büyük örnekler) üzerinde SAP HANA ilgili sorunları çözümlemek için bir sorunun kök nedenini daraltmak kullanışlıdır. SAP yardımcı olmak için belgeler, büyük bir miktarını yayımladı.

SAP HANA performansı ile ilgili ilgili sık sorulan sorular aşağıdaki SAP notları bulunabilir:

- [SAP Not #2222200 – SSS: SAP HANA ağ](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP Not #2100040 – SSS: SAP HANA CPU](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP Not #199997 – SSS: SAP HANA bellek](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP Not #200000 – SSS: SAP HANA performansı iyileştirme](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP Not #199930 – SSS: SAP HANA g/ç çözümleme](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP Not #2177064 – SSS: SAP HANA hizmeti yeniden başlatın ve kilitleniyor](https://launchpad.support.sap.com/#/notes/2177064)

**SAP HANA uyarıları**

İlk adım olarak, geçerli SAP HANA uyarı günlüklerini denetleyin. SAP HANA Studio'da Git **Yönetim Konsolu: Uyarı: göster: tüm uyarıları**. Bu sekme kümesi minimum ve maksimum eşikleri dışında kalan belirli değerleri (boş fiziksel bellek, CPU kullanımı, vb.) için tüm SAP HANA uyarıları gösterir. Varsayılan olarak, denetimleri 15 dakikada bir otomatik olarak yenilenir.

![SAP HANA Studio'da yönetim konsoluna gidin: Uyarı: göster: tüm uyarılar](./media/troubleshooting-monitoring/image1-show-alerts.png)

**CPU**

Hatalı eşik ayarı nedeniyle tetiklenen bir uyarı için bir çözüm varsayılan değer ya da daha uygun bir eşik değere sıfırlamaktır.

![Varsayılan değer veya daha uygun bir eşik değere sıfırlayın](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

Aşağıdaki uyarılar CPU kaynağı sorunlarını gösterebilir:

- Ana bilgisayar CPU kullanımı (uyarı 5)
- En son noktası işlemi (uyarı 28)
- Kayıt noktası süresi (uyarı 54)

SAP HANA veritabanından aşağıdakilerden biri üzerinde yüksek CPU tüketimi karşılaşabilirsiniz:

- Uyarı 5 (ana bilgisayar CPU kullanımı) için geçerli veya son CPU kullanımını tetiklenir
- Genel Bakış ekranda görüntülenen CPU kullanımı

![CPU kullanımı genel bakış ekranda görüntülenen](./media/troubleshooting-monitoring/image3-cpu-usage.png)

Yük grafiği yüksek CPU tüketimi veya yüksek tüketim geçmişte gösterebilir:

![Yük grafik yüksek CPU tüketimi veya yüksek tüketim geçmişte gösterebilir.](./media/troubleshooting-monitoring/image4-load-graph.png)

Yüksek CPU kullanımı nedeniyle tetiklenen uyarı, ancak bunlarla sınırlı olmamak de dahil olmak üzere çeşitli nedenlerden kaynaklanabilir: belirli işlemleri, veri yükleme, asılı SQL deyimlerini ve hatalı sorgu performansı (örneğin, ile BW HANA küpleri üzerinde) uzun süre çalışan işlerin yürütme.

Başvurmak [SAP HANA sorunlarını giderme: CPU ilgili neden olur ve çözümleri](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

**İşletim Sistemi**

SAP HANA Linux üzerinde en önemli denetler biri saydam büyük sayfalar devre dışı emin olun, bkz: [SAP Not #2131662 – saydam büyük sayfalar (THP) SAP HANA sunucularda](https://launchpad.support.sap.com/#/notes/2131662).

- Saydam büyük sayfalar aşağıdaki Linux komutu aracılığıyla etkin olup olmadığını denetleyebilirsiniz: **kat /sys/kernel/mm/transparent\_hugepage ve etkin**
- Varsa _her zaman_ arasına köşeli aşağıdaki şekilde, saydam büyük sayfalar etkinleştirildiğini gösterir: [her zaman] madvise hiçbir zaman if _hiçbir zaman_ içine aşağıdaki şekilde köşeli parantez içine saydam büyük sayfalar devre dışı olduğunu gösterir: her zaman madvise [hiçbir zaman]

Aşağıdaki Linux komutu hiçbir şey döndürmesi gerekir: **rpm - qa | grep ulimit.** Görünürse _ulimit_ olan yüklü, bunu hemen kaldırın.

**Bellek**

SAP HANA veritabanı tarafından ayrılan bellek miktarını beklenenden daha yüksek olduğunu gözlemleyebilirsiniz. Aşağıdaki uyarılar yüksek bellek kullanımı ile ilgili sorunları belirtir:

- Konak fiziksel bellek kullanımı (uyarı 1)
- Ad sunucusu (uyarı 12) bellek kullanımı
- Sütun deposu tabloları (uyarı 40) toplam bellek kullanımı
- Bellek kullanımı (uyarı 43) hizmetleri hakkında
- Sütun deposu tabloların (uyarı 45) ana depolama bellek kullanımı
- Çalışma zamanı döküm dosyalarını (uyarı 46)

Başvurmak [SAP HANA sorunlarını giderme: bellek sorunları](http://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

**Ağ**

Başvurmak [SAP Not #2081065 – SAP HANA ağ sorunlarını giderme](https://launchpad.support.sap.com/#/notes/2081065) ve sorun giderme adımları bu SAP notta ağ gerçekleştirin.

1. İstemci ve sunucu arasındaki gidiş dönüş süresi çözümleniyor.
  A. SQL betiği çalıştırma [ _HANA\_ağ\_istemcileri_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Düğümler arası iletişim analiz edin.
  A. SQL betiği çalıştırma [ _HANA\_ağ\_Hizmetleri_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Linux komutu çalıştırmak **ifconfig** (çıkış paket kayıpları yaşanan gösterir).
4. Linux komutu çalıştırmak **tcpdump**.

Ayrıca, açık kaynak kullanın [IPERF](https://iperf.fr/) Aracı (veya benzeri) gerçek uygulama ağ performansını ölçmek için.

Başvurmak [SAP HANA sorunlarını giderme: ağ performansı ve bağlantı sorunları](http://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

**Depolama**

Bir son kullanıcı açısından bir uygulama (veya bir bütün olarak system) ağır şekilde çalışan, yanıt vermiyor veya g/ç performansı ile ilgili sorun varsa kilitlenmesine bile görünebilir. İçinde **birimleri** sekmesini SAP HANA Studio'da bağlı birimlerin ve hangi birimlerin her hizmeti tarafından kullanılan görebilirsiniz.

![SAP HANA Studio'da birimleri sekmesinde bağlı birimlerin ve hangi birimlerin her hizmeti tarafından kullanılan görebilirsiniz](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Birimlerin bağlı ekranın alt kısmında, birimler, dosyalar ve g/ç istatistikleri gibi ayrıntılarını görebilirsiniz.

![Birimlerin bağlı ekranın alt kısmında, birimler, dosyalar ve g/ç istatistikleri gibi ayrıntılarını görebilirsiniz](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Başvurmak [SAP HANA sorunlarını giderme: g/ç ana ilgili nedenler ve çözümler](http://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) ve [SAP HANA sorunlarını giderme: Disk ana ilgili nedenler ve çözümler](http://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

**Tanılama araçları**

SAP HANA sistem durumu denetimi HANA aracılığıyla gerçekleştirmek\_yapılandırma\_Minichecks. Bu aracı zaten uyarıları SAP HANA Studio'da olarak oluşturuldu kritik olabilecek teknik sorunlar döndürür.

Başvurmak [SAP Not #1969700 – SAP HANA için SQL deyimi koleksiyonu](https://launchpad.support.sap.com/#/notes/1969700) ve o Not bağlı SQL Statements.zip dosyasını indirin. Bu .zip dosyasını yerel sabit sürücüde depolayın.

SAP HANA Studio'da üzerinde **sistem bilgileri** sekmesinde, sağ tıklatın, **adı** sütun ve select **içeri aktarma SQL deyimlerini**.

![Sistem bilgileri sekmesinde, SAP HANA Studio'da Ad sütununda sağ tıklatın ve içeri aktarma SQL deyimlerini seçin](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Yerel olarak depolanan SQL Statements.zip dosyasını seçin ve karşılık gelen SQL deyimlerini sahip bir klasör için içeri aktarılacak. Bu noktada, birçok farklı tanılama denetimleri bu SQL deyimleri ile çalıştırılabilir.

Örneğin, SAP HANA sistem çoğaltma bant genişliği gereksinimlerini sınamak için sağ **bant genişliği** deyiminin altında **çoğaltma: bant genişliği** seçip **açık** SQL konsolunda.

Tam SQL deyimini giriş parametreleri (değiştirilebilir ve sonra yürütülür için değişiklik bölümü) izin vererek açar.

![Giriş parametreleri (değiştirilebilir ve sonra yürütülür için değişiklik bölümü) izin vererek tam SQL deyimini açar](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Başka bir örnek altında deyimleri üzerinde sağ **çoğaltma: genel bakış**. Seçin **yürütme** ve bağlam menüsünden:

![Başka bir çoğaltma altında deyimleri üzerinde sağ örnektir: genel bakış. Execute bağlam menüsünden seçin](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Bu sorun giderme konusunda yardımcı olacak bilgileri sonuçlanır:

![Bu sorun giderme konusunda yardımcı olacak bilgiler sonuçlanır](./media/troubleshooting-monitoring/image10-import-statements-d.png)

Aynı işlemi gerçekleştirmek için HANA\_yapılandırma\_Minichecks ve onay herhangi _X_ olarak işaretler _C_ (kritik) sütun.

Örnek çıktı:

**HANA\_yapılandırma\_MiniChecks\_Rev102.01 + 1** genel SAP HANA denetimleri için.

![HANA\_yapılandırma\_MiniChecks\_Rev102.01 + 1 genel SAP HANA denetimleri için](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Hizmetleri\_genel bakış** ne SAP HANA Hizmetleri şu anda çalışan bir genel bakış için.

![HANA\_Hizmetleri\_ne SAP HANA Hizmetleri şu anda çalışan bir genel bakış için genel bakış](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Hizmetleri\_istatistikleri** SAP HANA için hizmet bilgileri (CPU, bellek, vs.).

![HANA\_Hizmetleri\_istatistikleri SAP HANA için hizmet bilgileri ](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_yapılandırma\_genel bakış\_Rev110 +** SAP HANA örneği hakkında genel bilgi için.

![HANA\_yapılandırma\_genel bakış\_Rev110 + SAP HANA örneği hakkında genel bilgi için](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_yapılandırma\_parametreleri\_Rev70 +** SAP HANA parametreleri denetlemek için.

![HANA\_yapılandırma\_parametreleri\_SAP HANA parametreleri denetlemek için Rev70 +](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

