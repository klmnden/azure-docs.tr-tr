---
title: Data - veri yönetimi ağ geçidi taşıma | Microsoft Docs
description: Şirket içi ve bulut arasında veri taşımak için veri ağ geçidi kurun ayarlayın. Veri Yönetimi ağ geçidi Azure Data Factory, verilerinizi taşımak için kullanın.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 93b63e7c657282fc0ef285054ba90c9d6bc310b6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [şirket içi arasında veri kopyalama ve Data Factory sürüm 2 kullanarak bulut](../tutorial-hybrid-copy-powershell.md).

Bu makalede, şirket içi veri depoları ve veri fabrikası kullanarak bulut veri depoları arasında veri tümleştirme genel bir bakış sağlar. Derlemeler [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale ve diğer veri fabrikası temel kavramları makaleler: [veri kümeleri](data-factory-create-datasets.md) ve [ardışık düzen](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
Şirket içi makinenizdeki bir şirket içi veri deposundaki/gruptan veri taşımayı etkinleştirmek için veri yönetimi ağ geçidi yüklemeniz gerekir. Ağ geçidi veri deposuna bağlanabileceği sürece ağ geçidi veri depolama alanı ile aynı makinede veya farklı bir makineye yüklenebilir.

> [!IMPORTANT]
> Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale veri yönetimi ağ geçidi hakkında ayrıntılı bilgi için. 

Aşağıdaki örneklerde bir şirket içi veri taşır bir komut zinciriyle data factory oluşturmak gösterilmiştir **SQL Server** Azure blob depolama veritabanına. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız.

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>İzlenecek yol: Bulut için şirket içi veri kopyalama
Bu kılavuzda aşağıdaki adımları uygulayın: 

1. Veri fabrikası oluşturma.
2. Veri Yönetimi ağ geçidi oluşturun. 
3. Kaynak ve havuz veri depoları için bağlantılı Hizmetleri oluşturun.
4. Girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma.
5. Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar
Bu kılavuzda başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

* **Azure aboneliği**.  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Bkz: [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) Ayrıntılar için makale.
* **Azure depolama hesabı**. Blob depolama alanı olarak kullandığınız bir **hedef/havuz** verileri depolamak Bu öğreticide. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
* **SQL Server**. Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız. 

## <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, Azure portalında adlı bir Azure Data Factory örneğini oluşturmak için kullandığınız **ADFTutorialOnPremDF**.

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. ' I tıklatın **kaynak oluşturma**, tıklatın **Intelligence + analiz**, tıklatıp **Data Factory**.

   ![Yeni->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. İçinde **yeni data factory** want **ADFTutorialOnPremDF** adı.

    ![Panosuna Ekle](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **veri fabrikası adı "ADFTutorialOnPremDF" kullanılabilir değil**, veri fabrikası (örneğin, yournameADFTutorialOnPremDF) adını değiştirin ve oluşturmayı yeniden deneyin. Bu öğreticide adımları uygularken ADFTutorialOnPremDF yerine bu adı kullanın.
   >
   > Data factory adı gelecekte bir **DNS** adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
   >
   >
4. Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin.
5. Mevcut bir **kaynak grubu** seçin ya da bir kaynak grubu oluşturun. Adlı bir kaynak grubu öğretici için oluşturduğunuz: **ADFTutorialResourceGroup**.
6. Tıklatın **oluşturma** üzerinde **yeni data factory** sayfası.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.
   >
   >
7. Oluşturma işlemi tamamlandıktan sonra gördüğünüz **Data Factory** sayfasında aşağıdaki resimde gösterildiği gibi:

   ![Data Factory giriş sayfası](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Ağ geçidi oluşturma
1. İçinde **Data Factory** sayfasında, **yazar ve dağıtma** başlatmak için döşeme **Düzenleyicisi** data factory için.

    ![Geliştir ve Dağıt Kutucuğu](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. Data Factory Düzenleyici'yi tıklatın **... Daha fazla** araç ve ardından **yeni veri ağ geçidi**. Alternatif olarak, sağ tıklayarak **Data Gateways** ağaç görünümünde ve tıklatın **yeni veri ağ geçidi**.

   ![Araç çubuğundaki yeni veri ağ geçidi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. İçinde **oluşturma** want **adftutorialgateway** için **adı**, tıklatıp **Tamam**.     

    ![Ağ geçidi sayfası oluşturma](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > Bu kılavuzda, yalnızca bir düğümle (şirket içi Windows makine) mantıksal ağ geçidi oluşturun. Ağ geçidi ile birden çok şirket içi makineler ilişkilendirerek veri yönetimi ağ geçidi ölçeklendirebilirsiniz. Ölçeklendirmek yukarı bir düğümde aynı anda çalıştırabilirsiniz veri taşıma işlerinin sayısını artırarak. Bu özellik, tek bir düğüme sahip mantıksal bir ağ geçidi için de kullanılabilir. Bkz: [ölçeklendirme veri yönetimi ağ geçidi Azure veri fabrikası'nda](data-factory-data-management-gateway-high-availability-scalability.md) Ayrıntılar için makale.  
4. İçinde **yapılandırma** sayfasında, **doğrudan bu bilgisayar Yükle**. Bu eylem ağ geçidi yükleme paketi indirir, yükler, yapılandırır ve bilgisayarda ağ geçidi kaydeder.  

   > [!NOTE]
   > Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı kullanın.
   >
   > Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
   >
   > Aynı Firefox (yükleme eklenti) yapın. Tıklatın **menü Aç** araç çubuğunda (**üç yatay çizgi** sağ üst köşesinde), tıklatın **eklentileri**, arama "ClickOnce" sözcüğünü, aşağıdakilerden birini seçin ClickOnce uzantıları ve yükleyin.    
   >
   >

    ![Ağ geçidi - yapılandırma sayfası](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Bu şekilde, indirme, yükleme, yapılandırma ve tek tek adımda ağ geçidini kaydetmek için en kolay yolu (tek tıklatmayla) olur. Gördüğünüz **Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi** uygulaması bilgisayarınızda yüklü. Yürütülebilir dosya bulabileceğiniz **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared**.

    Ayrıca indirebilir ve bu sayfada bağlantıları kullanarak ağ geçidini el ile yükleyin ve gösterilen anahtarı kullanarak kaydetmek **yeni anahtar** metin kutusu.

    Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) ağ geçidi hakkında tüm ayrıntılar için makalenin.

   > [!NOTE]
   > Yüklemek ve veri yönetimi ağ geçidi başarıyla yapılandırmak için yerel bilgisayarda yönetici olması gerekir. Ek kullanıcı eklemek **veri yönetimi ağ geçidi kullanıcıları** yerel Windows grubu. Bu grubun üyeleri, ağ geçidini yapılandırmak için veri yönetimi ağ geçidi Yapılandırma Yöneticisi aracını kullanabilirsiniz.
   >
   >
5. Birkaç dakika bekleyin ya da aşağıdaki bildirim iletisini görene kadar bekleyin:

    ![Ağ geçidi yükleme başarılı](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi** bilgisayarınızdaki uygulama. İçinde **arama** penceresinde, türü **veri yönetimi ağ geçidi** bu yardımcı program erişmek için. Yürütülebilir dosya bulabileceğiniz **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetim Gateway\2.0\Shared**

    ![Ağ geçidi Yapılandırma Yöneticisi](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Gördüğünüzü onaylayın `adftutorialgateway is connected to the cloud service` ileti. Durum çubuğu alt görüntüler **bulut hizmetine bağlı** ile birlikte bir **yeşil onay işareti**.

    Üzerinde **giriş** sekmesinde da aşağıdaki işlemleri yapabilirsiniz:

   * **Kayıt** kayıt düğmesini kullanarak Azure portalından bir anahtara sahip bir ağ geçidi.
   * **Durdur** veri yönetimi ağ geçidi ana bilgisayar, ağ geçidi makine üzerinde çalışan hizmeti.
   * **Zamanlama güncelleştirmeleri** günün belirli bir zamanda yüklenecek.
   * Ağ geçidi olduğu zaman görüntülemek **son güncelleştirme**.
   * Ağ geçidi için bir güncelleştirme yüklenebilmesi için süreyi belirtin.
8. **Ayarlar** sekmesine geçin. Belirtilen sertifika **sertifika** bölüm portalda belirttiğiniz şirket içi veri depolama alanı için kimlik bilgilerini şifreleme/şifre çözme için kullanılır. (isteğe bağlı) Tıklatın **değişiklik** kullanarak kendi sertifikanızı kullanmayı. Varsayılan olarak, ağ geçidinin Data Factory hizmeti tarafından otomatik olarak oluşturulan sertifikayı kullanır.

    ![Ağ geçidi sertifika yapılandırma](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Üzerinde aşağıdaki işlemleri yapabilirsiniz **ayarları** sekmesi:

   * Görüntülemek veya ağ geçidi tarafından kullanılan sertifikanın dışa aktarın.
   * Ağ Geçidi tarafından kullanılan HTTPS uç noktası değiştirin.    
   * Ağ Geçidi tarafından kullanılacak bir HTTP proxy ayarlayın.     
9. (isteğe bağlı) Geçiş **tanılama** sekmesi, onay **ayrıntılı günlük kaydını etkinleştir** ağ geçidi ile ilgili tüm sorunları gidermek için kullanabileceğiniz ayrıntılı günlük kaydını etkinleştirmek istiyorsanız, seçeneği. Günlük kaydı bilgileri bulunabilir **Olay Görüntüleyicisi'ni** altında **uygulama ve hizmet günlükleri** -> **veri yönetimi ağ geçidi** düğümü.

    ![Tanılama sekmesi](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Ayrıca aşağıdaki eylemleri gerçekleştirebilirsiniz **tanılama** sekmesi:

   * Kullanım **Bağlantıyı Sına** ağ geçidini kullanarak şirket içi veri kaynağına bölümü.
   * Tıklatın **günlüklerini görüntüle** veri yönetimi ağ geçidi günlüğüne bir Olay Görüntüleyici penceresinde görmek için.
   * Tıklatın **günlükleri Gönder** sorunları, sorun giderme kolaylaştırmak üzere Microsoft'a günlük son yedi gün içeren bir zip dosyası karşıya yüklemek için.
10. Üzerinde **tanılama** sekmesinde **Bağlantıyı Sına** bölümünde, select **SqlServer** veri deposu türü için veritabanı sunucusu, veritabanı adını adını girin kimlik doğrulama türünü belirtin, kullanıcı adı ve parola girin ve tıklatın **Test** ağ geçidi veritabanına bağlanıp bağlanamadığınızı test etmek için.
11. Web tarayıcısı ve de geçiş **Azure portal**, tıklatın **Tamam** üzerinde **yapılandırma** sayfa ve daha sonra **yeni veri ağ geçidi** sayfası.
12. Görmeniz gerekir **adftutorialgateway** altında **Data Gateways** soldaki ağaç görünümünde.  Tıklarsanız, ilişkili JSON görmeniz gerekir.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, iki bağlı hizmet oluşturma: **AzureStorageLinkedService** ve **SqlServerLinkedService**. **SqlServerLinkedService** bir şirket içi SQL Server veritabanına bağlar ve **AzureStorageLinkedService** bağlantılı hizmeti bir Azure blob deposu data factory bağlar. Azure blob deposuna şirket içi SQL Server veritabanından veri kopyalayan daha sonra bu kılavuzda bir işlem hattı oluşturun.

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanına bağlı hizmet ekleme
1. İçinde **Data Factory düzenleyici**, tıklatın **yeni veri deposu** seçin ve araç **SQL Server**.

   ![Yeni SQL Server bağlantılı hizmeti](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. İçinde **JSON Düzenleyicisi** sağ tarafta aşağıdaki adımları uygulayın:

   1. İçin **gatewayName**, belirtin **adftutorialgateway**.    
   2. İçinde **connectionString**, aşağıdaki adımları uygulayın:    

      1. İçin **servername**, SQL Server veritabanını barındıran sunucunun adını girin.
      2. İçin **databasename**, veritabanının adını girin.
      3. Tıklatın **şifrele** araç çubuğunda. Kimlik bilgileri Yöneticisi uygulama bakın.

         ![Kimlik bilgileri Yöneticisi uygulaması](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. İçinde **kimlik bilgilerini ayarlama** iletişim kutusunda, kimlik doğrulama türü, kullanıcı adı ve parola belirtin ve tıklatın **Tamam**. Bağlantı başarılı olursa, JSON içinde depolanan şifrelenmiş kimlik bilgileri ve iletişim kutusunu kapatır.
      5. Otomatik olarak kapalı, iletişim kutusu başlatılan boş tarayıcı sekmesinde kapatın ve Azure portal ile sekmesine geri alın.

         Bu kimlik bilgileri ağ geçidi makinesi üzerinde olan **şifrelenmiş** Data Factory hizmetine sahip olan bir sertifikayı kullanarak. Bunun yerine veri yönetimi ağ geçidi ile ilişkili sertifika kullanmak istiyorsanız bkz [kimlik bilgilerini güvenli bir şekilde ayarlamak](#set-credentials-and-security).    
   3. Tıklatın **dağıtma** hizmeti SQL Server dağıtımı için komut çubuğunda bağlı. Bağlantılı hizmet ağaç görünümünde görmeniz gerekir.

      ![SQL Server bağlantılı hizmet ağaç görünümünde](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Bir Azure depolama hesabı için bağlı hizmet ekleme
1. İçinde **Data Factory düzenleyici**, tıklatın **yeni veri deposu** tıklatın ve komut çubuğunda **Azure depolama**.
2. Azure depolama hesabınızın adını girin **hesap adı**.
3. Azure depolama hesabınız için anahtarını girin **hesap anahtarı**.
4. Tıklatın **dağıtma** dağıtmak için **AzureStorageLinkedService**.

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda, kopyalama işlemi için girdi ve çıktı verilerini temsil eden girdi ve çıktı veri kümeleri oluşturursunuz (Şirket içi SQL Server veritabanı = > Azure blob depolama). Veri kümeleri oluşturmadan önce aşağıdaki adımları uygulayın (listeden sonra ayrıntılı adımlar verilir):

* Veri fabrikasına bağlı hizmet olarak eklediğiniz SQL Server Veritabanında **emp** adlı bir tablo oluşturun ve tabloya birkaç örnek giriş ekleyin.
* Veri fabrikasına bağlı hizmet olarak eklediğiniz Azure blob depolama hesabında **adftutorial** adlı bir blob kapsayıcı oluşturun.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Şirket içi SQL Server öğretici için hazırlama
1. Şirket içi SQL Server bağlı hizmeti için belirttiğiniz veritabanında (**SqlServerLinkedService**), aşağıdaki SQL betiğini kullanarak veritabanında **emp** tablosunu oluşturun.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Tabloya bazı örnekler ekleyin:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma

1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Daha fazla**, tıklatın **yeni veri kümesi** komut çubuğu ve tıklatın **SQL Server tablosuna**.
2. Sağ bölmedeki JSON aşağıdaki metinle değiştirin:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   Aşağıdaki noktalara dikkat edin:

   * **tür** ayarlanır **SqlServerTable**.
   * **tableName** ayarlanır **emp**.
   * **linkedServiceName** ayarlanır **SqlServerLinkedService** (Bu kılavuzda daha önce bu bağlı hizmetin oluşturmuştunuz.).
   * Azure Data Factory başka bir ardışık düzen tarafından üretilen olmayan bir giriş veri kümesi için ayarlamalısınız **dış** için **doğru**. Bu girdi verileri Azure Data Factory hizmetine dış üretilen gösterir. İsteğe bağlı olarak tüm dış veri ilkeleri kullanılarak belirtebilirsiniz **externalData** öğesinde **İlkesi** bölümü.    

   Bkz: [/SQL Server'dan veri taşıma](data-factory-sqlserver-connector.md) JSON özellikleri hakkında ayrıntılı bilgi için.
3. Tıklatın **dağıtma** veri kümesini dağıtmak için komut çubuğunda.  

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma

1. İçinde **Data Factory düzenleyici**, tıklatın **yeni veri kümesi** komut çubuğu ve tıklatın **Azure Blob Depolama**.
2. Sağ bölmedeki JSON aşağıdaki metinle değiştirin:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Aşağıdaki noktalara dikkat edin:

   * **tür** ayarlanır **AzureBlob**.
   * **linkedServiceName** ayarlanır **AzureStorageLinkedService** (Bu bağlı hizmeti 2. adımda oluşturmuştunuz).
   * **folderPath** ayarlanır **adftutorial/outfromonpremdf** outfromonpremdf adftutorial kapsayıcı klasöründe olduğu. Oluşturma **adftutorial** zaten yoksa, kapsayıcı.
   * **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmetinin her saat bir çıktı veri dilimi oluşturur **emp** Azure SQL veritabanı tablosunda.

   Belirtmezseniz, bir **fileName** için bir **çıktı tablosu**, de oluşturulan dosyaları **folderPath** şu biçimde adlandırılır: Data.<Guid>. txt (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   Ayarlamak için **folderPath** ve **fileName** göre dinamik olarak **SliceStart** zaman, partitionedBy özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2014-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2014/10/20, fileName de 08.csv olarak ayarlanır.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    Bkz: [/Azure Blob storage'da veri taşıma](data-factory-azure-blob-connector.md) JSON özellikleri hakkında ayrıntılı bilgi için.
3. Tıklatın **dağıtma** veri kümesini dağıtmak için komut çubuğunda. Her iki veri kümelerinin ağaç görünümünde gördüğünüzü onaylayın.  

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda, oluşturduğunuz bir **ardışık düzen** biriyle **kopyalama etkinliği** kullanan **EmpOnPremSQLTable** giriş olarak ve **OutputBlobTable** olarak çıktı.

1. Veri Fabrikası Düzenleyicisi'nde tıklatın **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın.
2. Sağ bölmedeki JSON aşağıdaki metinle değiştirin:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on-prem SQL server to blob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin.
   >
   >

   Aşağıdaki noktalara dikkat edin:

   * Etkinlikler bölümünde, yalnızca etkinliği yoktur, **türü** ayarlanır **kopya**.
   * **Giriş** etkinlik için **EmpOnPremSQLTable** ve **çıkış** etkinlik için **OutputBlobTable**.
   * İçinde **typeProperties** bölümünde **SqlSource** olarak belirtilen **kaynak türünü** ve ** BlobSink ** olarak belirtilen **Havuz türü**.
   * SQL sorgusu `select * from emp` için belirtilen **sqlReaderQuery** özelliği **SqlSource**.

   Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2014-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız.

   **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9/9/9999** olarak ayarlayın.

   Temel veri dilimlerinin işlenir süre tanımlama **kullanılabilirlik** her Azure Data Factory veri kümesi için tanımlanan özellikler.

   Örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.        
3. Tıklatın **dağıtma** veri kümesini dağıtmak için komut çubuğunda (tablo dikdörtgen bir veri kümesi olur). İşlem hattını ağaç görünümünde altında gösterildiğini onaylayın **ardışık düzen** düğümü.  
4. Şimdi, **X** iki kez geri almak için sayfayı kapatmak için **Data Factory** için sayfa **ADFTutorialOnPremDF**.

**Tebrikler!** Başarıyla bir Azure data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturulan ve işlem hattını zamanladınız.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Data factory’yi Diyagram Görünümünde görüntüleme
1. İçinde **Azure portal**, tıklatın **diyagramı** döşeme için giriş sayfasındaki **ADFTutorialOnPremDF** veri fabrikası. :

    ![Diyagram bağlantı](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Aşağıdaki görüntüye benzer bir diyagram görmeniz gerekir:

    ![Diyagram Görünümü](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Yakınlaştırma, uzaklaştırma, % 100, uygun, otomatik olarak ardışık düzen ve veri kümeleri, konum ve çizgileri gösterebilirsiniz (Seçilen öğelerin Yukarı Akış ve aşağı akış öğelerini vurgular) Yakınlaştır.  Bir nesne (giriş/çıkış veri kümesi veya işlem hattı) özelliklerini görmek için çift tıklatabilirsiniz.

## <a name="monitor-pipeline"></a>İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız. Veri kümelerini ve işlem hatlarını izlemek için de PowerShell cmdlet'lerini kullanabilirsiniz. İzleme hakkında daha fazla bilgi için bkz [izleme ve yönetme ardışık düzen](data-factory-monitor-manage-pipelines.md).

1. Diyagramda çift **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Tüm veri dilimi yukarı içinde olduğuna dikkat edin **hazır** (başlangıç saati bitiş saati için) ardışık düzen süresi geçmiş olduğundan belirtin. SQL Server veritabanında veri ekledikten ve bunu her zaman için de olabilir. Hiç dilim gösterilmediğini onaylayın **sorunu dilimleri** alt bölüm. Tüm dilimleri görmek için tıklatın **daha görmek** dilimler listesinin altındaki.
3. Şimdi **veri kümeleri** sayfasında, **OutputBlobTable**.

    ![OputputBlobTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Listeden herhangi bir veri dilimine tıklayın ve görmeniz gerekir **veri dilimi** sayfası. Dilimin etkinlik çalışır bakın. Yalnızca bir etkinlik genellikle bakın.  

    ![Veri dilimi dikey penceresi](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz.
5. Tıklatın **etkinlik** görmek için altındaki listeden **etkinlik çalışma ayrıntıları**.

   ![Etkinlik çalışma Ayrıntıları sayfası](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Verimlilik, süre ve veri aktarımı için kullanılan ağ geçidi gibi bilgileri görür.
6. Tıklatın **X** tüm sayfaları dek kapatmak için
7. Giriş sayfasına geri dönmek **ADFTutorialOnPremDF**.
8. (isteğe bağlı) ' I tıklatın **ardışık düzen**, tıklatın **ADFTutorialOnPremDF**ve detaya girdi tablolarında (**tüketilen**) veya çıkış veri kümeleri (**üretilen**).
9. Gibi araçlar kullanın [Microsoft Storage Gezgini](http://storageexplorer.com/) blob/dosya her saat oluşturulduğunu doğrulamak için.

   ![Azure Depolama Gezgini](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale için veri yönetimi ağ geçidi ile ilgili tüm ayrıntıları.
* Bkz: [kopyalama verileri Azure Blob'tan Azure SQL'e](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyalama etkinliği bir havuz veri deposu için bir kaynak veri deposundan verileri taşımak için nasıl kullanılacağı hakkında bilgi edinmek için.
