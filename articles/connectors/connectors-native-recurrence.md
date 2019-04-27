---
title: Zamanlama ve Azure Logic Apps ile otomatik görevler ve iş akışları çalıştırma | Microsoft Docs
description: Azure Logic apps'te yinelenme Bağlayıcısı ile zamanlanmış ve yinelenen görevleri otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
tags: connectors
ms.topic: article
ms.date: 01/08/2019
ms.openlocfilehash: eb22539d1f433e396935f82e4cb3786d5699d21a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60447527"
---
# <a name="create-and-run-recurring-tasks-and-workflows-with-azure-logic-apps"></a>Oluşturma ve Azure Logic Apps ile yinelenen görevleri ve iş akışları çalıştırma

Eylemler, iş yükleri veya düzenli olarak çalıştırılan işlemlerin zamanlamak için ile başlayan bir mantıksal uygulama iş akışı oluşturma **zamanlama - yinelenme** [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts). Bir tarih ve saat başlangıç iş akışı ve bu örnekleri ve daha fazlası gibi görevleri gerçekleştirmek için bir yinelenme zamanlaması için ayarlayabilirsiniz:

* İç veri alın: [SQL saklı yordamı çalıştırmak](../connectors/connectors-create-api-sqlazure.md) her gün.
* Dış Veri Al: Hava durumu raporları NOAA her 15 dakikada çekin.
* Rapor verileri: Tüm siparişleri belirli bir miktar alt sınırından daha büyük bir özeti geçen hafta içinde e-posta.
* İşlem verileri: Karşıya yüklenen görüntüleri bugünün yoğun olmayan saatleri hafta içi her gün sıkıştırın.
* Verileri temizleme: Üç aydan eski tüm tweetleri silin.
* Arşiv verileri için: Faturalar ayda bir yedekleme hizmetine gönderin.

Bu tetikleyici, birçok desen, örneğin destekler:

* Hemen çalıştırma ve yineleme her *n* saniye, dakika, saat, gün haftalar veya aylar sayısı.
* Belirli bir zamanda başlatmak sonra çalıştırın ve yineleme her *n* saniye, dakika, saat, gün, hafta veya ayda sayısı.
* Çalıştırın ve bir veya daha fazla zaman her gün 8:00:00 ve 17:00:00 Örneğin, tekrarlayın.
* Çalıştırın ve her hafta, ancak yalnızca Cumartesi ve Pazar gibi belirli bir gün için yineleyin.
* Çalıştırın ve her hafta, ancak yalnızca belirli günleri ve saatleri, Pazartesi ile Cuma günleri saat 8: 00'da ve 17:00 gibi için yineleyin.

Yinelenme tetikleyici her etkinleştirildiğinde Logic Apps oluşturur ve mantıksal uygulama iş akışınızı yeni bir örneğini çalıştırır. 

Mantıksal uygulamanızı tetikleyecek ve gelecekte yalnızca bir kez çalıştırmak için bkz: [işler yalnızca bir kez çalıştır](#run-once) bu konuda.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Mantıksal uygulamanız için yineleme tetikleyicisi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantıksal uygulama oluşturma veya bilgi [boş bir mantıksal uygulama oluşturma işlemini](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Logic Apps Tasarımcısı'nda, arama kutusunun altındaki göründükten sonra seçin **tüm**. Arama kutusuna filtreniz olarak "yinelenme" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Yinelenme - zamanlama** 

   !["– Zamanlama yinelenme" tetikleyicisini seçin](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Bu tetikleyici, mantıksal uygulamanızı ilk adımı bir sunulmuştur.

3. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, her hafta, iş akışınızı çalıştırmak için bu özellikleri ayarlayın. 

   ![Kümesi aralığı ve sıklığı](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Daha fazla zamanlama seçeneklerini seçin **Gelişmiş Seçenekleri Göster**. 

   ![Diğer seçenekler](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. Artık bu seçenekler ayarlayabilirsiniz: 

   * Başlangıç tarihi ve saati tetikleme adımını ayarlayın. 
   Başlangıç tarihi ve saati belirtirseniz, bir saat dilimi de uygulayabilirsiniz. 

   * "Day" veya "Week" sıklığı için'i seçerseniz yinelenme için belirli saatler seçebilirsiniz. 

   * "Week" seçeneğini belirlerseniz, belirli gün haftanın çok seçebilirsiniz.
   
   ![Gelişmiş zamanlama seçenekleri](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Örneğin, 4 Eylül 2017, Pazartesi, bugün olduğunu varsayın. 
   Aşağıdaki yinelenme tetikleyicisini etkinleşmez *herhangi erken* başlangıç tarihi ve saati olduğu 18 Eylül 2017, Pazartesi 08: 00'te Pasifik saati. 
   Yinelenme zamanlaması 10:30 AM, ancak ayarlanmış 12:30 PM ve 2:30 PM yalnızca Pazartesi günleri. Bu nedenle ilk kez tetiklenir ve bir mantıksal uygulama iş akışı örneği oluşturur, 10: 30'da olduğu. 
   Başlangıç iş ne zaman hakkında daha fazla bilgi için bkz: [start zamanı örnekleri](#start-time).
   12:30 PM ve 2:30 PM, gelecekteki çalışmalarını aynı gün gerçekleşir. 
   Her yineleme, kendi iş akışı örneği oluşturur. Bundan sonra tüm yeniden sonraki Pazartesi tüm zamanlama tekrarlar. 
   [*Diğer bir örnek örnekleri nelerdir?*](#example-recurrences)

   ![Gelişmiş zamanlama örneği](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > Yalnızca "Day" veya "Week" sıklığı seçtiğinizde, belirtilen yinelenme için Önizleme tetikleyici gösterir.
   
6. Şimdi eylemlerle kalan iş akışınızı oluşturun veya denetim ifadeleri akış. Ekleyebileceğiniz diğer eylemler için bkz. [Bağlayıcılar](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Tetikleyici ayrıntıları

Bu özellikler için yineleme tetikleyicisi yapılandırabilirsiniz.

| Ad | Gerekli | Özellik adı | Tür | Açıklama | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Sıklık** | Evet | frequency | String | Yineleme için zaman birimi: **İkinci**, **dakika**, **saat**, **gün**, **hafta**, veya **ay** | 
| **Aralık** | Evet | interval | Tamsayı | İş akışı sıklığı temel alarak çalışan ne sıklıkta açıklar pozitif bir tamsayı. <p>Aralığı için varsayılan değer 1'dir. Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 12.000 1 saat </br>-Dakikası: 1-72,000 dakika </br>-Saniye: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve "Month" sıklığıdır yinelenme her 6 ayda olur. | 
| **Saat dilimi** | Hayır | timeZone | String | Bu tetikleyiciyi kabul etmez çünkü yalnızca bir başlangıç zamanı belirttiğinizde geçerlidir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini seçin. | 
| **Başlangıç saati** | Hayır | startTime | String | Bir başlangıç zamanı şu biçimde belirtin: <p>YYYY-MM-ddTHH bir saat dilimi seçerseniz <p>-veya- <p>YYYY-AA-saat dilimi seçmezseniz ssZ <p>Örneğin, 18 Eylül 2017 2: 00'da isterseniz, ardından belirtin "2017-09-18T14:00:00" Pasifik saati gibi bir saat dilimi seçin. Ya da belirtin "2017-09-18T14:00:00Z" olmadan bir saat dilimi. <p>**Not:** Bu başlangıç zamanı izlemelidir [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçiminde](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi seçmezseniz, sonunda boşluk olmadan "Z" harfi eklemeniz gerekir. Bu "Z" eş değeri başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlamalar için ilk yinelenme, başlangıç zamanıdır sırada karmaşık zamanlamalar için tetikleyici başlangıç saatinden herhangi bir erken etkinleşmez. [*Başlangıç tarihi ve saati kullanabilirim, yolları nelerdir?*](#start-time) | 
| **Şu günlerde** | Hayır | weekDays | Dize veya dize dizisi | "Week" seçeneğini belirlerseniz, iş akışını çalıştırmak istediğiniz bir veya daha fazla gün seçebilirsiniz: **Pazartesi**, **Salı**, **Çarşamba**, **Perşembe**, **Cuma**, **Cumartesi**, ve **Pazar** | 
| **Şu saatlerde** | Hayır | hours | Tamsayı veya tamsayı dizisi | "Day" veya "Week" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 23 iş akışını çalıştırmak istediğinizde günün saat seçebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtin, 10 AM, PM 12 ve 2 Pasifik saat işaretlerinde olarak alırsınız. | 
| **Şu dakikalarda** | Hayır | minutes | Tamsayı veya tamsayı dizisi | "Day" veya "Week" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 59 arasında iş akışını çalıştırmak istediğinizde saat dakika seçebilirsiniz. <p>Örneğin, "30" dakika işareti belirtebilirsiniz ve önceki örnekte için günün saatlerini kullanarak 10:30 AM, alın 12:30 PM ve 2:30 PM. | 
||||| 

## <a name="json-example"></a>JSON örneği

İşte bir örnek [yinelenme tetikleyicisi tanımı](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger):

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
         "startTime": "2017-09-07T14:00:00",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="faq"></a>SSS

<a name="run-once"></a>

**S:** Mantıksal uygulama gelecekte yalnızca bir kez çalıştırılacak istersem? </br>
**C:** Mantıksal uygulamanızı tetikleyecek ve yinelenen olmadan bir kez çalıştırmak için kullanabileceğiniz **Zamanlayıcı: Bir kez işlerinizi** şablonu. Yeni bir mantıksal uygulama oluşturduktan sonra ancak Logic Apps Tasarımcısı'nda altında açmadan önce **şablonları** bölümü, gelen **kategori** listesinden **zamanlama**ve ardından seçin Şablonu:

![Seçin "Zamanlayıcı: İşleri bir kez çalıştır"şablonu](./media/connectors-native-recurrence/choose-run-once-template.png)

Ya da boş mantıksal uygulama şablonunu kullanıyorsanız, mantıksal uygulamanız ile başlayın **olduğunda bir HTTP isteği alındığında - istek** tetikleyici. Tetikleyici başlangıç zamanı, bir parametre olarak geçiriyoruz. Sonraki adım için ekleme **Geciktir: zamanlama** eylem ve sonraki eylem çalışmaya başladığında zaman sağlamak.

<a name="example-recurrences"></a>

**S:** Diğer örnek yineleme zamanlaması nelerdir? </br>
**C:** Daha fazla örnekleri aşağıda verilmiştir:

| Yineleme | Interval | Sıklık | Başlangıç saati | Şu günlerde | Şu saatlerde | Şu dakikalarda | Not |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| (Hiçbir başlangıç tarihi ve saati) her 15 dakikada bir Çalıştır | 15 | Dakika | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlamayı hemen başlar ve son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. | 
| (Başlangıç tarihi ve saati ile) 15 dakikada bir Çalıştır | 15 | Dakika | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. | 
| Her saat saat (başlangıç tarihi ve saati ile) çalıştırın | 1 | Saat | *startDate*Thh:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati. Gelecekteki yinelenme "00" dakika işaretinde saatte bir çalıştır. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. | 
| Her gün (başlangıç tarihi ve saati yok) saatte bir Çalıştır | 1 | Saat | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlama, hemen başlar ve son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. | 
| Her gün (başlangıç tarihi ve saati ile) saatte bir Çalıştır | 1 | Saat | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. | 
| 15 dakikada bir saati aşan, her saat (başlangıç tarihi ve saati ile) çalıştırın. | 1 | Saat | *startDate*T00:15:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati, 00: 15'da, 01:15:00, 02:15:00, çalışan ve benzeri. | 
| Son saat başı (başlangıç tarihi ve saati yok) 15 dakikada bir Çalıştır | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | 00: 15'da, 01:15:00, 02:15:00, bu zamanlamaya çalışır ve benzeri. Ayrıca, bu zamanlamayı sıklık değeri "Hour" ve "15" dakika içeren bir başlangıç saati eşdeğerdir. | 
| 15 dakikalık işaretinde (başlangıç tarihi ve saati yok) 15 dakikada bir Çalıştır | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlama, sonraki 15 dakika işareti belirtilene kadar başlamaz. | 
| 8: 00'da günde (başlangıç tarihi ve saati yok) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | {none} | Bu zamanlama, 8: 00'da her gün belirtilen bir zamanlamaya göre çalıştırır. | 
| 8: 00'da günde (başlangıç tarihi ve saati ile) çalıştırma | 1 | Gün | *startDate*T08:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlama, belirtilen başlangıç saatini temel alan 8: 00'da her gün çalışır. | 
| 8: 30'da günde (başlangıç tarihi ve saati yok) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | 30 | Bu zamanlama, 8: 30'da her gün belirtilen bir zamanlamaya göre çalıştırır. | 
| 8:30 AM her gün (başlangıç tarihi ve saati ile) çalışma | 1 | Gün | *startDate*T08:30:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlama, saat 8: 30'da belirtilen başlangıç tarihinde başlar. | 
| 8:30 AM ve 4:30 PM her gün olarak çalışır | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30 | | 
| 8: 30'da, 8: 45'te Çalıştır 4:30 PM ve 16:45 saatleri her gün | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30, 45 | | 
| Her Cumartesi 5 saat (başlangıç tarihi ve saati yok) çalıştırın | 1 | Hafta | {none} | "Saturday" | 17 | 0 | Bu zamanlamayı her Cumartesi 5: 00'da çalışır. | 
| Her Cumartesi saat 17: 00 (başlangıç tarihi ve saati ile) çalıştırın | 1 | Hafta | *startDate*T17:00:00Z | "Saturday" | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati, bu durumda, 9 Eylül 2017 saat 17:00:00. Her Cumartesi, gelecekteki tekrarlar 5: 00'da çalıştırın. | 
| Her Salı, Perşembe saat 17: 00 çalıştırın | 1 | Hafta | {none} | "Salı", "Perşembe" | 17 | {none} | Bu zamanlamayı her Salı ve Perşembe 5: 00'da çalışır. | 
| Çalışma saatleri sırasında saatte bir Çalıştır | 1 | Hafta | {none} | Cumartesi ve Pazar hariç tüm gün seçin. | Günün istediğiniz saatleri seçin. | Her dakika istediğiniz saati seçin. | Çalışma saatlerinizi 5:00 PM için 8: 00'da, örneğin, ardından "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" günün saati seçin. <p>Çalışma saatlerinizi 8:30:00 için 5:30 PM, gün ve "30" önceki saat saat, dakika seçin. | 
| Hafta sonu günde bir kez çalıştır | 1 | Hafta | {none} | "Saturday", "Sunday" | Günün istediğiniz saatleri seçin. | Saatin herhangi bir dakika uygun olanını seçin. | Bu zamanlamayı her Cumartesi ve Pazar belirtilen zamanlamayla çalıştırır. | 
| İki haftada Pazartesi günleri yalnızca 15 dakikada bir Çalıştır | 2 | Hafta | {none} | "Pazartesi" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlamayı her 15 dakikada işaretinde her Pazartesi çalıştırır. | 
| Ayda bir gün için saatte bir Çalıştır | 1 | Ay | {bkz. Not} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {bkz. Not} | Başlangıç tarihi ve saati belirtmezseniz, bu zamanlamanın oluşturulma tarihi ve saati kullanır. Yinelenme zamanlaması için dakika denetlemek için bir başlangıç zamanı saat, dakika belirtin veya oluşturma zamanı kullanın. Başlangıç zamanı veya oluşturma zamanı Sabah 8:25 ise, bu zamanlamaya 8: 25'da, 25 9: AM, 25 10: AM, örneğin, çalışan ve benzeri. | 
||||||||| 

<a name="start-time"></a>

**S:** Başlangıç tarihi ve saati kullanabilirim, yolları nelerdir? </br>
**C:** Yineleme başlangıç tarihi ve saati ile nasıl kontrol edebilir ve Logic Apps altyapısı bu tekrarlar nasıl yürütür gösteren bazı desenleri şunlardır:

| Başlangıç saati | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme | 
| ---------- | --------------------------- | ------------------------ | 
| {none} | İlk iş yükü anında çalışır. <p>Son çalıştırma saatini temel alan gelecekteki iş yüklerini çalışır. | İlk iş yükü anında çalışır. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. | 
| Başlangıç zamanı geçmişte | Belirtilen başlangıç saati ve son çalıştırma atar kez göre çalışma süreleri hesaplar. Çalışma zamanı şu anda sonraki geleceğe ilk iş yükü çalıştırır. <p>Son çalışma zamanı'den hesaplamaları göre gelecekteki iş yüklerini çalışır. <p>Daha fazla açıklama için bu tablodan sonraki örneğe bakın. | İlk iş yükü çalıştıran *başlamaz* başlangıç saatinden başlangıç zamanından hesaplanan zamanlamaya göre. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. <p>**Not:** Bir yineleme ile ilgili bir zamanlama belirtin, ancak saat veya dakika boyunca zamanlama belirtmezseniz, gelecekteki çalışma zamanları saat veya dakika, ilk çalışma zamanından sırasıyla kullanılarak hesaplanır. | 
| Başlangıç zamanı gelecekte veya güncel | İlk iş yükü, belirtilen başlangıç zamanında çalışır. <p>Son çalışma zamanı'den hesaplamaları göre gelecekteki iş yüklerini çalışır. | İlk iş yükü çalıştıran *başlamaz* başlangıç saatinden başlangıç zamanından hesaplanan zamanlamaya göre. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. <p>**Not:** Bir yineleme ile ilgili bir zamanlama belirtin, ancak saat veya dakika boyunca zamanlama belirtmezseniz, gelecekteki çalışma zamanları saat veya dakika, ilk çalışma zamanından sırasıyla kullanılarak hesaplanır. | 
||||

**Örneğin, son bir başlangıç saati yinelenme olduğunda ancak zamanlama** 

| Başlangıç saati | Geçerli zaman | Yineleme | Zamanlama |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09-**07**T14:00:00Z | 2017-09-**08**T13:00:00Z | Her 2 gün | {none} | 
||||| 

Bu senaryoda, mantıksal altyapısı hesaplar çalışma zamanları başlangıç saatini temel alan uygulamalar çalışma zamanları ve sonraki gelecek başlangıç zamanı ilk çalıştırma için kullandığı geçen atar. Bu ilk çalıştırma başlangıç zamanından hesaplanan zamanlamaya gelecekteki çalışmalarını temel alır. Bu Yinelenmenin nasıl göründüğünü aşağıda verilmiştir:

| Başlangıç saati | İlk çalıştırma | Gelecekteki çalışma zamanları | 
| ---------- | ------------ | ---------- | 
| 2017-09 -**07** , 2:00 PM | 2017-09 -**09** , 2:00 PM | 2017-09 -**11** , 2:00 PM </br>2017-09 -**13** , 2:00 PM </br>2017-09 -**15** , 2:00 PM </br>ve benzeri...
||||

Bu nedenle bu senaryo için belirttiğiniz başlangıç geçmiş ne kadar olursa olsun zaman, örneğin, 2017-09 -**05** 2: 00'dan en veya 2017-09 -**01** 2: 00'te, ilk çalışma zamanında aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [İş akışı eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Bağlayıcılar](../connectors/apis-list.md)
