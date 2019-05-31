---
title: Yineleme tetikleyicisi - Azure Logic Apps ile yinelenen görevleri zamanlama
description: Zamanlama ve yinelenme tetikleyicisi Azure Logic Apps ile yinelenen otomatik görevler ve iş akışları çalıştırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: f5fc778ee4d8f91232bc732cc276f642f748b29d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297509"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-recurrence-trigger-in-azure-logic-apps"></a>Oluşturmanıza, zamanlamanıza ve Azure Logic apps'te yineleme tetikleyicisi ile yinelenen görevleri ve iş akışları çalıştırma

Görevler, işlemleri veya işleri düzenli olarak belirli bir zamanlamaya göre çalıştırmak için yerleşik ile mantıksal uygulama iş akışınızı başlatabilirsiniz **yinelenme - zamanlama** tetikleyici. İş akışı ve bir yineleme için iş akışının yinelenen başlatmak için bir saat dilimi yanı sıra bir tarih ve saat ayarlayabilirsiniz. Yinelenme için herhangi bir nedenle kaçırdıysanız, bu tetikleyici, sonraki zamanlanan aralıklarla yinelenen devam eder. Yerleşik zamanlama Tetikleyicileri ve eylemleri hakkında daha fazla bilgi için bkz. [zamanlama ve otomatik olarak yinelenen bir çalıştırma, görevleri ve Azure Logic Apps ile iş akışlarını](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Daha gelişmiş yinelenme ve karmaşık zamanlamalar yanı sıra bu tetikleyiciyi destekleyen bazı desenleri şunlardır:

* Hemen çalıştırma ve yineleme her *n* saniye, dakika, saat, gün, hafta veya ayda sayısı.

* Başlangıç bir belirli bir tarih ve saatte, ardından çalıştırın ve yineleme her *n* saniye, dakika, saat, gün, hafta veya ayda sayısı.

* Çalıştırın ve bir veya daha fazla zaman her gün 8:00:00 ve 17:00:00 Örneğin, tekrarlayın.

* Çalıştırın ve her hafta, ancak yalnızca Cumartesi ve Pazar gibi belirli bir gün için yineleyin.

* Çalıştırın ve her hafta, ancak yalnızca belirli günleri ve saatleri, Pazartesi ile Cuma günleri saat 8: 00'da ve 17:00 gibi için yineleyin.

Bu tetikleyici ve kayan pencere tetikleyicisi arasındaki farklar veya yinelenen iş akışları zamanlama hakkında daha fazla bilgi için bkz: [zamanlama ve çalıştırma yinelenen görevler, süreçleri ve Azure Logic Apps ile iş akışlarını otomatik](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Mantıksal uygulamanızı tetikleyecek ve gelecekteki bakın yalnızca bir kez çalıştırmak istiyorsanız [işler yalnızca bir kez çalıştır](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkında temel bilgilere [logic apps](../logic-apps/logic-apps-overview.md). Logic apps kullanmaya yeni başladıysanız öğrenin [ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-recurrence-trigger"></a>Yineleme tetikleyicisi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantıksal Uygulama Tasarımcısı, arama kutusuna göründükten sonra filtreniz olarak "yinelenme" girin. Tetikleyiciler listesinden, mantıksal uygulama iş akışınızı ilk adımı olarak şu tetikleyiciyi seçin: **Yineleme**

   !["Yinelenme" tetikleyicisini seçin](./media/connectors-native-recurrence/add-recurrence-trigger.png)

1. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, her hafta, iş akışınızı çalıştırmak için bu özellikleri ayarlayın.

   ![Kümesi aralığı ve sıklığı](./media/connectors-native-recurrence/recurrence-trigger-details.png)

   | Özellik | Gerekli | JSON adı | Type | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **Aralık** | Evet | interval | Tamsayı | İş akışı sıklığı temel alarak çalışan ne sıklıkta açıklar pozitif bir tamsayı. Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve "Month" sıklığıdır yinelenme her 6 ayda olur. |
   | **Sıklık** | Evet | frequency | String | Yineleme için zaman birimi: **İkinci**, **dakika**, **saat**, **gün**, **hafta**, veya **ay** |
   ||||||

   Daha fazla zamanlama seçeneklerini açın **yeni parametre Ekle** listesi. 
   Seçtiğiniz herhangi bir seçenek Tetikle seçimi yaptıktan sonra görünür.

   ![Gelişmiş zamanlama seçenekleri](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   | Özellik | Gerekli | JSON adı | Type | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **Saat dilimi** | Hayır | timeZone | String | Bu tetikleyiciyi kabul etmez çünkü yalnızca bir başlangıç zamanı belirttiğinizde geçerlidir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini seçin. |
   | **Başlangıç saati** | Hayır | startTime | String | Bir başlangıç tarihi ve saati şu biçimde belirtin: <p>YYYY-MM-ddTHH bir saat dilimi seçerseniz <p>veya <p>YYYY-AA-saat dilimi seçmezseniz ssZ <p>Örneğin, 18 Eylül 2017 2: 00'da isterseniz, ardından belirtin "2017-09-18T14:00:00" Pasifik Standart Saati gibi bir saat dilimi seçin. Ya da belirtin "2017-09-18T14:00:00Z" olmadan bir saat dilimi. <p>**Not:** Bu başlangıç zamanı izlemelidir [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi seçmezseniz, sonunda boşluk olmadan "Z" harfi eklemeniz gerekir. Bu "Z" eş değeri başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlamalar için ilk yinelenme, başlangıç zamanıdır sırada karmaşık zamanlamalar için tetikleyici başlangıç saatinden herhangi bir erken etkinleşmez. [*Başlangıç tarihi ve saati kullanabilirim, yolları nelerdir?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   | **Şu günlerde** | Hayır | weekDays | Dize veya dize dizisi | "Week" seçeneğini belirlerseniz, iş akışını çalıştırmak istediğiniz bir veya daha fazla gün seçebilirsiniz: **Pazartesi**, **Salı**, **Çarşamba**, **Perşembe**, **Cuma**, **Cumartesi**, ve **Pazar** |
   | **Şu saatlerde** | Hayır | hours | Tamsayı veya tamsayı dizisi | "Day" veya "Week" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 23 iş akışını çalıştırmak istediğinizde için günün saat olarak seçebilirsiniz. <p><p>Örneğin, "10", "12" ve "14" belirtirseniz, 10 AM, PM 12 ve gün saat 2 PM Al, ancak yinelenme başladığında dakika gün sayısı temel alınarak hesaplanır. Günün dakikasının ayarlamak için değerini belirtin. **başından bu dakika** özelliği. |
   | **Şu dakikalarda** | Hayır | minutes | Tamsayı veya tamsayı dizisi | "Day" veya "Week" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 59 arasında iş akışını çalıştırmak istediğinizde saat dakika seçebilirsiniz. <p>Örneğin, "30" dakika işareti belirtebilirsiniz ve önceki örnekte için günün saatlerini kullanarak 10:30 AM, alın 12:30 PM ve 2:30 PM. |
   |||||

   Örneğin, 4 Eylül 2017, Pazartesi, bugün olduğunu varsayın. Aşağıdaki yinelenme tetikleyicisini etkinleşmez *herhangi erken* başlangıç tarihi ve saati olduğu 18 Eylül 2017, Pazartesi 08: 00'te Pasifik saati. Yinelenme zamanlaması 10:30 AM, ancak ayarlanmış 12:30 PM ve 2:30 PM yalnızca Pazartesi günleri. Bu nedenle ilk kez tetiklenir ve bir mantıksal uygulama iş akışı örneği oluşturur, 10: 30'da olduğu. Başlangıç iş ne zaman hakkında daha fazla bilgi için bkz: [start zamanı örnekleri](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time).

   12:30 PM ve 2:30 PM, gelecekteki çalışmalarını aynı gün gerçekleşir. Her yineleme, kendi iş akışı örneği oluşturur. Bundan sonra tüm yeniden sonraki Pazartesi tüm zamanlama tekrarlar. [*Diğer bir örnek örnekleri nelerdir?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#example-recurrences)

   ![Gelişmiş zamanlama örneği](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > Yalnızca "Day" veya "Week" sıklığı seçtiğinizde, belirtilen yinelenme için Önizleme tetikleyici gösterir.

1. Şimdi diğer eylemlerle kalan iş akışınızı oluşturun. Ekleyebileceğiniz diğer eylemler için bkz. [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md).

## <a name="workflow-definition---recurrence"></a>İş akışı tanımı - yinelenme

JSON kullanan mantıksal uygulamanızın temel iş akışı tanımında görüntüleyebileceğiniz [yinelenme tetikleyicisi tanımı](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger) seçtiğiniz seçeneklere sahip. Bu tanım tasarımcı araç çubuğunda görüntülemek için seçin **kod görünümü**. Tasarımcıya geri dönmek için tasarımcı araç çubuğunda seçin **Tasarımcısı**.

Bu örnekte, temel alınan bir iş akışı tanımında bir yinelenme tetikleyicisi tanımı nasıl görünebileceği gösterilmiştir:

``` json
"triggers": {
   "Recurrence": {
      "type": "Recurrence",
      "recurrence": {
         "frequency": "Week",
         "interval": 1,
         "schedule": {
            "hours": [
               10,
               12,
               14
            ],
            "minutes": [
               30
            ],
            "weekDays": [
               "Monday"
            ]
         },
         "startTime": "2017-09-07T14:00:00Z",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Gecikmeli eylemleri akışlarıyla Duraklat](../connectors/connectors-native-delay.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)
