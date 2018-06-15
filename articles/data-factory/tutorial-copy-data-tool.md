---
title: Azure Veri Kopyalama aracını kullanarak veri kopyalama | Microsoft Docs
description: Azure Blob depolamadaki verileri bir SQL veritabanına kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/09/2018
ms.author: jingwang
ms.openlocfilehash: d2f1d089c6a08a1dc90f82fd9d1c3cb2b6f6dc0a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30171817"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-the-copy-data-tool"></a>Veri Kopyalama aracını kullanarak Azure Blob depolama alanında SQL veritabanına veri kopyalama
> [!div class="op_single_selector" title1="Select the version of the Data Factory service that you're using:"]
> * [Sürüm 1 - Genel kullanıma sunuldu](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](tutorial-copy-data-tool.md)

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak bir Azure Blob depolamadaki verileri bir SQL veritabanına kopyalarsınız. 

> [!NOTE]
> Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
>
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory’nin genel kullanıma açık 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.


Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: Blob depolama alanını _kaynak_ veri deposu olarak kullanın. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account) bölümündeki yönergelere bakın.
* **Azure SQL Veritabanı**: Bir SQL veritabanını _havuz_ veri deposu olarak kullanın. SQL veritabanınız yoksa [SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) konusundaki yönergelere bakın.

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Bu adımları uygulayarak Blob depolama alanınızı ve SQL veritabanınızı öğretici için hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. **Not Defteri**'ni başlatın. Aşağıdaki metni kopyalayıp diskinizde **inputEmp.txt** adlı bir dosyaya kaydedin:

    ```
    John|Doe
    Jane|Doe
    ```

2. **adfv2tutorial** adlı bir kapsayıcı oluşturun ve inputEmp.txt dosyasını kapsayıcıya yükleyin. Bu görevleri [Azure Depolama Gezgini](http://storageexplorer.com/) gibi çeşitli araçlar kullanarak gerçekleştirebilirsiniz.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. Aşağıdaki SQL betiğini kullanarak SQL veritabanınızda **dbo.emp** adlı bir tablo oluşturun:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. Azure hizmetlerinin SQL Server’a erişmesine izin ver. SQL Server’ı çalıştıran sunucunuz için **Azure hizmetlerine erişime izin ver** ayarının etkin olduğunu doğrulayın. Bu ayar, Data Factory’nin SQL Server örneğinize veri yazmasına imkan tanır. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    a. Sol taraftan **Diğer hizmetler**’i ve ardından **SQL sunucuları**’nı seçin.

    b. Sunucunuzu seçip **AYARLAR** > **Güvenlik Duvarı**’nı seçin.

    c. **Güvenlik Duvarı ayarları** sayfasında, **Azure hizmetlerine erişime izin ver** seçeneğini **AÇIK** olarak ayarlayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden **+ Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin: 
   
   ![Yeni veri fabrikası oluşturma](./media/tutorial-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası](./media/tutorial-copy-data-tool/new-azure-data-factory.png)
 
   Veri fabrikanızın adı _genel olarak benzersiz_ olmalıdır. Aşağıdaki hata iletisini alabilirsiniz:
   
   ![Yeni veri fabrikası hata iletisi](./media/tutorial-copy-data-tool/name-not-available-error.png)

   Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
3. Yeni veri fabrikasının oluşturulacağı Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).

5. **Sürüm** bölümünde **V2 (Önizleme)** sürümünü seçin.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikanız tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda ve bölgelerde olabilir.
7. **Panoya sabitle**’yi seçin. 
8. **Oluştur**’u seçin.
9. Panodaki **Veri Fabrikası Dağıtılıyor** kutucuğu işlem durumunu gösterir.

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-tool/deploying-data-factory.png)
10. Oluşturma işlemi tamamlandıktan sonra **Data Factory** giriş sayfası görüntülenir.
   
    ![Data factory giriş sayfası](./media/tutorial-copy-data-tool/data-factory-home-page.png)
11. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğunu seçin. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala** kutucuğunu seçin. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-copy-data-tool/copy-data-tool-tile.png)
2. **Özellikler** sayfasındaki **Görev adı** bölümüne **CopyFromBlobToSqlPipeline** adını girin. Sonra **İleri**’yi seçin. Data Factory kullanıcı arabirimi, belirtilen görev adına sahip bir işlem hattı oluşturur. 

    ![Özellikler sayfası](./media/tutorial-copy-data-tool/copy-data-tool-properties-page.png)
3. **Kaynak veri deposu** sayfasında **Azure Blob Depolama**’yı ve ardından **İleri**’yi seçin. Kaynak veriler bir Blob depolama alanında bulunur. 

    ![Kaynak veri deposu sayfası](./media/tutorial-copy-data-tool/source-data-store-page.png)
4. **Azure Blob Depolama hesabı belirtin** sayfasında aşağıdaki adımları gerçekleştirin:

    a. **Bağlantı adı** bölümüne **AzureStorageLinkedService** adını girin.

    b. **Depolama hesabı adı** açılan listesinden depolama hesabınızın adını seçin.

    c. **İleri**’yi seçin. 

    ![Depolama hesabını belirtme](./media/tutorial-copy-data-tool/specify-blob-storage-account.png)

    Bağlı hizmetler, bir veri deposunu veya işlemi veri fabrikasına bağlar. Bu durumda, depolama hesabınızı veri deposuna bağlamak için bir depolama bağlı hizmeti oluşturmanız gerekir. Bağlı hizmet, Data Factory’nin çalışma zamanında Blob depolama alanına bağlanmak için kullandığı bağlantı bilgilerini içerir. Veri kümesi tarafından kaynak verileri içeren kapsayıcı, klasör ve dosya (isteğe bağlı) belirtilir. 

5. **Girdi dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın:
    
    a. **adfv2tutorial/input** klasörüne göz atın.

    b. **inputEmp.txt** dosyasını seçin.

    c. **Seç** seçeneğini belirleyin. Alternatif olarak, **inputEmp.txt** dosyasına çift tıklayabilirsiniz.

    d. **İleri**’yi seçin. 

    ![Girdi dosyası veya klasörü seçin](./media/tutorial-copy-data-tool/choose-input-file-folder.png)

6. **Dosya biçimi ayarları** sayfasında aracın sütun ve satır sınırlayıcılarını otomatik olarak belirlediğine dikkat edin. **İleri**’yi seçin. Ayrıca, bu sayfada verilerin önizlemesini yapabilir ve giriş verilerinin şemasını görüntüleyebilirsiniz. 

    ![Dosya biçimi ayarları](./media/tutorial-copy-data-tool/file-format-settings-page.png)
7. **Hedef veri deposu** sayfasında **Azure SQL Veritabanı** öğesini seçip **İleri**’yi seçin.

    ![Hedef veri deposu](./media/tutorial-copy-data-tool/destination-data-storage-page.png)
8. **Azure SQL veritabanını belirtin** sayfasında aşağıdaki adımları uygulayın: 

    a. **Bağlantı adı** bölümüne **AzureSqlDatabaseLinkedService** adını girin.

    b. **Sunucu adı** bölümünde SQL Server örneğinizi seçin.

    c. **Veritabanı adı** bölümünde SQL veritabanınızı seçin.

    d. **Kullanıcı adı** bölümüne kullanıcının adını girin.

    e. **Parola** bölümüne kullanıcının parolasını girin.

    f. **İleri**’yi seçin. 

    ![SQL veritabanını belirtme](./media/tutorial-copy-data-tool/specify-azure-sql-database.png)

    Bağlı hizmetle bir veri kümesi ilişkilendirilmelidir. Bağlı hizmet, Data Factory’nin çalışma zamanında SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtir.

9. **Tablo eşleme** sayfasında **[dbo].[emp]** tablosunu seçip **İleri**’yi seçin. 

    ![Tablo eşleme](./media/tutorial-copy-data-tool/table-mapping-page.png)
10. **Şema eşleme** sayfasında, girdi dosyasındaki birinci ve ikinci sütunun **emp** tablosundaki **FirstName** ve **LastName** sütunuyla eşlendiğine dikkat edin.

    ![Şema eşleme sayfası](./media/tutorial-copy-data-tool/schema-mapping-page.png)
11. **Ayarlar** sayfasında **İleri**’yi seçin. 

    ![Ayarlar sayfası](./media/tutorial-copy-data-tool/settings-page.png)
12. **Özet** sayfasında ayarları gözden geçirin ve **İleri**’yi seçin.

    ![Özet sayfası](./media/tutorial-copy-data-tool/summary-page.png)
13. **Dağıtım** sayfasında, işlem hattını (görev) izlemek için **İzleyici**’yi seçin.

    ![Dağıtım sayfası](./media/tutorial-copy-data-tool/deployment-page.png)
14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-copy-data-tool/monitor-pipeline-runs.png)
15. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstteki **İşlem hatları** bağlantısını seçin. Görünümü yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-tool/monitor-activity-runs.png)
16. Düzenleyici moduna geçmek için soldaki **Düzenle** sekmesini seçin. Düzenleyici kullanılarak araç üzerinden oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Düzenleyicide açık olan varlığın JSON kodunu görüntülemek için **Kod**’u seçin. Bu varlıkları Data Factory kullanıcı arabiriminde düzenlemeyle ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

    ![Düzenleyici sekmesi](./media/tutorial-copy-data-tool/edit-tab.png)
17. Verilerin SQL veritabanınızdaki **emp** tablosuna eklendiğini doğrulayın.

    ![SQL çıkışını doğrulama](./media/tutorial-copy-data-tool/verify-sql-output.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, bir Blob depolama alanındaki verileri bir SQL veritabanına kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-data-tool.md)
