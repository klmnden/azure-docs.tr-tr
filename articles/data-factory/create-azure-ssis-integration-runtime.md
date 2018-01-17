---
title: "Kendini barındıran tümleştirmesi çalışma zamanı Azure Data Factory oluşturma | Microsoft Docs"
description: "SQL Server saklı yordam etkinliği bir saklı yordam bir Azure SQL Database veya Azure SQL veri ambarı Data Factory işlem hattı çağırmak için nasıl kullanabileceğinizi öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: 7796df75d811ad34967aee66478eae992fd449fe
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma
Bu makalede Azure Data Factory bir Azure SSIS tümleştirmesi çalışma zamanı sağlamak için adımları sağlar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak Azure’da bu çalışma zamanına SQL Server Integration Services (SSIS) paketleri dağıtabilirsiniz.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md) konusunu inceleyin.

Öğretici: [Öğreticisi: SQL Server Integration Services (SSIS) Azure'a dağıtabilmeniz](tutorial-deploy-ssis-packages-azure.md) Azure SQL veritabanını depo olarak SSIS kataloğunu kullanarak Azure SSIS tümleştirmesi çalışma zamanı (IR) oluşturulacağını gösterir. Bu makalede öğreticiyi genişletir ve aşağıdakilerin nasıl yapılacağını gösterir: 

- Azure SQL yönetilen örneği (özel olarak incelenmektedir) SSIS katalog (SSISDB veritabanı) barındırmak için kullanın.
- Bir Azure sanal ağı (VNet) için Azure SSIS IR katılın. 

Bir sanal ağa bir Azure SSIS IR birleştirme ve Azure portalında VNet yapılandırma kavramsal bilgi için bkz: [katılma Azure SSIS IR vnet'e](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**. Bir aboneliğiniz yoksa, bir [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz.
- **Azure SQL Veritabanı sunucusu** veya **SQL Server Yönetilen Örneği (özel önizleme) (Genişletilmiş Özel Önizleme)**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Bu sunucu, SSIS Katalog veritabanını (SSISDB) barındırır. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. Azure SQL sunucusunun fiyatlandırma katmanı unutmayın. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarına listesi için bkz: [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md).
- **Klasik Sanal Ağ (VNet) (isteğe bağlı)**. Aşağıdaki koşulların en az biri geçerli ise bir Azure Sanal Ağınız (VNet) olmalıdır:
    - SSIS Katalog veritabanını bir sanal ağın parçası olan SQL Server Yönetilen Örneği (özel önizleme) üzerinde barındırıyorsanız.
    - Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.
- **Azure PowerShell**. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) bölümündeki yönergeleri izleyin. Bulutta SSIS paketleri çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı sağlamak üzere betik çalıştırmak için PowerShell kullanıyorsanız. 

> [!NOTE]
> Azure Data Factory V2 ve Azure-SSIS Integration Runtime tarafından desteklenen tüm bölgelerin listesi için bkz. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/). **Veri ve Analiz**’i genişleterek **Data Factory V2** ve **SSIS Integration Runtime** seçeneklerini görün.

## <a name="use-azure-portal"></a>Azure portalını kullanma

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. İçinde **yeni data factory** want **MyAzureSsisDataFactory** için **adı**. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hata iletisini alırsanız data factory (örneğin, yournameMyAzureSsisDataFactory) adını değiştirin ve oluşturmayı yeniden deneyin. Bkz: [Data Factory - adlandırma kurallarını](naming-rules.md) Data Factory yapıtlarının adlandırma kuralları için makale.
  
       `Data factory name “MyAzureSsisDataFactory” is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Data Factory oluşturulması için desteklenen konumlar listesinde gösterilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra gördüğünüz **Data Factory** görüntüde gösterildiği gibi sayfa.
   
   ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Tıklatın **Yazar & İzleyici** veri fabrikası kullanıcı arabirimi (UI) ayrı bir sekmede başlatmak için. 

### <a name="provision-an-azure-ssis-integration-runtime"></a>Bir Azure SSIS tümleştirmesi çalışma zamanı sağlama

1. Get başlangıç sayfasını tıklatın **SSIS tümleştirmesi çalışma zamanı yapılandırma** döşeme. 

   ![SSIS tümleştirmesi çalışma zamanı döşeme yapılandırın](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)
2. İçinde **genel ayarları** sayfasında **tümleştirmesi çalışma zamanı Kurulum**, aşağıdaki adımları uygulayın: 

   ![Genel ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    1. Belirtin bir **adı** tümleştirmesi çalışma zamanı için.
    2. Seçin **konumu** tümleştirmesi çalışma zamanı için. Yalnızca desteklenen konumlar görüntülenir.
    3. Seçin **düğüm boyutu** olan SSIS çalışma zamanı ile yapılandırılmalıdır.
    4. Belirtin **düğümleri sayısı** kümedeki.
    5. **İleri**’ye tıklayın. 
1. İçinde **SQL ayarlarını**, aşağıdaki adımları uygulayın: 

    ![SQL ayarları](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    1. Azure belirtin **abonelik** Azure SQL sunucusuna sahip. 
    2. Azure SQL sunucunuzu seçin **Katalog veritabanı sunucusu uç noktası**.
    3. Girin **yönetici** kullanıcı adı.
    4. Girin **parola** yönetici için.  
    5. Seçin **hizmet katmanı** SSISDB veritabanı için. Basic varsayılan değerdir.
    6. **İleri**’ye tıklayın. 
1.  İçinde **Gelişmiş ayarları** sayfasında, bir değer seçin **en fazla paralel yürütmeleri düğüm başına**.   

    ![Gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)    
5. Bu adım **isteğe bağlı**. Katılmak için seçin tümleştirmesi çalışma zamanı istediğiniz klasik bir sanal ağ (VNet) olup olmadığını **katılmak ve VNet izinleri/ayarlarını yapılandırmak Azure services izin vermek için bir VNet Azure SSIS tümleştirmesi çalışma zamanı modülü seçin** seçenek ve aşağıdaki adımları uygulayın: 

    ![VNet ile gelişmiş ayarları](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)    

    1. Belirtin **abonelik** Klasik VNet sahip. 
    2. Seçin **VNet**. <br/>
    4. Seçin **alt**.<br/> 
1. Tıklatın **son** Azure SSIS Integration zamanının oluşturmayı başlatmak için. 

    > [!IMPORTANT]
    > - Bu işlem yaklaşık 20 tamamlanması için dakika sürer.
    > - Data Factory hizmetinin, Azure SQL SSIS Katalog veritabanı (SSISDB) hazırlamak için veritabanına bağlar. Betik ayrıca belirtilmişse sanal ağınıza ilişkin izin ve ayarları yapılandırır ve yeni Azure SSIS tümleştirme çalışma zamanı örneğini sanal ağa ekler.
7. İçinde **bağlantıları** penceresinde, anahtara **tümleştirme çalışma zamanları** gerekiyorsa. Tıklatın **yenileme** yenilemek için. 

    ![Oluşturma durumu](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)
8. Bağlantılar altından kullanmak **Eylemler** izlemek, Durdur/Başlat, düzenlemek veya tümleştirme çalışma zamanı silmek için sütun. Son bağlantı tümleştirmesi çalışma zamanı JSON kodunu görüntülemek için kullanın. Yalnızca IR durdurulduğunda düzenleme ve silme düğmeleri etkinleştirilir. 

    ![Azure SSIS IR - Eylemler](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)        
9. Tıklatın **İzleyici** altında bağlantı **Eylemler**.  

    ![Azure SSIS IR - Ayrıntılar](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-details.png)
10. Varsa bir **hata** Azure SSIS IR ile ilişkili hata sayısı bu sayfada gördüğünüz ve görüntülemek için bağlantıyı hata hakkındaki ayrıntılar. Örneğin, veritabanı sunucusunda SSIS katalog zaten varsa, SSISDB veritabanını varlığını gösteren bir hata görürsünüz.  
11. Tıklatın **tümleştirme çalışma zamanları** üst data factory ile ilişkili tüm tümleştirme çalışma zamanları görmek için önceki sayfasına geri gidin.  

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Portalda Azure SSIS tümleştirme çalışma zamanları

1. Azure veri fabrikası Arabiriminde geçmek **Düzenle** sekmesini tıklatın, **bağlantıları**ve ardından geçmek **tümleştirme çalışma zamanları** içinde varolan tümleştirme çalışma zamanları görüntülemek için Veri Fabrikası. 
    ![Varolan IRS görüntüleyin](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)
1. Tıklatın **yeni** yeni bir Azure SSIS IR oluşturmak için 

    ![Menüsü aracılığıyla tümleştirme çalışma zamanı](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)
2. Bir Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için tıklatın **yeni** görüntüde gösterildiği gibi. 
3. Tümleştirme çalışma zamanı Kurulumu penceresinde seçin **Azure'da yürütmek için yükseltme shift mevcut SSIS paketleri**ve ardından **sonraki**.

    ![Tümleştirme çalışma zamanı türünü belirtin](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)
4. Bkz: [Azure SSIS tümleştirmesi çalışma zamanı sağlamak](#provision-an-azure-ssis-integration-runtime) geri kalan bölümü adımları Azure SSIS IR ayarlamak için

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

### <a name="create-variables"></a>Değişken oluşturma
Bu öğreticideki betiklerde kullanılacak değişkenleri tanımlayın:

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

# Remove these the following two OPTIONAL variables if you are using Azure SQL Database. 
# These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# Get the following information from the properties page for your Classic Virtual Network in the Azure portal
# It should be in the format: $VnetId = "/subscriptions/<Azure Subscription ID>/resourceGroups/<Azure Resource Group>/providers/Microsoft.ClassicNetwork/virtualNetworks/<Class Virtual Network Name>"

# OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

```
### <a name="log-in-and-select-subscription"></a>Oturum açma ve abonelik seçme
Oturum açma ve Azure aboneliğinizi seçin komut dosyasını aşağıdaki kodu ekleyin: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database"></a>Veritabanı bağlantısını doğrulama
Azure SQL veritabanı sunucusu server.database.windows.net veya Azure SQL yönetilen örneği (özel olarak incelenmektedir) sunucusu uç noktası doğrulamak için aşağıdaki betiği ekleyin. 

```powershell
$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}
```

### <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
Azure-SSIS tümleştirme çalışma zamanının katılacağı sanal ağın izinlerini/ayarlarını otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin.

```powershell
# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma
SSIS paketleri Azure'da çalışan bir Azure SSIS tümleştirmesi çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın: veritabanı (Azure SQL veritabanı ile türüne göre bölümünden komut dosyası kullanma Kullanmakta olduğunuz Azure SQL yönetilen örneği (özel olarak incelenmektedir)). 

#### <a name="azure-sql-database-to-host-the-ssisdb-database-ssis-catalog"></a>(SSIS katalog) SSISDB veritabanını barındırmak için Azure SQL veritabanı 

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -CatalogPricingTier $SSISDBPricingTier `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode
```

VNetId ve alt ağ için değerleri geçirmeniz gerekmez şirket içi veri erişimi gerekmedikçe diğer bir deyişle, şirket içi veri kaynakları/hedefleri SSIS paketlerinizi sahip. CatalogPricingTier parametresi için değer geçmesi gerekir. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarına listesi için bkz: [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md).

#### <a name="azure-sql-managed-instance-private-preview-to-host-the-ssisdb-database"></a>Azure SQL yönetilen SSISDB veritabanını barındırmak üzere örneği (özel olarak incelenmektedir)

```powershell
$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName
```

Azure SQL yönetilen bir VNet katıldığı örneği (özel olarak incelenmektedir) ile VnetId ve alt ağ parametreleri için değerleri geçirmeniz gerekir. CatalogPricingTier parametresi yönetilen Azure SQL örneği için geçerli değildir. 

### <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```
Bu komutun tamamlanması **20-30 dakika** sürer. 


### <a name="full-script"></a>Tam betik
Burada, Azure SSIS IR oluşturan ve bir sanal ağa katılırsa tam komut verilmiştir. Bu komut dosyası SSIS katalog barındırmak için Azure SQL yönetilen örneği (mı) kullandığınızı varsayar. 

```powershell
# Azure Data Factory version 2 information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$".
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$DataFactoryLocation = "EastUS" 

# Azure-SSIS integration runtime information - This is the Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS integration runtime name]"
$AzureSSISDescription = "This is my Azure-SSIS integration runtime"
$AzureSSISLocation = "EastUS" 
# In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeSize = "Standard_A4_v2" 
# In public preview, only 1-10 nodes are supported.
$AzureSSISNodeNumber = 2 
# In public preview, only 1-8 parallel executions per node are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two OPTIONAL variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
# In public preview, only classic virtual network (VNet) is supported.
$VnetId = "[your VNet resource ID or leave it empty]" 
$SubnetName = "[your subnet name or leave it empty]" 

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

$SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID="+ $SSISDBServerAdminUserName +";Password="+ $SSISDBServerAdminPassword
$sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
Try
{
    $sqlConnection.Open();
}
Catch [System.Data.SqlClient.SqlException]
{
    Write-Warning "Cannot connect to your Azure SQL DB logical server/Azure SQL MI server, exception: $_"  ;
    Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
    $yn = Read-Host
    if(!($yn -ieq "Y"))
    {
        Return;
    } 
}

# Register to Azure Batch resource provider
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    $BatchObjectId = (Get-AzureRmADServicePrincipal -ServicePrincipalName "MicrosoftAzureBatch").Id
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    # Assign VM contributor role to Microsoft.Batch
    New-AzureRmRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
}

Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

$secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
$serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)
Set-AzureRmDataFactoryV2IntegrationRuntime  -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Type Managed `
                                            -CatalogServerEndpoint $SSISDBServerEndpoint `
                                            -CatalogAdminCredential $serverCreds `
                                            -Description $AzureSSISDescription `
                                            -Location $AzureSSISLocation `
                                            -NodeSize $AzureSSISNodeSize `
                                            -NodeCount $AzureSSISNodeNumber `
                                            -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                            -VnetId $VnetId `
                                            -Subnet $SubnetName

write-host("##### Starting #####")
Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```



## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran Azure SQL sunucunuza bağlanın. Azure SQL sunucusunun adı şu biçimdedir: &lt;servername&gt;. database.windows.net (Azure SQL veritabanı için). Yönergeler için [Paket dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) makalesine bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belge diğer Azure SSIS IR konulara bakın:

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-deploy-ssis-packages-azure.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına (VNet) ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımları sunar. 