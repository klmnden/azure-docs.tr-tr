---
title: Azure Data Factory ile verileri toplu olarak kopyalama | Microsoft Docs
description: Azure Data Factory ve Kopyalama Etkinliği’ni kullanarak bir kaynak veri deposundan hedef veri deposuna toplu veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 4ba94187cb014256d63e80cb23defc5099aac52d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445555"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama
Bu öğreticide **Azure SQL Veritabanından Azure SQL Veri Ambarı'na birkaç tabloyu kopyalama** işlemi gösterilmektedir. Aynı düzeni diğer kopyalama senaryolarında da uygulayabilirsiniz. Örneğin, SQL Server/Oracle’dan Azure SQL Veritabanı/Veri Ambarı/Azure Blob’a tablo kopyalama, Blob’dan Azure SQL Veritabanı tablolarına farklı yollar kopyalama.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

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

1. SQL Veri Ambarında karşılık gelen tablo şemalarını oluşturun. Azure SQL Veritabanından Azure SQL Veri Ambarına **şema geçirmek** için [Geçiş Aracı](https://www.microsoft.com/download/details.aspx?id=49100)’nı kullanabilirsiniz. Daha sonraki bir adımda verileri geçirmek/kopyalamak için Azure Data Factory’yi kullanın.

## <a name="azure-services-to-access-sql-server"></a>SQL sunucusuna erişime yönelik Azure hizmetleri

Hem SQL Veritabanı hem de SQL Veri Ambarı için Azure hizmetlerinin SQL sunucusuna erişmesine izin verin. **Azure hizmetlerine erişime izin ver** ayarının Azure SQL sunucusunda **AÇIK** olduğundan emin olun. Bu ayar, Data Factory hizmetinin Azure SQL Veritabanınızdan verileri okumasına ve Azure SQL Veri Ambarına veri yazmasına olanak tanır. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

1. Soldaki **Diğer hizmetler** hub’ına ve sonra **SQL sunucuları**’na tıklayın.
1. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
1. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialBulkCopyDF** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-bulk-copy-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialBulkCopyDF). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name “ADFTutorialBulkCopyDF” is not available`
1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
1. **Sürüm** için **V2**'yi seçin.
1. Data factory için **konum** seçin. Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için aşağıdaki sayfada faiz ve ardından genişletin bölgeleri seçin **Analytics** bulunacak **Data Factory**: [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
1. **Panoya sabitle**’yi seçin.     
1. **Oluştur**’a tıklayın.
1. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

     ![veri fabrikası dağıtılıyor kutucuğu](media//tutorial-bulk-copy-portal/deploying-data-factory.png)
1. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
     ![Data factory giriş sayfası](./media/tutorial-bulk-copy-portal/data-factory-home-page.png)
1. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede açmak için **Author & Monitor** (Oluştur ve İzle) kutucuğuna tıklayın.
1. **Başlarken** sayfasında, aşağıdaki resimde gösterildiği gibi sol bölmede bulunan **Düzenle** sekmesine geçin:  

     ![Başlarken sayfası](./media/tutorial-bulk-copy-portal/get-started-page.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlemlerinizi veri fabrikasına bağlamak için bağlı hizmetler oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için kullandığı bağlantı bilgilerini içerir. 

Bu öğreticide, Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure Blob Depolama veri depolarınızı veri fabrikanıza bağlarsınız. Azure SQL Veritabanı, kaynak veri deposudur. Azure SQL Veri Ambarı, havuz/hedef veri deposudur. Azure Blob Depolama, PolyBase kullanılarak veriler SQL Veri Ambarı'na yüklenmeden önce verilerin hazırlanması için kullanılır. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Kaynak Azure SQL Veritabanı bağlı hizmetini oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. 

1. Pencerenin en altından **Bağlantılar**’a ve sonra araç çubuğunda **+ Yeni**’ye tıklayın. 

    ![New Linked Service (Yeni bağlı hizmet) düğmesi](./media/tutorial-bulk-copy-portal/new-linked-service-button.png)
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure SQL Veritabanı**’nı seçip **Devam**’a tıklayın. 

    ![Azure SQL Veritabanı’nı seçin](./media/tutorial-bulk-copy-portal/select-azure-sql-database.png)
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureSqlDatabaseLinkedService** adını girin. 
    1. **Sunucu adı** için Azure SQL sunucunuzu seçin.
    1. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    1. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    1. Kullanıcının **parolasını** girin. 
    1. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
    1. **Kaydet**’e tıklayın.

        ![Azure SQL Veritabanı ayarları](./media/tutorial-bulk-copy-portal/azure-sql-database-settings.png)

### <a name="create-the-sink-azure-sql-data-warehouse-linked-service"></a>Havuz Azure SQL Veri Ambarı bağlı hizmetini oluşturma

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure SQL Veri Ambarı**’nı seçip **Devam**’a tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureSqlDWLinkedService** adını girin. 
    1. **Sunucu adı** için Azure SQL sunucunuzu seçin.
    1. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    1. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    1. Kullanıcının **parolasını** girin. 
    1. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
    1. **Kaydet**’e tıklayın.

### <a name="create-the-staging-azure-storage-linked-service"></a>Hazırlama Azure Depolama bağlı hizmetini oluşturma
Bu öğreticide Azure Blob depolamayı daha iyi bir kopyalama performansı için PolyBase’i etkinleştiren geçici bir hazırlama alanı olarak kullanırsınız.

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure Blob Depolama**’yı seçip **Devam**’a tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde aşağıdaki adımları izleyin: 

    1. **Ad** için **AzureStorageLinkedService** adını girin. 
    1. **Depolama hesabı adı** için **Azure Depolama hesabınızı** seçin.
    1. **Kaydet**’e tıklayın.


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu öğreticide, verilerin depolandığı konumu belirten kaynak ve havuz veri kümelerini oluşturacaksınız. 

**AzureSqlDatabaseDataset** giriş veri kümesi, **AzureSqlDatabaseLinkedService**'e işaret eder. Bağlı hizmet, veritabanına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, kaynak verileri içeren veritabanı ve tablonun adını belirtir. 

**AzureSqlDWDataset** çıkış veri kümesi **AzureSqlDWLinkedService**'e işaret eder. Bağlı hizmet, veri ambarına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, verilerin kopyalandığı hedef veritabanı ve tabloyu belirtir. 

Bu öğreticide, kaynak ve hedef SQL tabloları veri kümesi tanımında sabit kodlanmamıştır. Bunun yerine, ForEach etkinliği tablonun adını çalışma zamanında Kopyalama etkinliğine geçirir. 

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 

    ![Yeni veri kümesi menüsü](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve **Son**’a tıklayın. **AzureSqlTable1** başlıklı yeni bir sekme görüyor olmalısınız. 
    
    ![Azure SQL Veritabanı’nı seçin](./media/tutorial-bulk-copy-portal/select-azure-sql-database-dataset.png)
1. En alttaki Özellikler penceresinde, **Ad** olarak **AzureSqlDatabaseDataset** girin.

1. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

   1. **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** hizmetini seçin.
   1. **Tablo** olarak herhangi bir tabloyu seçin. Bu tablo işlevsiz bir tablodur. İşlem hattını oluştururken kaynak veri kümesinde bir sorgu belirtirsiniz. Azure SQL veritabanından verileri ayıklamak için sorgu kullanılır. Alternatif olarak, **Düzenle** onay kutusuna tıklayabilir ve tablo adı olarak **dummyName** girebilirsiniz. 

      ![Kaynak veri kümesi bağlantı sayfası](./media/tutorial-bulk-copy-portal/source-dataset-connection-page.png)
 

### <a name="create-a-dataset-for-sink-sql-data-warehouse"></a>Havuz SQL Veri Ambarı için veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 
1. **Yeni Veri Kümesi** penceresinde **Azure SQL Veri Ambarı**’nı seçin ve **Son**’a tıklayın. **AzureSqlDWTable1** başlıklı yeni bir sekme görüyor olmalısınız. 
1. En alttaki Özellikler penceresinde, **Ad** olarak **AzureSqlDWDataset** girin.
1. **Parametreler** sekmesine geçin, **+ Yeni**'ye tıklayın ve parametre adı olarak **DWTableName** girin. Bu adı sayfadan kopyalayıp yapıştırırken **DWTableName** komutunun sonunda **boşluk karakteri olmadığından** emin olun. 

    ![Kaynak veri kümesi bağlantı sayfası](./media/tutorial-bulk-copy-portal/sink-dataset-new-parameter.png)

1. **Bağlantı** sekmesine geçin, 

    a. **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** hizmetini seçin.

    b. **Tablo** için **Düzenle** seçeneğini belirtin, tablo adı giriş kutusuna tıklayın ve altındaki **Dinamik içerik ekle** bağlantısına tıklayın. 
    
    ![Parametre adı](./media/tutorial-bulk-copy-portal/table-name-parameter.png)

    c. **Dinamik İçerik Ekle** sayfasında **Parametreler** bölümünde bulunan ve en iyi ifade metin kutusunu `@dataset().DWTableName` ile otomatik olarak dolduran **DWTAbleName** öğesine ve ardından **Son**'a tıklayın. Veri kümesinin **tableName** özelliği, **DWTableName** parametresine bağımsız değişken olarak geçirilen değere ayarlanır. ForEach etkinliği bir tablo listesi boyunca yinelenir ve tabloları birer birer Kopyalama etkinliğine geçirir. 

    ![Veri kümesi parametresi derleyici](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)

## <a name="create-pipelines"></a>İşlem hattı oluşturma
Bu öğreticide, iki işlem hattı oluşturacaksınız: **Iterateandcopysqltables** ve **GetTableListAndTriggerCopyData**. 

**GetTableListAndTriggerCopyData** işlem hattı iki adım gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için **IterateAndCopySQLTables** işlem hattını tetikler.

**GetTableListAndTriggerCopyData**, parametre olarak bir tablo listesi alır. Listedeki her bir tablo için verileri hazırlanmış kopya ve PolyBase kullanarak Azure SQL Veritabanından Azure SQL Veri Ambarına kopyalar.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>IterateAndCopySQLTables işlem hattını oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. **Genel** sekmesinde ad için **IterateAndCopySQLTables** girişini belirtin. 

1. **Parametreler** sekmesine geçin ve aşağıdaki eylemleri gerçekleştirin: 

    1. **+ Yeni** öğesine tıklayın. 
    1. **name** parametresi için **tableList** girin.
    1. **Tür** için **Dizi**'yi seçin.

        ![İşlem hattı parametresi](./media/tutorial-bulk-copy-portal/first-pipeline-parameter.png)
1. **Etkinlikler** araç kutusunda **Yineleme ve Koşullar**’ı genişletin ve **ForEach** etkinliğini sürükleyerek işlem hattı tasarım yüzeyine bırakın. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. 

    a. En alttaki **Genel** sekmesinde, **Ad** olarak **IterateSQLTables** girin. 

    b. **Ayarlar** sekmesine geçin, **Öğeler** giriş kutusuna ve ardından altındaki **Dinamik içerik ekle** bağlantısına tıklayın. 

    ![ForEach etkinliği ayarları](./media/tutorial-bulk-copy-portal/for-each-activity-settings.png)

    c. **Dinamik İçerik Ekle** sayfasında Sistem Değişkenleri ve İşlevleri bölümünü daraltın, **Parametreler** bölümünde bulunan en iyi ifade metin kutusunu `@pipeline().parameter.tableList` ile otomatik olarak dolduran **tableList** öğesine ve ardından **Son**'a tıklayın. 

    ![Foreach parametresi derleyici](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. **Etkinlikler** sekmesine geçin, **Etkinlik ekle**'ye tıklayarak **ForEach** etkinliğine bir alt etkinlik ekleyin.

1. **Etkinlikler** araç kutusunda **DataFlow**’u genişletin ve **Kopyalama** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın. En üstteki içerik haritası menüsüne dikkat edin. IterateAndCopySQLTable işlem hattı adı ve IterateSQLTables ise ForEach etkinliği adıdır. Tasarımcı, etkinlik kapsamdadır. ForEach düzenleyicisinden yeniden işlem hattı düzenleyicisine geçmek için, içerik haritası menüsündeki bağlantıya tıklayın. 

    ![ForEach içinde kopyalama](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)
1. **Kaynak** sekmesine geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. **Kullanıcı Sorgusu** için **Sorgu** seçeneğini belirtin. 
    1. **Sorgu** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> **Sorgu** için aşağıdaki ifadeyi girin -> **Son**'u seçin.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 

        ![Kopyalama kaynak ayarları](./media/tutorial-bulk-copy-portal/copy-source-settings.png)
1. **Havuz** sekmesine geçin ve aşağıdaki adımları izleyin: 

    1. **Havuz Veri Kümesi** olarak **AzureSqlDWDataset** seçin.
    1. DWTableName parametresinin değeri için giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin, betik olarak `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` ifadesini girin -> **Son**'u seçin.
    1. **Polybase Ayarları**'nı genişletin ve **Polybase'e izin ver**'i seçin. 
    1. **Tür varsayılanını kullan** seçeneğini temizleyin. 
    1. **Betiği kopyala** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> betik için aşağıdaki ifadeyi girin -> **Son**'u seçin. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Kopyalama havuz ayarları](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)

1. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Hazırlamayı Etkinleştir** için **True** değerini seçin.
    1. **Depo Hesabı Bağlı Hizmeti** için **AzureStorageLinkedService** hizmetini seçin.

        ![Hazırlamayı etkinleştirme](./media/tutorial-bulk-copy-portal/copy-sink-staging-settings.png)

1. İşlem hattı ayarlarını doğrulamak için üst işlem hattı araç çubuğunda **Doğrula**’ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>GetTableListAndTriggerCopyData işlem hattını oluşturma

Bu işlem hattı iki adım gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için "IterateAndCopySQLTables" işlem hattını tetikler.

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. Özellikler penceresinde, işlem hattının adını **GetTableListAndTriggerCopyData** olarak değiştirin. 

1. **Etkinlikler** araç kutusunda **Genel**’i genişletin, bir **Arama** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın ve aşağıdaki adımları uygulayın:

    1. **Ad** olarak **LookupTableList** girin. 
    1. **Açıklama** olarak **Tablo listesini Azure SQL veritabanından al** girin.

        ![Arama etkinliği - genel sayfası](./media/tutorial-bulk-copy-portal/lookup-general-page.png)
1. **Ayarlar** sayfasına geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. **Sorgu Kullan** için **Sorgu**’yu seçin. 
    1. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. **Yalnızca ilk satır** alanının onay işaretini temizleyin.

        ![Arama etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. **İşlem Hattı Yürütme** etkinliğini Etkinlikler araç kutusundan sürükleyip işlem hattı tasarımcı yüzeyine bırakın ve adı **TriggerCopy** olarak ayarlayın.

    ![İşlem Hattı Yürütme etkinliği - genel sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-general-page.png)    
1. **Ayarlar** sayfasına geçin ve aşağıdaki adımları izleyin: 

    1. **Çağrılan işlem hattı** olarak **IterateAndCopySQLTables** seçin. 
    1. **Gelişmiş** bölümünü genişletin. 
    1. **Parametreler** bölümünde **+ Yeni** öğesine tıklayın. 
    1. **name** parametresi için **tableList** girin.
    1. DEĞER giriş kutusuna tıklayın -> altındaki **Dinamik içerik ekle**'yi seçin -> tablo adı değeri olarak `@activity('LookupTableList').output.value` girin -> **Son**' seçin. Arama etkinliğinden gelen sonuç listesini ikinci işlem hattına giriş olarak ayarlarsınız. Sonuç listesinde yer alan tabloların içerdiği verilerin hedefe kopyalanması gerekir. 

        ![İşlem hattı yürütme etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)
1. İşlem Hattı Yürütme etkinliğinin sol tarafındaki Arama etkinliğine eklenmiş olan **yeşil kutuyu** sürükleyerek, **Arama** etkinliğini **İşlem Hattı Yürütme** etkinliğine **bağlayın**.

    ![Arama ve İşlem Hattı Yürütme etkinliklerini bağlama](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)
1. İşlem hattını doğrulamak için araç çubuğundaki **Doğrula**'ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

1. Varlıkları (veri kümeleri, işlem hatları, vb.) Data Factory hizmetine yayımlamak için pencerenin üst tarafındaki **Tümünü Yayımla**’ya tıklayın. Yayımlama başarılı olana kadar bekleyin. 

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

**GetTableListAndTriggerCopyData** işlem hattına gidin, **Tetikle**'ye ve ardından **Şimdi Tetikle**'ye tıklayın. 

![Şimdi tetikle](./media/tutorial-bulk-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine geçin. Çözümünüzde her iki işlem hattının da çalıştırmalarını görene kadar **Yenile**'ye tıklayın. **Başarılı oldu** durumunu görene kadar listeyi yenilemeye devam edin. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-bulk-copy-portal/pipeline-runs.png)
1. GetTableListAndTriggerCopyData işlem hattıyla ilişkilendirilmiş etkinlik çalışmalarını görüntülemek için, söz konusu işlem hattının Eylemler bağlantısı içindeki ilk bağlantıya tıklayın. Bu işlem hattı çalıştırması için iki etkinlik çalıştırması görüyor olmalısınız. 

    ![Etkinlik çalıştırmaları](./media/tutorial-bulk-copy-portal/activity-runs-1.png)    
1. **Arama** etkinliğinin çıkışını görüntülemek için, bu etkinliğin **Çıkış** sütunundaki bağlantıya tıklayın. **Çıkış** penceresinin ekranı kaplamasını sağlamalı ve pencereyi geri yüklemelisiniz. Gözden geçirdikten sonra, **X** simgesine tıklayarak **Çıkış** penceresini kapatın.

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
1. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstteki **İşlem hatları** bağlantısına tıklayın. **IterateAndCopySQLTables** işlem hattı için **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına (**Eylemler** sütunundaki ilk bağlantı) tıklayın. Aşağıdaki görüntüde gösterildiği gibi bir çıktı görmeniz gerekir: Bir fark **kopyalama** etkinliği çıkışında her tablo **arama** etkinlik çıkışı. 

    ![Etkinlik çalıştırmaları](./media/tutorial-bulk-copy-portal/activity-runs-2.png)
1. Verilerin, bu öğreticide kullandığınız hedef SQL Veri Ambarı'na kopyalandığından emin olun. 

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
