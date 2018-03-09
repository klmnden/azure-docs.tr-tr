---
title: "Azure günlük analizi günlük arama Portalı'nı kullanarak | Microsoft Docs"
description: "Bu makalede günlük aramalar oluşturun ve günlük arama Portalı'nı kullanarak günlük analizi çalışma alanınızda depolanan verileri çözümleyen açıklar bir öğretici içerir.  Öğreticiye farklı veri türlerinin döndürülmesine ve sonuçların çözümlenmesi için bazı basit sorguların çalıştırılması da dahildir."
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
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 37213012e817f0fae21a47a4334a519bbbca206b
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Günlük arama Portalı'nı kullanarak Azure günlük analizi günlük aramalar oluşturun

> [!NOTE]
> Bu makalede Azure günlük analizi yeni sorgu dili kullanarak günlük arama portalında açıklanmaktadır.  Yeni dil hakkında daha fazla bilgi ve çalışma alanı yükseltme yordamı elde [Azure günlük analizi çalışma alanınız için yeni günlük arama yükseltme](log-analytics-log-search-upgrade.md).  
>
> Çalışma alanınızı yeni sorgu dili yükseltilmedi, için başvurmalıdır [Bul günlük aramaları günlük analizi kullanarak verileri](log-analytics-log-searches.md) günlük arama Portalı'nın geçerli sürümü hakkında bilgi için.

Bu makalede günlük aramalar oluşturun ve günlük arama Portalı'nı kullanarak günlük analizi çalışma alanınızda depolanan verileri çözümleyen açıklar bir öğretici içerir.  Öğreticiye farklı veri türlerinin döndürülmesine ve sonuçların çözümlenmesi için bazı basit sorguların çalıştırılması da dahildir.  Doğrudan değiştirmek yerine sorgu değiştirmek için günlük arama portal özelliklerinde odaklanır.  Doğrudan sorgu düzenleme hakkında daha fazla bilgi için bkz: [sorgu dili başvurusu](https://go.microsoft.com/fwlink/?linkid=856079).

Günlük arama portalı yerine Advanced Analytics portalında aramaları oluşturmak için bkz [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856587).  Her iki portalları aynı sorgu dili günlük analizi çalışma alanındaki aynı verilere erişmek için kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir günlük analizi çalışma alanı çözümlemek için sorgular için veri üretir en az bir bağlı kaynağıyla zaten sahip olduğunuzu varsayar.  

- Bir çalışma alanı yoksa, en yordamı kullanarak boş bir tane oluşturabilirsiniz [günlük analizi çalışma alanı ile çalışmaya başlama](log-analytics-get-started.md).
- En az bir bağlanma [Windows Aracısı](log-analytics-windows-agent.md) veya bir [Linux Aracısı](log-analytics-linux-agents.md) çalışma alanı.  

## <a name="open-the-log-search-portal"></a>Günlük Araması portalını açma
Günlük Araması portalını açarak işleme başlayın. 

1. Azure portalı açın.
2. Günlük analizi gidin ve çalışma alanınızı seçin.
3. Seçin **oturum arama**.

![Günlük Ara düğmesi](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Basit bir arama oluşturma
Üzerinde çalışılacak bazı verileri almanın en hızlı yolu, tablodaki tüm kayıtları döndüren basit bir sorgudur.  Çalışma alanınıza bağlı Windows veya Linux istemcileriniz varsa Olay (Windows) veya Syslog (Linux) tablosunda verileriniz olur.

Arama kutusuna aşağıdaki sorgulardan birini yazın ve arama düğmesine tıklayın.  

```
Event
```
```
Syslog
```

Veriler, varsayılan liste görünümünde döndürülür ve toplamda kaç kaydın döndürüldüğünü görebilirsiniz.

![Basit sorgu](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Her kaydın yalnızca ilk birkaç özelliği görüntülenir.  Belirli bir kayda ait tüm özellikleri görüntülemek için **daha fazla göster** seçeneğine tıklayın.

![Kayıt ayrıntıları](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Zaman kapsamını Ayarla
Günlük analizi tarafından toplanan her kaydına sahip bir **TimeGenerated** kaydın oluşturulduğu saat ve tarihi içeren özelliği.  Günlük arama portal sorguda yalnızca kayıtlarıyla döndüren bir **TimeGenerated** zaman kapsamdaki ekranın sol tarafında görüntülenir.  

Zaman filtresi açılır seçerek veya kaydırıcıyı değiştirerek değiştirebilirsiniz.  Kaydırıcı her zaman diliminin aralıkta kayıtlarını göreli sayısını gösteren bir çubuk grafiği görüntüler.  Bu kesimin aralığın bağlı olarak değişir.

Varsayılan süre kapsamı **1 gün**.  Bu değeri değiştirmek **7 gün**, ve kayıtların toplam sayısını artırmalısınız.

![Tarih saat kapsamı](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Sorgu sonuçlarını filtreleme
Ekranın sol tarafında, doğrudan değişiklik yapmadan sorguya filtreleme eklemenize olanak tanıyan bir filtre bölmesi mevcuttur.  Döndürülen kayıtları birkaç özelliklerini ilk on değerleriyle kendi kayıt sayısı ile birlikte görüntülenir.

**Event** ile çalışıyorsanız **EVENTLEVELNAME**’in altındaki **Error**’ın yanındaki onay kutusunu işaretleyin.   **Syslog** ile çalışıyorsanız **SEVERITYLEVEL**’ın altındaki **err**’in yanındaki onay kutusunu işaretleyin.  Bu, sonuçları hata olaylarıyla sınırlamak için sorguyu aşağıdakilerden birine değiştirir.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtre](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Kayıtlardan birindeki özellik menüsünden **Filtrelere ekle** seçeneğini belirleyerek filtre bölmesine özellik ekleyin.

![Filtreye ekle menüsü](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Filtrelemek istediğiniz değere sahip bir kayıt için özellik menüsünden **Filtre** seçeneğini belirleyerek aynı filtreyi ayarlayabilirsiniz.  

Yalnızca **filtre** adlarının mavi olan özellikleri seçeneği.  Bunlar, arama koşulları için dizini oluşturulmuş *aranabilir* alanlardır.  Gri alanlar yalnızca **Başvuruları göster** seçeneğinin bulunduğu *serbest metin aranabilir* alanlardır.  Bu seçenek, herhangi bir özellikte söz konusu değere sahip kayıtları döndürür.

![Filtre menüsü](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Kayıt menüsünde **Gruplandırma ölçütü** seçeneğini belirleyerek tek bir özellikteki sonuçları gruplandırabilirsiniz.  Bu işlem, sorgunuza sonuçları bir grafikte görüntüleyen bir [özetleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) işleci ekler.  Birden fazla özelliği gruplandırabilirsiniz, ancak sorguyu doğrudan düzenlemeniz gerekir.  Kayıt sonraki menüsünü **bilgisayar** özelliği ve select **'Bilgisayar' grupla**.  

![Bilgisayara göre gruplandırma](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Sonuçlar üzerinde çalışma
Günlük Araması portalında, bir sorgunun sonuçları üzerinde çalışılmasına yönelik çeşitli özellikler mevcuttur.  Asıl sorguyu değiştirmeden verileri çözümlemek için sonuçları sıralayabilir, filtreleyebilir ve gruplandırabilirsiniz.  Bir sorgunun sonuçları varsayılan olarak sıralı değildir.

Verileri, filtreleme ve sıralama için ek seçenekler sunan bir tablo formunda görüntülemek için **Tablo** seçeneğine tıklayın.  

![Tablo görünümü](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Bu kaydın ayrıntılarını görüntülemek için bir kaydın yanındaki oka tıklayın.

![Sonuçları sıralama](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Herhangi bir alanın sütun başlığına tıklayarak bu alan üzerinde sıralama yapın.

![Sonuçları sıralama](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtre düğmesine tıklayarak ve bir filtre koşulu sağlayarak sütundaki belirli bir değerdeki sonuçları filtreleyin.

![Sonuçları filtreleme](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Bir sütun başlığını sonuçların üst tarafına sürükleyerek söz konusu sütun üzerinde gruplandırma yapın.  Birden çok sütunu üst tarafa sürükleyerek birden çok alan üzerinde gruplandırma yapabilirsiniz.

![Sonuçları gruplandırma](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Performans verileriyle çalışma
Hem Windows hem de Linux aracıları için performans verileri depolanan **Perf** tablosundaki Log Analytics çalışma alanında depolanmaktadır.  Diğer kaydı gibi performans kayıtları aramak ve biz tüm performans kayıtları olayları ile olduğu gibi veren basit bir sorgu yazabilirsiniz.

```
Perf
```

![Performans verileri](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Ancak tüm performans nesneleri ve sayaçları için milyonlarca kaydın döndürülmesi çok kullanışlı değildir.  Verileri filtrelemek için, yukarıda kullandığınız yöntemleri kullanabilir veya aşağıdaki sorguyu doğrudan günlük araması kutusuna yazabilirsiniz.  Bu hem Windows hem de Linux bilgisayarlar için yalnızca işlemci kullanımı kayıtlarını döndürür.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![İşlemci kullanımı](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Bu, verileri belirli bir sayaç ile sınırlar ancak yine de özellikle kullanışlı olacak bir biçime getirmez.  Verileri bir çizgi grafikte görüntüleyebilirsiniz, ancak öncelikle Computer ve TimeGenerated özelliklerine göre gruplandırmanız gerekir.  Birden çok alan üzerinde gruplandırma yapmak için sorguyu doğrudan değiştirmeniz gerekir, bu nedenle sorguyu aşağıdaki şekilde değiştirin.  Bu, her saat için ortalama değeri hesaplamak üzere **CounterValue** özelliğindeki [ort](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) işlevini kullanır.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Performans verileri grafiği](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Veriler uygun şekilde gruplandırıldığına göre [işleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) işlecini ekleyerek bunları görsel bir grafikte görüntüleyebilirsiniz.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Çizgi grafik](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Sonraki adımlar

- Günlük analizi sorgu dili hakkında daha fazla bilgi [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856079).
- Eğitmen kullanılarak üzerinden yol [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587) olanak sağlayan aynı sorguları çalıştırmak ve günlük arama portalı aynı verilere erişmek.
