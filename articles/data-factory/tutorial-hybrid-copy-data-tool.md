---
title: Azure Veri Kopyalama aracını kullanarak şirket içi verileri kopyalama | Microsoft Docs
description: Şirket içi bir SQL Server veritabanındaki verileri Azure Blob depolamaya kopyalamak için bir Azure veri fabrikası oluşturun ve Veri Kopyalama aracını kullanın.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: hero-article
ms.date: 01/04/2018
ms.author: jingwang
ms.openlocfilehash: b080029cdf7e7200663830abad02ae7e61dbdb99
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053363"
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage-by-using-the-copy-data-tool"></a>Veri Kopyalama aracını kullanarak şirket içi bir SQL Server veritabanındaki verileri Azure Blob depolamaya kopyalama
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Geçerli sürüm](tutorial-hybrid-copy-data-tool.md)

Bu öğreticide, Azure portalını kullanarak bir veri fabrikası oluşturursunuz. Sonra, Veri Kopyalama aracını kullanarak şirket içi bir SQL Server veritabanındaki verileri Azure Blob depolamaya kopyalarsınız.

> [!NOTE]
> - Azure Data Factory’yi ilk kez kullanıyorsanız bkz. [Data Factory'ye giriş](introduction.md).
Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

## <a name="prerequisites"></a>Ön koşullar
### <a name="azure-subscription"></a>Azure aboneliği
Başlamadan önce, mevcut bir Azure aboneliğiniz yoksa [ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Azure rolleri
Veri fabrikası örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabına *katkıda bulunan* veya *sahip* rolü atanmalı veya bu hesap Azure aboneliğinin *yöneticisi* olmalıdır. 

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin. Sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için bkz. [Rol ekleme](../billing/billing-add-change-azure-subscription-administrator.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 ve 2017
Bu öğreticide, şirket içi SQL Server veritabanını bir *kaynak* veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. Daha sonra SQL Server veritabanınızda **emp** adlı bir tablo oluşturur ve tabloya birkaç örnek girdi eklersiniz. 

1. SQL Server Management Studio’yu başlatın. Makinenizde zaten yüklü değilse [SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sayfasına gidin. 

2. Kimlik bilgilerinizi kullanarak SQL Server örneğinize bağlanın. 

3. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'nı seçin. 

4. **Yeni Veritabanı** penceresinde, veritabanı için bir ad girin ve **Tamam**'ı seçin. 

5. **emp** tablosunu oluşturmak ve içine bazı örnek verileri eklemek için veritabanında aşağıdaki sorgu betiğini çalıştırın. Ağaç görünümünde, oluşturduğunuz veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

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

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir depolama hesabınız yoksa yeni hesap oluşturma yönergeleri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri şirket içi SQL Server veritabanından (kaynak) bu Blob depolama alanına (havuz) kopyalar. 

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

1. **Depolama hesabı** penceresinde **Genel Bakış**’a geçin ve sonra **Bloblar**’ı seçin. 

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)

2. **Blob hizmeti** penceresinde **Kapsayıcı**’yı seçin. 

    ![Kapsayıcı düğmesi](media/tutorial-hybrid-copy-powershell/add-container-button.png)

3. **Yeni kapsayıcı** penceresinde, **Ad** kutusuna **adftutorial** girin ve ardından **Tamam**’ı seçin. 

    ![Yeni kapsayıcıyı](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)

4. Kapsayıcılar listesinde **adftutorial**’ı seçin.

    ![Kapsayıcı seçimi](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)

5. **adftutorial** öğesine ait **Kapsayıcı** penceresini açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

    ![Kapsayıcı penceresi](media/tutorial-hybrid-copy-powershell/container-page.png)


## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüden **Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin. 
  
   ![Yeni veri fabrikası oluşturma](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin. 
   
     ![Yeni veri fabrikası](./media/tutorial-hybrid-copy-data-tool/new-azure-data-factory.png)

   Veri fabrikasının adı *genel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

   ![Yeni veri fabrikasının adı](./media/tutorial-hybrid-copy-data-tool/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
  
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
        
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).
5. **Sürüm** bölümünde **V2**'yi seçin.
6. **Konum** bölümünde veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri Fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka konumlarda/bölgelerde olabilir.
7. **Panoya sabitle**’yi seçin. 
8. **Oluştur**’u seçin.
9. Panoda, **Data Factory Dağıtılıyor** durumuna sahip aşağıdaki kutucuğu görürsünüz:

    ![Veri fabrikası dağıtılıyor kutucuğu](media/tutorial-hybrid-copy-data-tool/deploying-data-factory.png)
10. Oluşturma işlemi bittikten sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfası görüntülenir.
  
    ![Data factory giriş sayfası](./media/tutorial-hybrid-copy-data-tool/data-factory-home-page.png)
11. Azure Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için **Geliştir ve İzle**’yi seçin. 

## <a name="use-the-copy-data-tool-to-create-a-pipeline"></a>Veri Kopyalama aracını kullanarak işlem hattı oluşturma

1. **Başlayalım** sayfasında, Veri Kopyalama aracını açmak için **Veri Kopyala**’yı seçin. 

   ![Veri Kopyalama aracının kutucuğu](./media/tutorial-hybrid-copy-data-tool/copy-data-tool-tile.png)

2. Veri Kopyalama aracının **Özellikler** sayfasında, **Görev adı** bölümüne **CopyFromOnPremSqlToAzureBlobPipeline** adını girin. Sonra **İleri**’yi seçin. Veri Kopyalama aracı, bu alan için belirttiğiniz ada sahip bir işlem hattı oluşturur. 

   ![Görev adı](./media/tutorial-hybrid-copy-data-tool/properties-page.png)

3. **Kaynak veri deposu** sayfasında **Yeni bağlantı oluştur**'a tıklayın. 

   ![Yeni bağlı hizmet oluşturma](./media/tutorial-hybrid-copy-data-tool/create-new-source-data-store.png)

4. **Yeni Bağlı Hizmet** bölümünde **SQL Server** aratın ve **İleri**'yi seçin. 

   ![SQL Server seçimi](./media/tutorial-hybrid-copy-data-tool/select-source-data-store.png)

5. Yeni Bağlantılı Hizmet (SQL Server) **Ad**** altına **SqlServerLinkedService** yazın. **Tümleştirme çalışma zamanı aracılığıyla bağlan** için **+Yeni** seçeneğini belirleyin. Şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup bunu makinenize indirmeniz ve Data Factory’ye kaydetmeniz gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanı, şirket içi ortamınızla bulut arasında veri kopyalar.

   ![Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-link.png)

6. **Tümleştirme Çalışma Zamanı Kurulumu** iletişim kutusunda **Özel Ağ**’ı seçin. Sonra **İleri**’yi seçin. 

   ![](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog0.png)

7. **Tümleştirme Çalışma Zamanı Kurulumu** iletişim kutusunda, **Ad** bölümüne **TutorialIntegrationRuntime** yazın. Sonra **İleri**’yi seçin. 

   ![Tümleştirme çalışma zamanı adı](./media/tutorial-hybrid-copy-data-tool/create-integration-runtime-dialog.png)

8. **Bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın** seçeneğini belirleyin. Bu işlem, tümleştirme çalışma zamanını makinenize yükler ve Data Factory’ye kaydeder. Alternatif olarak, el ile kurulum seçeneğini kullanarak yükleme dosyasını indirip çalıştırabilir ve anahtarı kullanarak tümleştirme çalışma zamanını kaydedebilirsiniz. 

    ![Bu bilgisayarda hızlı kurulum başlat bağlantısı](./media/tutorial-hybrid-copy-data-tool/launch-express-setup-link.png)

9. İndirilen uygulamayı çalıştırın. Pencerede hızlı kurulum durumunu görürsünüz. 

    ![Hızlı kurulum durumu](./media/tutorial-hybrid-copy-data-tool/express-setup-status.png)

10. **Integration Runtime** alanı için **TutorialIntegrationRuntime** seçeneğinin belirlendiğini onaylayın.

    ![Tümleştirme çalışma zamanı seçildi](./media/tutorial-hybrid-copy-data-tool/integration-runtime-selected.png)

11. **Şirket içi SQL Server veritabanını belirtin** bölümünde aşağıdaki adımları uygulayın: 

      a. **Ad** bölümüne **SqlServerLinkedService** adını girin.

      b. **Sunucu adı** bölümüne şirket içi SQL Server örneğinizin adını girin.

      c. **Veritabanı adı** bölümüne şirket içi veritabanınızın adını girin.

      d. **Kimlik doğrulaması türü** bölümünde uygun kimlik doğrulamasını seçin.

      e. **Kullanıcı adı** bölümüne şirket içi SQL Server’a erişimi olan kullanıcının adını girin.

      f. Kullanıcının **parolasını** girin. **Son**’u seçin. 

12. **İleri**’yi seçin.

     ![](./media/tutorial-hybrid-copy-data-tool/select-source-linked-service.png)

13. **Verilerin kopyalanacağı tabloları seçin veya özel sorgu kullanın** sayfasında, listeden **[dbo].[emp]** tablosunu seçip **İleri**’yi seçin. Veritabanınıza göre diğer tablolardan da seçim yapabilirsiniz.

     ![Ürün tablosu seçimi](./media/tutorial-hybrid-copy-data-tool/select-emp-table.png)

14. **Hedef veri deposu** sayfasında **Yeni bağlantı oluştur**'u seçin

     //image create-new-sink-connection.png

     ![Hedefe bağlı hizmet oluşturma](./media/tutorial-hybrid-copy-data-tool/create-new-sink-connection.png)

15. **Yeni Bağlı Hizmet** bölümünde **Azure Blob** arayıp seçin ve **Devam**'a tıklayın. 

     ![Blob depolama seçimi](./media/tutorial-hybrid-copy-data-tool/select-destination-data-store.png)

16. **Yeni Bağlı Hizmet (Azure Blob Depolama)** iletişim kutusunda aşağıdaki adımları uygulayın: 

     a. **Ad**** bölümüne **AzureStorageLinkedService** adını girin.

     b. **Tümleştirme çalışma zamanıyla bağlan** bölümünde **TutorialIntegrationRuntime** öğesini seçin

     c. **Depolama hesabı adı** bölümünde, açılan listeden depolama hesabınızı seçin. 

     d. **İleri**’yi seçin.

     ![Depolama hesabını belirtme](./media/tutorial-hybrid-copy-data-tool/specify-azure-blob-storage-account.png)

17. **Hedef veri deposu** iletişim kutusunda **İleri**'yi seçin. **Bağlantı özellikleri** bölümünde **Azure Blob Depolama** olarak **Azure depolama hizmeti**'ni seçin. **İleri**’yi seçin. 

     ![Bağlantı özellikleri](./media/tutorial-hybrid-copy-data-tool/select-connection-properties.png)

18. **Çıkış dosyasını veya klasörünü seçin** iletişim kutusunda, **Klasör yolu** bölümüne **adftutorial/fromonprem** yolunu girin. Ön koşulların bir parçası olarak **adftutorial** kapsayıcısını oluşturdunuz. Çıkış klasörü yoksa (bu örnekte **fromonprem**), Data Factory tarafından otomatik olarak oluşturulur. Ayrıca, **Gözat** düğmesini kullanarak blob depolamaya ve bunun kapsayıcılarına/klasörlerine göz atabilirsiniz. **Dosya adı** bölümünde değer belirtmezseniz varsayılan olarak kaynaktaki ad (bu örnekte **dbo.emp**) kullanılır.
           
     ![Çıktı dosyasını veya klasörünü seçin](./media/tutorial-hybrid-copy-data-tool/choose-output-file-folder.png)

19. **Dosya biçimi ayarları** iletişim kutusunda **İleri**’yi seçin. 

     ![Dosya biçimi ayarları sayfası](./media/tutorial-hybrid-copy-data-tool/file-format-settings-page.png)

20. **Ayarlar** iletişim kutusunda **İleri**’yi seçin. 

     ![Ayarlar sayfası](./media/tutorial-hybrid-copy-data-tool/settings-page.png)

21. **Özet** iletişim kutusunda tüm ayarların değerlerini gözden geçirin ve **İleri**’yi seçin. 

     ![Özet sayfası](./media/tutorial-hybrid-copy-data-tool/summary-page.png)

22. **Dağıtım** sayfasında, oluşturduğunuz işlem hattını veya görevi izlemek için **İzleyici**’yi seçin.

     ![Dağıtım sayfası](./media/tutorial-hybrid-copy-data-tool/deployment-page.png)

23. **İzleyici** sekmesinde, oluşturduğunuz işlem hattının durumunu görüntüleyebilirsiniz. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek ve işlem hattını yeniden çalıştırmak için **Eylem** sütunundaki bağlantıları kullanabilirsiniz. 

     ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-hybrid-copy-data-tool/monitor-pipeline-runs.png)

24. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Kopyalama işlemiyle ilgili ayrıntıları görmek için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. **İşlem Hattı Çalıştırmaları** görünümüne dönmek için üstteki **İşlem Hatları**'nı seçin.

     ![Etkinlik çalıştırmalarını izleme](./media/tutorial-hybrid-copy-data-tool/monitor-activity-runs.png)

25. Çıktı dosyasını **adftutorial** kapsayıcısının **fromonprem** klasöründe gördüğünüzü onaylayın. 

     ![Çıktı blobu](./media/tutorial-hybrid-copy-data-tool/output-blob.png)

26. Düzenleyici moduna geçmek için soldaki **Düzenle** sekmesini seçin. Düzenleyiciyi kullanarak araç tarafından oluşturulan bağlı hizmetleri, veri kümelerini ve işlem hatlarını güncelleştirebilirsiniz. Düzenleyicide açılan varlıkla ilişkili JSON kodunu görüntülemek için **Kod**’u seçin. Bu varlıklarım Data Factory kullanıcı arabiriminde nasıl düzenleneceği ile ilgili ayrıntılar için [bu öğreticinin Azure portalı sürümüne](tutorial-copy-data-portal.md) bakın.

     ![Düzenle sekmesi](./media/tutorial-hybrid-copy-data-tool/edit-tab.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, şirket içi bir SQL Server veritabanındaki verileri Blob depolama alanına kopyalar. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Veri Kopyalama aracını kullanarak bir işlem hattı oluşturun.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Data Factory tarafından desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Verilerin toplu olarak kaynaktan hedefe nasıl kopyalanacağı hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy-portal.md)
