---
title: Stream Analytics başvuru verileri ve arama tabloları kullanın | Microsoft Docs
description: Stream Analytics sorgu başvuru verileri kullanın
keywords: Arama tablosu, başvuru verileri
services: stream-analytics
documentationcenter: ''
author: jseb225
manager: ryanw
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: 77a4a9a28060206a30c658216156d7339bddc398
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Başvuru verileri veya arama tabloları bir akış analizi Giriş akışı kullanma
Başvuru verileri (arama tablosu olarak da bilinir) statik sınırlı bir veri kümesi veya yavaşlamasının doğası gereği değiştirme bir arama gerçekleştirmek ya da veri akışı ile ilişkilendirmek için kullanılır. Yapmak için Azure Stream Analytics işiniz başvuru verilerinde kullanımı, genellikle kullanacağınız bir [başvuru veri birleştirme](https://msdn.microsoft.com/library/azure/dn949258.aspx) Sorgunuzdaki. Akış analizi başvuru verileri için depolama katmanı olarak Azure Blob Depolama kullanır ve Azure Data Factory başvurusuyla veri dönüştürülen ve/veya başvuru veriler olarak kullanmak için Azure Blob Depolama birimine kopyalanan [bulut tabanlı herhangi bir sayıda ve Şirket içi veri depolarına](../data-factory/copy-activity-overview.md). Başvuru verileri blob adı alanında belirtilen tarih/saat artan (giriş yapılandırmada tanımlanan) BLOB'lar dizisi olarak modellenir. Bu **yalnızca** dizisi sonuna bir tarih/saat kullanarak eklenmesini destekleyen **büyük** sıradaki son blob tarafından belirtilenden.

Akış analizi sahip bir **blob 100 MB sınırı** ancak işleri, birden çok başvuru BLOB'ları kullanarak işleyebilir **yol deseni** özelliği.


## <a name="configuring-reference-data"></a>Başvuru verileri yapılandırma
Başvuru verileri yapılandırmak için önce türünde bir girişi oluşturmak ihtiyacınız **başvuru verileri**. Aşağıdaki tabloda açıklamasını ile giriş başvuru verileri oluşturulurken sağlamanız gerekir her bir özellik açıklanmaktadır:


<table>
<tbody>
<tr>
<td>Özellik Adı</td>
<td>Açıklama</td>
</tr>
<tr>
<td>Giriş Diğer Adı</td>
<td>İş sorguda bu girişi başvurmak için kullanılan kolay adı.</td>
</tr>
<tr>
<td>Depolama Hesabı</td>
<td>Bloblarınızın bulunduğu depolama hesabının adı. Akış analizi işi ile aynı abonelikte olması durumunda, açılan listeden seçebilirsiniz.</td>
</tr>
<tr>
<td>Depolama Hesabı Anahtarı</td>
<td>Depolama hesabıyla ilişkili gizli anahtar. Stream Analytics işiniz ile aynı abonelikte depolama hesabı ise, bu otomatik olarak doldurulur.</td>
</tr>
<tr>
<td>Depolama kapsayıcısı</td>
<td>Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir.</td>
</tr>
<tr>
<td>Yol Deseni</td>
<td>Belirtilen kapsayıcı içinde bloblarınızın bulunması için kullanılan yolu. Yol içinde şu 2 değişkenin bir veya daha fazla örneğini belirtmeyi seçebilirsiniz:<BR>{date} {time}<BR>Örnek 1: products/{date}/{time}/product-list.csv<BR>Örnek 2: products/{date}/product-list.csv
</tr>
<tr>
<td>[İsteğe bağlı] tarih biçimi</td>
<td>Belirttiğiniz yol deseni içinde {date} kullandıysanız, açılan listeden desteklenen biçimlerden birinde bloblarınızın organize edilmiştir tarih biçimi seçebilirsiniz.<BR>Örnek: YYYY/AA/GG, GG/AA/YYYY, vs.</td>
</tr>
<tr>
<td>[İsteğe bağlı] saat biçimi</td>
<td>Belirttiğiniz yol deseni içinde {time} kullandıysanız, açılan listeden desteklenen biçimlerden birinde bloblarınızın düzenlenmiş zaman biçimini seçebilirsiniz.<BR>Örnek: Ss, ss/dd veya HH mm</td>
</tr>
<tr>
<td>Olay Serileştirme Biçimi</td>
<td>Sorgularınızın beklediğiniz şekilde çalışıp çalışmadığından emin olmak için, Akış Analizi'nin gelen veri akışları için hangi serileştirme biçimini kullandığınızı bilmesi gerekir. Başvuru verileri için CSV ve JSON desteklenen biçimler:.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Şu anda desteklenen tek kodlama biçimi UTF-8 durumda</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Başvuru verileri bir zamanlama oluşturma
Başvuru verileri yavaş değişen bir veri kümesi ise, başvuru verileri {date} kullanarak giriş yapılandırmasında bir yol deseni belirterek etkin yenileme için destek ve {değiştirme belirteçleri time}. Bu yol deseni temel alınarak güncelleştirilen başvuru veri tanımlarını Yukarı Akış analizi seçer. Örneğin, bir düzeni `sample/{date}/{time}/products.csv` bir tarih biçimi ile **"YYYY-AA-GG"** ve bir saat biçimini **"Ss-dd"** güncelleştirilmiş blob seçmek için Stream Analytics bildirir `sample/2015-04-16/17-30/products.csv` 17:30:00 saatleri Nisan 16 üzerinde adresindeki , 2015 UTC saat dilimi.

> [!NOTE]
> Şu anda yalnızca blob adı kodlanmış zamana makine süresini ilerler, akış analizi işleri için blob yenileme arayın. Örneğin, iş şuna `sample/2015-04-16/17-30/products.csv` 17:30:00 saatleri 16 Nisan 2015 UTC üzerinde daha olası ancak daha önce hiçbir bölge Zaman hemen sonra. İçinde *hiçbir zaman* bulunduğundan sonuncu daha önce kodlanmış bir zaman bir blob arayın.
> 
> Örneğin İş blob bulduğunda `sample/2015-04-16/17-30/products.csv` 17:30:00 saatleri 16 Nisan 2015'ten önce kodlanmış bir tarih ile herhangi bir dosya yoksayacak bunu geç ulaşan varsa `sample/2015-04-16/17-25/products.csv` blob oluşturulan aynı kapsayıcıda bu iş kullanmaz.
> 
> Benzer şekilde, `sample/2015-04-16/17-30/products.csv` yalnızca 10:03 PM 16 Nisan 2015 oluşturulur ancak daha önceki bir tarihi ile hiçbir blob kapsayıcısında mevcut olduğundan, işi 10:03 PM 16 Nisan 2015 başlayarak bu dosyayı kullanmak ve o zamana kadar önceki başvuru verileri kullanın.
> 
> Bu konuda bir özel iş geçmişe yeniden işlemek verilere gerektiğinde veya iş ilk olduğunda başlatılır. Başlangıç zamanında iş üretilen işi başlatmadan önce belirtilen süre için en son blob arıyor. Bu olduğundan emin olmak için gerçekleştirilir bir **boş** iş başlatıldığında, veri kümesi başvurusu. Bir bulunamazsa işi aşağıdaki tanı görüntüler: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) akış analizi tarafından başvuru veri tanımlarını güncelleştirmek için gereken güncelleştirilmiş BLOB'ları oluşturma görevini düzenlemek için kullanılabilir. Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Veri Fabrikası destekleyen [çok sayıda bulut bağlanma tabanlı ve şirket içi veri depolarına](../data-factory/copy-activity-overview.md) ve taşıma verileri kolayca belirttiğiniz düzenli bir zamanlamaya göre. Daha fazla bilgi ve önceden tanımlanmış bir zamanlamayla yenilenir akış analizi için başvuru verileri oluşturmak için Data Factory işlem hattı ayarlama konusunda adım adım yönergeler için bu denetleyin [GitHub örnek](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Başvuru verileriniz yenilenirken ipuçları
1. Başvuru verileri BLOB'ları üzerine blob yeniden yüklemek Stream Analytics neden olmaz ve bazı durumlarda iş başarısız olmasına neden olabilir. Başvuru verileri değiştirmek için önerilen yöntem iş girişinde tanımlanan aynı kapsayıcı ve yol desenini kullanarak yeni bir blob ekleyin ve tarih/saat kullanmaktır **büyük** sıradaki son blob tarafından belirtilenden.
2. Başvuru verileri BLOB'ları olan **değil** blob'un "Son değiştirme" zamana göre ancak yalnızca göre sıralanmış blob içinde belirtilen tarih ve saat {date} kullanarak ad ve {time} değişimler.
3. Liste çok sayıda BLOB için zorunda kalmamak için çok eski BLOB'ları için artık işleme yapılır silme göz önünde bulundurun. ASA kısa süreli bir yeniden başlatma gibi bazı senaryolarda yeniden işlemek sahip geçebilir unutmayın.

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Nesnelerin İnterneti'nden gelen verilerdeki akış analizlerine yönelik bir yönetilen hizmet olan Stream Analytics'e giriş yaptınız. Bu hizmet hakkında daha fazla bilgi edinmek için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
