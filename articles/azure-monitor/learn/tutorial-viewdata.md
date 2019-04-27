---
title: Toplanan Azure Log Analytics verilerini görüntüleme veya çözümleme | Microsoft Docs
description: Bu makale, günlük aramalarının oluşturulması ve Log Analytics kaynağınızdaki verilerin Günlük Araması portalı kullanılarak çözümlenmesine ilişkin bir öğretici içerir.  Öğreticiye farklı veri türlerinin döndürülmesine ve sonuçların çözümlenmesi için bazı basit sorguların çalıştırılması da dahildir.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/31/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: c64c0837956c59cf7a810bb06ca63bc21c5158d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60590427"
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Log Analytics günlük araması ile toplanan verileri görüntüleme veya çözümleme

Log Analytics’te, toplanan verileri çözümlemek için sorguları yapılandırarak günlük aramalarından yararlanabilir, en fazla önem verdiğiniz aramaların grafik görünümleriyle özelleştirebileceğiniz önceden mevcut olan panoları kullanabilirsiniz.  Azure VM’lerinizden ve Etkinlik Günlüklerinizden işletimsel verileri toplamayı tanımladığınıza göre bu öğreticide aşağıdaki konularda bilgi edinebilirsiniz:

> [!div class="checklist"]
> * Olay verilerinde basit bir arama gerçekleştirme ve sonuçları değiştirip filtrelemek için özellikleri kullanma 
> * Performans verileriyle çalışmayı öğrenme

Bu öğreticideki örneği tamamlamak için [Log Analytics çalışma alanına bağlı](../../azure-monitor/learn/quick-collect-azurevm.md) mevcut bir sanal makinenizin olması gerekir.  

Döndürülen verilerle etkileşimli bir şekilde çalışmanın yanı sıra sorguları oluşturma ve düzenleme, iki yöntemden biri kullanılarak gerçekleştirilebilir.  Temel sorgular için Azure portalında Günlük Araması sayfasını, gelişmiş sorgulama için de Gelişmiş Analiz portalını kullanabilirsiniz. İki portal arasındaki işlev farkları hakkında daha fazla bilgi için bkz. [Azure Log Analytics’te günlük sorguları oluşturmaya ve düzenlemeye yönelik portallar](../../azure-monitor/log-query/portals.md)

Bu öğreticide, Azure portalındaki Günlük Araması özelliğiyle çalışacağız. 

## <a name="log-in-to-azure-portal"></a>Azure portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="open-the-log-search-portal"></a>Günlük Araması portalını açma 
Günlük Araması portalını açarak işleme başlayın.   

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **İzleyici** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **İzleyici**'yi seçin.
2. İzleyici gezinti menüsünde **Log Analytics**'i ve ardından bir çalışma alanını seçin.

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

![Basit sorgu](media/tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Her kaydın yalnızca ilk birkaç özelliği görüntülenir.  Belirli bir kayda ait tüm özellikleri görüntülemek için **daha fazla göster** seçeneğine tıklayın.

## <a name="filter-results-of-the-query"></a>Sorgu sonuçlarını filtreleme
Ekranın sol tarafında, doğrudan değişiklik yapmadan sorguya filtreleme eklemenize olanak tanıyan bir filtre bölmesi mevcuttur.  Söz konusu kayıt türü için çeşitli kayıt özellikleri görüntülenir ve arama sonuçlarınızı daraltmak için bir veya daha fazla özellik değeri seçebilirsiniz.

**Event** ile çalışıyorsanız **EVENTLEVELNAME**’in altındaki **Error**’ın yanındaki onay kutusunu işaretleyin.   **Syslog** ile çalışıyorsanız **SEVERITYLEVEL**’ın altındaki **err**’in yanındaki onay kutusunu işaretleyin.  Bu, sonuçları hata olaylarıyla sınırlamak için sorguyu aşağıdakilerden birine değiştirir.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtre](media/tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Kayıtlardan birindeki özellik menüsünden **Filtrelere ekle** seçeneğini belirleyerek filtre bölmesine özellik ekleyin.

![Filtreye ekle menüsü](media/tutorial-viewdata/log-analytics-portal-eventlist-03.png)

Filtrelemek istediğiniz değere sahip bir kayıt için özellik menüsünden **Filtre** seçeneğini belirleyerek aynı filtreyi ayarlayabilirsiniz.  

**Filtre** seçeneğine yalnızca, üzerine geldiğinizde adları mavi renkte görünen özellikler için sahip olursunuz.  Bunlar, arama koşulları için dizini oluşturulmuş *aranabilir* alanlardır.  Gri alanlar yalnızca **Başvuruları göster** seçeneğinin bulunduğu *serbest metin aranabilir* alanlardır.  Bu seçenek, herhangi bir özellikte söz konusu değere sahip kayıtları döndürür.

Kayıt menüsünde **Gruplandırma ölçütü** seçeneğini belirleyerek tek bir özellikteki sonuçları gruplandırabilirsiniz.  Bu işlem, sorgunuza sonuçları bir grafikte görüntüleyen bir [özetleme](/azure/kusto/query/summarizeoperator) işleci ekler.  Birden fazla özelliği gruplandırabilirsiniz, ancak sorguyu doğrudan düzenlemeniz gerekir.  **Bilgisayar** özelliğinin yanındaki kayıt menüsünü seçin ve **'Bilgisayara' Göre Gruplandır** seçeneğini belirleyin.  

![Bilgisayara göre gruplandırma](media/tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Sonuçlar üzerinde çalışma
Günlük Araması portalında, bir sorgunun sonuçları üzerinde çalışılmasına yönelik çeşitli özellikler mevcuttur.  Asıl sorguyu değiştirmeden verileri çözümlemek için sonuçları sıralayabilir, filtreleyebilir ve gruplandırabilirsiniz.  Bir sorgunun sonuçları varsayılan olarak sıralı değildir.

Verileri, filtreleme ve sıralama için ek seçenekler sunan bir tablo formunda görüntülemek için **Tablo** seçeneğine tıklayın.  

![Tablo görünümü](media/tutorial-viewdata/log-search-portal-table-01.png)

Bu kaydın ayrıntılarını görüntülemek için bir kaydın yanındaki oka tıklayın.

![Sonuçları sıralama](media/tutorial-viewdata/log-search-portal-table-02.png)

Herhangi bir alanın sütun başlığına tıklayarak bu alan üzerinde sıralama yapın.

![Sonuçları sıralama](media/tutorial-viewdata/log-search-portal-table-03.png)

Filtre düğmesine tıklayarak ve bir filtre koşulu sağlayarak sütundaki belirli bir değerdeki sonuçları filtreleyin.

![Sonuçları filtreleme](media/tutorial-viewdata/log-search-portal-table-04.png)

Bir sütun başlığını sonuçların üst tarafına sürükleyerek söz konusu sütun üzerinde gruplandırma yapın.  Birden çok sütunu üst tarafa sürükleyerek birden çok alan üzerinde gruplandırma yapabilirsiniz.

![Sonuçları gruplandırma](media/tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Performans verileriyle çalışma
Hem Windows hem de Linux aracıları için performans verileri depolanan **Perf** tablosundaki Log Analytics çalışma alanında depolanmaktadır.  Performans kayıtları diğer tüm kayıtlar gibi görünür ve biz de şimdi olaylarda olduğu gibi tüm performans kayıtlarını döndüren basit bir sorgu yazacağız.

```
Perf
```

![Performans verileri](media/tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Ancak tüm performans nesneleri ve sayaçları için milyonlarca kaydın döndürülmesi çok kullanışlı değildir.  Verileri filtrelemek için, yukarıda kullandığınız yöntemleri kullanabilir veya aşağıdaki sorguyu doğrudan günlük araması kutusuna yazabilirsiniz.  Bu hem Windows hem de Linux bilgisayarlar için yalnızca işlemci kullanımı kayıtlarını döndürür.

```
Perf | where ObjectName == "Processor"  | where CounterName == "% Processor Time"
```

![İşlemci kullanımı](media/tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

Bu, verileri belirli bir sayaç ile sınırlar ancak yine de özellikle kullanışlı olacak bir biçime getirmez.  Verileri bir çizgi grafikte görüntüleyebilirsiniz, ancak öncelikle Computer ve TimeGenerated özelliklerine göre gruplandırmanız gerekir.  Birden çok alan üzerinde gruplandırma yapmak için sorguyu doğrudan değiştirmeniz gerekir, bu nedenle sorguyu aşağıdaki şekilde değiştirin.  Bu, her saat için ortalama değeri hesaplamak üzere **CounterValue** özelliğindeki [ort](/azure/kusto/query/avg-aggfunction) işlevini kullanır.

```
Perf  
| where ObjectName == "Processor"  | where CounterName == "% Processor Time"
| summarize avg(CounterValue) by Computer, TimeGenerated
```

![Performans verileri grafiği](media/tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Veriler uygun şekilde gruplandırıldığına göre [işleme](/azure/kusto/query/renderoperator) işlecini ekleyerek bunları görsel bir grafikte görüntüleyebilirsiniz.  

```
Perf  
| where ObjectName == "Processor" | where CounterName == "% Processor Time" 
| summarize avg(CounterValue) by Computer, TimeGenerated 
| render timechart
```

![Çizgi grafik](media/tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, olay ve performans verilerini çözümlemeye yönelik temel günlük aramaları oluşturmayı öğrendiniz.  Verileri pano oluşturarak görselleştirmeyi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Log Analytics panoları oluşturma ve paylaşma](tutorial-logs-dashboards.md)
