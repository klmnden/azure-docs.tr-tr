---
title: "Azure Data Factory kullanarak şirket içi verileri buluta kopyalama | Microsoft Docs"
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
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 24a4255a23f0b9b9da5d8c3cefeefb8fe250f2f1
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="tutorial-copy-data-between-on-premises-and-cloud"></a>Öğretici: Şirket içi ile bulut arasında veri kopyalama
Bu öğreticide, Azure PowerShell kullanarak verileri şirket içi SQL Server veritabanından bir Azure Blob depolama alanına kopyalayan Data Factory işlem hattı oluşturacaksınız. Azure Data Factory şirket içinde barındırılan tümleştirme çalışma zamanı oluşturup kullanacaksınız. Bu, şirket içi veri depoları ile bulut veri depolarının tümleştirilmesine olanak tanır.  Veri fabrikası oluşturmaya yönelik diğer araçlar/SDK’lar hakkında bilgi edinmek için bkz. [Hızlı Başlangıçlar](quickstart-create-data-factory-dot-net.md).

Bu makale, Data Factory hizmetine ayrıntılı giriş bilgileri sağlamaz. Azure Data Factory hizmetine giriş bilgileri için bkz. [Azure Data Factory'ye giriş](introduction.md). 

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Azure Depolama bağlı hizmetlerini oluşturma. 
> * SQL Server ve Azure Blob veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

### <a name="sql-server-2014-or-2016"></a>SQL Server 2014 veya 2016. 
Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız. SQL Server veritabanınızda **emp** adlı bir tablo oluşturun ve tabloya birkaç örnek girdi ekleyin.

1. **SQL Server Management Studio**'yu başlatın. SQL Server 2016 kullanıyorsanız, SQL Server Management Studio'yu [indirme merkezinden](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) ayrıca yüklemeniz gerekebilir. 
2. Kimlik bilgilerinizi kullanarak SQL sunucunuza bağlanın. 
3. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'na tıklayın. **Yeni Veritabanı** iletişim kutusunda, veritabanı için bir **ad** girin ve **Tamam**'a tıklayın. 
4. Veritabanında aşağıdaki sorgu betiğini çalıştırın; bu betik **emp** tablosunu oluşturur. Ağaç görünümünde, oluşturduğunuz **veritabanına** sağ tıklayın ve **Yeni Sorgu**'ya tıklayın. 

    ```sql   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Veritabanında, tabloya birkaç örnek veri ekleyen aşağıdaki komutları çalıştırın:

    ```sql
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="azure-storage-account"></a>Azure Storage hesabı
Bu öğreticide, genel amaçlı Azure Depolama Hesabı'nı (özel olarak Blob Depolama) **hedef/havuz** veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

#### <a name="get-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu hızlı başlangıçta, Azure depolama hesabınızın adını ve anahtarını kullanırsınız. Aşağıdaki yordam, depolama hesabınızın adını ve anahtarını alma adımlarını sağlar. 

1. Web tarayıcısını açın ve [Azure Portal](https://portal.azure.com)'a gidin. Azure kullanıcı adınız ve parolanızla oturum açın. 
2. Sol taraftaki menüde **Diğer hizmetler >** öğesine tıklayın, **Depolama** anahtar sözcüğüyle filtreleyin ve **Depolama hesapları**'nı seçin.

    ![Depolama hesabını arama](media/tutorial-hybrid-copy-powershell/search-storage-account.png)
3. Depolama hesapları listesinde, depolama hesabınız için filtre uygulayın (gerekirse) ve ardından **depolama hesabınızı** seçin. 
4. **Depolama hesabı** sayfasında, menüden **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adını ve anahtarını alma](media/tutorial-hybrid-copy-powershell/storage-account-name-key.png)
5. **Depolama hesabı adı** ve **key1** alanlarının değerlerini panoya kopyalayın. Bunları not defterine veya başka bir düzenleyici yapıştırın ve kaydedin. Öğreticide depolama hesabı adını ve anahtarı kullanırsınız. 

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma 
Bu bölümde, Azure blob depolamanızda adftutorial adlı bir blob kapsayıcısı oluşturursunuz. 

1. Makinenizde [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) yoksa yükleyin. 
2. Makinenizde **Microsoft Azure Depolama Gezgini**'ni başlatın.   
3. **Azure Depolamaya Bağlan** penceresinde, **Depolama hesap adı ve anahtarı kullan**'ı seçin ve **İleri**'ye tıklayın. **Azure Depolamaya Bağlan** penceresini görmüyorsanız, ağaç görünümünde **Depolama Hesapları**'na sağ tıklayın ve **Azure depolamaya bağlan**'a tıklayın. 

    ![Azure depolamaya bağlanma](media/tutorial-hybrid-copy-powershell/storage-explorer-connect-azure-storage.png)
4. **Ad ve Anahtar kullanarak ekle** penceresinde, önce adımda kaydettiğiniz **Hesap adı** ve **Hesap anahtarı** değerlerini yapıştırın. Ardından **İleri**'ye tıklayın. 
5. **Bağlantı Özeti** penceresinde **Bağlan**'a tıklayın.
6. Ağaç görünümünde **(Yerel ve Bağlı)** -> **Depolama Hesapları**'nın altında depolama hesabınızı gördüğünüzü doğrulayın. 
7. **Blob Kapsayıcıları**'nı genişletin ve **adftutorial** adlı bir blob kapsayıcısının olmadığını doğrulayın. Zaten varsa, kapsayıcıyı oluşturma adımlarını atlayın. 
8. **Blob Kapsayıcıları**'na sağ tıklayın ve **Blob Kapsayıcısı Oluştur**'u seçin.

    ![Blob kapsayıcısı oluşturma](media/tutorial-hybrid-copy-powershell/stroage-explorer-create-blob-container-menu.png)
9. Ad olarak **adftutorial** girin ve **ENTER** tuşuna basın. 
10. Ağaç görünümünde **adftutorial** kapsayıcısının seçildiğini doğrulayın. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
En son Azure PowerShell makinenizde önceden yüklü değilse, yükleyin. 

1. Web tarayıcısında [Azure SDK İndirmeleri ve SDK'lar](https://azure.microsoft.com/downloads/) sayfasına gidin. 
2. **Komut satırı araçları** -> **PowerShell** bölümünde **Windows yüklemesi**'ne tıklayın. 
3. Azure PowerShell'i yüklemek için **MSI** dosyasını çalıştırın. 

Ayrıntılı yönergeler için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma
Makinenizde **PowerShell**'i başlatın. Bu hızlı başlangıcın sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız komutları yeniden çalıştırmanız gerekir.

1. Aşağıdaki komutu çalıştırın ve Azure Portal'da oturum açmak için kullandığınız Azure kullanıcı adını ve parolasını girin:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Birden çok Azure aboneliğiniz varsa, bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **SubscriptionId**’yi Azure aboneliğinizin kimliği ile değiştirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Daha sonra PowerShell komutlarında kullanacağınız kaynak grubu adı için bir değişken tanımlayın. Aşağıdaki komut metnini PowerShell'e kopyalayın [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) için çift tırnak içinde bir ad belirtin ve ardından komutu çalıştırın. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Daha sonra PowerShell komutlarında kullanabileceğiniz veri fabrikası adı için bir değişken tanımlayın. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Veri fabrikasının konumu için bir değişken tanımlayın: 

    ```powershell
    $location = "East US"
    ```
4. Azure kaynak grubunu oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 

    Kaynak grubu zaten varsa, üzerine yazılmasını istemeyebilirsiniz. `$resourceGroupName` değişkenine farklı bir değer atayın ve yeniden deneyin. Kaynak grubunu diğeriyle paylaşmak isterseniz, sonraki adımdan devam edin.  
5. Veri fabrikası oluşturmak için aşağıdaki **Set-AzureRmDataFactoryV2** cmdlet’ini çalıştırın: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Aşağıdaki noktalara dikkat edin:

* Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız adı değiştirip yeniden deneyin.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Data Factory örnekleri oluşturmak için Azure aboneliğinde **katkıda bulunan** veya **yönetici** rolünüz olmalıdır.
* Data Factory sürüm 2 şu anda Doğu ABD, Doğu ABD2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.

## <a name="create-a-self-hosted-ir"></a>Şirket içinde barındırılan IR oluşturma

Bu bölümde Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturabilir ve bir şirket içi düğümle (makine) ilişkilendirebilirsiniz.

1. Tümleştirme çalışma zamanının adı için bir değişken oluşturun. 

    ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
    ```
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma. Aynı ada sahip başka bir tümleştirme çalışma zamanı varsa benzersiz bir ad kullanın.

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

3. Şirket içinde barındırılan tümleştirme çalışma zamanını bulutta Data Factory hizmetine kaydetmek üzere kimlik doğrulama anahtarlarını almak için aşağıdaki komutu çalıştırın. Şirket içinde barındırılan tümleştirme çalışma zamanını kaydetmek için anahtarlardan birini kopyalayın.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Örnek çıktı aşağıdaki gibidir:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

## <a name="install-integration-runtime"></a>Tümleştirme çalışma zamanını yükleme
1. Şirket içinde barındırılan tümleştirme çalışma zamanını yerel Windows makinesine [indirin](https://www.microsoft.com/download/details.aspx?id=39717) ve yüklemeyi çalıştırın. 
2. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazına Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.  
3. **Son Kullanıcı Lisans Anlaşması** sayfasında koşulları ve lisans anlaşmasını kabul edin ve **İleri**'ye tıklayın. 
4. **Hedef Klasör** sayfasında **İleri**'ye tıklayın. 
5. **Microsoft Tümleştirme Çalışma Zamanı Yüklemeye Hazır** bölümünde **Yükle**'ye tıklayın. 
6. Bilgisayarın kullanılmadığında uyku veya hazırda bekletme moduna geçecek şekilde yapılandırıldığını bildiren bir uyarı iletisi görürseniz, **Tamam**'a tıklayın. 
7. **Microsoft Tümleştirme Çalışma Zamanı Kurulum Sihirbazı Tamamlandı** sayfasında **Son**'a tıklayın.
8. **Tümleştirme Çalışma Zamanını (Şirket içinde barındırılan) Kaydet** sayfasında, önceki bölümde kaydettiğiniz anahtarı yapıştırın ve **Kaydol**'a tıklayın. 

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

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Azure Depolama bağlı hizmeti oluşturma (hedef/havuz)

1. **C:\ADFv2Tutorial** klasöründe şu içeriğe sahip **AzureStorageLinkedService.json** adlı bir JSON dosyası oluşturun: Henüz yoksa ADFv2Tutorial adlı bir klasör oluşturun.  

    > [!IMPORTANT]
    > Dosyayı kaydetmeden önce &lt;accountName&gt; ve &lt;accountKey&gt; değerlerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```
2. **Azure PowerShell**’de **ADFv2Tutorial** klasörüne geçin.

   **AzureStorageLinkedService** bağlı hizmetini oluşturmak için **Set-AzureRmDataFactoryV2LinkedService** cmdlet’ini çalıştırın. Bu öğreticide kullanılan cmdlet’ler, **ResourceGroupName** ve **DataFactoryName** parametrelerinin değerlerini alır. Alternatif olarak, bir cmdlet’i her çalıştırdığınızda ResourceGroupName ve DataFactoryName yazmadan Set-AzureRmDataFactoryV2 cmdlet’i tarafından döndürülen **DataFactory** nesnesini geçirebilirsiniz.

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

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>SQL Server bağlı hizmeti oluşturma ve şifreleme (kaynak)

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **SqlServerLinkedService.json** adlı bir JSON dosyası oluşturun: Dosyayı kaydetmeden önce **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>** ve **&lt;password>** değerlerini SQL Server değerleriyle değiştirin. 

    > [!IMPORTANT]
    > **&lt;integration** **runtime** **name>** değerlerini tümleştirme çalışma zamanınızın adıyla değiştirin.

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
2. Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde JSON yükünden alınan gizli verileri şifrelemek için, **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** komutunu çalıştırıp yukarıdaki JSON yükünü geçirin. Bu şifreleme, kimlik bilgilerinin Veri Koruma Uygulama Programlama Arabirimi (DPAPI) kullanılarak şifrelenmesini sağlar. Şifrelenmiş kimlik bilgileri şirket içinde barındırılan tümleştirme çalışma zamanı düğümüne yerel olarak (yerel makineye) kaydedilir. Çıktı yükü, şifrelenmiş kimlik bilgilerini içeren başka bir JSON dosyasına (bu örnekte 'encryptedLinkedService.json') yönlendirilebilir.
    
   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName $integrationRuntimeName -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. **SqlServerLinkedService** öğesini oluşturmak için önceki adımda yer alan JSON dosyasını kullanarak aşağıdaki komutu çalıştırın:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda, kopyalama işlemi için girdi ve çıktı verilerini temsil eden girdi ve çıktı veri kümeleri oluşturursunuz (Şirket içi SQL Server veritabanı = > Azure blob depolama).

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

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

1. **C:\ADFv2Tutorial** klasöründe aşağıdaki içeriğe sahip **AzureBlobDataset.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > Bu örnek kod Azure Blob Depolamada **adftutorial** adlı bir blob kapsayıcıya sahip olduğunuzu varsayar.

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

## <a name="create-pipelines"></a>İşlem hattı oluşturma

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>"SqlServerToBlobPipeline" işlem hattını oluşturma

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



## <a name="start-and-monitor-a-pipeline-run"></a>İşlem hattı çalıştırmasını başlatma ve izleme

1. "SQLServerToBlobPipeline" işlem hattı için bir işlem hattı çalıştırması başlatın ve gelecekte izlemek üzere işlem hattı çalıştırma kimliğini yakalayın.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. "**SQLServerToBlobPipeline**" işlem hattının çalıştırma durumunu sürekli olarak denetlemek için aşağıdaki betiği çalıştırın ve kesin sonucun çıktısını alın.

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

3. "**SQLServerToBlobPipeline**" işlem hattının çalıştırma kimliğini alabilir ve ayrıntılı etkinlik çalıştırma sonucunu aşağıdaki gibi denetleyebilirsiniz.

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
İşlem hattı `adftutorial` blob kapsayıcısında `fromonprem` adlı klasörü otomatik olarak oluşturur. Çıktı klasöründe **dbo.emp.txt** dosyasını gördüğünüzü onaylayın. [Azure Depolama gezginini](https://azure.microsoft.com/features/storage-explorer/) kullanarak çıktının oluşturulduğunu doğrulayın. 

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
