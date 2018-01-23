---
title: "Azure Veri Kopyalama aracını kullanarak şirket içi verileri kopyalama | Microsoft Docs"
description: "Şirket içi bir SQL Server veritabanındaki verileri Azure Blob depolamaya kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: aba8b13d47b2b9236a0d5cc868e53780e894c406
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-copy-data-tool"></a>Veri Kopyalama aracını kullanarak şirket içi bir SQL Server veritabanındaki verileri Azure Blob depolamaya kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Sürüm 2 - Önizleme](tutorial-hybrid-copy-data-tool.md)

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak şirket içi bir SQL Server veritabanındaki verileri Azure blob depolamaya kopyalarsınız.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).
>
> - Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 ile çalışmaya başlama](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak işlem hattı oluşturma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

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

5. **Emp** tablosunu oluşturup bu tabloya örnek veri eklemek veritabanında şu sorgu betiğini çalıştırın: Ağaç görünümünde, oluşturduğunuz veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

    ```sql
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

### <a name="azure-storage-account"></a>Azure Storage hesabı
Bu öğreticide, genel amaçlı bir Azure depolama hesabını (özel olarak Blob depolamayı) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa yeni hesap oluşturma yönergeleri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri şirket içi SQL Server veritabanından (kaynak) bu Azure Blob depolama alanına (havuz) kopyalar. 

#### <a name="get-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdaki adımları gerçekleştirerek depolama hesabınızın adını ve anahtarını alın: 

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

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasına **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka konumlarda/bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın. 

## <a name="use-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. Veri Kopyalama aracını başlatmak için başlangıç sayfasında **Veri Kopyalama**’ya tıklayın. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)
2. Veri Kopyalama aracının **Özellikler** sayfasında, **Görev adı** olarak **CopyFromOnPremSqlToAzureBlobPipeline** adını belirtin ve **İleri**’ye tıklayın. Veri Kopyalama aracı, bu alan için belirttiğiniz ada sahip bir işlem hattı oluşturur. 
    
   ![Özellikler sayfası](./media/tutorial-hybrid-copy-data-tool/properties-page.png)
3. **Kaynak veri deposu** sayfasında **SQL Server**’ı seçip **İleri**’ye tıklayın. Listede **SQL Server**’ı görmek için aşağı kaydırmanız gerekebilir. 

   ![Kaynak veri deposu sayfası](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)
4. **Bağlantı adı** için **SqlServerLinkedService** adını girip **Tümleştirme Çalışma Zamanı Oluştur** bağlantısına tıklayın. Şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup bunu makinenize indirmeniz ve Data Factory hizmetine kaydetmeniz gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanı, şirket içi ortamınızla Azure bulut arasında veri kopyalar.

   ![Tümleştirme çalışma zamanı oluşturma bağlantısı](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)
5. **Tümleştirme Çalışma Zamanı Oluştur** iletişim kutusunda **Ad** alanı için **TutorialIntegration Runtime** ifadesini girip **Oluştur**’a tıklayın. 

   ![Tümleştirme çalışma zamanı oluşturma iletişim kutusu](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)
6. **Bu bilgisayarda hızlı kurulum başlat**’a tıklayın. Bu işlem, tümleştirme çalışma zamanını makinenize yükler ve Data Factory hizmetine kaydeder. Alternatif olarak, el ile kurulum seçeneğini kullanarak yükleme dosyasını indirip çalıştırabilir ve anahtarı kullanarak tümleştirme çalışma zamanını kaydedebilirsiniz. 

    ![Tümleştirme çalışma zamanı oluştur bağlantısı](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)
7. İndirilen uygulamayı çalıştırın. Pencerede hızlı kurulum **durumunu** görürsünüz. 

    ![Hızlı kurulum durumu](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)
8. **Tümleştirme Çalışma Zamanı** alanı için **TutorialIntegrationRuntime** seçeneğinin belirlendiğini onaylayın.

    ![Tümleştirme çalışma zamanı seçildi](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)
9. **Şirket içi SQL Server veritabanını belirtin** alanında aşağıdaki adımları uygulayın: 

    1. **Bağlantı adı** için **OnPremSqlLinkedService** adını girin.
    2. **Sunucu adı** için şirket içi SQL Server’ınızın adını girin.
    3. **Veritabanı adı** için şirket içi veritabanınızın adını girin. 
    4. **Kimlik doğrulaması türü** için uygun kimlik doğrulamasını seçin.
    5. **Kullanıcı adı** için şirket içi SQL Server’a erişimi olan kullanıcının adını girin.
    6. Kullanıcının **parolasını** girin. 
10. **Verilerin kopyalanacağı tabloları seçin veya özel sorgu kullanın** sayfasında, listeden **[dbo].[emp]** tablosunu seçip **İleri**’ye tıklayın. 

    ![Emp tablosunu seçin](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)
11. **Hedef veri deposu** sayfasında **Azure Blob Depolama**’yı seçip **İleri**’ye tıklayın.

    ![Azure Blob Depolama’yı seçin](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)
12. **Azure Blob depolama hesabı belirtin** sayfasında aşağıdaki adımları uygulayın: 

    1. **Bağlantı adı** için **AzureStorageLinkedService** adını girin.
    2. **Depolama hesabı adı** için açılan listeden Azure depolama hesabınızı seçin. 
    3. **İleri**’ye tıklayın.

        ![Azure Blob Depolama’yı seçin](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)
13. **Çıktı dosyasını veya klasörünü seçin** sayfasında aşağıdaki adımları uygulayın: 
    
    1. **Klasör yolu** için **adftutorial/fromonprem** yolunu girin. Ön koşulların bir parçası olarak **adftutorial** kapsayıcısını oluşturdunuz. Çıktı klasörü mevcut değilse Data Factory hizmeti tarafından otomatik olarak oluşturulur. Ayrıca, **Gözat** düğmesini kullanarak blob depolamaya ve bunun kapsayıcılarına/klasörlerine gidebilirsiniz. Varsayılan olarak çıktı dosyasının adının **dbo.emp** olarak ayarlandığına dikkat edin.
        
        ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)
14. **Dosya biçimi ayarları** sayfasında **İleri**’ye tıklayın. 

    ![Dosya biçimi ayarları sayfası](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)
15. **Ayarlar** sayfasında **İleri**’ye tıklayın. 

    ![Ayarlar sayfası](./media/tutorial-hybrid-copy-data-tool/settings-page.png)
16. **Özet** sayfasında tüm ayarların değerlerini gözden geçirin ve **İleri**’ye tıklayın. 

    ![Özet sayfası](./media/tutorial-hybrid-copy-data-tool/summary-page.png)
17. **Dağıtım** sayfasında, oluşturduğunuz işlem hattını veya görevi izlemek için **İzleyici**’ye tıklayın.

    ![Dağıtım sayfası](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)
18. **İzleyici** sekmesinde, oluşturduğunuz işlem hattının durumunu görüntüleyebilirsiniz. **Eylem** sütunundaki bağlantılar, işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemenize ve işlem hattını yeniden çalıştırmanıza imkan tanır. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)
19. İşlem hattıyla ilişkili tüm etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Göster** bağlantısına tıklayın. Kopyalama işlemiyle ilgili ayrıntıları görmek için **Eylemler** sütunundaki **Ayrıntılar** bağlantısına (gözlük simgesi) tıklayın. Üst taraftan **İşlem hatları**’na tıklayarak işlem hattı çalıştırmaları görünümüne dönebilirsiniz.

    ![Etkinlik çalıştırmaları](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)
20. Çıktı dosyasını **adftutorial** kapsayıcısının **fromonprem** klasöründe gördüğünüzü onaylayın.   
 
    ![Çıktı blobu](./media/tutorial-hybrid-copy-data-tool/output-blob.png)
21. Düzenleyici moduna geçmek için soldaki **Düzenle** sekmesine tıklayın. Düzenleyiciyi kullanarak araç tarafından oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Düzenleyicide açılan varlıkla ilişkili JSON kodunu görüntülemek için **Kod**’a tıklayın. Bu varlıkları Data Factory kullanıcı arabiriminde düzenlemeyle ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

    ![Düzenle sekmesi](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, şirket içi bir SQL Server veritabanındaki verileri bir Azure blob depolama alanına kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak işlem hattı oluşturma
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Data Factory tarafından desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Kaynaktan hedefe verileri toplu olarak kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy-portal.md)