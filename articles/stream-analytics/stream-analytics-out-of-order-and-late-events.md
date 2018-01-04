---
title: "İşleme olayı sırası ve Azure akış Analizi ile lateness | Microsoft Docs"
description: "Stream Analytics veri akışında düzen dışı ya da geç olaylarla işleyişi hakkında bilgi edinin."
keywords: "bozuk, geç, olayları"
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
<<<<<<< HEAD
ms.openlocfilehash: 208dfa14d5d18e106d654539cd80bafdeb90cdf8
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: HT
=======
ms.openlocfilehash: 71929b449f2a0fa55327fd3f9741208506859e85
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="azure-stream-analytics-event-order-considerations"></a>Azure Stream Analytics olay sipariş konuları

## <a name="arrival-time-and-application-time"></a>Geliş saati ve uygulama zamanı

Olayları bir zamana bağlı veri akışında her olayın zaman damgası atanır. Azure Stream Analytics, geliş saati ya da uygulama zamanı kullanarak, her olay için zaman damgası atar. **System.Timestamp** olaya atanan zaman damgası sütununa sahip. 

Olay kaynağı ulaştığında geliş saati giriş kaynakta atanır. Kullanarak geliş saati erişebilirsiniz **EventEnqueuedTime** olay hub'ı giriş ve kullanmak için özelliği [BlobProperties.LastModified](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) blob giriş özelliği. 

Uygulama zamanı olayı oluşturulur ve yükü parçası olduğunda atanır. Uygulama zamanına göre olayları işlemek için **zaman damgası tarafından** yan tümcesinde select sorgu. Varsa **zaman damgası tarafından** yan tümcesi etmeksizin, olayları geliş saati tarafından işlenir. 

Azure Stream Analytics zaman damgası sırada bir çıktı üretir ve sipariş verilerle ilgili ayarları sağlar.


## <a name="handling-of-multiple-streams"></a>Birden çok akış işleme

Bir Azure akış analizi işi aşağıdaki gibi durumlarda birden çok zaman çizelgelerini olaylarından birleştirir:

* Birden çok bölüm çıkışı üretir. Açık bir yoksa sorgular **bölüm PartitionID tarafından** yan tümcesi tüm bölümler olaylarından birleştirmek gerekir.
* İki veya daha fazla farklı giriş kaynaklarıyla birleşimi.
* Giriş kaynaklarıyla birleştirme.

Birden çok zaman çizelgelerini burada birleştirilir senaryolarda, Azure akış analizi zaman damgası için bir çıktı üretir *t1* yalnızca birleştirilen tüm kaynakları en az zaman sonra *t1*. Örneğin, sorgu bir olay hub bölümünden iki bölümü vardır okur varsayın. Bölümden biri *P1*, olayları zamana kadar olan *t1*. Başka bir bölüme *P2*, olayları zamana kadar olan *t1 + x*. Çıktı sonra zamana kadar üretilen *t1*. Ancak varsa açık bir **bölüm PartitionID tarafından** yan tümcesi, her iki bölüm ilerleme bağımsız olarak.

Geç varış dayanıklılık ayarı bazı bölümleri verilerde yokluğu uğraşmanız kullanılır.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Geç varış dayanıklılık ve düzen dışı tolerans yapılandırma
Sırayla olmayan giriş akışları ya da şunlardır:
* Sıralanmış (ve bu nedenle Gecikmeli)
* Kullanıcı tarafından belirtilen ilkesine göre sistem tarafından ayarlanır

Akış analizi tarafından uygulama zamanı işlerken geç ve sipariş olayları göstereceği. Aşağıdaki ayarlar kullanılabilir **olay sıralama** Azure portalında seçeneği: 

![Stream Analytics olay işleme](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

### <a name="late-arrival-tolerance"></a>Geç varış dayanıklılık
Yalnızca uygulama zamanına göre işlerken geç varış dayanıklılık geçerlidir. Aksi takdirde, ayar yok sayılır.

Geç varış dayanıklılık geliş saati ve uygulama saat arasındaki en fazla farktır. Bir olay daha sonraki geç varış tolerans ulaşan varsa (örneğin, uygulama zamanı *app_t* < geliş saati *arr_t* -geç varış İlkesi dayanıklılık *late_t*), Olay geç varış tolerans üst sınır değerine ayarlanmış (*arr_t* - *late_t*). Geç varış olay oluşturma ile olay giriş kaynağı, alınmasını arasındaki en büyük gecikme penceredir. 

Birden çok bölüm aynı giriş akışından veya birden çok giriş akışları birleştirildiğinde, geç varış dayanıklılık yeni veriler için her bölüm bekleyeceği en fazla süreyi ' dir. 

Geç varış toleransı göre ayarlama önce gerçekleşir. Düzen dışı toleransı göre ayarlama sonraki olur. **System.Timestamp** olaya atanan son zaman damgası sütununa sahip.

### <a name="out-of-order-tolerance"></a>Düzen dışı tolerans
Sıralama dışında ulaşır ancak kümesi düzen dışı tolerans penceresi içinde zaman damgası tarafından kaldırılmasında olaylar. Daha sonraki tolerans penceresi geldiğinde olayları ya da şunlardır:
* **Ayarlanmış**: kabul edilebilir en son zaman gelmedi görünecek şekilde ayarlanmış. 
* **Bırakılan**: atılır.

Stream Analytics içinde sıra dışı tolerans penceresi alınan olayları yeniden sıralar, sorgu çıktısı düzen dışı tolerans penceresi tarafından ertelendi.

### <a name="example"></a>Örnek

* Geç varış dayanıklılık = 10 dakika<br/>
* Düzen dışı tolerans = 3 dakika<br/>
* Uygulama tarafından işleme<br/>
* Etkinlikler:
   1. **Uygulama zamanı** 00:00:00 = **geliş saati** 00:10:01 = **System.Timestamp** 00:00: çünkü ayarlanmış 01, = (**geliş saati - uygulama**) değil birden çok geç varış tolerans.
   2. **Uygulama zamanı** 00:00:01 = **geliş saati** 00:10:01 = **System.Timestamp** 00:00: uygulama zamanı geç varış pencereye olduğundan ayarlanmış değil 01, =.
   3. **Uygulama zamanı** 00:10:00 = **geliş saati** 00:10:02 = **System.Timestamp** 00:10: uygulama zamanı geç varış pencereye olduğundan ayarlanmış değil 00 =.
   4. **Uygulama zamanı** 00:09:00 = **geliş saati** 00:10:03 = **System.Timestamp** 00:09: uygulama zaman aşımı sıra içinde olduğundan özgün zaman damgasıyla kabul 00 = dayanıklılık.
   5. **Uygulama zamanı** 00:06:00 = **geliş saati** 00:10:04 = **System.Timestamp** 00:07: uygulama zamanı düzen dışı tolerans eski olduğu için ayarlanmış 00 =.

### <a name="practical-considerations"></a>Dikkat edilecek noktalar
Daha önce belirtildiği gibi geç varış dayanıklılık uygulama zamanı geliş saati arasındaki en fazla farktır. Uygulama zamanına göre işlerken düzen dışı tolerans ayarını uygulanmadan önce yapılandırılmış geç varış tolerans sonraki olayları ayarlanır. Bu nedenle, bozuk en az geç varış dayanıklılık ve düzen dışı tolerans geçerlidir.

Düzen dışı olayları bir akış içinde nedenleri şunlardır:
* Saat arasında Gönderenler eğme.
* Gönderen ve giriş olay kaynağı arasındaki değişken gecikme süresi.

Geç varış nedenleri şunlardır:
* Toplu işleme ve olaylar için bir aralığı aralığından sonra daha sonra gönderme Gönderenler.
* Olay göndereni için gönderme ve olay giriş kaynağı, alan arasında gecikme süresi.

Geç varış dayanıklılık ve belirli bir iş için düzen dışı tolerans yapılandırırken, doğruluk, gecikme gereksinimlerine ve önceki etkenleri göz önünde bulundurun.

Aşağıda birkaç örnek verilmiştir.

#### <a name="example-1"></a>Örnek 1 
Sorgunun bir **bölüm PartitionID tarafından** yan tümcesi. Tek bir bölüm içinde olayları eşzamanlı gönderme yöntemleri gönderilir. Zaman uyumlu olaylar gönderilen kadar yöntemleri blok gönderin.

Bu durumda, sonraki olay gönderilmeden önce olayları açık onay sırayla gönderildiğinden sıfır bozuk. Geç varış olay oluşturma ve olay yanı sıra, gönderen ve giriş kaynağı arasındaki en büyük gecikme süresi gönderme arasındaki en büyük gecikme olur.

#### <a name="example-2"></a>Örnek 2
Sorgunun bir **bölüm PartitionID tarafından** yan tümcesi. Tek bir bölüm içinde olayları zaman uyumsuz gönderme yöntemleri gönderilir. Zaman uyumsuz gönderme yöntemlerini düzen dışı olayları neden aynı anda birden çok gönderir başlatabilirsiniz.

Bu durumda, bozuk ve geç varış olan en az olay oluşturma ve olay yanı sıra, gönderen ve giriş kaynağı arasındaki en büyük gecikme süresi gönderme arasındaki en büyük gecikme.

#### <a name="example-3"></a>Örnek 3
Sorgu sahip olmayan bir **bölüm PartitionID tarafından** yan tümcesi ve en az iki bölümü vardır.

Yapılandırma örneği 2 ile aynıdır. Ancak, veri bölümleri birinde yokluğu çıktı ek bir geç gerçekleşme tolerans penceresi tarafından gecikmeye yol açabilir.

## <a name="handling-event-producers-with-differing-timelines"></a>Olay üreticileri farklı zaman çizelgeleri ile işleme
Tek bir giriş olay akışı genellikle tek bir cihazı gibi birden çok olay üreticileri kaynaklanan olayları da içerir. Bu olaylar daha önce bahsedilen nedenlerden dolayı bozuk ulaşır. Olay üreticileri arasında düzensiz büyük olabilir, ancak bu senaryolarda, tek bir üretici olayların içinde düzensiz küçük (veya hatta varolmayan).

Azure Stream Analytics düzen dışı olayları ilgilenmek için genel mekanizmaları sağlar. (Straggling olaylar Sistem erişmek için beklenirken), işleme gecikmeleri böyle mekanizmaları neden bırakılmış veya olayları veya her ikisi de ayarlandı.

Henüz birçok senaryolarda, istenen sorgu farklı olay üreticileri olaylarından bağımsız olarak işliyor. Örneğin, bu cihaz başına penceresi başına olay toplama. Bu durumlarda, diğer olay üreticileri Yakala beklenirken bir olay üretici karşılık gelen çıktı gecikme gerek yoktur. Diğer bir deyişle, üreticiler arasında eğme zaman uğraşmanız gerek yoktur. Onu yok sayabilirsiniz.

Elbette, bu çıkış olaylarındaki damgalarını göre sırası anlamına gelir. Aşağı Akış tüketici bu tür davranış ile mücadele etmek mümkün olması gerekir. Ancak her olay çıktıda doğrudur.

Azure Stream Analytics kullanarak bu işlevselliği uygulayan [TIMESTAMP BY OVER](https://msdn.microsoft.com/library/azure/mt573293.aspx) yan tümcesi.

## <a name="summary"></a>Özet
* Geç varış dayanıklılık ve doğruluk ve gecikme süresi gereksinimlerine göre sırası penceresini yapılandırın. Ayrıca, olayların nasıl gönderileceğini düşünün.
* Düzen dışı tolerans geç varış dayanıklılık küçük öneririz.
* Birden çok zaman çizelgelerini birleştirirken veri kaynakları veya bölümleri birinde olmaması çıktı ek bir geç gerçekleşme tolerans penceresi tarafından geciktirebilir.

## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
