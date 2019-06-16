---
title: Olay ilkeleri için Azure Stream Analytics sıralama yapılandırma
description: Bu makalede yapılandırma ayarları Stream Analytics'te bile sıralama açıklar
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 970eeb871775e24abb87c8b977e214645e514d3b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60789493"
---
# <a name="configuring-event-ordering-policies-for-azure-stream-analytics"></a>Olay ilkeleri için Azure Stream Analytics sıralama yapılandırma

Bu makalede, geç varış ve sırası olay ilkelerini ayarlama ve Azure Stream Analytics'te açıklar. Bu ilkeler yalnızca kullandığınızda uygulanır [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) sorgunuzda yan tümcesi.

## <a name="event-time-and-arrival-time"></a>Olay saati ve geliş saati

Stream Analytics işinizi bağlı olayları işleyebilmektedir *olay saati* veya *geliş saati*. **Olay/uygulama süresi** (olay oluşturulduğunda) olay yükü zaman damgası olan. **Geliş saati** giriş kaynağı (olay hub'ları / IOT Hub/Blob Depolama) olayın alındığı zaman damgası andır. 

Varsayılan olarak, Stream Analytics tarafından olayları işler *geliş saati*, ancak olaylar tarafından işlenecek seçebilirsiniz *olay saati* kullanarak [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) sorgunuzda yan tümcesi. Geç varış ve sırası ilkeleri, yalnızca olay zamanına göre olayları işlemesi ise geçerlidir. Bu ayarları yapılandırırken senaryonuz için gecikme süresi ve doğruluk gereksinimlerini göz önünde bulundurun. 

## <a name="what-is-late-arrival-policy"></a>Geç varış İlkesi nedir?

Olaylar bazen çeşitli nedenlerle geç gelen. Örneğin, 40 saniye geç ulaşan bir olay Olay saati olur 00:10:00 ve alma zamanı = 00:10:40 =. Geç varış İlkesi 15 saniye, 15 saniye ya da (Stream Analytics tarafından işlendiğinde değil) iptal edilir ya da kendi olay saati ayarlanan daha sonra geldiğinde herhangi bir olay olarak ayarlarsanız. 40 saniye geç (birden çok ilke kümesi), olay geldi olarak yukarıdaki örnekte, olay saati, geç varış İlkesi 00:10:25 (geliş saati - geç varış ilke değeri) en büyük ayarlanır. Geç varış ilkesi varsayılan 5 saniyedir.

## <a name="what-is-out-of-order-policy"></a>Çıkış sırası İlkesi nedir? 

Olay sıralama dışına çıkan de gelebilir. Olay saati geç varış ilke temelinde ayarlandıktan sonra otomatik olarak bırakın veya sıra dışı olayları ayarlamak seçebilirsiniz. Bu ilke 8 saniye olarak ayarlarsanız, 8 saniyelik aralıklarda içinde sıralamaya ancak geldiğinde meydana gelen olayları olay zamanına göre sıralanır. Daha sonra gelen olayları bırakılan ya da en fazla sırası ilke değerine ayarlanır. Varsayılan sırası 0 saniye ilkesidir. 

## <a name="adjust-or-drop-events"></a>Ayarlayabilir ya da açılan olaylar

Yapılandırdığınız ilkelere dayalı olayları geç veya çıkış sırası geldiğinde, (Stream Analytics tarafından işlendiğinde değil) bu tür olayların bırakın veya ayarlanmış kendi olay süresine sahip.

Bize bir örnek uygulamada bu ilkelerin bakın.
<br> **Geç varış İlkesi:** 15 saniye
<br> **Çıkış sırası İlkesi:** 8 saniye

| Olay No | Olay saati | Geliş saati | System.Timestamp | Açıklama |
| --- | --- | --- | --- | --- |
| **1** | 00:10:00  | 00:10:40  | 00:10:25  | Olay geç ve dış dayanıklılık düzeyi geldi. Bu nedenle olay süresi en geç varış toleransı ayarlanmış.  |
| **2** | 00:10:30 | 00:10:41  | 00:10:30  | Olay geç ancak dayanıklılık düzeyi içindeki geldi. Bu nedenle olay saati ayarlanmış değil.  |
| **3** | 00:10:42 | 00:10:42 | 00:10:42 | Olay saati geldi. Gerekli ayarlama yok.  |
| **4** | 00:10:38  | 00:10:43  | 00:10:38 | Olay sırası ancak dayanıklılık 8 saniye içinde geldi. Bu nedenle, olay saati ayarlanmış değil. Analiz amacıyla bu olayı olay numarası 4 önceki olarak kabul edilir.  |
| **5** | 00:10:35 | 00:10:45  | 00:10:37 | Olay 8 saniye sırası ve dış tolerans geldi. Bu nedenle, olay saati sırası dayanıklılık maksimum ayarlanır. |

## <a name="can-these-settings-delay-output-of-my-job"></a>Bu ayarlar işim çıktısını geciktirip? 

Evet. Varsayılan olarak, sırası ilke, sıfır (00 dakika 00 saniye için) ayarlanır. Varsayılan değiştirirseniz, işinizin ilk çıkış bu değere göre Gecikmeli (veya üstü). 

Girişlerinizi bölümlerinden olayları almazsa, geç varış ilke değeri geciktirileceği, çıktıyı beklemelisiniz. Neden aşağıdaki InputPartition hata bölümünü okuyun. 

## <a name="i-see-lateinputevents-messages-in-my-activity-log"></a>My etkinlik günlüğünde LateInputEvents iletileri görüyorum

Bu iletiler, olayları geç gelen ve bırakılan veya yapılandırmanıza göre ayarlanmış olduğunu bildirmek için gösterilir. Geç varış ilke uygun şekilde yapılandırdıysanız, bu iletileri yoksayabilirsiniz. 

Bu ileti örneği verilmiştir: <br>
<code>
{"message Time":"2019-02-04 17:11:52Z","error":null,
"message":"First Occurred: 02/04/2019 17:11:48 | Resource Name: ASAjob | Message: Source 'ASAjob' had 24 data errors of kind 'LateInputEvent' between processing times '2019-02-04T17:10:49.7250696Z' and '2019-02-04T17:11:48.7563961Z'. Input event with application timestamp '2019-02-04T17:05:51.6050000' and arrival time '2019-02-04T17:10:44.3090000' was sent later than configured tolerance.","type":"DiagnosticMessage","correlation ID":"49efa148-4asd-4fe0-869d-a40ba4d7ef3b"} 
</code>

## <a name="i-see-inputpartitionnotprogressing-in-my-activity-log"></a>My etkinlik günlüğünde InputPartitionNotProgressing görüyorum

Giriş kaynağı (olay hub'ı / IOT Hub) büyük olasılıkla birden çok bölümü vardır. En az zaman t1 yalnızca birleştirilen tüm bölümleri olduktan sonra azure Stream Analytics için zaman damgası t1 çıktıyı üretir. Örneğin, sorgu iki bölüme sahip bir event hub bölümünden okur varsayalım. Bölümlerinden, P1, olayları zaman t1 kadar sahiptir. Başka bir bölüme, P2, olayları zaman t1 + x kadar sahiptir. Çıkış kadar zaman t1 sonra oluşturulur. Ancak, bölüm kimliği yan tümcesi ile açık bir bölüm varsa, her iki bölüm bağımsız olarak ilerleme. 

Birden çok bölüm aynı giriş akışından bir araya geldiğinde, geç varış toleransı yeni veriler için her bölüm beklediği en uzun süreyi ' dir. Olay Hub'ında bir bölüm yok ya da IOT hub'ı girişleri almazsa, bu bölümü için zaman çizelgesi kadar süren değil, geç varış toleransı eşiğe ulaşır. Bu çıkış geç varış toleransı eşiğinin geciktirir. Böyle durumlarda, şu iletiyi görebilirsiniz:
<br><code>
{"message Time":"2/3/2019 8:54:16 PM UTC","message":"Input Partition [2] does not have additional data for more than [5] minute(s). Partition will not progress until either events arrive or late arrival threshold is met.","type":"InputPartitionNotProgressing","correlation ID":"2328d411-52c7-4100-ba01-1e860c757fc2"} 
</code><br><br>
En az bir giriş bölümünde boştur ve çıkış geç varış eşiğinin geciktirir bildirmek için bu ileti. Bunu çözmek için önerilir ya da: 
1. Olay hub'ı / IOT hub'ınızın tüm bölümleri alma giriş emin olun. 
2. By PartitionID tümcesi sorgunuzda bölümü kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
* [Zaman işleme konuları](stream-analytics-time-handling.md)
* [Stream Analytics'te mevcut olan ölçümler](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
