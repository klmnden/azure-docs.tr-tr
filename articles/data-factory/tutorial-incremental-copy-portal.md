---
title: Azure Data Factory kullanarak bir tabloyu artımlı olarak kopyalama | Microsoft Docs
description: Bu öğreticide, verileri Azure SQL veritabanından Azure Blob depolama alanına artımlı olarak kopyalayan bir Azure veri fabrikası işlem hattı oluşturacaksınız.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/11/2018
ms.author: yexu
ms.openlocfilehash: 1bc4bd9b95dc7e45b9b90fbe096ed71c5aa9bedf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60571507"
---
# <a name="incrementally-load-data-from-an-azure-sql-database-to-azure-blob-storage"></a>Azure SQL veritabanından Azure Blob depolama alanına verileri artımlı olarak yükleme
Bu öğreticide, Azure SQL veritabanındaki bir tablodan Azure Blob depolama alanına delta veri yükleyen işlem hattına sahip bir Azure veri fabrikası oluşturacaksınız. 

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Eşik değerini depolamak için veri deposunu hazırlama.
> * Veri fabrikası oluşturma.
> * Bağlı hizmet oluşturma. 
> * Kaynak, havuz ve eşik veri kümeleri oluşturun.
> * İşlem hattı oluşturma.
> * İşlem hattını çalıştırma.
> * İşlem hattı çalıştırmasını izleme. 
> * Sonuçları gözden geçirme
> * Kaynağa daha fazla veri ekleyin.
> * İşlem hattını yeniden çalıştırın.
> * İkinci işlem hattı çalıştırmasını izleme
> * İkinci çalıştırmanın sonuçlarını gözden geçirme


## <a name="overview"></a>Genel Bakış
Yüksek düzeyli çözüm diyagramı aşağıdaki gibidir: 

![Artımlı olarak veri yükleme](media/tutorial-Incremental-copy-portal/incrementally-load.png)

Bu çözümü oluşturmak için önemli adımlar şunlardır: 

1. **Eşit sütununu seçin**.
    Kaynak veri deposunda her çalıştırma için yeni veya güncelleştirilmiş kayıtları dilimlemek için kullanılabilen bir sütun seçin. Normalde, satırlar oluşturulduğunda veya güncelleştirildiğinde seçilen bu sütundaki veriler (örneğin, last_modify_time veya kimlik) artmaya devam eder. Bu sütundaki en büyük değer eşik olarak kullanılır.

2. **Eşik değerini depolamak için veri deposunu hazırlayın**. Bu öğreticide, eşik değerini bir SQL veritabanında depolayacaksınız.
    
3. **Aşağıdaki iş akışı ile bir işlem hattı oluşturun**: 
    
    Bu çözümdeki işlem hattı aşağıdaki etkinlikleri içerir:
  
    * İki Arama etkinliği oluşturun. Son eşik değerini almak için ilk Arama etkinliğini kullanın. Yeni eşik değerini almak için ikinci Arama etkinliğini kullanın. Bu eşik değerleri, Kopyalama etkinliğine geçirilir. 
    * Eşik sütununun değeri eski eşik değerinden büyük ve yeni eşik değerinden küçük olacak şekilde, satırları kaynak veri deposundan kopyalayan bir Kopyalama etkinliği oluşturun. Ardından, delta veriler kaynak veri deposundan Blob depolama alanına yeni bir dosya olarak kopyalanır. 
    * Sonraki seferde çalışan işlem hattı için eşik değerini güncelleştiren bir StoredProcedure etkinliği oluşturun. 


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Önkoşullar
* **Azure SQL Veritabanı**. Veritabanını kaynak veri deposu olarak kullanabilirsiniz. SQL veritabanınız yoksa, oluşturma adımları için bkz. [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).
* **Azure Depolama**. Blob depolamayı havuz veri deposu olarak kullanabilirsiniz. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md). adftutorial adlı bir kapsayıcı oluşturun. 

### <a name="create-a-data-source-table-in-your-sql-database"></a>SQL veritabanınızda bir veri kaynağı tablosu oluşturma
1. SQL Server Management Studio’yu açın. **Sunucu Gezgini**’nde veritabanına sağ tıklayın ve **Yeni Sorgu**’yu seçin.

2. SQL veritabanınızda aşağıdaki SQL komutunu çalıştırarak veri kaynağı deponuz olarak `data_source_table` adlı bir tablo oluşturun: 
    
    ```sql
    create table data_source_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );

    INSERT INTO data_source_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'aaaa','9/1/2017 12:56:00 AM'),
    (2, 'bbbb','9/2/2017 5:23:00 AM'),
    (3, 'cccc','9/3/2017 2:36:00 AM'),
    (4, 'dddd','9/4/2017 3:21:00 AM'),
    (5, 'eeee','9/5/2017 8:06:00 AM');
    ```
    Bu öğreticide eşik sütunu olarak LastModifytime kullanılır. Veri kaynağı deposundaki veriler aşağıdaki tabloda gösterilmiştir:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    ```

### <a name="create-another-table-in-your-sql-database-to-store-the-high-watermark-value"></a>Üst eşik değerini depolamak için SQL veritabanınızda başka bir tablo oluşturma
1. SQL veritabanınızda aşağıdaki SQL komutunu çalıştırarak eşik değerini depolamak için `watermarktable` adlı bir tablo oluşturun:  
    
    ```sql
    create table watermarktable
    (
    
    TableName varchar(255),
    WatermarkValue datetime,
    );
    ```
2. Üst eşiğin varsayılan değerini kaynak veri deposunun tablo adıyla ayarlayın. Bu öğreticide tablo adı data_source_table şeklindedir.

    ```sql
    INSERT INTO watermarktable
    VALUES ('data_source_table','1/1/2010 12:00:00 AM')    
    ```
3. `watermarktable` tablosundaki verileri gözden geçirin.
    
    ```sql
    Select * from watermarktable
    ```
    Çıkış: 

    ```
    TableName  | WatermarkValue
    ----------  | --------------
    data_source_table | 2010-01-01 00:00:00.000
    ```

### <a name="create-a-stored-procedure-in-your-sql-database"></a>SQL veritabanınızda bir saklı yordam oluşturma 

SQL veritabanınızda bir saklı yordam oluşturmak için aşağıdaki komutu çalıştırın:

```sql
CREATE PROCEDURE usp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN
    
    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName
    
END
```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. **Yeni veri fabrikası** sayfasında **ad** için **ADFIncCopyTutorialDF** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-incremental-copy-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayla birlikte kırmızı bir ünlem işareti görürseniz veri fabrikasının adını değiştirin (örneğin adınızADFIncCopyTutorialDF) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name "ADFIncCopyTutorialDF" is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
        Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2**'yi seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.      
8. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-incremental-copy-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-incremental-copy-portal/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu öğreticide tek işlem hattında zincirlenmiş iki Arama etkinliği, bir Kopyalama etkinliği ve bir StoredProcedure etkinliği ile işlem hattı oluşturacaksınız. 

1. Data Factory kullanıcı arabiriminin **başlarken** sayfasında **İşlem hattı oluştur** kutucuğuna tıklayın. 

   ![Data Factory kullanıcı arabiriminin başlarken sayfası](./media/tutorial-incremental-copy-portal/get-started-page.png)    
3. İşlem hattının **Özellikler** penceresinin **Genel** sayfasında **IncrementalCopyPipeline** adını girin. 

   ![İşlem hattı adı](./media/tutorial-incremental-copy-portal/pipeline-name.png)
4. Eski filigran değerini almak için ilk arama etkinliğini ekleyelim. **Etkinlikler** araç kutusunda **Genel**’i genişletin ve bir **Arama** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın. Etkinliğin adını **LookupOldWaterMarkActivity** olarak değiştirin.

   ![İlk arama etkinliği - ad](./media/tutorial-incremental-copy-portal/first-lookup-name.png)
5. **Ayarlar** sekmesine geçin ve **Kaynak Veri Kümesi** için **+ Yeni**’ye tıklayın. Bu adımda, **filigran tablosundaki** verileri temsil eden bir veri kümesi oluşturursunuz. Bu tablo, önceki kopyalama işleminde kullanılan eski filigranı içerir. 

   ![Yeni veri kümesi menüsü - eski filigran](./media/tutorial-incremental-copy-portal/new-dataset-old-watermark.png)
6. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçip **Son**’a tıklayın. Veri kümesi için yeni bir sekme açıldığını görürsünüz. 

   ![Azure SQL Veritabanı’nı seçin](./media/tutorial-incremental-copy-portal/select-azure-sql-database-old-watermark.png)
7. Veri kümesinin özellikler penceresinde, **Ad** için **WatermarkDataset** adını girin.

   ![Filigran veri kümesi - ad](./media/tutorial-incremental-copy-portal/watermark-dataset-name.png)
8. Azure SQL veritabanınıza yönelik bir bağlantı oluşturmak (bağlı hizmet oluşturmak) için **Bağlantı** sekmesine geçin ve **+ Yeni**’ye tıklayın. 

   ![Yeni bağlı hizmet düğmesi](./media/tutorial-incremental-copy-portal/watermark-dataset-new-connection-button.png)
9. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    1. **Ad** için **AzureSqlDatabaseLinkedService** adını girin. 
    2. **Sunucu adı** için Azure SQL Server’ınızı seçin.
    3. Azure SQL Server’a erişim için **kullanıcı adını** girin. 
    4. Kullanıcının **parolasını** girin. 
    5. Azure SQL veritabanı bağlantısını test etmek için **Bağlantıyı sına**’ya tıklayın.
    6. **Kaydet**’e tıklayın.
    7. **Bağlantı** sekmesinde **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** seçeneğinin belirlendiğinden emin olun.
       
        ![Yeni bağlı hizmet penceresi](./media/tutorial-incremental-copy-portal/azure-sql-linked-service-settings.png)
10. **Tablo** için **[dbo].[watermarktable]** seçeneğini belirleyin. Tablodaki verilerin önizlemesini yapmak istiyorsanız **Veri önizlemesini görüntüle**’ye tıklayın.

    ![Filigran veri kümesi - bağlantı ayarları](./media/tutorial-incremental-copy-portal/watermark-dataset-connection-settings.png)
11. Üstteki işlem hattı sekmesine veya soldaki ağaç görünümünden işlem hattının adına tıklayarak işlem hattı düzenleyicisine geçin. **Arama** etkinliğinin özellikler penceresinde **Kaynak Veri Kümesi** alanı için **WatermarkDataset** seçeneğinin belirlendiğinden emin olun. 

    ![İşlem hattı - eski filigran veri kümesi](./media/tutorial-incremental-copy-portal/pipeline-old-watermark-dataset-selected.png)
12. **Etkinlikler** araç kutusunda **Genel**’i genişletip başka bir **Arama** etkinliğini işlem hattı tasarımcısının yüzeyine sürükleyin ve özellikler penceresinin **Genel** sekmesinde adı **LookupNewWaterMarkActivity** olarak ayarlayın. Bu Arama etkinliği, kaynak verileri içeren tablodan hedefe kopyalanacak yeni filigran değerini alır. 

    ![İkinci arama etkinliği - ad](./media/tutorial-incremental-copy-portal/second-lookup-activity-name.png)
13. İkinci **Arama** etkinliğinin özellikler penceresinde **Ayarlar** sekmesine geçip **Yeni**’ye tıklayın. Yeni filigran değerini (en yüksek LastModifyTime değeri) içeren kaynak tabloyu gösteren bir veri kümesi oluşturursunuz. 

    ![İkinci arama etkinliği - yeni veri kümesi](./media/tutorial-incremental-copy-portal/second-lookup-activity-settings-new-button.png)
14. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçip **Son**’a tıklayın. Bu veri kümesi için yeni bir sekme açıldığını görürsünüz. Ayrıca, veri kümesini ağaç görünümünde de görebilirsiniz. 
15. Özellikler penceresinin **Genel** sekmesinde **Ad** için **SourceDataset** adını girin. 

    ![Kaynak veri kümesi - ad](./media/tutorial-incremental-copy-portal/source-dataset-name.png)
16. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** hizmetini seçin.
    2. Tablo için **[dbo].[data_source_table]** tablosunu seçin. Bu öğreticinin sonraki bölümlerinde, bu veri kümesinde bir sorgu belirtirsiniz. Bu sorgu, bu adımda belirttiğiniz tablodan önceliklidir. 

        ![İkinci arama etkinliği - yeni veri kümesi](./media/tutorial-incremental-copy-portal/source-dataset-connection.png)
17. Üstteki işlem hattı sekmesine veya soldaki ağaç görünümünden işlem hattının adına tıklayarak işlem hattı düzenleyicisine geçin. **Arama** etkinliğinin özellikler penceresinde **Kaynak Veri Kümesi** alanı için **SourceDataset** seçeneğinin belirlendiğinden emin olun. 
18. **Sorgu Kullan** alanı için **Sorgu**’yu seçin ve aşağıdaki sorguyu girin: **data_source_table** tablosundan yalnızca en yüksek **LastModifytime** değerini seçersiniz. Bu sorgu olmadığında, veri kümesi tanımında tablo adını (data_source_table) belirttiğiniz için veri kümesi tablodaki tüm satırları alır.

    ```sql
    select MAX(LastModifytime) as NewWatermarkvalue from data_source_table
    ```

    ![İkinci arama etkinliği - sorgu](./media/tutorial-incremental-copy-portal/query-for-new-watermark.png)
19. **Etkinlikler** araç kutusunda **Veri Akışı**’nı genişletin ve Etkinlikler araç kutusundan **Kopyalama** etkinliğini sürükleyip adı **IncrementalCopyActivity** olarak ayarlayın. 

    ![Kopyalama etkinliği - ad](./media/tutorial-incremental-copy-portal/copy-activity-name.png)
20. Arama etkinliklerine bağlı **yeşil düğmeyi** Kopyalama etkinliğine sürükleyerek **her iki Arama etkinliğini Kopyalama etkinliğine bağlayın**. Kopyalama etkinliğinin kenarlık renginin mavi olduğunu gördüğünüzde fare düğmesini bırakın. 

    ![Arama etkinliklerini Kopyalama etkinliklerine bağlama](./media/tutorial-incremental-copy-portal/connection-lookups-to-copy.png)
21. **Kopyalama etkinliğini** seçin ve **Özellikler** penceresinde etkinliğin özelliklerini gördüğünüzü onaylayın. 

    ![Kopyalama etkinliğinin özellikleri](./media/tutorial-incremental-copy-portal/back-to-copy-activity-properties.png)
22. **Özellikler** penceresinde **Kaynak** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Kaynak Veri Kümesi** alanı için **SourceDataset**’i seçin. 
    2. **Sorgu Kullan** alanı için **Sorgu**’yu seçin. 
    3. **Sorgu** alanı için aşağıdaki SQL sorgusunu girin. 

        ```sql
        select * from data_source_table where LastModifytime > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and LastModifytime <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'
        ```
    
        ![Kopyalama etkinliği - kaynak](./media/tutorial-incremental-copy-portal/copy-activity-source.png)
23. **Havuz** sekmesine geçin ve **Havuz Veri Kümesi** alanı için **+ Yeni**’ye tıklayın. 

    ![Yeni Veri Kümesi düğmesi](./media/tutorial-incremental-copy-portal/new-sink-dataset-button.png)
24. Bu öğreticide, havuz veri deposu Azure Blob Depolama türündedir. Bu nedenle, **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama**’yı seçin ve **son**’a tıklayın. 

    ![Azure Blob Depolama’yı seçin](./media/tutorial-incremental-copy-portal/select-azure-blob-storage.png)
25. Özellikler penceresinin **Genel** sekmesinde, **Ad** için **SinkDataset** adını girin. 

    ![Havuz Veri Kümesi - ad](./media/tutorial-incremental-copy-portal/sink-dataset-name.png)
26. **Bağlantı** sekmesine geçip **+ Yeni**’ye tıklayın. Bu adımda, **Azure Blob Depolama**’nıza yönelik bir bağlantı (bağlı hizmet) oluşturursunuz.

    ![Havuz Veri Kümesi - yeni bağlantı](./media/tutorial-incremental-copy-portal/sink-dataset-new-connection.png)
26. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın: 

    1. **Ad** için **AzureStorageLinkedService** adını girin. 
    2. **Depolama hesabı adı** için Azure Depolama hesabınızı seçin.
    3. **Kaydet**’e tıklayın. 

        ![Azure Depolama Bağlı hizmeti - ayarlar](./media/tutorial-incremental-copy-portal/azure-storage-linked-service-settings.png)
27. **Bağlantı** sekmesinde aşağıdaki adımları uygulayın:

    1. **Bağlı hizmet** için **AzureStorageLinkedService**’in seçildiğini onaylayın. 
    2. **Dosya yolu**’nun **klasör** bölümü için **adftutorial/incrementalcopy** yolunu girin. **adftutorial** blob kapsayıcısı adı, **incrementalcopy** klasör adıdır. Bu kod parçacığı blob depolama hesabınızda adftutorial adlı bir blob kapsayıcıya sahip olduğunuzu varsayar. Henüz yoksa kapsayıcıyı oluşturun veya var olan bir kapsayıcının adına ayarlayın. **incrementalcopy** adlı çıktı dosyası mevcut değilse Azure Data Factory tarafından otomatik olarak oluşturulur. Bir blob kapsayıcısındaki klasörlerden birine gitmek istiyorsanız **Dosya yolu** için **Gözat** düğmesini de kullanabilirsiniz. .RunId, '.txt')`.
    3. **Dosya yolu** alanının **dosya adı** bölümü için `@CONCAT('Incremental-', pipeline().RunId, '.txt')` adını kullanın. Dosya adı, ifade kullanılarak dinamik olarak oluşturulur. Her işlem hattı çalıştırması benzersiz bir kimliğe sahiptir. Kopyalama etkinliği, dosya adını oluşturmak için çalışma kimliğini kullanır. 

        ![Havuz Veri Kümesi - bağlantı ayarları](./media/tutorial-incremental-copy-portal/sink-dataset-connection-settings.png)
28. Üstteki işlem hattı sekmesine veya soldaki ağaç görünümünden işlem hattının adına tıklayarak **işlem hattı** düzenleyicisine geçin. 
29. **Etkinlikler** araç kutusunda **Genel**’i genişletin ve **Etkinlikler** araç kutusundan **Saklı Yordam** etkinliğini sürükleyip işlem hattı tasarımcısının yüzeyine bırakın. **Kopyalama** etkinliğinin yeşil (Başarılı) çıktısını **Saklı Yordam** etkinliğine **bağlayın**. 
    
    ![Kopyalama etkinliği - kaynak](./media/tutorial-incremental-copy-portal/connect-copy-to-stored-procedure-activity.png)
24. İşlem hattı tasarımcısında **Saklı Yordam Etkinliğini** seçip etkinliğin adını **StoredProceduretoWriteWatermarkActivity** olarak değiştirin. 

    ![Saklı Yordam Etkinliği - ad](./media/tutorial-incremental-copy-portal/stored-procedure-activity-name.png)
25. **SQL Hesabı** sekmesine geçin ve **Bağlı hizmet** için *AzureSqlDatabaseLinkedService** seçeneğini belirleyin. 

    ![Saklı Yordam Etkinliği - SQL Hesabı](./media/tutorial-incremental-copy-portal/sp-activity-sql-account-settings.png)
26. **Saklı Yordam** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. İçin **saklı yordam adı**seçin **usp_write_watermark**. 
    2. Saklı yordam parametrelerinin değerlerini belirtmek için, **Parametreyi içeri aktar**’a tıklayın ve parametreler için aşağıdaki değerleri girin: 

        | Ad | Tür | Değer | 
        | ---- | ---- | ----- | 
        | LastModifiedtime | DateTime | @{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue} |
        | TableName | String | @{activity('LookupOldWaterMarkActivity').output.firstRow.TableName} |

    ![Saklı Yordam Etkinliği - saklı yordam ayarları](./media/tutorial-incremental-copy-portal/sproc-activity-stored-procedure-settings.png)
27. İşlem hattı ayarlarını doğrulamak için araç çubuğunda **Doğrula**’ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu** penceresini kapatmak için >> seçeneğine tıklayın.   

    ![İşlem hattını doğrulama](./media/tutorial-incremental-copy-portal/validate-pipeline.png)
28. **Tümünü Yayımla** düğmesine tıklayarak varlıkları (bağlı hizmetler, veri kümeleri ve işlem hatları) Azure Data Factory hizmetinde yayımlayın. Yayımlamanın başarılı olduğunu belirten bir ileti görene kadar bekleyin. 

    ![Yayımla düğmesi](./media/tutorial-incremental-copy-portal/publish-button.png)

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme
1. Araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın. 

    ![Şimdi Tetikle düğmesi](./media/tutorial-incremental-copy-portal/trigger-now.png)
2. **İşlem Hattı Çalıştırma** penceresinde **Son**’u seçin. 

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. Soldaki **İzleyici** sekmesine geçin. El ile tetikleme işlemi tarafından tetiklenen işlem hattı çalıştırmasının durumunu görebilirsiniz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. 
    
    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-portal/pipeline-runs.png)
2. Bu işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki ilk bağlantıya (**Etkinlik Çalıştırmalarını Görüntüle**) tıklayın. Üst taraftan **İşlem Hatları**’na tıklayarak önceki görünüme dönebilirsiniz. Listeyi yenilemek için **Yenile** düğmesine tıklayın.

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-portal/activity-runs.png)

## <a name="review-the-results"></a>Sonuçları gözden geçirin
1. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi araçları kullanarak Azure Depolama Hesabınıza bağlanın. **adftutorial** kapsayıcısının **incrementalcopy** klasöründe bir çıktı dosyası oluşturulduğunu doğrulayın.

    ![İlk çıktı dosyası](./media/tutorial-incremental-copy-portal/first-output-file.png)
2. Çıktı dosyasını açın ve **data_source_table** tablosundaki tüm verilerin blob dosyasına kopyalandığına dikkat edin. 

    ```
    1,aaaa,2017-09-01 00:56:00.0000000
    2,bbbb,2017-09-02 05:23:00.0000000
    3,cccc,2017-09-03 02:36:00.0000000
    4,dddd,2017-09-04 03:21:00.0000000
    5,eeee,2017-09-05 08:06:00.0000000
    ```
3. `watermarktable` içindeki en son değeri denetleyin. Eşik değerinin güncelleştirildiğini görürsünüz.

    ```sql
    Select * from watermarktable
    ```
    
    Çıktı şöyledir:
 
    | TableName | WatermarkValue |
    | --------- | -------------- |
    | data_source_table | 2017-09-05    8:06:00.000 | 

## <a name="add-more-data-to-source"></a>Kaynağa daha fazla veri ekleme

Yeni verileri SQL veritabanına (veri kaynağı deposu) ekleyin.

```sql
INSERT INTO data_source_table
VALUES (6, 'newdata','9/6/2017 2:23:00 AM')

INSERT INTO data_source_table
VALUES (7, 'newdata','9/7/2017 9:01:00 AM')
``` 

SQL veritabanında güncelleştirilmiş veriler:

```
PersonID | Name | LastModifytime
-------- | ---- | --------------
1 | aaaa | 2017-09-01 00:56:00.000
2 | bbbb | 2017-09-02 05:23:00.000
3 | cccc | 2017-09-03 02:36:00.000
4 | dddd | 2017-09-04 03:21:00.000
5 | eeee | 2017-09-05 08:06:00.000
6 | newdata | 2017-09-06 02:23:00.000
7 | newdata | 2017-09-07 09:01:00.000
```


## <a name="trigger-another-pipeline-run"></a>Başka bir işlem hattı çalıştırması tetikleme
1. **Düzenle** sekmesine geçin. Tasarımcıda açık değilse ağaç görünümünden işlem hattına tıklayın. 

    ![Şimdi Tetikle düğmesi](./media/tutorial-incremental-copy-portal/edit-tab.png)
2. Araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın. 

    ![Şimdi Tetikle düğmesi](./media/tutorial-incremental-copy-portal/trigger-now.png)

## <a name="monitor-the-second-pipeline-run"></a>İkinci işlem hattı çalıştırmasını izleme

1. Soldaki **İzleyici** sekmesine geçin. El ile tetikleme işlemi tarafından tetiklenen işlem hattı çalıştırmasının durumunu görebilirsiniz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. 
    
    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-portal/pipeline-runs-2.png)
2. Bu işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki ilk bağlantıya (**Etkinlik Çalıştırmalarını Görüntüle**) tıklayın. Üst taraftan **İşlem Hatları**’na tıklayarak önceki görünüme dönebilirsiniz. Listeyi yenilemek için **Yenile** düğmesine tıklayın.

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-portal/activity-runs-2.png)

## <a name="verify-the-second-output"></a>İkinci çıkışı doğrulama

1. Blob depolama alanında başka bir dosyanın oluşturulduğunu görürsünüz. Bu öğreticide yeni dosya adı `Incremental-<GUID>.txt` şeklindedir. Bu dosyayı açın, içinde 2 satır kaydı göreceksiniz.

    ```
    6,newdata,2017-09-06 02:23:00.0000000
    7,newdata,2017-09-07 09:01:00.0000000    
    ```
2. `watermarktable` içindeki en son değeri denetleyin. Eşik değerinin tekrar güncelleştirildiğini görürsünüz.

    ```sql
    Select * from watermarktable
    ```
    örnek çıktı: 
    
    | TableName | WatermarkValue |
    | --------- | --------------- |
    | data_source_table | 2017-09-07 09:01:00.000 |


     
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Eşik değerini depolamak için veri deposunu hazırlama.
> * Veri fabrikası oluşturma.
> * Bağlı hizmet oluşturma. 
> * Kaynak, havuz ve eşik veri kümeleri oluşturun.
> * İşlem hattı oluşturma.
> * İşlem hattını çalıştırma.
> * İşlem hattı çalıştırmasını izleme. 
> * Sonuçları gözden geçirme
> * Kaynağa daha fazla veri ekleyin.
> * İşlem hattını yeniden çalıştırın.
> * İkinci işlem hattı çalıştırmasını izleme
> * İkinci çalıştırmanın sonuçlarını gözden geçirme

Bu öğreticide, işlem hattı SQL veritabanındaki tek bir tablodan Blob depolama alanına veri kopyalamıştır. Şirket içi SQL Server veritabanındaki birden çok tablodan SQL veritabanına veri kopyalama hakkında bilgi edinmek için şu öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
>[SQL Server’daki birden fazla tablodan Azure SQL Veritabanı’na artımlı olarak veri yükleme](tutorial-incremental-copy-multiple-tables-portal.md)



