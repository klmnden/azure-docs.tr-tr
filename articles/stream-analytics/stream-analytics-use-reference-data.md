---
title: Azure Stream analytics'te aramaları için başvuru verilerini kullanma
description: Bu makalede, başvuru verileri arama veya bir Azure Stream Analytics iş sorgu tasarım verileri ilişkilendirmek için nasıl kullanılacağını açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: ed50dfd7e3c423c1c26a7dc19ae60dcb319f1850
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621607"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Stream analytics'te aramaları için başvuru verilerini kullanma

Başvuru verileri (arama tablosu olarak da bilinir) statik veya yavaş doğası gereği, değişen bir arama gerçekleştirmek için veya veri akışlarınız genişletmek için kullanılan sınırlı bir veri kümesi var. Örneğin, bir IOT senaryosu içinde (Bu genellikle değişmez) algılayıcıları hakkındaki meta verileri içinde başvuru verilerini depolamak ve gerçek zamanlı IOT veri akışları ile katılın. Azure Stream Analytics, düşük gecikme süreli akış işlemesi için bellek başvuru verileri yükler. Yapmak için Azure Stream Analytics işinizi başvuru verilerinde kullanımı, genel olarak kullanacağınız bir [başvuru veri birleştirme](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) sorgunuzda. 

Stream Analytics Azure Blob Depolama ve Azure SQL veritabanı, başvuru verileri için depolama katmanı olarak destekler. Size ayrıca dönüştürün ve/veya başvuru veri kopyalama Blob depolama alanına kullanmak için Azure Data Factory tarafından [herhangi bir bulut tabanlı sayısı ve şirket içi veri depolarına](../data-factory/copy-activity-overview.md).

## <a name="azure-blob-storage"></a>Azure Blob depolama

Başvuru verileri blob adı, belirtilen tarih/saat, artan bloblar (giriş yapılandırmasında tanımlanmış) öğesinin bir dizisi olarak modellenir. Bunu **yalnızca** dizinin sonuna bir tarih/saat kullanarak eklemeyi destekler **büyük** dizideki son blob tarafından belirtilenden.

### <a name="configure-blob-reference-data"></a>BLOB başvurusu verilerini Yapılandır

Başvuru veri yapılandırmak için önce türünde bir giriş oluşturmak için ihtiyacınız **başvuru verilerini**. Aşağıdaki tabloda, giriş başvuru verileri ile açıklamasını oluşturulurken sağlamak için ihtiyacınız olacak her bir özellik açıklanmaktadır:

|**Özellik adı**  |**Açıklama**  |
|---------|---------|
|Girdi Diğer Adı   | İşin sorgusunda bu giriş başvurmak için kullanılan bir kolay ad.   |
|Depolama Hesabı   | Bloblarınızın bulunduğu depolama hesabının adıdır. Stream Analytics işinizi ile aynı abonelikte etkinleştirilmişse, açılan listeden seçebilirsiniz.   |
|Depolama Hesabı Anahtarı   | Depolama hesabı ile ilişkili gizli anahtar. Stream Analytics işinizi ile aynı abonelikte depolama hesabı seçiliyse otomatik olarak doldurulur.   |
|Depolama kapsayıcısı   | Kapsayıcıları Microsoft Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir.   |
|Yol Deseni   | Belirtilen kapsayıcı içinde bloblarınızın bulunması için kullanılan yol. Yol içinde şu 2 değişkenin bir veya daha fazla örneğini belirtmeyi seçebilirsiniz:<BR>{date} {time}<BR>Örnek 1: products/{date}/{time}/product-list.csv<BR>Örnek 2: products/{date}/product-list.csv<BR>Örnek 3: product-list.csv<BR><br> Blob belirtilen yolda mevcut değilse, Stream Analytics işi süresiz olarak blob kullanılabilir olana kadar bekleyin.   |
|Tarih biçimi [isteğe bağlı]   | {Date}, belirtilen yol deseni içinde kullandıysanız, açılan listeden desteklenen biçimlerden bloblarınızın düzenlenmiş tarih biçimi seçebilirsiniz.<BR>Örnek: YYYY/AA/GG/AA/GG/YYYY, vb.   |
|Saat biçimi [isteğe bağlı]   | Belirttiğiniz yol deseni içinde {time} kullandıysanız, açılan listeden desteklenen biçimlerden bloblarınızın düzenlenmiş saat biçimi seçebilirsiniz.<BR>Örnek: HH, ss/dd veya ss dd.  |
|Olay Serileştirme Biçimi   | Sorgularınızın beklediğiniz, Stream Analytics hangi serileştirme biçimini bilmesi gereken şekilde çalıştığından emin olmak için gelen veri akışları için kullanıyorsunuz. Başvuru verileri için CSV ve JSON desteklenen biçimler şunlardır.  |
|Encoding   | Şu anda desteklenen tek kodlama biçimi UTF-8'dir.  |

### <a name="static-reference-data"></a>Statik başvuru verileri

Ardından, başvuru verilerini değiştirmek için beklenmiyor, statik başvuru verileri giriş yapılandırmasında statik bir yolu belirterek etkin desteği. Belirtilen yol BLOB'dan Azure Stream Analytics seçer. {date} ve {time} değiştirme belirteçleri gerekli değildir. Başvuru verileri Stream Analytics'te sabit olduğundan, statik başvuru veri blob'u üzerine önerilmez.

### <a name="generate-reference-data-on-a-schedule"></a>Başvuru verilerinin bir zamanlamaya göre oluşturun

Başvuru veri yavaş değişen bir veri kümesi ise, {date} kullanan giriş yapılandırmada bir yol deseni belirterek data etkin başvuru yenilemek için'ı destekler ve {substitution belirteçleri zaman}. Stream Analytics bu yol deseni temel alınarak güncelleştirilmiş başvuru veri tanımlarını alır. Örneğin, bir desen `sample/{date}/{time}/products.csv` bir tarih biçimi ile **"YYYY-AA-GG"** ve bir saat biçimini **"Ss dd"** güncelleştirilmiş blob seçmek için Stream Analytics bildirir `sample/2015-04-16/17-30/products.csv` saat 17:30:00 16 Nisan üzerinde , 2015 UTC saat dilimi.

Azure Stream Analytics, yenilenmiş bir başvuru veri BLOB için bir dakika aralıklarla otomatik olarak tarar. Kısa bir gecikme ile (örneğin, 10:30:30) zaman damgası 10:30:00 ile bir blob yüklenirse, bu blob başvuran Stream Analytics işinde kısa bir gecikme fark edeceksiniz. Bu senaryolara önlemek için bu blobu hedef geçerlilik süresinden daha önce karşıya yüklemek için önerilir (10: Bu örnekte 30:00) Stream Analytics işi bulmak ve bellekte yüklemek ve işlemleri gerçekleştirmek için yeterli zaman izin vermek için. 

> [!NOTE]
> Şu anda yalnızca blob adı, kodlanmış zaman makine saatini ilerler, Stream Analytics işleri için blob yenileme arayın. Örneğin, iş arar `sample/2015-04-16/17-30/products.csv` 17:30:00 16 Nisan 2015 UTC daha olası ancak daha önce Hayır bölge Zaman hemen sonra. Götürür *hiçbir zaman* bulunması sonuncu daha önce kodlanmış bir zaman blob'u arayın.
> 
> Örneğin, iş blob bulduğunda `sample/2015-04-16/17-30/products.csv` 5:30 PM 16 Nisan 2015'ten önce kodlanmış bir tarih sahip tüm dosyaları göz ardı eder Bu nedenle geç gelen `sample/2015-04-16/17-25/products.csv` blob oluşturulur aynı kapsayıcıda, iş kullanmaz.
> 
> Benzer şekilde, `sample/2015-04-16/17-30/products.csv` yalnızca 10: 03'te 16 Nisan 2015 oluşturulur ancak daha önceki bir tarihi ile herhangi bir blob kapsayıcısında mevcut olduğundan, işi 10:03 16 Nisan 2015'den başlar bu dosyayı kullanın ve o zamana kadar önceki başvuru verilerini kullanması.
> 
> Bu konuda bir özel iş verileri yeniden işlemek için geçmişe gerektiğinde veya iş ilk olduğunda başlatılır. Başlatma zamanında iş için en son işi başlatmadan önce belirtilen zaman üretilen blob arıyor. Bu olduğundan emin olmak için yapılır bir **boş** işi başladığında veri kümesine başvuru. Biri bulunamazsa iş aşağıdaki tanı gösterilir: `Initializing input without a valid reference data blob for UTC time <start time>`.

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) Stream Analytics tarafından başvuru veri tanımlarını güncelleştirmek için gereken güncelleştirilmiş BLOB'ları oluşturma görevini düzenlemek için kullanılabilir. Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory destekler [bağlanan çok sayıda bulut tabanlı ve şirket içi veri depoları](../data-factory/copy-activity-overview.md) ve taşıma verileri kolayca belirttiğiniz düzenli bir zamanlamaya göre. Daha fazla bilgi ve başvuru verileri için önceden tanımlanmış bir zamanlamaya göre yeniler Stream Analytics'e oluşturmak için bir Data Factory işlem hattı ayarlayın hakkında adım adım yönergeler için bu kontrol [GitHub örnek](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

### <a name="tips-on-refreshing-blob-reference-data"></a>Blob başvurusu verileri yenilemek için ipuçları

1. Sabit olduğundan başvuru veri BLOB üzerine yazmayın.
2. Başvuru verileri yenilemek için önerilen yöntemdir:
    * {Date} kullanan / {yol deseninde time}
    * Giriş iş içinde tanımlanan aynı kapsayıcı ve yol deseni kullanılarak yeni bir blob ekleyin
    * Bir tarih/saat kullanmak **büyük** dizideki son blob tarafından belirtilenden.
3. Başvuru veri BLOB'ları olan **değil** ancak yalnızca blobun "Son değiştirme" zamana göre sıralı blob'u belirtilen tarih ve saat {date} kullanan adlandırın ve {time} değişimler.
3. Liste çok sayıda BLOB yapmamaya için çok eski BLOB'ları için artık işlem yapılmayacak silmeden göz önünde bulundurun. Yeniden başlatma gibi bazı senaryolarda küçük bir miktar yeniden işlemek zorunda ASA geçebilir unutmayın.

## <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL veritabanı başvuru verileri, Stream Analytics işi tarafından alınır ve işleme için bellekte bir anlık görüntü olarak depolanır. Başvuru verilerinizin anlık görüntü de yapılandırma ayarlarında belirttiğiniz bir depolama hesabında bir kapsayıcıda depolanır. Kapsayıcı otomatik-iş başlatıldığında oluşturulur. İş durduruldu veya başarısız durumuna girer, iş yeniden başlatıldığında otomatik olarak oluşturulan kapsayıcıları silinir.  

Yavaş değişen bir veri kümesi başvurusu verilerinizi ise, düzenli aralıklarla işinizde kullanılan anlık görüntü yenilemeniz gerekir. Stream Analytics, Azure SQL veritabanı giriş bağlantısını yapılandırırken, yenileme hızı ayarlamanıza olanak tanır. Stream Analytics çalışma zamanı, Azure SQL veritabanı yenileme hızı tarafından belirtilen aralıkta sorgular. Desteklenen hızlı yenileme hızı anda dakika başına ' dir. Her yenileme için Stream Analytics depolama hesabında yeni bir anlık görüntü depolar.

Stream Analytics, Azure SQL veritabanı'nı sorgulamak için iki seçenek sunar. Anlık görüntü sorgu zorunludur ve her bir iş eklenmelidir. Stream Analytics, düzenli aralıklarla, yenileme aralığına dayalı bir anlık görüntü sorgu çalıştırır ve (snapshot) sorgusunun sonucunu başvuru veri kümesi kullanır. Anlık görüntü sorgu Çoğu senaryoda sığması, ancak büyük veri kümeleri ve hızlı yenileme hızı ile ilgili performans sorunları yaşarsanız delta sorgu seçeneğini kullanabilirsiniz. Başvuru veri kümesi döndürmek için birden fazla 60 saniye Süren sorgular zaman aşımı neden olur.

Delta sorgu seçeneği ile Stream Analytics başlangıçta temel başvuru veri kümesi almak için anlık görüntü sorgusu çalıştırır. Sonra Stream Analytics, düzenli aralıklarla artımlı değişiklikler almak için yenileme aralığına dayalı delta sorgu çalıştırır. Bu artımlı değişiklikler sürekli olarak güncel tutmak için başvuru veri kümesi olarak uygulanır. Delta sorgu kullanarak, depolama maliyetini azaltmak ve ağ g/ç işlemleri yardımcı olabilir.

### <a name="configure-sql-database-reference"></a>SQL veritabanı başvurusu yapılandırma

SQL veritabanı başvurusu verilerinizi yapılandırmak için öncelikle oluşturmak için ihtiyacınız **başvuru verilerini** giriş. Aşağıdaki tabloda, giriş başvuru verileri ile açıklamasını oluşturulurken sağlamak için ihtiyacınız olacak her bir özellik açıklanmaktadır. Daha fazla bilgi için [kullanımı başvuru verilerini bir SQL veritabanı için Azure Stream Analytics işi](sql-reference-data.md).

|**Özellik adı**|**Açıklama**  |
|---------|---------|
|Girdi diğer adı|İşin sorgusunda bu giriş başvurmak için kullanılan bir kolay ad.|
|Subscription|Aboneliğinizi seçin|
|Database|Başvuru verileri Azure SQL veritabanı.|
|Kullanıcı Adı|Azure SQL veritabanı ile ilişkili kullanıcı adı.|
|istemcisiyle yönetilen bir cihaz için)|Azure SQL veritabanı ile ilişkili parola.|
|Düzenli aralıklarla yenileyin|Bu seçenek, bir yenileme hızını seçmenize olanak tanır. "On" seçme içinde DD:HH:MM yenileme hızı belirtmenize olanak sağlayacak.|
|Anlık görüntü sorgu|SQL veritabanı'ndan başvuru verilerini alır. varsayılan sorgu seçenek budur.|
|Delta sorgu|Büyük veri kümeleri ve kısa ile Gelişmiş senaryolar için yenileme hızı, bir delta sorgu eklemek seçin.|

## <a name="size-limitation"></a>Boyut sınırlaması

Stream Analytics, başvuru verileri ile destekler **boyut üst sınırı 300 MB'dir**. Başvuru verileri en büyük boyutu 300 MB sınırını yalnızca basit bir sorgu ile ulaşılabilir. Pencereli toplamlar, zamana bağlı birleşimler ve geçici analiz işlevleri gibi durum bilgisi olan işleme eklemek için sorgu karmaşıklığı arttıkça başvuru veri azaldıkça boyutu desteklenen en yüksek bekleniyor. Azure Stream Analytics başvuru verileri yüklemek ve karmaşık işlemleri, iş belleğiniz bitebilir ve başarısız. Böyle durumlarda, % 100 SU % Utilization ölçümünü ulaşacak.    

|**Akış birimi sayısı**  |**Desteklenen yaklaşık en fazla boyutu (MB)**  |
|---------|---------|
|1\.   |50   |
|3   |150   |
|6 ve sonraki süreci desteleyen   |300   |

Bir işin 6 ötesinde akış birimi sayısını artırabilir, başvuru verileri, desteklenen en büyük boyutunu artırmaz.

Sıkıştırma desteğine başvuru verileri için kullanılabilir değil. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
