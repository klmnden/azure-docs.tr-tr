---
title: İzleme ve sorun giderme HANA'dan yan üzerinde SAP HANA (büyük örnekler) azure'da | Microsoft Docs
description: İzleme ve HANA (büyük örnekler) Azure üzerinde SAP HANA tarafında gelen sorun giderme.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 71970a74817665c97a9522fbc9a68dd3834252b9
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59616365"
---
# <a name="monitoring-and-troubleshooting-from-hana-side"></a>HANA tarafından izleme ve sorun giderme

SAP HANA (büyük örnekler) azure'da ilgili sorunları etkili bir şekilde analiz etmek için sorunun kök nedenini daraltmak kullanışlıdır. SAP yardımcı olan belgeler, büyük bir miktarını yayımlamıştır.

SAP HANA performansıyla ilgili ilgili sık sorulan sorular, aşağıdaki SAP notları bulunabilir:

- [SAP notu #2222200 – SSS: SAP HANA ağ](https://launchpad.support.sap.com/#/notes/2222200)
- [SAP notu #2100040 – SSS: SAP HANA CPU](https://launchpad.support.sap.com/#/notes/0002100040)
- [SAP notu #199997 – SSS: SAP HANA bellek](https://launchpad.support.sap.com/#/notes/2177064)
- [SAP notu #200000 – SSS: SAP HANA performansı iyileştirme](https://launchpad.support.sap.com/#/notes/2000000)
- [SAP notu #199930 – SSS: SAP HANA g/ç analizi](https://launchpad.support.sap.com/#/notes/1999930)
- [SAP notu #2177064 – SSS: SAP HANA hizmeti yeniden başlatın ve kilitlenmeleri](https://launchpad.support.sap.com/#/notes/2177064)

## <a name="sap-hana-alerts"></a>SAP HANA uyarıları

İlk adım, geçerli SAP HANA uyarı günlüklerini kontrol edin. SAP HANA Studio, Git **Yönetim Konsolu: Uyarılar: Göster: tüm uyarıları**. Bu sekme kümesi minimum ve maksimum eşikleri dışında kalan belirli değerleri (boş fiziksel bellek, CPU kullanımı, vb.) tüm SAP HANA uyarılar gösterilir. Denetimleri, varsayılan olarak 15 dakikada bir otomatik olarak yenilenir.

![SAP HANA Studio yönetim konsoluna gidin: Uyarılar: Göster: tüm uyarıları](./media/troubleshooting-monitoring/image1-show-alerts.png)

## <a name="cpu"></a>CPU

Hatalı eşik ayarı nedeniyle tetiklenen bir uyarı için bir çözüm için varsayılan değer ya da daha makul bir eşik değeri sıfırlamaktır.

![Varsayılan değer ya da daha makul bir eşik değeri sıfırlayın](./media/troubleshooting-monitoring/image2-cpu-utilization.png)

Aşağıdaki uyarılar CPU kaynak sorunlarını gösterebilir:

- Ana bilgisayar CPU kullanımı (uyarı 5)
- En son kayıt noktası işlemi (uyarı 28)
- Kayıt noktası süresi (uyarı 54)

Yüksek CPU tüketimi üzerinde SAP HANA veritabanından aşağıdakilerden birini görebilirsiniz:

- Geçerli ya da son CPU kullanımı için uyarı 5 (ana bilgisayar CPU kullanımı) tetiklenir
- Genel Bakış ekranında görüntülenen CPU kullanımı

![Genel Bakış ekranda CPU kullanımı](./media/troubleshooting-monitoring/image3-cpu-usage.png)

Grafı yükleyin, yüksek CPU tüketimi veya yüksek tüketim geçmişte gösterebilir:

![Geçmişte yüksek CPU tüketimi veya yüksek tüketim yük grafını Göster](./media/troubleshooting-monitoring/image4-load-graph.png)

Yüksek CPU kullanımı nedeniyle tetiklenen uyarı, ancak bunlarla sınırlı olmamak de dahil olmak üzere çeşitli nedenlerden kaynaklanabilir: belirli işlemleri, veri yükleme, asılı SQL deyimlerini ve hatalı sorgu performansı (örneğin, BW on HANA ve uzun süre çalışan işlerin yürütülmesi Küpler).

Başvurmak [SAP HANA sorunlarını giderme: CPU ilgili neden olur ve çözümleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/4f/bc915462db406aa2fe92b708b95189/content.htm?frameset=/en/db/6ca50424714af8b370960c04ce667b/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=46&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

## <a name="operating-system"></a>İşletim Sistemi

Linux üzerinde SAP HANA için en önemli denetimleri biri saydam büyük sayfaları devre dışı bırakıldığından emin olmak için bkz: [SAP notu #2131662 – saydam büyük sayfalar (THP) SAP HANA sunucularda](https://launchpad.support.sap.com/#/notes/2131662).

- Saydam büyük sayfaları aşağıdaki Linux komutu etkin olup olmadığını kontrol edebilirsiniz: **Kedi /sys/kernel/mm/transparent\_hugepage/etkin**
- Varsa _her zaman_ alınmış aşağıda gösterildiği gibi köşeli ayraç içinde saydam büyük sayfalar etkinleştirildiğini gösterir: [her zaman] madvise hiçbir zaman if _hiçbir zaman_ alınmış aşağıda gösterildiği gibi köşeli ayraç içinde saydam büyük anlamına Sayfaları devre dışı bırakıldı: her zaman madvise [hiçbir zaman]

Aşağıdaki Linux komutu hiçbir şey döndürmeyen: **rpm - qa | grep ulimit.** Görünürse _ulimit_ olan yüklüyse, bunu hemen kaldırın.

## <a name="memory"></a>Bellek

SAP HANA veritabanı tarafından ayrılan bellek miktarını beklenenden daha yüksek olup olmadığına bakın. Aşağıdaki uyarılar, yüksek bellek kullanımı ile ilgili sorunlar olduğunu gösterir:

- Konak fiziksel bellek kullanımı (uyarı 1)
- Ad sunucusu (uyarı 12) bellek kullanımı
- Sütun Store tablolar (uyarı 40) toplam bellek kullanımı
- Hizmetler (uyarı 43) bellek kullanımı
- Bellek kullanımı (uyarı 45) sütun Store tablonun ana depolama
- Çalışma zamanı döküm dosyaları (uyarı 46)

Başvurmak [SAP HANA sorunlarını giderme: Bellek sorunlarını](https://help.sap.com/saphelp_hanaplatform/helpdata/en/db/6ca50424714af8b370960c04ce667b/content.htm?frameset=/en/59/5eaa513dde43758b51378ab3315ebb/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=26&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

## <a name="network"></a>Ağ

Başvurmak [SAP notu #2081065 – SAP HANA ağ sorunlarını giderme](https://launchpad.support.sap.com/#/notes/2081065) ve sorun giderme adımları bu SAP notuna göz atın, ağ gerçekleştirin.

1. İstemci ve sunucu arasındaki gidiş dönüş süresi çözümleniyor.
  A. SQL betiğini çalıştırmak [ _HANA\_ağ\_istemcileri_](https://launchpad.support.sap.com/#/notes/1969700)_._
  
2. Düğümler arası iletişimin analiz edin.
  A. SQL betiğini Çalıştır [ _HANA\_ağ\_Hizmetleri_](https://launchpad.support.sap.com/#/notes/1969700)_._

3. Linux komutu Çalıştır **ifconfig** (paket kayıpları oluşan çıktı gösterir).
4. Linux komutu Çalıştır **tcpdump**.

Ayrıca, açık kaynak kullanan [IPERF](https://iperf.fr/) Aracı (veya benzeri) gerçek uygulama ağ performansını ölçmek için.

Başvurmak [SAP HANA sorunlarını giderme: Ağ performansı ve bağlantı sorunları](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a3/ccdff1aedc4720acb24ed8826938b6/content.htm?frameset=/en/dc/6ff98fa36541e997e4c719a632cbd8/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=142&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

## <a name="storage"></a>Depolama

Bir son kullanıcı perspektifinden uygulamanın (veya sistemin bir bütün olarak) ağır şekilde çalışır, yanıt vermiyor veya g/ç performansı ile ilgili sorunlar varsa yanıt vermemesine bile görünebilir. İçinde **birimleri** sekmesinde SAP HANA Studio'da bağlı birimlerin ve hangi birimlerin her hizmeti tarafından kullanılan görebilirsiniz.

![SAP HANA Studio birimleri sekmede bağlı birimlerin ve hangi birimlerin her hizmeti tarafından kullanılan görebilirsiniz](./media/troubleshooting-monitoring/image5-volumes-tab-a.png)

Birimlerin bağlı ekranın alt bölümünde, birimler, dosyalar ve g/ç istatistikleri gibi ayrıntılarını görebilirsiniz.

![Birimlerin bağlı ekranın alt bölümünde birimler, dosyalar ve g/ç istatistikleri gibi ayrıntılarını görebilirsiniz.](./media/troubleshooting-monitoring/image6-volumes-tab-b.png)

Başvurmak [SAP HANA sorunlarını giderme: G/ç ilgili temel nedenler ve çözümler](https://help.sap.com/saphelp_hanaplatform/helpdata/en/dc/6ff98fa36541e997e4c719a632cbd8/content.htm?frameset=/en/47/4cb08a715c42fe9f7cc5efdc599959/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=55&amp;show_children=false) ve [SAP HANA sorunlarını giderme: Disk ilgili kök neden olur ve çözümleri](https://help.sap.com/saphelp_hanaplatform/helpdata/en/47/4cb08a715c42fe9f7cc5efdc599959/content.htm?frameset=/en/44/3e1db4f73d42da859008df4f69e37a/frameset.htm&amp;current_toc=/en/85/d132c3f05e40a2b20c25aa5fd6331b/plain.htm&amp;node_id=53&amp;show_children=false) ayrıntılı sorun giderme adımları için site.

## <a name="diagnostic-tools"></a>Tanılama araçları

HANA üzerinden bir SAP HANA sistem durumu denetimi gerçekleştirmek\_yapılandırma\_Minichecks. Bu araç, zaten uyarıları SAP HANA Studio olarak gündeme gelmiş olası kritik teknik sorunlar döndürür.

Başvurmak [SAP notu #1969700 – SAP HANA için SQL deyimi koleksiyonu](https://launchpad.support.sap.com/#/notes/1969700) ve bu not için bağlı SQL Statements.zip dosyasını indirin. Bu .zip dosyasını yerel sabit sürücüsünde Store.

SAP HANA Studio üzerinde **sistem bilgileri** sekmesinde, sağ **adı** sütun ve select **içeri aktarma SQL deyimlerini**.

![Sistem bilgileri sekmesinde SAP HANA Studio ve Ad sütununda sağ tıklayın ve içeri aktarma SQL deyimlerini seçin](./media/troubleshooting-monitoring/image7-import-statements-a.png)

Yerel olarak depolanan SQL Statements.zip dosyasını seçin ve karşılık gelen SQL deyimleri ile bir klasörü içeri aktarılır. Bu noktada, birçok farklı tanılama denetimleri, bu SQL deyimleri ile çalıştırılabilir.

Örneğin, SAP HANA sistem çoğaltması bant genişliği gereksinimlerini test etmek için sağ **bant genişliği** deyiminin altında **çoğaltma: Bant genişliği** seçip **açık** SQL konsolunda.

Tam SQL deyimi, giriş parametrelerini (değiştirilebilir ve sonra çalıştırılabilir için değişiklik bölümü) izin vererek açılır.

![Tam SQL deyimini giriş parametrelerini (değiştirilebilir ve sonra çalıştırılabilir için değişiklik bölümü) sağlayan açılır.](./media/troubleshooting-monitoring/image8-import-statements-b.png)

Başka bir örnek altında deyimleri sağ **çoğaltma: Genel Bakış**. Seçin **yürütme** bağlam menüsünden:

![Çoğaltma altında deyimleri sağ başka bir örnek verilmiştir: Genel bakış. Bağlam menüsünden Çalıştır'ı seçin](./media/troubleshooting-monitoring/image9-import-statements-c.png)

Bu sorun giderme konusunda yardımcı olacak bilgileri sonuçlanır:

![Bu sorun giderme konusunda yardımcı olacak bilgiler sonuçlanır](./media/troubleshooting-monitoring/image10-import-statements-d.png)

HANA için de aynısını yapın\_yapılandırma\_Minichecks ve herhangi bir denetleme _X_ olarak işaretler _C_ (kritik) sütunu.

Örnek çıktı:

**HANA\_yapılandırma\_MiniChecks\_Rev102.01 + 1** genel SAP HANA denetimleri için.

![HANA\_yapılandırma\_MiniChecks\_Rev102.01 + 1 genel SAP HANA denetimleri](./media/troubleshooting-monitoring/image11-configuration-minichecks.png)

**HANA\_Hizmetleri\_genel bakış** ne SAP HANA Hizmetleri şu anda çalışan genel bir bakış için.

![HANA\_Hizmetleri\_ne SAP HANA Hizmetleri şu anda çalışan genel bir bakış için genel bakış](./media/troubleshooting-monitoring/image12-services-overview.png)

**HANA\_Hizmetleri\_istatistikleri** SAP HANA için hizmet bilgileri (CPU, bellek, vs.).

![HANA\_Hizmetleri\_istatistiklerini SAP HANA için hizmet bilgileri](./media/troubleshooting-monitoring/image13-services-statistics.png)

**HANA\_yapılandırma\_genel bakış\_Rev110 +** SAP HANA örneği hakkında genel bilgi için.

![HANA\_yapılandırma\_genel bakış\_Rev110 + SAP HANA örneği hakkında genel bilgi için](./media/troubleshooting-monitoring/image14-configuration-overview.png)

**HANA\_yapılandırma\_parametreleri\_Rev70 +** SAP HANA parametreleri denetlemek için.

![HANA\_yapılandırma\_parametreleri\_SAP HANA parametreleri denetlemek için Rev70 +](./media/troubleshooting-monitoring/image15-configuration-parameters.png)

**Sonraki adımlar**

- Başvuru [STONITH kullanarak SUSE içinde ayarlanmış yüksek kullanılabilirlik](ha-setup-with-stonith.md).