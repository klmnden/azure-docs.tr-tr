---
title: Azure Stream Analytics çıkışlarından anlama
description: Bu makalede Azure akış analizi analiz sonuçları için Power BI dahil olmak üzere, kullanılabilir veri çıkış seçenekleri açıklanmaktadır.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/14/2018
ms.openlocfilehash: f2f616c5908d8583764425b62acd1650283d0695
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701726"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Azure Stream Analytics çıkışlarından anlama
Bu makalede Azure akış analizi işi için çıktıların farklı türleri açıklanmaktadır. Çıkışları depolamak ve Stream Analytics işi sonuçlarını kaydetmenize olanak tanır. Çıktı verileri kullanarak bunu yapabilirsiniz daha fazla İş analizi ve verilerinizi veri ambarı. 

Stream Analytics sorgu tasarlarken, çıkış kullanmanın adına başvuran [yan tümcesi içinde](https://msdn.microsoft.com/azure/stream-analytics/reference/into-azure-stream-analytics). İş başına tek bir çıkış veya sorguda birden çok INTO yan tümcesi sağlayarak gerekiyorsa, iş akışı başına birden çok çıkış kullanabilirsiniz.

Oluşturma, düzenleme ve test Stream Analytics işi için çıkarır, kullanabileceğiniz [Azure portal](stream-analytics-quick-create-portal.md#configure-output-to-the-job), [Azure PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.Net API](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/en-us/rest/api/streamanalytics/stream-analytics-output), ve [Visual Studio](stream-analytics-tools-for-visual-studio.md).

Bazı çıkışları türleri Destek [bölümleme](#partitioning), ve [çıkış toplu boyutları](#output-batch-size) verimliliği en iyi duruma getirmek üzere değişir.


## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics destekler [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store, büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Data Lake Store herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için verilerin depolamanıza olanak sağlar. Akış analizi Data Lake Store erişmek için yetkili gerekir.

Stream Analytics çıktısını Azure Data Lake Store şu anda Azure Çin (21Vianet) ve Azure Almanya (T-sistemleri uluslararası) bölgelerde kullanılabilir değil.

### <a name="authorize-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı yetki

1. Data Lake Storage, Azure portalında bir çıkış olarak seçildiğinde, mevcut bir Data Lake Store bağlantı yetkilendirmek istenir.  

   ![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Data Lake Store'a erişim zaten varsa, seçin **şimdi Yetkilendir** ve yukarı belirten bir sayfa açılır **yetkilendirme için yeniden yönlendirme**. Yetkilendirme başarılı olduktan sonra Data Lake Store çıkış yapılandırmanıza izin verir sayfayla sunulur.

3. Kimliği doğrulanmış Data Lake Store hesabına sahip olduğunda, Data Lake Store çıktı için özelliklerini yapılandırabilirsiniz. Aşağıdaki tablo özellik adları ve Data Lake Store çıkış yapılandırmak için açıklamalarına listesidir.

   ![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

| Özellik adı | Açıklama | 
| --- | --- |
| Çıktı diğer adı | Bu Data Lake Store sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. | 
| Hesap adı | Burada, Çıkış göndermeyi Data Lake Storage hesabının adı. Aboneliğinizdeki kullanılabilir Data Lake Store hesapları aşağı açılan listesi ile sunulur. |
| Yol ön eki deseni | Belirtilen veri Gölü deposu hesabı içinde dosyalarınızı yazmak için kullanılan dosya yolu. {Date} bir veya daha fazla örneğini belirtmeyi ve {değişkenleri time}.</br><ul><li>Örnek 1: klasör1/logs / {date} / {time}</li><li>Örnek 2: klasör1/logs / {date}</li></ul><br>Zaman damgası oluşturulan klasör yapısının UTC ve yerel saat izler.</br><br>Dosya yolu deseni bitmiyorsa içermiyor "/" dosya yolu son desende filename önek olarak kabul edilir. </br></br>Bu durumlarda yeni dosyalar oluşturulur:<ul><li>Çıktı şemada değiştirme</li><li>Harici veya dahili bir işi yeniden başlatın.</li></ul> |
| Tarih biçimi | İsteğe bağlı. Bir tarih belirteci önek yolunda kullanılırsa, dosyalarınızı organize edilmiştir tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
|Saat biçimi | İsteğe bağlı. Zaman belirteci önek yolunda kullanılırsa, dosyalarınızı düzenlenmiş zaman biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.| 
| Encoding | Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.|
| Sınırlayıcı | Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Desteklenen değerler: virgül, noktalı virgül, boşluk, sekme ve, dikey çubuk.|
| Biçimlendir | Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış.|

### <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi yenileyin
Data Lake Store hesabınızın parolasını işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse sağlamalarını gerekir. Sağlamalarını yok, işinizi çıktısı sonuçlara neden değil ve işlem günlükleri olarak yeniden kimlik doğrulama gereksinimini belirten bir hata gösterir. Şu anda, burada kimlik doğrulama belirteci 90 günde Data Lake Store çıktıyla tüm işleri el ile yenilenmesi gerekiyor bir sınırlama yoktur. 

Yetkilendirme, yenilemek için **durdurmak** işinizi > gidin, Data Lake Store çıkış >'ı tıklatın **yetkilendirmeyi yenileyin** bağlantı ve bir sayfa kısa bir süre için pop belirten yukarı **yeniden yönlendirme Yetkilendirme...** . Sayfa otomatik olarak kapanır ve başarılı olursa, gösterir **yetkilendirme başarıyla yeniledi**. Ardından tıklamanız gerekir **kaydetmek** sayfanın sonundaki ve işinizi gelen yeniden başlatarak geçebilirsiniz **son durdurulma zamanı** veri kaybını önlemek için.

![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Veritabanı
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çıkış olarak kendiliğinden ilişkisel veriler için veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar için kullanılabilir. Akış analizi işleri, bir Azure SQL veritabanında var olan bir tabloya yazma.  Tablo şemasını alanları ve işinizi çıktısını olan türlerinin tam olarak eşleşmelidir. Bir [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) SQL veritabanı output seçeneği de aracılığıyla bir çıktı olarak da belirtilebilir. Aşağıdaki tablo özellik adları ve SQL veritabanı çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Bu veritabanı için sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Database | Burada, Çıkış göndermeyi veritabanının adı. |
| Sunucu adı | SQL veritabanı sunucusu adı. |
| Kullanıcı adı | Veritabanına yazma erişimi olan kullanıcı. |
| Parola | Databas.e bağlanmak için parola |
| Tablo | Çıktı yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır ve bu tablonun şeması alanları ve, iş çıkışı tarafından oluşturulan türlerinin sayısı tam olarak eşleşmelidir. |

> [!NOTE]
> Şu anda Azure SQL veritabanı teklifi Stream Analytics işi çıktısında desteklenir. Ancak, bağlı olan bir veritabanı SQL Server çalıştıran bir Azure sanal makine desteklenmiyor. Bu, sonraki sürüm değişikliklerine tabidir.
> 

## <a name="blob-storage"></a>Blob depolama
BLOB storage bulutta büyük miktarda yapılandırılmamış veriyi depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar.  Azure Blob Depolama ve kullanım giriş için belgelerine bakın [BLOB'ları kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Özellik adlarının ve kendi açıklama blob çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama | 
| --- | --- |
| Çıkış Diğer Adı | Bu blob depolama sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Depolama Hesabı | Burada, Çıkış göndermeyi depolama hesabı adı. |
| Depolama Hesabı Anahtarı | Depolama hesabıyla ilişkili gizli anahtar. |
| Depolama kapsayıcısı | Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir. |
| Yol deseni | İsteğe bağlı. Belirtilen kapsayıcı içinde bloblarınızın yazmak için kullanılan dosya yolu deseni. </br></br> Yol deseninde BLOB'lar yazılır sıklığını belirtmek için tarih saat değişkenin bir veya daha fazla örneğini kullanmayı seçebilirsiniz: </br> {date} {time} </br> </br>İçin kaydolduysanız [Önizleme](https://aka.ms/ASAPreview), olay verilerinizden göre bölüm BLOB kadar olan bir özel {alanı} adı burada alan adı alfasayısal ve boşluk içerebilir tire ve alt çizgi de belirtebilir. Özel alanları kısıtlamaları aşağıdakileri içerir: <ul><li>(Sütun "ID" ve "id" sütun arasında ayrım) büyük/küçük duyarsızlığı durumda</li><li>İç içe alanlar izin verilmiyor (Bunun yerine bir diğer ad iş sorguda düzleştirmek"alanı için" kullanma)</li><li>İfadeler bir alan adı kullanılamaz</li></ul>Örnekler: <ul><li>Örnek 1: cluster1/logs / {date} / {time}</li><li>Örnek 2: cluster1/logs / {date}</li><li>Örnek 3 (Önizleme): cluster1 / {client_id} / {date} / {time}</li><li>Örnek 4 (Önizleme): cluster1 / {myField} sorgu olduğu: SELECT data.myField AS alanım gelen giriş;</li></ul><br>Zaman damgası oluşturulan klasör yapısının UTC ve yerel saat izler.</br><BR> Dosya adlandırma aşağıdaki kuralını aşağıdaki gibidir: </br> {Yol öneki Pattern}/schemaHashcode_Guid_Number.extension </br></br> Örnek çıktı dosyaları: </br><ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li><li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul><br/>
| Tarih biçimi | İsteğe bağlı. Bir tarih belirteci önek yolunda kullanılırsa, dosyalarınızı organize edilmiştir tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG |
| Saat biçimi | İsteğe bağlı. Zaman belirteci önek yolunda kullanılırsa, dosyalarınızı düzenlenmiş zaman biçimini belirtin. Şu anda desteklenen tek değer HH ' dir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir.
| Encoding | Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir. |
| Sınırlayıcı | Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |
| Biçimlendir | Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış. |

BLOB Depolama çıkış olarak kullanırken, aşağıdaki durumlarda blob yeni bir dosya oluşturulur:

* Dosya izin verilen blokları (şu anda 50.000) üst sınırını aşarsa. En fazla izin verilen blob boyutu erişmeden blokları izin verilen maksimum sayısı üst sınırına. Örneğin, çıkış oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu. Çıkış oranı düşükse, her bloğu daha az veri vardır ve dosya boyutu küçüktür.
* Varsa çıktıda bir şema değişikliği vardır ve çıkış biçimi sabit şemasına (CSV ve Avro) gerektirir.  
* Bir iş, durdurma ve makineyi başlatmayı harici olarak bir kullanıcı tarafından veya dahili sistem bakımı veya hata kurtarma için yeniden durumunda.  
* Sorgu tam olarak bölümlenmiş, yeni dosya her çıktı bölümü için oluşturulur.  
* Bir dosya veya bir kapsayıcı depolama hesabının kullanıcı tarafından silindi.  
* Çıkış yolu önek deseni kullanarak bölümlenmiş saat ise, sorgu sonraki saat taşındığında yeni blob kullanılır.
* Çıktı özel bir alan tarafından bölümlenmişse henüz yoksa yeni bir blob bölüm anahtarı oluşturulur.
* Çıktı nerede partition anahtar önemliliği 8000 aşıyor özel bir alan tarafından bölümlenmişse yeni blob bölüm anahtarı oluşturulabilir.

## <a name="event-hub"></a>Olay Hub'ı
[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) hizmetidir'ın yüksek düzeyde ölçeklenebilir yayımlama-abone olma olay yutucu. Saniye başına milyonlarca olayı toplayabilirsiniz. Akış analizi işi çıktısını başka bir iş akışında girişi olduğunda bir olay hub'ı çıktı olarak kullanılır.

Olay hub'ı veri akışlarını çıkış olarak yapılandırmak için gereken birkaç parametre vardır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Bu olay Hub'ına sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Olay Hub'ı ad alanı |Bir Event Hub'ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, bir olay hub'ı ad alanı da oluşturmuş olursunuz. |
| Olay Hub'ı adı | Olay hub'ı çıkışı adı. |
| Olay Hub'ı ilke adı | Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| Olay Hub'ı ilke anahtarı | Olay hub'ı ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı. |
| Bölüm anahtarı sütunu [isteğe bağlı] | Bu sütun, olay hub'ı çıkışı bölüm anahtarı içerir. |
| Olay serileştirme biçimi | Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding | CSV ve JSON, UTF-8 şu anda desteklenen tek kodlama biçimi içindir. |
| Sınırlayıcı | Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |
| Biçimlendir | Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) Stream Analytics işi için bir çıktı olarak çözümleme sonuçları için bir zengin görselleştirme deneyimi sağlamak için kullanılabilir. Bu özellik, işletimsel panoları, rapor oluşturma ve raporlama güdümlü ölçüm için kullanılabilir.

Power BI çıkışı Stream Analytics şu anda Azure Çin (21Vianet) ve Azure Almanya (T-sistemleri uluslararası) bölgelerde kullanılabilir değil.

### <a name="authorize-a-power-bi-account"></a>Power BI hesabınız yetkilendirmek
1. Power BI, Azure portalında bir çıkış olarak seçildiğinde, yeni bir Power BI hesabı oluşturmak için veya varolan bir Power BI kullanıcı yetkilendirmek için istenir.  
   
   ![Power BI kullanıcı yetkilendirme](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2. Verme henüz bir tane ve şimdi Yetkilendir'i tıklatın, yeni bir hesap oluşturun.  Aşağıdaki sayfası gösterilir:
   
   ![Azure hesabı Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3. Bu adımda, Power BI çıkışı yetkisi vermek için iş veya Okul hesabı sağlayın. Zaten Power BI için kayıtlı değilsiniz, oturum şimdi seçin. Power BI için kullandığınız iş veya Okul hesabı ile oturum açmış olduğunuz Azure aboneliği hesabından farklı olabilir.

### <a name="configure-the-power-bi-output-properties"></a>Power BI çıktı özelliklerini yapılandırın
Kimliği doğrulanmış Power BI hesabınızı edindikten sonra Power BI çıktı için özelliklerini yapılandırabilirsiniz. Aşağıdaki tablo, özellik adlarının listesi ve bunların açıklaması, Power BI çıkışı yapılandırmak için ' dir.

| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Bu Powerbı çıkış sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Grup çalışma alanı |Diğer Power BI kullanıcılarıyla veri paylaşımını etkinleştirmek için Power BI hesabınızı içinde grupları seçin veya bir gruba yazmak istemiyorsanız "Çalışma Alanım" seçin.  Varolan bir grup güncelleştirme, Power BI kimlik doğrulamayı yenilemesini gerektirir. |
| Veri kümesi adı |Bir veri kümesi adı sağlayın, kullanmak Power BI çıktısı için istenen |
| Tablo adı |Power BI çıkış veri kümesi altında bir tablo adı sağlayın. Şu anda Power BI çıkışı akış analizi işleri yalnızca bir tablo bir veri kümesinde olabilir |

Power BI çıkışı ve Pano yapılandırma kılavuz için bkz: [Azure akış analizi & Power BI](stream-analytics-power-bi-dashboard.md) makalesi.

> [!NOTE]
> Açıkça veri kümesi ve tablo Power BI Panonuzda oluşturmayın. Veri kümesi ve tablo otomatik olarak doldurulur iş başlatıldığında ve iş pompa çıkış Power BI'da oturum başlatır. Sorgu iş herhangi bir sonuç, veri kümesi oluşturmaz ve tablo oluşturulamayacağını unutmayın. Power BI veri kümesi ve bu Stream Analytics işinde sağlanan aynı ada sahip bir tablo zaten sahipse, var olan verileri yazılır unutmayın.
> 

### <a name="schema-creation"></a>Şema oluşturma
Bir zaten yoksa, azure Stream Analytics Power BI veri kümesi ve kullanıcı adına tablo oluşturur. Diğer durumlarda, tablo yeni değerlerle güncelleştirilir. Şu anda bir sınırlama yoktur, yalnızca bir tablo bir veri kümesi içinde bulunabilir.

### <a name="data-type-conversion-from-stream-analytics-to-power-bi"></a>Power BI-Stream Analytics veri türü dönüşümü
Çıkış şeması değişirse azure Stream Analytics veri modeli çalışma zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişiklikleri ve eklenmesi veya sütunları kaldırılması tüm izlenir.

Bu tablo veri türü dönüştürmelerini gelen kapsar [Stream Analytics veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx) yayımlamasını güç için [varlık veri modeli (EDM) türleri](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) POWER BI veri kümesi ve tablo yoksa.

Akış analizi | Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Dize
datetime | Tarih saat
float | Çift
Kayıt dizisi | Türü, sabit değer "IRecord" veya "IArray" dize

### <a name="schema-update"></a>Şema güncelleştirmesi
Akış analizi çıktıda olayları ilk dizi göre veri modeli şeması oluşturur. Daha sonra gerekirse, veri modeli şeması özgün şemasına uygun değildir gelen olayları uyum sağlayacak şekilde güncelleştirildi.

`SELECT *` Dinamik şema güncelleştirmesi satırlarda önlemek için sorgu kaçınılmalıdır. Olası performans etkileri ek olarak, sonuçlar için harcanan süre belirsizliğini ise ayrıca sonuçlanabilir. Power BI Panosu üzerinde gösterilen gereken alanları tam seçilmelidir. Ayrıca, veri değerlerinin seçtiğiniz veri türü ile uyumlu olmalıdır.


Önceki/geçerli | Int64 | Dize | Tarih saat | Çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | Çift
Çift | Çift | Dize | Dize | Çift
Dize | Dize | Dize | Dize |  | Dize | 
Tarih saat | Dize | Dize |  Tarih saat | Dize


### <a name="renew-power-bi-authorization"></a>Power BI yetkilendirmeyi yenileyin
Stream Analytics işiniz oluşturulmuş veya son kimliği doğrulanmış sonra Power BI hesabınızın parolasını değiştirirse, akış analizi kimlik doğrulamaya gerekir. Çok faktörlü kimlik doğrulama (MFA), Azure Active Directory (AAD) Kiracı yapılandırdıysanız, ayrıca Power BI yetkilendirme iki haftada yenilemek gerekir. Bu sorun belirtisi hiçbir iş çıktısı ve "kimlik doğrulama kullanıcı hatayla" işlem günlükleri şöyledir:

  ![Power BI yenileme belirteci hata](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Bu sorunu çözmek için çalışan bir işi durdurmak ve Power BI çıktısına gidin.  Seçin **yetkilendirmeyi yenileyin** gelen işi yeniden başlatın ve bağlama **son durdurulma zamanı** veri kaybını önlemek için.

  ![Power BI yetkilendirme yeniler](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tablo Depolama
[Azure Table storage](../storage/common/storage-introduction.md) yüksek oranda kullanılabilir, yüksek düzeyde ölçeklenebilir depolama sunar, böylece bir uygulamayı otomatik olarak kullanıcı taleplerini karşılayacak şekilde ölçeklendirebilirsiniz. Tablo depolama hangisinin şemanın daha az kısıtlamalar ile yapılandırılmış veri için yararlanabilirsiniz Microsoft'un NoSQL anahtar/öznitelik deposudur. Azure Table storage kalıcılığını ve verimli alma verilerini depolamak için kullanılabilir.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik Adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Bu tablo depolama sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Depolama hesabı |Burada, Çıkış göndermeyi depolama hesabı adı. |
| Depolama hesabı anahtarı |Depolama hesabıyla ilişkili erişim anahtarı. |
| Tablo Adı |Tablonun adı. Henüz yoksa tablo oluşturulan. |
| Bölüm anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, bir varlığın birincil anahtarının ilk bölümü forms belirli bir tablodaki bölümde benzersiz tanımlayıcısıdır. En çok 1 KB olabilen bir dize değeridir. |
| Satır anahtarı |Satır anahtarını içeren çıkış sütununun adı. Satır anahtarını belli bir bölüm içinde bir varlık için benzersiz bir tanımlayıcı değil. Bir varlığın birincil anahtarının ikinci bölümü oluşturur. Satır anahtarı boyutu en çok 1 KB olabilen bir dize değeridir. |
| Toplu işlem boyutu |Bir toplu işlemi için kayıt sayısı. Varsayılan değer (100) çoğu işleri yeterli olur. Başvurmak [tablo toplu işlem spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) bu ayarını değiştirme hakkında daha fazla ayrıntı için. |
 
## <a name="service-bus-queues"></a>Service Bus Kuyrukları
[Hizmet veri yolu kuyrukları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir ilk olarak, bir veya birden çok rakip tüketiciye ilk çıkar (FIFO) ileti teslimi sunar. Genellikle, ileti aldı ve hangi kuyruğa eklendikleri ve her ileti alındı ve tek bir ileti tüketicisi tarafından alınıp zamana bağlı düzende alıcılar tarafından işlenen beklenir.

Aşağıdaki tablo özellik adları ve sıra çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı |Bu hizmet veri yolu kuyruğu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Service Bus ad alanı |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. |
| Kuyruk adı |Hizmet veri yolu kuyruğu adı. |
| Kuyruk ilkesi adı |Bir kuyruk oluşturduğunuzda kuyruk yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| Kuyruk ilkesi anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olay serileştirme biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 desteklenen tek kodlama biçimi şu anda içindir. |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |
| Biçimlendir |Yalnızca JSON türü için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. |

Bölüm sayısı [Service Bus SKU ve boyutuna göre](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.

## <a name="service-bus-topics"></a>Service Bus Konuları
Hizmet veri yolu kuyrukları gönderenden alıcıya, bire bir iletişim yöntemi sunarken [Service Bus konu başlıklarına](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir-çok form iletişim sağlar.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Bu hizmet veri yolu konusu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Service Bus ad alanı |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş |
| Konu adı |Konular, olay hub'ları ve kuyrukları benzer Mesajlaşma varlıklarıdır. Bir dizi farklı cihaz ve Hizmetleri hizmetten olay akışları toplayacak şekilde tasarlanmışlardır. Bir konu oluşturduğunuzda, ayrıca belirli bir ad verilir. Bir abonelik yapılandırılmadığı sürece bir konu başlığına gönderilen iletileri kullanılabilir değilse, böylece olun konu altında bir veya daha fazla abonelik |
| Konu ilkesi adı |Bir konu oluşturduğunuzda konu yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesi ayarlayın ve erişim anahtarları izinleri bir ada sahip |
| Konu ilkesi anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olay serileştirme biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8 durumda |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |

Bölüm sayısı [Service Bus SKU ve boyutuna göre](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) hizmet teklifleri sınırsız esnek ölçek dünya zengin sorgu ve şema belirsiz veri modelleri otomatik dizin oluşturma işlemi geçici düşük gecikme süresi garanti ve endüstri lideri Genel dağıtılmış, birden çok model bir veri tabanıdır kapsamlı SLA. Stream Analytics Cosmos DB koleksiyonu seçenekleri hakkında bilgi edinmek için bkz [Stream Analytics çıkış olarak Cosmos DB ile](stream-analytics-documentdb-output.md) makalesi.

Stream Analytics Azure Cosmos DB çıktısı şu anda Azure Çin (21Vianet) ve Azure Almanya (T-sistemleri uluslararası) bölgelerde kullanılabilir değil.

> [!Note]
> Şu anda Azure akış analizi yalnızca CosmosDB kullanarak bağlantı destekler **SQL API**.
> Diğer Azure Cosmos DB API'leri henüz desteklenmiyor. Noktası Azure akış analizi Azure Cosmos DB hesaplarına diğer API'leri ile oluşturduysanız, verilerin düzgün depolanabilir değil. 

Aşağıdaki tabloda Azure Cosmos DB çıktı oluşturmak için özellikleri açıklanmaktadır.
| Özellik adı | açıklama |
| --- | --- |
| Çıktı diğer adı | Bu akış analizi sorgu çıktı başvurmak için bir diğer ad. |
| Havuz | Cosmos DB |
| İçeri aktarma seçeneği | "Cosmos DB aboneliğinizden seçmek için" ya da seçin, veya "sağla Cosmos DB ayarlarını el ile".
| Hesap kimliği | Adı veya bitiş noktası Cosmos DB hesabının URI'si. |
| Hesap anahtarı | Cosmos DB hesabı için paylaşılan erişim anahtarı. |
| Database | Cosmos DB veritabanı adı. |
| Koleksiyon adı deseni | Koleksiyon adını veya kullanılacak koleksiyonlar için kendi desen. <br/>Koleksiyon adı biçimi, burada bölümlerin 0'dan başlar isteğe bağlı {partition} belirteci kullanılarak oluşturulabilir. İki örnek:  <br/>1. _MyCollection_ – "MyCollection" adlı bir koleksiyon bulunmalıdır.  <br/>2. _MyCollection {partition}_ – bölümleme sütunu bağlı. <br/>Bölümleme sütunu koleksiyonların mevcut olması – "MyCollection0", "MyCollection1", "MyCollection2" ve benzeri. |
| Bölüm Anahtarı | İsteğe bağlı. Bu, yalnızca, koleksiyon adı deseni {partition} belirteci kullanıyorsanız gereklidir.<br/> Bölüm anahtarı çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adıdır.<br/> Tek koleksiyon çıktı için herhangi bir rastgele çıkış sütunu kullanılabilir. Örneğin, PartitionID. |
| Belge Kimliği |İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayalı birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.  

## <a name="azure-functions"></a>Azure İşlevleri
Azure İşlevleri, açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmanıza olanak sağlayan bir sunucusuz işlem hizmetidir. Azure veya üçüncü taraf hizmetleri gerçekleşen olaylar tarafından tetiklenen kodları uygulama olanak sağlar.  Tetikleyiciler için yanıt Özelliği Azure işlevlerinin bir Azure akış analizi için doğal bir çıktı kolaylaştırır. Bu çıkış bağdaştırıcısı Stream Analytics Azure işlevleri bağlanmak ve yanıt olayları çeşitli olarak bir komut dosyası veya kod parçası çalıştırmak kullanıcıların sağlar.

Stream Analytics Azure işlevleri çıktısı şu anda Azure Çin (21Vianet) ve Azure Almanya (T-sistemleri uluslararası) bölgelerde kullanılabilir değil.

Azure Stream Analytics Azure işlevleri HTTP Tetikleyicileri çağırır. Aşağıdaki yapılandırılabilir özelliklere sahip yeni Azure işlevi çıkış bağdaştırıcısı kullanılabilir:

| Özellik adı | açıklama |
| --- | --- |
| İşlev uygulaması |Azure işlevleri uygulamanızın adı |
| İşlev |Azure işlevleri uygulamanızda işlevin adı |
| Anahtar |Başka bir abonelik Azure işlevinden kullanmak istiyorsanız, işlevinizi erişmeye yönelik anahtar sağlayarak bunu yapabilirsiniz |
| En yüksek toplu iş boyutu |Bu özellik, Azure işlevinizi gönderilen her bir çıkış toplu iş boyutu üst sınırı ayarlamak için kullanılabilir. Varsayılan olarak, bu değer 256 KB'dir |
| En yüksek toplu iş sayısı  |Adından da anlaşılacağı gibi bu özellik için Azure işlevleri gönderilir her yığında olayların maksimum sayısını belirtmenize olanak sağlar. Varsayılan Maksimum toplu iş sayısı değeri 100'dür |

Azure Stream Analytics Azure işlevinden 413 (http istek varlığı çok büyük) özel durum aldığında, Azure işlevleri gönderir toplu boyutunu azaltır. Azure işlevi kodunuzda bu özel durumun Azure akış analizi büyük boyutlu toplu göndermez emin olmak için kullanın. Ayrıca, işlevde kullanılan Maksimum toplu iş sayısı ve boyutu değerleri Stream Analytics portalda girdiğiniz değerleri ile tutarlı olduğundan emin olun. 

Ayrıca, bir durumda bir zaman penceresi için giriş olay olduğunda, çıktı oluşturulur. Sonuç olarak, computeResult işlev çağrılmaz. Bu davranış, yerleşik pencereli toplama işlevleri tutarlıdır.

## <a name="partitioning"></a>Bölümleme

Bölüm destek ve her bir çıkış türü için çıktı yazıcılarının sayısı aşağıdaki tabloda özetlenmiştir:

| Çıkış türü | Bölümleme desteği | Bölüm anahtarı  | Çıktı yazıcılarının sayısı | 
| --- | --- | --- | --- |
| Azure Data Lake Store | Evet | Kullanım {date} ve {time} belirteçleri yol önek deseni. Gibi YYYY/AA/GG, GG/AA/YYYY-AA-GG-YYYY tarih biçimini seçin. SS saat biçimi için kullanılır. | Giriş için bölümleme izleyen [tam olarak paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). | 
| Azure SQL Database | Hayır | None | Geçerli değil. | 
| Azure Blob depolama | Evet | Kullanım {date} ve {time} belirteçleri, olay alanlarındaki yol deseni. Gibi YYYY/AA/GG, GG/AA/YYYY-AA-GG-YYYY tarih biçimini seçin. SS saat biçimi için kullanılır. Bir parçası olarak [Önizleme](https://aka.ms/ASAPreview), blob çıkış, bir tek özel olay özniteliği {fieldname} bölümlenmiş olması. | Giriş için bölümleme izleyen [tam olarak paralelleştirilebilir sorguları](stream-analytics-scale-jobs.md). | 
| Azure Olay Hub'ı | Evet | Evet | Bölüm hizalama bağlı olarak değişir.</br> Olay hub'ı bölüm sayısı, Event Hub'ı bölüm anahtarı eşit (Yukarı Akış önceki) sorgu adım ile yazıcılarının sayısı hizalanır aynıdır çıkış çıkış. Her yazıcı EventHub'ın kullandığı [EventHubSender sınıfı](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) olayları belirli bir bölüme göndermek için. </br> Çıktı olay hub'ı bölüm anahtarı (Yukarı Akış önceki) sorgu adım ile yazıcılarının sayısı hizalı değil olduğunda, önceki adımda bölüm sayısı ile aynı. Her yazıcı EventHubClient kullanan [SendBatchAsync sınıfı](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) tüm çıktı bölümleri olayları göndermek için. |
| Power BI | Hayır | None | Geçerli değil. | 
| Azure Tablo depolama | Evet | Herhangi bir çıktı sütun.  | Giriş için bölümleme izleyen [tam olarak sorguları paralel birkaç ölçeklendirin](stream-analytics-scale-jobs.md). | 
| Azure hizmet veri yolu konusu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.| Çıktı konuda bölüm sayısı ile aynıdır.  |
| Azure hizmet veri yolu kuyruğu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.| Çıkış sırası bölüm sayısı ile aynıdır. |
| Azure Cosmos DB | Evet | {Partition} belirteci koleksiyon adı deseni kullanın. {partition} değer sorgusunda PARTITION BY yan tümcesi temel alır. | Giriş için bölümleme izleyen [tam olarak sorguları paralel birkaç ölçeklendirin](stream-analytics-scale-jobs.md). |
| Azure İşlevleri | Hayır | None | Geçerli değil. | 

## <a name="output-batch-size"></a>Toplu iş boyutu
Azure Stream Analytics değişken boyutu toplu olayları işlemek ve çıktıları yazmak için kullanır. Stream Analytics altyapısı genellikle bir ileti aynı anda yazmaz emin olun ve verimlilik için toplu kullanır. Gelen ve giden olayları oranı olduğunda yüksek büyük toplu kullanır. Çıkış hızı düşük olduğunda gecikme süresi en aza indirmek için daha küçük toplu kullanır. 

Aşağıdaki tabloda bazı toplu işleme çıktısını almak için ilgili önemli noktalar açıklanır:

| Çıkış türü | En büyük mesaj boyutu | Toplu iş boyutu en iyi duruma getirme |
| :--- | :--- | :--- | 
| Azure Data Lake Store | Bkz: [Data Lake Storage sınırlar](../azure-subscription-service-limits.md#data-lake-store-limits) | Yazma işlemi başına en fazla 4 MB |
| Azure SQL Database | Tek toplu başına 10.000 en fazla satır Ekle</br>Tek toplu ekleme başına 100 min satır </br>Ayrıca bkz. [Azure SQL sınırlar](../sql-database/sql-database-resource-limits.md) |  Her toplu başlangıçta en büyük toplu iş boyutu ile eklenen toplu olduğu ve toplu yarısı (kadar Min toplu iş boyutu) SQL yeniden denenebilir hatayla göre Böl. |
| Azure Blob depolama | Bkz: [Azure depolama sınırları](../azure-subscription-service-limits.md#storage-limits) | En fazla Blob blok boyutu 4 MB'tır</br>50000 maksimum Blob bock sayısı: |
| Azure Olay Hub'ı   | İleti başına 256 KB </br>Ayrıca bkz. [olay hub'ları sınırlar](../event-hubs/event-hubs-quotas.md) |    Giriş Çıkış bölümleme Hizala değil, her olay ayrı ayrı bir EventData paketlenmiş ve toplu (Premium SKU 1 MB) maksimum ileti boyutu kadar gönderilir. </br></br>  Giriş-Çıkış bölümleme hizalanır, birden çok olay tek bir EventData maksimum ileti boyutu kadar içine paketlenmiş ve gönderilir.    |
| Power BI | Bkz: [Power BI Rest API sınırlar](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure Tablo depolama | Bkz: [Azure depolama sınırları](../azure-subscription-service-limits.md#storage-limits) | Varsayılan tek işlem başına 100 varlık olduğu ve gerektiğinde daha küçük bir değer için yapılandırılabilir. |
| Azure Service Bus kuyruğu   | İleti başına 256 KB</br> Ayrıca bkz. [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek olay |
| Azure Service Bus konu | İleti başına 256 KB</br> Ayrıca bkz. [Service Bus sınırlar](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek olay |
| Azure Cosmos DB   | Bkz: [Azure Cosmos DB sınırlar](../azure-subscription-service-limits.md#azure-cosmos-db-limits) | Toplu iş boyutu ve sıklığı ayarlanmış dinamik olarak bağlı CosmosDB yanıtları yazma. </br> Stream analytics'ten önceden belirlenmiş sınırlama yoktur. |
| Azure İşlevleri   | | Varsayılan toplu iş boyutu 246 KB'tır. </br> Toplu iş başına varsayılan olay sayısı 100'dür. </br> Toplu iş boyutu yapılandırılabilir bir değerdir ve artırılabilir veya azaltılabilir Stream Analytics [çıktı seçenekleri](#azure-functions). 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
