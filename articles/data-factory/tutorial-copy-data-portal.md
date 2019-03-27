---
title: Veri Fabrikası işlem hattı oluşturmak için Azure portalını kullanma | Microsoft Docs
description: Bu öğreticide işlem hattıyla veri fabrikası oluşturmak için Azure portalını kullanmaya yönelik adım adım yönergeler sağlanır. İşlem hattı, verileri Azure Blob depolama alanından SQL veritabanına kopyalamak için kopyalama etkinliğini kullanır.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: jingwang
ms.openlocfilehash: 38c9c97af0be77bf9ad4bea2d24676c7448b3aea
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447555"
---
# <a name="copy-data-from-azure-blob-storage-to-a-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Blob depolama alanında SQL veritabanına veri kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak bir veri fabrikası oluşturursunuz. Bu veri fabrikasındaki işlem hattı, verileri Azure Blob Depolama alanından SQL veritabanına kopyalar. Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

> [!NOTE]
> - İlk kez Data Factory kullanıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**. Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).
* **Azure SQL Veritabanı**. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. SQL veritabanınız yoksa, oluşturma adımları için bkz. [SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md).

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları uygulayarak öğretici için Blob depolama alanınızı ve SQL veritabanınızı hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve diskinizde **emp.txt** dosyası olarak kaydedin:

    ```
    John,Doe
    Jane,Doe
    ```

1. Blob depolama alanınızda **adftutorial** adlı bir kapsayıcı oluşturun. Bu kapsayıcıda **input** adlı bir klasör oluşturun. Sonra, **emp.txt** dosyasını **input** klasörüne yükleyin. Bu görevleri yerine getirmek için Azure Portal'ı veya [Azure Depolama Gezgini](https://storageexplorer.com/) gibi araçları kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. SQL veritabanınızda **dbo.emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın:

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

1. Azure hizmetlerinin SQL Server’a erişmesine izin verin. Data Factory’nin SQL Server’ınıza veri yazabilmesi için SQL Server’ınız için **Azure hizmetlerine erişime izin ver** ayarının **AÇIK** olduğundan emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

    a. Solda **Diğer hizmetler** > **SQL sunucuları** seçeneğini belirleyin.

    b. Sunucunuzu seçin ve **AYARLAR** bölümünden **Güvenlik Duvarı**’nı seçin.

    c. **Güvenlik Duvarı ayarları** sayfasında **Azure hizmetlerine erişime izin ver** için **AÇIK** seçeneğini belirleyin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız. 

1. Açık **Microsoft Edge** veya **Google Chrome**. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**: 
  
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası](./media/tutorial-copy-data-portal/new-azure-data-factory.png)
 
   Azure data factory adı *küresel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).
  
   ![Hata iletisi](./media/tutorial-copy-data-portal/name-not-available-error.png)
4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin. 
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md). 
6. **Sürüm** bölümünde **V2**'yi seçin.
7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
8. **Panoya sabitle**’yi seçin. 
9. **Oluştur**’u seçin. 
10. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz: 

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-copy-data-portal/deploying-data-factory.png)
1. Oluşturma işlemi bittikten sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfası görüntülenir.
   
    ![Data factory giriş sayfası](./media/tutorial-copy-data-portal/data-factory-home-page.png)
1. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, veri fabrikasında kopyalama etkinliği ile bir işlem hattı oluşturacaksınız. Kopyalama etkinliği, verileri Blob depolama alanından SQL Veritabanı'na kopyalar. [Hızlı başlangıç eğitiminde](quickstart-create-data-factory-portal.md), şu adımları izleyerek bir işlem hattı oluşturdunuz:

1. Bağlı hizmeti oluşturma. 
1. Giriş ve çıkış veri kümelerini oluşturma.
1. İşlem hattı oluşturma.

Bu öğreticide işlem hattını oluşturmaya başlayacaksınız. Daha sonra işlem hattını yapılandırmanız gerektiğinde bağlı hizmetleri ve veri kümelerini oluşturacaksınız. 

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. 

   ![İşlem hattı oluşturma](./media/tutorial-copy-data-portal/create-pipeline-tile.png)
1. İşlem hattının **Genel** sekmesinde **Ad** alanına **CopyPipeline** yazın.

1. İçinde **etkinlikleri** araç kutusunda **taşıma andTransform** kategorisi ve sürükle ve bırak **veri kopyalama** işlem hattı tasarımcısının yüzeyine etkinliğini araç kutusundan. **Ad** için **CopyFromBlobToSql** adını belirtin.

    ![Kopyalama etkinliği](./media/tutorial-copy-data-portal/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Kaynağı yapılandırma

1. **Kaynak** sekmesine gidin. Kaynak veri kümesi oluşturmak için **+ Yeni** seçeneğini belirleyin. 

1. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama Alanı**’nı ve ardından **Son**’u seçin. Kaynak veriler bir Blob depolama alanında olduğundan kaynak veri kümesi olarak **Azure Blob Depolama Alanı**'nı seçmeniz gerekir. 

    ![Storage seçimi](./media/tutorial-copy-data-portal/select-azure-blob-dataset.png)

1. Bu blob veri kümesi için yeni bir sekme açıldığını görürsünüz. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SourceBlobDataset** girin.

    ![Veri kümesi adı](./media/tutorial-copy-data-portal/dataset-name.png)

1. **Özellikler** penceresinin **Bağlantı** sekmesine gidin. **Bağlı hizmet** metin kutusunun yanındaki **+ Yeni** seçeneğini belirleyin. 

    ![New Linked Service (Yeni bağlı hizmet) düğmesi](./media/tutorial-copy-data-portal/source-dataset-new-linked-service-button.png)

1. **Yeni Bağlı Hizmet** penceresine **AzureStorageLinkedService** adını girin, **Depolama hesabı adı** listesinden depolama hesabınızı seçin ve **Kaydet**'i seçerek bağlı hizmeti dağıtın.

    ![Yeni bağlı hizmet](./media/tutorial-copy-data-portal/new-azure-storage-linked-service.png)

1. Bağlı hizmet oluşturulduktan sonra veri kümesi ayarlarına dönersiniz. **Dosya yolu**’nun yanındaki **Gözat** seçeneğini belirleyin.

    ![Dosya yolu için gözat düğmesi](./media/tutorial-copy-data-portal/file-browse-button.png)

1. **adftutorial/input** klasörüne gidin, **emp.txt** dosyasını ve ardından **Son**'u seçin. 

    ![Giriş dosyasını seçme](./media/tutorial-copy-data-portal/select-input-file.png)

1. **Dosya biçimi** olarak **Metin biçimi**'nin ve **Sütun sınırlayıcısı** olarak da **Virgül(`,`)** değerinin ayarlandığından emin olun. Kaynak dosyada farklı satır ve sütun sınırlayıcıları kullanılıyorsa, **Dosya biçimi** için **Metin Biçimini Algıla**'yı seçebilirsiniz. Verileri Kopyala aracı sizin için dosya biçimini ve sınırlayıcıları otomatik olarak algılar. Yine de bu değerleri geçersiz kılabilirsiniz. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme** ‘yi seçin.

    ![Metin biçimini algılama](./media/tutorial-copy-data-portal/detect-text-format.png)

1. **Özellikler** penceresinin **Şema** sekmesine gidin ve **Şemayı İçeri Aktar**’ı seçin. Uygulamanın kaynak dosyada iki sütun algıladığına dikkat edin. Kaynak veri deposundaki sütunları havuz veri deposu ile eşlemek için şemayı buraya aktarmanız gerekir. Sütunları eşlemeniz gerekmiyorsa, bu adımı atlayabilirsiniz. Bu öğreticide, şemayı içeri aktarın.

    ![Kaynak şemayı algılama](./media/tutorial-copy-data-portal/detect-source-schema.png)  

1. Şimdi işlem hattı -> **Kaynak** sekmesine gidin ve **SourceBlobDataset** öğesinin seçili olduğundan emin olun. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme** ‘yi seçin. 
    
    ![Kaynak veri kümesi](./media/tutorial-copy-data-portal/source-dataset-selected.png)

### <a name="configure-sink"></a>Havuzu yapılandırma

1. **Havuz** sekmesine gidin ve havuz veri kümesi oluşturmak için **+Yeni** seçeneğini belirleyin. 

    ![Havuz veri kümesi](./media/tutorial-copy-data-portal/new-sink-dataset-button.png)
1. İçinde **yeni veri kümesi** penceresinde bağlayıcıları filtrelemenize ve ardından seçmek için arama kutusuna "SQL" Giriş **Azure SQL veritabanı**ve ardından **son**. Bu öğreticide verileri bir SQL veritabanına kopyalayacaksınız. 

    ![SQL veritabanı seçimi](./media/tutorial-copy-data-portal/select-azure-sql-dataset.png)
1. **Özellikler** penceresinin **Genel** sekmesinde **Ad** bölümüne **OutputSqlDataset** girin. 
    
    ![Çıkış veri kümesi adı](./media/tutorial-copy-data-portal/output-dataset-name.png)
1. **Bağlantı**sekmesine gidin ve **Bağlı hizmet**’in yanındaki **+ Yeni** seçeneğini belirleyin. Bağlı hizmetle bir veri kümesi ilişkilendirilmelidir. Bağlı hizmet, Data Factory’nin çalışma zamanında SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtir. 
    
    ![Bağlı hizmet](./media/tutorial-copy-data-portal/new-azure-sql-database-linked-service-button.png)       
1. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** bölümüne **AzureSqlDatabaseLinkedService** girin.

    b. **Sunucu adı** bölümünde SQL Server örneğinizi seçin.

    c. **Veritabanı adı** bölümünde SQL veritabanınızı seçin.

    d. **Kullanıcı adı** bölümüne kullanıcının adını girin.

    e. **Parola** bölümüne kullanıcının parolasını girin.

    f. Bağlantıyı test etmek için **Bağlantıyı sına**’yı seçin.

    g. Bağlı hizmeti kaydetmek için **Kaydet**’i seçin. 
    
    ![Yeni bağlı hizmeti kaydedin](./media/tutorial-copy-data-portal/new-azure-sql-linked-service-window.png)

1. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin. 

    ![Tablo](./media/tutorial-copy-data-portal/select-emp-table.png)
1. **Şema** sekmesine gidin ve **Şemayı İçeri Aktar**’ı seçin. 

    ![Şemayı içeri aktar’ı seçin](./media/tutorial-copy-data-portal/import-destination-schema.png)
1. **ID** sütununu ve ardından **Sil**’i seçin. **ID** sütunu, SQL veritabanındaki bir kimlik sütunu olduğundan kopyalama etkinliğinin bu sütuna veri eklemesi gerekmez.

    ![ID sütununu silme](./media/tutorial-copy-data-portal/delete-id-column.png)
1. İşlem hattının bulunduğu sekmeye gidin ve **Havuz Veri Kümesi**’nde **OutputSqlDataset** seçeneğinin belirlendiğinden emin olun.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal/pipeline-tab-2.png)        

### <a name="configure-mapping"></a>Eşlemeyi yapılandırma

**Özellikler** penceresinin altındaki **Eşleme** sekmesine gidin ve **Şemaları İçeri Aktar**’ı seçin. Kaynak dosyadaki birinci ve ikinci sütunların SQL veritabanındaki **FirstName** ve **LastName** alanlarına eşlendiğini görebilirsiniz.

![Şemaları eşleme](./media/tutorial-copy-data-portal/map-schemas.png)

## <a name="validate-the-pipeline"></a>İşlem hattını doğrulama
İşlem hattını doğrulamak için araç çubuğundan **Doğrula**'yı seçin.
 
Sağ üstteki **Kod**'a tıklayarak işlem hattıyla ilişkili JSON kodunu görebilirsiniz.

## <a name="debug-and-publish-the-pipeline"></a>İşlem hattında hata ayıklama ve işlem hattını yayımlama
Yapıtları (bağlı hizmetler, veri kümeleri ve işlem hattı) Data Factory'de veya kendi Azure Repos Git deponuzda yayımlamadan önce işlem hattında hata ayıklayabilirsiniz. 

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir. 

1. İşlem hattı başarılı bir şekilde, üst araç çubuğunda, çalıştırabilirsiniz belirleyin **tümünü Yayımla**. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) Data Factory'de yayımlar.

    ![Yayımlama](./media/tutorial-copy-data-portal/publish-button.png)

1. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üstteki **Bildirimleri Göster**'e (zil düğmesi) tıklayın. 

## <a name="trigger-the-pipeline-manually"></a>İşlem hattını el ile tetikleme
Bu adımda, önceki adımda yayımladığınız işlem hattını el ile tetiklersiniz. 

1. Araç çubuğunda **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin. **İşlem Hattı Çalıştırma** sayfasında **Son**’u seçin.  

1. Soldaki **İzleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve işlem hattını yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-copy-data-portal/monitor-pipeline.png)
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte, tek bir etkinlik olduğundan listede tek giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için en üstteki **İşlem Hatları**'nı seçin. Görünümü yenilemek için **Yenile**’yi seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-copy-data-portal/view-activity-runs.png)
1. SQL veritabanında **emp** tablosuna iki satır daha eklendiğinden emin olun. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>İşlem hattını bir zamanlamaya göre tetikleme
Bu zamanlamada, işlem hattı için bir zamanlayıcı tetikleyicisi oluşturacaksınız. Tetikleyici, işlem hattını saatlik veya günlük gibi belirli bir zamanlamaya göre çalıştırır. Bu örnekte, tetikleyiciyi belirtilen bitiş tarihi saatine kadar dakikada bir çalıştırılacak şekilde ayarlarsınız. 

1. Sol üstte, izleyici sekmesinin üzerindeki **Yazar** sekmesine gidin. 

1. İşlem hattınıza gidin, araç çubuğunda **Tetikleyici**'ye tıklayıp **Yeni/Düzenle**'yi seçin. 

1. **Tetikleyici Ekle** penceresinde **Tetikleyici seç**’i ve sonra **+Yeni**’yi seçin. 

    ![Yeni düğmesi](./media/tutorial-copy-data-portal/add-trigger-new-button.png)
1. **Yeni Tetikleyici** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** bölümüne **RunEveryMinute** girin.

    b. **Bitiş** bölümünde **Tarih** seçeneğini belirleyin.

    c. **Bitiş Tarihi** bölümde açılan listeden seçim yapın.

    d. **Geçerli gün** seçeneğini belirleyin. Varsayılan olarak, bitiş günü olarak bir sonraki gün ayarlanır.

    e. **dakika** bölümünü, geçerli tarih saati birkaç dakika geçecek şekilde güncelleştirin. Tetikleyicinin etkinleştirilmesi için, önce sizin değişiklikleri yayımlamanız gerekir. Birkaç dakika sonraya ayarlarsanız ve bu süre içinde değişiklikleri yayımlamazsanız, tetikleyici çalıştırması göremezsiniz.

    f. **Uygula**’yı seçin. 

    ![Tetikleyici özellikleri](./media/tutorial-copy-data-portal/set-trigger-properties.png)

    g. **Etkinleştirildi** seçeneğini belirleyin. Bunu devre dışı bırakabilir ve daha sonra bu onay kutusunu kullanarak etkinleştirebilirsiniz.

    h. **İleri**’yi seçin.

    ![Etkinleştirildi düğmesi](./media/tutorial-copy-data-portal/trigger-activiated-next.png)

    > [!IMPORTANT]
    > Her bir işlem hattı çalıştırması ile bir maliyet ilişkilendirildiğinden bitiş tarihini uygun bir şekilde ayarlayın. 
1. **Tetikleyici Çalıştırma Parametreleri** sayfasında uyarıyı gözden geçirin ve **Son**'u seçin. Bu örnekteki işlem hattı hiçbir parametre almaz. 

    ![Tetikleyici çalıştırması parametreleri](./media/tutorial-copy-data-portal/trigger-pipeline-parameters.png)

1. Değişikliği yayımlamak için **Tümünü Yayımla**'ya tıklayın. 

1. Tetiklenen işlem hattı çalıştırmalarını görmek için sol taraftaki **İzleyici** sekmesine gidin. 

    ![Tetiklenen işlem hattı çalıştırmaları](./media/tutorial-copy-data-portal/triggered-pipeline-runs.png)    
1. **İşlem Hattı Çalıştırmaları** görünümünden **Tetikleyici Çalıştırmaları** görünümüne geçmek için, **İşlem Hattı Çalıştırmaları**'nı ve ardından **Tetikleyici Çalıştırmaları**'nı seçin.
    
    ![Tetikleyici çalıştırmaları](./media/tutorial-copy-data-portal/trigger-runs-menu.png)
1. Listede tetikleyici çalıştırmalarını görürsünüz. 

    ![Tetikleyici çalıştırmaları listesi](./media/tutorial-copy-data-portal/trigger-runs-list.png)
1. Belirtilen bitiş saatine kadar **emp** tablosuna dakikada iki satır (her işlem hattı çalıştırması için) eklendiğini doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Kopyalama etkinliğiyle işlem hattı oluşturma.
> * İşlem hattında test çalıştırması yapma.
> * İşlem hattını el ile tetikleme.
> * İşlem hattını bir zamanlamaya göre tetikleme.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.


Verileri şirket içinden buluta kopyalamayı öğrenmek için aşağıdaki öğreticiye geçin: 

> [!div class="nextstepaction"]
>[Şirket içinden buluta veri kopyalama](tutorial-hybrid-copy-portal.md)
