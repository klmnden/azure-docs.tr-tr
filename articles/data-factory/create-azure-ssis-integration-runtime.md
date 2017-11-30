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
ms.openlocfilehash: fcd2547112eb966420f33cec4939c83606029444
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Azure Data Factory'de bir Azure SSIS tümleştirmesi çalışma zamanı oluşturma
Bu makalede Azure Data Factory bir Azure SSIS tümleştirmesi çalışma zamanı sağlamak için adımları sağlar. Daha sonra, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak Azure’da bu çalışma zamanına SQL Server Integration Services (SSIS) paketleri dağıtabilirsiniz.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-introduction.md) konusunu inceleyin.

Öğretici: [Öğreticisi: SQL Server Integration Services (SSIS) Azure'a dağıtabilmeniz](tutorial-deploy-ssis-packages-azure.md) Azure SQL veritabanını depo olarak SSIS kataloğunu kullanarak Azure SSIS tümleştirmesi çalışma zamanı (IR) oluşturulacağını gösterir. Bu makalede öğreticiyi genişletir ve aşağıdakilerin nasıl yapılacağını gösterir: 

- Azure SQL yönetilen örneği (özel olarak incelenmektedir) SSIS katalog (SSISDB veritabanı) barındırmak için kullanın.
- Bir Azure sanal ağı (VNet) için Azure SSIS IR katılın. 

Bir sanal ağa bir Azure SSIS IR birleştirme ve Azure portalında VNet yapılandırma kavramsal bilgi için bkz: [katılma Azure SSIS IR vnet'e](join-azure-ssis-integration-runtime-virtual-network.md). 

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**. Bir aboneliğiniz yoksa, bir [ücretsiz deneme](http://azure.microsoft.com/pricing/free-trial/) hesabı oluşturabilirsiniz.
- **Azure SQL Veritabanı sunucusu** veya **SQL Server Yönetilen Örneği (özel önizleme) (Genişletilmiş Özel Önizleme)**. Henüz bir veritabanı sunucunuz yoksa, başlamadan önce Azure portalında bir tane oluşturun. Bu sunucu, SSIS Katalog veritabanını (SSISDB) barındırır. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden SSISDB’ye yürütme günlüklerini yazmasına olanak tanır. Azure SQL sunucusunun fiyatlandırma katmanı unutmayın. Azure SQL veritabanı için desteklenen fiyatlandırma katmanlarına listesi için bkz: [SQL veritabanı kaynak sınırları](../sql-database/sql-database-resource-limits.md).
- **Klasik Sanal Ağ (VNet) (isteğe bağlı)**. Aşağıdaki koşulların en az biri geçerli ise bir Azure Sanal Ağınız (VNet) olmalıdır:
    - SSIS Katalog veritabanını bir sanal ağın parçası olan SQL Server Yönetilen Örneği (özel önizleme) üzerinde barındırıyorsanız.
    - Şirket içi veri depolarına bir Azure-SSIS tümleştirme çalışma zamanı üzerinde çalışan SSIS paketlerinden bağlanmak istiyorsanız.
- **Azure PowerShell**. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) bölümündeki yönergeleri izleyin. Bulutta SSIS paketleri çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı sağlamak üzere betik çalıştırmak için PowerShell kullanıyorsanız. 

> [!NOTE]
> Azure Data Factory V2 ve Azure-SSIS Integration Runtime tarafından desteklenen tüm bölgelerin listesi için bkz. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/). **Veri ve Analiz**’i genişleterek **Data Factory V2** ve **SSIS Integration Runtime** seçeneklerini görün.


## <a name="create-variables"></a>Değişken oluşturma
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
$AzureSSISNodeSize = "Standard_A4_v2" # In public preview, only Standard_A4_v2|Standard_A8_v2|Standard_D1_v2|Standard_D2_v2|Standard_D3_v2|Standard_D4_v2 are supported.
$AzureSSISNodeNumber = 2 # In public preview, only 1-10 nodes are supported.
$AzureSSISMaxParallelExecutionsPerNode = 2 # In public preview, only 1-8 parallel executions per node are supported.

# SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or your Azure SQL Managed Instance (private preview) server endpoint]"
$SSISDBServerAdminUserName = "[your server admin username]"
$SSISDBServerAdminPassword = "[your server admin password]"

# Remove the SSISDBPricingTier variable if you are using Azure SQL Managed Instance (private preview)
# This parameter applies only to Azure SQL Database. For the basic pricing tier, specify "Basic", not "B". For standard tiers, specify "S0", "S1", "S2", 'S3", etc.
$SSISDBPricingTier = "[your Azure SQL Database pricing tier. Examples: Basic, S0, S1, S2, S3, etc.]"

## Remove these two variables if you are using Azure SQL Database. 
## These two parameters apply if you are using VNet and Azure SQL Managed Instance (private preview). 
$VnetId = "[your VNet resource ID or leave it empty]" # OPTIONAL: In public preview, only classic virtual network (VNet) is supported.
$SubnetName = "[your subnet name or leave it empty]" # OPTIONAL: In public preview, only classic VNet is supported.

```

## <a name="validate-the-connection-to-database"></a>Veritabanı bağlantısını doğrulama
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

## <a name="log-in-and-select-subscription"></a>Oturum açma ve abonelik seçme
Oturum açma ve Azure aboneliğinizi seçin komut dosyasını aşağıdaki kodu ekleyin: 

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $SubscriptionName
```

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
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

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzureRmResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzureRmDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma
Azure’da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın: 

Kullanıyorsanız **Azure SQL veritabanı** (SSIS katalog) SSISDB veritabanını barındırmak için: 


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

Kullanıyorsanız **Azure SQL yönetilen örneği (özel olarak incelenmektedir)** SSISDB veritabanını barındırmak için:

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

## <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma
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

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma
Şimdi SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak SSIS paketlerinizi Azure’a dağıtın. SSIS kataloğunu (SSISDB) barındıran Azure SQL sunucunuza bağlanın. Azure SQL sunucusunun adı şu biçimdedir: &lt;servername&gt;. database.windows.net (Azure SQL veritabanı için). Yönergeler için [Paket dağıtma](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server) makalesine bakın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu belge diğer Azure SSIS IR konulara bakın:

- [Azure SSIS tümleştirmesi çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure SSIS IR genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-deploy-ssis-packages-azure.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makale bir Azure-SSIS IR’yi durdurma, başlatma veya kaldırma işlemini gösterir. Ayrıca, IR’ye daha fazla düğüm ekleyerek Azure-SSIS IR’nizi ölçeklendirmeyi gösterir. 
- [Azure-SSIS IR’yi bir sanal ağa ekleme](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına (VNet) ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımları sunar. 