---
title: Azure Stream Analytics çıkışları anlama
description: Bu makalede, Azure Stream Analytics, Power BI gibi analiz sonuçları için kullanılabilir veri çıkış seçenekleri açıklanır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/25/2019
ms.custom: seodec18
ms.openlocfilehash: 3fab76613bb992b29ceeef12cf5f410c5c3b208d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205525"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışları anlama
Bu makalede, Azure Stream Analytics işi için çıktıların türlerini açıklar. Çıkış, depolamak ve Stream Analytics işi sonuçlarını kaydetmek olanak tanır. Yapabileceğiniz çıktı verilerini kullanarak, İş analizi ve veri depolama verilerinizi daha fazla.

Stream Analytics sorgunuz tasarlarken kullanarak çıktının adına başvurmak [yan tümcesi içinde](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). İş başına tek bir çıkış veya (gerekiyorsa), birden çok INTO yan tümceleri sorguda sağlayarak iş akışı başına birden çok çıkış kullanabilirsiniz.

Test Stream Analytics işi oluşturmak ve düzenlemek için çıkışları kullanabileceğiniz [Azure portalında](stream-analytics-quick-create-portal.md#configure-job-output), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output), ve [Visual Studio](stream-analytics-quick-create-vs.md).

Bazı çıkış türleri desteği [bölümleme](#partitioning). [Çıktı toplu iş boyutu](#output-batch-size) aktarım hızını iyileştirmek için farklılık gösterir.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics destekler [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store, büyük veri analizi iş yükleri için bir kuruluş çapında hiper ölçekli depodur. Data Lake Store, herhangi bir boyut, türü ve işletimsel ve keşfe dönük çözümleme için alma hızı verileri depolamak için kullanabilirsiniz. Stream Analytics, Data Lake Store erişim iznine sahip olması gerekir.

Stream analytics'ten Azure Data Lake Store çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

### <a name="authorize-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı yetki

1. Azure Portalı'ndaki bir çıkış olarak Data Lake Store seçtiğinizde, var olan Data Lake Store örneğine bir bağlantı yetkilendirme istenir.

   ![Data Lake Store için bir bağlantı yetkilendirme](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)

2. Data Lake Store için zaten erişimi varsa seçin **şimdi Yetkilendir**. Bir sayfa açılır ve gösterir **yetkilendirme için yeniden yönlendirme**. Yetkilendirme başarılı olduktan sonra Data Lake Store çıkış yapılandırmanıza olanak tanıyan sayfası gösterilir.

3. Kimliği doğrulanmış Data Lake Store hesabı oluşturduktan sonra Data Lake Store çıkışınızı özelliklerini yapılandırabilirsiniz.

   ![Stream Analytics çıktı olarak Data Lake Store tanımlayın](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)

Özellik adları ve açıklamaları, Data Lake Store çıkış yapılandırmak için aşağıdaki tabloda listelenmektedir.   

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgular, Data Lake Store sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Hesap adı | Çıkış burada gönderiyorsanız Data Lake Store hesabının adıdır. Aşağı açılan listesini aboneliğinizde mevcut bir Data Lake Store hesapları ile sunulur. |
| Yol ön eki deseni | Belirtilen Data Lake Store hesabındaki dosyaları yazmak için kullanılan dosya yolu. {Değişkenleri time} ve {date} bir veya daha fazla örneğini belirtin:<br /><ul><li>Örnek 1: klasör1/günlükler / {tarih} / {time}</li><li>Örnek 2: klasör1/günlükler / {tarih}</li></ul><br />Oluşturulan klasör yapısını zaman damgasını UTC ve yerel saat izler.<br /><br />Dosya yolu deseni sonunda eğik çizgi (/) içermiyorsa, son deseni dosya yolunda dosya adı ön eki olarak kabul edilir. <br /><br />Bu durumlarda, yeni dosyalar oluşturulur:<ul><li>Çıkış şemayı değiştirme</li><li>Harici veya dahili bir işi yeniden başlatın.</li></ul> |
| Tarih biçimi | İsteğe bağlı. Ön ek yolu tarih belirteci kullandıysanız, dosyalarınızı düzenlenmiş tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
|Saat biçimi | İsteğe bağlı. Ön ek yolu zaman belirteç kullandıysanız, dosyalarınızı düzenlenmiş saat biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıktı verilerini seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.|
| Encoding | CSV veya JSON biçimi kullanıyorsanız, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.|
| Sınırlayıcı | Yalnızca CSV serileştirme için geçerlidir. Stream Analytics, CSV verileri seri hale getirme için birkaç ortak sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk.|
| Biçimlendir | Yalnızca JSON serileştirmesi için geçerlidir. **Ayrılmış çizgi** çıktı yeni satırla ayrılmış her bir JSON nesnesi sağlanarak biçimlendirileceğini belirtir. **Dizi** çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, çıktı dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez çünkü satır ayrılmış JSON, kullanılacak tercih edilir.|

### <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi Yenile
İşinizi oluşturulduğu veya en son kimlik doğrulaması parolasını değişmişse Data Lake Store hesabınızı yeniden kimlik doğrulamaya zorlayabilir gerekir. Sağlamalarını yoksa, iş çıktısı sonuçlar üretmez ve yapılan işlem günlüklerinde gereksinimini belirten bir hata gösterir. 

Şu anda, kimlik doğrulama belirteci 90 günde bir Data Lake Store çıkış olan tüm işler için el ile yenilenmesi gerekiyor. Bu sınırlama tarafından üstesinden gelebilir [yönetilen kimlikleri (Önizleme) ile kimlik doğrulaması](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-managed-identities-adls).

Yetkilendirmeyi yenilemek için:

1. Seçin **Durdur** işinizi durdurmak için.
1. Gidin, Data Lake Store için çıktı ve seçin **yetkilendirmeyi yenilemek** bağlantı.

   Kısa bir süre için bir açılır sayfa gösteren **yetkilendirme için yeniden yönlendirme**. Yetkilendirme başarılı olursa sayfasını gösteren **yetkilendirme başarıyla yenilendi** ve otomatik olarak kapanır. 
   
1. Seçin **Kaydet** sayfanın alt kısmındaki. Ardından, işten yeniden başlatabilirsiniz **son durduruldu zamanı** veri kaybını önlemek için.

![Çıkış, Data Lake Store yetkilendirmeyi Yenile](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)

## <a name="sql-database"></a>SQL Veritabanı
Kullanabileceğiniz [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) kendiliğinden ilişkisel veriler veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar çıktı olarak. SQL veritabanı'nda var olan bir tablo için Stream Analytics işlerini yazma. Tablo şemasını, alanları ve bunların türlerini işinizin çıktısında tam olarak eşleşmelidir. Ayrıca belirtebileceğiniz [Azure SQL veri ambarı](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) aracılığıyla SQL veritabanı çıktı olarak çıkış seçeneği. Yazma aktarım hızını iyileştirmek için yollar hakkında bilgi edinmek için [çıktı olarak Azure SQL veritabanı ile Stream Analytics](stream-analytics-sql-output-perf.md) makalesi. 

Aşağıdaki tabloda özellik adları ve SQL veritabanı çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu veritabanı sorgusu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Database | Çıkış burada gönderiyorsanız veritabanının adı. |
| Sunucu adı | SQL veritabanı sunucu adı. |
| Kullanıcı adı | Veritabanına yazma erişimi olan kullanıcı adı. Stream Analytics, yalnızca SQL kimlik doğrulamasını destekler. |
| Parola | Veritabanına bağlanmak için parola. |
| Tablo | Çıkış yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır. Bu tablonun şeması, alanları ve bunların türlerini, iş çıktısı üreten sayısı tam olarak eşleşmelidir. |
|Bölüm düzeni devral| Tablo için birden çok yazarları içeren tam olarak paralel topolojisi etkinleştirmek için önceki bir sorgu adımına bölümleme düzeni devralma seçeneği. Daha fazla bilgi için [Azure SQL veritabanı için Azure Stream Analytics çıkış](stream-analytics-sql-output-perf.md).|
|Eşleşme toplu iş sayısı| Önerilen sınır gönderilen her toplu ile kayıtlarının sayısı üzerinde işlem ekleyin.|

> [!NOTE]
> Şu anda Azure SQL veritabanı teklifi, Stream Analytics işi çıktısında için desteklenir. Eklenen bir veritabanı ile SQL Server çalıştıran bir Azure sanal makine desteklenmiyor. Gelecekteki sürümlerde değişebilir budur.
>

## <a name="blob-storage"></a>Blob depolama
Azure Blob Depolama, bulutta büyük miktarda yapılandırılmamış veriyi depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob Depolama ve bunun kullanımını giriş için bkz [BLOB'ları kullanmayı](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Özellik adları ve açıklamalarının bir blob çıktı oluşturmak için aşağıdaki tabloda listelenmektedir.

| Özellik adı       | Açıklama                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Çıktı diğer adı        | Sorgular, bu blob depolama sorgusu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Depolama hesabı     | Çıkış burada gönderiyorsanız depolama hesabının adıdır.               |
| Depolama hesabı anahtarı | Depolama hesabı ile ilişkili gizli anahtar.                              |
| Depolama kapsayıcısı   | Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırması. Blob hizmeti için bir blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir. |
| Yol deseni | İsteğe bağlı. Belirtilen kapsayıcı içinde bloblarınızın yazmak için kullanılan dosya yolu deseni. <br /><br /> Yol deseninde blobları yazılır sıklığı belirtecek şekilde tarih ve saat değişkenin bir veya daha fazla örneğini kullanmayı seçebilirsiniz: <br /> {date} {time} <br /><br />Bir özel {alanı} adı bölümü blob'lara olay verilerinizden belirtmek için özel blob bölümleme kullanabilirsiniz. Alan adı, alfasayısal ve boşluk, kısa çizgi ve alt çizgi içerebilir. Özel alanları kısıtlamaları şunlardır: <ul><li>Alan adlarını büyük küçük harfe duyarlı değildir. Örneğin, hizmet sütun "ID" sütun "id." ayırt edilemiyor</li><li>İç içe alanlar izin verilmez. Bunun yerine, bir diğer ad "alanı düzleştirmek için" iş sorguda kullanın.</li><li>İfadeler, alan adı olarak kullanılamaz.</li></ul> <br />Bu özellik, özel bir tarih/saat biçimi Belirleyicisi yapılandırmaları yolunda kullanımını etkinleştirir. Özel tarih ve saat biçimleri, her seferinde belirtilen biri tarafından alınmış olmalıdır {datetime:\<belirticisi >} anahtar sözcüğü. Verilen girişler için \<belirticisi > yyyy, MM, M, dd, d, ss, H, mm, m, ss veya s. {Datetime:\<belirticisi >} anahtar sözcüğü birden çok kez yolunda özel bir tarih/saat yapılandırmaları oluşturmak için kullanılabilir. <br /><br />Örnekler: <ul><li>Örnek 1: küme1/günlükler / {tarih} / {time}</li><li>Örnek 2: küme1/günlükler / {tarih}</li><li>Örnek 3: cluster1 / {client_id} / {tarih} / {time}</li><li>Örnek 4: cluster1 / {datetime:ss} / {myField} sorgu olduğu: SELECT data.myField AS alanım gelen giriş;</li><li>Örnek 5: küme1/yıl {datetime:yyyy} = / ay = {datetime:MM} / gün {datetime:dd} =</ul><br />Oluşturulan klasör yapısını zaman damgasını UTC ve yerel saat izler.<br /><br />Dosya adlandırma aşağıdaki kuralını kullanır: <br /><br />{Yol ön eki Pattern}/schemaHashcode_Guid_Number.extension<br /><br />Örnek çıktı dosyaları:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />Bu özellik hakkında daha fazla bilgi için bkz. [Azure Stream Analytics özel blob çıkış bölümleme](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Tarih biçimi | İsteğe bağlı. Ön ek yolu tarih belirteci kullandıysanız, dosyalarınızı düzenlenmiş tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
| Saat biçimi | İsteğe bağlı. Ön ek yolu zaman belirteç kullandıysanız, dosyalarınızı düzenlenmiş saat biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding    | CSV veya JSON biçimi kullanıyorsanız, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir. |
| Sınırlayıcı   | Yalnızca CSV serileştirme için geçerlidir. Stream Analytics, CSV verileri seri hale getirme için birkaç ortak sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir      | Yalnızca JSON serileştirmesi için geçerlidir. **Ayrılmış çizgi** çıktı yeni satırla ayrılmış her bir JSON nesnesi sağlanarak biçimlendirileceğini belirtir. **Dizi** çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, çıktı dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez çünkü satır ayrılmış JSON, kullanılacak tercih edilir. |

Çıktı olarak Blob Depolama kullanırken, aşağıdaki durumlarda BLOB yeni bir dosya oluşturulur:

* Dosya sayısı izin verilen blok (şu anda 50.000) aşarsa. İzin verilen en fazla blokların sayısı izin verilen en yüksek blob boyutu erişmeden ulaşın. Örneğin, çıkış oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu büyüktür. Çıkış oranı düşükse, her blok daha az veri varsa ve dosya boyutu küçüktür.
* Çıktı, çıkış biçimi bir şema değişikliği ise sabit şemasına (CSV ve Avro) gerektirir.
* Bir iş, harici olarak, durdurma ve başlatma, kullanıcı tarafından veya dahili sistem bakım ya da hata kurtarma için yeniden başlatılması durumunda.
* Her bölüm çıktı için sorgu tam olarak bölümlendiğinde ise ve bir yeni dosya oluşturulur.
* Kullanıcı bir dosyayı veya depolama hesabı kapsayıcısı silerse.
* Varsa çıktı yol ön eki deseni kullanarak bölümlenmiş zaman ve yeni bir blob sorgu bir sonraki saate hareket ettiğinde kullanılır.
* Çıkış, özel bir alan bölümlenmiş ve yeni bir blob bölüm anahtarı if oluşturulursa yok.
* Çıktı tarafından bölümlendiğinde ise burada bölüm anahtarı kardinalite özel bir alan 8000 aşıyor ve bölüm anahtarı yeni bir blob oluşturulur.

## <a name="event-hubs"></a>Event Hubs
[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) hizmetidir yüksek düzeyde ölçeklenebilir Yayımla-abone ol olay yutucu. Bu, saniye başına milyonlarca olayı toplayabilirsiniz. Stream Analytics iş çıktısını başka bir iş akışında girişi olduğunda bir olay hub'ının çıktı olarak kullanılır.

Çıkış olarak event hubs'tan veri akışlarını yapılandırmanız için birkaç parametre ihtiyacınız vardır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgu çıkışının bu olay hub'ına doğrudan sorgularında kullanılan bir kolay ad. |
| Olay hub’ı ad alanı |Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcı. Yeni bir olay hub'ı oluşturduğunuzda, bir olay hub'ı ad alanı da oluşturmuş. |
| Olay hub'ı adı | Olay hub'ı çıkış adı. |
| Olay hub'ı ilke adı | Olay hub'ın oluşturabilirsiniz. paylaşılan erişim ilkesi **yapılandırma** sekmesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| Olay hub'ı ilke anahtarı | Olay hub'ı ad kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. |
| Bölüm anahtarı sütunu | İsteğe bağlı. Olay hub'ı çıkışı için bölüm anahtarını içeren bir sütun. |
| Olay serileştirme biçimi | Çıktı verilerini seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding | CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| Sınırlayıcı | Yalnızca CSV serileştirme için geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir | Yalnızca JSON serileştirmesi için geçerlidir. **Ayrılmış çizgi** çıktı yeni satırla ayrılmış her bir JSON nesnesi sağlanarak biçimlendirileceğini belirtir. **Dizi** çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, çıktı dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez çünkü satır ayrılmış JSON, kullanılacak tercih edilir. |
| Özellik sütunları | İsteğe bağlı. Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken virgülle ayrılmış sütun. Bu özellik hakkında daha fazla bilgi bölümündedir [çıkışı için özel meta veri özelliklerini](#custom-metadata-properties-for-output). |

## <a name="power-bi"></a>Power BI
Kullanabileceğiniz [Power BI](https://powerbi.microsoft.com/) için analiz sonuçları için bir zengin görselleştirme deneyiminin sunulabilmesi için bir Stream Analytics işi çıktı olarak. İşletimsel panolar, rapor oluşturma ve ölçüm temelli raporlama için bu özelliği kullanabilirsiniz.

Stream Analytics'ten alınan Power BI çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

### <a name="authorize-a-power-bi-account"></a>Power BI hesabı yetki
1. Power BI, Azure Portalı'ndaki bir çıkış olarak seçildiğinde, varolan bir Power BI kullanıcı yetkilendirmek için veya yeni bir Power BI hesabı oluşturmanız istenir.
   
   ![Çıkış yapılandırmak için bir Power BI kullanıcı yetkilendirmek](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)

2. Yoksa henüz yoksa, ve ardından yeni bir hesap oluşturun **şimdi Yetkilendir**. Aşağıdaki sayfa açılır:
   
   ![Azure hesabınızdan Power BI'da kimlik doğrulaması](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)

3. Power BI çıkışına yetkisi vermek için iş veya Okul hesabı sağlayın. Henüz Power BI için oturumunuz, seçin **şimdi kaydolun**. Power BI için kullandığınız iş veya Okul hesabı ile artık oturum açmadıysanız Azure aboneliği hesabından farklı olabilir.

### <a name="configure-the-power-bi-output-properties"></a>Power BI çıktı özelliklerini yapılandırma
Power BI hesabı kimlik doğrulaması yaptıktan sonra Power BI çıkışınızı özelliklerini yapılandırabilirsiniz. Özellik adları ve açıklamaları, Power BI çıkışına yapılandırmak için aşağıdaki tabloda listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu Power BI çıkışına sorgu çıkışı yönlendirmek için kullanılan kolay bir ad girin. |
| Grup çalışma alanı |Diğer Power BI kullanıcıları ile veri paylaşımını etkinleştirmek için Power BI hesabınızda içinde grupları seçebilir veya seçin **çalışma Alanım** grubuna yazma istemiyorsanız. Mevcut bir grubu güncelleştiriliyor, Power BI kimlik doğrulaması yenileme gerektirir. |
| Veri kümesi adı |Power BI çıkışına kullanmak istediğiniz bir veri kümesi adı belirtin. |
| Tablo adı |Power BI çıkış veri kümesi altında bir tablo adı sağlayın. Şu anda Power BI çıkışına Stream Analytics işlerine bir veri kümesinde yalnızca bir tabloya sahip olabilir. |

Power BI çıkışına ve Pano yapılandırma yönergeleri için bkz [Azure Stream Analytics ve Power BI](stream-analytics-power-bi-dashboard.md) makalesi.

> [!NOTE]
> Açıkça tablo ve veri kümesini Power BI panosunda oluşturmayın. Tablo ve veri kümesi otomatik olarak iş başlatıldığında ve iş parçacıklarının çıkış Power BI'a başlar doldurulur. İş sorgusu hiçbir sonuç oluşturmaz, tablo ve veri kümesi oluşturulmaz. Power BI veri kümesi ve bu Stream Analytics işinde sağlanan adla aynı ada sahip bir tablo zaten varsa, mevcut verilerin üzerine yazılır.
>

### <a name="create-a-schema"></a>Bir şema oluşturun
Azure Stream Analytics, zaten yoksa kullanıcı için bir Power BI veri kümesi ve tablo şemanızı oluşturur. Diğer durumlarda, tablonun yeni değerleri ile güncelleştirilir. Şu anda yalnızca bir tabloya bir veri kümesi içinde bulunabilir. 

Power BI, ilk giren ilk çıkar (FIFO) bekletme ilkesi kullanır. Bunu 200.000 satır sayısına ulaşana kadar bir tablodaki veri toplar.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Bir veri türü, Stream Analytics'ten Power BI'a Dönüştür.
Çıkış şema değişirse azure Stream Analytics veri modeli zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişikliklerini ve eklenmesi veya kaldırılmasını sütunları tüm izlenir.

Bu tablo veri türü dönüştürme gelen kapsayan [Stream Analytics veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx) Power bı'a [varlık veri modeli (EDM) türleri](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model), Power BI veri kümesine ve tablo mevcut değilse.

Stream Analytics'ten | Power BI
-----|-----
bigint | Int64
nvarchar(max) | Dize
datetime | Tarih saat
float | çift
Kayıt dizisi | Dize türü, sabit değer "IRecord" veya "IArray"

### <a name="update-the-schema"></a>Şemayı Güncelleştir
Stream Analytics, olay çıktıdaki ilk kümesini göre veri modeli şemayı algılar. Daha sonra gerekirse, veri modeli şemasını özgün şemasına uygun olmayabilir gelen olayları uyum sağlayacak şekilde güncelleştirilir.

Önlemek `SELECT *` satırlarda dinamik şema güncelleştirmesi önlemek için sorgu. Olası performans etkilerinin yanı sıra sonuçları için geçen süre belirsizliğini neden olabilir. Power BI panosunda gösterilmesini gereken tam alanları seçin. Ayrıca, veri değerleri, seçilen veri türüyle uyumlu olmalıdır.


Önceki/geçerli | Int64 | Dize | Tarih saat | çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | çift
çift | çift | Dize | Dize | çift
Dize | Dize | Dize | Dize | Dize 
Tarih saat | Dize | Dize |  Tarih saat | Dize


### <a name="renew-power-bi-authorization"></a>Power BI yetkilendirmeyi Yenile
Stream Analytics işinizi oluşturulduğu veya en son kimlik doğrulaması sonra Power BI hesabınızın parolasını değiştirirse, Stream Analytics yeniden kimlik doğrulamaya zorlayabilir gerekir. Azure multi-Factor Authentication, Azure Active Directory (Azure AD) kiracınız yapılandırılmışsa, ayrıca Power BI yetkilendirme iki haftada yenilemeniz gerekir. Bu sorunun belirtisi, hiçbir iş çıktısının ve bir "kimliği doğrula kullanıcı hatası" işlem günlüklerinde verilmiştir:

  ![Power BI kimlik doğrulaması, kullanıcı hatası](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)

Bu sorunu çözmek için çalışan işini durdurma ve Power BI çıkışınızı gidin. Seçin **yetkilendirmeyi yenilemek** bağlantı ve iş öğesinden yeniden **son durduruldu zamanı** veri kaybını önlemek için.

  ![Çıkış için Power BI yetkilendirmeyi Yenile](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)

## <a name="table-storage"></a>Table Storage
[Azure tablo depolama](../storage/common/storage-introduction.md) yüksek oranda kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanın kullanıcı talebi karşılamak üzere otomatik olarak ölçeklendirebilirsiniz. Tablo, daha az şema kısıtlamaları olan yapılandırılmış veriler için kullanabileceğiniz Microsoft'un NoSQL anahtar/öznitelik deposu depolamadır. Azure tablo depolama, Kalıcılık ve verimli alma verilerini depolamak için kullanılabilir.

Özellik adları ve açıklamalarının bir tablo çıktı oluşturmak için aşağıdaki tabloda listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Bu tablo depolaması sorgu çıkışının yönlendirileceği sorgularında kullanılan bir kolay ad. |
| Depolama hesabı |Çıkış burada gönderiyorsanız depolama hesabının adıdır. |
| Depolama hesabı anahtarı |Depolama hesabıyla ilişkilendirilmiş erişim anahtarı. |
| Tablo adı |Tablonun adı. Tablo yoksa oluşturulur. |
| Bölüm anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı bölüm içinde bir varlığın birincil anahtarının ilk bölümünü oluşturan bir tablo için benzersiz bir tanımlayıcıdır. Bu boyutu en çok 1 KB olabilen bir dize değeridir. |
| Satır anahtarı |Satır anahtarını içeren çıkış sütununun adı. Satır anahtarı bölüm içindeki bir varlık için benzersiz bir tanımlayıcıdır. Bu, bir varlığın birincil anahtarının ikinci bölümü oluşturur. Satır anahtarı boyutu en çok 1 KB olabilen bir dize değeridir. |
| Toplu işlem boyutu |Bir toplu işlem için kayıt sayısı. Varsayılan değer (100) çoğu işleri için yeterli olur. Bkz: [tablo toplu işlem spec](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table._table_batch_operation) bu ayarı değiştirme hakkında daha fazla ayrıntı için. |

## <a name="service-bus-queues"></a>Service Bus kuyrukları
[Service Bus kuyruklarını](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir veya birden çok rakip tüketiciye bir FIFO mesaj teslimatı sağlar. Genellikle, iletiler alınan ve zamana bağlı kuyruğa eklendikleri sırayla alıcılar tarafından işlenebilir. Her bir ileti alındı ve yalnızca bir ileti tüketicisi tarafından alınıp.

Özellik adları ve açıklamalarının bir kuyruk çıkış oluşturmak için aşağıdaki tabloda listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu Service Bus kuyruğu sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Service Bus ad alanı |Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcı. |
| Kuyruk adı |Service Bus kuyruk adı. |
| Kuyruk ilkesi adı |Bir kuyruk oluşturduğunuzda, sıra üzerinde paylaşılan erişim ilkeleri de oluşturabilirsiniz **yapılandırma** sekmesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| Kuyruk ilkesi anahtarı |Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. |
| Olay serileştirme biçimi |Çıktı verilerini seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| Sınırlayıcı |Yalnızca CSV serileştirme için geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir |Yalnızca JSON türü için geçerlidir. **Ayrılmış çizgi** çıktı yeni satırla ayrılmış her bir JSON nesnesi sağlanarak biçimlendirileceğini belirtir. **Dizi** çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. |
| Özellik sütunları | İsteğe bağlı. Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken virgülle ayrılmış sütun. Bu özellik hakkında daha fazla bilgi bölümündedir [çıkışı için özel meta veri özelliklerini](#custom-metadata-properties-for-output). |

Bölüm sayısı [Service Bus SKU ve boyutuna bağlı olarak](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değerdir.

## <a name="service-bus-topics"></a>Hizmet Veri Yolu konuları
Service Bus kuyrukları, gönderenden alıcıya bire bir iletişim yöntemi sunar. [Service Bus konu başlıklarını](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir-çok form iletişim sağlar.

Özellik adları ve açıklamalarının bir konu çıktı oluşturmak için aşağıdaki tabloda listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu Service Bus konusu sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Service Bus ad alanı |Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcı. Yeni bir olay hub'ı oluşturduğunuzda, bir Service Bus ad alanı da oluşturmuş olursunuz. |
| Konu adı |Konular, olay hub'ları ile kuyruklarda benzer Mesajlaşma varlıklarıdır. Cihazlar ve hizmetler, olay akışları toplayacak şekilde tasarlanmışlardır. Bir konu oluşturduğunuzda, belirli bir ada da sağlamıştır. Bir konu başlığına gönderilen iletilerin bir abonelik oluşturulur sürece kullanılamaz, bu nedenle sağlamak konusunda altında bir veya daha fazla abonelik yok. |
| Konu ilkesi adı |Bir konu oluşturduğunuzda konu üzerinde paylaşılan erişim ilkeleri de oluşturabilirsiniz **yapılandırma** sekmesi. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| Konu ilkesi anahtarı |Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. |
| Olay serileştirme biçimi |Çıktı verilerini seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV veya JSON biçimi kullanıyorsanız, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir. |
| Sınırlayıcı |Yalnızca CSV serileştirme için geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Özellik sütunları | İsteğe bağlı. Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken virgülle ayrılmış sütun. Bu özellik hakkında daha fazla bilgi bölümündedir [çıkışı için özel meta veri özelliklerini](#custom-metadata-properties-for-output). |

Bölüm sayısı [Service Bus SKU ve boyutuna bağlı olarak](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için bir benzersiz bir tamsayı değerdir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) dünyanın dört bir yanındaki, zengin sorgu ve şemadan veri modelleri üzerinde otomatik dizin oluşturma sınırsız elastik ölçeğin sunan Global olarak dağıtılmış veritabanı hizmetidir. Stream Analytics için Azure Cosmos DB koleksiyonu seçenekleri hakkında bilgi edinmek için bkz. [çıktı olarak Azure Cosmos DB ile bir Stream Analytics](stream-analytics-documentdb-output.md) makalesi.

Stream analytics'ten Azure Cosmos DB çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

> [!Note]
> Şu anda Azure Stream Analytics tek bağlantısını için Azure Cosmos DB SQL API'sini kullanarak destekler.
> Diğer Azure Cosmos DB API henüz desteklenmiyor. Noktası Azure Stream Analytics Azure Cosmos DB hesaplarına diğer API'lerle oluşturduysanız, verileri düzgün bir şekilde depolanabilir değil.

Aşağıdaki tabloda, bir Azure Cosmos DB çıktı oluşturmak için özellikleri tanımlar.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Bu çıktı, Stream Analytics sorgunuzda başvurmak için bir diğer ad. |
| Havuz | Azure Cosmos DB. |
| İçeri aktarma seçeneği | Seçin ya da **Cosmos DB'den seçin, aboneliğinizin** veya **sağlamak Cosmos DB ayarlarını el ile**.
| Hesap Kimliği | Adı veya uç noktası URI'si, Azure Cosmos DB hesabı. |
| Hesap anahtarı | Azure Cosmos DB hesabı için paylaşılan erişim anahtarı. |
| Database | Azure Cosmos DB veritabanının adı. |
| Koleksiyon adı deseni | Veya deseni kullanılacak koleksiyonların koleksiyon adı. <br />Koleksiyon adı biçimi, bölümlerin 0'dan başladığı isteğe bağlı {partition} belirteci kullanarak oluşturabilirsiniz. İki örnek:  <br /><ul><li> _MyCollection_: "MyCollection" adlı bir koleksiyon olmalıdır.</li>  <li> _{Partition} MyCollection_: Bölümleme sütunu temel.</li></ul> Bölümleme sütunu koleksiyonların mevcut olması gerekir: "MyCollection0," "MyCollection1," "MyCollection2," ve benzeri. |
| Bölüm anahtarı | İsteğe bağlı. Yalnızca, koleksiyon adı deseni {partition} belirteci kullanıyorsanız, buna ihtiyacınız olur.<br /> Bölüm anahtarı için çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adıdır.<br /> Tek bir koleksiyon çıkışı için herhangi bir rastgele çıkış sütunu kullanabilirsiniz. PartitionID buna bir örnektir. |
| Belge Kimliği |İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayalı olduğu birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.

## <a name="azure-functions"></a>Azure İşlevleri
Azure işlevleri, açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmak için kullanabileceğiniz bir sunucusuz işlem hizmetidir. Azure veya iş ortağı Hizmetleri gerçekleşen olaylar tarafından tetiklenen kodu uygulama olanak tanır. Tetikleyicilere yanıt vermeye yönelik bu özelliği, Azure işlevleri, Azure Stream Analytics için doğal bir çıktı sağlar. Bu çıkış bağdaştırıcısı, kullanıcıların Stream Analytics, Azure işlevleri'ne bağlanın ve çok çeşitli olaylara yanıt olarak bir betik veya kod parçası çalıştırmak olanak tanır.

Stream analytics'ten Azure işlevleri çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

Azure Stream Analytics, Azure işlevleri HTTP Tetikleyicileri çağırır. Azure işlevler çıkış bağdaştırıcısı aşağıdaki yapılandırılabilir özelliklerle kullanılabilir:

| Özellik adı | Açıklama |
| --- | --- |
| İşlev uygulaması |Azure işlevler uygulamanızın adı. |
| İşlev |Azure işlevler uygulamanızdaki işlevin adı. |
| Anahtar |Bir Azure işlevi başka bir Abonelikteki kullanmak istiyorsanız, işlevinizi erişmeye yönelik anahtar sağlayarak bunu yapabilirsiniz. |
| En yüksek toplu iş boyutu |Olanak sağlayan bir özellik, Azure işlevinize gönderilen her bir çıktı toplu işinin en büyük boyutunu ayarlayın. Giriş bayt cinsinden birimidir. Varsayılan olarak, bu 262.144 bayt (256 KB) değerdir. |
| En yüksek toplu iş sayısı  |Bir özellik, en yüksek olay sayısı, Azure işlevleri'ne gönderilen her toplu belirtmenize olanak sağlar. Varsayılan değer 100’dür. |

Azure Stream Analytics, bir 413 ("http istek varlığı çok büyük") aldığında özel durumu bir Azure işlevi, Azure İşlevler'e gönderdiği toplu işlerin boyutunu azaltır. Azure işlev kodunuzda bu özel durumun Azure Stream Analytics toplu işler göndermediğinden göndermez emin olmak için kullanın. Ayrıca, işlevde kullanılan en yüksek toplu iş sayı ve boyut değerlerinin Stream Analytics portalına girilen değerlerle tutarlı olduğundan emin olun.

Ayrıca, bir durumda bir zaman penceresinde'na gelen bir olay olduğunda, çıktı oluşturulur. Sonuç olarak, **computeResult** çağrılan işlev değil. Bu davranış, yerleşik pencereli toplama işlevleri ile tutarlıdır.

## <a name="custom-metadata-properties-for-output"></a>Çıkış için özel meta veri özelliklerini 

Kullanıcı özellikleri, giden iletiler için sorgu sütun ekleyebilirsiniz. Bu sütunlar yüküne yapılmaz. Mevcut bir sözlük çıkış iletisi biçiminde özelliklerdir. *Anahtar* sütun adı ve *değer* özellikleri sözlükteki sütun değeri. Kayıt ve dizi tüm Stream Analytics veri türleri desteklenir.  

Desteklenen çıkışlar: 
* Service Bus kuyruğu 
* Service Bus konusu 
* Olay hub'ı 

Aşağıdaki örnekte, eklediğimiz iki alanı `DeviceId` ve `DeviceStatus` meta veri. 
* Sorgu: `select *, DeviceId, DeviceStatus from iotHubInput`
* Çıkış yapılandırması: `DeviceId,DeviceStatus`

![Özellik sütunları](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

Aşağıdaki ekran görüntüsü gösterildiği çıkış iletisi özellikleri EventHub içinde inceledi [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer).

![Özel olay özellikleri](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="partitioning"></a>Bölümleme

Bölüm destek ve çıkış yazarların her çıkış türü sayısı aşağıdaki tabloda özetlenmiştir:

| Çıkış türü | Bölümleme desteği | Bölüm anahtarı  | Çıkış yazıcılar sayısı |
| --- | --- | --- | --- |
| Azure Data Lake Store | Evet | Kullanım: {date} ve {time} belirteçleri yol ön eki deseni. YYYY/MM/DD, GG/AA/YYYY veya AA-GG-YYYY gibi tarih biçimi seçin. SS saat biçimi için kullanılır. | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). |
| Azure SQL Database | Evet | Sorgusunda PARTITION BY yan tümcesi temel. | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). Elde hakkında daha fazla daha iyi yazma verimliliği performansından Azure SQL veritabanı'na veri yükleme zaman bilgi edinmek için [Azure SQL veritabanı için Azure Stream Analytics çıkış](stream-analytics-sql-output-perf.md). |
| Azure Blob depolama | Evet | Kullanım {date} ve {time} belirteçleri, olay alanlarından yol deseni. YYYY/MM/DD, GG/AA/YYYY veya AA-GG-YYYY gibi tarih biçimi seçin. SS saat biçimi için kullanılır. BLOB çıkış bölümlenebilir tek bir özel olay özniteliğiyle {fieldname} veya {datetime:\<belirticisi >}. | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). |
| Azure Event Hubs | Evet | Evet | Bölüm hizalama bağlı olarak değişir.<br /> Olay hub'ı çıkışı için bölüm anahtarı eşit Yukarı Akış (önceki) sorgu adımı hizalandığında yazıcılar sayısı olay hub'ı çıkışında bölüm sayısı ile aynıdır. Her yazıcı kullanan [EventHubSender sınıfı](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) bölüme olayları göndermek için. <br /> Olay hub'ı çıkışı için bölüm anahtarı Yukarı Akış (önceki) sorgu adımı hizalanmadığında yazıcılar sayısı, önceki adımda bölüm sayısı ile aynıdır. Her yazıcı kullanan [SendBatchAsync sınıfı](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) içinde **EventHubClient** çıkış bölümlere tüm olayları göndermek için. |
| Power BI | Hayır | None | Geçerli değildir. |
| Azure Tablo depolama | Evet | Herhangi bir çıktı sütunu.  | Giriş bölümleme için aşağıdaki [tam olarak, sorguları paralel](stream-analytics-scale-jobs.md). |
| Azure Service Bus konusu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için bir benzersiz bir tamsayı değerdir.| Çıkış konudaki bölüm sayısı ile aynıdır.  |
| Azure Service Bus kuyruğu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için bir benzersiz bir tamsayı değerdir.| Çıkış kuyruğuna bölüm sayısı ile aynıdır. |
| Azure Cosmos DB | Evet | Koleksiyon adı deseni {partition} belirteci kullanın. {Partition} değeri, sorgu PARTITION BY yan tümcesi dayanır. | Giriş bölümleme için aşağıdaki [tam olarak, sorguları paralel](stream-analytics-scale-jobs.md). |
| Azure İşlevleri | Hayır | None | Geçerli değildir. |

Çıkış bağdaştırıcınızı bölümlenmemiş bir giriş bölümündeki verileri eksikliği geç varış süreyi kadar bir gecikme neden olur. Böyle durumlarda, işlem hattınızda performans sorunlarına neden bir tek yazıcı için çıkış birleştirilir. Geç varış İlkesi hakkında daha fazla bilgi edinmek için [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Toplu iş boyutu
Azure Stream Analytics, değişken boyutlu toplu olayları işlemek ve çıktıları yazmak için kullanır. Stream Analytics altyapısı genellikle bir kerede tek bir ileti yazma değil emin olun ve verimlilik için toplu işlemi kullanır. Stream Analytics, gelen ve giden olaylarının hızı yüksek olduğunda, daha büyük toplu işler kullanır. Çıkış oranı düşük olduğunda, daha küçük toplu işler gecikme süresi düşük tutmak için kullanır.

Aşağıdaki tabloda bazı toplu işleme çıktısı için dikkat edilecek noktalar açıklanır:

| Çıkış türü | En büyük mesaj boyutu | Toplu iş boyutu en iyi duruma getirme |
| :--- | :--- | :--- |
| Azure Data Lake Store | Bkz: [Data Lake Storage sınırlar](../azure-subscription-service-limits.md#data-lake-store-limits). | Yazma işlemi başına en fazla 4 MB'ı kullanın. |
| Azure SQL Veritabanı | tekil toplu başına en fazla 10.000 satır ekleyin.<br />tekil toplu başına en az 100 satır ekleyin. <br />Bkz: [Azure SQL sınırlar](../sql-database/sql-database-resource-limits.md). |  Her batch başlangıçta eklenmiş en yüksek toplu iş boyutu toplu olur. SQL yeniden denenebilir hatayla göre toplu yarıya (en düşük toplu iş boyutu ulaşana kadar) bölebilirsiniz. |
| Azure Blob depolama | Bkz: [Azure depolama sınırlarını](../azure-subscription-service-limits.md#storage-limits). | En yüksek blob blok boyutu 4 MB'dir.<br />En yüksek blob bock sayısı 50. 000 ' dir. |
| Azure Event Hubs  | İleti başına 256 KB. <br />Bkz: [Event Hubs sınırlar](../event-hubs/event-hubs-quotas.md). |  Giriş/Çıkış bölümleme hizalı değil, her olay, tek tek paketlenmiş **EventData** ve en büyük ileti boyutu (Premium SKU için 1 MB) kadar bir dizi içinde gönderilir. <br /><br />  Birden çok olay tek bir giriş/çıkış bölümleme hizalandığında paketlenir **EventData** , en büyük ileti boyutu en fazla örnek ve gönderilir.  |
| Power BI | Bkz: [Power BI Rest API'si sınırlar](https://msdn.microsoft.com/library/dn950053.aspx). |
| Azure Tablo depolama | Bkz: [Azure depolama sınırlarını](../azure-subscription-service-limits.md#storage-limits). | Tek bir işlem başına 100 varlık varsayılandır. Gerektiği gibi küçük bir değer olarak yapılandırabilirsiniz. |
| Azure Service Bus kuyruğu   | İleti başına 256 KB.<br /> Bkz: [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md). | İleti başına tek bir olay kullanın. |
| Azure Service Bus konusu | İleti başına 256 KB.<br /> Bkz: [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md). | İleti başına tek bir olay kullanın. |
| Azure Cosmos DB   | Bkz: [Azure Cosmos DB sınırlar](../azure-subscription-service-limits.md#azure-cosmos-db-limits). | Yığın boyutu ve sıklığı ayarlanmış dinamik olarak yazma alarak Azure Cosmos DB yanıtlarını. <br /> Stream Analytics'ten alınan önceden belirlenmiş bir sınırlama yoktur. |
| Azure İşlevleri   | | Varsayılan batch 262.144 bayt (256 KB) boyutudur. <br /> Toplu iş başına varsayılan olay sayısı 100'dür. <br /> Toplu iş boyutu yapılandırılabilir ve artırabilir veya azaltılabilir Stream Analytics'te [çıktı seçenekleri](#azure-functions).

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> 
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
