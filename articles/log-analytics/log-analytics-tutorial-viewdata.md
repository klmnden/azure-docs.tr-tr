---
title: "Görüntülemek veya toplanan Azure günlük analizi veri çözümleme | Microsoft Docs"
description: "Bu makalede günlük aramalar oluşturun ve günlük arama Portalı'nı kullanarak, günlük analizi kaynağında depolanan verileri çözümleyen açıklar bir öğretici içerir.  Öğretici, farklı türdeki veri ve analiz etme sonuçları döndürmek için bazı basit sorgu çalıştırmayı içerir."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: dfcbb925a16ca1e53d10b7bf70d03e62bc9dae69
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Görüntülemek veya günlük analizi günlük arama ile toplanan verileri analiz etme

Günlük analizi toplanan verileri çözümlemek, özelleştirebileceğiniz önceden varolan Pano en değerli aramalarınız Grafik görünümlerini kullanın sorguları oluşturarak günlük aramaları yararlanabilirsiniz.  Bu öğreticide, Azure Vm'leri ve etkinlik günlükleri işletimsel veri toplamayı tanımladığınız, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Yeni sorgu dili Azure günlük analizi kaynağınız yükseltme 
> * Basit Arama olay verilerini gerçekleştirir ve özelliklerini değiştirmek ve sonuçlara filtre uygulamak için kullanma 
> * Performans verileri ile çalışma konusunda bilgi edinin

Bu öğreticide örnek tamamlamak için var olan bir sanal makine olması [için günlük analizi çalışma alanına bağlı](log-analytics-quick-collect-azurevm.md).  

Oluşturma ve etkileşimli olarak döndürülen verilerle çalışma yanı sıra sorguları düzenleme iki yolla başarılı biri olabilir.  Temel sorguları için günlük arama sayfası Azure portalında kullanmak veya gelişmiş sorgulama için Gelişmiş Analytics Portalı'nı kullanabilirsiniz. İşlev iki portalı arasındaki fark hakkında daha fazla bilgi için bkz: [oluşturma ve Azure günlük analizi günlük sorgularda düzenleme için portalları](log-analytics-log-search-portals.md)

Bu öğreticide, biz günlük arama ile Azure portalında çalışacaksınız. 

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com). 

## <a name="open-the-log-search-portal"></a>Günlük arama portalını açın 
Günlük arama Portalı'nı açarak başlayın.   

1. Azure portalında tıklatın **daha fazla hizmet** sol alt köşesindeki üzerinde bulunamadı. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **oturum Analytics**.
2. Günlük analizi abonelikleri bölmesinde, bir çalışma alanını seçin ve ardından **günlük arama** döşeme.<br> ![Günlük Ara düğmesi](media/log-analytics-tutorial-viewdata/azure-portal-01.png)

Başlık yükseltmek için davet portalında günlük analizi kaynak sayfanızın üstte fark etmiş.<br> ![Azure Portalı'ndaki bildirim günlük analizi yükseltme](media/log-analytics-tutorial-viewdata/log-analytics-portal-upgradebanner.png)

Günlük analizi oluşturmak kolaylaştırır sorgular yapar yakın zamanda yeni bir sorgu dili sunulan, çeşitli correlate veri kaynakları ve eğilimleri veya sorunlarını hızlı bir şekilde tanımlamak için analiz.

Yükseltme basit bir işlemdir.  Bildiren başlığında tıklatarak işlemi başlatabilirsiniz **daha fazla bilgi edinin ve yükseltme**.  Yükseltme hakkında ek bilgi yükseltme bilgileri sayfasında aracılığıyla okuyun ve ardından **Şimdi Yükselt**.

İşlemin tamamlanması birkaç dakika sürer ve bu süre boyunca altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde. Hakkında daha fazla bilgiyi [yeni sorgu dili yararları](log-analytics-log-search-upgrade.md#why-the-new-language).

## <a name="create-a-simple-search"></a>Basit Arama oluşturma
Tablodaki tüm kayıtları döndürür basit bir sorgu çalışmak için bazı veri almak için en hızlı yoludur.  Varsa tüm Windows veya Linux istemcileri, çalışma alanına bağlı sonra verileri olay (Windows) veya Syslog (Linux) tablosu sahip olacaksınız.

Aşağıdaki sorgularda arama kutusuna yazın ve arama düğmesini tıklatın.  

```
Event
```
```
Syslog
```

Varsayılan liste görünümünde veriler döndürülür ve kaç tane toplam kaydı döndürülmedi görebilirsiniz.

![Basit Sorgu](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Her kayıt yalnızca ilk birkaç özellikleri görüntülenir.  Tıklatın **daha fazla Göster** belirli bir kaydın tüm özelliklerini görüntülemek için.

## <a name="filter-results-of-the-query"></a>Filtre sorgusunun sonuçları
Ekranın sol tarafındaki filtreleme için sorgu doğrudan değiştirmeden eklemenize olanak sağlayan Filtre Bölmesi ' dir.  Bu kayıt türü için çeşitli kaydı özellikler görüntülenir ve arama sonuçlarınızı daraltmak için bir veya daha fazla özellik değerlerini seçebilirsiniz.

İle çalışıyorsanız **olay**, yanındaki onay kutusunu işaretleyin **hata** altında **EVENTLEVELNAME**.   İle çalışıyorsanız **Syslog**, yanındaki onay kutusunu işaretleyin **hata** altında **önem düzeyi**.  Bu, sorgu sonuçları hata olayları sınırlandırmak için aşağıdakilerden birine değiştirir.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtre](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Özellikler Filtre bölmesini seçerek eklemek **için Filtre Ekle** menüsünden özelliği kayıtları biri.

![Filtre menü ekleme](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-03.png)

Seçerek, aynı filtre ayarlayabilirsiniz **filtre** filtrelemek istediğiniz değer içeren bir kayıt özelliği menüsünden.  

Yalnızca **filtre** adlarının üzerine geldiğinizde mavi olan özellikleri seçeneği.  Bunlar *aranabilir* için dizin alanları arama koşulları.  Gri alanlar *serbest metin aranabilir* yalnızca olan alanları **Göster başvuruları** seçeneği.  Bu seçenek, o değeri herhangi bir özellik olan kayıtları döndürür.

Tek bir özellik sonuçlarına seçerek gruplandırabilirsiniz **Group by** kaydı menü seçeneği.  Bu ekler bir [özetlemek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) sonuçları bir grafik görüntüler, sorgu işleci.  Birden fazla özellik gruplandırabilirsiniz, ancak sorgu doğrudan düzenlemeniz gerekir.  Kayıt sonraki menüsünü **bilgisayar** özelliği ve select **'Bilgisayar' grupla**.  

![Bilgisayar grubu](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Sonuçları ile çalışma
Günlük arama portal çeşitli bir sorgunun sonuçlarını ile çalışmak için özellikler vardır.  Sıralayabilir, filtre ve gerçek sorgu değiştirilmeden verileri çözümlemek için Grup sonuçları.  Varsayılan olarak bir sorgunun sonuçlarını sıralı değil.

Verileri filtreleme ve sıralama için ek seçenekler sağlayan Tablo formunda görüntülemek için tıklatın **tablo**.  

![Tablo görünümü](media/log-analytics-tutorial-viewdata/log-search-portal-table-01.png)

Bu kayıt ayrıntılarını görüntülemek için bir kayıt tarafından oka tıklayın.

![Sıralama sonuçları](media/log-analytics-tutorial-viewdata/log-search-portal-table-02.png)

Herhangi bir alanı kendi sütun başlığına tıklayarak sıralayın.

![Sıralama sonuçları](media/log-analytics-tutorial-viewdata/log-search-portal-table-03.png)

Belirli bir sütun değeri sonuçlarına filtre düğmesini tıklatarak ve bir filtre koşulu sağlayan filtreleyin.

![Sonuçları filtresi](media/log-analytics-tutorial-viewdata/log-search-portal-table-04.png)

Bir sütun üzerinde sonuçları üstündeki sütun başlığını sürükleyerek grup.  Birden çok sütun dön sürükleyerek üzerinde birden çok alan gruplandırabilirsiniz.

![Grup sonuçları](media/log-analytics-tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Performans verileri ile çalışma
Windows ve Linux aracıları için performans verilerini günlük analizi çalışma alanında depolanan **Perf** tablo.  Diğer kaydı gibi performans kayıtları aramak ve tüm performans kayıtları olayları ile olduğu gibi veren basit bir sorgu yazmak için kalacaklarını.

```
Perf
```

![Performans verileri](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Tüm performans nesneleri ve sayaçları kayıtlarını milyonlarca ancak döndüren çok kullanışlı değildir.  Verileri filtreleyin veya yalnızca aşağıdaki sorguyu doğrudan günlük arama kutusuna yukarıda kullanılan aynı yöntemleri kullanabilirsiniz.  Bu, Windows ve Linux bilgisayarlar için kullanım kayıtları yalnızca işlemci döndürür.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![İşlemci kullanımı](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Bu verileri belirli bir sayaç için sınırlar, ancak bunu hala bu özellikle yararlı bir formda put değil.  Veri bir çizgi grafiği görüntüler, ancak ilk bilgisayar ve TimeGenerated göre gruplandırmanız gerekir.  Birden çok alanları gruplandırmak için sorguyu doğrudan değiştirin, gerekir böylece şu sorguyu değiştirin.  Bu kullanır [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) üzerinde işlev **CounterValue** her bir saat ortalama değerini hesaplamak için özellik.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Performans veri grafiği](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Veriler uygun gruplandırılır, onu visual grafik ekleyerek görüntüleyebileceğiniz [işleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) işleci.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Çizgi grafiği](media/log-analytics-tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, olay ve performans verileri çözümlemek için temel günlük aramalarını oluşturmak öğrendiniz.  Pano oluşturarak verileri görselleştirmek öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Günlük analizi panoları paylaşmak ve oluşturma](log-analytics-tutorial-dashboards.md)