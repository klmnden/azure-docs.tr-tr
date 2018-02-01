---
title: Azure Data Factory ile verileri toplu olarak kopyalama | Microsoft Docs
description: "Azure Data Factory ve Kopyalama Etkinliği’ni kullanarak bir kaynak veri deposundan hedef veri deposuna toplu veri kopyalama hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 6aa5d4aa032ef4dc3583bf76b9c451874b74f9a6
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama
Bu öğreticide **Azure SQL Veritabanından Azure SQL Veri Ambarı'na birkaç tabloyu kopyalama** işlemi gösterilmektedir. Aynı düzeni diğer kopyalama senaryolarında da uygulayabilirsiniz. Örneğin, SQL Server/Oracle’dan Azure SQL Veritabanı/Veri Ambarı/Azure Blob’a tablo kopyalama, Blob’dan Azure SQL Veritabanı tablolarına farklı yollar kopyalama.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
> - Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Yüksek düzeyde, bu öğretici aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Depolama bağlı hizmetleri oluşturma.
> * Azure SQL Veritabanı ve Azure SQL Veri Ambarı veri kümeleri oluşturma.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturma. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Bu öğreticide Azure Portal kullanılır. Veri fabrikası oluşturmaya yönelik diğer araçlar/SDK’lar hakkında bilgi edinmek için bkz. [Hızlı Başlangıçlar](quickstart-create-data-factory-dot-net.md). 

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı
Bu senaryoda, Azure SQL Veritabanında SQL Veri Ambarı’na kopyalamak istediğiniz birkaç tablo vardır. İş akışının işlem hatlarında gerçekleşen adımlarının mantıksal sırası şöyledir:

![İş akışı](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* İlk işlem hattı, havuz veri depolarına kopyalanması gereken tabloların listesini arar.  Alternatif olarak, havuz veri deposuna kopyalanacak tüm tabloları listeleyen bir meta veri tablosu tutabilirsiniz. İşlem hattı daha sonra veritabanındaki her bir tabloda yinelenen ve veri kopyalama işlemini gerçekleştiren başka bir işlem hattını tetikler.
* İkinci işlem hattı gerçek kopyalama işlemini gerçekleştirir. Tablo listesini bir parametre olarak alır. Listedeki her tablo için, en iyi performansı elde etmek üzere [Blob depolama ve PolyBase yoluyla hazırlanan kopyayı](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) kullanarak Azure SQL Veritabanındaki ilgili tabloyu SQL Veri Ambarında karşılık gelen tabloya kopyalayın. Bu örnekte, ilk işlem hattı tablo listesini bir parametre değeri olarak geçirir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı**. Azure Depolama hesabı, toplu kopyalama işleminde hazırlama blob depolama alanı olarak kullanılır. 
* **Azure SQL Veritabanı**. Bu veritabanı, kaynak verileri içerir. 
* **Azure SQL Veri Ambarı**. Bu veri ambarı, SQL Veritabanından kopyalanan verileri tutar. 

### <a name="prepare-sql-database-and-sql-data-warehouse"></a>SQL Veritabanı ve SQL Veri Ambarı’nı hazırlama

**Kaynak Azure SQL Veritabanı’nı hazırlama**:

[Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesini izleyerek Adventure Works LT örnek verileriyle bir Azure SQL Veritabanı oluşturun. Bu öğretici, bu örnek veritabanındaki tüm tabloları SQL ambarına kopyalar.

**Havuz Azure SQL Veri Ambarını hazırlama**:

1. Azure SQL Veri Ambarınız yoksa, oluşturma adımları için [Azure SQL Veri Ambarı oluşturma](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) makalesine bakın.

2. SQL Veri Ambarında karşılık gelen tablo şemalarını oluşturun. Azure SQL Veritabanından Azure SQL Veri Ambarına **şema geçirmek** için [Geçiş Aracı](https://www.microsoft.com/download/details.aspx?id=49100)’nı kullanabilirsiniz. Daha sonraki bir adımda verileri geçirmek/kopyalamak için Azure Data Factory’yi kullanın.

## <a name="azure-services-to-access-sql-server"></a>SQL sunucusuna erişime yönelik Azure hizmetleri

Hem SQL Veritabanı hem de SQL Veri Ambarı için Azure hizmetlerinin SQL sunucusuna erişmesine izin verin. **Azure hizmetlerine erişime izin ver** ayarının Azure SQL sunucusunda **AÇIK** olduğundan emin olun. Bu ayar, Data Factory hizmetinin Azure SQL Veritabanınızdan verileri okumasına ve Azure SQL Veri Ambarına veri yazmasına olanak tanır. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

1. Soldaki **Diğer hizmetler** hub’ına ve sonra **SQL sunucuları**’na tıklayın.
2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
3. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-bulk-copy-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialBulkCopyDF** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-bulk-copy-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialBulkCopyDF). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name “ADFTutorialBulkCopyDF” is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Data Factory V2 şu anda Doğu ABD, Doğu ABD 2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//tutorial-bulk-copy-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
    ![Data factory giriş sayfası](./media/tutorial-bulk-copy-portal/data-factory-home-page.png)
10. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın.
11. **Başlarken** sayfasında, aşağıdaki resimde gösterildiği gibi sol bölmede bulunan **Düzenle** sekmesine geçin:  

    ![Başlarken sayfası](./media/tutorial-bulk-copy-portal/get-started-page.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlemlerinizi veri fabrikasına bağlamak için bağlı hizmetler oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için kullandığı bağlantı bilgilerini içerir. 

Bu öğreticide, Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Blob Depolama Alanı veri depolarınızı veri fabrikanıza bağlarsınız. Azure SQL Veritabanı, kaynak veri deposudur. Azure SQL Veri Ambarı, havuz/hedef veri deposudur. Azure Blob Depolama Alanı, PolyBase kullanılarak veriler SQL Veri Ambarı'na yüklenmeden önce verilerin hazırlanmasıdır. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Kaynak Azure SQL Veritabanı bağlı hizmetini oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. 

1. Pencerenin en altından **Bağlantılar**’a ve sonra araç çubuğunda **+ Yeni**’ye tıklayın. 

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-bulk-copy-portal/new-linked-service-button.png)
2. **Yeni Bağlı Hizmet** penceresinde **Azure SQL Veritabanı**’nı seçip **Devam**’a tıklayın. 

    ![Azure SQL Veritabanı’nı seçin](./media/tutorial-bulk-copy-portal/select-azure-sql-database.png)
3. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureSqlDatabaseLinkedService** adını girin. 
    2. **Sunucu adı** için Azure SQL sunucunuzu seçin.
    3. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    4. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    5. Kullanıcının **parolasını** girin. 
    6. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
    7. **Kaydet**’e tıklayın.

        ![Azure SQL Veritabanı ayarları](./media/tutorial-bulk-copy-portal/azure-sql-database-settings.png)

### <a name="create-the-sink-azure-sql-data-warehouse-linked-service"></a>Havuz Azure SQL Veri Ambarı bağlı hizmetini oluşturma

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
2. **Yeni Bağlı Hizmet** penceresinde **Azure SQL Veri Ambarı**’nı seçip **Devam**’a tıklayın. 
3. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureSqlDWLinkedService** adını girin. 
    2. **Sunucu adı** için Azure SQL sunucunuzu seçin.
    3. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    4. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    5. Kullanıcının **parolasını** girin. 
    6. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
    7. **Kaydet**’e tıklayın.

### <a name="create-the-staging-azure-storage-linked-service"></a>Hazırlama Azure Depolama bağlı hizmetini oluşturma
Bu öğreticide Azure Blob depolamayı daha iyi bir kopyalama performansı için PolyBase’i etkinleştiren geçici bir hazırlama alanı olarak kullanırsınız.

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
2. **Yeni Bağlı Hizmet** penceresinde **Azure Blob Depolama**’yı seçip **Devam**’a tıklayın. 
3. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureStorageLinkedService** adını girin. 
    2. **Depolama hesabı adı** için **Azure Depolama hesabınızı** seçin.
    4. **Kaydet**’e tıklayın.


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu öğreticide, verilerin depolandığı konumu belirten kaynak ve havuz veri kümelerini oluşturacaksınız. 

AzureSqlDatabaseDataset giriş veri kümesi, AzureSqlDatabaseLinkedService'e işaret eder. Bağlı hizmet, veritabanına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, kaynak verileri içeren veritabanı ve tablonun adını belirtir. 

AzureSqlDWDataset çıkış veri kümesi AzureSqlDWLinkedService'e işaret eder. Bağlı hizmet, veri ambarına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, verilerin kopyalandığı hedef veritabanı ve tabloyu belirtir. 

Bu öğreticide, kaynak ve hedef SQL tabloları veri kümesi tanımında sabit kodlanmamıştır. Bunun yerine, ForEach etkinliği tablonun adını çalışma zamanında Kopyalama etkinliğine geçirir. 

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 

    ![Yeni veri kümesi menüsü](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
2. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve **Son**’a tıklayın. **AzureSqlTable1** başlıklı yeni bir sekme görüyor olmalısınız. 
    
    ![Azure SQL Veritabanı’nı seçin](./media/tutorial-bulk-copy-portal/select-azure-sql-database-dataset.png)
3. En alttaki Özellikler penceresinde, **Ad** olarak **AzureSqlDatabaseDataset** girin.

    ![Kaynak veri kümesi adı](./media/tutorial-bulk-copy-portal/source-dataset-general.png)
4. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** hizmetini seçin.
    2. **Tablo** olarak herhangi bir tabloyu seçin. Bu tablo işlevsiz bir tablodur. İşlem hattını oluştururken kaynak veri kümesinde bir sorgu belirtirsiniz. Azure SQL veritabanından verileri ayıklamak için sorgu kullanılır. Alternatif olarak, **Düzenle** onay kutusuna tıklayabilir ve tablo adı olarak **dummyName** girebilirsiniz. 

    ![Kaynak veri kümesi bağlantı sayfası](./media/tutorial-bulk-copy-portal/source-dataset-connection-page.png)
 

### <a name="create-a-dataset-for-sink-sql-data-warehouse"></a>Havuz SQL Veri Ambarı için veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 
2. **Yeni Veri Kümesi** penceresinde **Azure SQL Veri Ambarı**’nı seçin ve **Son**’a tıklayın. **AzureSqlDWTable1** başlıklı yeni bir sekme görüyor olmalısınız. 
3. En alttaki Özellikler penceresinde, **Ad** olarak **AzureSqlDWDataset** girin.
4. **Bağlantı** sekmesine geçin ve **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** seçeneğini belirleyin.
5. **Parametreler** sekmesine geçin ve **+ Yeni**’ye tıklayın.

    ![Kaynak veri kümesi bağlantı sayfası](./media/tutorial-bulk-copy-portal/sink-dataset-new-parameter-button.png)
6. Parametre adı olarak **DWTableName** girin. Bu adı sayfadan kopyalayıp yapıştırırken **DWTableName** komutunun sonunda **boşluk karakteri olmadığından** emin olun. 
7. **Parametreli özellikler** bölümünde, **tableName** özelliği için `@{dataset().DWTableName}` girin. Veri kümesinin **tableName** özelliği, **DWTableName** parametresine bağımsız değişken olarak geçirilen değere ayarlanır. ForEach etkinliği bir tablo listesi boyunca yinelenir ve birer birer Kopyalama etkinliğine geçirilir. 
   
    ![Parametre adı](./media/tutorial-bulk-copy-portal/dwtablename-tablename.png)

## <a name="create-pipelines"></a>İşlem hattı oluşturma
Bu öğreticide, iki işlem hattı oluşturursunuz: **IterateAndCopySQLTables** ve **GetTableListAndTriggerCopyData**. 

**GetTableListAndTriggerCopyData** işlem hattı iki adım gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için **IterateAndCopySQLTables** işlem hattını tetikler.

**GetTableListAndTriggerCopyData**, parametre olarak bir tablo listesi alır. Listedeki her bir tablo için verileri hazırlanmış kopya ve PolyBase kullanarak Azure SQL Veritabanından Azure SQL Veri Ambarına kopyalar.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>IterateAndCopySQLTables işlem hattını oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
2. Özellikler penceresinde, işlem hattının adını **IterateAndCopySQLTables** olarak değiştirin. 

    ![İşlem hattı adı](./media/tutorial-bulk-copy-portal/first-pipeline-name.png)
3. **Parametreler** sekmesine geçin ve aşağıdaki eylemleri gerçekleştirin: 

    1. **+ Yeni** öğesine tıklayın. 
    2. **name** parametresi için **tableList** girin.
    3. **Tür** olarak **Object** seçin.

        ![İşlem hattı parametresi](./media/tutorial-bulk-copy-portal/first-pipeline-parameter.png)
4. **Etkinlikler** araç kutusunda **Yineleme ve Koşullar**’ı genişletin ve **ForEach** etkinliğini sürükleyerek işlem hattı tasarım yüzeyine bırakın. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. En alttaki **Özellikler** penceresinde, **Ad** olarak **IterateSQLTables** girin. 

    ![ForEach etkinliği adı](./media/tutorial-bulk-copy-portal/for-each-activity-name.png)
5. **Ayarlar** sekmesine geçin ve **Öğeler** için `@pipeline().parameters.tableList` değerini girin.

    ![ForEach etkinliği ayarları](./media/tutorial-bulk-copy-portal/for-each-activity-settings.png)
6. **ForEach** etkinliğine bir alt etkinlik eklemek için, ForEach etkinliğine **çift tıklayın** (veya) **Düzenle (kalem simgesi)** öğesine tıklayın. Etkinliğin eylem bağlantılarını görmek için o etkinliği seçmeniz gerekir. 

    ![ForEach etkinliği adı](./media/tutorial-bulk-copy-portal/edit-for-each-activity.png)
7. **Etkinlikler** araç çubuğunda **Veri Akışı**'nı genişletin, **Kopyalama** etkinliğini sürükleyip işlem hattı tasarım yüzeyine bırakın ve Özellikler penceresinde adı **CopyData** olarak değiştirin. En üstteki içerik haritası menüsüne dikkat edin. IterateAndCopySQLTable işlem hattı adı ve IterateSQLTables ise ForEach etkinliği adıdır. Tasarımcı, etkinlik kapsamdadır. ForEach düzenleyicisinden yeniden işlem hattı düzenleyicisine geçmek için, içerik haritası menüsündeki bağlantıya tıklayın. 

    ![ForEach içinde kopyalama](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)
8. **Kaynak** sekmesine geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    2. **Kullanıcı Sorgusu** için **Sorgu** seçeneğini belirtin. 
    3. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 

        ![Kopyalama kaynak ayarları](./media/tutorial-bulk-copy-portal/copy-source-settings.png)
9. **Havuz** sekmesine geçin ve aşağıdaki adımları izleyin: 

    1. **Havuz Veri Kümesi** olarak **AzureSqlDWDataset** seçin.
    2. **Polybase Ayarları**'nı genişletin ve **Polybase'e izin ver**'i seçin. 
    3. **Tür varsayılanını kullan** seçeneğini temizleyin. 
    4. **Temizleme Betiği** için aşağıdaki SQL betiğini girin. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Kopyalama havuz ayarları](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)
10. **Parametreler** sekmesine geçin, **DWTableName** parametresini içeren **Havuz Veri Kümesi** bölümünü görmek için gerekirse ekranı aşağı kaydırın. Bu parametrenin değerini `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` olarak ayarlayın.

    ![Kopyalama havuz parametreleri](./media/tutorial-bulk-copy-portal/copy-sink-parameters.png)
11. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Hazırlamayı Etkinleştir** için **True** değerini seçin.
    2. **Depo Hesabı Bağlı Hizmeti** için **AzureStorageLinkedService** hizmetini seçin.

        ![Hazırlamayı etkinleştirme](./media/tutorial-bulk-copy-portal/copy-sink-staging-settings.png)
12. İşlem hattı ayarlarını doğrulamak için **Doğrula**'ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

    ![İşlem hattı doğrulama raporu](./media/tutorial-bulk-copy-portal/first-pipeline-validation-report.png)

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>GetTableListAndTriggerCopyData işlem hattını oluşturma

Bu işlem hattı iki adım gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için "IterateAndCopySQLTables" işlem hattını tetikler.

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
2. Özellikler penceresinde, işlem hattının adını **GetTableListAndTriggerCopyData** olarak değiştirin. 

    ![İşlem hattı adı](./media/tutorial-bulk-copy-portal/second-pipeline-name.png)
3. **Etkinlikler** araç kutusunda **SQL Veritabanı**’nı genişletin, bir **Arama** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın ve aşağıdaki adımları uygulayın:

    1. **Ad** olarak **LookupTableList** girin. 
    2. **Açıklama** olarak **Tablo listesini Azure SQL veritabanından al** girin.

        ![Arama etkinliği - genel sayfası](./media/tutorial-bulk-copy-portal/lookup-general-page.png)
4. **Ayarlar** sayfasına geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    2. **Sorgu Kullan** için **Sorgu**’yu seçin. 
    3. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    4. **Yalnızca ilk satır** alanının onay işaretini temizleyin.

        ![Arama etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
5. **İşlem Hattı Yürütme** etkinliğini Etkinlikler araç kutusundan sürükleyip işlem hattı tasarımcı yüzeyine bırakın ve adı **TriggerCopy** olarak ayarlayın.

    ![İşlem Hattı Yürütme etkinliği - genel sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-general-page.png)    
6. **Ayarlar** sayfasına geçin ve aşağıdaki adımları izleyin: 

    1. **Çağrılan işlem hattı** olarak **IterateAndCopySQLTables** seçin. 
    2. **Gelişmiş** bölümünü genişletin. 
    3. **Parametreler** bölümünde **+ Yeni** öğesine tıklayın. 
    4. **name** parametresi için **tableList** girin.
    5. **value** parametresi için `@activity('LookupTableList').output.value` girin. Arama etkinliğinden gelen sonuç listesini ikinci işlem hattına giriş olarak ayarlarsınız. Sonuç listesinde yer alan tabloların içerdiği verilerin hedefe kopyalanması gerekir. 

        ![İşlem hattı yürütme etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)
7. İşlem Hattı Yürütme etkinliğinin sol tarafındaki Arama etkinliğine eklenmiş olan **yeşil kutuyu** sürükleyerek, **Arama** etkinliğini **İşlem Hattı Yürütme** etkinliğine **bağlayın**.

    ![Arama ve İşlem Hattı Yürütme etkinliklerini bağlama](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)
8. İşlem hattını doğrulamak için araç çubuğundaki **Doğrula**'ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

    ![İkinci işlem hattı - doğrulama raporu](./media/tutorial-bulk-copy-portal/second-pipeline-validation-report.png)
9. Varlıkları (veri kümeleri, işlem hatları, vb.) Data Factory hizmetine yayımlamak için **Yayımla**’ya tıklayın. Yayımlama başarılı olana kadar bekleyin. 

    ![Yayımla düğmesi](./media/tutorial-bulk-copy-portal/publish.png)

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

1. **GetTableListAndTriggerCopyData** sekmesinin etkin olduğunu onaylayın. 
2. **Tetikle**’ye ve sonra da **Şimdi Tetikle**’ye tıklayın. 

    ![Şimdi tetikle](./media/tutorial-bulk-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine geçin. Çözümünüzde her iki işlem hattının da çalıştırmalarını görene kadar **Yenile**'ye tıklayın. **Başarılı oldu** durumunu görene kadar listeyi yenilemeye devam edin. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-bulk-copy-portal/pipeline-runs.png)
2. GetTableListAndTriggerCopyData işlem hattıyla ilişkilendirilmiş etkinlik çalışmalarını görüntülemek için, söz konusu işlem hattının Eylemler bağlantısı içindeki ilk bağlantıya tıklayın. Bu işlem hattı çalıştırması için iki etkinlik çalıştırması görüyor olmalısınız. 

    ![Etkinlik çalıştırmaları](./media/tutorial-bulk-copy-portal/activity-runs-1.png)    
3. **Arama** etkinliğinin çıkışını görüntülemek için, bu etkinliğin **Çıkış** sütunundaki bağlantıya tıklayın. **Çıkış** penceresinin ekranı kaplamasını sağlamalı ve pencereyi geri yüklemelisiniz. Gözden geçirdikten sonra, **X** simgesine tıklayarak **Çıkış** penceresini kapatın.

    ```json
    {
        "count": 9,
        "value": [
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Customer"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Product"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductModelProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductCategory"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Address"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "CustomerAddress"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderDetail"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderHeader"
            }
        ],
        "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
        "effectiveIntegrationRuntimes": [
            {
                "name": "DefaultIntegrationRuntime",
                "type": "Managed",
                "location": "East US",
                "billedDuration": 0,
                "nodes": null
            }
        ]
    }
    ```    
4. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstteki **İşlem hatları** bağlantısına tıklayın. **IterateAndCopySQLTables** işlem hattı için **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına (**Eylemler** sütunundaki ilk bağlantı) tıklayın. Aşağıdaki resimde gösterilen çıkışı görüyor olmalısınız: **Arama** etkinliği çıkışında her tablo için bir **Kopyalama** etkinliği olduğuna dikkat edin. 

    ![Etkinlik çalıştırmaları](./media/tutorial-bulk-copy-portal/activity-runs-2.png)
5. Verilerin, bu öğreticide kullandığınız hedef SQL Veri Ambarı'na kopyalandığından emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Depolama bağlı hizmetleri oluşturma.
> * Azure SQL Veritabanı ve Azure SQL Veri Ambarı veri kümeleri oluşturma.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturma. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Kaynaktan hedefe verileri artımlı olarak koppyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:
> [!div class="nextstepaction"]
>[Veri artımlı olarak kopyalama](tutorial-incremental-copy-portal.md)