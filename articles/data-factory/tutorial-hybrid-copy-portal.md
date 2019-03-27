---
title: Azure Data Factory kullanarak verileri SQL Server’dan Blob depolamaya kopyalama | Microsoft Docs
description: Azure Data Factory’de şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposundan buluta veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/11/2018
ms.author: jingwang
ms.openlocfilehash: f2dc2418354cb1083c02516fbcdea710a74152ad
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445421"
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Verileri şirket içi SQL Server veritabanından Azure Blob depolamaya kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak verileri şirket içi bir SQL Server veritabanından Azure Blob depolama alanına kopyalayan bir veri fabrikası işlem hattı oluşturursunuz. Verileri şirket içi ile bulut veri depoları arasında taşıyan, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup kullanabilirsiniz.

> [!NOTE]
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
Veri fabrikası örnekleri oluşturmak için Azure’da oturum açarken kullandığınız kullanıcı hesabına *Katkıda bulunan* veya *Sahip* rolü atanmalı ya da bu hesap Azure aboneliğinin *yöneticisi* olmalıdır. 

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin. Sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için, bkz. [RBAC ve Azure portalı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-portal.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 ve 2017
Bu öğreticide, şirket içi SQL Server veritabanını bir *kaynak* veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. Daha sonra SQL Server veritabanınızda **emp** adlı bir tablo oluşturur ve tabloya birkaç örnek girdi eklersiniz. 

1. SQL Server Management Studio’yu başlatın. Makinenizde zaten yüklü değilse [SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sayfasına gidin. 

1. Kimlik bilgilerinizi kullanarak SQL Server örneğinize bağlanın. 

1. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'nı seçin. 
1. **Yeni Veritabanı** penceresinde, veritabanı için bir ad girin ve **Tamam**'ı seçin. 

1. **emp** tablosunu oluşturmak ve içine bazı örnek verileri eklemek için veritabanında aşağıdaki sorgu betiğini çalıştırın:

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

1. Ağaç görünümünde, oluşturduğunuz veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa bkz. [Depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. 

#### <a name="get-the-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, depolama hesabınızın adını ve anahtarını kullanırsınız. Depolama hesabınızın adını ve anahtarını almak için aşağıdaki adımları gerçekleştirin: 

1. Azure kullanıcı adı ve parolanızla [Azure portalında](https://portal.azure.com) oturum açın. 

1. Sol bölmede **Diğer hizmetler**’i seçin. **Depolama** anahtar sözcüğünü kullanarak filtreleyin ve **Depolama hesapları**’nı seçin.

    ![Depolama hesabı araması](media/tutorial-hybrid-copy-powershell/search-storage-account.png)

1. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse). Sonra depolama hesabınızı seçin. 

1. **Depolama hesabı** penceresinde **Erişim anahtarları**'nı seçin.

    ![Erişim tuşları](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)

1. **Depolama hesabı adı** ve **key1** kutularında değerleri kopyalayın ve ardından onları öğreticide daha sonra kullanmak için Not Defteri'ne veya başka bir düzenleyiciye yapıştırın. 

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma 
Bu bölümde, Blob depolama alanınızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. 

1. **Depolama hesabı** penceresinde **Genel Bakış**’a gidip **Bloblar**’ı seçin. 

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)

1. **Blob hizmeti** penceresinde **Kapsayıcı**’yı seçin. 

    ![Kapsayıcı düğmesi](media/tutorial-hybrid-copy-powershell/add-container-button.png)

1. **Yeni kapsayıcı** penceresinde, **Ad** bölümüne **adftutorial** adını girin. Sonra **Tamam**’ı seçin. 

    ![Yeni kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

1. Kapsayıcılar listesinde **adftutorial**’ı seçin.

    ![Kapsayıcı seçimi](media/tutorial-hybrid-copy-powershell/select-adftutorial-container.png)

1. **adftutorial** öğesine ait **kapsayıcı** penceresini açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

    ![Kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. Sol menüden **kaynak Oluştur** > **veri ve analiz** > **Data Factory**:
   
   ![“Yeni” bölmesinde Data Factory seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
   
     ![Yeni veri fabrikası sayfası](./media/tutorial-hybrid-copy-portal/new-azure-data-factory.png)

Veri fabrikasının adı *genel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

   ![Yeni veri fabrikasının adı](./media/tutorial-hybrid-copy-portal/name-not-available-error.png)
1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
   
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.
        
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).
1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (örneğin, Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
1. **Panoya sabitle**’yi seçin. 
1. **Oluştur**’u seçin.
1. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz:

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-hybrid-copy-portal/deploying-data-factory.png)
1. Oluşturma işlemi bittikten sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfası görüntülenir:
   
    ![Data factory giriş sayfası](./media/tutorial-hybrid-copy-portal/data-factory-home-page.png)
1. Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğunu seçin. 


## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. Sizin için otomatik olarak bir işlem hattı oluşturulur. İşlem hattının ağaç görünümünde yer aldığını ve düzenleyicisinin açık olduğunu görürsünüz. 

   ![Başlayalım sayfası](./media/tutorial-hybrid-copy-portal/get-started-page.png)

1. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SQLServerToBlobPipeline** adını girin.

   ![İşlem hattı adı](./media/tutorial-hybrid-copy-portal/pipeline-name.png)

1. **Etkinlikler** araç kutusunda **Veri Akışı**’nı genişletin. **Kopyalama** etkinliğini kopyalayıp işlem hattı tasarım yüzeyine bırakın. Etkinliğin adını **CopySqlServerToAzureBlobActivity** olarak ayarlayın.

   ![Etkinlik adı](./media/tutorial-hybrid-copy-portal/copy-activity-name.png)

1. **Özellikler** penceresinde **Kaynak** sekmesine gidin ve **+ Yeni**’yi seçin.

   ![Kaynak sekmesi](./media/tutorial-hybrid-copy-portal/source-dataset-new-button.png)

1. **Yeni Veri Kümesi** penceresinde **SQL Server**’ı arayın. **SQL Server**’ı ve ardından **Son**’u seçin. **SqlServerTable1** başlıklı yeni bir sekme görürsünüz. Ayrıca, soldaki ağaç görünümünde **SqlServerTable1** veri kümesini görürsünüz. 

   ![SQL Server seçimi](./media/tutorial-hybrid-copy-portal/select-sql-server.png)

1. **Özellikler** penceresinin altındaki **Genel** sekmesinde **Ad** bölümüne **SqlServerDataset** adını girin.

   ![Kaynak veri kümesi adı](./media/tutorial-hybrid-copy-portal/source-dataset-name.png)

1. **Bağlantı** sekmesine gidip **+ Yeni**’yi seçin. Bu adımda, kaynak veri deposuna (SQL Server veritabanı) yönelik bir bağlantı oluşturursunuz. 

   ![Kaynak veri kümesine bağlantı](./media/tutorial-hybrid-copy-portal/source-connection-new-button.png)

1. **Yeni Bağlı Hizmet** penceresinde **Name** olarak **SqlServerLinkedService** girin. **Tümleştirme çalışma zamanı aracılığıyla bağlan** için **Yeni**'yi seçin. Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server veitabanınızdaki verileri Blob depolama alanına kopyalayan bileşendir. 

   ![Yeni tümleştirme çalışma zamanı](./media/tutorial-hybrid-copy-portal/new-integration-runtime-button.png)

1. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Özel Ağ**’ı seçip **İleri**’yi seçin. 

   ![Özel ağ seçimi](./media/tutorial-hybrid-copy-portal/select-private-network.png)

1. Tümleştirme çalışma zamanı için bir ad girin ve **İleri**’yi seçin.

    ![Tümleştirme çalışma zamanı adı](./media/tutorial-hybrid-copy-portal/integration-runtime-name.png)

1. Altında **seçenek 1: Hızlı Kurulum**seçin **bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın**. 

    ![Hızlı kurulum bağlantısı](./media/tutorial-hybrid-copy-portal/click-express-setup.png)

1. **Integration Runtime (Şirket İçinde Barındırılan) Hızlı Kurulum** penceresinde **Kapat**’ı seçin. 

    ![Integration runtime (şirket içinde barındırılan) hızlı kurulum](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)

1. **Yeni Bağlı Hizmet** penceresinde **Tümleştirme çalışma zamanı ile bağlan** bölümünde yukarıda oluşturulan **Tümleştirme Çalışma Zamanı**'nın seçili olduğundan emin olun. 

    ![](./media/tutorial-hybrid-copy-portal/select-integration-runtime.png)

1. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    a. **Ad** bölümüne **SqlServerLinkedService** adını girin.

    b. **Tümleştirme çalışma zamanı aracılığıyla bağlan** bölümünde daha önce oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanının göründüğünü onaylayın.

    c. **Sunucu adı** bölümüne SQL Server örneğinizin adını girin. 

    d. **Veritabanı adı** alanında, **emp** tablosunu içeren veritabanının adını belirtin.

    e. **Kimlik doğrulaması türü** bölümünde, Data Factory’nin SQL Server veritabanınıza bağlanmak için kullanması gereken uygun kimlik doğrulaması türünü seçin.

    f. **Kullanıcı adı** ve **Parola** bölümlerine kullanıcı adını ve parolasını girin. Kullanıcı hesabınızda veya sunucu adında ters eğik çizgi karakteri (\\) kullanmanız gerekirse önüne kaçış karakterini (\\) koyun. Örneğin, *etkialanim\\\\kullanicim* şeklinde kullanın.

    g. **Bağlantıyı sına**’yı seçin. Data Factory’nin oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanı aracılığıyla SQL Server veritabanınıza bağlanabildiğini onaylamak için bu adımı uygulayın.

    h. Bağlı hizmeti kaydetmek için **Son**’u seçin.

       

1. Kaynak veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Özellikler** penceresinin **Bağlantı** sekmesinde aşağıdaki adımları uygulayın: 

    a. **Bağlı hizmet** bölümünde **SqlServerLinkedService**’i gördüğünüzü onaylayın.

    b. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin.

    ![Kaynak veri kümesi bağlantı bilgileri](./media/tutorial-hybrid-copy-portal/source-dataset-connection.png)

1. **SQLServerToBlobPipeline**’ı içeren sekmeye gidin veya ağaç görünümünden **SQLServerToBlobPipeline**’ı seçin. 

    ![İşlem hattı sekmesi](./media/tutorial-hybrid-copy-portal/pipeline-tab.png)

1. **Özellikler** penceresinin altındaki **Havuz** sekmesine gidin ve **+ Yeni**’yi seçin. 

    ![Havuz sekmesi](./media/tutorial-hybrid-copy-portal/sink-dataset-new-button.png)

1. **Yeni Veri Kümesi** penceresinde **Azure Blob Depolama Alanı**’nı seçin. Ardından **Son**’u seçin. Veri kümesi için yeni bir sekme açıldığını görürsünüz. Ayrıca, veri kümesini ağaç görünümünde de görebilirsiniz. 

    ![Blob depolama seçimi](./media/tutorial-hybrid-copy-portal/select-azure-blob-storage.png)

1. **Ad** bölümüne **AzureBlobDataset** adını girin.

    ![Havuz veri kümesi adı](./media/tutorial-hybrid-copy-portal/sink-dataset-name.png)

1. **Özellikler** penceresinin altındaki **Bağlantı** sekmesine gidin. **Bağlı hizmet**’in yanından **+ Yeni**’yi seçin. 

    ![New Linked Service (Yeni bağlı hizmet) düğmesi](./media/tutorial-hybrid-copy-portal/new-storage-linked-service-button.png)

1. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın:

    a. **Ad** bölümüne **AzureStorageLinkedService** adını girin.

    b. **Depolama hesabı adı** bölümünde depolama hesabınızı seçin.

    c. Depolama hesabı bağlantınızı test etmek için **Bağlantıyı sına**’yı seçin.

    d. **Kaydet**’i seçin.

    ![Depolama bağlı hizmeti ayarları](./media/tutorial-hybrid-copy-portal/azure-storage-linked-service-settings.png) 

1. Havuz veri kümesinin açık olduğu pencereye dönmeniz gerekir. **Bağlantı** sekmesinde aşağıdaki adımları uygulayın: 

    a. **Bağlı hizmet** bölümünde **AzureStorageLinkedService**’in seçildiğini onaylayın.

    b. **Dosya yolu**'nun **klasör**/ **Dizin** bölümüne **adftutorial/fromonprem** yazın. Çıktı klasörü adftutorial kapsayıcısında mevcut değilse Data Factory tarafından otomatik olarak oluşturulur.

    c. **Dosya yolu**'nun **dosya adı** bölümü için **Dinamik içerik ekle**'yi seçin.   

    ![Dinamik dosya adı değeri](./media/tutorial-hybrid-copy-portal/file-name.png)

    d. `@CONCAT(pipeline().RunId, '.txt')` ekleyip **Son**'u seçin. Bu işlemin ardından dosya PipelineRunID.txt olarak yeniden adlandırılır. 

    ![Dosya adını çözümlemek için dinamik ifade](./media/tutorial-hybrid-copy-portal/add-dynamic-file-name.png)

    ![Havuz veri kümesine bağlantı](./media/tutorial-hybrid-copy-portal/sink-dataset-connection.png)

1. İşlem hattının açık olduğu sekmeye gidin veya ağaç görünümünde işlem hattını seçin. **Havuz Veri Kümesi** bölümünde **AzureBlobDataset**’in seçili olduğunu onaylayın. 

    ![Havuz veri kümesi seçildi](./media/tutorial-hybrid-copy-portal/sink-dataset-selected.png)

1. İşlem hattı ayarlarını doğrulamak için işlem hattının araç çubuğunda **Doğrula**’yı seçin. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **Kapat**’ı seçin. 

    ![İşlem hattını doğrulama](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)

1. Oluşturduğunuz varlıkları Data Factory’de yayımlamak için **Tümünü Yayımla**’yı seçin.

    ![Yayımla düğmesi](./media/tutorial-hybrid-copy-portal/publish-button.png)

1. **Yayımlama başarılı** yazan açılan pencereyi görene kadar bekleyin. Yayımlama durumunu denetlemek için soldaki **Bildirimleri Göster** bağlantısını seçin. Bildirim penceresini kapatmak için **Kapat**’ı seçin. 

    ![Yayımlama başarılı](./media/tutorial-hybrid-copy-portal/publishing-succeeded.png)


## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme
Araç çubuğunda işlem hattı için **Tetikleyici**’yi ve sonra **Şimdi Tetikle**’yi seçin.

![İşlem hattı çalıştırması tetikleme](./media/tutorial-hybrid-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine gidin. Önceki adımda el ile tetiklediğiniz işlem hattını görürsünüz. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görüntülemek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. İşlem hattında tek bir etkinlik olduğundan yalnızca etkinlik çalıştırmalarını görürsünüz. Kopyalama işlemiyle ilgili ayrıntıları görmek için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstten **İşlem Hatları**'nı seçin.

    ![Etkinlik çalıştırmalarını izleme](./media/tutorial-hybrid-copy-portal/activity-runs.png)

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı, `adftutorial` blob kapsayıcısında *fromonprem* adlı çıktı klasörünü otomatik olarak oluşturur. Çıktı klasöründe *[pipeline().RunId].txt* dosyasını gördüğünüzü onaylayın. 

![Çıkış dosyası adını onaylama](./media/tutorial-hybrid-copy-portal/sink-output.png)


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
