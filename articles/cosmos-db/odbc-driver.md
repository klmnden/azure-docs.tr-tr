---
title: Azure Cosmos BI analiz araçları kullanarak Veritabanına bağlanın | Microsoft Docs
description: Normalleştirilmiş veri BI ve veri analizi yazılımda görüntülenebilir tabloları ve görünümleri oluşturmak üzere Azure Cosmos DB ODBC sürücüsü kullanmayı öğrenin.
keywords: ODBC, odbc sürücüsü
services: cosmos-db
author: mimig1
manager: jhubbard
editor: ''
documentationcenter: ''
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 03/22/2018
ms.author: mimig
ms.openlocfilehash: 445acafeef67027712826f644afaa1784569b30d
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="connect-to-azure-cosmos-db-using-bi-analytics-tools-with-the-odbc-driver"></a>Azure Cosmos ODBC sürücüsüyle BI analiz araçları kullanarak veritabanına bağlan

Azure Cosmos DB ODBC sürücüsü Azure Cosmos çözümleyebilir ve bu çözümleri Azure Cosmos DB verilerinizin görselleştirmeleri oluşturmak için SQL Server Integration Services, Power BI Desktop ve Tableau gibi BI analiz araçları kullanarak DB bağlanmanıza olanak sağlar.

Azure Cosmos DB ODBC sürücüsü ODBC 3.8 uyumlu olduğundan ve ANSI SQL-92 sözdizimini destekler. Sürücüyü Azure Cosmos veritabanı veri renormalize yardımcı olması için zengin özellikleri sunar. Sürücü kullanarak, Azure Cosmos veritabanı veri tabloları ve görünümleri olarak gösterebilir. Sürücü, tablolar ve sorgular tarafından dahil olmak üzere Grup, güncelleştirmeler, ekler ve siler görünümler karşı SQL işlemleri gerçekleştirmenizi sağlar.

## <a name="why-do-i-need-to-normalize-my-data"></a>Verilerimi normalleştirmek neden gerekiyor mu?
Azure Cosmos DB şemasız bir veritabanı olduğundan, kendi veri modeli anında yinelemek ve bunları sınırlamak için kesin bir şema olmayan uygulamaları etkinleştirerek uygulamaların hızlı geliştirme sağlar. Tek bir Azure Cosmos DB veritabanı çeşitli yapıların JSON belgeleri içerebilir. Bu hızlı uygulama geliştirme için mükemmeldir, ancak çözümlemek ve veri analizi ve BI araçları kullanarak, verilerinizin raporlar oluşturmak istediğinizde, veri genellikle düzleştirilmiş ve belirli bir şemaya uyması gerekir.

ODBC sürücüsü nereden geldiğini budur. ODBC sürücüsünü kullanarak, tabloları ve görünümleri veri çözümleme ve raporlama ihtiyaçlarınızı sığdırma Azure Cosmos veritabanı veri şimdi renormalize. Renormalized şemaları temel alınan veri üzerinde hiçbir etkisi ve bunlara bağlı olması geliştiriciler sınırlamak değil, bunlar, verilere erişmek için ODBC uyumlu araçları yararlanmanızı sağlar. Artık Azure Cosmos DB veritabanınızı yalnızca Geliştirme ekibiniz için sık kullanılan olmayacak, ancak veri çözümleyiciler memnuniyet çok.

Şimdi sağlar ODBC sürücüsü ile çalışmaya başlayın.

## <a id="install"></a>1. adım: Azure Cosmos DB ODBC sürücüsünü yükleme

1. Ortamınız için sürücüleri yükleyin:

    | Yükleyici | Desteklenen işletim sistemleri| 
    |---|---| 
    |[Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) 64-bit Windows için| Windows 8.1 veya daha sonra 64 bit sürümleri, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2.| 
    |[Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) 32-bit 64-bit Windows için| Windows 8.1 veya daha sonra 64 bit sürümleri, Windows 8, Windows 7, Windows XP, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ve Windows Server 2003.| 
    |[Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) 32-bit Windows için|Windows 8.1 veya daha sonra 32 bit sürümleri, Windows 8, Windows 7, Windows XP ve Windows Vista.|

    Başlatan MSI dosyası yerel olarak çalıştır **Microsoft Azure Cosmos DB ODBC sürücüsünü Yükleme Sihirbazı'nı**. 
2. ODBC sürücüsü yüklemek için giriş varsayılan kullanarak Yükleme Sihirbazı'nı tamamlayın.
3. Açık **ODBC Veri Kaynağı Yöneticisi** bilgisayarınızda uygulama, bunu yapabilirsiniz yazarak **ODBC veri kaynakları** Windows Arama kutusu. 
    Sürücü tıklayarak yüklendiğini doğrulayabilirsiniz **sürücüleri** sekmesi ve sağlama **Microsoft Azure Cosmos DB ODBC sürücüsü** listelenir.

    ![Azure Cosmos DB ODBC Veri Kaynağı Yöneticisi](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>2. adım: Azure Cosmos DB veritabanınıza bağlanmak

1. Sonra [Azure Cosmos DB ODBC sürücüsünü yükleme](#install), **ODBC Veri Kaynağı Yöneticisi** penceresinde tıklatın **Ekle**. Bir kullanıcı veya sistem DSN oluşturabilirsiniz. Bu örnekte, bir kullanıcı DSN'si oluşturuyorsunuz.
2. İçinde **yeni veri kaynağı oluştur** penceresinde, seçin **Microsoft Azure Cosmos DB ODBC sürücüsü**ve ardından **son**.
3. İçinde **Azure Cosmos DB ODBC sürücüsü SDN Kurulum** penceresinde aşağıdakileri doldurun: 

    ![Azure Cosmos DB ODBC sürücüsü DSN Kurulum penceresi](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Veri kaynağı adı**: ODBC DSN için kendi kolay ad. Bu ad, Azure Cosmos DB hesabınıza benzersiz olduğundan, birden çok hesabı varsa, bu nedenle uygun şekilde adlandırın.
    - **Açıklama**: veri kaynağı kısa bir açıklaması.
    - **Ana bilgisayar**: Azure Cosmos DB hesabınız için URI. Bu Azure portalında Azure Cosmos DB anahtarları sayfasından aşağıdaki ekran görüntüsünde gösterildiği gibi alabilirsiniz. 
    - **Erişim tuşu**: birincil veya ikincil, okuma-yazma veya salt okunur anahtarı Azure Cosmos DB anahtarlardan sayfasında Azure Portalı'nda aşağıdaki ekran görüntüsünde gösterildiği gibi. DSN salt okunur veri işleme ve raporlama için kullanılan salt okunur anahtarı kullanmanızı öneririz.
    ![Azure Cosmos DB anahtarları sayfası](./media/odbc-driver/odbc-driver-keys.png)
    - **Erişim anahtarı şifrelemek**: Bu makinenin kullanıcı temelli en iyi seçenek seçin. 
4. Tıklatın **Test** düğmesi Azure Cosmos DB hesabınıza bağlandığınızdan emin olun. 
5. Tıklatın **Gelişmiş Seçenekler** ve aşağıdaki değerleri ayarlayın:
    - **Sorgu tutarlılık**: seçin [tutarlılık düzeyi](consistency-levels.md) işlemleriniz için. Varsayılan oturumdur.
    - **Yeniden deneme sayısı**: ilk istek hizmet azaltma nedeniyle tamamlanmazsa bir işlem yeniden deneneceğini sayısını girin.
    - **Şema dosyası**: çeşitli seçenekler burada sahip.
        - Bu giriş (boş), olduğu gibi bırakarak varsayılan olarak, ilk sayfa verileri her koleksiyon şeması belirlemek tüm koleksiyonlar için sürücü tarar. Bu koleksiyon eşleme bilinir. Tanımlanan bir şema dosyası olmadan sürücü her bir sürücü oturumu için tarama yapması ve daha yüksek başlangıç saati DSN kullanarak bir uygulama, bir neden olabilir. Bir şema dosyası her zaman için DSN ilişkilendirmek öneririz.
        - Bir şema dosyası zaten varsa (kullanılarak oluşturulan bir büyük olasılıkla [şema Düzenleyicisi](#schema-editor)), tıklayabilirsiniz **Gözat**, dosyaya gidin, tıklatın **kaydetmek**ve ardından **Tamam**.
        - Yeni bir şema oluşturmak istiyorsanız, tıklatın **Tamam**ve ardından **şema Düzenleyicisi** ana penceresinde. İle devam [şema Düzenleyicisi](#schema-editor) bilgi. Yeni şema dosyasını oluşturduktan sonra geri dönüp hatırlamak **Gelişmiş Seçenekler** penceresi yeni oluşturulan şema dosyası içerir.

6. Tamamlayıp kapattığınızda sonra **Azure Cosmos DB ODBC sürücüsü DSN Kurulum** penceresinde, yeni kullanıcı DSN Kullanıcı DSN sekmesine eklenir.

    ![Yeni Azure Cosmos DB ODBC DSN kullanıcı DSN'si sekmesindeki](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>3. adım: Koleksiyon eşleme yöntemini kullanarak bir şema tanımı oluşturma

İki tür kullanabileceğiniz örnekleme yöntemi vardır: **koleksiyonu eşleme** veya **tablo sınırlayıcıları**. Bir örnekleme oturumu örnekleme yöntemlerin her ikisi de kullanabilir, ancak her koleksiyon yalnızca belirli örnekleme yöntemini kullanabilirsiniz. Aşağıdaki adımları verileri için bir şema koleksiyonu eşleme yöntemini kullanarak bir veya daha fazla içinde koleksiyonlar oluşturun. Bu örnekleme yöntemi veri sayfasındaki verilerin yapısını belirlemek için bir koleksiyon alır. Bu, bir tablonun ODBC tarafındaki bir koleksiyona ye dönüştürür. Bir koleksiyondaki veriler homojen olduğunda bu örnekleme verimli ve hızlı yöntemdir. Bir koleksiyon heterogenous türdeki veriler içeriyorsa, şunu kullanmanızı öneririz [tablo sınırlayıcıları yöntemi eşleme](#table-mapping) gibi veri yapılarını koleksiyondaki belirlemek için daha sağlam bir örnekleme yöntemi sağlar. 

1. Adım 1-4'te tamamladıktan sonra [Azure Cosmos DB veritabanına bağlan](#connect), tıklatın **şema Düzenleyicisi** içinde **Azure Cosmos DB ODBC sürücüsü DSN Kurulum** penceresi.

    ![Azure Cosmos DB ODBC sürücüsü DSN Kurulumu penceresinde şema Düzenleyici düğmesi](./media/odbc-driver/odbc-driver-schema-editor.png)
2. İçinde **şema Düzenleyicisi** penceresinde tıklatın **Yeni Oluştur**.
    **Oluşturmak şema** penceresi Azure Cosmos DB hesaptaki tüm koleksiyonları görüntüler. 
3. Örnek ve ardından bir veya daha fazla koleksiyon seçmeniz **örnek**. 
4. İçinde **Tasarım görünümünde** sekmesi, veritabanı, şema ve tablo temsil edilen. Tablo görünümünde tarama sütun adları (SQL adı, kaynak adı, vb.) ile ilişkili özellikler kümesini görüntüler.
    Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü, SQL uzunluğu (varsa), Ölçek (varsa), duyarlık (varsa) ve null atanabilir.
    - Ayarlayabileceğiniz **sütunu gizle** için **true** sorgu sonuçlarındaki bu sütununu hariç tutmak istiyorsanız. Sütunları gizleme sütun işaretli = true hala şemanın bir parçası olsa seçimi ve yansıtma, döndürülmez. Örneğin, "_" ile başlayan tüm Azure Cosmos DB sistem için gerekli özellikler gizleyebilirsiniz.
    - **Kimliği** normalleştirilmiş şemasında birincil anahtar olarak kullanılan gizlenemez yalnızca alan bir sütundur. 
5. Şema tanımlama tamamladıktan sonra tıklatın **dosya** | **kaydetmek**, şemayı kaydetmek için dizine gidin ve ardından **kaydetmek**.

    Gelecekte bu şemayı DSN ile kullanmak istiyorsanız, Azure Cosmos DB ODBC sürücüsü DSN Kurulum penceresi (aracılığıyla ODBC Veri Kaynağı Yöneticisi) açın, Gelişmiş Seçenekler'i ve ardından kaydedilen şemaya şema dosyası kutusuna gidin. Bir şema dosyası var olan bir DSN kaydetme veri ve şema tarafından tanımlanan yapısını kapsamına DSN bağlantısı değiştirir.

## <a id="table-mapping"></a>4. adım: Tablo ayırıcısı kullanarak bir şema tanımı oluşturma yöntemi eşleme

İki tür kullanabileceğiniz örnekleme yöntemi vardır: **koleksiyonu eşleme** veya **tablo sınırlayıcıları**. Bir örnekleme oturumu örnekleme yöntemlerin her ikisi de kullanabilir, ancak her koleksiyon yalnızca belirli örnekleme yöntemini kullanabilirsiniz. 

Aşağıdaki adımları kullanarak bir veya daha fazla koleksiyonda verileri için bir şema oluşturma **tablo sınırlayıcıları** yöntemi eşleme. Koleksiyonunuz heterojen türdeki veriler içerdiğinde bu örnekleme yöntemini kullanmanızı öneririz. Bir dizi öznitelikleri ve karşılık gelen değerler için örnekleme kapsamını belirlemek için bu yöntemi kullanabilirsiniz. Örneğin, bir belgeyi "Type" özelliği içeriyorsa, bu özellik değerlerine örnekleme kapsamını belirleyebilirsiniz. Örnekleme nihai sonucu tablolar, belirttiğiniz türü için değerlerin her biri için bir dizi olacaktır. Örneğin, yazın = araba araba tablosu türü işlenirken oluşturacak = düzlemi bir düz tablo oluşturmak.

1. Adım 1-4'te tamamladıktan sonra [Azure Cosmos DB veritabanına bağlan](#connect), tıklatın **şema Düzenleyicisi** Azure Cosmos DB ODBC sürücüsü DSN Kurulumu penceresinde.
2. İçinde **şema Düzenleyicisi** penceresinde tıklatın **Yeni Oluştur**.
    **Oluşturmak şema** penceresi Azure Cosmos DB hesaptaki tüm koleksiyonları görüntüler. 
3. Bir koleksiyon seçin **Sample View** sekmesinde **eşleme tanımı** sütunu koleksiyon tıklatın **Düzenle**. Ardından **eşleme tanımı** penceresinde, seçin **tablo sınırlayıcıları** yöntemi. Ardından şunları yapın:

    a. İçinde **öznitelikleri** sınırlayıcı özelliğinin adını yazın. Bu örneği için şehir için örnekleme kapsam yapmak istediğiniz ve ENTER tuşuna basın, belgedeki bir özelliktir. 

    b. Yalnızca belirli değerleri girdiğiniz özniteliği için örnekleme kapsam istiyorsanız, öznitelik seçimi kutusunda seçin ve ardından bir değer girin **değeri** kutusu, örneğin, Seattle ve ENTER tuşuna basın. Öznitelikler için birden çok değer eklemeye devam edebilirsiniz. Yalnızca doğru öznitelik değerleri girerken seçili olduğundan emin olun.

    Örneğin dahil, bir **öznitelikleri** değeri Şehir ve istediğiniz Tablonuzun yalnızca bir şehir değer satırlarla New York ve Dubai dahil sınırlamak, öznitelikler kutusu ve New York ve ardından Dubai içinde Şehir girersiniz **değerleri** kutusu.
4. **Tamam**’a tıklayın. 
5. Buna örneklemek istediğiniz koleksiyonları eşleme tanımlarında tamamladıktan sonra **şema Düzenleyicisi** penceresinde tıklatın **örnek**.
     Her sütun için değiştirebileceğiniz sütun SQL adı, SQL türü, SQL uzunluğu (varsa), Ölçek (varsa), duyarlık (varsa) ve null atanabilir.
    - Ayarlayabileceğiniz **sütunu gizle** için **true** sorgu sonuçlarındaki bu sütununu hariç tutmak istiyorsanız. Sütunları gizleme sütun işaretli = true hala şemanın bir parçası olsa seçimi ve yansıtma, döndürülmez. Örneğin, ile başlayan tüm Azure Cosmos DB gerekli sistem özellikleri gizleyebilirsiniz `_`.
    - **Kimliği** normalleştirilmiş şemasında birincil anahtar olarak kullanılan gizlenemez yalnızca alan bir sütundur. 
6. Şema tanımlama tamamladıktan sonra tıklatın **dosya** | **kaydetmek**, şemayı kaydetmek için dizine gidin ve ardından **kaydetmek**.
7. Geri **Azure Cosmos DB ODBC sürücüsü DSN Kurulum** penceresinde tıklatın **Gelişmiş Seçenekler**. Ardından **şema dosyası** kutusuna kaydedilmiş şema dosyasına gidin ve tıklatın **Tamam**. Tıklatın **Tamam** DSN yeniden kaydetmek için. Bu, oluşturduğunuz şema DSN'ye kaydeder. 

## <a name="optional-set-up-linked-server-connection"></a>(İsteğe bağlı) Bağlantılı sunucu bağlantısını Ayarla

Bir bağlı sunucu bağlantısı kurmak ayarlayarak Azure Cosmos DB SQL Server Management Studio (SSMS) gelen sorgulayabilirsiniz.

1. Bölümünde açıklandığı gibi bir sistem veri kaynağı oluşturun [2. adım](#connect), örneğin adlandırılmış `SDS Name`.
2. [SQL Server Management Studio'yu yüklemeniz](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
3. SSMS sorgu Düzenleyicisi'nde bir bağlı sunucu nesnesi oluşturmak `DEMOCOSMOS` aşağıdaki komutlarla veri kaynağı için. Değiştir `DEMOCOSMOS` bağlantılı sunucunuzun adıyla ve `SDS Name` sistemi veri kaynağı adı.

    ```sql
    USE [master]
    GO
    
    EXEC master.dbo.sp_addlinkedserver @server = N'DEMOCOSMOS', @srvproduct=N'', @provider=N'MSDASQL', @datasrc=N'SDS Name'
    
    EXEC master.dbo.sp_addlinkedsrvlogin @rmtsrvname=N'DEMOCOSMOS', @useself=N'False', @locallogin=NULL, @rmtuser=NULL, @rmtpassword=NULL
    
    GO
    ```
    
Yeni bağlantılı sunucu adı görmek için bağlı sunucular listesini yenileyin.

![Bağlantılı sunucu SSMS olarak](./media/odbc-driver/odbc-driver-linked-server-ssms.png)

### <a name="query-linked-database"></a>Bağlantılı veritabanını sorgulama

Bağlantılı veritabanını sorgulamak için bir SSMS sorgusu girin. Bu örnekte, sorgu adlı koleksiyon tablosunda seçer `customers`:

```sql
SELECT * FROM OPENQUERY(DEMOCOSMOS, 'SELECT *  FROM [customers].[customers]')
```

Sorgu yürütün. Sonuç buna benzer olmalıdır:

```
attachments/  1507476156    521 Bassett Avenue, Wikieup, Missouri, 5422   "2602bc56-0000-0000-0000-59da42bc0000"   2015-02-06T05:32:32 +05:00 f1ca3044f17149f3bc61f7b9c78a26df
attachments/  1507476156    167 Nassau Street, Tuskahoma, Illinois, 5998   "2602bd56-0000-0000-0000-59da42bc0000"   2015-06-16T08:54:17 +04:00 f75f949ea8de466a9ef2bdb7ce065ac8
attachments/  1507476156    885 Strong Place, Cassel, Montana, 2069       "2602be56-0000-0000-0000-59da42bc0000"   2015-03-20T07:21:47 +04:00 ef0365fb40c04bb6a3ffc4bc77c905fd
attachments/  1507476156    515 Barwell Terrace, Defiance, Tennessee, 6439     "2602c056-0000-0000-0000-59da42bc0000"   2014-10-16T06:49:04 +04:00      e913fe543490432f871bc42019663518
attachments/  1507476156    570 Ruby Street, Spokane, Idaho, 9025       "2602c156-0000-0000-0000-59da42bc0000"   2014-10-30T05:49:33 +04:00 e53072057d314bc9b36c89a8350048f3
```

> [!NOTE]
> Bağlantılı Cosmos DB sunucusu Dört bölümlü adlandırmayı desteklemiyor. Hata aşağıdaki iletiye benzer döndürülür:

```
Msg 7312, Level 16, State 1, Line 44

Invalid use of schema or catalog for OLE DB provider "MSDASQL" for linked server "DEMOCOSMOS". A four-part name was supplied, but the provider does not expose the necessary interfaces to use a catalog or schema.
``` 

## <a name="optional-creating-views"></a>(İsteğe bağlı) Görünümler oluşturma
Tanımlayın ve görünümler örnekleme işleminin bir parçası oluşturun. Bu görünümler SQL görünümleri eşdeğerdir. Salt okunur ve seçimleri kapsamı olan ve Azure Cosmos DB SQL yansıtmaların tanımlanmalıdır. 

Verileriniz için bir görünüm oluşturmak için **şema Düzenleyicisi'ni** penceresi, **görünüm tanımları** sütunu,'ı tıklatın **Ekle** örnek koleksiyonuna satırda. Ardından **görünüm tanımları** penceresinde aşağıdakileri yapın:
1. Tıklatın **yeni**, örneğin, EmployeesfromSeattleView görünümü için bir ad girin ve ardından **Tamam**.
2. İçinde **düzenleme görünümü** penceresinde, bir Azure Cosmos DB sorgusu girin. Bu Azure Cosmos DB SQL sorgusu, örneğin olmalıdır`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`ve ardından **Tamam**.

Dilediğiniz gibi birçok görünümler oluşturabilirsiniz. İşiniz bittiğinde görünümleri tanımlama, sonra verileri örnek oluşturabilirsiniz. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>5. adım: verilerinizi Power BI Desktop gibi BI araçları görüntülemek

ODBC uyumlu bir araçlarıyla DocumentADB bağlanmak için yeni DSN kullanabilirsiniz - Bu adım yalnızca, Power BI Desktop bağlanmak ve bir Power BI görselleştirmesi oluşturmak nasıl gösterir.

1. Open Power BI Desktop.
2. Tıklatın **veri alma**.
3. İçinde **Veri Al** penceresinde tıklatın **diğer** | **ODBC** | **Bağlan**.
4. İçinde **gelen ODBC** penceresi, veri kaynağının adını ve ardından select **Tamam**. Bırakabilirsiniz **Gelişmiş Seçenekler** girişleri boş.
5. İçinde **ODBC sürücüsü kullanarak bir veri kaynağına erişim** penceresinde, seçin **varsayılan veya özel** ve ardından **Bağlan**. Eklenecek gerekmez **kimlik bilgisi bağlantı dizesi özellikleri**.
6. İçinde **Gezgini** pencerede, sol bölmede, veritabanı şemasını genişletin ve ardından tabloyu seçin. Sonuçlar bölmesinde, oluşturduğunuz Şeması'nı kullanarak verileri içerir.
7. Power BI desktop verileri görselleştirmek için tablo adının önünde kutuyu işaretleyin ve ardından **yük**.
8. Şimdiye kadar üzerinde Power BI Desktop'ta sol, veri sekmesinde seçin ![Power BI Desktop'ta veri sekmesi](./media/odbc-driver/odbc-driver-data-tab.png) Verilerinizi onaylamak için içeri aktarıldı.
9. Artık Power BI rapor sekmesinde tıklayarak kullanan görseller oluşturabilirsiniz ![Power BI Desktop rapor sekmesinde](./media/odbc-driver/odbc-driver-report-tab.png)a **yeni Visual**ve ardından kutucuğunuzun özelleştirme. Power BI Desktop'ta görselleştirmeleri oluşturma hakkında daha fazla bilgi için bkz: [görselleştirme türlerinin Power bı'da](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki hata iletisini alırsanız olun **konak** ve **erişim tuşu** Azure portalında kopyalanan değerleri [2. adım](#connect) doğru olduğunu onaylayın ve ardından yeniden deneyin. Sağındaki kopyalama düğmelerini kullanın **konak** ve **erişim tuşu** değerler hatasız kopyalamak için Azure portalında değerleri.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"The input authorization token can't serve the request. Please check that the expected payload is built as per the protocol, and check the key being used. Server used the following payload to sign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB hakkında daha fazla bilgi için bkz: ['na Hoş Geldiniz Azure Cosmos DB](introduction.md).
