---
title: "Azure Data Factory kullanarak verileri SQL Server’dan Blob depolamaya kopyalama | Microsoft Docs"
description: "Azure Data Factory’de şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposundan Azure bulutuna veri kopyalama hakkında bilgi edinin."
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
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: 7b734a76545dbcbddac3c7ad7beae60d662a9129
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="tutorial-copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Öğretici: Verileri şirket içi SQL Server veritabanından Azure Blob depolamaya kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak verileri şirket içi bir SQL Server veritabanından Azure Blob depolama alanına kopyalayan bir veri fabrikası işlem hattı oluşturursunuz. Verileri şirket içi ile bulut veri depoları arasında taşıyan, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup kullanabilirsiniz. 

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.
> 
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md). 

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Azure Depolama bağlı hizmetlerini oluşturma. 
> * SQL Server ve Azure Blob veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

## <a name="prerequisites"></a>Ön koşullar
### <a name="azure-subscription"></a>Azure aboneliği
Başlamadan önce, mevcut bir Azure aboneliğiniz yoksa [ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Azure rolleri
Veri fabrikası örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabına *katkıda bulunan* veya *sahip* rolü atanmalı veya bu hesap Azure aboneliğinin *yöneticisi* olmalıdır. 

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin, sağ üst köşeden kullanıcı adınızı seçtikten sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için [Rol ekleme](../billing/billing-add-change-azure-subscription-administrator.md) makalesine bakın.

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 ve 2017
Bu öğreticide, şirket içi SQL Server veritabanını bir *kaynak* veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Azure Blob depolama alanına (havuz) kopyalar. Daha sonra SQL Server veritabanınızda **emp** adlı bir tablo oluşturur ve tabloya birkaç örnek girdi eklersiniz. 

1. SQL Server Management Studio’yu başlatın. Makinenizde zaten yüklü değilse [SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)’ye gidin. 

2. Kimlik bilgilerinizi kullanarak SQL Server örneğinize bağlanın. 

3. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'nı seçin. 
 
4. **Yeni Veritabanı** penceresinde, veritabanı için bir ad girin ve **Tamam**'ı seçin. 

5. **emp** tablosunu oluşturmak ve içine bazı örnek verileri eklemek için veritabanında aşağıdaki sorgu betiğini çalıştırın:

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

6. Ağaç görünümünde, oluşturduğunuz veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

### <a name="azure-storage-account"></a>Azure Storage hesabı
Bu öğreticide, genel amaçlı bir Azure depolama hesabını (özel olarak Blob depolamayı) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri şirket içi SQL Server veritabanından (kaynak) bu Azure Blob depolama alanına (havuz) kopyalar. 

#### <a name="get-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdakileri yaparak depolama hesabınızın adını ve anahtarını alın: 

1. Azure kullanıcı adı ve parolanızla [Azure portalında](https://portal.azure.com) oturum açın. 

2. Sol bölmede, **Depolama** anahtar sözcüğünü kullanarak **Diğer hizmetler** filtresini ve ardından **Depolama hesapları**’nı seçin.

    ![Depolama hesabını arama](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse) ve depolama hesabınızı seçin. 

4. **Depolama hesabı** penceresinde **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adını ve anahtarını alma](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. **Depolama hesabı adı** ve **key1** kutularında değerleri kopyalayın ve ardından onları öğreticide daha sonra kullanmak için Not Defteri'ne veya başka bir düzenleyiciye yapıştırın. 

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma 
Bu bölümde, Azure Blob depolama alanınızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. 

1. **Depolama hesabı** penceresinde **Genel Bakış**’a geçin ve sonra **Bloblar**’ı seçin. 

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. **Blob hizmeti** penceresinde **Kapsayıcı**’yı seçin. 

    ![Kapsayıcı ekle düğmesi](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. **Yeni kapsayıcı** penceresinde, **Ad** kutusuna **adftutorial** girin ve ardından **Tamam**’ı seçin. 

    ![Kapsayıcı adını girin](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Kapsayıcılar listesinde **adftutorial**’ı seçin.  

    ![Kapsayıcıyı seçin](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. **adftutorial** öğesine ait **kapsayıcı** penceresini açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

    ![Kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturur ve Azure Data Factory kullanıcı arabirimini başlatarak veri fabrikasında bir işlem hattı oluşturursunuz. 

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
        Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.      
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın. 


## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **Başlarken** sayfasında **İşlem hattı oluştur**’a tıklayın. Sizin için otomatik olarak bir işlem hattı oluşturulur. İşlem hattının ağaç görünümünde yer aldığını ve düzenleyicisinin açık olduğunu görürsünüz. 

   ![Başlarken sayfası](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. Alttaki **Özellikler** penceresinin **Genel** sekmesinde **Ad** için **SQLServerToBlobPipeline** adını girin.

   ![İşlem hattı adı](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
2. **Etkinlikler** araç kutusunda **Veri Akışı**’nı genişletin ve **Kopyala** etkinliğini sürükleyerek işlem hattı tasarım yüzeyine bırakın. Etkinliğin adını **CopySqlServerToAzureBlobActivity** olarak ayarlayın.

   ![Etkinlik adı](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
3. Özellikler penceresinde **Kaynak** sekmesine geçin ve **+ Yeni**’ye tıklayın.

   ![Yeni kaynak veri kümesi - düğme](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
4. **Yeni Veri Kümesi** penceresinde **SQL Server**’ı arayın ve **SQL Server**’ı seçip **Son**’a tıklayın. **SqlServerTable1** başlıklı yeni bir sekme görürsünüz. Ayrıca, soldaki ağaç görünümünde **SqlServerTable1** veri kümesini görürsünüz. 

   ![SQL Server'ı seçin](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
5. Özellikler penceresinin **Genel** sekmesinde **Ad** için **SqlServerDataset** adını girin.
    
   ![Kaynak Veri Kümesi - ad](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
6. **Bağlantılar** sekmesine geçip **+ Yeni**’ye tıklayın. Bu adımda, kaynak veri deposuna (SQL Server veritabanı) yönelik bir bağlantı oluşturursunuz. 

   ![Kaynak Veri Kümesi - yeni bağlantı düğmesi](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
7. **Yeni Bağlı Hizmet** penceresinde **Yeni tümleştirme çalışma zamanı**’na tıklayın. Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server veitabanınızdaki verileri Azure Blob depolama alanına kopyalayan bileşendir. 

   ![Yeni tümleştirme çalışma zamanı](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
8. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Özel Ağ**’ı seçip **İleri**’ye tıklayın. 

   ![Özel ağı seçin](./media/tutorial-hybrid-copy-portal/select-private-network.png)
9. Tümleştirme çalışma zamanı için bir ad girin ve **İleri**’ye tıklayın.  
    
   ![Tümleştirme Çalışma Zamanı - ad](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
10. **1. Seçenek: Hızlı kurulum** bölümünde **Bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın** seçeneğine tıklayın. 

   ![Hızlı kurulum bağlantısına tıklayın](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
11. **Tümleştirme Çalışma Zamanı (Şirket İçinde Barındırılan) Hızlı Kurulum** penceresinde **Kapat**’a tıklayın. 

   ![Tümleştirme çalışma zamanı kurulumu - başarılı](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
12. Web tarayıcısında, **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Son**’a tıklayın. **Yeni Bağlı Hizmet** penceresine dönmeniz gerekir.

   ![Tümleştirme çalışma zamanı kurulumu - son](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
13. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    1. **Ad** için **SqlServerLinkedService** adını girin.
    2. **Tümleştirme çalışma zamanı aracılığıyla bağlan** için daha önce oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanının göründüğünü onaylayın.
    3. **Sunucu adı** için SQL Server’ınızın adını belirtin. 
    4. **Veritabanı adı** alanı için **emp** tablosunu içeren veritabanının adını belirtin. 
    5. Data Factory’nin SQL Server veritabanınıza bağlanmak için kullanması gereken uygun **kimlik doğrulaması türünü** seçin. 
    6. **Kullanıcı adı** ve **parolayı** girin. Kullanıcı hesabınızda veya sunucu adında ters eğik çizgi karakteri (\\) kullanmanız gerekirse önüne kaçış karakterini (\\) koyun. Örneğin, *etkialanim\\\\kullanicim* şeklinde kullanın. 
    7. **Bağlantıyı sına**’ya tıklayın. Data Factory hizmetinin oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanı aracılığıyla SQL Server veritabanınıza bağlanabildiğini onaylamak için bu adımı uygulayın. 
    8. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın.

       ![SQL Server bağlı hizmeti - ayarlar](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
14. Kaynak veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Özellikler** penceresinin **Bağlantı** bölümünde aşağıdaki adımları uygulayın: 

    1. **Bağlı hizmet** için **SqlServerLinkedService**’i gördüğünüzü onaylayın. 
    2. **Tablo** için **[dbo].[emp]** seçeneğini belirleyin.

        ![Kaynak Veri Kümesi - bağlantı bilgileri](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
15. SQLServerToBlobPipeline’ı içeren sekmeye geçin (veya) ağaç görünümünden **SQLServerToBlobPipeline**’a tıklayın. 

    ![İşlem hattı sekmesi](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
16. **Özellikler** penceresinde **Havuz** sekmesine geçin ve **+ Yeni**’ye tıklayın. 

    ![Havuz veri kümesi - Yeni düğme](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
17. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama**’yı seçip **Son**’a tıklayın. Veri kümesi için yeni bir sekme açıldığını görürsünüz. Ayrıca, ağaç görünümünde veri kümesini görebilirsiniz. 

    ![Azure Blob Depolama’yı seçin](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
18. **Ad** için **AzureBlobDataset** adını girin.

    ![Havuz veri kümesi - ad](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
19. **Özellikler** penceresinde **Bağlantı** sekmesine geçin ve **Bağlı hizmet** için **+ Yeni**’ye tıklayın. 

    ![Yeni bağlı hizmet - düğme](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
20. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    1. **Ad** için **AzureStorageLinkedService** adını girin.
    2. **Depolama hesabı adı** için Azure depolama hesabınızı seçin. 
    3. **Bağlantıyı sına**’ya tıklayarak Azure depolama hesabınıza yönelik bağlantıyı sınayın.
    4. **Kaydet**’e tıklayın.

        ![Azure Depolama bağlı hizmeti - ayarlar](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
21.  Havuz veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Bağlantı** sekmesinde aşağıdaki adımları uygulayın: 

        1. **Bağlı hizmet** için **AzureStorageLinkedService**’in seçildiğini onaylayın.
        2. **Dosya yolu**’nun **klasör** bölümü için **adftutorial/fromonprem** yolunu girin. Çıktı klasörü adftutorial kapsayıcısında mevcut değilse Data Factory hizmeti tarafından otomatik olarak oluşturulur.
        3. **Dosya yolu**’nun **dosya adı** bölümü için `@CONCAT(pipeline().RunId, '.txt')` adını girin.

            ![Havuz veri kümesi - bağlantı](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
22. İşlem hattının açık olduğu sekmeye geçin (veya) **ağaç görünümünde** **işlem hattına** tıklayın. **Havuz Veri Kümesi** için **AzureBlobDataset**’in seçili olduğunu onaylayın. 

    ![Havuz veri kümesi seçildi ](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
23. İşlem hattı ayarlarını doğrulamak için işlem hattının araç çubuğunda Doğrula’ya tıklayın. Sağ köşedeki **X** simgesine tıklayarak **Hat Doğrulama Raporu**’nu kapatın. 

    ![İşlem hattını doğrulama](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
1. **Yayımla**’ya tıklayarak oluşturduğunuz varlıkları Azure Data Factory’de yayımlayın.

    ![Yayımla düğmesi](./media/tutorial-hybrid-copy-portal/publish-button.png)
24. **Yayımlama başarılı** açılan penceresini görene kadar bekleyin. Yayımlama durumunu soldaki **Bildirimleri Göster** bağlantısına tıklayarak da denetleyebilirsiniz. **X** simgesine tıklayarak bildirim penceresini kapatın. 

    ![Yayımlama başarılı](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırması tetikleme
İşlem hattının araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın.

![Şimdi Tetikle](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine geçin. Önceki adımda el ile tetiklediğiniz işlem hattını görürsünüz. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Göster** bağlantısına tıklayın. İşlem hattında tek bir etkinlik olduğundan yalnızca etkinlik çalıştırmalarını görürsünüz. Kopyalama işlemiyle ilgili ayrıntıları görmek için **Eylemler** sütunundaki **Ayrıntılar** bağlantısına (gözlük simgesi) tıklayın. Üst taraftan **İşlem hatları**’na tıklayarak işlem hattı çalıştırmaları görünümüne dönebilirsiniz.

    ![Etkinlik çalıştırmaları](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı, `adftutorial` blob kapsayıcısında *fromonprem* adlı çıktı klasörünü otomatik olarak oluşturur. Çıktı klasöründe *dbo.emp.txt* dosyasını gördüğünüzü onaylayın. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı penceresinde **Yenile**’yi seçin.

    ![Oluşturulan çıktı klasörü](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Klasör listesinde `fromonprem` seçeneğini belirleyin. 
3. `dbo.emp.txt` adlı bir dosya gördüğünüzü onaylayın.

    ![Çıktı dosyası](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Azure Depolama bağlı hizmetlerini oluşturma. 
> * SQL Server ve Azure Blob veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

Data Factory tarafından desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Kaynaktan hedefe verileri toplu olarak kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy-portal.md)
