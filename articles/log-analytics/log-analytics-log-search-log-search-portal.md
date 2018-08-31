---
title: Azure Log Analytics'te günlük araması portalını kullanma | Microsoft Docs
description: Bu makale, günlük aramalarının oluşturulması ve günlük araması portalını kullanarak Log Analytics çalışma alanınızda depolanmış verileri analiz etme açıklayan bir öğretici içerir.  Öğreticiye farklı veri türlerinin döndürülmesine ve sonuçların çözümlenmesi için bazı basit sorguların çalıştırılması da dahildir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 532df20a7639f42d8ba1c840a5fd19f0ad0e4042
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43246342"
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Günlük araması portalını kullanarak Azure Log Analytics günlük aramalarının oluşturulması

Bu makale, günlük aramalarının oluşturulması ve günlük araması portalını kullanarak Log Analytics çalışma alanınızda depolanmış verileri analiz etme açıklayan bir öğretici içerir.  Öğreticiye farklı veri türlerinin döndürülmesine ve sonuçların çözümlenmesi için bazı basit sorguların çalıştırılması da dahildir.  Doğrudan değiştirme yerine sorgu değiştirilmeden için günlük araması portalında özelliklere odaklanır.  Doğrudan sorgu düzenleme hakkında daha fazla bilgi için bkz [sorgu dili başvurusu](https://go.microsoft.com/fwlink/?linkid=856079).

Gelişmiş analiz portalını günlük araması portalı yerine, aramalar oluşturmak için bkz [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856587).  Her iki Portal, Log Analytics çalışma alanında aynı verilere erişmek için aynı sorgu dilini kullanın.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, Log Analytics çalışma ile verileri analiz etmek sorgular oluşturur ve bağlı en az bir kaynak zaten sahibi olduğunuzu varsayar.  

- Bir çalışma alanınız yoksa, ücretsiz bir yordamı kullanarak oluşturabileceğiniz [bir Log Analytics çalışma alanını kullanmaya başlama](log-analytics-get-started.md).
- En az bir bağlama [Windows Aracısı](log-analytics-windows-agent.md) veya bir [Linux Aracısı](log-analytics-linux-agents.md) çalışma alanına.  

## <a name="open-the-log-search-portal"></a>Günlük Araması portalını açma
Günlük Araması portalını açarak işleme başlayın. 

1. Azure portalı açın.
2. Log Analytics'e gidip çalışma alanınızı seçin.
3. **Günlükler**’i seçin.


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

## <a name="set-the-time-scope"></a>Zaman kapsamı ayarlayın
Log Analytics tarafından toplanan tüm kayıtların sahip bir **TimeGenerated** kaydın oluşturulduğu saat ve tarihi içeren özelliği.  Günlük araması portalı bir sorguda yalnızca kayıtlarla döndürür bir **TimeGenerated** zaman kapsamında ekranın sol tarafında görüntülenir.  

Süresi filtre açılır menüyü seçerek veya kaydırıcıyı değiştirerek değiştirebilirsiniz.  Kaydırıcı kayıt her zaman bir segment aralığı içinde göreli sayısını gösteren bir çubuk grafik görüntüler.  Bu kesim aralığı bağlı olarak değişir.

Varsayılan zaman kapsamı **1 gün**.  Bu değeri değiştirmek **7 gün**, ve kayıt toplam sayısını artırmalısınız.

![Tarih zaman kapsamı](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Sorgu sonuçlarını filtreleme
Ekranın sol tarafında, doğrudan değişiklik yapmadan sorguya filtreleme eklemenize olanak tanıyan bir filtre bölmesi mevcuttur.  Döndürülen kayıtları çeşitli özellikleri, kullanıcıların kayıt sayısı ile en iyi on değerleriyle görüntülenir.

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

Yalnızca **filtre** mavi adında özelliklerle seçeneği.  Bunlar, arama koşulları için dizini oluşturulmuş *aranabilir* alanlardır.  Gri alanlar yalnızca **Başvuruları göster** seçeneğinin bulunduğu *serbest metin aranabilir* alanlardır.  Bu seçenek, herhangi bir özellikte söz konusu değere sahip kayıtları döndürür.

![Filtre menüsü](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Kayıt menüsünde **Gruplandırma ölçütü** seçeneğini belirleyerek tek bir özellikteki sonuçları gruplandırabilirsiniz.  Bu işlem, sorgunuza sonuçları bir grafikte görüntüleyen bir [özetleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) işleci ekler.  Birden fazla özelliği gruplandırabilirsiniz, ancak sorguyu doğrudan düzenlemeniz gerekir.  **Bilgisayar** özelliğinin yanındaki kayıt menüsünü seçin ve **'Bilgisayara' Göre Gruplandır** seçeneğini belirleyin.  

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
Hem Windows hem de Linux aracıları için performans verileri depolanan **Perf** tablosundaki Log Analytics çalışma alanında depolanmaktadır.  Performans kayıtları diğer kayıtlar gibi görünür ve tüm performans kayıtlarını şimdi olaylarda olduğu gibi döndüren basit bir sorgu yazabiliriz.

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

- Log Analytics sorgu dili hakkında daha fazla bilgi [Analytics portalı ile çalışmaya başlama](https://go.microsoft.com/fwlink/?linkid=856079).
- Bir öğretici kullanarak yol [Gelişmiş analiz portalını](https://go.microsoft.com/fwlink/?linkid=856587) aynı sorguları çalıştırmak ve günlük araması portalını aynı verilere erişmesine olanak tanıyan.
