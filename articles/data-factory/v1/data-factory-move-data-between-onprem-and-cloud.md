---
title: Verileri - veri yönetimi ağ geçidi taşıma | Microsoft Docs
description: Şirket içi ve bulut arasında veri taşımak için bir veri ağ geçidi ayarlama. Veri Yönetimi ağ geçidi, Azure Data Factory'de veri taşımak için kullanın.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 4eb881992b7e40e0a9d67bd2cee94f1f09958e9e
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59524115"
---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory kullanarak Bulut ve şirket içi arasında veri kopyalama](../tutorial-hybrid-copy-powershell.md).

Bu makale, Data Factory ile bulut veri depoları ile şirket içi veri depoları arasında veri tümleştirmesi genel bir bakış sağlar. Yapılar [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makale ve diğer data factory çekirdek kavramları makaleler: [veri kümeleri](data-factory-create-datasets.md) ve [işlem hatları](data-factory-create-pipelines.md).

## <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
Şirket içi makinenizde bir şirket içi veri deposuna/deposundan veri taşımayı etkinleştirmek için veri yönetimi ağ geçidi yüklemeniz gerekir. Ağ geçidi veri deposuna bağlanabilirsiniz sürece ağ geçidi, veri deposu ile aynı makinede veya farklı bir makineye yüklenebilir.

> [!IMPORTANT]
> Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri yönetimi ağ geçidi hakkında bilgi için makalenin. 

Aşağıdaki örneklerde, bir şirket içi veri taşıyan bir işlem hattıyla veri fabrikası oluşturma işlemini göstermektedir **SQL Server** veritabanına bir Azure blob depolama. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız.

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>İzlenecek yol: şirket içi verileri buluta kopyalama
Bu kılavuzda aşağıdaki adımları uygulayın: 

1. Veri fabrikası oluşturma.
2. Veri Yönetimi ağ geçidi oluşturun. 
3. Kaynak ve havuz veri deposu için bağlı hizmetler oluşturacaksınız.
4. Girdi ve çıktı verilerini temsil eden veri kümeleri oluşturun.
5. Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.

## <a name="prerequisites-for-the-tutorial"></a>Öğreticisi için Önkoşullar
Bu kılavuzda başlamadan önce aşağıdaki önkoşullara sahip olmalıdır:

* **Azure aboneliği**.  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Bkz: [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/) makale Ayrıntılar için.
* **Azure depolama hesabı**. Blob depolama alanı olarak kullandığınız bir **hedef/havuz** Bu öğreticide verileri depolayın. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın.
* **SQL Server**. Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız. 

## <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, Azure portalında adlı bir Azure Data Factory örneği oluşturmak için kullandığınız **ADFTutorialOnPremDF**.

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Tıklayın **kaynak Oluştur**, tıklayın **zeka + analiz**, tıklatıp **Data Factory**.

   ![Yeni->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. İçinde **yeni veri fabrikası** want **ADFTutorialOnPremDF** adı.

    ![Başlangıç panosuna Ekle](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **Veri Fabrikası adı "ADFTutorialOnPremDF" kullanılamıyor**(örneğin, yournameADFTutorialOnPremDF) veri fabrikasının adını değiştirin ve yeniden oluşturmayı deneyin. Bu öğreticinin geri kalan adımları gerçekleştirirken ADFTutorialOnPremDF yerine bu adı kullanın.
   >
   > Veri fabrikasının adı olarak kaydedilmiş olabilir bir **DNS** gelecekte ve herkese görünür hale gelmiş adı.
   >
   >
4. Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin.
5. Mevcut bir **kaynak grubu** seçin ya da bir kaynak grubu oluşturun. Öğreticide, adlı bir kaynak grubu oluşturun: **ADFTutorialResourceGroup**.
6. Tıklayın **Oluştur** üzerinde **yeni veri fabrikası** sayfası.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.
   >
   >
7. Oluşturma işlemi tamamlandıktan sonra gördüğünüz **Data Factory** sayfasında aşağıdaki görüntüde gösterildiği gibi:

   ![Data Factory giriş sayfası](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Ağ geçidi oluşturma
1. İçinde **Data Factory** sayfasında **yazar ve dağıtma** başlatmak için **Düzenleyicisi** data factory için.

    ![Geliştir ve Dağıt Kutucuğu](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. Data Factory Düzenleyicisi'nde tıklayın **... Daha fazla** araç ve ardından **yeni data gateway'e**. Alternatif olarak, sağ **veri ağ geçitleri** ağaç görünümü seçeneğine tıklayıp **yeni data gateway'e**.

   ![Araç çubuğundaki yeni veri ağ geçidi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. İçinde **Oluştur** want **adftutorialgateway** için **adı**, tıklatıp **Tamam**.     

    ![Ağ geçidi sayfası oluşturma](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > Bu kılavuzda, yalnızca bir düğümle (şirket içinde Windows makinesi) mantıksal ağ geçidi oluşturun. Birden çok şirket içi makine ile ağ geçidi ile ilişkilendirilmesi yoluyla veri yönetimi ağ geçidi ölçeklendirebilirsiniz. Ölçeklendirebileceğiniz yukarı bir düğümde eşzamanlı olarak çalışabilecek veri taşıma işlerinin sayısını artırarak. Bu özellik, tek bir düğüm ile mantıksal bir ağ geçidi için de kullanılabilir. Bkz: [ölçeklendirme veri yönetimi ağ geçidi Azure Data factory'de](data-factory-data-management-gateway-high-availability-scalability.md) makale Ayrıntılar için.  
4. İçinde **yapılandırma** sayfasında **doğrudan bu bilgisayara yüklemek**. Bu eylem ağ geçidi yükleme paketi indirir, yükler, yapılandırır ve bilgisayara ağ geçidi kaydeder.  

   > [!NOTE]
   > Internet Explorer veya Microsoft ClickOnce uyumlu web tarayıcısı kullanın.
   >
   > Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
   >
   > Aynı Firefox (eklenti yükleme) yapın. Tıklayın **menüyü Aç** araç çubuğunda (**üç yatay çizgiye** sağ üst köşedeki), tıklayın **eklentileri**, "ClickOnce" anahtar sözcüğüyle arama yapın, aşağıdakilerden birini seçin ClickOnce uzantılarından ve yükleyin.    
   >
   >

    ![Ağ geçidi - yapılandırma sayfası](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Bu şekilde, indirin, yükleyin, yapılandırın ve ağ geçidi tek bir adımda kaydetmek için en kolay yolu (tek tıklamayla) olur. Gördüğünüz **Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi** uygulaması bilgisayarınızda yüklü. Yürütülebilir dosya da bulabilirsiniz **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared**.

    Ayrıca indirin ve bu sayfa, bağlantıları kullanarak ağ geçidini el ile yükleyin ve gösterilen bir anahtar kullanarak kayıt **yeni anahtar** metin kutusu.

    Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale ağ geçidi hakkında tüm ayrıntılar için.

   > [!NOTE]
   > Yüklemek ve veri yönetimi ağ geçidi başarıyla yapılandırmak için yerel bilgisayarda yönetici olması gerekir. Ek kullanıcılar ekleyebilirsiniz **veri yönetimi ağ geçidi kullanıcıları** yerel Windows grubu. Bu grubun üyeleri, ağ geçidini yapılandırmak için veri yönetimi ağ geçidi Yapılandırma Yöneticisi aracını kullanabilirsiniz.
   >
   >
5. Birkaç dakika bekleyin ya da aşağıdaki bildirim iletisini görene kadar bekleyin:

    ![Başarılı ağ geçidi yükleme](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. Başlatma **veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni** bilgisayarınızda uygulama. İçinde **arama** penceresinde, tür **veri yönetimi ağ geçidi** bu yardımcı programına erişmek için. Yürütülebilir dosya da bulabilirsiniz **ConfigManager.exe** klasöründeki: **C:\Program Files\Microsoft veri yönetimi Gateway\2.0\Shared**

    ![Ağ geçidi Yapılandırma Yöneticisi](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. Gördüğünüzü onaylayın `adftutorialgateway is connected to the cloud service` ileti. Durum çubuğu altındaki görüntüler **bulut hizmetine bağlı** ile birlikte bir **yeşil onay işareti**.

    Üzerinde **giriş** sekmesinde, aşağıdaki işlemleri de yapabilirsiniz:

   * **Kayıt** Kaydet düğmesine kullanarak Azure portalından bir anahtara sahip bir ağ geçidi.
   * **Durdur** veri yönetimi ağ geçidi konağı, ağ geçidi makinesinde çalışan hizmeti.
   * **Güncelleştirmeleri zamanla** günün belirli bir zamanda yüklenecek.
   * Ağ geçidi kullanılırken görüntülemek **son güncelleştirme**.
   * Ağ geçidi için bir güncelleştirme yüklenebilir süreyi belirtin.
8. **Ayarlar** sekmesine geçin. Belirtilen sertifika **sertifika** bölümü, portalda belirttiğiniz şirket içi veri deposu için kimlik bilgilerini şifreleme/şifre çözme için kullanılır. (isteğe bağlı) Tıklayın **değişiklik** kendi sertifikanızı yerine kullanılacak. Varsayılan olarak, ağ geçidi Data Factory hizmeti tarafından otomatik olarak oluşturulan sertifika kullanır.

    ![Ağ geçidi sertifika yapılandırması](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Üzerinde aşağıdaki işlemleri yapabilirsiniz **ayarları** sekmesinde:

   * Görüntüleyebilir veya ağ geçidi tarafından kullanılan sertifikayı dışarı aktarın.
   * Ağ Geçidi tarafından kullanılan HTTPS uç noktası değiştirin.    
   * Ağ Geçidi tarafından kullanılacak bir HTTP proxy ayarlayın.     
9. (isteğe bağlı) Geçiş **tanılama** sekmesinde, onay **ayrıntılı günlüğe yazmayı etkinleştir** ağ geçidi ile ilgili tüm sorunları gidermek için kullanabileceğiniz ayrıntılı günlük kaydını etkinleştirmek istiyorsanız seçeneği. Günlük bilgilerini bulunabilir **Olay Görüntüleyicisi'ni** altında **uygulama ve hizmet günlükleri** -> **veri yönetimi ağ geçidi** düğümü.

    ![Tanılama sekmesi](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Ayrıca aşağıdaki eylemleri gerçekleştirebilirsiniz **tanılama** sekmesinde:

   * Kullanım **Test Bağlantısı** ağ geçidini kullanarak şirket içi veri kaynağına bölümü.
   * Tıklayın **günlüklerini görüntüle** veri yönetimi ağ geçidi günlüğüne bir Olay Görüntüleyici penceresinde görmek için.
   * Tıklayın **günlükleri Gönder** Microsoft sorunlarınızı sorun giderme sürecini kolaylaştırmak için son yedi gün günlükleri ile bir zip dosyası karşıya yüklemek için.
10. Üzerinde **tanılama** sekmesinde **Test Bağlantısı** bölümünden **SqlServer** veri deposu türü için veritabanının adı, veritabanı sunucusu adını girin kimlik doğrulama türünü belirtin, kullanıcı adı ve parola girin ve tıklayın **Test** ağ geçidi veritabanına bağlanıp bağlanamadığınızı test etmek için.
11. Web tarayıcısı ve buna geçiş **Azure portalında**, tıklayın **Tamam** üzerinde **yapılandırma** sayfası ve ardından **yeni veri ağ geçidi** sayfası.
12. Görmelisiniz **adftutorialgateway** altında **veri ağ geçitleri** soldaki ağaç görünümünde.  Tıklarsanız, ilişkili JSON görmeniz gerekir.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda iki bağlı hizmet oluşturursunuz: **AzureStorageLinkedService** ve **SqlServerLinkedService**. **SqlServerLinkedService** bir şirket içi SQL Server veritabanına bağlar ve **AzureStorageLinkedService** bağlı hizmet bir Azure blob depolama veri fabrikasına bağlar. Daha sonra verileri şirket içi SQL Server veritabanından Azure blob deposuna kopyalar. Bu kılavuzda bir işlem hattı oluşturursunuz.

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanına bağlı hizmet Ekle
1. İçinde **Data Factory düzenleyici**, tıklayın **yeni veri deposu** seçin ve araç **SQL Server**.

   ![Yeni SQL Server bağlı hizmeti](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. İçinde **JSON Düzenleyicisi** sağ tarafta, aşağıdaki adımları uygulayın:

   1. İçin **gatewayName**, belirtin **adftutorialgateway**.    
   2. İçinde **connectionString**, aşağıdaki adımları uygulayın:    

      1. İçin **servername**, SQL Server veritabanını barındıran sunucunun adını girin.
      2. İçin **databasename**, veritabanının adını girin.
      3. Tıklayın **şifrele** araç çubuğunda. Kimlik bilgileri Yöneticisi uygulaması görürsünüz.

         ![Kimlik bilgileri Yöneticisi uygulaması](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. İçinde **kimlik bilgilerini ayarlama** iletişim kutusunda, kimlik doğrulama türü, kullanıcı adı ve parola belirtin ve tıklayın **Tamam**. Bağlantı başarılı olursa, şifrelenmiş kimlik bilgileri JSON biçiminde depolanır ve iletişim kutusunu kapatır.
      5. Otomatik olarak kapalı değilse, iletişim kutusunu başlatan boş tarayıcı sekmesini kapatın ve Azure portalı ile sekmesindeki geri kazanın.

         Ağ geçidi makinesinde bu kimlik bilgileri olan **şifrelenmiş** Data Factory hizmetine sahip olan bir sertifikayı kullanarak. Bunun yerine veri yönetimi ağ geçidi ile ilişkili sertifika kullanmak istiyorsanız, küme kimlik bilgilerini güvenli bir şekilde bakın.    
   3. Tıklayın **Dağıt** SQL Server'ı dağıtmak için komut çubuğunda bağlı hizmeti. Bağlı hizmet ağaç görünümünde görmeniz gerekir.

      ![Ağaç görünümünde SQL Server bağlı hizmeti](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Bir Azure depolama hesabı için bağlı hizmet Ekle
1. İçinde **Data Factory düzenleyici**, tıklayın **yeni veri deposu** tıklayın ve komut çubuğunda **Azure depolama**.
2. Azure depolama hesabınızın adını **hesap adı**.
3. Azure depolama hesabınız için anahtarı girin **hesap anahtarı**.
4. Tıklayın **Dağıt** dağıtmak için **AzureStorageLinkedService**.

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

1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Daha fazla**, tıklayın **yeni veri kümesi** tıklayın ve komut çubuğunda **SQL Server tablosunu**.
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
   * **linkedServiceName** ayarlanır **SqlServerLinkedService** (Bu bağlı hizmeti bu kılavuzda daha önce oluşturmuştunuz.).
   * Azure Data factory'de başka bir işlem hattı tarafından oluşturulmayan bir giriş veri kümesi için ayarlamalısınız **dış** için **true**. Bu, girdi verilerini Azure Data Factory hizmetine dış üretilen gösterir. İsteğe bağlı olarak herhangi bir dış veri ilkeleri kullanılarak belirtebilirsiniz **externalData** öğesinde **ilke** bölümü.    

   Bkz: [/SQL Server'dan veri taşıma](data-factory-sqlserver-connector.md) JSON özellikleri hakkında ayrıntılar için.
3. Tıklayın **Dağıt** veri kümesini dağıtmak için komut çubuğunda.  

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma

1. İçinde **Data Factory düzenleyici**, tıklayın **yeni veri kümesi** tıklayın ve komut çubuğunda **Azure Blob Depolama**.
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
   * **folderPath** ayarlanır **adftutorial/outfromonpremdf** outfromonpremdf klasörü adftutorial kapsayıcısında olduğu. Oluşturma **adftutorial** zaten yoksa, kapsayıcı.
   * **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmetinin her saat bir çıktı veri dilimi oluşturur **emp** Azure SQL veritabanı tablosunda.

   Belirtmezseniz, bir **fileName** için bir **çıktı tablosu**'de oluşturulan dosyaları **folderPath** şu biçimde adlandırılır: `Data.<Guid>.txt` (örneğin:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   Ayarlanacak **folderPath** ve **fileName** göre dinamik olarak **SliceStart** saat, partitionedBy özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2014-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2014/10/20, fileName de 08.csv olarak ayarlanır.

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

    Bkz: [Azure Blob Depolama içine/dışına veri taşıma](data-factory-azure-blob-connector.md) JSON özellikleri hakkında ayrıntılar için.
3. Tıklayın **Dağıt** veri kümesini dağıtmak için komut çubuğunda. Her iki veri kümeleri ağaç görünümünde gördüğünüzü onaylayın.  

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda, oluşturduğunuz bir **işlem hattı** biriyle **kopyalama etkinliği** kullanan **EmpOnPremSQLTable** giriş olarak ve **OutputBlobTable** olarak çıktı.

1. Data Factory Düzenleyicisi'nde tıklayın **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın.
2. Sağ bölmedeki JSON aşağıdaki metinle değiştirin:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on premises SQL to Azure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on premises SQL server to blob",
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

   * Etkinlikler bölümünde, yalnızca bir etkinlik olduğundan, **türü** ayarlanır **kopyalama**.
   * **Giriş** etkinlik için **EmpOnPremSQLTable** ve **çıkış** etkinlik için **OutputBlobTable**.
   * İçinde **typeProperties** bölümünde **SqlSource** olarak belirtilen **kaynak türünü** ve **BlobSink** belirtilen**Havuz türü**.
   * SQL sorgusu `select * from emp` için belirtilen **sqlReaderQuery** özelliği **SqlSource**.

   Başlangıç ve bitiş tarih saatleri [ISO biçiminde](https://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2014-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız.

   **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9/9/9999** olarak ayarlayın.

   Göre veri dilimleri işlenir süre tanımladığınız **kullanılabilirlik** , her Azure Data Factory veri kümesi için tanımlanan özellikleri.

   Örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.        
3. Tıklayın **Dağıt** veri kümesini dağıtmak için komut çubuğunda (tablo dikdörtgen bir veri kümesi olur). İşlem hattını ağaç görünümünde altında gösterildiğini onaylayın **işlem hatları** düğümü.  
4. Şimdi, tıklayın **X** iki kez geri almak için bu sayfayı kapatmak için **Data Factory** sayfasındaki **ADFTutorialOnPremDF**.

**Tebrikler!** Başarılı bir şekilde bir Azure data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturduğunuza ve işlem hattını zamanladınız.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Data factory’yi Diyagram Görünümünde görüntüleme
1. İçinde **Azure portalında**, tıklayın **diyagram** kutucuğuna giriş sayfasında **ADFTutorialOnPremDF** veri fabrikası. :

    ![Bağlantı diyagramı](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Aşağıdaki görüntüye benzer bir diyagram görmeniz gerekir:

    ![Diyagram Görünümü](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Yakınlaştırmak, uzaklaştırabilir, % 100 yakınlaştırma uygun, otomatik olarak işlem hatlarını ve veri kümeleri getirin ve yakınlaştırabilir Göster (Seçilen öğelerin yukarı ve aşağı akış öğelerini vurgular) yakınlaştırma.  Bir nesnenin (giriş/çıkış veri kümesi veya işlem hattı) özelliklerini görmek için çift tıklayabilirsiniz.

## <a name="monitor-pipeline"></a>İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız. Veri kümelerini ve işlem hatlarını izlemek için de PowerShell cmdlet'lerini kullanabilirsiniz. İzleme hakkında daha fazla ayrıntı için bkz. [yönetme işlem hatlarını izleme ve](data-factory-monitor-manage-pipelines.md).

1. Diyagramda, çift **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Tüm veri dilimler yukarı içinde olduğuna dikkat edin **hazır** (başlangıç zamanı bitiş zamanından) işlem hattı süresi geçmiş olduğundan belirtin. Ayrıca SQL Server veritabanında veri eklenen ve her zaman yoktur çünkü bir hizmettir. Hiç dilim gösterilmediğini onaylayın **sorun dilimleri** alttaki bölümü. Tüm dilimleri görmek için tıklatın **daha fazla** dilimleri listesinin altındaki.
3. Şimdi, **veri kümeleri** sayfasında **OutputBlobTable**.

    ![OputputBlobTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Listeden herhangi bir veri dilimine tıklayın ve görmelisiniz **veri dilimi** sayfası. Dilimle ilgili etkinlik çalıştırmalarını görürsünüz. Yalnızca bir etkinlik genellikle çalıştırma görürsünüz.  

    ![Veri dilimi dikey penceresi](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz.
5. Tıklayın **etkinlik çalıştırması** görmek için alt kısımdaki listeden **etkinlik çalıştırması ayrıntıları**.

   ![Etkinlik çalışma Ayrıntıları sayfası](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Aktarım hızı, süresini ve veri aktarımı için kullanılan ağ geçidi gibi bilgileri görür.
6. Tıklayın **X** tüm sayfaları dek kapatmak için
7. Giriş sayfasına dönmek **ADFTutorialOnPremDF**.
8. (isteğe bağlı) Tıklayın **işlem hatları**, tıklayın **ADFTutorialOnPremDF**, girdi tablolarında detaya gidin (**tüketilen**) veya çıktı veri kümeleri (**üretilen**).
9. Gibi araçları kullanın [Microsoft Storage Gezgini](https://storageexplorer.com/) her saat için blob/dosyası oluşturulduğunu doğrulayın.

   ![Azure Depolama Gezgini](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale veri yönetimi ağ geçidi hakkında tüm ayrıntılar için.
* Bkz: [kopyalayın verileri Azure Blob'tan Azure SQL'e](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) bir havuz veri deposu için bir kaynak veri deposundan veri taşımak için kopyalama etkinliği'ni kullanma hakkında bilgi edinmek için.
