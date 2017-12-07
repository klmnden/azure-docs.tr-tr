---
title: "Azure Data Factory ile verileri SQL Server’dan Blob Depolama’ya kopyalama | Microsoft Docs"
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: ba47f3e3f331929b884f27f49bf6e484ff363765
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="tutorial-copy-data-from-on-premises-sql-server-to-azure-blob-storage"></a>Öğretici: Verileri şirket içi SQL Server'dan Azure Blob Depolama’ya kopyalama
Bu öğreticide, Azure PowerShell kullanarak verileri şirket içi SQL Server veritabanından bir Azure Blob depolama alanına kopyalayan Data Factory işlem hattı oluşturacaksınız. Verileri şirket içi ile bulut veri depoları arasında taşıyan, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup kullanabilirsiniz. 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.
> 
> Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md). 

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

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
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

### <a name="azure-roles"></a>Azure rolleri
Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır. Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalında sağ üst köşedeki **kullanıcı adınıza** tıklayın ve **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için [Rol ekleme](../billing/billing-add-change-azure-subscription-administrator.md) makalesine bakın.

### <a name="sql-server-201420162017"></a>SQL Server 2014/2016/2017
Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) bir Azure blob depolama alanına (havuz) kopyalar. SQL Server veritabanınızda **emp** adlı bir tablo oluşturun ve tabloya birkaç örnek girdi ekleyin. 

1. Makinenizde **SQL Server Management Studio**’yu başlatın. Makinenizde SQL Server Management Studio yoksa [indirme merkezinden](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) yükleyin. 
2. Kimlik bilgilerinizi kullanarak SQL sunucunuza bağlanın. 
3. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'na tıklayın. **Yeni Veritabanı** iletişim kutusunda, veritabanı için bir **ad** girin ve **Tamam**'a tıklayın. 
4. Veritabanında aşağıdaki sorgu betiğini çalıştırın; bu betik **emp** tablosunu oluşturur ve içine bazı örnek verileri ekler. Ağaç görünümünde, oluşturduğunuz **veritabanına** sağ tıklayın ve **Yeni Sorgu**'ya tıklayın. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )

    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    GO
    ```

### <a name="azure-storage-account"></a>Azure Storage hesabı
Bu öğreticide, genel amaçlı Azure Depolama Hesabı'nı (özel olarak Blob Depolama) **hedef/havuz** veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri şirket içi SQL Server veritabanından (kaynak) bu Azure blob depolama alanına (havuz) kopyalar. 

#### <a name="get-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdaki yordam, depolama hesabınızın adını ve anahtarını alma adımlarını sağlar. 

1. Web tarayıcısını açın ve [Azure Portal](https://portal.azure.com)'a gidin. Azure kullanıcı adınız ve parolanızla oturum açın. 
2. Sol taraftaki menüde **Diğer hizmetler >** öğesine tıklayın, **Depolama** anahtar sözcüğüyle filtreleyin ve **Depolama hesapları**'nı seçin.

    ![Depolama hesabını arama](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse) ve ardından **depolama hesabınızı** seçin. 
4. **Depolama hesabı** sayfasında, menüden **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adını ve anahtarını alma](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. **Depolama hesabı adı** ve **key1** alanlarının değerlerini panoya kopyalayın. Bunları not defterine veya başka bir düzenleyici yapıştırın ve kaydedin. Öğreticide depolama hesabı adını ve anahtarı kullanırsınız. 

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma 
Bu bölümde, Azure blob depolamanızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz. 

1. **Depolama hesabı** sayfasında **Genel Bakış**’a geçin ve sonra **Bloblar**’a tıklayın. 

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)
1. **Blob hizmeti** sayfasında, araç çubuğundaki **+ Kapsayıcı**’ya tıklayın. 

    ![Kapsayıcı ekle düğmesi](media/tutorial-hybrid-copy-powershell/add-container-button.png)
3. **Yeni kapsayıcı** iletişim kutusunda ad olarak **adftutorial** girin ve **Tamam**’a tıklayın. 

    ![Kapsayıcı adını girin](media/tutorial-hybrid-copy-powershell/new-container-dialog.png)
4. Kapsayıcılar listesinde **adftutorial** seçeneğine tıklayın.  

    ![Kapsayıcıyı seçin](media/tutorial-hybrid-copy-powershell/seelct-adftutorial-container.png)
5. **adftutorial** için **kapsayıcı** sayfasını açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

    ![Kapsayıcı sayfası](media/tutorial-hybrid-copy-powershell/container-page.png)

### <a name="windows-powershell"></a>Windows PowerShell

#### <a name="install-powershell"></a>PowerShell yükleme
En son PowerShell makinenizde önceden yüklü değilse, yükleyin. 

1. Web tarayıcısında [Azure SDK İndirmeleri ve SDK'lar](https://azure.microsoft.com/downloads/) sayfasına gidin. 
2. **Komut satırı araçları** -> **PowerShell** bölümünde **Windows yüklemesi**'ne tıklayın. 
3. Azure PowerShell'i yüklemek için **MSI** dosyasını çalıştırın. 

Ayrıntılı yönergeler için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>PowerShell’de oturum açın

1. Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar PowerShell penceresini açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.

    ![PowerShell’i başlatın](media/tutorial-hybrid-copy-powershell/search-powershell.png)
1. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız Azure kullanıcı adını ve parolasını girin:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Birden çok Azure aboneliğiniz varsa, bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Birden çok Azure aboneliğiniz varsa, birlikte çalışmak istediğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. Örneğin: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup"
    ```
2. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$resourceGroupName` değişkenine farklı bir değer atayın ve komutu yeniden çalıştırın.
3. Daha sonra PowerShell komutlarında kullanabileceğiniz veri fabrikası adı için bir değişken tanımlayın. Adlar bir harf veya rakamla başlamalıdır; yalnızca harf, rakam ve tire (-) karakterinden oluşabilir.

    > [!IMPORTANT]
    >  Veri fabrikasının adını genel olarak benzersiz olacak şekilde güncelleştirin. Örneğin, ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFTutorialFactory"
    ```
1. Veri fabrikasının konumu için bir değişken tanımlayın: 

    ```powershell
    $location = "East US"
    ```  
5. Veri fabrikası oluşturmak için aşağıdaki **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2TutorialDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, **katkıda bulunan** veya **sahip** rollerinin üyesi ya da bir Azure aboneliğinin **yöneticisi** olmalıdır.
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

## <a name="create-a-self-hosted-ir"></a>Şirket içinde barındırılan IR oluşturma

Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server verilerini Azure blob depolama alanına kopyalayan bileşendir. 

1. Tümleştirme çalışma zamanının adı için bir değişken oluşturun. Benzersiz bir ad kullanın ve adı not edin. Daha sonra bu öğreticide kullanacaksınız. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Oluşturulan tümleştirme çalışma zamanının durumunu almak için aşağıdaki komutu çalıştırın.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Şirket içinde barındırılan tümleştirme çalışma zamanını bulutta Data Factory hizmetine kaydetmek üzere **kimlik doğrulaması anahtarlarını** almak için aşağıdaki komutu çalıştırın. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Sonraki adımda makinenize yüklediğiniz şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek için anahtarların birini (çift tırnak işaretlerini hariç tutarak) kopyalayın.  

## <a name="install-integration-runtime"></a>Tümleştirme çalışma zamanını yükleme
1. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine [indirin](https://www.microsoft.com/download/details.aspx?id=39717) ve yüklemeyi çalıştırın. 
2. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazına Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.  
3. **Son Kullanıcı Lisans Anlaşması** sayfasında koşulları ve lisans anlaşmasını kabul edin ve **İleri**'ye tıklayın. 
4. **Hedef Klasör** sayfasında **İleri**'ye tıklayın. 
5. **Microsoft Tümleştirme Çalışma Zamanı Yüklemeye Hazır** bölümünde **Yükle**'ye tıklayın. 
6. Bilgisayarın kullanılmadığında uyku veya hazırda bekletme moduna geçecek şekilde yapılandırıldığını bildiren bir uyarı iletisi görürseniz, **Tamam**'a tıklayın. 
7. **Güç Seçenekleri** penceresini görürseniz kapatın ve kurulum penceresine geçin. 
8. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazı Tamamlandı** sayfasında **Son**'a tıklayın.
9. **Tümleştirme Çalışma Zamanını (Şirket içinde barındırılan) Kaydet** sayfasında, önceki bölümde kaydettiğiniz anahtarı yapıştırın ve **Kaydol**'a tıklayın. 

   ![Tümleştirme çalışma zamanını kaydetme](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)
2. Şirket içinde barındırılan tümleştirme çalışma zamanı başarıyla kaydedildiğinde aşağıdaki iletiyi görürsünüz:

   ![Başarıyla kaydedildi](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

3. **Yeni Tümleştirme Çalışma Zamanı (Şirket içinde barındırılan) Düğümü** sayfasında **İleri**'ye tıklayın. 

    ![Yeni Tümleştirme Çalışma Zamanı Düğümü sayfası](media/tutorial-hybrid-copy-powershell/new-integration-runtime-node-page.png)
4. **İntranet İletişim Kanalı**'nda **Atla**'ya tıklayın. Çok düğümlü bir tümleştirme çalışma zamanı ortamında düğüm içi iletişimin güvenliğini sağlamak için TLS/SSL sertifikası seçebilirsiniz. 

    ![İntranet iletişim kanalı sayfası](media/tutorial-hybrid-copy-powershell/intranet-communication-channel-page.png)
5. **Tümleştirme Çalışma Zamanını (Şirket içinde barındırılan) Kaydet** sayfasında **Configuration Manager'ı Başlat**'a tıklayın. 
6. Düğüm bulut hizmetine bağlandığında şu sayfayı görürsünüz:

   ![Düğüm bağlı](media/tutorial-hybrid-copy-powershell/node-is-connected.png)
7. Şimdi SQL Server veritabanınıza yönelik bağlantıyı test edin.

    ![Tanılama sekmesi](media/tutorial-hybrid-copy-powershell/config-manager-diagnostics-tab.png)   

    - **Configuration Manager** penceresinde **Tanılama** sekmesine geçin.
    - **Veri kaynağı türü** olarak **SqlServer**’ı seçin.
    - **Sunucu** adını girin.
    - **Veritabanı** adını girin. 
    - **Kimlik doğrulaması** modunu seçin. 
    - **Kullanıcı** adını girin. 
    - Kullanıcı adı için **parola** girin.
    - Tümleştirme çalışma zamanının SQL Server’a bağlanabildiğini onaylamak için **Test**’e tıklayın. Bağlantı başarılı olursa yeşil bir onay işareti görürsünüz. Başarılı olmazsa, hata ile ilişkili hata iletisini görürsünüz. Sorunları giderin ve tümleştirme çalışma zamanının SQL Server’a bağlanabildiğinden emin olun.
    - Bu değerleri not edin (kimlik doğrulama türü, sunucu, veritabanı, kullanıcı, parola). Daha sonra bu öğreticide kullanacaksınız. 
    
      
## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturun. Bu öğreticide, Azure Depolama Hesabınız ile şirket içi SQL Server’ınızı veri deposuna bağlarsınız. Bağlı hizmetler, Data Factory hizmetinin bunlara bağlanmak için çalışma zamanında kullandığı bağlantı bilgilerini içerir. 

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Azure Depolama bağlı hizmeti oluşturma (hedef/havuz)
Bu adımda, Azure Depolama Hesabınızı veri fabrikasına bağlarsınız.

1. **C:\ADFv2Tutorial** klasöründe şu içeriğe sahip **AzureStorageLinkedService.json** adlı bir JSON dosyası oluşturun: Henüz yoksa ADFv2Tutorial adlı bir klasör oluşturun.  

    > [!IMPORTANT]
    > Dosyayı kaydetmeden önce &lt;accountName&gt; ve &lt;accountKey&gt; değerlerini **Azure depolama hesabınızın** adı ve anahtarıyla değiştirin. [Önkoşullar](#get-storage-account-name-and-account-key) dahilinde bunları not alın.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>;EndpointSuffix=core.windows.net"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

    Not Defteri’ni kullanıyorsanız, **Farklı kaydet** iletişim kutusunda **Farklı kaydetme türü** için **Tüm dosyalar**ı seçin. Aksi takdirde, dosyaya `.txt` uzantısını ekleyebilir. Örneğin, `AzureStorageLinkedService.json.txt`. Dosyayı Not Defteri’nde açmadan önce Dosya Gezgini’nde oluşturduysanız, **Bilinen dosya türleri için uzantıları gizle** seçeneği varsayılan olarak ayarlandığı için `.txt` uzantısını görmeyebilirsiniz. Sonraki adıma geçmeden önce `.txt` uzantısını kaldırın. 
2. **Azure PowerShell**’de **C:\ADFv2Tutorial** klasörüne geçin.

   **AzureStorageLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. 

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Örnek çıktı aşağıdaki gibidir:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

    "Dosya bulunamadı" hatasını alırsanız. Dosyanın var olduğunu onaylamak için `dir` komutunu kullanın. Dosya `.txt` uzantısına sahipse (örneğin, AzureStorageLinkedService.json.txt) dosyayı kaldırın ve PowerShell komutunu tekrar çalıştırın. 

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>SQL Server bağlı hizmeti oluşturma ve şifreleme (kaynak)
Bu adımda şirket içi SQL Server’ınızı veri fabrikasına bağlarsınız.

1. **C:\ADFv2Tutorial** klasöründe şu içeriğe sahip **SqlServerLinkedService.json** adlı bir JSON dosyası oluşturun: SQL Server’a bağlanmak için kullandığınız **kimlik doğrulaması** yöntemine göre doğru bölümü seçin.  

    > [!IMPORTANT]
    > SQL Server’a bağlanmak için kullandığınız **kimlik doğrulaması** yöntemine göre doğru bölümü seçin.

    **SQL kimlik doğrulaması (sa) kullanıyorsanız aşağıdaki JSON tanımını kopyalayın:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```    
    **Windows kimlik doğrulaması kullanıyorsanız aşağıdaki JSON tanımını kopyalayın:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<server>;Database=<database>;Integrated Security=True"
                },
                "userName": "<user> or <domain>\\<user>",
                "password": {
                    "type": "SecureString",
                    "value": "<password>"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }    
    ```
    > [!IMPORTANT]
    > - SQL Server’a bağlanmak için kullandığınız **kimlik doğrulaması** yöntemine göre doğru bölümü seçin.
    > - **&lt;integration** **runtime** **name>** değerlerini tümleştirme çalışma zamanınızın adıyla değiştirin.
    > - Dosyayı kaydetmeden önce **&lt;sunucuadı>**, **&lt;veritabanıadı>**, **&lt;kullanıcıadı>** ve **&lt;parola>** değerlerini SQL Server değerleriyle değiştirin.
    > - Kullanıcı hesabınızda veya sunucu adında eğik çizgi karakteri (`\`) kullanmanız gerekirse kaçış karakterini (`\`) kullanın. Örneğin, `mydomain\\myuser`. 

2. Hassas verileri (kullanıcı adı, parola vb.) şifrelemek için **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet’ini çalıştırın. Bu şifreleme, kimlik bilgilerinin Veri Koruma Uygulama Programlama Arabirimi (DPAPI) kullanılarak şifrelenmesini sağlar. Şifrelenmiş kimlik bilgileri, şirket içinde barındırılan tümleştirme çalışma zamanı düğümünde (yerel makine) yerel olarak kaydedilir. Çıktı yükü, şifrelenmiş kimlik bilgilerini içeren başka bir JSON dosyasına (bu örnekte 'encryptedLinkedService.json') yönlendirilebilir.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. **EncryptedSqlServerLinkedService** öğesini oluşturan aşağıdaki komutu çalıştırın:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda, kopyalama işlemi için girdi ve çıktı verilerini temsil eden girdi ve çıktı veri kümeleri oluşturursunuz (Şirket içi SQL Server veritabanı = > Azure blob depolama).

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma
Bu adımda, SQL Server veritabanındaki verileri temsil eden bir veri kümesini tanımlarsınız. Veri kümesi **SqlServerTable** türündedir. Önceki adımda oluşturduğunuz **SQL Server bağlı hizmetini** ifade eder. Bağlı hizmet, Data Factory hizmetinin çalışma zamanında SQL Server örneğinize bağlanmak için kullandığı **bağlantı bilgilerini** içerir. Bu veri kümesi, verileri içeren veritabanındaki **SQL tablosunu** belirtir. Bu öğreticide, bu tablo kaynak verileri içeren `emp` tablosudur. 

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerDataset.json** adlı bir JSON dosyası oluşturun:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. **SqlServerDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Havuz Azure Blob Depolama için veri kümesi oluşturma
Bu adımda, Azure Blob Depolama birimine kopyalanacak verileri temsil eden bir veri kümesi tanımlarsınız. Veri kümesi **AzureBlob** türündedir. Bu öğreticide daha önce oluşturduğunuz **Azure Depolama bağlı hizmetine** başvurur. Bağlı hizmet, Data Factory hizmetinin Azure Depolama Hesabınıza bağlanmak için kullandığı **bağlantı bilgilerini** içerir. Bu **veri kümesi**, Azure depolama alanında SQL Server veritabanından verilerin kopyalandığı **klasörü** belirtir. Bu öğreticide bu, `adftutorial/fromonprem` adlı klasördür; burada `adftutorial` blob kapsayıcısı ve `fromonprem` klasördür. 

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **AzureBlobDataset.json** adlı bir JSON dosyası oluşturun:

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. **AzureBlobDataset** veri kümesini oluşturmak için **Set-AzureRmDataFactoryV2Dataset** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu öğreticide, kopyalama etkinliği ile bir işlem hattı oluşturursunuz. Kopyalama etkinliği, **SqlServerDataset**’i giriş veri kümesi olarak, **AzureBlobDataset**’i çıkış veri kümesi olarak kullanır. Kaynak türü **SqlSource**, havuz türü ise **BlobSink** olarak ayarlanır.

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerToBlobPipeline.json** adlı bir JSON dosyası oluşturun:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. **SQLServerToBlobPipeline** işlem hattını oluşturmak için **Set-AzureRmDataFactoryV2Pipeline** cmdlet’ini çalıştırın.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Örnek çıktı aşağıdaki gibidir:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```


## <a name="create-a-pipeline-run"></a>İşlem hattı çalıştırması oluşturma
"SQLServerToBlobPipeline" işlem hattı için bir işlem hattı çalıştırması başlatın ve gelecekte izlemek üzere işlem hattı çalıştırma kimliğini yakalayın.

```powershell
$runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
```

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **SQLServerToBlobPipeline** işlem hattının çalıştırılma durumunu sürekli olarak denetlemek için aşağıdaki betiği çalıştırın ve nihai sonucu yazdırın. Aşağıdaki betiği kopyalayıp PowerShell penceresine yapıştırın ve ENTER tuşuna basın.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Örnek çalıştırmanın çıktısı aşağıdaki gibidir:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. **SQLServerToBlobPipeline** işlem hattının çalıştırma kimliğini alabilir ve aşağıdaki komutu çalıştırarak ayrıntılı etkinlik çalıştırma sonucunu denetleyebilirsiniz: 

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Örnek çalıştırmanın çıktısı aşağıdaki gibidir:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 3,
      "throughput": 0.01171875,
      "errors": [],
      "effectiveIntegrationRuntime": "MyIntegrationRuntime",
      "billedDuration": 3
    }
    ```

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı `adftutorial` blob kapsayıcısında `fromonprem` adlı klasörü otomatik olarak oluşturur. Çıktı klasöründe **dbo.emp.txt** dosyasını gördüğünüzü onaylayın. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı sayfasında **Yenile**’ye tıklayın.

    ![oluşturulan çıktı klasörü](media/tutorial-hybrid-copy-powershell/fromonprem-folder.png)
2. Klasör listesinde `fromonprem` seçeneğine tıklayın. 
3. `dbo.emp.txt` adlı bir dosya gördüğünüzü onaylayın.

    ![çıktı dosyası](media/tutorial-hybrid-copy-powershell/fromonprem-file.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Azure blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Azure Depolama bağlı hizmetlerini oluşturma. 
> * SQL Server ve Azure Blob veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

Azure Data Factory tarafından desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Kaynaktan hedefe verileri toplu olarak kopyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy.md)
