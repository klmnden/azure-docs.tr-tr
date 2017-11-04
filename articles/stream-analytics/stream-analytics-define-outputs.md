---
title: "Stream Analytics çıkışları: depolama, çözümleme için Seçenekler | Microsoft Docs"
description: "Stream Analytics veri çıkış seçenekleri analiz sonuçları için Power BI dahil olmak üzere hedefleme hakkında bilgi edinin."
keywords: "veri dönüştürme, analiz sonuçları, veri depolama seçenekleri"
services: stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: ba6697ac-e90f-4be3-bafd-5cfcf4bd8f1f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 33d0b9aa37cc92dda27f1cf21f1d393b42b8c09b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Stream Analytics çıkışları: depolama, çözümleme için seçenekleri
Akış analizi işi yazarken sonuç verileri nasıl kullanılacağını düşünün. Stream Analytics işi sonuçlarını nasıl görüntüler ve burada depolayacak mı?

Uygulama düzenleri çeşitli etkinleştirmek için Azure akış analizi çıkış depolamak ve çözümleme sonuçlarını görüntülemek için farklı seçenekler vardır. Bu, iş çıkışı görüntülemek kolaylaştırır ve veri ambarı ve diğer amaçlar için kullanım ve iş çıktısı depolanmasını esneklik sağlar. İşte yapılandırılmış herhangi bir çıktı iş başlatıldı ve akan olayları başlatmak için önce mevcut olması gerekir. Blob Depolama çıkış olarak kullanırsanız, örneğin, iş bir depolama hesabı otomatik olarak oluşturmaz. ASA işi başlatılmadan önce kullanıcı tarafından oluşturulmuş gerekir.

## <a name="azure-data-lake-store"></a>Azure Data Lake Store
Stream Analytics destekler [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/). Bu depolama herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için verilerin depolamanıza olanak sağlar. Ayrıca, Stream Analytics Data Lake Store erişmek için yetkili gerekir. Yetkilendirme ve (gerekirse) için Data Lake Store kaydolma hakkında ayrıntılı olarak ele alınmıştır [Data Lake çıktı makale](stream-analytics-data-lake-output.md).

### <a name="authorize-an-azure-data-lake-store"></a>Bir Azure Data Lake Store yetkilendirmek
Data Lake Storage, Azure portalında bir çıkış olarak seçildiğinde, mevcut bir Data Lake Store bağlantı yetkilendirmek için istenir.  

![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Ardından Data Lake Store çıktı özelliklerini aşağıda görüldüğü gibi doldurun:

![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Aşağıdaki tabloda özellik adları ve Data Lake Store çıktı oluşturmak için gereken bunların açıklaması listelenmektedir.

<table>
<tbody>
<tr>
<td><B>ÖZELLİK ADI</B></td>
<td><B>AÇIKLAMA</B></td>
</tr>
<tr>
<td>Çıkış diğer adları</td>
<td>Bu, sorgu çıktısı bu Data Lake Store'a doğrudan sorgularda kullanılan kolay bir addır.</td>
</tr>
<tr>
<td>Hesap Adı</td>
<td>Burada, Çıkış göndermeyi Data Lake Storage hesabının adı. Bir aşağı açılan listeden portalda oturum açmış kullanıcının erişimi olan Data Lake Store hesapları ile sunulur.</td>
</tr>
<tr>
<td>Yol önek deseni</td>
<td>Dosya adlandırma aşağıdaki kuralını izler: <BR>{Yol öneki Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR>Örnek çıktı dosyaları:<BR>Myoutput/20170901/00/45434_gguid_1.csv <BR>Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR>Ayrıca, yeni bir dosya oluşturulduğu durumlar şunlardır:<BR>1.Çıktı şemada değiştirme <BR>2.Harici veya dahili bir işi yeniden başlatın.<BR><BR>Ayrıca, dosya yolu deseni bitmiyorsa içermiyor "/" dosya yolu son desende filename önek olarak kabul edilir varsa,.<BR><BR>Örnek:<BR>Yol deseni için: klasör1/günlükleri/ss, oluşturulan dosya gibi görünebilir: folder1/logs/02_134343_gguid_1.csv</td>
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
<td>Olayı seri hale getirme biçimi</td>
<td>Çıkış verileri seri hale getirme biçimi. JSON, CSV ve Avro desteklenir.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Bir kodlama, CSV veya JSON biçiminde, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.</td>
</tr>
<tr>
<td>sınırlayıcı</td>
<td>Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir.</td>
</tr>
<tr>
<td>Biçimi</td>
<td>Yalnızca JSON serileştirmesi için geçerlidir. Ayrılmış çizgi çıkış sahip yeni bir çizgiyle ayrılmış her bir JSON nesnesi olarak biçimlendirileceğini belirtir. Dizi çıkışı bir dizi JSON nesnesi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapatılacak. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Data Lake Store yetkilendirmeyi yenileyin
Parolasını işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse Data Lake Store hesabınızı yeniden kimlik doğrulaması gerekir.

![Data Lake Store yetkilendirmek](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  

## <a name="sql-database"></a>SQL Veritabanı
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) çıkış olarak kendiliğinden ilişkisel veriler için veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar için kullanılabilir. Akış analizi işleri, bir Azure SQL veritabanında var olan bir tabloya yazacaksınız.  Tablo şeması alanları ve işinizi çıktısını olan türlerinin tam olarak eşleşmesi gerektiğini unutmayın. Bir [Azure SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) (Bu, bir önizleme özelliği) SQL veritabanı output seçeneği de aracılığıyla bir çıktı olarak da belirtilebilir. Aşağıdaki tablo özellik adları ve SQL veritabanı çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, sorgu çıktısı bu veritabanına doğrudan sorgularda kullanılan kolay bir addır. |
| Database |Burada, Çıkış göndermeyi veritabanının adı |
| Sunucu adı |SQL veritabanı sunucusu adı |
| Kullanıcı adı |Veritabanına yazma erişimi olan kullanıcı adı |
| Parola |Veritabanına bağlanmak için parola |
| Tablo |Çıktı yazılacağı tablo adı. Tablo adı büyük/küçük harfe duyarlıdır ve bu tablonun şeması alanları ve, iş çıkışı tarafından oluşturulan türlerinin sayısı tam olarak eşleşmelidir. |

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
<td>Çıkış diğer adları</td>
<td>Bu, sorgu çıktısı bu blob depolama alanına yönlendirmek için sorguda kullanılan kolay bir addır.</td>
</tr>
<tr>
<td>Depolama Hesabı</td>
<td>Burada, Çıkış göndermeyi depolama hesabı adı.</td>
</tr>
<tr>
<td>Depolama hesabı anahtarı</td>
<td>Depolama hesabıyla ilişkili gizli anahtar.</td>
</tr>
<tr>
<td>Depolama kapsayıcısı</td>
<td>Kapsayıcılar Microsoft Azure Blob hizmetinde depolanan BLOB'lar için mantıksal bir gruplandırmasını sağlar. Blob hizmeti için bir blob karşıya yüklediğinde, o blob için bir kapsayıcı belirtmeniz gerekir.</td>
</tr>
<tr>
<td>Yol önek deseni [isteğe bağlı]</td>
<td>Belirtilen kapsayıcı içinde bloblarınızın yazmak için kullanılan dosya yolu deseni. <BR> Yol deseninde BLOB'lar yazılır sıklığını belirtmek için şu 2 değişkenin bir veya daha fazla örneğinin kullanılacağını seçebilirsiniz: <BR> {date} {time} <BR> Örnek 1: cluster1/logs / {date} / {time} <BR> Örnek 2: cluster1/logs / {date} <BR> <BR> Dosya adlandırma aşağıdaki kuralını izler: <BR> {Yol öneki Pattern}/schemaHashcode_Guid_Number.extension <BR> <BR> Örnek çıktı dosyaları: <BR> Myoutput/20170901/00/45434_gguid_1.csv <BR> Myoutput/20170901/01/45434_gguid_1.csv <BR> <BR> Ayrıca, yeni bir dosya oluşturulduğu durumlar şunlardır: <BR> 1.Geçerli dosya blokları (şu anda 50.000) izin verilen maksimum sayısını aşıyor <BR> 2.Çıktı şemada değiştirme <BR> 3.Harici veya dahili bir işi yeniden başlatın.  </td>
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
<td>Olayı seri hale getirme biçimi</td>
<td>Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Bir kodlama, CSV veya JSON biçiminde, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8'dir.</td>
</tr>
<tr>
<td>sınırlayıcı</td>
<td>Yalnızca, CSV serileştirme için de geçerlidir. Akış analizi, CSV verileri seri hale getirme için bir dizi ortak sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir.</td>
</tr>
<tr>
<td>Biçimi</td>
<td>Yalnızca JSON serileştirmesi için geçerlidir. Ayrılmış çizgi çıkış sahip yeni bir çizgiyle ayrılmış her bir JSON nesnesi olarak biçimlendirileceğini belirtir. Dizi çıkışı bir dizi JSON nesnesi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapatılacak. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Olay Hub'ı
[Olay hub'ları](https://azure.microsoft.com/services/event-hubs/) yüksek düzeyde ölçeklenebilir yayımlama-abone olma olay yutucu değil. Saniye başına milyonlarca olayı toplayabilirsiniz.  Akış analizi işi çıktısını başka bir iş akışında girişi kullanırken bir olay hub'ı çıktı olarak kullanılır.

Olay hub'ı veri akışlarını çıkış olarak yapılandırmak için gereken birkaç parametre vardır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, sorgu çıktısı bu olay Hub'ına doğrudan sorgularda kullanılan kolay bir addır. |
| Hizmet veri yolu Namespace |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş |
| Olay Hub'ı |Olay hub'ı çıkışı adı |
| Olay hub'ı ilke adı |Paylaşılan Erişim İlkesi olay hub'ı yapılandırma sekmesinde oluşturulabilir. Her paylaşılan erişim ilkesinin bir ad, ayarladığınız izinler ve erişim anahtarları |
| Olay hub'ı İlkesi anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Bölüm anahtarı sütunu [isteğe bağlı] |Bu sütun, olay hub'ı çıkışı bölüm anahtarı içerir. |
| Olayı seri hale getirme biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 desteklenen tek kodlama biçimi şu anda içindir. |
| sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir. |
| Biçimi |Yalnızca JSON serileştirmesi için geçerlidir. Ayrılmış çizgi çıkış sahip yeni bir çizgiyle ayrılmış her bir JSON nesnesi olarak biçimlendirileceğini belirtir. Dizi çıkışı bir dizi JSON nesnesi biçimlendirileceğini belirtir. Yalnızca sonraki zaman penceresi işi durur veya Stream Analytics taşınmıştır, bu diziye kapatılacak. Genel olarak, tercih edilir satırını kullanmak için çıktı dosyası için hala yazıldığı sırada hiçbir özel işlem gerektirmez beri JSON, ayrılmış. |

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

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, bu Powerbı çıkış sorgu çıktısını yönlendirmek için sorguda kullanılan kolay bir addır. |
| Grup çalışma |Diğer Power BI kullanıcılarıyla veri paylaşımını etkinleştirmek için Power BI hesabınızı içinde grupları seçin veya bir gruba yazmak istemiyorsanız "Çalışma Alanım" seçin.  Varolan bir grup güncelleştirme, Power BI kimlik doğrulamayı yenilemesini gerektirir. |
| Veri kümesi adı |Bir veri kümesi adı sağlayın, kullanmak Power BI çıktısı için istenen |
| Tablo adı |Power BI çıkış veri kümesi altında bir tablo adı sağlayın. Şu anda Power BI çıkışı akış analizi işleri yalnızca bir tablo bir veri kümesinde olabilir |

Power BI çıkışı ve Pano yapılandırma kılavuz için lütfen bkz. [Azure akış analizi & Power BI](stream-analytics-power-bi-dashboard.md) makalesi.

> [!NOTE]
> Açıkça veri kümesi ve tablo Power BI Panonuzda oluşturmayın. Veri kümesi ve tablo, otomatik olarak iş başlatıldığında ve iş pompa çıkış Power BI'da oturum başlatır doldurulur. Sorgu iş herhangi bir sonuç oluşturmaz, veri kümesi ve tablo oluşturulmayacak olduğunu unutmayın. Ayrıca Power BI veri kümesi ve bu Stream Analytics işinde sağlanan aynı ada sahip bir tablo zaten sahipse, var olan verilerin üzerine yazılacak unutmayın.
> 
> 

### <a name="schema-creation"></a>Şema oluşturma
Bir zaten yoksa, azure Stream Analytics Power BI veri kümesi ve kullanıcı adına tablo oluşturur. Diğer durumlarda, tablo yeni değerlerle güncelleştirilir. Şu anda, bir veri kümesi içinde yalnızca bir tablo sınırlaması bulunabilir.

### <a name="data-type-conversion-from-asa-to-power-bi"></a>Power BI-ASA veri türü dönüşümü
Çıkış şeması değişirse azure Stream Analytics veri modeli çalışma zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişiklikleri ve eklenmesi veya sütunları kaldırılması tüm izlenir.

Bu tablo veri türü dönüştürmelerini gelen kapsar [Stream Analytics veri türleri](https://msdn.microsoft.com/library/azure/dn835065.aspx) yayımlamasını güç için [varlık veri modeli (EDM) türleri](https://powerbi.microsoft.com/documentation/powerbi-developer-walkthrough-push-data/) POWER BI veri kümesi ve tablo yoksa.


Akış analizi | Power BI
-----|-----|------------
bigint | Int64
nvarchar(max) | Dize
Tarih saat | Tarih saat
Kayan nokta | Çift
Kayıt dizisi | Türü, sabit değer "IRecord" veya "IArray" dize

### <a name="schema-update"></a>Şema güncelleştirmesi
Akış analizi çıktıda olayları ilk dizi göre veri modeli şeması oluşturur. Daha sonra gerekirse, veri modeli şeması özgün şemasına uygun değildir gelen olayları uyum sağlayacak şekilde güncelleştirildi.

`SELECT *` Dinamik şema güncelleştirmesi satırlarda önlemek için sorgu kaçınılmalıdır. Olası performans etkileri yanı sıra indeterminacy sonuçlar için geçen süre içinde de sonuçlanabilir. Power BI Panosu üzerinde gösterilen gereken alanları tam seçilmelidir. Ayrıca, veri değerlerinin seçtiğiniz veri türü ile uyumlu olmalıdır.


Önceki/geçerli | Int64 | Dize | Tarih saat | Çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | Çift
Çift | Çift | Dize | Dize | Çift
Dize | Dize | Dize | Dize |  | Dize | 
Tarih saat | Dize | Dize |  Tarih saat | Dize


### <a name="renew-power-bi-authorization"></a>Power BI yetkilendirmeyi yenileyin
Parolasını işinizi oluşturulmuş veya son kimliği doğrulanmış oluşturulmasından sonra değiştirilmişse Power BI hesabınızı yeniden kimlik doğrulaması gerekir. Çok faktörlü kimlik doğrulama (MFA), Azure Active Directory (AAD) Kiracı yapılandırılmışsa, Power BI yetkilendirme 2 haftada bir yenileme gerekir. Bu sorun belirtisi hiçbir iş çıktısı ve "kimlik doğrulama kullanıcı hatayla" işlem günlükleri şöyledir:

  ![Power BI yenileme belirteci hata](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

Bu sorunu çözmek için çalışan bir işi durdurmak ve Power BI çıktısına gidin.  "Yenileme yetkilendirme" bağlantısına tıklayın ve son durduruldu veri kaybını önlemek için zaman işi yeniden başlatın.

  ![Power BI yenileme yetkilendirme](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Tablo Depolama
[Azure Table storage](../storage/common/storage-introduction.md) yüksek oranda kullanılabilir, yüksek düzeyde ölçeklenebilir depolama sunar, böylece bir uygulamayı otomatik olarak kullanıcı taleplerini karşılayacak şekilde ölçeklendirebilirsiniz. Table storage hangisinin daha az kısıtlamalar şema ile yapılandırılmış veri için yararlanabilirsiniz Microsoft'un NoSQL anahtar/öznitelik deposudur. Azure Table storage kalıcılığını ve verimli alma verilerini depolamak için kullanılabilir.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, sorgu çıktısı bu tablo depolama alanına yönlendirmek için sorguda kullanılan kolay bir addır. |
| Depolama Hesabı |Burada, Çıkış göndermeyi depolama hesabı adı. |
| Depolama hesabı anahtarı |Depolama hesabıyla ilişkili erişim anahtarı. |
| Tablo adı |Tablonun adı. Henüz yoksa tablo oluşturulur. |
| Bölüm anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, bir varlığın birincil anahtarının ilk bölümü forms belirli bir tablodaki bölümde benzersiz tanımlayıcısıdır. En çok 1 KB olabilen bir dize değeridir. |
| Satır anahtarı |Satır anahtarını içeren çıkış sütununun adı. Satır anahtarını belli bir bölüm içinde bir varlık için benzersiz bir tanımlayıcı değil. Bir varlığın birincil anahtarının ikinci bölümü oluşturur. Satır anahtarı boyutu en çok 1 KB olabilen bir dize değeridir. |
| Toplu iş boyutu |Bir toplu işlemi için kayıt sayısı. Genellikle varsayılan çoğu işleri için yeterliyse başvurmak [tablo toplu işlem spec](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) bu ayarını değiştirme hakkında daha fazla ayrıntı için. |
 
## <a name="service-bus-queues"></a>Service Bus Kuyrukları
[Hizmet veri yolu kuyrukları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir ilk olarak, bir veya birden çok rakip tüketiciye ilk çıkar (FIFO) ileti teslimi sunar. Genellikle, ileti aldı ve hangi kuyruğa eklendikleri ve her ileti alındı ve tek bir ileti tüketicisi tarafından alınıp zamana bağlı düzende alıcılar tarafından işlenen beklenir.

Aşağıdaki tablo özellik adları ve sıra çıktı oluşturmak için bunların açıklaması listelenmektedir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, bu hizmet veri yolu kuyruğu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay bir addır. |
| Hizmet veri yolu Namespace |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. |
| Kuyruk adı |Hizmet veri yolu kuyruğu adı. |
| Sıra ilke adı |Bir kuyruk oluşturduğunuzda kuyruk yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir ad, ayarladığınız izinler ve erişim anahtarları. |
| Sıra ilke anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olayı seri hale getirme biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON, UTF-8 desteklenen tek kodlama biçimi şu anda içindir. |
| sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir. |
| Biçimi |Yalnızca JSON türü için geçerlidir. Ayrılmış çizgi çıkış sahip yeni bir çizgiyle ayrılmış her bir JSON nesnesi olarak biçimlendirileceğini belirtir. Dizi çıkışı bir dizi JSON nesnesi biçimlendirileceğini belirtir. |

## <a name="service-bus-topics"></a>Service Bus Konuları
Hizmet veri yolu kuyrukları gönderenden alıcıya, bire bir iletişim yöntemi sunarken [Service Bus konu başlıklarına](https://msdn.microsoft.com/library/azure/hh367516.aspx) bir-çok form iletişim sağlar.

Özellik adlarının ve tablo çıktısı oluşturmak için bunların açıklamaları aşağıdaki tabloda listelenmiştir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıkış diğer adları |Bu, bu hizmet veri yolu konusu sorgu çıktısını yönlendirmek için sorguda kullanılan kolay bir addır. |
| Hizmet veri yolu Namespace |Bir hizmet veri yolu ad alanı, Mesajlaşma varlıkları kümesine ilişkin bir kapsayıcıdır. Yeni bir olay hub'ı oluşturduğunuzda, hizmet veri yolu ad alanı da oluşturmuş |
| Konu adı |Konular, olay hub'ları ve kuyrukları benzer Mesajlaşma varlıklarıdır. Bir dizi farklı cihaz ve Hizmetleri hizmetten olay akışları toplayacak şekilde tasarlanmışlardır. Bir konu oluşturduğunuzda, ayrıca belirli bir ad verilir. Bir abonelik oluşturulur sürece, bunu bir konu başlığına gönderilen iletiler kullanılamaz konu altında bir veya daha fazla abonelik olmadığından emin olun |
| Konu ilke adı |Bir konu oluşturduğunuzda konu yapılandırma sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Her paylaşılan erişim ilkesinin bir ad, ayarladığınız izinler ve erişim anahtarları |
| Konu ilke anahtarı |Hizmet veri yolu ad alanına erişimi kimlik doğrulaması için kullanılan paylaşılan erişim anahtarı |
| Olayı seri hale getirme biçimi |Çıkış verileri seri hale getirme biçimi.  JSON, CSV ve Avro desteklenir. |
 | Encoding |Bir kodlama, CSV veya JSON biçiminde, belirtilmiş olması gerekir. Şu anda desteklenen tek kodlama biçimi UTF-8 durumda |
| sınırlayıcı |Yalnızca, CSV serileştirme için de geçerlidir. Akış Analizi, CSV biçiminde verilerin serileştirilmesi için yaygın olarak kullanılan bazı sınırlayıcıları destekler. Virgül, noktalı virgül, boşluk, sekme ve dikey çubuk bunun desteklenen değerlerdir. |

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) hizmet teklifleri sınırsız esnek ölçek dünya zengin sorgu ve şema belirsiz veri modelleri otomatik dizin oluşturma işlemi geçici garanti düşük gecikme süresi ve endüstri lideri Genel dağıtılmış, birden çok model bir veri tabanıdır kapsamlı SLA.

Aşağıdaki liste özellik adları ve bir Azure Cosmos DB çıktı oluşturmak için bunların açıklaması ayrıntıları.

* **Diğer ad çıktı** – Bu çıktı ASA Sorgunuzdaki başvurmak için diğer bir  
* **Hesap adı** – adını veya bitiş noktası Cosmos DB hesabının URI'si.  
* **Anahtar hesap** – Cosmos DB hesabı için paylaşılan erişim anahtarı.  
* **Veritabanı** – Cosmos DB veritabanı adı.  
* **Koleksiyon adı deseni** – koleksiyon adını veya kendi deseni kullanılacak koleksiyonlar için. Koleksiyon adı biçimi, burada bölümlerin 0'dan başlar isteğe bağlı {partition} belirteci kullanılarak oluşturulabilir. Örnek geçerli girişler şunlardır:  
  1\) MyCollection – "MyCollection" adlı bir koleksiyon bulunmalıdır.  
  2\) – böyle koleksiyonları bulunmalıdır – MyCollection {partition} "MyCollection0", "MyCollection1", "MyCollection2" ve benzeri.  
* **Anahtar bölüm** – isteğe bağlıdır. Bu, yalnızca, koleksiyon adı deseni {bölümlendirmesini} belirteci kullanıyorsanız gereklidir. Çıkışın koleksiyonlar üzerinde bölümlenmesine yönelik anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı. Tek koleksiyon çıktı için herhangi bir rastgele çıkış sütunu olabilir örneğin PartitionID kullanılır.  
* **Belge Kimliği** – isteğe bağlıdır. Hangi ekleme veya güncelleştirme işlemleri dayalı birincil anahtarın belirtilmesi için kullanılan çıkış olaylarındaki alanın adı.  

## <a name="azure-functions-in-preview"></a>Azure işlevlerinde (Önizleme)
Azure İşlevleri, açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmanıza olanak sağlayan bir sunucusuz işlem hizmetidir. Azure veya üçüncü taraf hizmetleri gerçekleşen olaylar tarafından tetiklenen kodları uygulama olanak sağlar.  Tetikleyiciler için yanıt Özelliği Azure işlevlerinin bir Azure akış analizi için doğal bir çıktı kolaylaştırır. Bu çıkış bağdaştırıcısı Stream Analytics Azure işlevleri bağlanmak ve yanıt olayları çeşitli olarak bir komut dosyası veya kod parçası çalıştırmak kullanıcıların sağlar.

Azure Stream Analytics Azure işlevleri HTTP Tetikleyicileri çağırır. Aşağıdaki yapılandırılabilir özelliklere sahip yeni Azure işlevi çıkış bağdaştırıcısı kullanılabilir:

| Özellik adı | Açıklama |
| --- | --- |
| İşlev uygulaması |Azure işlevleri uygulamanızın adı |
| İşlevi |Azure işlevleri uygulamanızda işlevin adı |
| En büyük toplu iş boyutu |Bu özellik, Azure işlevinizi gönderilen her bir çıkış toplu iş boyutu üst sınırı ayarlamak için kullanılabilir. Varsayılan olarak, bu değer 256 KB'dir |
| En büyük toplu iş sayısı  |Adından da anlaşılacağı gibi bu özellik için Azure işlevleri gönderilir her yığında olayların maksimum sayısını belirtmenize olanak sağlar. Varsayılan Maksimum toplu iş sayısı değeri 100'dür |
| Anahtar |Başka bir abonelik Azure işlevinden kullanmak istiyorsanız, işlevinizi erişmeye yönelik anahtar sağlayarak bunu yapabilirsiniz |

Azure Stream Analytics Azure işlevinden 413 (http istek varlığı çok büyük) özel durum aldığında, bunu Azure işlevleri gönderir toplu boyutunu azaltır unutmayın. Azure işlevi kodunuzda bu özel durumun Azure akış analizi büyük boyutlu toplu göndermez emin olmak için kullanın. Ayrıca, lütfen işlevde kullanılan Maksimum toplu iş sayısı ve boyutu değerlerinin Stream Analytics portalda girdiğiniz değerleri ile tutarlı olduğundan emin olun. 

Ayrıca, bir durumda bir zaman penceresi için giriş olay olduğunda, çıktı oluşturulur. Sonuç olarak, computeResult işlev çağrılmaz. Bu davranış, yerleşik pencereli toplama işlevleri tutarlıdır.


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
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
