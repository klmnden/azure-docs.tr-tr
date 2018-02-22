---
title: "Azure Data Factory kullanarak verileri SQL Server’dan Blob depolamaya kopyalama | Microsoft Docs"
description: "Azure Data Factory’de şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposundan buluta veri kopyalama hakkında bilgi edinin."
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
ms.openlocfilehash: ced708febe848d4555429b78c0227a35b7f0c79f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Verileri şirket içi SQL Server veritabanından Azure Blob depolamaya kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak verileri şirket içi bir SQL Server veritabanından Azure Blob depolama alanına kopyalayan bir veri fabrikası işlem hattı oluşturursunuz. Verileri şirket içi ile bulut veri depoları arasında taşıyan, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup kullanabilirsiniz.

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Data Factory’nin genel kullanıma açık 1. sürümünü kullanıyorsanız bkz. [Data Factory sürüm 1 için belgeler](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Bu makale, Data Factory’ye giriş konusunda ayrıntılı bilgi sağlamaz. Daha fazla bilgi için bkz. [Data Factory'ye giriş](introduction.md). 

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
Veri fabrikası örnekleri oluşturmak için Azure’da oturum açarken kullandığınız kullanıcı hesabına *katkıda bulunan* veya *sahip* rolü atanmalı ya da bu hesap Azure aboneliğinin *yöneticisi* olmalıdır. 

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin. Sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için bkz. [Rol ekleme](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 ve 2017
Bu öğreticide, şirket içi SQL Server veritabanını bir *kaynak* veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. Daha sonra SQL Server veritabanınızda **emp** adlı bir tablo oluşturur ve tabloya birkaç örnek girdi eklersiniz. 

1. SQL Server Management Studio’yu başlatın. Makinenizde zaten yüklü değilse [SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sayfasına gidin. 

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

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. 

#### <a name="get-the-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, depolama hesabınızın adını ve anahtarını kullanırsınız. Depolama hesabınızın adını ve anahtarını almak için aşağıdaki adımları gerçekleştirin: 

1. Azure kullanıcı adı ve parolanızla [Azure portalında](https://portal.azure.com) oturum açın. 

2. Sol bölmede **Diğer hizmetler**’i seçin. **Depolama** anahtar sözcüğünü kullanarak filtreleyin ve **Depolama hesapları**’nı seçin.

    ![Depolama hesabı araması](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse). Sonra depolama hesabınızı seçin. 

4. **Depolama hesabı** penceresinde **Erişim anahtarları**'nı seçin.

    ![Erişim tuşları](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

5. **Depolama hesabı adı** ve **key1** kutularında değerleri kopyalayın ve ardından onları öğreticide daha sonra kullanmak için Not Defteri'ne veya başka bir düzenleyiciye yapıştırın. 

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma 
Bu bölümde, Blob depolama alanınızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. 

1. **Depolama hesabı** penceresinde **Genel Bakış**’a gidip **Bloblar**’ı seçin. 

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. **Blob hizmeti** penceresinde **Kapsayıcı**’yı seçin. 

    ![Kapsayıcı düğmesi](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. **Yeni kapsayıcı** penceresinde, **Ad** bölümüne **adftutorial** adını girin. Sonra **Tamam**’ı seçin. 

    ![Yeni kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Kapsayıcılar listesinde **adftutorial**’ı seçin.

    ![Kapsayıcı seçimi](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. **adftutorial** öğesine ait **kapsayıcı** penceresini açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

    ![Kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. Soldaki menüden **Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin.
   
   ![Yeni veri fabrikası oluşturma](./media/tutorial-hybrid-copy-portal/new-azure-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)
 
   Veri fabrikasının adı *genel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).
  
   ![Yeni veri fabrikasının adı](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).
6. **Sürüm** bölümünde **V2 (Önizleme)** seçeneğini belirleyin.
7. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (örneğin, Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
8. **Panoya sabitle**’yi seçin. 
9. **Oluştur**’u seçin.
10. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz:

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
11. Oluşturma işlemi bittikten sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfası görüntülenir:
   
    ![Data factory giriş sayfası](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
12. Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğunu seçin. 


## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. Sizin için otomatik olarak bir işlem hattı oluşturulur. İşlem hattının ağaç görünümünde yer aldığını ve düzenleyicisinin açık olduğunu görürsünüz. 

   ![Başlayalım sayfası](./media/tutorial-hybrid-copy-portal/get-started-page.png)
2. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SQLServerToBlobPipeline** adını girin.

   ![İşlem hattı adı](./media/tutorial-hybrid-copy-portal/pipeline-name.png)
3. **Etkinlikler** araç kutusunda **Veri Akışı**’nı genişletin. **Kopyalama** etkinliğini kopyalayıp işlem hattı tasarım yüzeyine bırakın. Etkinliğin adını **CopySqlServerToAzureBlobActivity** olarak ayarlayın.

   ![Etkinlik adı](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)
4. **Özellikler** penceresinde **Kaynak** sekmesine gidin ve **+ Yeni**’yi seçin.

   ![Kaynak sekmesi](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)
5. **Yeni Veri Kümesi** penceresinde **SQL Server**’ı arayın. **SQL Server**’ı ve ardından **Son**’u seçin. **SqlServerTable1** başlıklı yeni bir sekme görürsünüz. Ayrıca, soldaki ağaç görünümünde **SqlServerTable1** veri kümesini görürsünüz. 

   ![SQL Server seçimi](./media/tutorial-hybrid-copy-portal/select-sql-server.png)
6. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SqlServerDataset** adını girin.
    
   ![Kaynak veri kümesi adı](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)
7. **Bağlantı** sekmesine gidip **+ Yeni**’yi seçin. Bu adımda, kaynak veri deposuna (SQL Server veritabanı) yönelik bir bağlantı oluşturursunuz. 

   ![Kaynak veri kümesine bağlantı](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)
8. **Yeni Bağlı Hizmet** penceresinde **Yeni Integration Runtime**’ı seçin. Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server veitabanınızdaki verileri Blob depolama alanına kopyalayan bileşendir. 

   ![Yeni tümleştirme çalışma zamanı](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)
9. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Özel Ağ**’ı seçip **İleri**’yi seçin. 

   ![Özel ağ seçimi](./media/tutorial-hybrid-copy-portal/select-private-network.png)
10. Tümleştirme çalışma zamanı için bir ad girin ve **İleri**’yi seçin.
    
    ![Tümleştirme çalışma zamanı adı](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)
11. **1. Seçenek: Hızlı kurulum** bölümünde **Bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın** seçeneğini belirleyin. 

    ![Hızlı kurulum bağlantısı](./media/tutorial-hybrid-copy-portal/click-exress-setup.png)
12. **Integration Runtime (Şirket İçinde Barındırılan) Hızlı Kurulum** penceresinde **Kapat**’ı seçin. 

    ![Integration runtime (şirket içinde barındırılan) hızlı kurulum](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)
13. Web tarayıcısında, **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Son**’u seçin. 

    ![Tümleştirme çalışma zamanı kurulumu](./media/tutorial-hybrid-copy-portal/click-finish-integration-runtime-setup.png)
14. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    a. **Ad** bölümüne **SqlServerLinkedService** adını girin.

    b. **Tümleştirme çalışma zamanı aracılığıyla bağlan** bölümünde daha önce oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanının göründüğünü onaylayın.

    c. **Sunucu adı** bölümüne SQL Server örneğinizin adını girin. 

    d. **Veritabanı adı** alanında, **emp** tablosunu içeren veritabanının adını belirtin.

    e. **Kimlik doğrulaması türü** bölümünde, Data Factory’nin SQL Server veritabanınıza bağlanmak için kullanması gereken uygun kimlik doğrulaması türünü seçin.

    f. **Kullanıcı adı** ve **Parola** bölümlerine kullanıcı adını ve parolasını girin. Kullanıcı hesabınızda veya sunucu adında ters eğik çizgi karakteri (\\) kullanmanız gerekirse önüne kaçış karakterini (\\) koyun. Örneğin, *etkialanim\\\\kullanicim* şeklinde kullanın.

    g. **Bağlantıyı sına**’yı seçin. Data Factory’nin oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanı aracılığıyla SQL Server veritabanınıza bağlanabildiğini onaylamak için bu adımı uygulayın.

    h. Bağlı hizmeti kaydetmek için **Kaydet**’i seçin.

       ![Yeni bağlı hizmet ayarları](./media/tutorial-hybrid-copy-portal/sql-server-linked-service-settings.png)
15. Kaynak veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Özellikler** penceresinin **Bağlantı** sekmesinde aşağıdaki adımları uygulayın: 

    a. **Bağlı hizmet** bölümünde **SqlServerLinkedService**’i gördüğünüzü onaylayın.

    b. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin.

    ![Kaynak veri kümesi bağlantı bilgileri](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)
16. **SQLServerToBlobPipeline**’ı içeren sekmeye gidin veya ağaç görünümünden **SQLServerToBlobPipeline**’ı seçin. 

    ![İşlem hattı sekmesi](./media/tutorial-hybrid-copy-portal/pipeliene-tab.png)
17. **Özellikler** penceresinin altındaki **Havuz** sekmesine gidin ve **+ Yeni**’yi seçin. 

    ![Havuz sekmesi](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)
18. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama Alanı**’nı seçin. Ardından **Son**’u seçin. Veri kümesi için yeni bir sekme açıldığını görürsünüz. Ayrıca, veri kümesini ağaç görünümünde de görebilirsiniz. 

    ![Blob depolama seçimi](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)
19. **Ad** bölümüne **AzureBlobDataset** adını girin.

    ![Havuz veri kümesi adı](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)
20. **Özellikler** penceresinin altındaki **Bağlantı** sekmesine gidin. **Bağlı hizmet**’in yanından **+ Yeni**’yi seçin. 

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)
21. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    a. **Ad** bölümüne **AzureStorageLinkedService** adını girin.

    b. **Depolama hesabı adı** bölümünde depolama hesabınızı seçin.

    c. Depolama hesabı bağlantınızı test etmek için **Bağlantıyı sına**’yı seçin.

    d. **Kaydet**’i seçin.

    ![Depolama bağlı hizmeti ayarları](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 
22.  Havuz veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Bağlantı** sekmesinde aşağıdaki adımları uygulayın: 

        a. **Bağlı hizmet** bölümünde **AzureStorageLinkedService**’in seçildiğini onaylayın.

        b. **Dosya yolu**’nun **klasör** bölümü için **adftutorial/fromonprem** yolunu girin. Çıktı klasörü adftutorial kapsayıcısında mevcut değilse Data Factory tarafından otomatik olarak oluşturulur.

        c. **Dosya yolu**’nun **dosya adı** bölümü için `@CONCAT(pipeline().RunId, '.txt')` adını girin.

     ![Havuz veri kümesine bağlantı](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)
23. İşlem hattının açık olduğu sekmeye gidin veya ağaç görünümünde işlem hattını seçin. **Havuz Veri Kümesi** bölümünde **AzureBlobDataset**’in seçili olduğunu onaylayın. 

    ![Havuz veri kümesi seçildi](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)
24. İşlem hattı ayarlarını doğrulamak için işlem hattının araç çubuğunda **Doğrula**’yı seçin. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **Kapat**’ı seçin. 

    ![İşlem hattını doğrulama](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
25. Oluşturduğunuz varlıkları Data Factory’de yayımlamak için **Tümünü Yayımla**’yı seçin.

    ![Yayımla düğmesi](./media/tutorial-hybrid-copy-portal/publish-button.png)
26. **Yayımlama başarılı** yazan açılan pencereyi görene kadar bekleyin. Yayımlama durumunu denetlemek için soldaki **Bildirimleri Göster** bağlantısını seçin. Bildirim penceresini kapatmak için **Kapat**’ı seçin. 

    ![Yayımlama başarılı](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)
    

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme
Araç çubuğunda işlem hattı için **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin.

![İşlem hattı çalıştırması tetikleme](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine gidin. Önceki adımda el ile tetiklediğiniz işlem hattını görürsünüz. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
2. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. İşlem hattında tek bir etkinlik olduğundan yalnızca etkinlik çalıştırmalarını görürsünüz. Kopyalama işlemiyle ilgili ayrıntıları görmek için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstten **İşlem Hatları**'nı seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı, `adftutorial` blob kapsayıcısında *fromonprem* adlı çıktı klasörünü otomatik olarak oluşturur. Çıktı klasöründe *dbo.emp.txt* dosyasını gördüğünüzü onaylayın. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı penceresinde **Yenile**’yi seçin.

    ![Oluşturulan çıktı klasörü](media/tutorial-hybrid-copy-portal/fromonprem-folder.png)
2. Klasör listesinde `fromonprem` seçeneğini belirleyin. 
3. `dbo.emp.txt` adlı bir dosya gördüğünüzü onaylayın.

    ![Çıktı dosyası](media/tutorial-hybrid-copy-portal/fromonprem-file.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Depolama bağlı hizmetlerini oluşturun. 
> * SQL Server ve Blob depolama veri kümeleri oluşturun.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

Data Factory tarafından desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Verilerin toplu olarak kaynaktan hedefe nasıl kopyalanacağını öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy-portal.md)
