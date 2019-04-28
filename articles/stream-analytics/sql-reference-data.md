---
title: Azure Stream Analytics işi (Önizleme) için bir SQL veritabanı başvuru verilerini kullanma
description: Bu makalede, Azure portalında ve Visual Studio'da Azure Stream Analytics işi için başvuru veri girişi SQL veritabanı kullanmayı açıklar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: 3368be291770133cdfa10158f6e30540e17b8223
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61363760"
---
# <a name="use-reference-data-from-a-sql-database-for-an-azure-stream-analytics-job-preview"></a>Azure Stream Analytics işi (Önizleme) için bir SQL veritabanı başvuru verilerini kullanma

Azure Stream Analytics, Azure SQL veritabanı kaynağı başvuru verileri için giriş olarak destekler. SQL veritabanı, Stream Analytics işiniz için Visual Studio ve Azure portalında Stream Analytics araçları ile başvuru veri kullanabilirsiniz. Bu makalede, iki yöntem de bunu nasıl yapacağınızı gösterir.

## <a name="azure-portal"></a>Azure portal

Azure portalını kullanarak bir başvuru giriş kaynağı Azure SQL veritabanı eklemek için aşağıdaki adımları kullanın:

### <a name="portal-prerequisites"></a>Portal önkoşulları

1. Bir Stream Analytics işi oluşturun.

2. Stream Analytics işi tarafından kullanılacak bir depolama hesabı oluşturun.

3. Stream Analytics işi tarafından başvuru verisi olarak kullanılacak bir veri kümesi ile Azure SQL veritabanınızı oluşturun.

### <a name="define-sql-database-reference-data-input"></a>SQL veritabanı başvuru veri girişi tanımlayın

1. Stream Analytics işinizi seçin **girişleri** altında **iş topolojisi**. Tıklayın **başvuru Girişi Ekle** ve **SQL veritabanı**.

   ![Stream Analytics işi girişi](./media/sql-reference-data/stream-analytics-inputs.png)

2. Stream Analytics giriş yapılandırmaları doldurun. Veritabanı adı, sunucu adı, kullanıcı adı ve parola seçin. Başvuru verilerini düzenli aralıklarla yenilenmesi için giriş istiyorsanız, yenileme hızı DD:HH:MM belirtmek "On" seçin. Büyük veri kümeleriyle kısa yenileme hızı varsa, kullanabileceğiniz bir [delta sorgu](sql-reference-data.md#delta-query).

   ![SQL veritabanı başvurusu yapılandırma](./media/sql-reference-data/sql-input-config.png)

3. Anlık görüntü sorgu SQL sorgu Düzenleyicisi'nde test edin. Daha fazla bilgi için [bağlanmak ve veri sorgulamak için Azure portalında SQL sorgu Düzenleyicisi'ni kullanın](../sql-database/sql-database-connect-query-portal.md)

### <a name="specify-storage-account-in-job-config"></a>İş Yapılandırması ' depolama hesabı belirtin

Gidin **depolama hesabı ayarlarını** altında **yapılandırma** seçip **depolama hesabı ekleme**.

   ![Stream Analytics depolama hesabı ayarları](./media/sql-reference-data/storage-account-settings.png)

### <a name="start-the-job"></a>İşi başlatma

Diğer girişler, çıkışlar ve sorgu yapılandırdıktan sonra Stream Analytics işi başlatabilirsiniz.

## <a name="tools-for-visual-studio"></a>Visual Studio Araçları

Visual Studio kullanarak bir başvuru giriş kaynağı Azure SQL veritabanı eklemek için aşağıdaki adımları kullanın:

### <a name="visual-studio-prerequisites"></a>Visual Studio önkoşulları

1. Visual Studio 2017'yi kullanıyorsanız, 15.8.2 veya üstüne güncelleştirin. Unutmayın 16,0 ve yukarıdaki bu noktada desteklenmez.

2. [Visual Studio için Stream Analytics Araçları'nı yükleme](stream-analytics-tools-for-visual-studio-install.md). Visual Studio aşağıdaki sürümleri desteklenir:

   * Visual Studio 2015
   * Visual Studio 2017

3. Sahibi [Visual Studio için Stream Analytics Araçları](stream-analytics-quick-create-vs.md) hızlı başlangıç.

4. Depolama hesabı oluşturma.

### <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

Başvuru verileri depolamak için bir tablo oluşturmak için SQL Server Management Studio'yu kullanın. Bkz: [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](../sql-database/sql-database-design-first-database.md) Ayrıntılar için.

Aşağıdaki ifade aşağıdaki örnekte kullanılan örnek tablo oluşturuldu:

```SQL
create table chemicals(Id Bigint,Name Nvarchar(max),FullName Nvarchar(max));
```

### <a name="choose-your-subscription"></a>Aboneliğinizi seçin

1. Visual Studio'nun **Görünüm** menüsünde **Sunucu Gezgini**'ni seçin.

2. Sağ tıklayın **Azure**seçin **Microsoft Azure aboneliğine bağlanma**ve Azure hesabınızla oturum açın.

### <a name="create-a-stream-analytics-project"></a>Stream Analytics projesi oluşturma

1. **Dosya > Yeni Proje**'yi seçin. 

2. Sol taraftaki şablon listesinden **Stream Analytics**'i ve ardından **Azure Stream Analytics Uygulaması**'nı seçin. 

3. Proje girin **adı**, **konumu**, ve **çözüm adı**seçip **Tamam**.

   ![Visual Studio'da yeni Stream Analytics projesi](./media/sql-reference-data/stream-analytics-vs-new-project.png)

### <a name="define-sql-database-reference-data-input"></a>SQL veritabanı başvuru veri girişi tanımlayın

1. Yeni bir giriş oluşturun.

   ![Visual Studio'da yeni Stream Analytics'e giriş](./media/sql-reference-data/stream-analytics-vs-input.png)

2. Çift **söz konusu Input.json** içinde **Çözüm Gezgini**.

3. Doldurun **Stream Analytics'e giriş yapılandırma**. Sunucu adı, veritabanı adı seçin yenileme türü ve yenileme hızı. Yenileme hızı belirttiğiniz biçimde `DD:HH:MM`.

   ![Visual Studio'da Stream Analytics'e giriş yapılandırma](./media/sql-reference-data/stream-analytics-vs-input-config.png)

   "Yalnızca bir kez" veya "düzenli aralıklarla yürütün" seçeneğini belirlerseniz, bir SQL CodeBehind dosya adlı **[Giriş diğer adı].snapshot.sql** projesi altında oluşturulan **söz konusu Input.json** dosya düğümü.

   ![Visual Studio'da arka plan kod giriş](./media/sql-reference-data/once-or-periodically-codebehind.png)

   "Yenileme düzenli aralıklarla ile Delta" seçeneğini belirlerseniz, iki SQL CodeBehind dosya oluşturulur: **[Giriş diğer adı].snapshot.sql** ve **[Giriş diğer adı].delta.sql**.

   ![Çözüm Gezgini'nde code behind](./media/sql-reference-data/periodically-delta-codebehind.png)

4. SQL Dosyası Düzenleyicisi'nde açın ve SQL sorgusu yaz.

5. Visual Studio 2017 kullandığınızı ve yüklediğiniz SQL Server veri araçları, tıklayarak sorguyu test edebilirsiniz **yürütme**. SQL veritabanı'na bağlanma yardımcı olacak bir sihirbaz penceresi açılır ve sorgu sonucu altındaki penceresinde görünür.

### <a name="specify-storage-account"></a>Depolama hesabı belirtin

Açık **JobConfig.json** SQL Başvurusu anlık görüntülerini depolamak için depolama hesabı belirtmek için.

   ![Visual Studio'da Stream Analytics işi yapılandırma](./media/sql-reference-data/stream-analytics-job-config.png)

### <a name="test-locally-and-deploy-to-azure"></a>Yerel olarak test etmek ve Azure'a dağıtma

İşi Azure'a dağıtmadan önce Canlı giriş verileri yerel olarak karşı sorgu mantığının test edebilirsiniz. Bu özellik hakkında daha fazla bilgi için bkz. [canlı verileri Azure Stream Analytics araçları Visual Studio (Önizleme) kullanarak yerel olarak Test](stream-analytics-live-data-local-testing.md). Bitirdiğinizde test tıklayın **azure'a Gönder**. Başvuru [bir Stream Analytics Visual Studio için Azure Stream Analytics araçlarını kullanarak oluşturmak](stream-analytics-quick-create-vs.md) işin başlaması öğrenmek için hızlı başlangıç.

## <a name="delta-query"></a>Delta sorgu

Delta sorgu kullanarak [zamana bağlı tablolarda Azure SQL veritabanı'nda](../sql-database/sql-database-temporal-tables.md) önerilir.

1. Zamana bağlı tablo, Azure SQL veritabanı'nda oluşturun.
   
   ```SQL 
      CREATE TABLE DeviceTemporal 
      (  
         [DeviceId] int NOT NULL PRIMARY KEY CLUSTERED 
         , [GroupDeviceId] nvarchar(100) NOT NULL
         , [Description] nvarchar(100) NOT NULL 
         , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
         , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
         , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
      )  
      WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DeviceHistory));  -- DeviceHistory table will be used in Delta query
   ```
2. Anlık görüntü sorgu yazar. 

   Kullanım  **\@snapshotTime** parametresini kullanarak SQL veritabanı zamana bağlı tablo geçerli sistem saatini başvuru veri kümesi almak için Stream Analytics çalışma zamanı isteyin. Bu parametre sağlamazsanız, bir saat farklarından kaynaklanan yanlış temel başvuru veri kümesi alma riski oluşur. Tam bir anlık görüntü sorgu örneği aşağıda gösterilmiştir:
   ```SQL
      SELECT DeviceId, GroupDeviceId, [Description]
      FROM dbo.DeviceTemporal
      FOR SYSTEM_TIME AS OF @snapshotTime
   ```
 
2. Delta sorgu yazar. 
   
   Bu sorgu, eklenen veya silinmiş bir başlangıç saati içinde SQL veritabanınızdaki tüm satırları alır  **\@deltaStartTime**ve bitiş zamanını  **\@deltaEndTime**. Delta sorgu anlık görüntü sorgu aynı sütunları yanı sıra, sütun döndürmelidir  **_işlemi_**. Satır eklenmiş veya arasında silinmiş bu sütun tanımlar  **\@deltaStartTime** ve  **\@deltaEndTime**. Ortaya çıkan satırlar olarak işaretlenmiş **1** kayıtların eklenme ise veya **2** silinmiş. 

   Güncelleştirilen kayıtları, zamana bağlı tablo ekleme ve silme işleminin yakalayarak muhasebe yapar. Stream Analytics çalışma zamanı, başvuru verileri güncel tutmak için önceki anlık görüntüye delta sorgu sonuçlarını sonra uygulanır. Aşağıdaki show delta sorgu örneğidir:

   ```SQL
      SELECT DeviceId, GroupDeviceId, Description, 1 as _operation_
      FROM dbo.DeviceTemporal
      WHERE ValidFrom BETWEEN @deltaStartTime AND @deltaEndTime   -- records inserted
      UNION
      SELECT DeviceId, GroupDeviceId, Description, 2 as _operation_
      FROM dbo.DeviceHistory   -- table we created in step 1
      WHERE ValidTo BETWEEN @deltaStartTime AND @deltaEndTime     -- record deleted
   ```
 
   Stream Analytics çalışma zamanı düzenli aralıklarla kontrol noktalarını depolamak için delta sorgu yanı sıra anlık görüntü sorgu çalışabilir unutmayın.

## <a name="faqs"></a>SSS

**Ek ücret Azure Stream Analytics'te SQL başvuru veri girişi kullanarak neden olur?**

Vardır hiç [akış birimi başına maliyet](https://azure.microsoft.com/pricing/details/stream-analytics/) Stream Analytics işinde. Ancak, Stream Analytics işi, ilişkili Azure depolama hesabınız olması gerekir. Stream Analytics işi SQL DB sorgular (işi sırasında başlatmak ve yenileme aralığı) başvuru veri kümesi ve anlık görüntü depolama hesabında depoları alınamadı. Bu anlık görüntüler depolama ayrıntılı belirlenen ek ücretleri ödemesi [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/) Azure depolama hesabı için.

**Başvuru veri anlık görüntü okunuyor nasıl bilebilirim SQL Veritabanından sorgulanabilir ve Azure Stream Analytics işinde kullanılır?**

Mantıksal SQL veritabanı başvuru verilerini giriş durumunu izlemek için kullanabileceğiniz adıyla (altında ölçümleri Azure portalı) filtrelenmiş iki ölçüm vardır.

   * Inputevents: Bu ölçüm, SQL veritabanı başvuru veri kümesinden yüklenen kayıt sayısını ölçer.
   * InputEventBytes: Bu ölçüm, Stream Analytics işi, belleğe başvuru veri anlık görüntüsünün boyutu ölçer. 

Bu ölçüm her ikisinin bir birleşimi iş başvuru veri kümesi getirmek için SQL veritabanı sorgulama ve sonra bellek yükleme durumunda çıkarsamak için kullanılabilir.

**Ben Azure SQL veritabanı'nın özel bir türü gerekiyor mu?**

Azure Stream Analytics, Azure SQL veritabanı'nın herhangi bir türü ile çalışır. Ancak, başvuru veri girişi için ayarlanmış yenileme hızı, sorgu yükünü etkileyebilecek anlamak önemlidir. Delta sorgu seçeneği kullanmak için zamana bağlı tablolarda Azure SQL veritabanı'nda kullanmanız önerilir.

**Ben, SQL veritabanı başvuru veri girişi girişten örnekleme yapabilirsiniz?**

Bu özellik kullanılamıyor.

**Neden Azure Stream Analytics anlık görüntüleri Azure depolama hesabında depoluyor mu?**

Stream Analytics tam bir olay işlemesi ve en az bir olay teslimini garanti eder. Burada geçici iş etkisi sorunları durumlarda, az miktarda bir yeniden yürütme durumunu geri yüklemek gereklidir. Yeniden yürütme etkinleştirmek için bir Azure depolama hesabında depolanan bu anlık görüntüler için gereklidir. Denetim noktası yeniden yürütme hakkında daha fazla bilgi için bkz. [Azure Stream Analytics işlerini kontrol noktası ve yeniden yürütme kavramları](stream-analytics-concepts-checkpoint-replay.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Stream analytics'te aramaları için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)
* [Hızlı Başlangıç: Visual Studio için Azure Stream Analytics araçları kullanarak bir Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Azure Stream Analytics araçları Visual Studio (Önizleme) kullanarak yerel olarak test canlı verileri](stream-analytics-live-data-local-testing.md)
