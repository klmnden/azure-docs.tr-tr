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
ms.openlocfilehash: 208dfa14d5d18e106d654539cd80bafdeb90cdf8
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Azure Stream Analytics olay sipariş değerlendirme

## <a name="understand-arrival-time-and-application-time"></a>Geliş saati ve uygulama zamanı anlayın.

Olayları bir zamana bağlı veri akışında her olay bir zaman damgası atanır. Azure Stream Analytics geliş saati ya da uygulama zamanı kullanarak her olay zaman damgası atar. "System.Timestamp" sütun olaya atanan zaman damgası vardır. Olay kaynağı ulaştığında geliş saati giriş kaynakta atanır. Geliş saati için olay hub'ı giriş EventEnqueuedTime olduğu ve [blob son değiştirme zamanı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) blob giriş. Uygulama zamanı olayı oluşturulur ve yükü parçası olduğunda atanır. Uygulama zamanına göre olayları işlemek için "Zaman damgası tarafından" yan tümcesi select sorgusundaki kullanın. "Zaman damgası tarafından" yan tümcesi olmazsa olayları geliş saati tarafından işlenir. Geliş saati, olay hub'ı için EventEnqueuedTime özelliğini kullanarak ve blob giriş BlobLastModified özelliği kullanılarak erişilebilir. Azure Stream Analytics zaman damgası sırada bir çıktı üretir ve bozuk verilerle mücadele etmek için birkaç ayarları sağlar.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Birden çok akış Azure Stream Analytics işlenmesi

Azure Stream Analytics işi dahil olmak üzere birkaç durumda birden çok zaman çizelgelerini olaylarından birleştirir,

* Birden çok bölüm çıkışı üretir. Bir açık "bölümü tarafından PartitionID" yoksa sorgular tüm bölümler olaylarından birleştirmek gerekir.
* İki veya daha fazla farklı giriş kaynaklarıyla birleşimi.
* Giriş kaynaklarıyla birleştirme.

Birden çok zaman çizelgelerini burada birleştirilir senaryolarda, bir zaman damgası için çıktı Azure akış analizi üretecektir *t1* yalnızca birleştirilen tüm kaynakları en az zaman sonra *t1*.
Örneğin, sorgu okur bir *olay hub'ı* iki bölüm ve bir bölümün bölümündeki *P1* olayları zamana kadar olan *t1* ve başka bir bölüme  *P2* olayları zamana kadar olan *t1 + x*, çıktı zamana kadar üretilen *t1*.
Ancak olduğunda açık bir *"Bölümü tarafından PartitionID"* yan tümcesi, her iki bölüm ilerler bağımsız olarak.
Geç varış dayanıklılık ayar bazı bölümleri verilerde yokluğu uğraşmanız kullanılır.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Geç varış dayanıklılık ve sıralama dışı tolerans yapılandırma
Sırayla olmayan giriş akışları ya da şunlardır:
* Sıralanmış (ve bu nedenle **Gecikmeli**).
* Sistem tarafından kullanıcı tanımlı ilkesine göre ayarlanır.

Akış analizi tarafından işlerken geç ve bozuk olayları göstereceği **uygulama zamanı**. Aşağıdaki ayarlar kullanılabilir **olay sıralama** Azure portalında seçeneği: 

![Stream Analytics olay işleme](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

**Geç varış dayanıklılık**
* Bu ayar yalnızca uygulama zamanına göre işlerken geçerlidir, aksi halde yoksayılır.
* Geliş saati ve uygulama saat arasındaki maksimum fark budur. Uygulama süresi (saat varış - geç varış penceresi) önce ise (süresi varış - geç varış penceresi) için ayarlanır
* Birden çok bölüm aynı giriş akışından veya birden çok giriş akışları birlikte birlikte kullanıldığında, geç varış dayanıklılık yeni veriler için her bölüm bekleyeceği en fazla süreyi ' dir. 

Kısaca, geç varış pencere olay oluşturma ile olay giriş kaynağı, alınmasına arasındaki en büyük gecikme olur.
Geç varış toleransı göre ayarlama ilk yapılır ve bozuk sonraki yapılır. **System.Timestamp** olaya atanan son zaman damgası sütunu olacaktır.

**Sıralama dışı tolerans**
* Sıranın dışında ancak "sıralama dışı tolerans penceresi" kümesi içinde gelmesini olaylar **zaman damgası tarafından kaldırılmasında**. 
* Dayanıklılık daha sonra gelen olaylar **bırakılan veya ayarlanmış**.
    * **Ayarlanmış**: kabul edilebilir en son zaman gelmedi görünecek şekilde ayarlanmış. 
    * **Bırakılan**: atılır.

"Sıralama dışı tolerans penceresi içinde" alınan olayları yeniden sıralamak için sorgu çıkışıdır **sıralama dışı tolerans penceresi tarafından Gecikmeli**.

**Örnek**

* Geç varış dayanıklılık = 10 dakika<br/>
* Sipariş tolerans dışı = 3 dakika<br/>
* Uygulama tarafından işleme<br/>
* Etkinlikler:
   * Olay 1 _uygulama zamanı_ 00:00:00 = _geliş saati_ 00:10:01 = _System.Timestamp_ 00:00: çünkü ayarlanmış 01, = (_geliş saati_  -  _Uygulama zamanı_) geç varış dayanıklılık büyük.
   * Olay 2 _uygulama zamanı_ 00:00:01 = _geliş saati_ 00:10:01 = _System.Timestamp_ 00:00: uygulama süresi içinde geç varış olduğundan ayarlanmış değil 01, = penceresini açın.
   * %3 olayı _uygulama zamanı_ 00:10:00 = _geliş saati_ 00:10:02 = _System.Timestamp_ 00:10: uygulama zamanı geç varış pencereye olduğundan ayarlanmış değil 00 = .
   * Olay 4 _uygulama zamanı_ 00:09:00 = _geliş saati_ 00:10:03 = _System.Timestamp_ 00:09: uygulama süresi içinde olduğu gibi özgün damgasıyla kabul 00 = Sipariş dayanıklılık.
   * Olay 5 _uygulama zamanı_ 00:06:00 = _geliş saati_ 00:10:04 = _System.Timestamp_ 00:07: uygulama zamanı bozuk eski olduğu için ayarlanmış 00 = dayanıklılık.

## <a name="practical-considerations"></a>Dikkat edilecek noktalar
Yukarıda belirtildiği gibi *geç varış dayanıklılık* en fazla uygulama zamanı ve geliş saati arasındaki farktır.
Ayrıca zaman uygulama tarafından işleme süresi, yapılandırılan sonraki olayları *geç varış dayanıklılık* önce ayarlanmış *sıralama dışı tolerans* ayarı uygulanır. Bu nedenle, bozuk en az geç varış dayanıklılık ve sıralama dışı tolerans geçerlidir.

Bir akış içinde sıra dışı olaylar dahil olmak üzere nedenlerden ötürü gerçekleşir,
* Saat eğriltme Gönderenler arasında.
* Gönderen ve giriş olay kaynağı arasındaki değişken gecikme süresi.

Geç varış dahil olmak üzere nedenlerden ötürü olduğunda,
* Göndericiler toplu ve olaylar için bir aralığı daha sonra aralığından sonra gönderin.
* Olay göndereni için gönderme ve olay giriş kaynağı, alan arasında gecikme süresi.

Yapılandırılırken *geç varış dayanıklılık* ve *sıralama dışı tolerans* Etkenler yukarıda ve belirli bir iş, doğruluk, gecikme gereksinimleri için kabul edilmesi.

Aşağıda birkaç örnek verilmiştir

### <a name="example-1"></a>Örnek 1: 
Sorgu "Bölümü tarafından PartitionID" yan tümcesine sahip ve tek bir bölüm içinde eşzamanlı gönderme yöntemlerini kullanarak olayları gönderilir. Zaman uyumlu olaylar gönderilen kadar yöntemleri blok gönderin.
Bu durumda, sonraki olay göndermeden önce açık onay sırayla olayları gönderildiğinden sıfır bozuk. Olay oluşturma ve gönderme olay + gönderen ve giriş kaynağı arasındaki en büyük gecikme süresi arasında en büyük gecikme geç varış olduğu

### <a name="example-2"></a>Örnek 2:
Sorgu "Bölümü tarafından PartitionID" yan tümcesi varsa ve tek bir bölüm içinde zaman uyumsuz gönderme yöntemini kullanarak olayları gönderilir. Zaman uyumsuz gönderme yöntemlerini bozuk olayları neden aynı anda birden çok gönderir başlatabilirsiniz.
Bu durumda, bozuk ve geç varış, olay oluşturma ve gönderme olay + gönderen ve giriş kaynağı arasındaki en büyük gecikme süresi arasında en az en büyük gecikme şunlardır.

### <a name="example-3"></a>Örnek 3:
Sorgu "Bölümü tarafından PartitionID" sahip değil ve en az iki bölümü vardır.
Örnek 2 aynı yapılandırmadır. Bölümleri birindeki verileri yokluğu çıkış ancak, ek bir tarafından geciktirebilir * geç varış dayanıklılık "penceresi.

## <a name="to-summarize"></a>Özetlemek için
* Geç varış dayanıklılık ve bozuk penceresi doğruluk, gecikme gereksinimlerine göre yapılandırılmalıdır ve olayları nasıl gönderileceğini de dikkate almalısınız.
* Sıralama dışı tolerans geç varış dayanıklılık küçük önerilir.
* Birden çok zaman çizelgelerini birleştirilirken veri kaynakları veya bölümleri birinde olmaması çıktı ek bir geç gerçekleşme tolerans penceresi tarafından geciktirebilir.

## <a name="get-help"></a>Yardım alın
Ek Yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
* [Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Stream Analytics ile çalışmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Stream Analytics sorgu dili başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics Yönetimi REST API Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
