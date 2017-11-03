---
title: "Zamanlama görevleri ve düzenli olarak çalışan iş akışları - Azure Logic Apps | Microsoft Docs"
description: "Oluşturma ve düzenli olarak çalışan görevler, Eylemler, iş akışları, işlemleri ve logic apps ile iş yüklerini zamanlama"
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 77567302c529e6e06e58534ffc9db44c9a85bdb7
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="schedule-tasks-and-workflows-that-run-regularly-with-logic-apps"></a>Görevleri zamanlayın ve logic apps ile düzenli olarak çalıştırılan iş akışları

Görevler, Eylemler, iş yükleri veya düzenli olarak çalıştırılan işlemlerin zamanlamak için ile başlayan bir mantıksal uygulama iş akışı oluşturabilirsiniz **çizelgesi - yinelenme** [tetikleyici](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts). Bu tetikleyici ile bir tarih ve saat yinelenme ve bu örnekler ve daha fazlası gibi görevleri gerçekleştirmek için bir yineleme zamanlaması başlatmak için ayarlayabilirsiniz:

* İç Veri Al: [SQL saklı yordamı çalıştırdığınızda](../connectors/connectors-create-api-sqlazure.md) her gün.
* Dış Veri Al: hava raporları NOAA 15 dakikada bir çekme.
* Rapor verileri: tüm siparişleri belirli bir miktar büyük bir özeti geçen hafta içinde e-posta.
* İşlem verileri: sıkıştırma bugün yükledi görüntüleri saatlerde her hafta içi günü.
* Veri temizleme: tüm tweet'leri üç aydan daha eski silin.
* Arşiv verileri: itme faturalar yedekleme hizmetine her ay.

Bu tetikleyici birçok desenleri, örneğin destekler:

* Hemen çalıştırmak ve yineleyin her  *n*  saniye, dakika, saat, gün, hafta veya ay sayısı.
* Belirli bir zamanda başlatmak sonra çalıştırın ve yineleyin her  *n*  saniye, dakika, saat, gün, hafta veya ay sayısı.
* Çalıştırın ve bir veya daha fazla zaman her gün 8:00 ve 17: 00'dan Örneğin, yineleyin.
* Çalıştırın ve her hafta, ancak yalnızca Cumartesi ve Pazar gibi belirli günler için yineleyin.
* Çalıştırın ve her hafta, ancak yalnızca belirli günleri ve saatleri Pazartesi-Cuma 8:00:00 ve 17: 00'dan gibi için yineleyin.

Yineleme tetikleyici her başlatıldığında, Logic Apps oluşturur ve logic app akışınızı yeni bir örneğini çalıştırır.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı ile başlayabilirsiniz](https://azure.microsoft.com/free/). Ya da [Kullandıkça Öde aboneliğine kaydolabilirsiniz](https://azure.microsoft.com/pricing/purchase-options/).

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/logic-apps-create-a-logic-app.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Bir yineleme tetikleyici mantığı uygulamanıza ekleme

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş mantıksal uygulama oluşturma veya bilgi [boş mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

2. Arama kutusuna, Logic Apps Tasarımcısı görüntülendikten sonra filtre olarak "recurrence" girin. Seçin **çizelgesi - yinelenme** tetikleyici. 

   ![Zamanlama - yinelenme tetikleyici](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Bu tetikleyici mantıksal uygulamanızı ilk adımda sunulmuştur.

3. Yinelenme aralığını ve sıklığını ayarlayın. Bu örnekte, her hafta, iş akışınızı çalıştırmak için bu özellikleri ayarlayın. 

   ![Kümesi aralığı ve sıklığı](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Daha fazla zamanlama seçeneklerini seçin **Gelişmiş Seçenekleri Göster**. 

   ![Daha fazla seçenek](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. Şimdi bu seçenekleri ayarlayabilirsiniz: 

   * Başlangıç tarihi ve tetikleyici tetikleme zamanını ayarlayın. 
   Başlangıç tarihi ve saati belirtirseniz, ayrıca bir saat dilimi uygulayabilirsiniz. 

   * "Gün" veya "Hafta" sıklığı için seçerseniz, yineleme için belirli saatler seçebilirsiniz. 

   * "Hafta" seçeneğini belirlerseniz, haftanın belirli günlerine çok seçebilirsiniz.
   
   ![Gelişmiş zamanlama seçenekleri](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Örneğin, bu bugün 4 Eylül 2017, Pazartesi günü olduğunu varsayın. 
   Aşağıdaki yinelenme tetikleyici harekete değil *herhangi erken* başlangıç tarihi ve saati olduğu 18 Eylül 2017, Pazartesi 8:00 AM Pasifik Saatine göre. 
   Bununla birlikte, 10:30:00 için yineleme ayarlamaktır 12:30 Şöyle ve 2:30 PM Pazartesi yalnızca '. Bu nedenle ilk kez tetikleyici başlatılır ve bir mantıksal uygulama iş akışı örneği oluşturur 10: 30'da değil. 
   Başlangıç iş ne zaman hakkında daha fazla bilgi için bkz: [saat örnekler Başlat](#start-time).
   Gelecekteki çalıştırır 12:30 Şöyle ve 2:30 Şöyle aynı gün gerçekleşir. 
   Her yineleme kendi iş akışı örneği oluşturur. Bundan sonra tüm zamanlama tüm yeniden sonraki Pazartesi tekrarlar. 
   [*Diğer bir örnek örnekleri nelerdir?*](#example-recurrences)

   ![Gelişmiş zamanlama örneği](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > Yalnızca "Gün" veya "Hafta" sıklığı seçtiğinizde tetikleyici için belirtilen yinelenme Önizleme gösterir.
   
6. Şimdi kalan akışınızı eylemleri ile oluşturabilir veya denetim ifadeleri akış. Ekleyebileceğiniz diğer eylemler için bkz: [Bağlayıcılar](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Tetikleyici ayrıntıları

Bu özellikler yineleme tetikleyici için yapılandırabilirsiniz.

| Ad | Gerekli | Özellik adı | Tür | Açıklama | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Sıklık** | Evet | frequency | Dize | Yineleme için zaman birimi: **ikinci**, **Minute**, **saat**, **gün**, **hafta**, veya  **Ay** | 
| **Aralığı** | Evet | interval | Tamsayı | İş akışı sıklığı temel alarak çalışan ne sıklıkta açıklar pozitif bir tamsayı. <p>Varsayılan aralığı 1'dir. Minimum ve maksimum aralıkları şunlardır: <p>-Ay: 1-16 ay </br>-Günü: 1-500 gün </br>-Saat: 1-12.000 saatleri </br>-Dakika: 1-72,000 dakika </br>-İkinci: 1-9,999,999 saniye<p>Örneğin, aralığı 6'dır ve sıklığı "Ay" ise, yineleme 6 ayda olur. | 
| **Saat dilimi** | Hayır | saat dilimi | Dize | Bu tetikleyici kabul etmez olduğundan yalnızca bir başlangıç saati belirttiğinizde uygulanır [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Uygulamak istediğiniz saat dilimini seçin. | 
| **Başlangıç zamanı** | Hayır | startTime | Dize | Başlangıç zamanı şu biçimde girin: <p>YYYY-MM-ddTHH bir saat dilimi seçerseniz <p>-veya- <p>YYYY-MM-saat dilimi seçmezseniz: ssZ <p>2: 00'dan 18 Eylül 2017 istiyorsanız, bu nedenle örneğin sonra belirtin "2017-09-18T14:00:00" Pasifik saati gibi bir saat dilimi seçin. Ya da belirtin "2017-09-18T14:00:00Z" bir saat dilimi olmadan. <p>**Not:** bu başlangıç saati izlemelisiniz [ISO 8601 tarih saat belirtimi](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) içinde [UTC tarih saat biçimini](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), olmadan bir [UTC farkı](https://en.wikipedia.org/wiki/UTC_offset). Bir saat dilimi seçmezseniz, boşluk olmadan sonunda harf "Z" eklemeniz gerekir. Bu "Z" eşdeğer başvuruyor [Denizcilik zaman](https://en.wikipedia.org/wiki/Nautical_time). <p>Basit zamanlama için başlangıç saatini ilk oluşum olduğu sırada karmaşık zamanlamalar, tetikleyici herhangi erken başlangıç saatinden harekete değil. [*I başlangıç tarihini ve saatini kullanabileceğiniz yollar nelerdir?*](#start-time) | 
| **Şu günlerde** | Hayır | weekDays | Dize veya dize dizisi | "Hafta" seçeneğini seçerseniz, iş akışını çalıştırmak istediğinizde bir veya daha fazla gün seçebilirsiniz: **Pazartesi**, **Salı**, **Çarşamba**, **Perşembe** , **Cuma**, **Cumartesi**, ve **Pazar** | 
| **Bu saat** | Hayır | hours | Tamsayı veya tamsayı dizisi | "Gün" veya "Hafta" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 23 iş akışını çalıştırmak istediğinizde gün saat seçebilirsiniz. <p>Örneğin, "10", "12" ve "14" belirtin, 10'da, 12 PM ve 2 PM saat işaretleri olarak alırsınız. | 
| **Bu dakika** | Hayır | minutes | Tamsayı veya tamsayı dizisi | "Gün" veya "Hafta" seçeneğini belirlerseniz, bir veya daha fazla tamsayılar 0 ile 59 arasında iş akışını çalıştırmak istediğinizde saat dakika seçebilirsiniz. <p>Örneğin, "30" olarak dakika işaretle belirtebilirsiniz ve 10:30:00, almak için günün saati önceki örneği kullanarak 12:30 Şöyle ve 2:30 PM. | 
||||| 

## <a name="json-example"></a>JSON örneği

Bir örnek yinelenme tetikleyici tanımı aşağıda verilmiştir:

``` json
{
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
}
```

## <a name="faq"></a>SSS

<a name="example-recurrences"></a>

**S:** diğer örnek yineleme zamanlamaları nelerdir? </br>
**Y:** diğer örnekler şunlardır:

| Yineleme | aralığı | Sıklık | Başlangıç zamanı | Şu günlerde | Şu saatlerde | Şu dakikalarda | Not |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| 15 dakikada bir (hiçbir başlangıç tarihi ve saati) çalıştırın | 15 | Dakika | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlatıldıktan hemen sonra son çalışma zamanı temel alınarak gelecekteki tekrarları hesaplar. | 
| 15 dakikada bir (başlangıç tarihi ve saati ile) çalıştırın | 15 | Dakika | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamıyorsa *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalışma zamanı temel alınarak gelecekteki tekrarları hesaplar. | 
| Her saat, saat (başlangıç tarihi ve saati) çalıştırın | 1 | Saat | *startDate*Thh:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamıyorsa *herhangi erken* belirtilen dönemden başlangıç tarihi ve saati. Gelecekteki tekrarları saatte "00" dakika işaretinde çalıştırın. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlamayı yalnızca bir gününü haftalık veya aylık bir gün sırasıyla çalışır. | 
| Her saat, her gün (hiçbir başlangıç tarihi ve saati) çalıştırın | 1 | Saat | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlamayı hemen başlar ve son çalışma zamanı temel alınarak gelecekteki tekrarları hesaplar. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlamayı yalnızca bir gününü haftalık veya aylık bir gün sırasıyla çalışır. | 
| Her saat, her gün (başlangıç tarihi ve saati ile) Çalıştır | 1 | Saat | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamıyorsa *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalışma zamanı temel alınarak gelecekteki tekrarları hesaplar. <p>Sıklık "Hafta" veya "Ay" ise, bu zamanlamayı yalnızca bir gününü haftalık veya aylık bir gün sırasıyla çalışır. | 
| 15 dakikada bir saatten, her saat (başlangıç tarihi ve saati ile) çalıştırın | 1 | Saat | *startDate*T00:15:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamıyorsa *herhangi erken* belirtilen tarih ve saat 00: 15'de, 1: 15'da, 2: 15'da, çalışan, başlatmak ve benzeri. | 
| 15 dakikada bir saatte (hiçbir başlangıç tarihi ve saati) saati aşan çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | 00: 15'de, 1: 15'da, 2: 15'da, bu zamanlamaya çalışır ve benzeri. Ayrıca, bu zamanlamanın sıklığını "Saat" ve "15" dakika ile bir başlangıç saati eşdeğerdir. | 
| 15 dakikalık işaretinde (hiçbir başlangıç tarihi ve saati) 15 dakikada çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Sonraki 15 dakika işareti belirtilen kadar bu zamanlamanın başlamıyor. | 
| 8: 00'da her gün (hiçbir başlangıç tarihi ve saati) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | {none} | Bu zamanlamayı 8: 00'da, her gün belirtilen bir zamanlamaya göre çalışır. | 
| 8: 00'da (ile başlangıç tarihi ve saati) her gün çalıştırın | 1 | Gün | *startDate*T08:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı belirtilen başlangıç zamanı temel alınarak 8: 00'da, her gün çalışır. | 
| 8:30:00 her gün (hiçbir başlangıç tarihi ve saati) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | 30 | Bu zamanlamayı 8: 30'te, her gün belirtilen bir zamanlamaya göre çalışır. | 
| 8: 30'da (ile başlangıç tarihi ve saati) her gün çalıştırın | 1 | Gün | *startDate*T08:30:00Z | {kullanılamaz} | {none} | {none} | 8: 30'da belirtilen başlangıç tarihi bu zamanlamanın başlatır. | 
| 8:30:00 ve 4:30 Şöyle her gün olarak çalışır | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30 | | 
| 8: 30'de, 8: 45'te, çalıştırmak 4:30 Şöyle ve 4:45 PM her gün | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30, 45 | | 
| Her Cumartesi 5'e (hiçbir başlangıç tarihi ve saati) çalıştırın | 1 | Hafta | {none} | "Cumartesi" | 17 | 00 | Bu zamanlamayı her Cumartesi 5: 00'da çalışır. | 
| Her Cumartesi 18: 00 (başlangıç tarihi ve saati ile) çalışma | 1 | Hafta | *startDate*T17:00:00Z | "Cumartesi" | {none} | {none} | Bu zamanlamayı başlamıyorsa *herhangi erken* belirtilen dönemden başlangıç tarihi ve saati, bu durumda, 9 Eylül 2017 17: 00'dan en. Gelecekteki tekrarları her Cumartesi 17: 00'dan çalıştırın. | 
| Her Salı, Perşembe 17: 00 saatleri sırasında çalıştırın | 1 | Hafta | {none} | "Salı", "Perşembe" | 17 | {none} | Bu zamanlamayı her Salı ve Perşembe 5: 00'da çalışır. | 
| Her saat çalışma saatleri sırasında çalıştırın | 1 | Hafta | {none} | Cumartesi ve Pazar dışındaki tüm günleri seçin. | İstediğiniz gün saat seçin. | Tüm dakika istediğiniz saat seçin. | Çalışma saatleri 8:00:00 ila 5:00 varsa, örneğin, ardından "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" günün saati seçin. <p>Çalışma saatleri 5:30 için 8:30:00 PM varsa, önceki saat gün ve "30" saat dakika seçin. | 
| Hafta sonu günde bir kez çalıştır | 1 | Hafta | {none} | "Cumartesi", "Pazar" | İstediğiniz gün saat seçin. | Tüm dakikası saati uygun şekilde seçin. | Bu zamanlamayı her Cumartesi ve Pazar belirtilen zamanlamayla çalışır. | 
| İki haftada Pazartesi üzerinde yalnızca her 15 dakikada çalıştırın | 2 | Hafta | {none} | "Pazartesi" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlamayı her diğer her 15 dakikalık işaretinde Pazartesi çalışır. | 
| Her saat ayda bir gün için Çalıştır | 1 | Ay | {bkz. Not} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {bkz. Not} | Başlangıç tarihi ve saati belirtmezseniz, bu zamanlamanın oluşturulma tarihi ve saati kullanır. Yinelenme zamanlaması için dakika denetlemek için bir başlangıç saati saat dakika belirtin veya oluşturulma zamanı kullanın. Başlangıç saati veya oluşturma zamanında 8:25 AM ise, bu zamanlamaya 8: 25'de, 9:25 AM, 10:25 AM, örneğin, çalışan ve benzeri. | 
||||||||| 

<a name="start-time"></a>

**S:** ı başlangıç tarihini ve saatini kullanabileceğiniz yollar nelerdir? </br>
**Y:** yineleme başlangıç tarihi ve saati ile nasıl kontrol edebilir ve nasıl Logic Apps altyapısı bu tekrarları yürütür Göster bazı desenleri şunlardır:

| Başlangıç zamanı | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme | 
| ---------- | --------------------------- | ------------------------ | 
| {none} | İlk iş yükü anında çalışır. <p>Son çalışma zamanı temel alınarak gelecekteki iş yüklerini çalıştırır. | İlk iş yükü anında çalışır. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalıştırır. | 
| Başlangıç zamanı geçmişte | Belirtilen başlangıç saati ve Çalıştır iptali son kez göre çalışma zamanları hesaplar. İlk iş yükü çalışma zamanını sonraki gelecekteki çalışır. <p>Son çalışma zamanını'den hesaplamaları göre gelecekteki iş yüklerini çalıştırır. <p>Daha fazla açıklama için bu tabloda aşağıdaki örneğe bakın. | İlk iş yükü çalıştıran *Hayır er* başlangıç saati hesaplanan zamanlamasına göre başlangıç saatinden. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalıştırır. <p>**Not:** yineleme sahip bir zamanlama belirtin, ancak saat veya dakika zamanlama için belirtmeyin durumunda gelecekteki çalışma zamanları saat veya dakika, sırasıyla, ilk çalışma saati kullanılarak hesaplanır. | 
| Mevcut veya gelecekteki başlangıç zamanı | İlk iş yükünü belirtilen başlama saatinde çalışır. <p>Son çalışma zamanını'den hesaplamaları göre gelecekteki iş yüklerini çalıştırır. | İlk iş yükü çalıştıran *Hayır er* başlangıç saati hesaplanan zamanlamasına göre başlangıç saatinden. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalıştırır. <p>**Not:** yineleme sahip bir zamanlama belirtin, ancak saat veya dakika zamanlama için belirtmeyin durumunda gelecekteki çalışma zamanları saat veya dakika, sırasıyla, ilk çalışma saati kullanılarak hesaplanır. | 
||||

**Örneğin, yineleme ancak hiçbir zamanlama ile son başlangıç zamanı** 

| Başlangıç zamanı | Geçerli saat | Yineleme | Zamanlama |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09 -**07**T14:00:00Z | 2017-09 -**08**T13:00:00Z | Her 2 gün | {none} | 
||||| 

Bu senaryoda, mantıksal altyapısı hesaplar çalışma zamanları başlangıç zamanı temel alınarak uygulamaları Geçmiş çalışma zamanları ve sonraki gelecekteki başlangıç zamanı ilk çalıştırma için kullandığı atar. Bu ilk çalıştırmada sonra gelecekteki çalıştırır başlangıç saati hesaplanan zamanlama dayanır. Bu yineleme nasıl göründüğünü aşağıda verilmiştir:

| Başlangıç zamanı | İlk çalıştırma | Çalışma zamanları gelecek | 
| ---------- | ------------ | ---------- | 
| 2017-09 -**07** 2:00 PM adresindeki | 2017-09 -**09** 2:00 PM adresindeki | 2017-09 -**11** 2:00 PM adresindeki </br>2017-09 -**13** 2:00 PM adresindeki </br>2017-09 -**15** 2:00 PM adresindeki </br>ve dahası...
||||| 

Bu nedenle bu senaryo için ne kadar belirttiğiniz başlangıç geçmişte geçtiğinden bağımsız zaman, örneğin, 2017-09 -**05** 2:00 PM adresindeki veya 2017-09 -**01** 2: 00'da, ilk çalışma zamanında aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [İş akışı eylemleri ve tetikleyicileri](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Bağlayıcılar](../connectors/apis-list.md)
