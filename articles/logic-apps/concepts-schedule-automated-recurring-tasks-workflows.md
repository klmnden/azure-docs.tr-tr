---
title: Yinelenen görevleri ve Azure Logic apps'te iş akışları zamanlama
description: Yinelenen otomatik görevler, süreçleri ve Azure Logic Apps ile iş akışlarını zamanlama hakkında bir genel bakış
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 7f15dc5b28a44dc8405e2f0524913e6012ebe380
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66356039"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Zamanlama ve Azure Logic Apps ile yinelenen otomatik görevler, süreçleri ve iş akışları çalıştırma

Logic Apps oluşturma ve otomatik yinelenen görevleri ve işlemleri bir zamanlamaya göre çalıştırma yardımcı olur. Zamanlama türü Tetikleyicileri olan bir yerleşik yineleme veya kayan pencere tetikleyici ile başlar bir mantıksal uygulama iş akışı oluşturarak hemen sonraki bir zamanda veya yinelenen bir aralıkta görevler çalıştırabilirsiniz. HTTP veya HTTPS uç noktaları gibi içindeki hem de Azure dışındaki hizmetleri çağıran, Azure depolama ve Azure Service Bus gibi Azure hizmetlerinin iletiler yayınlayın veya bir dosya paylaşımına karşıya yüklenen dosyaları alın. Yineleme tetikleyicisi ile karmaşık zamanlamalar da ayarlayabilirsiniz ve Gelişmiş Görev çalıştırmaya yönelik tekrarlar. Yerleşik zamanlama Tetikleyicileri ve eylemleri hakkında daha fazla bilgi için bkz. [zamanlama ve otomatik olarak yinelenen bir çalıştırma, görevleri ve Azure Logic Apps ile iş akışlarını](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Tür çalıştırmak üzere görev gösteren bazı örnekler şunlardır:

* Her gün bir SQL saklı yordamı çalıştırmak gibi iç veri alın.

* Çekme hava durumu raporları NOAA her 15 dakikada gibi dış veri al

* E-posta için tüm siparişleri geçtiğimiz haftanın belirli bir süre daha büyük bir özeti gibi rapor verileri gönderin.

* Bugün sıkıştırma gibi verileri işlemek, hafta içi her gün saatlerde görüntüleri yükledi.

* Üç aydan eski tüm tweetleri silme gibi verileri temizleyin.

* 1: 00'da sonraki dokuz ay boyunca her gün bir yedekleme hizmeti için anında iletme faturalar gibi verileri arşivleme.

Zamanlama yerleşik Eylemler, sonraki eylemi çalıştırır, örneğin önce akışınızı duraklatmak için de kullanabilirsiniz:

* Durum güncelleştirmesi e-posta göndermek için bir hafta kadar bekleyin.

* İş akışı, HTTP çağrısı sonucu alma ve sürdürme önce tamamlanması zaman sahip oluncaya kadar gecikme.

Bu makalede yerleşik zamanlama Tetikleyicileri ve eylemleri için özellikler açıklanmaktadır.

## <a name="schedule-triggers"></a>Zamanlama Tetikleyicileri

Mantıksal uygulama iş akışınızı yineleme tetikleyicisi veya kayan pencere tetikleyicisi,'ı kullanarak, herhangi bir özel hizmet veya sistem, örneğin, Office 365 Outlook veya SQL Server ile ilişkili olmayan başlatabilirsiniz. Bu tetikleyiciler, başlatın ve akışınızı üzerinde belirtilen yinelenme aralığını ve sıklığını, kaç saniye, dakika ve hem Tetikleyiciler için saat veya gün, hafta veya yineleme tetikleyicisi için ay sayısı gibi seçtiğiniz çalıştırın. Saat dilimi yanı sıra başlangıç tarihi ve saati de ayarlayabilirsiniz. Her zaman bir tetikleyici, Logic Apps oluşturur ve mantıksal uygulamanızın yeni bir iş akışı örneği çalışır.

Bu Tetikleyiciler arasındaki farklar şunlardır:

* **Yineleme**: İş akışınızı düzenli zaman aralıklarında, belirtilen bir zamanlamaya göre çalıştırır. Yinelenme kaçırdıysanız, yineleme tetikleyicisi eksik yinelenme işlemiyor ancak yinelenme sonraki zamanlanmış aralıkta ile yeniden başlatır. Saat dilimi yanı sıra bir başlangıç tarihi ve saati belirtebilirsiniz. "Day" seçeneğini belirlerseniz, günün saatlerini ve her gün saat 2:30 dakika, saat, örneğin, belirtebilirsiniz. "Week" seçeneğini belirlerseniz, Çarşamba ve Cumartesi gibi haftanın günlerini de seçebilirsiniz. Daha fazla bilgi için [oluştur, zamanlama ve çalıştırma yinelenen görevleri ve yineleme tetikleyicisi ile iş akışlarını](../connectors/connectors-native-recurrence.md).

* **Kayan pencere**: Verileri sürekli öbekler halinde işlemek düzenli zaman aralıklarında akışınız çalışır. Yinelenme kaçırdıysanız, kayan pencere tetikleyicisi döner ve eksik yinelenme işler. Başlangıç tarihi ve saati, saat dilimi ve süre her yineleme iş akışınızda daha sonra gerçekleştirmeyi belirtebilirsiniz. Bu tetikleyici, gün, hafta ve ay, gün, saat, dakika, saat ve haftanın günlerini belirtmek için seçenekler yok. Daha fazla bilgi için [oluşturma, zamanlama ve çalıştırma yinelenen görevleri ve kayan pencere tetikleyicisi ile iş akışlarını](../connectors/connectors-native-sliding-window.md).

## <a name="schedule-actions"></a>Eylemleri zamanlama

Mantıksal uygulama iş akışınızı içinde herhangi bir işlem sonra Gecikmeli ve Gecikmeli kadar Eylemler sonraki eylem çalışmadan önce bekleyin, iş akışınızı yapmak için kullanabilirsiniz.

* **gecikme**: Belirtilen saniye, dakika, saat, gün, hafta veya ayda gibi zaman birimlerinin sayısı için sonraki eylemi çalıştırmak için bekleyin. Daha fazla bilgi için [sonraki eylem iş akışlarındaki gecikme](../connectors/connectors-native-delay.md).

* **Geciktir:** : Belirtilen tarih ve saate kadar sonraki eylemi çalıştırmak için bekleyin. Daha fazla bilgi için [sonraki eylem iş akışlarındaki gecikme](../connectors/connectors-native-delay.md).

## <a name="patterns-for-start-date-and-time"></a>Desenler için başlangıç tarihi ve saati

<a name="start-time"></a>

Yineleme başlangıç tarihi ve saati ile nasıl kontrol edebilir ve Logic Apps Hizmetleri bu tekrarlar nasıl çalıştığını gösteren bazı desenleri şunlardır:

| Başlangıç saati | Zamanlama olmadan yinelenme | (Yalnızca yineleme tetikleyicisi) zamanlama ile yinelenme |
|------------|-----------------------------|----------------------------------------------------|
| {none} | İlk iş yükü anında çalışır. <p>Son çalıştırma saatini temel alan gelecekteki iş yüklerini çalışır. | İlk iş yükü anında çalışır. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. |
| Başlangıç zamanı geçmişte | **Yinelenme** tetikleyici: Belirtilen başlangıç saati ve son çalıştırma atar kez göre çalışma süreleri hesaplar. Çalışma zamanı şu anda sonraki geleceğe ilk iş yükü çalıştırır. <p>Son çalışma zamanı'den hesaplamaları göre gelecekteki iş yüklerini çalışır. <p><p>**Kayan pencere** tetikleyici: Belirtilen başlangıç zamanı ve çalıştırma sekme son kez göre çalışma süreleri hesaplar. <p>Belirtilen başlangıç zamanından hesaplamaları temel gelecekteki iş yüklerini çalışır. <p><p>Daha fazla açıklama için bu tablodan sonraki örneğe bakın. | İlk iş yükü çalıştıran *başlamaz* başlangıç saatinden başlangıç zamanından hesaplanan zamanlamaya göre. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. <p>**Not:** Bir yineleme ile ilgili bir zamanlama belirtin, ancak saat veya dakika boyunca zamanlama belirtmezseniz, gelecekteki çalışma zamanları saat veya dakika, ilk çalışma zamanından sırasıyla kullanılarak hesaplanır. |
| Başlangıç zamanı gelecekte veya güncel | İlk iş yükü, belirtilen başlangıç zamanında çalışır. <p>Son çalışma zamanı'den hesaplamaları göre gelecekteki iş yüklerini çalışır. | İlk iş yükü çalıştıran *başlamaz* başlangıç saatinden başlangıç zamanından hesaplanan zamanlamaya göre. <p>Belirtilen bir zamanlamaya göre gelecekteki iş yüklerini çalışır. <p>**Not:** Bir yineleme ile ilgili bir zamanlama belirtin, ancak saat veya dakika boyunca zamanlama belirtmezseniz, gelecekteki çalışma zamanları saat veya dakika, ilk çalışma zamanından sırasıyla kullanılarak hesaplanır. |
||||

*Örneğin Başlangıç zamanı geçmiş ve yinelenme olduğunda ancak zamanlama*

1: 00'da 8 Eylül 2017 geçerli tarih ve saat olduğunu varsayın. Başlangıç tarihi ve saati 7 Eylül 2017'den 2:00, geçmiş ve 2 günde bir çalışır bir yinelenme PM, belirttiğiniz.

| Başlangıç saati | Geçerli saati | Yinelenme | Zamanlama |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09 -**07** , 2:00 PM) | 2017-09-**08**T13:00:00Z <br>(2017-09 -**08** 13: 00'te) | Her 2 gün | {none} |
|||||

Yineleme tetikleyicisi için mantıksal altyapısı hesaplar çalışma zamanları başlangıç saatini temel alan uygulamalar, çalışma zamanları atar, sonraki gelecek başlangıç zamanı ilk kez kullandığı çalıştırın ve son çalışma saati gelecekteki alarak çalışan hesaplar.

Bu Yinelenmenin nasıl göründüğünü aşağıda verilmiştir:

| Başlangıç saati | İlk çalıştırma | Gelecekteki çalışma zamanları |
|------------|----------------|------------------|
| 2017-09 -**07** , 2:00 PM | 2017-09 -**09** , 2:00 PM | 2017-09 -**11** , 2:00 PM </br>2017-09 -**13** , 2:00 PM </br>2017-09 -**15** , 2:00 PM </br>ve benzeri... |
||||

Bu nedenle olursa olsun, belirttiğiniz başlangıç zamanı, örneğin, ne kadar geçmiş 2017-09 -**05** 2: 00'dan en veya 2017-09 -**01** 2: 00'te, ilk çalıştırma her zaman sonraki gelecek başlangıç zamanı kullanır.

Kayan pencere tetikleyicisi veya mantıksal altyapısı hesaplar çalışma zamanları başlangıç saatini temel alan uygulamalar için sekme son kez çalıştırın, ilk çalıştırma için başlangıç saatini kullanır ve gelecekteki çalışmalarını başlangıç saatine göre hesaplar.

Bu Yinelenmenin nasıl göründüğünü aşağıda verilmiştir:

| Başlangıç saati | İlk çalıştırma | Gelecekteki çalışma zamanları |
|------------|----------------|------------------|
| 2017-09 -**07** , 2:00 PM | 2017-09 -**07** , 2:00 PM | 2017-09 -**09** , 2:00 PM </br>2017-09 -**11** , 2:00 PM </br>2017-09 -**13** , 2:00 PM </br>2017-09 -**15** , 2:00 PM </br>ve benzeri... |
||||

Bu nedenle olursa olsun, belirttiğiniz başlangıç zamanı, örneğin, ne kadar geçmiş 2017-09 -**05** 2: 00'dan en veya 2017-09 -**01** 2: 00'te, ilk çalıştırma her zaman kullanır belirtilen başlangıç saati.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Örnek tekrarlar

Destek seçenekleri için tetikleyici ayarlayabileceğiniz çeşitli örnek yinelenme şunlardır:

| Tetikleyici | Yinelenme | Interval | Sıklık | Başlangıç saati | Şu günlerde | Şu saatlerde | Şu dakikalarda | Not |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Yinelenme, <br>Kayan Pencere | (Hiçbir başlangıç tarihi ve saati) her 15 dakikada bir Çalıştır | 15 | Dakika | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlamayı hemen başlar ve son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. |
| Yinelenme, <br>Kayan Pencere | (Başlangıç tarihi ve saati ile) 15 dakikada bir Çalıştır | 15 | Dakika | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. |
| Yinelenme, <br>Kayan Pencere | Her saat saat (başlangıç tarihi ve saati ile) çalıştırın | 1 | Saat | *startDate*Thh:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati. Gelecekteki Yinelenme başlangıç zamanından hesaplanan "00" dakika işareti saatte çalıştırın. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. |
| Yinelenme, <br>Kayan Pencere | Her gün (başlangıç tarihi ve saati yok) saatte bir Çalıştır | 1 | Saat | {none} | {kullanılamaz} | {none} | {none} | Bu zamanlama, hemen başlar ve son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. |
| Yinelenme, <br>Kayan Pencere | Her gün (başlangıç tarihi ve saati ile) saatte bir Çalıştır | 1 | Saat | *startDate*T*startTime*Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati sonra son çalıştırma saatini temel alan gelecekteki yinelenme hesaplar. <p>Sıklık, "Week" veya "Month" ise, bu zamanlamanın sırasıyla yalnızca bir gününü hafta veya ayda bir gün çalışır. |
| Yinelenme, <br>Kayan Pencere | 15 dakikada bir saati aşan, her saat (başlangıç tarihi ve saati ile) çalıştırın. | 1 | Saat | *startDate*T00:15:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati. Gelecekteki yinelenme başlangıçtan itibaren hesaplanır "15" dakika işareti çalıştırın, 00: 15'da, 01:15:00, 02:15:00, zaman ve benzeri. |
| Yinelenme | Son saat başı (başlangıç tarihi ve saati yok) 15 dakikada bir Çalıştır | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | 00: 15'da, 01:15:00, 02:15:00, bu zamanlamaya çalışır ve benzeri. Ayrıca, bu zamanlamayı sıklık değeri "Hour" ve "15" dakika içeren bir başlangıç saati eşdeğerdir. |
| Yinelenme | Her 15 dakikada belirtilen dakika işaretlerini (başlangıç tarihi ve saati yok) çalıştırın. | 1 | Gün | {none} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlama, sonraki 15 dakika işareti belirtilene kadar başlamaz. |
| Yinelenme | 8: 00'da günde (başlangıç tarihi ve saati yok) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | {none} | Bu zamanlama, 8: 00'da her gün belirtilen bir zamanlamaya göre çalıştırır. |
| Yinelenme | 8: 00'da günde (başlangıç tarihi ve saati ile) çalıştırma | 1 | Gün | *startDate*T08:00:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlama, belirtilen başlangıç saatini temel alan 8: 00'da her gün çalışır. | 
| Yinelenme | 8: 30'da günde (başlangıç tarihi ve saati yok) çalıştırın | 1 | Gün | {none} | {kullanılamaz} | 8 | 30 | Bu zamanlama, 8: 30'da her gün belirtilen bir zamanlamaya göre çalıştırır. |
| Yinelenme | 8:30 AM her gün (başlangıç tarihi ve saati ile) çalışma | 1 | Gün | *startDate*T08:30:00Z | {kullanılamaz} | {none} | {none} | Bu zamanlama, saat 8: 30'da belirtilen başlangıç tarihinde başlar. |
| Yinelenme | 8:30 AM ve 4:30 PM her gün olarak çalışır | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30 | |
| Yinelenme | 8: 30'da, 8: 45'te Çalıştır 4:30 PM ve 16:45 saatleri her gün | 1 | Gün | {none} | {kullanılamaz} | 8, 16 | 30, 45 | |
| Yinelenme | Her Cumartesi 5 saat (başlangıç tarihi ve saati yok) çalıştırın | 1 | Hafta | {none} | "Saturday" | 17 | 0 | Bu zamanlamayı her Cumartesi 5: 00'da çalışır. |
| Yinelenme | Her Cumartesi saat 17: 00 (başlangıç tarihi ve saati ile) çalıştırın | 1 | Hafta | *startDate*T17:00:00Z | "Saturday" | {none} | {none} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati, bu durumda, 9 Eylül 2017 saat 17:00:00. Her Cumartesi, gelecekteki tekrarlar 5: 00'da çalıştırın. |
| Yinelenme | Her Salı, Perşembe saat 17: 00 çalıştırın | 1 | Hafta | {none} | "Salı", "Perşembe" | 17 | {none} | Bu zamanlamayı her Salı ve Perşembe 5: 00'da çalışır. |
| Yinelenme | Çalışma saatleri sırasında saatte bir Çalıştır | 1 | Hafta | {none} | Cumartesi ve Pazar hariç tüm gün seçin. | Günün istediğiniz saatleri seçin. | Her dakika istediğiniz saati seçin. | Çalışma saatlerinizi 5:00 PM için 8: 00'da, örneğin, ardından "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" günün saati seçin. <p>Çalışma saatlerinizi 8:30:00 için 5:30 PM, gün ve "30" önceki saat saat, dakika seçin. |
| Yinelenme | Hafta sonu günde bir kez çalıştır | 1 | Hafta | {none} | "Saturday", "Sunday" | Günün istediğiniz saatleri seçin. | Saatin herhangi bir dakika uygun olanını seçin. | Bu zamanlamayı her Cumartesi ve Pazar belirtilen zamanlamayla çalıştırır. |
| Yinelenme | İki haftada Pazartesi günleri yalnızca 15 dakikada bir Çalıştır | 2 | Hafta | {none} | "Pazartesi" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Bu zamanlamayı her 15 dakikada işaretinde her Pazartesi çalıştırır. |
| Yinelenme | Her ay çalışacak | 1 | Ay | *startDate*T*startTime*Z | {kullanılamaz} | {kullanılamaz} | {kullanılamaz} | Bu zamanlamayı başlamaz *herhangi erken* belirtilen başlangıç tarihi ve saati ve gelecekteki yinelenme, başlangıç tarihi ve saati hesaplar. Başlangıç tarihi ve saati belirtmezseniz, bu zamanlamanın oluşturulma tarihi ve saati kullanır. |
| Yinelenme | Ayda bir gün için saatte bir Çalıştır | 1 | Ay | {bkz. Not} | {kullanılamaz} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {bkz. Not} | Başlangıç tarihi ve saati belirtmezseniz, bu zamanlamanın oluşturulma tarihi ve saati kullanır. Yinelenme zamanlaması için dakika denetlemek için bir başlangıç zamanı saat, dakika belirtin veya oluşturma zamanı kullanın. Başlangıç zamanı veya oluşturma zamanı Sabah 8:25 ise, bu zamanlamaya 8: 25'da, 25 9: AM, 25 10: AM, örneğin, çalışan ve benzeri. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Yalnızca bir kez çalıştır

Tek seferde gelecekte kullanabileceğiniz yalnızca mantıksal uygulamanızın çalıştırmak istiyorsanız **Zamanlayıcı: Bir kez işlerinizi** şablonu. Yeni bir mantıksal uygulama oluşturduktan sonra ancak Logic Apps Tasarımcısı ' altında açmadan önce **şablonları** bölümü, gelen **kategori** listesinden **zamanlama**ve ardından seçin Bu şablonu:

![Seçin "Zamanlayıcı: İşleri bir kez çalıştır"şablonu](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Veya mantıksal uygulamanız ile başlatabiliyorsanız **olduğunda bir HTTP isteği alındığında - istek** tetikleyin ve tetikleyici için bir parametre olarak başlangıç saatinden geçirin. İlk eylem için kullanmak **Geciktir: zamanlama** eylem ve sonraki eylem çalışmaya başladığında zamanını belirtin.

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturmanıza, zamanlamanıza ve yineleme tetikleyicisi ile yinelenen görevleri ve iş akışları çalıştırma](../connectors/connectors-native-recurrence.md)
* [Oluşturmanıza, zamanlamanıza ve kayan pencere tetikleyicisi ile yinelenen görevleri ve iş akışları çalıştırma](../connectors/connectors-native-sliding-window.md)
* [Gecikmeli eylemleri akışlarıyla Duraklat](../connectors/connectors-native-delay.md)