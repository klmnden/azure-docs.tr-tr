---
title: "Azure günlük analizi günlük arama Portalı'nı kullanarak | Microsoft Docs"
description: "Bu makalede günlük aramalar oluşturun ve günlük arama Portalı'nı kullanarak günlük analizi çalışma alanınızda depolanan verileri çözümleyen açıklar bir öğretici içerir.  Öğretici, farklı türdeki veri ve analiz etme sonuçları döndürmek için bazı basit sorgu çalıştırmayı içerir."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 6fc556ceb34cde26d5f3789a2397cdaa34b0b84d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Günlük arama Portalı'nı kullanarak Azure günlük analizi günlük aramalar oluşturun

> [!NOTE]
> Bu makalede Azure günlük analizi yeni sorgu dili kullanarak günlük arama portalında açıklanmaktadır.  Yeni dil hakkında daha fazla bilgi ve çalışma alanı yükseltme yordamı elde [Azure günlük analizi çalışma alanınız için yeni günlük arama yükseltme](log-analytics-log-search-upgrade.md).  
>
> Çalışma alanınızı yeni sorgu dili yükseltilmedi, için başvurmalıdır [Bul günlük aramaları günlük analizi kullanarak verileri](log-analytics-log-searches.md) günlük arama Portalı'nın geçerli sürümü hakkında bilgi için.

Bu makalede günlük aramalar oluşturun ve günlük arama Portalı'nı kullanarak günlük analizi çalışma alanınızda depolanan verileri çözümleyen açıklar bir öğretici içerir.  Öğretici, farklı türdeki veri ve analiz etme sonuçları döndürmek için bazı basit sorgu çalıştırmayı içerir.  Doğrudan değiştirmek yerine sorgu değiştirmek için günlük arama portal özelliklerinde odaklanır.  Doğrudan sorgu düzenleme hakkında daha fazla bilgi için bkz: [sorgu dili başvurusu](https://go.microsoft.com/fwlink/?linkid=856079).

Günlük arama portalı yerine Advanced Analytics portalında aramaları oluşturmak için bkz [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856587).  Her iki portalları aynı sorgu dili günlük analizi çalışma alanındaki aynı verilere erişmek için kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, bir günlük analizi çalışma alanı çözümlemek için sorgular için veri üretir en az bir bağlı kaynağıyla zaten sahip olduğunuzu varsayar.  

- Bir çalışma alanı yoksa, en yordamı kullanarak boş bir tane oluşturabilirsiniz [günlük analizi çalışma alanı ile çalışmaya başlama](log-analytics-get-started.md).
- En az bir bağlanma [Windows Aracısı](log-analytics-windows-agents.md) veya bir [Linux Aracısı](log-analytics-linux-agents.md) çalışma alanı.  

## <a name="open-the-log-search-portal"></a>Günlük arama portalını açın
Günlük arama Portalı'nı açarak başlayın.  Azure portal veya OMS portalı erişebilir.

1. Azure Portalı'nı açın.
2. Günlük analizi gidin ve çalışma alanınızı seçin.
3. Her iki select **günlük arama** Azure portalında kalın veya seçerek OMS Portalı'nı başlatmak için **OMS portalı** ve günlük arama düğmesini tıklatarak.

![Günlük Ara düğmesi](media/log-analytics-log-search-log-search-portal/log-search-button.png)

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

![Basit Sorgu](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Her kayıt yalnızca ilk birkaç özellikleri görüntülenir.  Tıklatın **daha fazla Göster** belirli bir kaydın tüm özelliklerini görüntülemek için.

![Kayıt ayrıntıları](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Zaman kapsamını Ayarla
Günlük analizi tarafından toplanan her kaydına sahip bir **TimeGenerated** kaydın oluşturulduğu saat ve tarihi içeren özelliği.  Günlük arama portal sorguda yalnızca kayıtlarıyla döndüren bir **TimeGenerated** zaman kapsamdaki ekranın sol tarafında görüntülenir.  

Zaman filtresi açılır seçerek veya kaydırıcıyı değiştirerek değiştirebilirsiniz.  Kaydırıcı her zaman diliminin aralıkta kayıtlarını göreli sayısını gösteren bir çubuk grafiği görüntüler.  Bu kesimin aralığın bağlı olarak değişir.

Varsayılan süre kapsamı **1 gün**.  Bu değeri değiştirmek **7 gün**, ve kayıtların toplam sayısını artırmalısınız.

![Tarih saat kapsamı](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtre sorgusunun sonuçları
Ekranın sol tarafındaki filtreleme için sorgu doğrudan değiştirmeden eklemenize olanak sağlayan Filtre Bölmesi ' dir.  Döndürülen kayıtları birkaç özelliklerini ilk on değerleriyle kendi kayıt sayısı ile birlikte görüntülenir.

İle çalışıyorsanız **olay**, yanındaki onay kutusunu işaretleyin **hata** altında **EVENTLEVELNAME**.   İle çalışıyorsanız **Syslog**, yanındaki onay kutusunu işaretleyin **hata** altında **önem düzeyi**.  Bu, sorgu sonuçları hata olayları sınırlandırmak için aşağıdakilerden birine değiştirir.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtre](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Özellikler Filtre bölmesini seçerek eklemek **için Filtre Ekle** menüsünden özelliği kayıtları biri.

![Filtre menü ekleme](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Seçerek, aynı filtre ayarlayabilirsiniz **filtre** filtrelemek istediğiniz değer içeren bir kayıt özelliği menüsünden.  

Yalnızca **filtre** adlarının mavi olan özellikleri seçeneği.  Bunlar *aranabilir* için dizin alanları arama koşulları.  Gri alanlar *serbest metin aranabilir* yalnızca olan alanları **Göster başvuruları** seçeneği.  Bu seçenek, o değeri herhangi bir özellik olan kayıtları döndürür.

![Filtre menüsü](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Tek bir özellik sonuçlarına seçerek gruplandırabilirsiniz **Group by** kaydı menü seçeneği.  Bu ekler bir [özetlemek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) sonuçları bir grafik görüntüler, sorgu işleci.  Birden fazla özellik gruplandırabilirsiniz, ancak sorgu doğrudan düzenlemeniz gerekir.  Kayıt sonraki menüsünü **bilgisayar** özelliği ve select **'Bilgisayar' grupla**.  

![Bilgisayar grubu](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Sonuçları ile çalışma
Günlük arama portal çeşitli bir sorgunun sonuçlarını ile çalışmak için özellikler vardır.  Sıralayabilir, filtre ve gerçek sorgu değiştirilmeden verileri çözümlemek için Grup sonuçları.  Varsayılan olarak bir sorgunun sonuçlarını sıralı değil.

Verileri filtreleme ve sıralama için ek seçenekler sağlayan Tablo formunda görüntülemek için tıklatın **tablo**.  

![Tablo görünümü](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Bu kayıt ayrıntılarını görüntülemek için bir kayıt tarafından oka tıklayın.

![Sıralama sonuçları](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Herhangi bir alanı kendi sütun başlığına tıklayarak sıralayın.

![Sıralama sonuçları](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Belirli bir sütun değeri sonuçlarına filtre düğmesini tıklatarak ve bir filtre koşulu sağlayan filtreleyin.

![Sonuçları filtresi](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Bir sütun üzerinde sonuçları üstündeki sütun başlığını sürükleyerek grup.  Birden çok sütun dön sürükleyerek üzerinde birden çok alan gruplandırabilirsiniz.

![Grup sonuçları](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Performans verileri ile çalışma
Windows ve Linux aracıları için performans verilerini günlük analizi çalışma alanında depolanan **Perf** tablo.  Diğer kaydı gibi performans kayıtları aramak ve biz tüm performans kayıtları olayları ile olduğu gibi veren basit bir sorgu yazabilirsiniz.

```
Perf
```

![Performans verileri](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Tüm performans nesneleri ve sayaçları kayıtlarını milyonlarca ancak döndüren çok kullanışlı değildir.  Verileri filtreleyin veya yalnızca aşağıdaki sorguyu doğrudan günlük arama kutusuna yukarıda kullanılan aynı yöntemleri kullanabilirsiniz.  Bu, Windows ve Linux bilgisayarlar için kullanım kayıtları yalnızca işlemci döndürür.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![İşlemci kullanımı](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Bu verileri belirli bir sayaç için sınırlar, ancak bunu hala bu özellikle yararlı bir formda put değil.  Veri bir çizgi grafiği görüntüler, ancak ilk bilgisayar ve TimeGenerated göre gruplandırmanız gerekir.  Birden çok alanları gruplandırmak için sorguyu doğrudan değiştirin, gerekir böylece şu sorguyu değiştirin.  Bu kullanır [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) üzerinde işlev **CounterValue** her bir saat ortalama değerini hesaplamak için özellik.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Performans veri grafiği](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Veriler uygun gruplandırılır, onu visual grafik ekleyerek görüntüleyebileceğiniz [işleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) işleci.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Çizgi grafiği](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Sonraki adımlar

- Günlük analizi sorgu dili hakkında daha fazla bilgi [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856079).
- Eğitmen kullanılarak üzerinden yol [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587) olanak sağlayan aynı sorguları çalıştırmak ve günlük arama portalı aynı verilere erişmek.
