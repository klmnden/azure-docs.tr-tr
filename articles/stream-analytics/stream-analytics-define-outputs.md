---
title: Azure akış analizi işi çıkışlarından türleri
description: Stream Analytics veri çıkış seçenekleri analiz sonuçları için Power BI dahil olmak üzere hedefleme hakkında bilgi edinin.
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/16/2018
ms.openlocfilehash: 30fa7e081c24339b7fa9f572d9feb25a0f920a86
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="stream-analytics-outputs-options-for-storage-and-analysis"></a>Stream Analytics çıkışları: depolama ve analiz için Seçenekler
Akış analizi işi yazarken sonuç verileri nasıl göz önünde bulundurun. Stream Analytics işi sonuçlarını nasıl görüntüleyebileceğiniz ve burada depolayabilir miyim?

Uygulama düzenleri çeşitli etkinleştirmek için Azure akış analizi çıkış depolamak ve çözümleme sonuçlarını görüntülemek için farklı seçenekler vardır. Bu, iş çıkışı görüntülemek kolaylaştırır ve veri ambarı ve diğer amaçlar için kullanım ve iş çıktısı depolanmasını esneklik sağlar. İşte yapılandırılmış herhangi bir çıktı iş başlatıldı ve akan olayları başlatmak için önce mevcut olması gerekir. Örneğin, çıkış olarak Blob Depolama Birimi kullanıyorsanız, işin bir depolama hesabı otomatik olarak oluşturmaz. Akış analizi işi başlatılmadan önce bir depolama hesabı oluşturun.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics destekler [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Azure Data Lake Store, büyük veri analitik iş yükleri için kuruluş çapında hiper ölçekli bir depodur. Data Lake Store herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için verilerin depolamanıza olanak sağlar. Ayrıca, Stream Analytics Data Lake Store erişmek için yetkili gerekir.

### <a name="authorize-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı yetki

1. Data Lake Storage, Azure portalında bir çıkış olarak seçildiğinde, mevcut bir Data Lake Store bağlantı yetkilendirmek istenir.  

   ![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

2. Data Lake Store'a erişim zaten varsa, "Şimdi Yetkilendir" tıklayın ve bir sayfa "Yeniden yönlendirmek için yetkilendirme" belirten yukarı açılır. Yetkilendirme başarılı olduktan sonra Data Lake Store çıkış yapılandırmanıza izin verir sayfayla sunulur.  

3. Kimliği doğrulanmış Data Lake Store hesabına sahip olduğunda, Data Lake Store çıktı için özelliklerini yapılandırabilirsiniz. Aşağıdaki tablo özellik adları ve Data Lake Store çıkış yapılandırmak için açıklamalarına listesidir.

   ![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

<table>
<tbody>
<tr>
<td><B>ÖZELLİK ADI</B></td>
<td><B>AÇIKLAMA</B></td>
</tr>
<tr>
<td>Çıkış Diğer Adı</td>
<td>Bu Data Lake Store sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı.</td>
</tr>
<tr>
<td>Hesap Adı</td>
<td>Burada, Çıkış göndermeyi Data Lake Storage hesabının adı. Aboneliğinizdeki kullanılabilir Data Lake Store hesapları aşağı açılan listesi ile sunulur.</td>
</tr>
<tr>
<td>Yol önek deseni</td>
<td>Belirtilen veri Gölü deposu hesabı içinde dosyalarınızı yazmak için kullanılan dosya yolu. {Date} bir veya daha fazla örneğini belirtmeyi ve {değişkenleri time}.<BR> Örnek 1: klasör1/logs / {date} / {time}<BR>Örnek 2: klasör1/logs / {date}<BR>Ayrıca, yeni bir dosya oluşturulduğu durumlar şunlardır:<BR>1. Çıktı şemada değiştirme <BR>2. Harici veya dahili bir işi yeniden başlatın.<BR><BR>Ayrıca, dosya yolu deseni bitmiyorsa içermiyor "/" dosya yolu son desende filename önek olarak kabul edilir varsa,.<BR></td>
</tr>
<tr>
<td>Tarih biçimi [<I>isteğe bağlı</I>]</td>
<td>Bir tarih belirteci önek yolunda kullanılırsa, dosyalarınızı organize edilmiştir tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG</td>
</tr>
<tr>
<td>Saat biçimi [<I>isteğe bağlı</I>]</td>
<td>Zaman belirteci önek yolunda kullanılırsa, dosyalarınızı düzenlenmiş zaman biçimini belirtin. Şu anda desteklenen tek değer HH ' dir.</td>
</tr>
<tr>
<td>Olay Serileştirme Biçimi</td>
<td>Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.</td>
</tr>
<tr>
<td>Sınırlayıcı</td>
<td>Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Desteklenen değerler: virgül, noktalı virgül, boşluk, sekme ve, dikey çubuk.</td>
</tr>
<tr>
<td>Biçimlendir</td>
<td>Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi yenileyin
Data Lake Store hesabınızın parolasını işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse sağlamalarını gerekir. Sağlamalarını yok, işinizin sonuçları çıktı değil ve yeniden yetkilendirme gereksinimini belirten bir hata işlemi günlüklerine kaydedilir. Şu anda, burada kimlik doğrulama belirteci 90 günde Data Lake Store çıktıyla tüm işleri el ile yenilenmesi gerekiyor bir sınırlama yoktur. 

Yetkilendirme, yenilemek için **durdurmak** işinizi > Data Lake Store çıkış gidin > tıklatın **yetkilendirmeyi yenileyin** bağlantı ve bir sayfa kısa bir süre için pop "Yeniden yönlendirmek için yetkilendirme.." belirten ayarlama. Sayfa otomatik olarak kapatılacak ve başarılı olursa, "Yetkilendirme başarıyla yenilendi" gösterir. Ardından tıklamanız gerekir **kaydetmek** sayfanın sonundaki ve işinizi gelen yeniden başlatarak geçebilirsiniz **son durdurulma zamanı** veri kaybını önlemek için.

![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Database
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çıkış olarak kendiliğinden ilişkisel veriler için veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar için kullanılabilir. Akış analizi işleri, bir Azure SQL veritabanında var olan bir tabloya yazma.  Tablo şemasını alanları ve işinizi çıktısını olan türlerinin tam olarak eşleşmelidir. Bir [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) SQL veritabanı output seçeneği de aracılığıyla bir çıktı olarak da belirtilebilir. Aşağıdaki tablo özellik adları ve SQL veritabanı çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu veritabanı için sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Database |Burada, Çıkış göndermeyi veritabanının adı |
| Sunucu Adı |SQL veritabanı sunucusu adı |
| Kullanıcı adı |Veritabanına yazma erişimi olan kullanıcı adı |
| Parola |Veritabanına bağlanmak için parola |
| Tablo |Çıktı yazıldığı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır ve bu tablonun şeması alanları ve, iş çıkışı tarafından oluşturulan türlerinin sayısı tam olarak eşleşmelidir. |

> [!NOTE]
> Şu anda Azure SQL veritabanı teklifi Stream Analytics işi çıktısında desteklenir. Ancak, bağlı olan bir veritabanı SQL Server çalıştıran bir Azure sanal makine desteklenmiyor. Bu, sonraki sürüm değişikliklerine tabidir.
> 
> 

## <a name="blob-storage"></a>Blob depolama
BLOB storage bulutta büyük miktarda yapılandırılmamış veriyi depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar.  Azure Blob Depolama ve kullanım giriş için belgelerine bakın [BLOB'ları kullanma](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

Özellik adlarının ve kendi açıklama blob çıktı oluşturmak için aşağıdaki tabloda listelenmiştir.

<table>
<tbody>
<tr>
<td>ÖZELLİK ADI</td>
<td>AÇIKLAMA</td>
</tr>
<tr>
<td>Çıkış Diğer Adı</td>
<td>Bu blob depolama sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı.</td>
</tr>
<tr>
<td>Depolama Hesabı</td>
<td>Burada, Çıkış göndermeyi depolama hesabı adı.</td>
</tr>
<tr>
<td>Depolama Hesabı Anahtarı</td>
<td>Depolama hesabıyla ilişkili gizli anahtar.</td>
</tr>
<tr>
<td>Depolama kapsayıcısı</td>
<td>Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir.</td>
</tr>
<tr>
<td>Yol önek deseni [isteğe bağlı]</td>
<td>Belirtilen kapsayıcı içinde bloblarınızın yazmak için kullanılan dosya yolu deseni. <BR> Yol deseninde BLOB'lar yazılır sıklığını belirtmek için şu 2 değişkenin bir veya daha fazla örneğinin kullanılacağını seçebilirsiniz: <BR> {date} {time} <BR> Örnek 1: cluster1/logs / {date} / {time} <BR> Örnek 2: cluster1/logs / {date} <BR> <BR> Dosya adlandırma aşağıdaki kuralını aşağıdaki gibidir: <BR> {Yol öneki Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Örnek çıktı dosyaları: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Ayrıca, yeni bir dosya oluşturulduğu durumlar şunlardır: <BR> 1. Geçerli dosya blokları (şu anda 50.000) izin verilen maksimum sayısını aşıyor <BR> 2. Çıktı şemada değiştirme <BR> 3. Harici veya dahili bir işi yeniden başlatın.  </td>
</tr>
<tr>
<td>[İsteğe bağlı] tarih biçimi</td>
<td>Bir tarih belirteci önek yolunda kullanılırsa, dosyalarınızı organize edilmiştir tarih biçimi seçebilirsiniz. Örnek: YYYY/AA/GG</td>
</tr>
<tr>
<td>[İsteğe bağlı] saat biçimi</td>
<td>Zaman belirteci önek yolunda kullanılırsa, dosyalarınızı düzenlenmiş zaman biçimini belirtin. Şu anda desteklenen tek değer HH ' dir.</td>
</tr>
<tr>
<td>Olay Serileştirme Biçimi</td>
<td>Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.</td>
</tr>
<tr>
<td>Sınırlayıcı</td>
<td>Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir.</td>
</tr>
<tr>
<td>Biçimlendir</td>
<td>Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış.</td>
</tr>
</tbody>
</table>

BLOB Depolama çıkış olarak kullanırken, aşağıdaki durumlarda blob yeni bir dosya oluşturulur:

* Dosya izin verilen blokları (blok sayısı izin verilen maksimum izin verilen en fazla blob boyutu erişmeden ulaşılamayabilir unutmayın. üst sınırını aşarsa Örneğin, çıkış oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu. Çıkış oranı düşükse, her bloğu daha az veri vardır ve dosya boyutu küçüktür.)  
* Varsa çıktıda bir şema değişikliği vardır ve çıkış biçimi sabit şemasına (CSV ve Avro) gerektirir.  
* Bir iş yeniden başlatılırsa herhangi bir harici veya dahili bir işi yeniden.  
* Sorgu tam olarak bölümlenmiş, yeni dosya her çıktı bölümü için oluşturulur.  
* Bir dosya veya bir kapsayıcı depolama hesabının kullanıcı tarafından silindi.  
* Çıkış yolu önek deseni kullanarak bölümlenmiş saat ise, sorgu sonraki saat taşındığında yeni blob kullanılır.

## <a name="event-hub"></a>Olay Hub'ı
[Olay hub'ları](https://azure.microsoft.com/services/event-hubs/) yüksek düzeyde ölçeklenebilir yayımlama-abone olma olay yutucu değil. Saniye başına milyonlarca olayı toplayabilirsiniz. Akış analizi işi çıktısını başka bir iş akışında girişi olduğunda bir olay hub'ı çıktı olarak kullanılır.

Olay hub'ı veri akışlarını çıkış olarak yapılandırmak için gereken birkaç parametre vardır.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu olay Hub'ına sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Service Bus Ad Alanı |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş |
| Olay Hub'ı |Olay hub'ı çıkışı adı |
| Olay Hub'ı İlke Adı |Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesi ayarlayın ve erişim anahtarları izinleri olan bir ada sahip |
| Olay Hub'ı İlke Anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Bölüm anahtarı sütunu [isteğe bağlı] |Bu sütun, olay hub'ı çıkışı bölüm anahtarı içerir. |
| Olay Serileştirme Biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 desteklenen tek kodlama biçimi şu anda içindir. |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |
| Biçimlendir |Yalnızca JSON serileştirmesi için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapalı. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış. |

## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/) Stream Analytics işi için bir çıktı olarak çözümleme sonuçları için bir zengin görselleştirme deneyimi sağlamak için kullanılabilir. Bu özellik, işletimsel panoları, rapor oluşturma ve raporlama güdümlü ölçüm için kullanılabilir.

### <a name="authorize-a-power-bi-account"></a>Power BI hesabınız yetkilendirmek
1. Power BI, Azure portalında bir çıkış olarak seçildiğinde, yeni bir Power BI hesabı oluşturmak için veya varolan bir Power BI kullanıcı yetkilendirmek için istenir.  
   
   ![Power BI kullanıcı yetkilendirme](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  
2. Verme henüz bir tane ve şimdi Yetkilendir'i tıklatın, yeni bir hesap oluşturun.  Aşağıdaki gibi bir ekranı sunulur.  
   
   ![Azure hesabı Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  
3. Bu adımda, Power BI çıkışı yetkisi vermek için iş veya Okul hesabı sağlayın. Zaten Power BI için kayıtlı değilsiniz, oturum şimdi seçin. Power BI için kullandığınız iş veya Okul hesabı ile oturum açmış olduğunuz Azure aboneliği hesabından farklı olabilir.

### <a name="configure-the-power-bi-output-properties"></a>Power BI çıktı özelliklerini yapılandırın
Kimliği doğrulanmış Power BI hesabınızı edindikten sonra Power BI çıktı için özelliklerini yapılandırabilirsiniz. Aşağıdaki tablo, özellik adlarının listesi ve bunların açıklaması, Power BI çıkışı yapılandırmak için ' dir.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu Powerbı çıkış sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Grup çalışma |Diğer Power BI kullanıcılarıyla veri paylaşımını etkinleştirmek için Power BI hesabınızı içinde grupları seçin veya bir gruba yazmak istemiyorsanız "Çalışma Alanım" seçin.  Varolan bir grup güncelleştirme, Power BI kimlik doğrulamayı yenilemesini gerektirir. |
| Veri kümesi adı |Bir veri kümesi adı sağlayın, kullanmak Power BI çıktısı için istenen |
| Tablo Adı |Power BI çıkış veri kümesi altında bir tablo adı sağlayın. Şu anda Power BI çıkışı akış analizi işleri yalnızca bir tablo bir veri kümesinde olabilir |

Power BI çıkışı ve Pano yapılandırma kılavuz için lütfen bkz. [Azure akış analizi & Power BI](stream-analytics-power-bi-dashboard.md) makalesi.

> [!NOTE]
> Açıkça veri kümesi ve tablo Power BI Panonuzda oluşturmayın. Veri kümesi ve tablo otomatik olarak doldurulur iş başlatıldığında ve iş pompa çıkış Power BI'da oturum başlatır. Sorgu iş herhangi bir sonuç, veri kümesi oluşturmaz ve tablo oluşturulamayacağını unutmayın. Power BI veri kümesi ve bu Stream Analytics işinde sağlanan aynı ada sahip bir tablo zaten sahipse, var olan verileri yazılır unutmayın.
> 
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
Kayan nokta | Çift
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
Power BI hesabınızın parolasını işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse sağlamalarını gerekir. Çok faktörlü kimlik doğrulama (MFA), Azure Active Directory (AAD) Kiracı yapılandırılmışsa, ayrıca Power BI yetkilendirme iki haftada yenilemeniz gerekir. Bu sorun belirtisi hiçbir iş çıktısı ve "kimlik doğrulama kullanıcı hatayla" işlem günlükleri şöyledir:

  ![Power BI yenileme belirteci hata](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Bu sorunu çözmek için çalışan bir işi durdurmak ve Power BI çıktısına gidin.  "Yenileme yetkilendirme" bağlantısına tıklayın ve son durduruldu veri kaybını önlemek için zaman işi yeniden başlatın.

  ![Power BI yetkilendirme yeniler](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tablo Depolama
[Azure Table storage](../storage/common/storage-introduction.md) yüksek oranda kullanılabilir, yüksek düzeyde ölçeklenebilir depolama sunar, böylece bir uygulamayı otomatik olarak kullanıcı taleplerini karşılayacak şekilde ölçeklendirebilirsiniz. Tablo depolama hangisinin şemanın daha az kısıtlamalar ile yapılandırılmış veri için yararlanabilirsiniz Microsoft'un NoSQL anahtar/öznitelik deposudur. Azure Table storage kalıcılığını ve verimli alma verilerini depolamak için kullanılabilir.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu tablo depolama sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Depolama Hesabı |Burada, Çıkış göndermeyi depolama hesabı adı. |
| Depolama Hesabı Anahtarı |Depolama hesabıyla ilişkili erişim anahtarı. |
| Tablo Adı |Tablonun adı. Henüz yoksa tablo oluşturulan. |
| Bölüm Anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, bir varlığın birincil anahtarının ilk bölümü forms belirli bir tablodaki bölümde benzersiz tanımlayıcısıdır. En çok 1 KB olabilen bir dize değeridir. |
| Satır Anahtarı |Satır anahtarını içeren çıkış sütununun adı. Satır anahtarını belli bir bölüm içinde bir varlık için benzersiz bir tanımlayıcı değil. Bir varlığın birincil anahtarının ikinci bölümü oluşturur. Satır anahtarı boyutu en çok 1 KB olabilen bir dize değeridir. |
| Toplu İşlem Boyutu |Bir toplu işlemi için kayıt sayısı. Genellikle varsayılan çoğu işleri için yeterliyse başvurmak [tablo toplu işlem spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) bu ayarını değiştirme hakkında daha fazla ayrıntı için. |
 
## <a name="service-bus-queues"></a>Service Bus Kuyrukları
[Hizmet veri yolu kuyrukları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir ilk olarak, bir veya birden çok rakip tüketiciye ilk çıkar (FIFO) ileti teslimi sunar. Genellikle, ileti aldı ve hangi kuyruğa eklendikleri ve her ileti alındı ve tek bir ileti tüketicisi tarafından alınıp zamana bağlı düzende alıcılar tarafından işlenen beklenir.

Aşağıdaki tablo özellik adları ve sıra çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu hizmet veri yolu kuyruğu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Service Bus Ad Alanı |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. |
| Kuyruk Adı |Hizmet veri yolu kuyruğu adı. |
| Kuyruk İlkesi Adı |Bir kuyruk oluşturduğunuzda kuyruk yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir adı, ayarlayın ve erişim anahtarları izinleri vardır. |
| Kuyruk İlkesi Anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olay Serileştirme Biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 desteklenen tek kodlama biçimi şu anda içindir. |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |
| Biçimlendir |Yalnızca JSON türü için geçerlidir. Yeni bir çizgiyle ayrılmış her bir JSON nesnesi sağlayarak çıktı biçimlendirilir ayrılmış satırını belirtir. Çıkış JSON nesnelerinin bir dizisi biçimlendirilir dizisini belirtir. |

Bölüm sayısı [Service Bus SKU ve boyutuna göre](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.

## <a name="service-bus-topics"></a>Service Bus Konuları
Hizmet veri yolu kuyrukları gönderenden alıcıya, bire bir iletişim yöntemi sunarken [Service Bus konu başlıklarına](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir-çok form iletişim sağlar.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış Diğer Adı |Bu hizmet veri yolu konusu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay adı. |
| Service Bus Ad Alanı |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş |
| Konu Adı |Konular, olay hub'ları ve kuyrukları benzer Mesajlaşma varlıklarıdır. Bir dizi farklı cihaz ve Hizmetleri hizmetten olay akışları toplayacak şekilde tasarlanmışlardır. Bir konu oluşturduğunuzda, ayrıca belirli bir ad verilir. Bir abonelik yapılandırılmadığı sürece bir konu başlığına gönderilen iletileri kullanılabilir değilse, böylece olun konu altında bir veya daha fazla abonelik |
| Konu İlkesi Adı |Bir konu oluşturduğunuzda konu yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesi ayarlayın ve erişim anahtarları izinleri bir ada sahip |
| Konu İlkesi Anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olay Serileştirme Biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
 | Encoding |Bir kodlama, CSV veya JSON biçimi kullanıyorsanız, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8 durumda |
| Sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekmesini ve dikey çubuk değerleri desteklenir. |

Bölüm sayısı [Service Bus SKU ve boyutuna göre](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) hizmet teklifleri sınırsız esnek ölçek dünya zengin sorgu ve şema belirsiz veri modelleri otomatik dizin oluşturma işlemi geçici düşük gecikme süresi garanti ve endüstri lideri Genel dağıtılmış, birden çok model bir veri tabanıdır kapsamlı SLA. Stream Analytics Cosmos DB koleksiyonu seçenekleri hakkında bilgi edinmek için bkz [Stream Analytics çıkış olarak Cosmos DB ile](stream-analytics-documentdb-output.md) makalesi.

> [!Note]
> Şu anda Azure akış analizi yalnızca CosmosDB kullanarak bağlantı destekler **SQL API**.
> Diğer Azure Cosmos DB API'leri henüz desteklenmiyor. Noktası Azure akış analizi Azure Cosmos DB hesaplarına diğer API'leri ile oluşturduysanız, verilerin düzgün depolanabilir değil. 

Aşağıdaki tabloda Azure Cosmos DB çıktı oluşturmak için özellikleri açıklanmaktadır.
| Özellik Adı | Açıklama |
| --- | --- |
| Çıkış diğer adı | Bu akış analizi sorgu çıktı başvurmak için bir diğer ad. |
| Havuz | Cosmos DB |
| İçeri aktarma seçeneği | "Cosmos DB aboneliğinizden seçmek için" ya da seçin, veya "sağla Cosmos DB ayarlarını el ile".
| Hesap kimliği | Adı veya bitiş noktası Cosmos DB hesabının URI'si. |
| Hesap anahtarı | Cosmos DB hesabı için paylaşılan erişim anahtarı. |
| Database | Cosmos DB veritabanı adı. |
| Koleksiyon adı deseni | Koleksiyon adını veya kullanılacak koleksiyonlar için kendi desen. <br/>Koleksiyon adı biçimi, burada bölümlerin 0'dan başlar isteğe bağlı {partition} belirteci kullanılarak oluşturulabilir. İki örnek:  <br/>1. _MyCollection_ – "MyCollection" adlı bir koleksiyon bulunmalıdır.  <br/>2. _MyCollection {partition}_ – bölümleme sütunu bağlı. <br/>Bölümleme sütunu koleksiyonların mevcut olması – "MyCollection0", "MyCollection1", "MyCollection2" ve benzeri. |
| Bölüm Anahtarı | İsteğe bağlı. Bu, yalnızca, koleksiyon adı deseni {partition} belirteci kullanıyorsanız gereklidir.<br/> Bölüm anahtarı çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adıdır.<br/> Tek koleksiyon çıktı için herhangi bir rastgele çıkış sütunu PartitionID örnek için kullanılabilir. |
| Belge Kimliği |İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayalı birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.  

## <a name="azure-functions"></a>Azure İşlevleri
Azure İşlevleri, açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmanıza olanak sağlayan bir sunucusuz işlem hizmetidir. Azure veya üçüncü taraf hizmetleri gerçekleşen olaylar tarafından tetiklenen kodları uygulama olanak sağlar.  Tetikleyiciler için yanıt Özelliği Azure işlevlerinin bir Azure akış analizi için doğal bir çıktı kolaylaştırır. Bu çıkış bağdaştırıcısı Stream Analytics Azure işlevleri bağlanmak ve yanıt olayları çeşitli olarak bir komut dosyası veya kod parçası çalıştırmak kullanıcıların sağlar.

Azure Stream Analytics Azure işlevleri HTTP Tetikleyicileri çağırır. Aşağıdaki yapılandırılabilir özelliklere sahip yeni Azure işlevi çıkış bağdaştırıcısı kullanılabilir:

| Özellik Adı | Açıklama |
| --- | --- |
| İşlev Uygulaması |Azure işlevleri uygulamanızın adı |
| İşlev |Azure işlevleri uygulamanızda işlevin adı |
| En büyük toplu iş boyutu |Bu özellik, Azure işlevinizi gönderilen her bir çıkış toplu iş boyutu üst sınırı ayarlamak için kullanılabilir. Varsayılan olarak, bu değer 256 KB'dir |
| En büyük toplu iş sayısı  |Adından da anlaşılacağı gibi bu özellik için Azure işlevleri gönderilir her yığında olayların maksimum sayısını belirtmenize olanak sağlar. Varsayılan Maksimum toplu iş sayısı değeri 100'dür |
| Anahtar |Başka bir abonelik Azure işlevinden kullanmak istiyorsanız, işlevinizi erişmeye yönelik anahtar sağlayarak bunu yapabilirsiniz |

Azure Stream Analytics Azure işlevinden 413 (http istek varlığı çok büyük) özel durum aldığında, bunu Azure işlevleri gönderir toplu boyutunu azaltır unutmayın. Azure işlevi kodunuzda bu özel durumun Azure akış analizi büyük boyutlu toplu göndermez emin olmak için kullanın. Ayrıca, işlevde kullanılan Maksimum toplu iş sayısı ve boyutu değerleri Stream Analytics portalda girdiğiniz değerleri ile tutarlı olduğundan emin olun. 

Ayrıca, bir durumda bir zaman penceresi için giriş olay olduğunda, çıktı oluşturulur. Sonuç olarak, computeResult işlev çağrılmaz. Bu davranış, yerleşik pencereli toplama işlevleri tutarlıdır.

## <a name="partitioning"></a>Bölümleme

Bölüm destek ve her bir çıkış türü için çıktı yazıcılarının sayısı aşağıdaki tabloda özetlenmiştir:

| Çıkış türü | Bölümleme desteği | Bölüm anahtarı  | Çıktı yazıcılarının sayısı | 
| --- | --- | --- | --- |
| Azure Data Lake Store | Evet | Kullanım {date} ve {time} belirteçleri yol önek deseni. Gibi YYYY/AA/GG, GG/AA/YYYY-AA-GG-YYYY tarih biçimini seçin. SS saat biçimi için kullanılır. | Giriş ile aynıdır. | 
| Azure SQL Database | Hayır | None | Geçerli değil. | 
| Azure Blob depolama | Evet | Kullanım {date} ve {time} belirteçleri yol deseni. Gibi YYYY/AA/GG, GG/AA/YYYY-AA-GG-YYYY tarih biçimini seçin. SS saat biçimi için kullanılır. | Giriş ile aynıdır. | 
| Azure Event hub'ı | Evet | Evet | Aynı olay hub'ı bölümleri çıktı. |
| Power BI | Hayır | None | Geçerli değil. | 
| Azure Tablo depolama | Evet | Herhangi bir çıktı sütun.  | Giriş veya önceki adımı ile aynıdır. | 
| Azure hizmet veri yolu konusu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.| Çıktı ile aynı.  |
| Azure hizmet veri yolu kuyruğu | Evet | Otomatik olarak seçilir. Bölüm sayısı dayanır [Service Bus SKU ve boyutu](../service-bus-messaging/service-bus-partitioning.md). Bölüm anahtarı her bölüm için benzersiz bir tamsayı değil.| Çıktı ile aynı. |
| Azure Cosmos DB | Evet | {Partition} belirteci koleksiyon adı deseni kullanın. {partition} değer sorgusunda PARTITION BY yan tümcesi temel alır. | Giriş ile aynıdır. |
| Azure İşlevleri | Hayır | None | Geçerli değil. | 


## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Nesnelerin İnterneti'nden gelen verilerdeki akış analizlerine yönelik bir yönetilen hizmet olan Stream Analytics'e giriş yaptınız. Bu hizmet hakkında daha fazla bilgi edinmek için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
