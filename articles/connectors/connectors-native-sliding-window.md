---
title: Kayan pencere tetikleyicisi - Azure Logic Apps ile yinelenen görevleri zamanlama
description: Zamanlama ve Azure Logic apps'te kayan pencere tetikleyicisi ile yinelenen otomatik görevler ve iş akışları çalıştırma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 44944955019fcf81fb0d296592577e2b00a15928
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299510"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-sliding-window-trigger-in-azure-logic-apps"></a>Oluşturmanıza, zamanlamanıza ve Azure Logic apps'te kayan pencere tetikleyicisi ile yinelenen görevleri ve iş akışları çalıştırma

Görevler, işlemleri veya sürekli öbekler halinde veri işlemesi işleri düzenli olarak çalıştırmak için mantıksal uygulama iş akışı ile başlayabilirsiniz **kayan pencere - zamanlama** tetikleyici. İş akışı ve bir yineleme için iş akışının yinelenen başlatmak için bir saat dilimi yanı sıra bir tarih ve saat ayarlayabilirsiniz. Yinelenme için herhangi bir nedenle kaçırdıysanız, bu tetikleyiciyi bu eksik yinelenme işler. Örneğin, böylece verileri boşlukları ödemeden eşitlendiğini yedekleme depolama alanı ve veritabanı arasında veri eşitlenirken kayan pencere tetikleyicisi kullanın. Yerleşik zamanlama Tetikleyicileri ve eylemleri hakkında daha fazla bilgi için bkz. [zamanlama ve otomatik olarak yinelenen bir çalıştırma, görevleri ve Azure Logic Apps ile iş akışlarını](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Bu tetikleyiciyi destekleyen bazı desenleri şunlardır:

* Hemen çalıştırma ve yineleme her *n* saniye, dakika veya saat sayısı.

* Başlangıç bir belirli bir tarih ve saatte, ardından çalıştırın ve yineleme her *n* saniye, dakika veya saat sayısı. Bu tetikleyici ile tüm yinelenme çalıştırır geçmişte bir başlangıç saati belirtebilirsiniz.

* Her yinelemeyi çalıştırmadan önce belirli bir süre için gecikme.

Bu tetikleyicinin yinelenme tetikleyicisini arasındaki farklar veya yinelenen iş akışları zamanlama hakkında daha fazla bilgi için bkz: [zamanlama ve çalıştırma yinelenen görevler, süreçleri ve Azure Logic Apps ile iş akışlarını otomatik](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Mantıksal uygulamanızı tetikleyecek ve gelecekteki bakın yalnızca bir kez çalıştırmak istiyorsanız [işler yalnızca bir kez çalıştır](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hakkında temel bilgilere [logic apps](../logic-apps/logic-apps-overview.md). Logic apps kullanmaya yeni başladıysanız öğrenin [ilk mantıksal uygulamanızı oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Kayan pencere tetikleyicisi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantıksal Uygulama Tasarımcısı, arama kutusuna göründükten sonra filtreniz olarak "kayan pencere" girin. Tetikleyiciler listesinden, mantıksal uygulama iş akışınızı ilk adımı olarak şu tetikleyiciyi seçin: **Kayan pencere**

   !["Kayan pencere" tetikleyicisi seçin](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, her hafta, iş akışınızı çalıştırmak için bu özellikleri ayarlayın.

   ![Kümesi aralığı ve sıklığı](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Özellik | Gerekli | JSON adı | Type | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **Aralık** | Evet | interval | Tamsayı | İş akışı sıklığı temel alarak çalışan ne sıklıkta açıklar pozitif bir tamsayı. Minimum ve maksimum aralıkları şunlardır: <p>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6 sıklığıdır "Hour" ise, yineleme 6 saatte bir olduğu. |
   | **Sıklık** | Evet | frequency | String | Yineleme için zaman birimi: **İkinci**, **dakika**, veya **saat** |
   ||||||

   ![Gelişmiş yinelenme seçenekleri](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Daha fazla yinelenme seçenekleri için açık **yeni parametre Ekle** listesi. 
   Seçtiğiniz herhangi bir seçenek Tetikle seçimi yaptıktan sonra görünür.

   | Özellik | Gerekli | JSON adı | Type | Açıklama |
   |----------|----------|-----------|------|-------------|
   | **gecikme** | Hayır | delay | String | Her yineleme kullanarak gecikme süresini [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Durations) |
   | **Saat dilimi** | Hayır | timeZone | String | Bu tetikleyiciyi kabul etmez çünkü yalnızca bir başlangıç zamanı belirttiğinizde geçerlidir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini seçin. |
   | **Başlangıç saati** | Hayır | startTime | String | Bir başlangıç tarihi ve saati şu biçimde belirtin: <p>YYYY-MM-ddTHH bir saat dilimi seçerseniz <p>veya <p>YYYY-AA-saat dilimi seçmezseniz ssZ <p>Örneğin, 18 Eylül 2017 2: 00'da isterseniz, ardından belirtin "2017-09-18T14:00:00" Pasifik Standart Saati gibi bir saat dilimi seçin. Ya da belirtin "2017-09-18T14:00:00Z" olmadan bir saat dilimi. <p>**Not:** Bu başlangıç zamanı izlemelidir [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi seçmezseniz, sonunda boşluk olmadan "Z" harfi eklemeniz gerekir. Bu "Z" eş değeri başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlamalar için ilk yinelenme, başlangıç zamanıdır için Gelişmiş yinelenme durumdayken, başlangıç saatinden herhangi bir erken tetikleyici etkinleşmez. [*Başlangıç tarihi ve saati kullanabilirim, yolları nelerdir?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Şimdi diğer eylemlerle kalan iş akışınızı oluşturun. Ekleyebileceğiniz diğer eylemler için bkz. [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>İş akışı tanımı - kayan pencere

JSON kullanan mantıksal uygulamanızın temel iş akışı tanımında, seçtiğiniz seçeneklere sahip kayan pencere tetikleyici tanımında görüntüleyebilirsiniz. Bu tanım tasarımcı araç çubuğunda görüntülemek için seçin **kod görünümü**. Tasarımcıya geri dönmek için tasarımcı araç çubuğunda seçin **Tasarımcısı**.

Bu örnekte, her yineleme için gecikme süresi için saatlik bir yinelenme beş saniye olduğu bir kayan pencere tetikleyici tanımında temel alınan bir iş akışı tanımında nasıl görünebileceği gösterilmektedir:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [İş akışı bir sonraki eylem gecikmesi](../connectors/connectors-native-delay.md)
* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)