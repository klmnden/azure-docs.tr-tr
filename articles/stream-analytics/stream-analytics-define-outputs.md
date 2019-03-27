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
ms.openlocfilehash: f2318d3026578aef1e0e5c08d4a816b8f95a366f
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448697"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışları anlama
Bu makalede, Azure Stream Analytics işi için çıktıların farklı türde açıklanır. Çıkış, depolamak ve Stream Analytics işi sonuçlarını kaydetmek olanak tanır. Yapabileceğiniz çıktı verilerini kullanarak, İş analizi ve veri depolama verilerinizi daha fazla.

Stream Analytics sorgunuz tasarlarken, çıkış kullanarak adına başvurmak [yan tümcesi içinde](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). İş başına tek bir çıkış veya birden çok INTO yan tümceleri sorguda sağlayarak ihtiyacınız varsa, iş akışı başına birden çok çıkış kullanabilirsiniz.

Test Stream Analytics işi oluşturmak ve düzenlemek için çıkışları kullanabileceğiniz [Azure portalında](stream-analytics-quick-create-portal.md#configure-job-output), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output), ve [Visual Studio](stream-analytics-quick-create-vs.md).

Bazı çıkış türleri desteği [bölümleme](#partitioning), ve [çıktı toplu iş boyutu](#output-batch-size) aktarım hızını iyileştirmek için farklılık gösterir.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics destekler [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store, büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Data Lake Store, işletimsel ve keşfe dönük çözümleme için hiçbir boyutu, türü ve alma hızındaki veriler depolamanızı olanaklı kılar. Stream Analytics, Data Lake Store erişim iznine sahip olması gerekir.

Stream analytics'ten Azure Data Lake Store çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

### <a name="authorize-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı yetki

1. Data Lake Storage, Azure Portalı'ndaki bir çıkış olarak seçildiğinde, mevcut bir Data Lake Store bağlantısı yetkilendirme istenir.

   ![Data Lake Store için Bağlantı Yetkilendirme](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)

2. Data Lake Store için zaten erişimi varsa seçin **şimdi Yetkilendir** ve yukarı belirten bir sayfa açılır **yetkilendirme için yeniden yönlendirme**. Yetkilendirme başarılı olduktan sonra Data Lake Store çıkış yapılandırmanıza olanak tanır sayfası ile sunulur.

3. Kimliği doğrulanmış Data Lake Store hesabı oluşturduktan sonra Data Lake Store çıkışınızı özelliklerini yapılandırabilirsiniz. Aşağıdaki tabloda, özellik adları ve Data Lake Store çıkışınızı yapılandırmak için bunların açıklaması listesidir.

   ![Stream Analytics çıktı olarak Data Lake Store tanımlayın](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgular, bu Data Lake Store sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Hesap adı | Burada, çıkış göndermek Data Lake depolama hesabının adıdır. Aşağı açılan listesini aboneliğinizde mevcut bir Data Lake Store hesapları ile sunulur. |
| Yol ön eki deseni | Belirtilen bir Data Lake Store hesabı dosyalarınızı yazmak için kullanılan dosya yolu. {Date} bir veya daha fazla örneğini belirtmeyi ve {değişkenleri zaman}.<br /><ul><li>Örnek 1: klasör1/günlükler / {tarih} / {time}</li><li>Örnek 2: klasör1/günlükler / {tarih}</li></ul><br />Oluşturulan klasör yapısını zaman damgası UTC ve yerel saat izler.<br /><br />Sonda bir dosya yolu deseni içermiyor "/" son deseni dosya yolunda dosya adı ön eki olarak kabul edilir. <br /><br />Bu durumlarda, yeni dosyalar oluşturulur:<ul><li>Çıkış şemayı değiştirme</li><li>Harici veya dahili bir işi yeniden başlatın.</li></ul> |
| Tarih biçimi | İsteğe bağlı. Ön ek yolu tarih belirteci kullandıysanız, dosyalarınızı düzenlenmiş tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
|Saat biçimi | İsteğe bağlı. Ön ek yolu zaman belirteç kullandıysanız, dosyalarınızı düzenlenmiş saat biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.|
| Encoding | CSV veya JSON biçiminde kullanırken, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.|
| Sınırlayıcı | Yalnızca, CSV serileştirme için de geçerlidir. Stream Analytics, CSV verileri seri hale getirme için birkaç ortak sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgül, noktalı virgül, boşluk, sekme ve dikey çubuk.|
| Biçimlendir | Yalnızca, JSON seri hale getirme için de geçerlidir. Satırla ayrılmış yeni satırla ayrılmış her bir JSON nesnesi sağlayarak çıkış biçimlendirileceğini belirtir. Dizi çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, tercih edilir satırını kullanmak için çıkış dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez beri JSON, ayrılmış.|

### <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi Yenile
İşinizi oluşturulduğu veya en son kimlik doğrulaması parolasını değişmişse Data Lake Store hesabınızı yeniden kimlik doğrulamaya zorlayabilir gerekir. Sağlamalarını yok, iş çıktısı sonuçlar üretmez ve yapılan işlem günlüklerinde gereksinimini belirten bir hata gösterir. Şu anda, kimlik doğrulama belirteci 90 günde bir Data Lake Store çıkış olan tüm işler için el ile yenilenmesi gereken yere bir sınırlama yoktur. Ancak, bu sınırlama tarafından üstesinden gelebilir [yönetilen kimlikleri (Önizleme) kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-managed-identities-adls).

Yetkilendirme, yenilemek için **Durdur** işinizi > Data Lake Store çıkışınızı gidin > tıklatın **yetkilendirmeyi yenilemek** bağlamak ve bir sayfa kısa bir süreliğine pop belirten yukarı **yönlendiriyor Yetkilendirme...** . Sayfa otomatik olarak kapanır ve başarılı olursa, gösteren **yetkilendirme başarıyla yenilendi**. Ardından'ye tıklamanız **Kaydet** sayfanın alt kısmındaki ve iş öğesinden yeniden başlatarak geçebilirsiniz **son durduruldu zamanı** veri kaybını önlemek için.

![Çıkış, Data Lake Store yetkilendirmeyi Yenile](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)

## <a name="sql-database"></a>SQL Veritabanı
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çıkış olarak kendiliğinden ilişkisel veriler veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar için kullanılabilir. Stream Analytics işleri, mevcut bir Azure SQL veritabanı tablosuna yazın. Tablo şemasını, alanları ve işinizin çıktısı olan türlerini tam olarak eşleşmelidir. Bir [Azure SQL veri ambarı](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) aracılığıyla SQL veritabanı output seçeneği de çıktı olarak belirtilebilir. Yazma aktarım hızını iyileştirmek için yollar hakkında bilgi edinmek için bkz [çıktı olarak Azure SQL DB ile bir Stream Analytics](stream-analytics-sql-output-perf.md) makalesi. Özellik adları ve açıklamaları, SQL veritabanı çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu veritabanı sorgusu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Database | Burada, çıkış göndermek veritabanının adı. |
| Sunucu adı | SQL veritabanı sunucu adı. |
| Kullanıcı adı | Veritabanına yazma erişimi olan kullanıcı. Stream Analytics, yalnızca SQL kimlik doğrulamasını da destekler. |
| Parola | Veritabanına bağlanmak için parola. |
| Tablo | Çıkış yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır ve bu tablonun şeması alanları ve bunların türlerini, iş çıktısı tarafından oluşturulan sayısı tam olarak eşleşmesi gerekir. |
|Bölüm düzeni devral| Bu tablo için birden çok yazarları içeren tam olarak paralel topolojisi etkinleştirmek için önceki bir sorgu adımına bölümleme düzeni devralmak sağlar. Daha fazla bilgi için [Azure SQL veritabanı için Azure Stream Analytics çıkış](stream-analytics-sql-output-perf.md).|
|Eşleşme toplu iş sayısı| Önerilen boyut üst sınırı kayıt sayısı, gönderilen her toplu ile işlem ekleyin.|

> [!NOTE]
> Şu anda Azure SQL veritabanı teklifi, Stream Analytics işi çıktısında için desteklenir. Ancak, eklenen bir veritabanı ile SQL Server çalıştıran bir Azure sanal makinesi için desteklenmiyor. Gelecekteki sürümlerde değişebilir budur.
>

## <a name="blob-storage"></a>Blob depolama
BLOB Depolama, bulutta büyük miktarda yapılandırılmamış veriyi depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Azure Blob Depolama ve bunun kullanımını giriş için belgelerine bakın [BLOB'ları kullanmayı](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Özellik adları ve açıklamaları blob çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik adı       | Açıklama                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Çıktı Diğer Adı        | Sorgular, bu blob depolama sorgusu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Depolama Hesabı     | Burada, çıkış göndermek depolama hesabının adı.               |
| Depolama Hesabı Anahtarı | Depolama hesabı ile ilişkili gizli anahtar.                              |
| Depolama kapsayıcısı   | Kapsayıcıları Microsoft Azure Blob hizmetinde depolanan bloblar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinizde, bu blob kapsayıcısı belirtmeniz gerekir. |
| Yol Deseni | İsteğe bağlı. Belirtilen kapsayıcı içinde bloblarınızın yazmak için kullanılan dosya yolu deseni. <br /><br /> Yol deseninde blobları yazılır sıklığı belirtecek şekilde tarih saat değişkenin bir veya daha fazla örneğini kullanmayı tercih edebilirsiniz: <br /> {date} {time} <br /><br />Bir özel {alanı} adı bölümü blob'lara olay verilerinizden belirtmek için özel blob bölümleme kullanabilirsiniz. Alan adı, alfasayısal ve boşluk, kısa çizgi ve alt çizgi içerebilir. Özel alanları kısıtlamaları şunlardır: <ul><li>Küçük harf duyarlılığının olmamasını (sütun "ID" ve "kimliği" sütunu arasında ayırt edilemez) durum</li><li>İç içe alanlar izin verilmez (Bunun yerine bir diğer ad iş sorguda "alanı düzleştirmek için" kullanın)</li><li>İfadeler, alan adı olarak kullanılamaz.</li></ul> <br /><br /> Bu özellik, özel bir tarih/saat biçimi Belirleyicisi yapılandırmaları yolunda kullanımını etkinleştirir. Özel tarih ve saat biçimleri, her seferinde belirtilen biri tarafından alınmış olmalıdır {datetime:\<belirticisi >} anahtar sözcüğü. İzin girişleri \<belirticisi > yyyy, MM, M, dd, d, ss, H, mm, m, ss veya s. {Datetime:\<belirticisi >} anahtar sözcüğü birden çok kez yolunda özel bir tarih/saat yapılandırmaları oluşturmak için kullanılabilir. <br /><br />Örnekler: <ul><li>Örnek 1: küme1/günlükler / {tarih} / {time}</li><li>Örnek 2: küme1/günlükler / {tarih}</li><li>Örnek 3: cluster1 / {client_id} / {tarih} / {time}</li><li>Örnek 4: cluster1 / {datetime:ss} / {myField} sorgu olduğu: SELECT data.myField AS alanım gelen giriş;</li><li>Örnek 5: küme1/yıl {datetime:yyyy} = / ay = {datetime:MM} / gün {datetime:dd} =</ul><br /><br />Oluşturulan klasör yapısını zaman damgası UTC ve yerel saat izler.<br /><br />Dosya adlandırma aşağıdaki kuralını izler: <br /><br />{Yol ön eki Pattern}/schemaHashcode_Guid_Number.extension<br /><br />Örnek çıktı dosyaları:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br /><br /> Bu özellik hakkında daha fazla bilgi için ziyaret [Azure Stream Analytics özel blob çıkış bölümleme](stream-analytics-custom-path-patterns-blob-storage-output.md). |
| Tarih biçimi | İsteğe bağlı. Ön ek yolu tarih belirteci kullandıysanız, dosyalarınızı düzenlenmiş tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
| Saat biçimi | İsteğe bağlı. Ön ek yolu zaman belirteç kullandıysanız, dosyalarınızı düzenlenmiş saat biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding    | CSV veya JSON biçiminde kullanırken, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir. |
| Sınırlayıcı   | Yalnızca, CSV serileştirme için de geçerlidir. Stream Analytics, CSV verileri seri hale getirme için birkaç ortak sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir      | Yalnızca, JSON seri hale getirme için de geçerlidir. Satırla ayrılmış yeni satırla ayrılmış her bir JSON nesnesi sağlayarak çıkış biçimlendirileceğini belirtir. Dizi çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, tercih edilir satırını kullanmak için çıkış dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez beri JSON, ayrılmış. |

BLOB Depolama, çıktı olarak kullanırken, aşağıdaki durumlarda BLOB yeni bir dosya oluşturulur:

* Dosya sayısı izin verilen blok (şu anda 50.000) aşarsa. İzin verilen en yüksek blob boyutu erişmeden blokları izin verilen üst sayısı sınırına. Örneğin, çıkış oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu büyüktür. Çıkış oranı düşükse, her blok daha az veri varsa ve dosya boyutu küçüktür.
* Varsa çıktıda bir şema değişikliği ve çıkış biçimi sabit şemasına (CSV ve Avro) gerektirir.
* Bir iş, harici olarak, durdurma ve başlatma, kullanıcı tarafından veya dahili sistem bakım ya da hata kurtarma için yeniden başlatılması durumunda.
* Tam sorgu bölümlendirilmişse her çıkış bölüm için yeni bir dosya oluşturulur.
* Bir dosya veya bir depolama hesabı kapsayıcı kullanıcı tarafından silindi.
* Yol ön eki deseni kullanarak bölümlenmiş zaman çıkış ise yeni bir blob sorgu bir sonraki saate hareket ettiğinde kullanılır.
* Çıktı tarafından özel bir alan bölümlendirilmişse henüz yoksa yeni bir blob bölüm anahtarı oluşturulur.
* Çıkış burada bölüm anahtarı kardinalite 8000 aşıyor özel bir alana göre bölümlere ayrılmışsa, yeni bir blob bölüm anahtarı oluşturulabilir.

## <a name="event-hub"></a>Olay Hub'ı
[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) hizmetidir yüksek düzeyde ölçeklenebilir Yayımla-abone ol olay yutucu. Bu, saniye başına milyonlarca olayı toplayabilirsiniz. Stream Analytics iş çıktısını başka bir iş akışında girişi olduğunda bir olay Hub'ının çıktı olarak kullanılır.

Olay hub'ı veri akışlarını çıkış olarak yapılandırmak için gereken birkaç parametre yok.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgu çıkışının bu olay hub'ına doğrudan sorgularında kullanılan bir kolay ad. |
| Olay hub'ı ad alanı |Bir olay hub'ı ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, bir olay hub'ı ad alanı da oluşturmuş olursunuz. |
| Olay Hub'ı adı | Olay hub'ı çıkış adı. |
| Olay Hub'ı ilke adı | Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| Olay Hub'ı ilke anahtarı | Olay hub'ı ad kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı. |
| Bölüm anahtarı sütunu [isteğe bağlı] | Bu sütun, olay hub'ı çıkışı için bölüm anahtarını içerir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding | CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| Sınırlayıcı | Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir | Yalnızca, JSON seri hale getirme için de geçerlidir. Satırla ayrılmış yeni satırla ayrılmış her bir JSON nesnesi sağlayarak çıkış biçimlendirileceğini belirtir. Dizi çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresine işini durdurur veya Stream Analytics taşınmıştır, bu dizi kapatıldı. Genel olarak, tercih edilir satırını kullanmak için çıkış dosyası için hala yazıldığı sırada herhangi bir özel işlem gerektirmez beri JSON, ayrılmış. |
| [İsteğe bağlı] özellik sütunları | Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken sütunlar virgülle ayrılmış. "Çıktı için özel meta veri özelliklerini" bölümünde bu özellikle ilgili daha fazla bilgi |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) için bir Stream Analytics işi çıktı olarak analiz sonuçları için bir zengin görselleştirme deneyiminin sunulabilmesi için kullanılabilir. Bu özellik, işletimsel panolar, rapor oluşturma ve raporlama temelli ölçüm için kullanılabilir.

Stream Analytics'ten alınan Power BI çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

### <a name="authorize-a-power-bi-account"></a>Power BI hesabı yetki
1. Power BI, Azure Portalı'ndaki bir çıkış olarak seçildiğinde, varolan bir Power BI kullanıcı yetkilendirmek için veya yeni bir Power BI hesabı oluşturmanız istenir.
   
   ![Power BI kullanıcı Çıkış'ı yapılandırmak için yetkilendirin](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)

2. Yoksa henüz yoksa ve şimdi Yetkilendir'ı tıklatın, yeni bir hesap oluşturun. Şu sayfaya gösterilmektedir:
   
   ![Azure hesabınızdan Power BI'da kimlik doğrulaması](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)

3. Bu adımda, Power BI çıkışına yetkisi vermek için iş veya Okul hesabı sağlayın. Zaten Power BI için kaydolduktan değil, oturum artık seçin. Power BI için kullandığınız iş veya Okul hesabı ile oturum açmış olduğunuz Azure aboneliği hesabından farklı olabilir.

### <a name="configure-the-power-bi-output-properties"></a>Power BI çıktı özelliklerini yapılandırma
Kimliği doğrulanmış Power BI hesabı oluşturduktan sonra Power BI çıkışınızı özelliklerini yapılandırabilirsiniz. Aşağıdaki tabloda, özellik adlarının listesini ve Power BI çıkışınızı yapılandırmak için bunların açıklaması ' dir.

| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıkışının bu Power BI çıkışı yönlendirmek için sorgularında kullanılan bir kolay ad. |
| Grup çalışma alanı |Diğer Power BI kullanıcıları ile veri paylaşımını etkinleştirmek için Power BI hesabınızı içinde grupları seçin veya bir gruba yazmak istemiyorsanız "Çalışma Alanım" seçim yapabilirsiniz. Mevcut bir grubu güncelleştiriliyor, Power BI kimlik doğrulaması yenileme gerektirir. |
| Veri kümesi adı |Bir veri kümesi adı belirtin, Power BI çıkışına kullanmak istediğiniz |
| Tablo adı |Power BI çıkış veri kümesi altında bir tablo adı sağlayın. Şu anda Power BI çıkışına Stream Analytics işlerine yalnızca bir tablo bir veri kümesi olabilir |

Power BI çıkışına ve Pano yapılandırma talimatları için bkz. [Azure Stream Analytics ve Power BI](stream-analytics-power-bi-dashboard.md) makalesi.

> [!NOTE]
> Açıkça tablo ve veri kümesini Power BI panosunda oluşturmayın. Tablo ve veri kümesi otomatik olarak doldurulur iş başlatıldığında ve iş parçacıklarının çıkış Power BI'da oturum başlatır. İş sorgusu hiçbir sonuç veri kümesini oluşturmaz ve tablo oluşturulamayacağını unutmayın. Power BI veri kümesi ve bu Stream Analytics işinde sağlanan aynı ada sahip bir tablo zaten varsa, var olan verilerin üzerine olduğunu unutmayın.
>

### <a name="schema-creation"></a>Şema oluşturma
Azure Stream Analytics, Power BI veri kümesine ve tablo kullanıcı adına bir zaten mevcut değilse oluşturur. Diğer durumlarda, tablonun yeni değerleri ile güncelleştirilir. Şu anda bir sınırlama yoktur, yalnızca bir tabloya bir veri kümesi içinde bulunabilir. Power BI FIFO bekletme ilkesi kullanır. 200.000 satır İsabetleri kadar etkin olduğunda, bir tablodaki veri toplar.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Power BI için veri türü dönüştürmesi Stream analytics'ten
Çıkış şema değişirse azure Stream Analytics veri modeli zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişikliklerini ve eklenmesi veya kaldırılmasını sütunları tüm izlenir.

Bu tablo veri türü dönüştürme gelen kapsayan [Stream Analytics veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx) yayımlamasını güç için [varlık veri modeli (EDM) türleri](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) bir POWER BI veri kümesine ve tablo yoksa.

Stream Analytics'ten | Power BI
-----|-----
bigint | Int64
nvarchar(max) | Dize
datetime | Tarih saat
float | çift
Kayıt dizisi | Dize türü, sabit değer "IRecord" veya "IArray"

### <a name="schema-update"></a>Şema güncelleştirmesi
Stream Analytics, olay çıktıdaki ilk kümesini göre veri modeli şemayı algılar. Daha sonra gerekirse, veri modeli şemasını özgün şemasına uygun değildir gelen olayları uyum sağlayacak şekilde güncelleştirilir.

`SELECT *` Satırlarda dinamik şema güncelleştirmesi önlemek için sorgu kaçınılmalıdır. Olası performans etkilerinin yanı sıra sonuçları için geçen süre belirsizliğini ise ayrıca sonuçlanabilir. Power BI panosunda gösterilen gereken alanları tam seçilmesi gerekir. Ayrıca, veri değerleri, seçilen veri türüyle uyumlu olmalıdır.


Önceki/geçerli | Int64 | Dize | Tarih saat | çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | çift
çift | çift | Dize | Dize | çift
Dize | Dize | Dize | Dize | Dize 
Tarih saat | Dize | Dize |  Tarih saat | Dize


### <a name="renew-power-bi-authorization"></a>Power BI yetkilendirmeyi Yenile
Stream Analytics işinizi oluşturulduğu veya en son kimlik doğrulaması sonra Power BI hesabınızın parolasını değiştirirse, Stream Analytics yeniden kimlik doğrulamaya zorlayabilir gerekir. Multi-Factor Authentication (MFA), Azure Active Directory (AAD) kiracınız yapılandırılmışsa, ayrıca Power BI yetkilendirme iki haftada yenilemeniz gerekir. Bu sorunun belirtisi, hiçbir iş çıktısının ve bir "kimliği doğrula kullanıcı hatası" işlem günlüklerinde verilmiştir:

  ![Power BI kimlik doğrulaması, kullanıcı hatası](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)

Bu sorunu çözmek için çalışan işini durdurma ve Power BI çıkışınızı gidin. Seçin **yetkilendirmeyi yenilemek** bağlantı ve iş öğesinden yeniden **son durduruldu zamanı** veri kaybını önlemek için.

  ![Çıkış için Power BI yetkilendirmeyi Yenile](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)

## <a name="table-storage"></a>Tablo Depolama
[Azure tablo depolama](../storage/common/storage-introduction.md) yüksek oranda kullanılabilir ve ölçeklenebilir depolama sunar, böylece uygulamanın kullanıcı talebi karşılamak üzere otomatik olarak ölçeklendirebilirsiniz. Tablo depolama, Microsoft'un NoSQL anahtar/öznitelik deposu, hangisinin, yapılandırılmış veriler için daha az şema kısıtlamaları olan yararlanabilir. Azure tablo depolama, Kalıcılık ve verimli alma verilerini depolamak için kullanılabilir.

Özellik adları ve açıklamaları tablo çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik Adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Bu tablo depolaması sorgu çıkışının yönlendirileceği sorgularında kullanılan bir kolay ad. |
| Depolama hesabı |Burada, çıkış göndermek depolama hesabının adı. |
| Depolama hesabı anahtarı |Depolama hesabıyla ilişkilendirilmiş erişim anahtarı. |
| Tablo Adı |Tablonun adı. Henüz yoksa bir tablo oluşturulur. |
| Bölüm anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, bir varlığın birincil anahtarının ilk bölümünü forms belirli bir tabloda bölüm için benzersiz bir tanımlayıcıdır. Boyutu en çok 1 KB olabilen bir dize değeridir. |
| Satır anahtarı |Satır anahtarını içeren çıkış sütununun adı. Satır anahtarı, belirli bir bölüme yer alan varlığa ilişkin benzersiz bir tanımlayıcıdır. Bu, bir varlığın birincil anahtarının ikinci bölümü oluşturur. Satır anahtarı boyutu en çok 1 KB olabilen bir dize değeridir. |
| Toplu işlem boyutu |Bir toplu işlem için kayıt sayısı. Varsayılan değer (100) çoğu işleri için yeterli olur. Başvurmak [tablo toplu işlem spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) bu ayarı değiştirme hakkında daha fazla ayrıntı için. |

## <a name="service-bus-queues"></a>Service Bus Kuyrukları
[Hizmet veri yolu kuyrukları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir ilk giren ilk çıkar (FIFO) bir veya birden çok rakip tüketiciye ileti teslimi sunar. Genellikle, iletileri alınan ve burada kuyruğa eklendikleri ve her bir ileti alındı ve yalnızca bir ileti tüketicisi tarafından alınıp zamana bağlı sırada alıcılar tarafından işlenen beklenir.

Özellik adları ve açıklamaları kuyruk çıkış oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu Service Bus kuyruğu sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Service Bus ad alanı |Service Bus ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. |
| Kuyruk adı |Hizmet veri yolu kuyruğu adı. |
| Kuyruk ilkesi adı |Bir kuyruk oluşturduğunuzda kuyruk yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir adı ayarlayın ve erişim anahtarları izinleri vardır. |
| Kuyruk ilkesi anahtarı |Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı |
| Olay serileştirme biçimi |Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON için UTF-8 şu anda desteklenen tek kodlama biçimi yoktur. |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| Biçimlendir |Yalnızca, JSON türü için de geçerlidir. Satırla ayrılmış yeni satırla ayrılmış her bir JSON nesnesi sağlayarak çıkış biçimlendirileceğini belirtir. Dizi çıkış bir JSON nesne dizisi biçimlendirileceğini belirtir. |
| [İsteğe bağlı] özellik sütunları | Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken sütunlar virgülle ayrılmış. "Çıktı için özel meta veri özelliklerini" bölümünde bu özellikle ilgili daha fazla bilgi |

Bölüm sayısı [Service Bus SKU ve boyutuna bağlı olarak](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değerdir.

## <a name="service-bus-topics"></a>Service Bus Konuları
Hizmet veri yolu kuyrukları gönderenden alıcıya, bire bir iletişim yöntemi sunarken [hizmet veri yolu konuları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir-çok form iletişim sağlar.

Özellik adları ve açıklamaları tablo çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgular, bu Service Bus konusu sorgu çıkışı yönlendirmek için kullanılan kolay bir ad. |
| Service Bus ad alanı |Service Bus ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, Service Bus ad alanı da oluşturmuş |
| Konu adı |Konular, olay hub'ları ile kuyruklarda benzer Mesajlaşma varlıklarıdır. Bir dizi farklı cihaz ve hizmetler hizmetten olay akışları toplayacak şekilde tasarlanmışlardır. Bir konu oluşturduğunuzda, belirli bir adı da verilir. Bir konu başlığına gönderilen iletilerin, bir abonelik oluşturulur sürece kullanılamaz, bu nedenle olun konu altında bir veya daha fazla abonelik |
| Konu ilkesi adı |Bir konu oluşturduğunuzda konu yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesi ayarlayın ve erişim anahtarları izinleri bir ada sahip |
| Konu ilkesi anahtarı |Service Bus ad alanı kimlik doğrulaması yapmak için kullanılan paylaşılan erişim anahtarı |
| Olay serileştirme biçimi |Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV veya JSON biçiminde kullanırken, bir kodlama belirtilmelidir. Şu anda desteklenen tek kodlama biçimi UTF-8 kodlamasıdır |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Desteklenen değerler şunlardır: virgülle, noktalı virgül, boşluk, sekme ve dikey çubuk. |
| [İsteğe bağlı] özellik sütunları | [İsteğe bağlı] Giden iletinin yükü yerine özellikleri kullanıcı olarak eklenmesi gereken sütunlar virgülle ayrılmış. "Çıktı için özel meta veri özelliklerini" bölümünde bu özellikle ilgili daha fazla bilgi |

Bölüm sayısı [Service Bus SKU ve boyutuna bağlı olarak](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değerdir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) Global olarak dağıtılmış çok modelli bir veritabanı hizmeti teklifleri sınırsız elastik ölçeğin, dünyanın dört bir yanındaki, zengin sorgu ve otomatik dizinleme şemadan veri modelleri, garantili düşük gecikme süresi ve sektör lideri olan kapsamlı SLA'lar. Stream Analytics için Cosmos DB koleksiyonu seçenekleri hakkında bilgi edinmek için başvurmak [çıktı olarak Cosmos DB ile bir Stream Analytics](stream-analytics-documentdb-output.md) makalesi.

Stream analytics'ten Azure Cosmos DB çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

> [!Note]
> Şu anda Azure Stream Analytics kullanarak CosmosDB bağlantı yalnızca destekler **SQL API**.
> Diğer Azure Cosmos DB API henüz desteklenmiyor. Noktası Azure Stream Analytics Azure Cosmos DB hesaplarına diğer API'lerle oluşturduysanız, verileri düzgün bir şekilde depolanabilir değil.

Aşağıdaki tabloda, bir Azure Cosmos DB çıktı oluşturmak için özellikleri tanımlar.

| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı | Bu çıktı, Stream Analytics sorgunuzda başvurmak için bir diğer ad. |
| Havuz | Cosmos DB |
| İçeri aktarma seçeneği | "Cosmos DB aboneliğinizden seçmek için" seçeneğini belirleyin, veya "sağlama Cosmos DB ayarlarını el ile".
| Hesap kimliği | Adı veya uç noktası URI'si Cosmos DB hesabı. |
| Hesap anahtarı | Cosmos DB hesabı için paylaşılan erişim anahtarı. |
| Database | Cosmos DB veritabanının adı. |
| Koleksiyon adı deseni | Koleksiyon adı veya onların kullanılacak koleksiyonların deseni. <br />Koleksiyon adı biçimi, bölümlerin 0'dan başladığı isteğe bağlı {partition} belirteci kullanılarak oluşturulabilir. İki örnek:  <br />1. _MyCollection_ – "MyCollection" adlı bir koleksiyon bulunmalıdır.  <br />2. _{Partition} MyCollection_ – bölümleme sütunu temel alarak. <br />Bölümleme sütunu koleksiyonların mevcut olması – "MyCollection0", "MyCollection1", "MyCollection2" ve benzeri. |
| Bölüm Anahtarı | İsteğe bağlı. Bu, yalnızca, koleksiyon adı deseni {partition} belirteci kullanıyorsanız gereklidir.<br /> Bölüm anahtarı için çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adıdır.<br /> Tek bir koleksiyon çıkışı için herhangi bir rastgele çıkış sütunu kullanılabilir. Örneğin, bölüm kimliği. |
| Belge Kimliği |İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayalı olduğu birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.

## <a name="azure-functions"></a>Azure İşlevleri
Azure İşlevleri, açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmanıza olanak sağlayan bir sunucusuz işlem hizmetidir. Azure veya üçüncü taraf hizmetlerinde gerçekleşen olayların tetiklediği kodu uygulamanıza olanak tanır. Tetikleyicilere yanıt vermeye yönelik bu özelliği, Azure işlevleri, bir Azure Stream Analytics için doğal bir çıktı sağlar. Bu çıkış bağdaştırıcısı, kullanıcıların Stream Analytics, Azure işlevleri'ne bağlanın ve çok çeşitli olaylara yanıt olarak bir betik veya kod parçası çalıştırmak olanak tanır.

Stream analytics'ten Azure işlevleri çıkışı şu anda Azure Çin (21Vianet) ve Azure Almanya'yı (T-Systems International) bölgelerinde kullanılabilir değil.

Azure Stream Analytics, Azure işlevleri HTTP Tetikleyicileri çağırır. Yeni Azure işlevi çıkış bağdaştırıcısı aşağıdaki yapılandırılabilir özelliklerle kullanılabilir:

| Özellik adı | açıklama |
| --- | --- |
| İşlev uygulaması |Azure işlevler uygulamanızın adı |
| İşlev |Azure işlevler uygulamanızdaki işlevin adı |
| Anahtar |Bir Azure işlevi başka bir Abonelikteki kullanmak istiyorsanız, işlevinizi erişmeye yönelik anahtar sağlayarak bunu yapabilirsiniz |
| En yüksek toplu iş boyutu |Bu özellik, Azure İşlevinize gönderilen her bir çıktı toplu işinin en büyük boyutunu ayarlamak için kullanılabilir. Giriş bayt cinsinden birimidir. Varsayılan olarak, bu değer 262.144 bayt (256 KB) durumdadır. |
| En yüksek toplu iş sayısı  |Adından da anlaşılacağı gibi bu özellik Azure işlevleri'ne gönderilir her toplu işteki en yüksek olay sayısı belirtmenize olanak sağlar. Varsayılan en büyük toplu iş sayısı değeri (100) |

Azure Stream Analytics, Azure işlevden 413 (istek varlığı çok büyük http) özel durum aldığında, Azure işlevleri'ne gönderdiği toplu işlerin boyutunu azaltır. Azure işlev kodunuzda bu özel durumun Azure Stream Analytics toplu işler göndermediğinden göndermez emin olmak için kullanın. Ayrıca, işlevde kullanılan en büyük toplu iş sayı ve boyut değerlerinin Stream Analytics portalına girilen değerlerle tutarlı olduğundan emin olun.

Ayrıca, bir durumda bir zaman penceresinde'na gelen bir olay olduğunda, çıktı oluşturulur. Sonuç olarak, computeResult işlevi çağrılmaz. Bu davranış, yerleşik pencereli toplama işlevleri ile tutarlıdır.

## <a name="custom-metadata-properties-for-output"></a>Çıkış için özel meta veri özelliklerini 

Bu özellik, sorgu sütunlar kullanıcı özelliklerini giden iletilerinize ekleme sağlar. Bu sütunlar yüküne geçmemektedir. Bu özellikler, mevcut bir sözlük çıkış iletisi biçiminde olur. Anahtar sütun adı ve değer özellikleri sözlükteki sütun değeri. Kayıt ve dizi tüm Stream Analytics veri türleri desteklenir.  

Desteklenen çıkışlar: 
* Service Bus Kuyrukları 
* Service Bus Konuları 
* Olay Hub'ı 

Örnek: Aşağıdaki örnekte, meta veriler için 2 alan cihaz kimliği ve DeviceStatus ekleyeceğiz. 
* Query: `select *, DeviceId, DeviceStatus from iotHubInput` .
* Çıkış yapılandırması: `DeviceId,DeviceStatus`.

![Özellik sütunları](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

İleti özelliklerini kullanarak EventHub inceledi çıkış [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer).

   ![Özel olay özellikleri](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="partitioning"></a>Bölümleme

Bölüm destek ve çıkış yazarların her çıkış türü sayısı aşağıdaki tabloda özetlenmiştir:

| Çıkış türü | Bölümleme desteği | Bölüm anahtarı  | Çıkış yazıcılar sayısı |
| --- | --- | --- | --- |
| Azure Data Lake Store | Evet | Kullanım: {date} ve {time} belirteçleri yol ön eki deseni. YYYY/MM/DD, GG/AA/YYYY-AA-GG-YYYY'gibi tarih biçimi seçin. SS saat biçimi için kullanılır. | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). |
| Azure SQL Database | Evet | Temel sorgu PARTITION BY yan tümcesi | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). Elde hakkında daha fazla daha iyi yazma aktarım hızı performansı SQL Azure veritabanı'na veri yükleme zaman bilgi edinmek için [Azure SQL veritabanı için Azure Stream Analytics çıkış](stream-analytics-sql-output-perf.md). |
| Azure Blob depolama | Evet | Kullanım {date} ve {time} belirteçleri, olay alanlarından yol deseni. YYYY/MM/DD, GG/AA/YYYY-AA-GG-YYYY'gibi tarih biçimi seçin. SS saat biçimi için kullanılır. BLOB çıkış bölümlenebilir tek bir özel olay özniteliğiyle {fieldname} veya {datetime:\<belirticisi >}. | Giriş bölümleme için aşağıdaki [tamamen paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). |
| Azure Olay Hub'ı | Evet | Evet | Bölüm hizalama bağlı olarak değişir.<br /> Olay hub'ı bölümleri sayısı olay Hub'ı bölüm anahtarı (Yukarı Akış önceki) sorgu adımı, yazarların sayısı eşit hizalanır aynıdır çıkış çıkış. Her yazıcı EventHub'ın kullandığı [EventHubSender sınıfı](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) bölüme olayları göndermek için. <br /> Çıktı, olay hub'ı bölüm anahtarı (Yukarı Akış önceki) sorgu adımı, yazıcıları sayısını hizalı değil ise, önceki adımda bölüm sayısı ile aynı. Her yazıcı EventHubClient kullanan [SendBatchAsync sınıfı](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) çıkış bölümlere tüm olayları göndermek için. |
| Power BI | Hayır | None | Geçerli değildir. |
| Azure Tablo depolama | Evet | Herhangi bir çıktı sütunu.  | Giriş bölümleme için aşağıdaki [tam olarak, sorguları paralel](stream-analytics-scale-jobs.md). |
| Azure Service Bus konusu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değerdir.| Çıkış konudaki bölüm sayısı ile aynıdır.  |
| Azure Service Bus kuyruğu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değerdir.| Çıkış kuyruğuna bölüm sayısı ile aynıdır. |
| Azure Cosmos DB | Evet | Koleksiyon adı deseni {partition} belirteci kullanın. {partition} değeri, sorgu PARTITION BY yan tümcesi dayanır. | Giriş bölümleme için aşağıdaki [tam olarak, sorguları paralel](stream-analytics-scale-jobs.md). |
| Azure İşlevleri | Hayır | None | Geçerli değildir. |

Çıkış bağdaştırıcınızı bölümlenmemiş bir giriş bölümündeki verileri eksikliği geç varış süreyi kadar bir gecikme neden olur.  Böyle durumlarda, performans sorunlarını, işlem hattınızda neden olabilecek bir tek yazıcı için çıkış birleştirilir. Geç varış İlkesi hakkında daha fazla bilgi edinmek için [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Toplu iş boyutu
Azure Stream Analytics, olayları işlemek ve çıktıları yazmak için değişken boyutu toplu işlemi kullanır. Stream Analytics altyapısı genellikle bir ileti aynı anda yazmaz emin olun ve verimlilik için toplu işlemi kullanır. Hem gelen hem de giden olayları oranı olduğunda yüksek büyük toplu işlemi kullanır. Çıkış oranı düşük olduğunda, daha küçük toplu işler gecikme süresi düşük tutmak için kullanır.

Aşağıdaki tabloda bazı toplu işleme çıktısı için dikkat edilecek noktalar açıklanır:

| Çıkış türü | En büyük mesaj boyutu | Toplu iş boyutu en iyi duruma getirme |
| :--- | :--- | :--- |
| Azure Data Lake Store | Bkz: [Data Lake depolama sınırları](../azure-subscription-service-limits.md#data-lake-store-limits) | Yazma işlemi başına 4 MB'a kadar |
| Azure SQL Database | Tekil toplu başına 10.000 en fazla satır Ekle<br />Tek bir toplu ekleme başına 100 en küçük satır <br />Ayrıca bkz: [Azure SQL sınırlar](../sql-database/sql-database-resource-limits.md) |  Her batch başlangıçta toplu en büyük toplu iş boyutu ile eklenmiş olan ve batch yarısını (en düşük toplu iş boyutu kadar) SQL yeniden denenebilir hatayla göre Böl. |
| Azure Blob depolama | Bkz: [Azure depolama sınırları](../azure-subscription-service-limits.md#storage-limits) | En yüksek Blob blok boyutu 4 MB'tır<br />50000 en yüksek Blob bock sayısı: |
| Azure Olay Hub'ı   | İleti başına 256 KB <br />Ayrıca bkz: [Event Hubs sınırlar](../event-hubs/event-hubs-quotas.md) |   Giriş Çıkış bölümleme hizalama değil, her olay bir EventData içinde tek tek paketlenmiş ve en büyük ileti boyutu (Premium SKU için 1 MB) kadar bir dizi içinde gönderilir. <br /><br />  Giriş-Çıkış bölümleme hizalandığında, birden çok olayı kadar en büyük ileti boyutu tek bir EventData halinde paketlenmiş ve gönderilir.  |
| Power BI | Bkz: [sınırlar Power BI Rest API'si](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure Tablo depolama | Bkz: [Azure depolama sınırları](../azure-subscription-service-limits.md#storage-limits) | Varsayılan tek işlem başına 100 varlık olduğundan ve gerektiği gibi küçük bir değer olarak yapılandırılabilir. |
| Azure Service Bus kuyruğu   | İleti başına 256 KB<br /> Ayrıca bkz: [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek olay |
| Azure Service Bus konusu | İleti başına 256 KB<br /> Ayrıca bkz: [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek olay |
| Azure Cosmos DB   | Bkz: [Azure Cosmos DB sınırlar](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Toplu iş boyutu ve sıklığı dinamik olarak ayarlanmış CosmosDB yanıtları yazmaktır. <br /> Stream Analytics'ten alınan belirlenmiş sınırlama yoktur. |
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
