---
title: "Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Scheduler ile derleme"
description: "Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Scheduler ile oluşturmayı öğrenin."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 4293442e13fc4bae871b1f32a3ed4231d9f32632
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Scheduler ile derleme

Azure Scheduler işi çekirdek zamanlamadır. Zamanlamayı nasıl ve ne zaman Zamanlayıcı İş yürütür belirler. 

Bir iş için birden çok tek seferlik ve yinelenen zamanlama ayarlamak için Zamanlayıcı'yı kullanabilirsiniz. Tek seferlik zamanlamaları belirli bir zamanda bir kez kov. Tek seferlik zamanlama etkili bir şekilde yalnızca bir kez yürütme yinelenen zamanlamalar. Önceden belirlenmiş bir sıklık yinelenen zamanlamalar yangın.

Bu esneklik ile çok çeşitli iş senaryoları için Zamanlayıcı'yı kullanabilirsiniz:

* **Düzenli veri temizleme**. Örneğin, günde üç aydan daha eski olan tüm tweet'leri silin.
* **Arşivleme**. Örneğin, her ay, yedekleme hizmetine itme Fatura geçmişi.
* **Dış veri istekleri**. Örneğin, her 15 dakikada bir yeni skı hava durumu raporu NOAA çeker.
* **Görüntü işleme**. Örneğin, her hafta içi günü, yoğun olmayan saatlerde o gün yüklenen görüntüleri sıkıştırmak için bulut kullanın.

Bu makalede, Zamanlayıcısı'nı kullanarak oluşturabileceğiniz örnek işleri size rehberlik eder. Her zamanlamayı açıklar JSON verilerini sunuyoruz. Kullanırsanız [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), bu aynı JSON kullanabilirsiniz [Scheduler işi oluşturma](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Bu makaledeki örnekler Zamanlayıcı destekleyen senaryoları derecesini gösterir. Örneklerde kapsamlı dahil olmak üzere birçok davranış desenler için zamanlama oluşturmak nasıl gösterilmektedir:

* Belirli bir tarih ve saatte bir kez gerçekleştirilir.
* Belirli bir sayıda yinelenmesini ve çalıştırın.
* Hemen çalıştırmak ve yineleme.
* Çalıştırın ve tekrarlamayı her  *n*  dakika, saat, gün, hafta veya ay, belirli bir zamanda başlatılıyor.
* Haftalık veya aylık sıklığı, ancak yalnızca haftanın belirli günlerinde veya ayın belirli günlerinde yinelenmesini ve çalıştırın.
* Çalıştırın ve bir süre içinde birden çok kez yinelenmesini. Örneğin, son Cuma günü ve her ayın veya 5: 15'da ve 17:15:00 saatleri her gün en son Pazartesi.

## <a name="date-and-date-time"></a>Tarih ve tarih-saat
Tarih zamanlayıcı işleri izleme başvuruları [ISO 8601 belirtimi](http://en.wikipedia.org/wiki/ISO_8601)ve yalnızca tarihi içerir.

Zamanlayıcı işlerinin tarih-saat başvurularında izleyin [ISO 8601 belirtimi](http://en.wikipedia.org/wiki/ISO_8601)ve tarihi ve saati içerir. UTC uzaklığı belirtmeyen bir tarih-saat UTC olduğu varsayılır.  

## <a name="use-json-and-the-rest-api-to-create-a-schedule"></a>Bir zamanlama oluşturmak için JSON ve REST API kullanın
Kullanarak basit bir zamanlama oluşturmak için [Scheduler REST API](https://msdn.microsoft.com/library/mt629143), ilk [aboneliğinizi kayıt kaynak sağlayıcısı ile](https://msdn.microsoft.com/library/azure/dn790548.aspx). Zamanlayıcı için sağlayıcı adı **Microsoft.Scheduler**. Ardından, [iş koleksiyonu oluşturma](https://msdn.microsoft.com/library/mt629159.aspx). Son olarak, [bir iş oluşturmak](https://msdn.microsoft.com/library/mt629145.aspx). 

Bir proje oluşturduğunuzda, JSON, gibi bu alıntı kullanarak zamanlama ve yinelenme belirtebilirsiniz:

    {
        "startTime": "2012-08-04T00:00Z", // Optional
         …
        "recurrence":                     // Optional
        {
            "frequency": "week",     // Can be "year", "month", "day", "week", "hour", or "minute"
            "interval": 1,                // How often to fire
            "schedule":                   // Optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // Optional (default to recur infinitely)
            "endTime": "2012-11-04",      // Optional (default to recur infinitely)
        },
        …
    }

## <a name="job-schema-basics"></a>İş şema temelleri
Aşağıdaki tabloda, bir işin yinelenme ve zamanlama ayarlamak için kullanın önemli öğeleri üst düzey bir genel bakış sağlar:

| JSON adı | Açıklama |
|:--- |:--- |
| **startTime** |Bir tarih-saat değeri. Temel tabloları için **startTime** ilk geçtiği. Karmaşık zamanlamalar için daha önce iş başlatır **startTime**. |
| **recurrence** |İş ve iş çalıştığı yineleme yineleme kurallarını belirtir. Yinelenme nesnesi öğeleri destekler **sıklığı**, **aralığı**, **endTime**, **sayısı**, ve **zamanlama**. Varsa **yineleme** tanımlanan **sıklığı** gereklidir. Diğer **yineleme** öğeler isteğe bağlıdır. |
| **frequency** |İşin yineleneceği sıklığı birim temsil eden bir dize. Desteklenen değerler "dakika", "saat", "gün", "hafta" ve "ay" dir. |
| **interval** |Pozitif bir tamsayı. **aralığı** için bir aralığı gösterir **sıklığı** ne sıklıkta işi çalıştırır belirleyen bir değer. Örneğin, varsa **aralığı** 3 ve **sıklığı** "hafta", her üç hafta iş yinelenen.<br /><br />Zamanlayıcı'yı destekleyen en **aralığı** 18 aylık sıklık, 78 haftalık sıklığı ve günlük sıklığı için 548. Saat ve dakika sıklığı için desteklenen aralık 1. < = **aralığı** < = 1000. |
| **endTime** |Sonrasında iş çalışmıyor tarih-saat belirten bir dize. İçin bir değer ayarlayabilirsiniz **endTime** geçmişte olmasıdır. Varsa **endTime** ve **sayısı** olmayan belirtilen işi çalıştırır sonsuz. Her ikisini birden içeremez **endTime** ve **sayısı** aynı işi. |
| **Sayısı** |Tamamlanmadan önce kaç kez belirten pozitif bir tamsayı (sıfırdan büyük) işi çalıştırır.<br /><br />**Count** iş belirlenmeye tamamlandı önce çalışır sayısını temsil eder. Örneğin, günlük ile çalıştırılan bir iş için bir **sayısı** 5 ve bir başlangıç tarihini Pazartesi, Cuma günü yürütme sonra işi tamamlar. Başlangıç tarihi geçmişte ise, ilk yürütme oluşturma saati hesaplanır.<br /><br />Öyle değilse **endTime** veya **sayısı** belirtilirse, işi çalıştırır sonsuz. Her ikisini birden içeremez **endTime** ve **sayısı** aynı işi. |
| **schedule** |Bir iş belirtilen sıklık yinelenme zamanlamaya göre kendi yinelenme değiştirir. A **zamanlama** değer dakika, saat, haftanın günü, ay gün ve hafta numarasını göre değişiklikler içeriyor. |

## <a name="job-schema-defaults-limits-and-examples"></a>İş şema Varsayılanları, sınırlar ve örnekler
Makalenin sonraki bölümlerinde ayrıntılı aşağıdaki öğelerin her birini aşağıdakiler ele:

| JSON adı | Değer türü | Gerekli mi? | Varsayılan değer | Geçerli değerler | Örnek |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** |string |Hayır |Hiçbiri |ISO 8601 tarih saatleri |`"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** |nesne |Hayır |None |Yinelenme nesnesi |`"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **frequency** |string |Evet |Hiçbiri |"dakika", "saat", "gün", "hafta", "ay" |`"frequency" : "hour"` |
| **interval** |numarası |Evet |None |1 ile 1000 |`"interval":10` |
| **endTime** |string |Hayır |Hiçbiri |Gelecekteki bir zamanı temsil eden bir tarih saat değeri |`"endTime" : "2013-02-09T09:30:00-08:00"` |
| **Sayısı** |numarası |Hayır |None |>= 1 |`"count": 5` |
| **schedule** |nesne |Hayır |None |Zamanlamaya nesnesi |`"schedule" : { "minute" : [30], "hour" : [8,17] }` |

## <a name="deep-dive-starttime"></a>Ayrıntılı Bakış: startTime
Aşağıdaki tabloda açıklanmaktadır nasıl **startTime** bir işi şekilde denetler:

| startTime değeri | Yineleme yok | Yineleme, herhangi bir zamanlaması | Zamanlama ile yinelenme |
|:--- |:--- |:--- |:--- |
| **Başlangıç saati yok** |Hemen bir kez çalıştırın. |Hemen bir kez çalıştırın. Son Yürütme saati hesaplanan sonraki yürütmelerde çalıştırın. |Hemen bir kez çalıştırın.<br /><br />Sonraki yürütmelerde yineleme zamanlamaya göre çalıştır. |
| **Başlangıç zamanı geçmişte** |Hemen bir kez çalıştırın. |Başlangıç zamanından sonra ilk sonraki yürütme zamanını hesaplamak ve o anda çalıştırın.<br /><br />Son Yürütme saati hesaplanan sonraki yürütmelerde çalıştırın. <br /><br />Daha fazla bilgi için bu tabloda aşağıdaki örneğe bakın. |İş başlatır *Hayır sooner daha* belirtilen başlangıç saati. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir.<br /><br />Sonraki yürütmelerde yineleme zamanlamaya göre çalıştır. |
| **Başlangıç zamanı gelecekte veya geçerli saati** |Belirtilen başlangıç zamanda bir kez çalıştırın. |Belirtilen başlangıç zamanda bir kez çalıştırın.<br /><br />Son Yürütme saati hesaplanan sonraki yürütmelerde çalıştırın.|İş başlatır *Hayır sooner daha* belirtilen başlangıç saati. Birinci başlangıç saati hesaplanan zamanlama, temel alır.<br /><br />Sonraki yürütmelerde yineleme zamanlamaya göre çalıştır. |

Neler bir örneğe bakalım zaman **startTime** geçmişte, yineleme ile ancak herhangi bir zamanlaması olan.  Geçerli saati 2015-04-08 olduğunu varsayın 13:00, **startTime** 2015-04-07 14:00 ve **yineleme** her iki gün (ile tanımlanmış **sıklığı**: gün ve **aralığı**: 2.) Unutmayın **startTime** geçmişte ve geçerli saatinden önce.

Bu koşullar altında 2015-04-09 14:00\ adresindeki üzerinde ilk kez yürütülmesi olacaktır. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar. Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır. Bu durumda, **startTime** 2015-04-07 2: 00'da, olduğundan, 2015-04-09 2: 00'da bu süre, iki gün sonraki örneğidir.

İlk yürütme aynı olacaktır olup olmadığını **startTime** 2015-04-05 olduğu 14:00 veya 2015-04-01 14:00\. İlk yürütme sonrasındaki yürütmeler zamanlama kullanılarak hesaplanır. Bunlar 2015-04-11 2:00 PM adresindeki sonra 2015-04-13 2: 00'dan en sonra 2015-04-15 2:00 PM konumunda olması ve benzeri.

Saat ve dakika zamanlamada, saat ve dakika ilk yürütme değerleri varsayılan sırasıyla ayarlanmazsa son olarak, ne zaman bir iş bir zamanlamaya sahiptir.

## <a name="deep-dive-schedule"></a>Derin Dalış: zamanlama
Kullanabileceğiniz **zamanlama** için *sınırı* iş yürütmeleri sayısı. Örneğin, bir işin durumunda bir **sıklığı** "ayın" yalnızca 31 günde çalışan bir zamanlama sahipse, iş yalnızca ilk otuz gün içeren aylarda çalıştırılır.

Aynı zamanda **zamanlama** için *genişletin* iş yürütmeleri sayısı. Örneğin, bir işin durumunda bir **sıklığı** "ayın" ayın günleri 1 ve 2 üzerinde çalışan bir zamanlama varsa, bir ay yerine yalnızca bir kez ayın birinci ve ikinci günlerde işi çalıştırır.

Birden çok zamanlama öğesi belirtirseniz, değerlendirme büyükten sırasıdır küçüğüne: hafta sayısı, ay gün, haftanın günü, saat ve dakika.

Aşağıdaki tabloda schedule öğeleri ayrıntılı bir şekilde açıklanmıştır:

| JSON adı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **minutes** |İşin çalıştırıldığı saat dakika. |Dizisi. |
| **hours** |İşin çalışacağı günün saati. |Dizisi. |
| **weekDays** |İşi çalıştırır haftanın günleri. Yalnızca haftalık sıklığıyla belirtilebilir. |Bir dizi (en büyük dizi boyutu 7'dir) aşağıdaki değerlerden herhangi birini:<br />-"Pazartesi"<br />-"Salı"<br />-"Çarşamba"<br />-"Perşembe"<br />-"Cuma"<br />-"Cumartesi"<br />-"Pazar"<br /><br />Duyarlı değildir. |
| **monthlyOccurrences** |Ayın hangi günleri iş çalışacağını belirler. Yalnızca bir aylık sıklığı ile belirtilebilir. |Bir dizi **monthlyOccurrences** nesneler:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **gün** işi çalışır, haftanın günü. Örneğin, *{Pazar}* ayın her Pazar ise. Gereklidir.<br /><br />**Geçişi** gün ay sırasında oluşum. Örneğin, *{Pazar, -1}* ayın son Pazar ise. İsteğe bağlı. |
| **monthDays** |Ayın işi çalıştırır. Yalnızca bir aylık sıklığı ile belirtilebilir. |Aşağıdaki değerleri dizisi:<br />-Herhangi bir değer < = -1 ve >-31 =<br />-Herhangi bir değer > = 1 ve < = 31|

## <a name="examples-recurrence-schedules"></a>Örnekler: Yineleme zamanlamaları
Aşağıdaki örnekler, çeşitli yineleme zamanlamaları gösterir. Zamanlamaya nesnesi ve onun alt örnekler odaklanır.

Bu zamanlamaları varsayımında **aralığı** 1 olarak ayarlayın\. Örnekler de doğru varsayar **sıklığı** değerlerinin değerleri **zamanlama**. Örneğin, kullanamazsınız bir **sıklığı** "gün" ve bir **monthDays** değişiklik **zamanlama**. Biz bu kısıtlamaları makalenin açıklanmaktadır.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` |5'te her gün çalıştırın.<br /><br />Zamanlayıcı her değeri "dakika" yer alan her değerin, tek tek, her zaman, proje çalışır listesini oluşturmak için "saat" eşleşir. |
| `{"minutes":[15], "hours":[5]}` |Her gün 05.15'te çalıştır. |
| `{"minutes":[15], "hours":[5,17]}` |Her gün 05.15 ve 17.15’te çalıştır. |
| `{"minutes":[15,45], "hours":[5,17]}` |Her gün 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |
| `{"minutes":[0,15,30,45]}` |15 dakikada bir çalıştır. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Saatte bir çalıştır.<br /><br />Bu iş saatte çalışır. Dakika değeri tarafından denetlenen **startTime**, belirtilmişse. Öyle değilse **startTime** değeri belirtilmişse, dakikayı oluşturulma tarihine göre denetlenir. Örneğin, iş başlangıç zamanı veya oluşturma zamanı (hangisi) saat 12: 25'e ise, 00:25 ', 01:25, çalışır 02:25,..., 23:25.<br /><br />Zamanlama bir iş ile eşdeğer olan bir **sıklığı** "saat", bir **aralığı** 1 ve Hayır **zamanlama** değeri. Bu zamanlama ile farklı kullanabileceğinizi farktır **sıklığı** ve **aralığı** diğer işleri oluşturmak için değerleri. Örneğin, varsa **sıklığı** ayda her gün yerine yalnızca bir kez zamanlama çalışır "ay" olan (varsa **sıklığı** "gün" olan). |
| `{minutes:[0]}` |Her saat başı çalıştır.<br /><br />Bu işlemi de her saat çalışır ancak saat (12 AM, AM 1, 2 AM ve benzeri). Bu bir iş ile eşdeğer olan bir **sıklığı** "saat", bir **startTime** değeri sıfır dakika ve Hayır **zamanlama**, sıklığı "gün" ise. Ancak, varsa **sıklığı** olan "hafta" veya "ay" zamanlama yalnızca bir gününü bir hafta veya ayda bir bir gün sırasıyla yürütür. |
| `{"minutes":[15]}` |Her saat saati aşan 15 dakika çalıştırılacak.<br /><br />00: 15'de, 1: 15'da, 2: 15'da, başlangıç her saat çalışır ve benzeri. 23:15:00 sonlandırır. |
| `{"hours":[17], "weekDays":["saturday"]}` |Haftada 5 PM Cumartesi çalıştırmanıza. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma 18: 00 çalıştırmanıza. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.15 ve 17.45'te çalıştır. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |5 AM ve 17: 00 saatleri Pazartesi, Çarşamba ve Cuma her hafta çalıştırın. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM Pazartesi, Çarşamba ve Cuma her hafta. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Haftanın her günü 15 dakikada bir çalıştır. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |09: 00 ve 4:45 PM arasında hafta içi günlerde her 15 dakikada çalıştırın. |
| `{"weekDays":["sunday"]}` |Pazar günleri başlama saatinde Çalıştır. |
| `{"weekDays":["tuesday", "thursday"]}` |Salı ve Perşembe günleri saat başlama saatinde Çalıştır. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Her ayın yirmi sekizinci günde 6 sabah çalıştırın (varsayarak bir **sıklığı** "ay"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Ayın son gününde 6 sabah çalıştırın.<br /><br />Bir işi ayın son gününde çalıştırmak istiyorsanız, -1 28, 29, 30 ve 31 gün yerine kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Her ayın ilk ve son gününde 6 sabah çalıştırın. |
| `{monthDays":[1,-1]}` |Başlangıç saatinde her ayın ilk ve son günü çalıştırın. |
| `{monthDays":[1,14]}` |Başlangıç saatinde her ayın ilk ve On dördüncü günü çalıştırın. |
| `{monthDays":[2]}` |Başlangıç saatinde ayın ikinci günü çalıştırın. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |5 sabah her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Başlangıç saatinde her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Başlangıç saatinde her ay ay sonundan üçüncü Cuma çalıştırın. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Her ayın ilk ve son Cuma günü saat 05.15'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |İlk ve son Cuma her ayın başlama saatinde Çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Başlangıç saatinde her ayın beşinci Cuma çalıştırın.<br /><br />Bir ay içinde hiçbir beşinci Cuma ise, iş çalıştırmaz. İş son gerçekleşen üzerinde çalıştırmak istiyorsanız -1 5 yerine örneği için kullanmayı düşünebilirsiniz Cuma günü. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Ayın son Cuma günü 15 dakikada bir çalıştır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Her ayın üçüncü Çarşamba günü 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |

## <a name="see-also"></a>Ayrıca bkz.

- [Scheduler nedir?](scheduler-intro.md)
- [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
- [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)
- [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)
- [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)
- [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
- [Azure Scheduler yüksek kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)
- [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
- [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

