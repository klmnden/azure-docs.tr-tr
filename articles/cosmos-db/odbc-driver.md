---
title: BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanma
description: Azure Cosmos DB ODBC sürücüsü, böylece normalleştirilmiş veri BI ve veri analizi yazılımda görüntülenebilir tabloları ve görünümleri oluşturmak için kullanmayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: 352cd23f00e911b895e52aacaced1bfba38f7f84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257246"
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>ODBC sürücüsü ile BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanma

Azure Cosmos DB ODBC sürücüsü, çözümleyebilir ve bu çözümleri Azure Cosmos DB verilerinizi görselleştirmeler oluşturmak için SQL Server Integration Services, Power BI Desktop ve Tableau gibi BI analizi araçları kullanarak Azure Cosmos DB'ye bağlanmak sağlar.

Azure Cosmos DB ODBC sürücüsü ODBC 3.8 ile uyumludur ve ANSI SQL-92 söz dizimini destekler. Sürücü, verileri Azure Cosmos DB'de yeniden normalleştirmenize yardımcı olan zengin özellikler sunar. Sürücüyü kullanarak verilerinizi Azure Cosmos DB'de tablo ve görünüm olarak gösterebilirsiniz. Sürücü sorgulara, eklemelere, güncelleştirmelere ve silmelere göre gruplama dahil olmak üzere tablo ve görünümlerde SQL işlemleri gerçekleştirmenizi sağlar.

> [!NOTE]
> ODBC sürücüsü ile Azure Cosmos DB'ye bağlanma, şu anda yalnızca Azure Cosmos DB SQL API hesapları için desteklenir.

## <a name="why-do-i-need-to-normalize-my-data"></a>Verilerim'leri normalleştirmek neden ihtiyacım var?
Azure Cosmos DB hızlı uygulama geliştirme ve veri modelleri için kesin bir şema sınırlı olmadan yineleme özelliğini etkinleştirir şemasız bir veritabanıdır. Tek bir Azure Cosmos DB veritabanı, JSON belgelerini çeşitli yapıları içerebilir. Bu hızlı uygulama geliştirme için harika bir deneyimdir ancak analiz edin ve veri analizi ve BI araçları kullanarak verilerinizi raporları oluşturmak istediğinizde, verilerin genellikle düzleştirilmiş ve belirli bir şemaya bağlı kalması gerekir.

Bu, ODBC sürücüsü burada devreye girer. ODBC sürücüsünü kullanarak Azure Cosmos DB'de tablolar ve görünümler, verileri analiz ve raporlama ihtiyaçlarını uyan verileri artık normalleştirebilir. Renormalized şemaları, temel alınan veriler üzerinde hiçbir etkisi ve bunları kullanan geliştiricilerin sınırlamak değil. Bunun yerine, verilere erişmek için ODBC uyumlu araçlarından yararlanarak sağlıyor. Bu nedenle, artık Azure Cosmos DB veritabanınıza yalnızca Geliştirme ekibiniz için sık kullanılan olmayacak, ancak, veri analistleri, çok seveceksiniz.

ODBC sürücüsü ile başlayalım.

## <a id="install"></a>1. adım: Azure Cosmos DB, ODBC sürücüsünü yükleme

1. Ortamınız için sürücüleri yükleyin:

    | Yükleyici | Desteklenen işletim sistemleri| 
    |---|---| 
    |[Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/cosmos-odbc-64x64) 64 bit Windows için| 64-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2.| 
    |[Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/cosmos-odbc-32x64) 32-bit, 64 bit Windows üzerinde| 64-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ve Windows Server 2003.| 
    |[Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/cosmos-odbc-32x32) 32-bit Windows için|32-bit sürüm Windows 8.1 veya üstü, Windows 8, Windows 7, Windows XP ve Windows Vista.|

    Başlatan MSI dosyasını yerel olarak çalıştırma **Microsoft Azure Cosmos DB ODBC sürücüsünü Yükleme Sihirbazı**. 

1. Giriş ODBC sürücüsünü yüklemek için varsayılan değeri kullanmanın yükleme sihirbazını tamamlayın.

1. Açık **ODBC Veri Kaynağı Yöneticisi** bilgisayarınızda uygulama. Yazarak bunu yapabilirsiniz **ODBC veri kaynakları** Windows Arama kutusuna. 
    Sürücü tıklayarak yüklendiği onaylayabilirsiniz **sürücüleri** sekmesi ve sağlama **Microsoft Azure Cosmos DB ODBC sürücüsü** listelenir.

    ![Azure Cosmos DB ODBC Veri Kaynağı Yöneticisi](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>2. adım: Azure Cosmos DB veritabanınıza bağlanın

1. Sonra [Azure Cosmos DB ODBC sürücüsünü yükleme](#install), **ODBC Veri Kaynağı Yöneticisi** penceresinde tıklayın **Ekle**. Bir kullanıcı veya sistem DSN'si oluşturabilirsiniz. Bu örnekte, bir kullanıcı DSN'si oluşturuyorsunuz.

1. İçinde **yeni veri kaynağı oluştur** penceresinde **Microsoft Azure Cosmos DB ODBC sürücüsü**ve ardından **son**.

1. İçinde **Azure Cosmos DB ODBC sürücü SDN Kurulumu** penceresinde aşağıdaki bilgileri doldurun: 

    ![Azure Cosmos DB ODBC sürücüsü DSN Kurulum penceresi](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Veri kaynağı adı**: ODBC DSN kendi kolay adı. Bu Azure Cosmos DB hesabınız için benzersiz bir addır, birden çok hesabı varsa, bu nedenle uygun şekilde adlandırın.
    - **Açıklama**: Veri kaynağı kısa bir açıklaması.
    - **Konak**: Azure Cosmos DB hesabınız için URI. Bu Azure Cosmos DB anahtarlar sayfasından Azure portalında, aşağıdaki ekran görüntüsünde gösterildiği gibi alabilirsiniz. 
    - **Erişim anahtarı**: Aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında Azure Cosmos DB anahtarlar sayfasında birincil veya ikincil, salt okunur veya salt okunur anahtar. DSN salt okunur veri işleme ve raporlama için kullanılıyorsa, salt okunur anahtarı kullanmanızı öneririz.
    ![Azure Cosmos DB anahtarları sayfası](./media/odbc-driver/odbc-driver-keys.png)
    - **Erişim anahtarı için şifreleme**: Bu makinenin kullanıcı temelli en iyi seçenek seçin. 
    
1. Tıklayın **Test** düğmesini Azure Cosmos DB hesabınıza bağlandığınızdan emin olun. 

1. Tıklayın **Gelişmiş Seçenekler** ve aşağıdaki değerleri ayarlayın:
    - **Sorgu tutarlılık**: Seçin [tutarlılık düzeyi](consistency-levels.md) işlemleriniz için. Varsayılan oturumdur.
    - **Yeniden deneme sayısı**: İlk istek, hizmet hız sınırlaması nedeniyle tamamlanmazsa, bir işlemin yeniden deneme sayısını girin.
    - **Şema dosyası**: Burada bir dizi seçeneğiniz vardır.
        - Bu giriş (boş) olduğu gibi bırakarak varsayılan olarak, her koleksiyonun şema belirlemek tüm koleksiyonlar için veriler'ın ilk sayfasında sürücü tarar. Bu işlem, koleksiyon eşlemesi bilinir. Tanımlı bir şema dosyası olmadan sürücü her bir sürücü oturumu için tarama yapması ve uygulamanın DSN daha yüksek başlangıç saati, neden olabilir. Bir şema dosyası her zaman için DSN ilişkilendirmenizi öneririz.
        - Bir şema dosyası (büyük olasılıkla bir şema Düzenleyicisi'ni kullanarak oluşturduğunuz) zaten varsa, tıklayabilirsiniz **Gözat**, dosyaya gidin, tıklayın **Kaydet**ve ardından **Tamam**.
        - Yeni bir şema oluşturmak istiyorsanız, tıklayın **Tamam**ve ardından **şema Düzenleyicisi** ana penceresinde. Şema Düzenleyicisi bilgilerin devam edin. Yeni şema dosyası oluşturduktan sonra geri dönüp unutmayın **Gelişmiş Seçenekler** penceresi, yeni oluşturulan şema dosyası eklenecek.

1. Tamamlayıp kapatmak sonra **Azure Cosmos DB ODBC sürücü DSN Kurulumu** penceresi, yeni kullanıcı DSN'si Kullanıcı DSN sekmesine eklenir.

    ![Yeni Azure Cosmos DB ODBC DSN Kullanıcı DSN sekmesinde](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>3. adım: Koleksiyon eşleme yöntemini kullanarak bir şema tanımı oluşturma

Kullanabileceğiniz örnekleme yöntemleri iki tür vardır: **koleksiyon eşleme** veya **tablo sınırlayıcılar**. Örnek bir oturum hem örnekleme yöntemleri kullanabilir, ancak her koleksiyon, yalnızca belirli örnekleme yöntemini kullanabilirsiniz. Aşağıdaki adımlar koleksiyonlarında, bir veya daha fazla koleksiyon eşleme yöntemini kullanarak veriler için bir şema oluşturun. Bu örnekleme yöntemi veri yapısını belirlemek için bir koleksiyonun sayfasındaki verileri alır. Bu tablo ODBC tarafında bir koleksiyona kendisini veya sırasını değiştirir. Bir koleksiyondaki verileri homojen bu örnekleme yöntemini verimli ve hızlı olur. Bir koleksiyon heterojen veri türünü içeriyorsa, kullanmanızı öneririz [tablo sınırlayıcılar yöntemi eşleme](#table-mapping) gibi veri yapıları koleksiyondaki belirlemek için daha sağlam bir örnekleme yöntemini sağlar. 

1. Adım 1-4'te tamamladıktan sonra [Azure Cosmos DB veritabanınıza bağlanma](#connect), tıklayın **şema Düzenleyicisi** içinde **Azure Cosmos DB ODBC sürücü DSN Kurulumu** penceresi.

    ![Azure Cosmos DB ODBC sürücü DSN Kurulumu penceresinde şema Düzenleyici düğmesi](./media/odbc-driver/odbc-driver-schema-editor.png)
1. İçinde **şema Düzenleyicisi** penceresinde tıklayın **Yeni Oluştur**.
    **Şema oluşturmak** penceresinde Azure Cosmos DB hesabını tüm koleksiyonları görüntüler. 

1. Örnek ve ardından bir veya daha fazla koleksiyon seçin **örnek**. 

1. İçinde **Tasarım görünümü** sekmesi, veritabanı, şema ve tablo temsil edilir. Tablo Görünümü'nde tarama sütun adları (SQL adı, kaynak adı, vb.) ile ilişkili özellikler kümesini görüntüler.
    Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü SQL uzunluğu (varsa), (varsa) ölçek, duyarlık (varsa) ve boş değer.
    - Ayarlayabileceğiniz **Gizle sütun** için **true** sorgu sonuçlarından sütuna dışlamak istiyorsanız. Sütunları gizleme sütun işaretli = true yine de şemanın bir parçası olsa seçimi ve projeksiyon döndürülmez. Örneğin, "_" ile başlayan tüm Azure Cosmos DB sistem için gerekli özellikler gizleyebilirsiniz.
    - **Kimliği** normalleştirilmiş şeması birincil anahtar olarak kullanılan gizlenemez tek alan bir sütundur. 

1. Şemasını tanımlamak tamamladıktan sonra tıklayın **dosya** | **Kaydet**şemayı kaydetmek için dizine gidin ve ardından **Kaydet**.

1. Bu şema DSN ile kullanmak için açık **Azure Cosmos DB ODBC sürücüsü DSN Kurulum penceresi** tıklayın (ODBC Veri Kaynağı Yöneticisi aracılığıyla), **Gelişmiş Seçenekler**ve ardından **şemadosyası** kutusunda, kaydedilmiş şemaya gidin. Var olan bir DSN bir şema dosyası kaydetme veri ve şema tarafından tanımlanan yapısına kapsamına DSN bağlantı değiştirir.

## <a id="table-mapping"></a>4. adım: Tablo sınırlayıcı kullanarak bir şema tanımı oluşturma yöntemi eşleme

Kullanabileceğiniz örnekleme yöntemleri iki tür vardır: **koleksiyon eşleme** veya **tablo sınırlayıcılar**. Örnek bir oturum hem örnekleme yöntemleri kullanabilir, ancak her koleksiyon, yalnızca belirli örnekleme yöntemini kullanabilirsiniz. 

Aşağıdaki adımları kullanarak bir veya daha fazla koleksiyonlarında veriler için bir şema oluşturmak **tablo sınırlayıcılar** yöntemi eşleme. Koleksiyonlarınız heterojen veri türünü içerdiğinde bu örnekleme yöntemini kullanmanızı öneririz. Bir dizi öznitelikleri ve karşılık gelen değerleri örnekleme kapsamını belirlemek için bu yöntemi kullanabilirsiniz. Örneğin, bir belge bir "Type" özelliği içeriyorsa, bu özelliğin değerlerine örnekleme kapsamını belirleyebilirsiniz. Örnekleme nihai sonucu tabloların her biri, belirttiğiniz türü değerleri için bir küme olacaktır. Örneğin = araba, bir araba tablosu türü üretir = düz bir düz tablo oluşturmak.

1. Adım 1-4'te tamamladıktan sonra [Azure Cosmos DB veritabanınıza bağlanma](#connect), tıklayın **şema Düzenleyicisi** Azure Cosmos DB ODBC sürücü DSN Kurulumu penceresinde.

1. İçinde **şema Düzenleyicisi** penceresinde tıklayın **Yeni Oluştur**.
    **Şema oluşturmak** penceresinde Azure Cosmos DB hesabını tüm koleksiyonları görüntüler. 

1. Bir koleksiyon seçin **örnek görünümü** sekmesinde **eşleme tanımı** sütun koleksiyonu için tıklatın **Düzenle**. Ardından **eşleme tanımı** penceresinde **tablo sınırlayıcılar** yöntemi. Ardından şunları yapın:

    a. İçinde **öznitelikleri** sınırlayıcı özelliğin adını yazın. Bu kapsam örnekleme için örneğin, şehir ve enter tuşuna basın, belgedeki bir özelliğidir. 

    b. Yukarıda girilen özniteliği için belirli değerlere örnekleme kapsam istiyorsanız, seçim kutusunda özniteliği seçin, bir değer girin **değer** kutusunda (örneğin, Seattle) ve enter tuşuna basın. Öznitelikler için birden çok değer eklemeye devam edebilirsiniz. Yalnızca doğru öznitelik değerleri girerken seçili olduğundan emin olun.

    Örneğin eklerseniz, bir **öznitelikleri** tablonuzda yalnızca Dubai ve New York city değerine sahip satırları içerecek şekilde sınırlamak istediğiniz şehir ve değerini, Şehir öznitelikleri kutusu ve New York sonra Dubai girersiniz**Değerleri** kutusu.

1. **Tamam** düğmesine tıklayın. 

1. Koleksiyonlar için eşleme tanımlarını tamamladıktan sonra örnek, buna istediğiniz **şema Düzenleyicisi** penceresinde tıklayın **örnek**.
     Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü SQL uzunluğu (varsa), (varsa) ölçek, duyarlık (varsa) ve boş değer.
    - Ayarlayabileceğiniz **Gizle sütun** için **true** sorgu sonuçlarından sütuna dışlamak istiyorsanız. Sütunları gizleme sütun işaretli = true yine de şemanın bir parçası olsa seçimi ve projeksiyon döndürülmez. İle başlayarak tüm Azure Cosmos DB gerekli sistem özellikleri gibi gizleyebilirsiniz `_`.
    - **Kimliği** normalleştirilmiş şeması birincil anahtar olarak kullanılan gizlenemez tek alan bir sütundur. 

1. Şemasını tanımlamak tamamladıktan sonra tıklayın **dosya** | **Kaydet**şemayı kaydetmek için dizine gidin ve ardından **Kaydet**.

1. Geri **Azure Cosmos DB ODBC sürücü DSN Kurulumu** penceresinde tıklayın **Gelişmiş Seçenekler**. Ardından **şema dosyası** kutusuna kaydedilmiş şema dosyasına gidin ve tıklatın **Tamam**. Tıklayın **Tamam** DSN yeniden kaydedin. Bu, oluşturduğunuz şema DSN'ye kaydeder. 

## <a name="optional-set-up-linked-server-connection"></a>(İsteğe bağlı) Bağlı sunucu bağlantısı kurma

Azure Cosmos DB SQL Server Management Studio (SSMS) gelen bir bağlı sunucu bağlantısı kurmak ayarlayarak sorgulayabilirsiniz.

1. Bölümünde anlatıldığı gibi bir sistem veri kaynağı oluşturma [2. adım](#connect), adlandırılmış örneğin `SDS Name`.

1. [SQL Server Management Studio'yu yüklemeniz](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve sunucuya bağlanın. 

1. SSMS sorgu Düzenleyicisi'nde bir bağlı sunucu nesnesi oluşturma `DEMOCOSMOS` aşağıdaki komutlarla veri kaynağı için. Değiştirin `DEMOCOSMOS` bağlı sunucunuzun adı ile ve `SDS Name` sistem veri kaynağı adı.

    ```sql
    USE [master]
    GO
    
    EXEC master.dbo.sp_addlinkedserver @server = N'DEMOCOSMOS', @srvproduct=N'', @provider=N'MSDASQL', @datasrc=N'SDS Name'
    
    EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'DEMOCOSMOS', @useself=N'False', @locallogin=NULL, @rmtuser=NULL, @rmtpassword=NULL
    
    GO
    ```
    
Yeni bağlantılı sunucu adıyla görmek için bağlı sunucular listesini yenileyin.

![Ssms'de bağlantılı sunucu](./media/odbc-driver/odbc-driver-linked-server-ssms.png)

### <a name="query-linked-database"></a>Bağlantılı veritabanı sorgulama

Bağlantılı veritabanı sorgulamak için bir SSMS sorgu girin. Bu örnekte, adlı koleksiyon tablosundan sorgu seçer `customers`:

```sql
SELECT * FROM OPENQUERY(DEMOCOSMOS, 'SELECT *  FROM [customers].[customers]')
```

Sorguyu yürütün. Sonuç şuna benzer olmalıdır:

```
attachments/  1507476156    521 Bassett Avenue, Wikieup, Missouri, 5422   "2602bc56-0000-0000-0000-59da42bc0000"   2015-02-06T05:32:32 +05:00 f1ca3044f17149f3bc61f7b9c78a26df
attachments/  1507476156    167 Nassau Street, Tuskahoma, Illinois, 5998   "2602bd56-0000-0000-0000-59da42bc0000"   2015-06-16T08:54:17 +04:00 f75f949ea8de466a9ef2bdb7ce065ac8
attachments/  1507476156    885 Strong Place, Cassel, Montana, 2069       "2602be56-0000-0000-0000-59da42bc0000"   2015-03-20T07:21:47 +04:00 ef0365fb40c04bb6a3ffc4bc77c905fd
attachments/  1507476156    515 Barwell Terrace, Defiance, Tennessee, 6439     "2602c056-0000-0000-0000-59da42bc0000"   2014-10-16T06:49:04 +04:00      e913fe543490432f871bc42019663518
attachments/  1507476156    570 Ruby Street, Spokane, Idaho, 9025       "2602c156-0000-0000-0000-59da42bc0000"   2014-10-30T05:49:33 +04:00 e53072057d314bc9b36c89a8350048f3
```

> [!NOTE]
> Bağlı bir Cosmos DB sunucusunun dört kısımlı adlandırmayı desteklemiyor. Hata aşağıdaki iletiye benzer döndürülür:

```
Msg 7312, Level 16, State 1, Line 44

Invalid use of schema or catalog for OLE DB provider "MSDASQL" for linked server "DEMOCOSMOS". A four-part name was supplied, but the provider does not expose the necessary interfaces to use a catalog or schema.
``` 

## <a name="optional-creating-views"></a>(İsteğe bağlı) Görünümler oluşturma
Tanımlayabilir ve örnekleme işleminin bir parçası görünümler oluşturun. Bu görünümler için SQL görünümleri eşdeğerdir. Bunlar salt okunur ve yansıtmaların tanımlanan Azure Cosmos DB SQL sorgusu ve seçimleri kapsamı içindedir. 

Verileriniz için bir görünüm oluşturmak için **şema Düzenleyicisi** penceresi, **tanımlarını** sütun tıklayın **Ekle** örnek koleksiyonuna satırda. 
    ![Veri görünümünü oluşturma](./media/odbc-driver/odbc-driver-create-view.png)


Ardından **tanımlarını** penceresinde aşağıdakileri yapın:

1. Tıklayın **yeni**, örneğin, EmployeesfromSeattleView görünümü için bir ad girin ve ardından **Tamam**.

1. İçinde **düzenleme görünümü** penceresinde Azure Cosmos DB sorgusu girin. Bu olmalıdır bir [Azure Cosmos DB SQL sorgusu](how-to-sql-query.md), örneğin `SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Manager FROM c WHERE c.City = "Seattle"`ve ardından **Tamam**.

    ![Bir görünüm oluşturma sırasında Sorgu Ekle](./media/odbc-driver/odbc-driver-create-view-2.png)


İstediğiniz gibi birçok bir görünüm oluşturabilirsiniz. İşiniz bittiğinde görünümleri tanımlama, ardından veri örnekleme yapabilirsiniz. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>5\. Adım: Power BI Desktop gibi BI Araçları'ndaki verilerinizi görüntüleyin

ODBC uyumlu herhangi bir aracı ile Azure Cosmos DB'ye bağlanmak için yeni DSN kullanabilirsiniz: Bu adım yalnızca, bağlanmak için Power BI Desktop ve Power BI görselleştirmeleri oluşturma işlemini göstermektedir.

1. Power BI Desktop’ı açın.

1. Tıklayın **veri alma**.

    ![Power BI Desktop'ta Veri Al](./media/odbc-driver/odbc-driver-power-bi-get-data.png)

1. İçinde **Veri Al** penceresinde tıklayın **diğer** | **ODBC** | **Connect**.

    ![Power BI Veri Al ODBC veri kaynağı seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-2.png)

1. İçinde **gelen ODBC** penceresinde, veri kaynağı adı oluşturduğunuz, seçin ve ardından **Tamam**. Bırakabilirsiniz **Gelişmiş Seçenekler** girdileri boş.

    ![Power BI Veri Al veri kaynağı adı (DSN) seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-3.png)

1. İçinde **ODBC sürücüsünü kullanarak bir veri kaynağına erişmek** penceresinde **varsayılan veya özel** ve ardından **Connect**. Eklenecek gerekmez **kimlik bilgisi bağlantı dizesi özellikleri**.

1. İçinde **Gezgin** pencerede sol bölmede, veritabanı şemasını genişletin ve sonra tabloyu seçin. Sonuçlar bölmesinde, oluşturduğunuz şemayı kullanarak veriler içerir.

    ![Power BI Get verileri tablo seçin](./media/odbc-driver/odbc-driver-power-bi-get-data-4.png)

1. Power BI Desktop'ta verileri görselleştirmek için tablo adının önüne kutuyu işaretleyin ve ardından **yük**.

1. En sağda bulunan Power BI Desktop'ta sol, veri sekmesinde seçin ![Power BI Desktop'ta veri sekmesi](./media/odbc-driver/odbc-driver-data-tab.png) Verilerinizi onaylamak için içeri aktarıldı.

1. Artık Power BI Report sekmesinde tıklayarak kullanarak görselleri oluşturabilirsiniz ![Power BI Desktop'ta rapor sekmesini](./media/odbc-driver/odbc-driver-report-tab.png)a **yeni görsel**ve ardından kutucuğunuzu özelleştirme. Power BI Desktop'ta görselleştirmeler oluşturma hakkında daha fazla bilgi için bkz. [Power bı'daki görselleştirme türleri](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki hata iletisini alırsanız olun **konak** ve **erişim anahtarı** Azure portalında kopyaladığınız değerleri [2. adım](#connect) doğru olduğundan ve yeniden deneyin. Sağ tarafındaki kopyalama düğmelerini **konak** ve **erişim anahtarı** değerler hatasız kopyalamak için Azure portalında değerleri.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hakkında daha fazla bilgi edinmek için bkz. [Azure Cosmos DB'ye hoş geldiniz](introduction.md).
