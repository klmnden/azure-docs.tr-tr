---
title: Gelişmiş iş zamanlama ve yinelenme - Azure Zamanlayıcı oluşturma
description: Azure Scheduler'da Gelişmiş zamanlama ve yinelenme işleri oluşturmayı öğrenin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.suite: infrastructure-services
ms.assetid: 5c124986-9f29-4cbc-ad5a-c667b37fbe5a
ms.topic: article
ms.date: 11/14/2018
ms.openlocfilehash: a413261d251c8dfc1de9209168ee8137b85009f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531819"
---
# <a name="build-advanced-schedules-and-recurrences-for-jobs-in-azure-scheduler"></a>Gelişmiş zamanlamalar ve yinelenme için Azure zamanlayıcı işleri oluşturma

> [!IMPORTANT]
> Kullanımdan kaldırılan Azure Scheduler uygulamasının yerini [Azure Logic Apps](../logic-apps/logic-apps-overview.md) alacaktır. İş zamanlamak için [Azure Logic Apps'ı deneyebilirsiniz](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

İçinde bir [Azure Scheduler](../scheduler/scheduler-intro.md) zamanlama iş Zamanlayıcı hizmeti, işi çalışır, nasıl ve ne zaman belirleyen çekirdek. Zamanlayıcı ile bir iş için birden çok kez ve yinelenen zamanlama ayarlayabilirsiniz. Tek seferlik zamanlama belirli bir zamanda yalnızca bir kez çalıştırın ve temel olarak yalnızca bir kez çalışan zamanlamaları. Yinelenen zamanlamalar, belirtilen bir sıklıkta üzerinde çalıştırın. Bu esneklik ile Zamanlayıcı çeşitli iş senaryoları için aşağıdaki gibi kullanabilirsiniz:

* **Düzenli aralıklarla veri temizleme**: Üç aydan eski tüm tweetleri siler günlük bir iş oluşturun.

* **Arşiv veri**: Bildirim geçmişi yedekleme hizmeti için fatura, aylık bir iş oluşturun.

* **Dış veri isteği**: 15 dakikada bir çalışır ve yeni bir hava durumu raporu NOAA çeken bir iş oluşturun.

* **Görüntü işleme**: Yoğun olmayan saatlerinde çalışır ve bulut bilgi işlem gün boyunca karşıya yüklenen görüntüleri sıkıştırma için kullandığı bir haftanın günü işi oluşturun.

Bu makalede örnek iş Zamanlayıcısı'nı kullanarak oluşturabilir ve [Azure Scheduler REST API](/rest/api/scheduler)ve her zamanlama için JavaScript nesne gösterimi (JSON) tanımı içerir. 

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Bu örnekler çeşitli Azure Scheduler'ı destekleyen senaryoları ve zamanlamalar için çeşitli davranış desenlerini, örneğin oluşturma işlemini gösterir:

* Belirli bir tarih ve saatte bir kez çalıştırın.
* Çalıştırın ve belirli bir sayıda yineleme.
* Hemen çalıştırma ve yineleme.
* Çalıştırma ve yineleme her *n* dakika, saat, gün, hafta veya ay, belirli bir zamanda başlatılıyor.
* Çalıştırın ve haftalık veya aylık, ancak yalnızca haftanın belirli gün veya ayın belirli günlerde Yinele.
* Çalıştırın ve belirli bir süre için birden çok kez Yinele. Örneğin, her ay Pazartesi ve son Cuma veya günlük olarak 5: 15'te saat ve saat 17:15:00.

Bu makalede daha sonra bu senaryolar daha ayrıntılı açıklanır.

<a name="create-scedule"></a>

## <a name="create-schedule-with-rest-api"></a>REST API ile zamanlama oluşturma

İle temel bir zamanlama oluşturmak için [Azure Scheduler REST API](/rest/api/scheduler), şu adımları izleyin:

1. Kullanarak bir kaynak sağlayıcısı ile Azure aboneliğinizi kaydetme [işlemi - Resource Manager REST API'si kaydetme](https://docs.microsoft.com/rest/api/resources/providers). Azure Zamanlayıcı hizmeti sağlayıcı adı **Microsoft.Scheduler**. 

1. Kullanarak bir iş koleksiyonu oluşturma [iş koleksiyonları için Create veya Update işleminde](https://docs.microsoft.com/rest/api/scheduler/jobcollections) Scheduler REST API. 

1. Kullanarak bir işi oluşturma [işleri için Create veya Update işleminde](https://docs.microsoft.com/rest/api/scheduler/jobs/createorupdate). 

## <a name="job-schema-elements"></a>İş şeması öğeleri

Bu tablo, yinelenme ve işlerinin zamanlamalarını ayarlarken kullanabileceğiniz ana JSON öğeler için üst düzey bir genel bakış sağlar. 

| Öğe | Gerekli | Açıklama | 
|---------|----------|-------------|
| **startTime** | Hayır | Bir tarih saat dizesi değeri [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601) iş ilk temel bir zamanlamanın başladığı belirtir. <p>Karmaşık zamanlamalar için'dan önce iş başlar **startTime**. | 
| **recurrence** | Hayır | İş çalıştırıldığında için yinelenme kurallarını. **Yinelenme** nesnesi şu öğeleri destekler: **sıklığı**, **aralığı**, **zamanlama**, **sayısı**, ve **endTime**. <p>Kullanırsanız **yinelenme** öğesi de kullanmalısınız **sıklığı** öğesi, diğer yandan **yinelenme** öğeler isteğe bağlıdır. |
| **frequency** | Evet, kullandığınızda **yinelenme** | Örnekleri arasında zaman birimi ve bu değerleri destekler: "Minute", "Hour", "Day", "Week", "Month" ve "Year" | 
| **interval** | Hayır | Zaman birimleri arasında oluşum sayısını belirleyen pozitif bir tamsayı temel alarak **sıklığı**. <p>Örneğin, varsa **aralığı** 10'dur ve **sıklığı** "Week" ise işin 10 haftada bir yinelenir. <p>İşte aralık her sıklığı için en iyi bir sayısı: <p>-18 ay <br>-78 hafta <br>-548 gün <br>-Saat ve dakika için 1 aralığı < = <*aralığı*>< = 1000. | 
| **schedule** | Hayır | Belirtilen dakika-işaretlerini, saat işaretlerinde, ayın günü ve haftanın günleri tabanlı yinelenme değişiklikleri tanımlar | 
| **count** | Hayır | İş tamamlamadan önce çalışan sayısını belirten pozitif bir tamsayı. <p>Örneğin, bir günlük iş olduğunda **sayısı** 7'ye ayarlayın ve Pazartesi, başlangıç tarihi, iş tamamlanana Pazar günü çalışıyor. Başlangıç tarihi zaten geçtiyse, ilk çalıştırma oluşturma süreye göre hesaplanır. <p>Olmadan **endTime** veya **sayısı**, sonsuz işi çalıştırır. Her ikisini birden kullanamazsınız **sayısı** ve **endTime** aynı işi, ancak tamamlanmadan önce kabul kural. | 
| **endTime** | Hayır | Bir tarih veya tarih/saat dize değeri [ISO 8601 biçimi](https://en.wikipedia.org/wiki/ISO_8601) iş durduğunda belirten çalışıyor. İçin bir değer ayarlayabilirsiniz **endTime** geçmişte olmasıdır. <p>Olmadan **endTime** veya **sayısı**, sonsuz işi çalıştırır. Her ikisini birden kullanamazsınız **sayısı** ve **endTime** aynı işi, ancak tamamlanmadan önce kabul kural. |
|||| 

Örneğin, bir basit zamanlama ve yinelenme bir iş için bu JSON şema açıklanmaktadır: 

```json
"properties": {
   "startTime": "2012-08-04T00:00Z", 
   "recurrence": {
      "frequency": "Week",
      "interval": 1,
      "schedule": {
         "weekDays": ["Monday", "Wednesday", "Friday"],
         "hours": [10, 22]                      
      },
      "count": 10,       
      "endTime": "2012-11-04"
   },
},
``` 

*Tarihleri ve tarih/saat değerleri*

* Scheduler işleri tarihleri yalnızca tarih ve izleyin [ISO 8601 belirtimi](https://en.wikipedia.org/wiki/ISO_8601).

* Tarih-saatleri zamanlayıcı işleri izleyin, tarih ve saat içerir [ISO 8601 belirtimi](https://en.wikipedia.org/wiki/ISO_8601)ve UTC'ye uzaklık belirtildiğinde UTC olarak kabul edilir. 

Daha fazla bilgi için [kavramları, terminolojisi ve varlık](../scheduler/scheduler-concepts-terms.md).

<a name="start-time"></a>

## <a name="details-starttime"></a>Ayrıntılar: startTime

Bu tabloda açıklanır nasıl **startTime** bir işin çalışma biçimini denetler:

| startTime | Yineleme yok | Yinelenme, zamanlama | Zamanlama ile yinelenme |
|-----------|---------------|-------------------------|--------------------------|
| **Başlangıç saati yok** | Hemen bir kez çalıştırın. | Hemen bir kez çalıştırın. Son Yürütme zamanına göre hesaplanan sonraki yürütmeleri çalıştırın. | Hemen bir kez çalıştırın. Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırın. | 
| **Başlangıç zamanı geçmişte** | Hemen bir kez çalıştırın. | İlk gelecek çalıştırma zamanı başlangıç zamanından sonra ve o anda çalıştırın hesaplayın. <p>Son Yürütme zamanına göre hesaplanan sonraki yürütmeleri çalıştırın. <p>Bu tablodan sonraki örneğe bakın. | İşi Başlat *başlamaz* belirtilen başlangıç saati. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir. <p>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırın. | 
| **Başlangıç zamanı gelecekte veya geçerli saati** | Belirtilen başlangıç zamanında bir kez çalıştırın. | Belirtilen başlangıç zamanında bir kez çalıştırın. <p>Son Yürütme zamanına göre hesaplanan sonraki yürütmeleri çalıştırın. | İşi Başlat *başlamaz* belirtilen başlangıç saati. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamaya göre gerçekleştirilir. <p>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırın. |
||||| 

Bu örnekte bu koşullar ile düşünün: bir başlangıç zamanı geçmişte bir yinelenme olmasına rağmen zamanlama.

```json
"properties": {
   "startTime": "2015-04-07T14:00Z", 
   "recurrence": {
      "frequency": "Day",
      "interval": 2
   }
}
```

* Geçerli tarih ve saat 1: 00'da 08 Nisan 2015 olduğu.

* Başlangıç tarihi ve saati, 07 Nisan 2015 2:00, geçerli tarih ve saat önce PM adresindeki olduğu.

* Yinelenme her iki gündür.

1. Bu şartlar altında ilk yürütme 09 Nisan 2015'te 2: 00'da olduğu. 

   Zamanlayıcı hesaplar. başlangıç zamanı temel alınarak yürütme örnekleri geçmişte herhangi bir örneği atar ve gelecekte bir sonraki örneği kullanır. 
   Bu durumda, **startTime** 09 Nisan 2015 2: 00'da olduğundan bu zamandan iki gün sonraki örnek, bu nedenle 2: 00'da, 07 Nisan 2015'te olan.

   İlk yürütme aynı olup olmadığını **startTime** 2015-04-05 14:00 veya 2015-04-01 14:00. İlk yürütme sonrasındaki sonraki yürütmeleri temel zamanlamaya göre hesaplanır. 
   
1. Yürütme, ardından şu sırayla uygulayın: 
   
   1. 2:00 PM, 2015-04-11
   1. 2:00 PM, 2015-04-13 
   1. 2015-04-15 gün 2:00 PM
   1. ve benzeri...

1. Son olarak, bir iş zamanlama ancak hiçbir belirtilen saat ve dakika, saat ve dakika içinde ilk kez yürütülmesi için bu değerleri varsayılan sırasıyla olduğunda.

<a name="schedule"></a>

## <a name="details-schedule"></a>Ayrıntılar: zamanlama

Kullanabileceğiniz **zamanlama** için *sınırı* iş yürütmelerinin sayısı. Örneğin, bir işlemle bir **sıklığı** "month" 31. günde çalışan bir zamanlama sahipse, işin yalnızca 31. günü olan aylarda çalışır.

Ayrıca **zamanlama** için *genişletin* iş yürütmelerinin sayısı. Örneğin, bir işlemle bir **sıklığı** "month" ayın günü 1 ve 2 çalışan bir zamanlama sahipse, işi ayda yalnızca bir kez yerine ayın birinci ve ikinci günlerinde çalışır.

Birden fazla zamanlama öğesini belirtirseniz, değerlendirme sırası en büyük olduğu için en küçük: hafta numarası, ayın günü, haftanın günü, saat ve dakika.

Aşağıdaki tabloda schedule öğeleri ayrıntılı bir şekilde açıklanmıştır:

| JSON adı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **minutes** |İşin çalıştırıldığı saat dakika. |Tamsayı dizisi. |
| **hours** |İşin çalıştığı günün saati. |Tamsayı dizisi. |
| **weekDays** |İşin çalıştığı hafta günleri. Yalnızca haftalık bir sıklık ile belirtilebilir. |Bir herhangi birinin dizisi (en fazla dizi boyutu 7'dir) aşağıdaki değerleri:<br />-"Pazartesi"<br />-"Salı"<br />-"Çarşamba"<br />-"Thursday"<br />-"Cuma"<br />-"Saturday"<br />-"Sunday"<br /><br />Duyarlı değildir. |
| **monthlyOccurrences** |İşi ayın hangi günlerinde çalışacağını belirler. Yalnızca aylık bir sıklık ile belirtilebilir. |Bir dizi **monthlyOccurrences** nesneler:<br /> `{ "day": day, "occurrence": occurrence}`<br /><br /> **gün** işin çalıştığı haftanın günüdür. Örneğin, *{Pazar}* ayın her Pazar günüdür. Gereklidir.<br /><br />**oluşum** gün ay boyunca tekrarı. Örneğin, *{Pazar -1}* ayın son Pazar günüdür. İsteğe bağlı. |
| **monthDays** |İşin çalıştığı ayın günü. Yalnızca aylık bir sıklık ile belirtilebilir. |Aşağıdaki değerlerin dizisi:<br />- <= -1 ve >= -31 koşullarına uyan herhangi bir değer<br />- >= 1 ve <= 31 koşullarına uyan herhangi bir değer|

## <a name="examples-recurrence-schedules"></a>Örnekler: Yinelenme zamanlaması

Aşağıdaki örnekler, çeşitli yineleme zamanlaması gösterir. Örnekler schedule nesnesine ve alt öğelerine odaklanın.

Bu zamanlamaları varsayımında **aralığı** 1 olarak ayarlayın\. Örneklerde de doğru varsayılır **sıklığı** değerleri değerlerinin **zamanlama**. Örneğin, kullanamazsınız bir **sıklığı** "Day" ve bir **monthDays** değişiklik **zamanlama**. Makalesinde daha önce bu kısıtlamalar açıklanmaktadır.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` |05: 00 her gün çalışır.<br /><br />Zamanlayıcı her değeri her değerinde "dakika", bir iş, her zaman çalışır bir liste oluşturmak için bir "saat" ile eşleşir. |
| `{"minutes":[15], "hours":[5]}` |Her gün 05.15'te çalıştır. |
| `{"minutes":[15], "hours":[5,17]}` |Her gün 05.15 ve 17.15’te çalıştır. |
| `{"minutes":[15,45], "hours":[5,17]}` |Her gün 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |
| `{"minutes":[0,15,30,45]}` |15 dakikada bir çalıştır. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` |Saatte bir çalıştır.<br /><br />Bu iş saatte bir çalışır. Dakika değeri tarafından denetlenen **startTime**, belirtilmişse. Hayır ise **startTime** değer belirtildi, dakikalık oluşturma zamanı tarafından denetlenir. Örneğin, iş başlangıç zamanı veya oluşturma zamanı (hangisi geçerliyse) 12:25 ise, 00:25, 01:25, çalışır 02:25, …, 23:25.<br /><br />Zamanlama bir işlemle aynıdır bir **sıklığı** değeri "hour", bir **aralığı** 1 ve Hayır **zamanlama** değeri. Bu zamanlama farklı ile kullanabileceğiniz fark **sıklığı** ve **aralığı** diğer işleri oluşturmak için değerleri. Örneğin, varsa **sıklığı** ayda yalnızca bir kez yerine her gün zamanlaması çalıştırmaları "month" olan (varsa **sıklığı** "day" şeklindedir). |
| `{minutes:[0]}` |Her saat başı çalıştır.<br /><br />Bu işlemi de her saat çalışır ancak saat (12 AM, AM 1, 2 ÖÖ ve benzeri). Bu zamanlamayı bir işlemle aynıdır bir **sıklığı** değeri "hour", bir **startTime** değeri sıfır dakika olan ve **zamanlama**, sıklığı "day" şeklindedir. Ancak, varsa **sıklığı** olan "week" veya "month" zamanlama yalnızca bir haftada bir gün veya ayda bir gün sırasıyla yürütür. |
| `{"minutes":[15]}` |15 dakika geçe saat başı Çalıştır.<br /><br />00: 15'da, 01:15:00, 02:15:00, başlangıç saatte bir çalışır ve benzeri. Bunu 11: 15'te sona erer. |
| `{"hours":[17], "weekDays":["saturday"]}` |Her hafta Cumartesi günleri 17: 00 çalıştırın. |
| `{hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma üzerinde 17: 00 çalıştırın. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` |Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.15 ve 17.45'te çalıştır. |
| `{"hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |5 ve 17: 00 Pazartesi, Çarşamba ve Cuma üzerinde her hafta çalıştırın. |
| `{"minutes":[15,45], "hours":[5,17], "weekDays":["monday", "wednesday", "friday"]}` |05:15:00, 45'te, çalıştırma-17:15 ve 17:45 Saatlerinde Pazartesi, Çarşamba ve Cuma her hafta üzerinde. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Haftanın her günü 15 dakikada bir çalıştır. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` |Hafta içi 09: 00 ve 16:45 saatleri arasında 15 dakikada bir çalıştır. |
| `{"weekDays":["sunday"]}` |Pazar günleri başlangıç zamanında Çalıştır. |
| `{"weekDays":["tuesday", "thursday"]}` |Salı ve Perşembe günleri başlangıç zamanında Çalıştır. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` |Her ayın 28 günlük 6'da Çalıştır (varsayılarak bir **sıklığı** , "month"). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` |Ayın son günü sabah 6 çalıştırın.<br /><br />Bir iş ayın son gününde çalıştırmak istiyorsanız, günde 28, 29, 30 veya 31 yerine-1 değerini kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` |Her ayın ilk ve son günü sabah 6 çalıştırın. |
| `{monthDays":[1,-1]}` |Başlangıç zamanında her ayın ilk ve son günü çalıştırın. |
| `{monthDays":[1,14]}` |Başlangıç zamanında her ayın birinci ve 14 gün çalıştırın. |
| `{monthDays":[2]}` |İkinci ayın başlangıç zamanında Çalıştır. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |5'te, her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` |Başlangıç zamanında her ayın ilk Cuma çalıştırın. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` |Başlangıç zamanında her ay, ayın sondan üçüncü Cuma gününde çalıştırmak. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |Her ayın ilk ve son Cuma günü saat 05.15'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` |İlk ve her ayın son Cuma günü, başlangıç zamanında Çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` |Başlangıç zamanında her ayın beşinci Cuma çalıştırın.<br /><br />Bir ayda beşinci Cuma günü yoksa, iş çalışmaz. İş son gerçekleşen üzerinde çalıştırmak istiyorsanız, -1 oluşumu için 5 yerine kullanmayı düşünebilirsiniz Cuma ayın günü. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` |Ayın son Cuma günü 15 dakikada bir çalıştır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` |Her ayın üçüncü Çarşamba günü 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Scheduler nedir?](scheduler-intro.md)
* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler sınırları, varsayılanları ve hata kodları](scheduler-limits-defaults-errors.md)
