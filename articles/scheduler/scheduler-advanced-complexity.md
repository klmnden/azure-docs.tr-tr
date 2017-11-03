---
title: "Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Zamanlayıcı ile oluşturma"
description: "Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Zamanlayıcı ile oluşturma"
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
ms.openlocfilehash: 20c3e3c1cb85308cad47054c2efa87f61cae0f22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-build-complex-schedules-and-advanced-recurrence-with-azure-scheduler"></a>Karmaşık zamanlamalar ve Gelişmiş yineleme Azure Zamanlayıcı ile oluşturma
## <a name="overview"></a>Genel Bakış
Azure Scheduler Kalp iş *zamanlama*. Zamanlamayı nasıl ve ne zaman Zamanlayıcı İş yürütür belirler.

Azure Zamanlayıcı bir iş için farklı bir kerelik ve yinelenen zamanlamalar belirtmenize olanak tanır. *Tek seferlik* zamanlamaları yangın bir belirtilen zamanda bir kez – etkili bir şekilde, bunlar *yinelenen* yalnızca bir kez yürütme zamanlamaları. Önceden belirlenmiş bir sıklık yinelenen zamanlamalar yangın.

Bu esneklik ile Azure Scheduler, çok çeşitli iş senaryolarını desteklemek olanak sağlar:

* Düzenli veri temizleme – örn., her gün 3 aydan daha eski tüm tweet'leri Sil
* Arşivleme – örn., her ay itme Fatura geçmişi yedekleme hizmeti
* Dış veri – istekleri her 15 dakikada örn., NOAA yeni skı hava durumu raporu isteme
* Görüntü işleme – örneğin her hafta içi günü, yoğun olmayan saatlere, görüntüleri sıkıştırmak için o gün karşıya bulut kullanın

Bu makalede, Azure Scheduler ile oluşturabileceğiniz örnek işleri size rehberlik eder. Her zamanlamayı açıklar JSON verilerini sunuyoruz. Kullanırsanız [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx), bu aynı JSON için kullanabileceğiniz [Azure Scheduler işi oluşturma](https://msdn.microsoft.com/library/mt629145.aspx).

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Bu konudaki birçok örnekler Azure Scheduler destekleyen senaryoları derecesini gösterir. Genel anlamıyla Bu örneklerde olanlar da dahil olmak üzere birçok davranış desenler için zamanlama oluşturmak nasıl gösterilmektedir:

* Belirli tarih ve saatte bir kez çalıştır
* Çalıştırın ve açık sayısı Yinele
* Hemen çalıştırmak ve Yinele
* Çalıştırın ve tekrarlamayı her  *n*  dakika, saat, gün, hafta veya ay, belirli bir zamanda başlatılıyor
* Çalıştırın ve haftalık veya aylık sıklığı ancak yalnızca belirli gün, haftanın belirli gün veya ayın belirli günlerine Yinele
* Çalıştırın ve birden çok kez – örn., son Cuma ve Pazartesi her ayın ya da 5: 15'da ve 17:15:00 saatleri her gün bir süre içinde adresindeki Yinele

## <a name="dates-and-datetimes"></a>Tarihleri ve tarih/saat
Azure zamanlayıcı işlerinin tarihleri izleyin [ISO 8601 belirtimi](http://en.wikipedia.org/wiki/ISO_8601) ve yalnızca tarihi içerir.

Azure zamanlayıcı işlerinin tarih-saat başvurularında izleyin [ISO 8601 belirtimi](http://en.wikipedia.org/wiki/ISO_8601) ve tarih ve saat bölümleri içerir. UTC uzaklığı belirtmiyor bir tarih-saat UTC olduğu varsayılır.  

## <a name="how-to-use-json-and-rest-api-for-creating-schedules"></a>Nasıl yapılır: Zamanlama oluşturmak için JSON ve REST API kullanma
Kullanarak bir basit zamanlama oluşturmak için [Azure Scheduler REST API](https://msdn.microsoft.com/library/mt629143), ilk [aboneliğinizi kayıt kaynak sağlayıcısı ile](https://msdn.microsoft.com/library/azure/dn790548.aspx) (Zamanlayıcı sağlayıcı adı *Microsoft.Scheduler* ), ardından [iş koleksiyonu oluşturma](https://msdn.microsoft.com/library/mt629159.aspx)ve son olarak [bir iş oluşturmak](https://msdn.microsoft.com/library/mt629145.aspx). Bir proje oluşturduğunuzda, zamanlama ve yinelenme aşağıda adlı çalışmasından bir gibi JSON kullanarak belirtebilirsiniz:

    {
        "startTime": "2012-08-04T00:00Z", // optional
         …
        "recurrence":                     // optional
        {
            "frequency": "week",     // can be "year" "month" "day" "week" "hour" "minute"
            "interval": 1,                // optional, how often to fire (default to 1)
            "schedule":                   // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]                      
            },
            "count": 10,                  // optional (default to recur infinitely)
            "endTime": "2012-11-04",      // optional (default to recur infinitely)
        },
        …
    }

## <a name="overview-job-schema-basics"></a>Genel Bakış: İş şema temelleri
Aşağıdaki tabloda bir işi zamanlama ve yineleme için ilgili temel öğe üst düzey bir genel bakış sağlar:

| **JSON adı** | **Açıklama** |
|:--- |:--- |
| ***startTime*** |*startTime* bir tarih-saat değil. Basit tabloları için *startTime* ilk geçtiği ve karmaşık zamanlamalar için daha önce işin başlayacağı *startTime*. |
| ***Yineleme*** |*Yineleme* nesne işin Yinelenme kuralları belirtir ve yineleme iş ile çalıştırır. Yinelenme nesnesi öğeleri destekler *sıklığı aralığını, endTime, count,* ve *zamanlama*. Varsa *yineleme* tanımlanan *sıklığı* gereklidir; diğer öğeleri *yineleme* isteğe bağlıdır. |
| ***Sıklık*** |*Sıklığı* işin yineleneceği sıklığı birim temsil eden dize. Desteklenen değerler *"dakika", "saat", "gün", "hafta",* veya *"ay."* |
| ***aralığı*** |*Aralığı* pozitif bir tamsayı olan ve aralığını gösterir *sıklığı* , işin ne sıklıkla çalışacağını belirler. Örneğin, varsa *aralığı* 3 ve *sıklığı* "hafta", 3 haftada iş yinelenen. Azure Zamanlayıcı destekleyen en *aralığı* aylık sıklık haftalık sıklığı için 78 haftalık veya günlük sıklığı 548 gün 18 ay sayısı. Saat ve dakika sıklığı için desteklenen aralık 1. < = *aralığı* < = 1000. |
| ***endTime*** |*EndTime* dize iş değil yürütülecek tarih-saat geçmiş belirtir. İçin geçerli değil bir *endTime* geçmişte. Öyle değilse *endTime* veya sayı belirtilirse, iş sonsuz çalıştırılır. Her ikisi de *endTime* ve *sayısı* aynı işi eklenemez. |
| ***sayısı*** |<p>*Sayısı* bu işlemi gerçekleştirmeden önce çalışmalı sayısını belirten bir pozitif tamsayı (sıfırdan büyük).</p><p>*Sayısı* işi çalıştırır tamamlandı olarak belirlenen önce sayısını temsil eder. Örneğin, günlük ile çalıştırılan bir iş için *sayısı* 5 ve başlangıç tarihi Pazartesi, Cuma günü yürütme sonra işi tamamlar. Başlangıç tarihi geçmişte ise, ilk yürütme oluşturma saati hesaplanır.</p><p>Öyle değilse *endTime* veya *sayısı* belirtilirse, işi çalıştırır sonsuz. Her ikisi de *endTime* ve *sayısı* aynı işi eklenemez.</p> |
| ***zamanlama*** |Bir iş belirtilen sıklık yinelenme zamanlamaya göre kendi yinelenme değiştirir. A *zamanlama* dakika, saat, haftanın günü, ay gün ve hafta numarasını göre değişiklikleri içerir. |

## <a name="overview-job-schema-defaults-limits-and-examples"></a>Genel Bakış: İş şema Varsayılanları, sınırlar ve örnekler
Bu genel bakışta sonra şimdi ayrıntılı bu öğelerin her biri açıklanmaktadır.

| **JSON adı** | **Değer türü** | **Gerekli?** | **Varsayılan değer** | **Geçerli değerler** | **Örnek** |
|:--- |:--- |:--- |:--- |:--- |:--- |
| ***startTime*** |Dize |Hayır |None |ISO-8601 Tarih-Saatleri |<code>"startTime" : "2013-01-09T09:30:00-08:00"</code> |
| ***Yineleme*** |Nesne |Hayır |None |Yinelenme nesnesi |<code>"recurrence" : { "frequency" : "monthly", "interval" : 1 }</code> |
| ***Sıklık*** |Dize |Evet |None |"dakika", "saat", "gün", "hafta", "ay" |<code>"frequency" : "hour"</code> |
| ***aralığı*** |Sayı |Hayır |1 |1-1000 arası. |<code>"interval":10</code> |
| ***endTime*** |Dize |Hayır |None |Gelecekteki bir zamanı temsil eden Tarih-Saat değeri |<code>"endTime" : "2013-02-09T09:30:00-08:00"</code> |
| ***sayısı*** |Sayı |Hayır |None |>= 1 |<code>"count": 5</code> |
| ***zamanlama*** |Nesne |Hayır |None |Zamanlama nesnesi |<code>"schedule" : { "minute" : [30], "hour" : [8,17] }</code> |

## <a name="deep-dive-starttime"></a>Derin Dalış: *startTime*
Aşağıdaki tabloda yakalamaları nasıl *startTime* bir işlemin nasıl yürütüleceğini denetler.

| **startTime değeri** | **Yineleme yok** | **Yineleme. Hiçbir zamanlama** | **Zamanlama ile yineleme** |
|:--- |:--- |:--- |:--- |
| **Başlangıç saati yok** |Hemen bir kez çalıştır |Hemen bir kez çalıştırın. Sonraki yürütmelerde son yürütme saati hesaplamaya göre Çalıştır |<p>Hemen bir kez çalıştır</p><p>Sonraki çalıştırmaları yinelenme zamanlamasına göre gerçekleştirir</p> |
| **Başlangıç zamanı geçmişte** |Hemen bir kez çalıştır |<p>İlk gelecekteki yürütme zamanı başlangıç zamanından sonra hesaplamak ve o anda çalıştırın</p><p>Son Yürütme saati göre sonraki yürütmelerde oncalculating çalıştırın</p><p>Daha ayrıntılı bir açıklama için bu tablodan sonra örnek bakın</p> |<p>İş başlatır *Hayır sooner daha* belirtilen başlangıç saati. Birinci başlangıç saati hesaplanan zamanlama bağlıdır</p><p>Sonraki çalıştırmaları yinelenme zamanlamasına göre gerçekleştirir</p> |
| **Başlangıç zamanı gelecekte veya mevcut** |Bir kez belirtilen başlama saatinde Çalıştır |<p>Bir kez belirtilen başlama saatinde Çalıştır</p><p>Sonraki yürütmelerde son yürütme saati hesaplamaya göre Çalıştır</p> |<p>İş başlatır *Hayır sooner daha* belirtilen başlangıç saati. Birinci başlangıç saati hesaplanan zamanlama bağlıdır</p><p>Sonraki çalıştırmaları yinelenme zamanlamasına göre gerçekleştirir</p> |

Neler bir örnek görelim nerede *startTime* geçmişte olan *yineleme* ancak hiçbir *zamanlama*.  Geçerli saati 2015-04-08 olduğunu varsayın 13:00, *startTime* 2015-04-07 14:00 ve *yineleme* 2 her gün (ile tanımlanmış *sıklığı*: gün ve *aralığı*: 2.) Unutmayın *startTime* geçmişte ve geçerli saatinden önce

Bu koşullar altında *ilk yürütme* 2015-04-09 14: 00'dan olacaktır\. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar.  Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır.  Bu durumda, *startTime* 2: 00'dan 2015-04-09 olan bu süre, 2 gün sonraki örneğidir 2: 00'da, 2015-04-07 olduğundan.

İlk kez yürütülmesi gerekir Not 2015-04-05 startTime olsa bile aynı olması 14:00 veya 2015-04-01 14:00\. İlk yürütme sonrasında sonraki yürütmelerde bunlar 2015-04-11 2:00 pm, ardından 2015-04-13 2:00 pm adresindeki en sonra 2015-04-15 2:00 pm adresindeki durumda olacaktır zamanlanmış – kullanılarak hesaplanan vs.

Bir iş saatleri ve/veya dakika zamanlamada ayarlanmazsa bir zamanlama olduğunda, son olarak, bunlar saatleri ve/veya ilk yürütme dakika sırasıyla varsayılan.

## <a name="deep-dive-schedule"></a>Derin Dalış: *zamanlama*
Bir yandan, bir *zamanlama* için *sınırı* iş yürütmeleri sayısı.  Örneğin, bir iş ile bir "ay" sıklığı sahipse bir *zamanlama* yalnızca 31 gün çalışır, işin bir 31 olan bu aylarda çalıştırılır<sup>st</sup> gün.

Öte yandan, bir *zamanlama* ayrıca *genişletin* iş yürütmeleri sayısı. Örneğin, bir iş ile bir "ay" sıklığı sahipse bir *zamanlama* ayın günleri 1 ve 2 çalışır, 1'işi çalıştırır<sup>st</sup> ve 2<sup>nd</sup> yerine yalnızca ayda bir ayın günleri.

Birden çok zamanlama öğesi belirtilmişse, Değerlendirme sırasını büyükten olduğu için en küçük – hafta sayısı, ay gün, haftanın günü, saat ve dakika.

Aşağıdaki tabloda açıklanmaktadır *zamanlama* ayrıntılı öğeleri.

| **JSON adı** | **Açıklama** | **Geçerli değerler** |
|:--- |:--- |:--- |
| **dakika** |İşin çalışacağı saat, dakika |<ul><li>Tamsayı veya</li><li>Tamsayı dizisi</li></ul> |
| **saatleri** |İş çalışacağı günün saatleri |<ul><li>Tamsayı veya</li><li>Tamsayı dizisi</li></ul> |
| **Haftanın günü** |İşin çalışacağı günleri. Yalnızca weekly frequency değeri ile belirtilebilir. |<ul><li>"Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma", "Cumartesi" ve "Pazar"</li><li>Yukarıdaki değerlerden oluşan dizi (maksimum dizi boyutu: 7)</li></ul>*Değil* büyük küçük harfe duyarlı |
| **monthlyOccurrences** |Ayın hangi günleri iş çalışacağını belirler. Yalnızca monthly frequency değeri ile belirtilebilir. |<ul><li>MonthlyOccurrence nesneler dizisi:</li></ul> <pre>{ "day": *day*,<br />  "occurrence": *occurrence*<br />}</pre><p> *gün* olan iş haftanın günü çalıştırılır, örneğin her Pazar ayın {Pazar}. Gereklidir.</p><p>Oluşum *oluşumu* gün ay sırasında örneğin {Pazar, -1} ayın son Pazar ise. İsteğe bağlı.</p> |
| **monthDays** |Ayın iş çalıştırılır. Yalnızca monthly frequency değeri ile belirtilebilir. |<ul><li>Herhangi bir değer < = -1 ve >-31 =.</li><li>Herhangi bir değer > = 1 ve < = 31 açın.</li><li>Yukarıdaki değerlerden oluşan bir dizi</li></ul> |

## <a name="examples-recurrence-schedules"></a>Örnekler: Yineleme zamanlamaları
Yineleme zamanlamaları – zamanlamaya nesnesi ve alt öğelerini odaklanan çeşitli örnekleri verilmiştir.

Tüm aşağıda zamanlamaları varsayımında *aralığı* 1 olarak ayarlayın\. Ayrıca, bir sağ sıklığı nedir için uygun varsayın gerekir *zamanlama* – örn., biri olamaz sıklığı "gün" kullanıyorsanız ve "monthDays" değişiklik zamanlamada. Tür kısıtlamaları, yukarıda açıklanan.

| **Örnek** | **Açıklama** |
|:--- |:--- |
| <code>{"hours":[5]}</code> |5'te çalıştırmak her gün. Azure Zamanlayıcı her değeri "dakika" yer alan her değerin, tek tek, her zaman hangi, çalıştırılacak iş listesini oluşturmak için "saat" eşleşir. |
| <code>{"minutes":[15], "hours":[5]}</code> |5: 15'da çalıştırma her gün |
| <code>{"minutes":[15], "hours":[5,17]}</code> |5: 15'da ve 17:15:00 saatleri sırasında çalıştıran her gün |
| <code>{"minutes":[15,45], "hours":[5,17]}</code> |5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM her gün |
| <code>{"minutes":[0,15,30,45]}</code> |Her 15 dakikada çalıştırın |
| <code>{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}</code> |Her saat çalıştırın. Bu iş saatte çalışır. Dakikayı tarafından denetlenen *startTime*, belirtilmişse veya, oluşturulma tarihine göre belirtilmemişse. Başlangıç saati veya oluşturma zamanı (hangisi) saat 12: 25'e ise, örneğin, iş 00:25 ', 01:25, çalıştırılacak 02:25,..., 23:25. Zamanlama bir işlemle zorunda eşdeğerdir *sıklığı* "saat", bir *aralığı* 1 ve Hayır *zamanlama*. Bu zamanlama ile farklı kullanılabilir olduğunu farktır *sıklığı* ve *aralığı* diğer işleri çok oluşturmak için. Örneğin, varsa *sıklığı* "ay" olan, zamanlama her gün yerine ayda yalnızca bir kez çalışır *sıklığı* "gün" olan |
| <code>{minutes:[0]}</code> |Her saat, saat üzerinde çalıştırın. Bu işlemi de her saat çalışır ancak saat (örn: 00: 00, 1 AM, 2: 00, vs.) Bu sıklığı olan "gün", ancak sıklığı "hafta" veya "ay" olsaydı, zamanlama yalnızca bir gününü bir hafta veya ayda bir bir gün yürütülür sırasıyla "saat" sıklığını işlemiyle, startTime sıfır dakika ve herhangi bir zamanlaması ile eşdeğerdir. |
| <code>{"minutes":[15]}</code> |15 dakika olarak çalıştır saati aşan her saat. 00:15'ten başlayarak 01:15, 02:15 gibi saatte bir çalışır ve 22:15 ile 23:15'te biter. |
| <code>{"hours":[17], "weekDays":["saturday"]}</code> |5 PM Cumartesi günleri çalışan her hafta |
| <code>{hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |5 PM Pazartesi, Çarşamba ve Cuma çalışan her hafta |
| <code>{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}</code> |17:15:00 saatleri ve 5:45 PM Pazartesi, Çarşamba ve Cuma çalışan her hafta |
| <code>{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |5 AM ve 17: 00 saatleri Pazartesi, Çarşamba ve Cuma çalışan her hafta |
| <code>{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}</code> |5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM Pazartesi, Çarşamba ve Cuma her hafta |
| <code>{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |Haftanın her günü 15 dakikada bir çalışır |
| <code>{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}</code> |09: 00 ve 4:45 PM her 15 dakikada günlerinde Çalıştır |
| <code>{"weekDays":["sunday"]}</code> |Başlangıç saatinde Pazar çalıştırın |
| <code>{"weekDays":["tuesday", "thursday"]}</code> |Salı ve Perşembe başlangıç saatinde Çalıştır |
| <code>{"minutes":[0], "hours":[6], "monthDays":[28]}</code> |6'da 28 gün her (ay sıklığını varsayılarak) ayı üzerinde çalıştırın |
| <code>{"minutes":[0], "hours":[6], "monthDays":[-1]}</code> |Ayın son gününde 6 sabah çalıştırın. Bir işi ayın son gününde çalıştırmak istiyorsanız, -1 28, 29, 30 ve 31 gün yerine kullanın. |
| <code>{"minutes":[0], "hours":[6], "monthDays":[1,-1]}</code> |6'da, her ayın ilk ve son gününde Çalıştır |
| <code>{monthDays":[1,-1]}</code> |Başlangıç saatinde her ayın ilk ve son gününde Çalıştır |
| <code>{monthDays":[1,14]}</code> |Başlangıç saatinde her ayın ilk ve On dördüncü gününde Çalıştır |
| <code>{monthDays":[2]}</code> |Başlangıç saatinde ayın ikinci günü çalıştırın |
| <code>{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |5 sabah her ayın ilk Cuma çalıştırın |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}</code> |: Başlangıç saatinde her ayın ilk Cuma çalıştırın |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}</code> |Ay sonundan üçüncü Cuma günü çalıştırmak başlangıç saatinde her ay |
| <code>{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |İlk ve son Cuma her ayın 5: 15'da çalışır |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}</code> |İlk ve son Cuma her ayın başlama saatinde Çalıştır |
| <code>{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}</code> |Başlangıç saatinde her ayın beşinci Cuma çalıştırın. Bir ay içinde hiçbir beşinci Cuma ise yalnızca beşinci Cuma günleri çalışmak üzere zamanlanmış olduğundan bu, çalışmaz. İş son gerçekleşen üzerinde çalıştırmak istiyorsanız -1, 5 yerine örneği için kullanma fikrini değerlendirebilirsiniz Cuma günü. |
| <code>{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}</code> |Ayın son Cuma günü 15 dakikada çalıştırın |
| <code>{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}</code> |5: 15'te, 5: 45'te, çalıştırmak 17:15:00 saatleri ve 5:45 PM her ayın 3 Çarşamba günü |

## <a name="see-also"></a>Ayrıca Bkz.
 [Scheduler nedir?](scheduler-intro.md)

 [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)

 [Azure portalında Scheduler’ı kullanmaya başlama](scheduler-get-started-portal.md)

 [Azure Scheduler’da planlar ve faturalama](scheduler-plans-billing.md)

 [Azure Scheduler REST API başvurusu](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)

 [Yüksek Azure Scheduler kullanılabilirliği ve güvenilirliği](scheduler-high-availability-reliability.md)

 [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)

 [Azure Scheduler giden bağlantı kimlik doğrulaması](scheduler-outbound-authentication.md)

