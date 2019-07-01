---
title: PowerShell ile Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama | Microsoft Docs
description: Azure'da SSIS paketleri dağıtıp çalıştırabilmek için PowerShell ile Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı sağlamayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: bbbbc45bd050b8f68499febeed6285ad1aa883c4
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484771"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Azure Data Factory'de PowerShell ile Azure-SSIS Tümleştirme Çalışma Zamanı Sağlama

Bu öğreticide, bir Azure SQL Server Integration Services (SSIS) Integration Runtime (IR) Azure Data Factory (ADF) sağlama adımlarını sunar. Azure SQL veritabanı sunucusu/tarafından yönetilen örneği (Proje dağıtım modeli) barındırılan SSIS kataloğunu (SSISDB) ve bu dosya sistemlerine/dosyasına dağıtılan dağıtılan paketleri çalıştıran bir azure-SSIS IR destekler paylaşımları/Azure dosyaları (paket dağıtım modeli). Azure-SSIS IR sağlandıktan sonra daha sonra SQL Server veri Araçları (SSDT) gibi alışık olduğunuz araçları kullanabilirsiniz / SQL Server Management Studio (SSMS) ve komut satırı yardımcı programları gibi `dtinstall` / `dtutil` / `dtexec`, dağıtın ve paketlerinizi Azure'da çalıştırın. Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!NOTE]
> Bu makalede, bir Azure-SSIS IR sağlamak için Azure PowerShell'i kullanır. Bir Azure-SSIS IR sağlamak için Azure portal/ADF uygulama kullanmak için bkz: [Öğreticisi: Azure-SSIS IR sağlama](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SSIS tümleştirme çalışma zamanı oluşturma
> * Azure SSIS tümleştirme çalışma zamanını başlatma
> * Tam betiği gözden geçirme
> * SSIS paketlerini dağıtma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure-SSIS IR hakkında kavramsal bilgiler için bkz. [Azure SSIS tümleştirme çalışma zamanına genel bakış](concepts-integration-runtime.md#azure-ssis-integration-runtime).
- **Azure SQL veritabanı sunucusu (isteğe bağlı)** . Bir veritabanı sunucusu zaten yoksa, başlamadan önce Azure portalında bir tane oluşturun. ADF sırayla SSISDB bu veritabanı sunucusunda oluşturur. Veritabanı sunucusunu tümleştirme çalışma zamanı ile aynı Azure bölgesinde oluşturmanız önerilir. Bu yapılandırma, tümleştirme çalışma zamanının Azure bölgelerinden geçmeden ssısdb'ye yürütme günlüklerini SSISDB yazma sağlar. 
    - Seçilen veritabanı sunucusuna göre SSISDB sizin adınıza tek veritabanı, elastik havuzun bir parçası veya Yönetilen Örnek biçiminde oluşturulabilir ve genel ağ üzerinden veya sanal ağa eklenerek erişilebilir. SSISDB'yi barındırmak için veritabanı sunucu türünü seçmek kılavuzluk etmesi için bkz [karşılaştırma Azure SQL veritabanı tek veritabanı/esnek havuz/yönetilen örnek](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Azure SQL veritabanı sunucusu ile sanal ağ hizmet uç noktaları/yönetilen örnek bir sanal ağdaki SSISDB barındırmak veya şirket içi verilere erişmesi için kullandığınız gerekirse, Azure-SSIS IR'yi bir sanal ağa katılın, bkz: [Azure-SSIS IR oluşturma bir sanal ağdaki](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucusunda **Azure hizmetlerine erişime izin ver** ayarının etkin olduğundan emin olun. Sanal ağ hizmet uç noktaları/yönetilen örnek SSISDB'yi barındırmak için bir sanal ağ ile Azure SQL veritabanı sunucusu kullandığınızda, bu geçerli değildir. Daha fazla bilgi için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). PowerShell kullanarak bu ayarı etkinleştirmek için bkz: [yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - İstemci makinenin IP adresini veya veritabanı sunucusu için Güvenlik Duvarı ayarlarındaki istemci IP adresi listesine istemci makinenin IP adresini içeren bir IP adresi aralığı ekleyin. Daha fazla bilgi için bkz. [Azure SQL Veritabanı'nda sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma](../sql-database/sql-database-firewall-configure.md).
    - Sunucu yönetici kimlik bilgilerinizle SQL kimlik doğrulaması veya Azure Active Directory (AAD) kimlik doğrulaması için ADF ile yönetilen kimlik kullanarak veritabanı sunucusunda bağlanabilirsiniz.  İkinci seçenekte ADF için yönetilen kimliğinizi veritabanı sunucusuna erişim izni olan bir AAD grubuna eklemeniz gerekir, bkz. [AAD kimlik doğrulaması ile Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Veritabanı sunucunuza bir SSISDB zaten sahip olmadığını onaylayın. Bir Azure-SSIS IR'nin sağlanması var olan bir SSISDB kullanma desteği olmamasıdır.
- **Azure PowerShell**. Bölümündeki yönergeleri [Azure PowerShell'i yükleme ve yapılandırma konusunda](/powershell/azure/install-Az-ps), isterseniz, Azure-SSIS IR sağlamak için bir PowerShell betiğini çalıştırmak

> [!NOTE]
> - ADF ve Azure-SSIS IR olduğu anda Azure bölgelerinin listesi için bkz. [ADF + SSIS IR bölgelere göre kullanılabilirliği](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="launch-windows-powershell-ise"></a>Windows PowerShell ISE’yi başlatma

**Windows PowerShell ISE**’yi yönetici ayrıcalıklarıyla başlatın. 

## <a name="create-variables"></a>Değişken oluşturma

Kopyalayın ve aşağıdaki betiği yapıştırın: değişkenlerin değerlerini belirtin. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

## <a name="sign-in-and-select-subscription"></a>Oturum açın ve aboneliği seçin

Komut dosyasını açıp Azure aboneliğinizi seçmek için aşağıdaki kodu ekleyin.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

## <a name="validate-the-connection-to-database-server"></a>Veritabanı sunucusu için bağlantıyı doğrulama

Azure SQL veritabanı sunucunuzun doğrulamak için aşağıdaki betiği ekleyin. 

```powershell
# Validate only if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
    Try
    {
        $sqlConnection.Open();
    }
    Catch [System.Data.SqlClient.SqlException]
    {
        Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
        Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
        $yn = Read-Host
        if(!($yn -ieq "Y"))
        {
            Return;
        } 
    }
}
```

Betiğin bir parçası Azure SQL veritabanı oluşturmak için aşağıdaki örneğe bakın: 

Önceden tanımlanmamış değişkenlerin değerlerini belirleyin. Örneğin: SSISDBServerName, Firewallıpaddress. 

```powershell
New-AzSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Oluşturma bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) kullanarak [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubunuz zaten varsa, bu kodu betiğinize kopyalamayın. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Tümleştirme çalışma zamanı oluşturma

Azure'da SSIS paketlerini çalıştıran bir Azure-SSIS tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutları çalıştırın.

SSISDB kullanmazsanız CatalogServerEndpoint CatalogPricingTier ve CatalogAdminCredential parametreleri atlayabilirsiniz.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -CatalogAdminCredential $serverCreds
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

## <a name="start-integration-runtime"></a>Tümleştirme çalışma zamanını başlatma

Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutları çalıştırın.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```

> [!NOTE]
> Herhangi bir özel kurulum süresi, bu işlem, 5 dakika içinde tamamlanmalıdır.
>
> SSISDB kullanırsanız, ADF hizmet SSISDB hazırlamak için veritabanı sunucusuna bağlanır. 
> 
> Bir Azure-SSIS IR sağladığınızda, Access Redistributable ve SSIS için Azure Feature Pack da yüklenir. Bu bileşenler, Excel/erişim dosyaları ve zaten yerleşik bileşenler tarafından desteklenen veri kaynaklarının yanı sıra çeşitli Azure veri kaynaklarıyla bağlantı sağlar. Ek bileşenleri de yükleyebilirsiniz bkz [Azure-SSIS IR için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="full-script"></a>Tam betik

Bu bölümdeki PowerShell Betiği, Azure-SSIS IR, SSIS paketlerini çalıştıran bir örneğini yapılandırır. Sonra bu betiği başarıyla çalıştırdıktan dağıtabilir ve Azure'da SSIS paketleri çalıştırma.

1. Windows PowerShell Tümleşik Komut Dosyası Ortamı’nı (ISE) başlatın.
2. ISE'de komut isteminden aşağıdaki komutu çalıştırın.  

    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```

3. Bu bölümde PowerShell betiğini kopyalayıp ISE’ye yapıştırın.
4. Betiğin başında tüm parametreler için uygun değerleri sağlayın.
5. Betiği çalıştırın. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you want to use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
    Try
    {
        $sqlConnection.Open();
    }
    Catch [System.Data.SqlClient.SqlException]
    {
        Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
        Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
        $yn = Read-Host
        if(!($yn -ieq "Y"))
        {
            Return;
        } 
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -CatalogAdminCredential $serverCreds
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")   
```

## <a name="monitor-and-manage-azure-ssis-ir"></a>Azure-SSIS IR’yi izleme ve yönetme

Bir Azure-SSIS IR’yi izleme ve yönetme hakkında ayrıntılar için aşağıdaki makalelere bakın. 

- [Azure-SSIS IR'yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Azure-SSIS IR'yi yönetme](manage-azure-ssis-integration-runtime.md)

## <a name="deploy-ssis-packages"></a>SSIS paketlerini dağıtma

SSISDB kullanırsanız, paketlerinizi dağıtmayı ve Azure-SSIS IR, sunucu uç noktası aracılığıyla veritabanı sunucunuza bağlanmasına SSDT/SSMS araçlarını kullanarak çalıştırın. Genel bir uç nokta ile Azure SQL veritabanı sunucusu/yönetilen örnek için sunucu uç noktası biçimidir `<server name>.database.windows.net` / `<server name>.public.<dns prefix>.database.windows.net,3342`sırasıyla. SSISDB kullanmazsanız, paketlerinizi dosya sistemleri/dosya paylaşımları/Azure'a dağıtabilirsiniz dosyaları ve bunları Azure-SSIS IR kullanarak çalıştırın `dtinstall` / `dtutil` / `dtexec` komut satırı yardımcı programları. Daha fazla bilgi için [dağıtma SSIS paketlerini](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Her iki durumda da Azure-SSIS IR üzerinde dağıtılan paketlerinizi çalıştırabilirsiniz ADF hatlarında kullanarak SSIS paketi yürütme etkinliği bkz [çağırma SSIS paketi yürütme birinci sınıf bir ADF etkinlik olarak](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Ayrıca, SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Dağıtma, çalıştırma ve SSIS paketlerini azure'da izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure'da SSISDB bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure'da paket yürütmeler zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SSIS tümleştirme çalışma zamanı oluşturma
> * Azure SSIS tümleştirme çalışma zamanını başlatma
> * Tam betiği gözden geçirme
> * SSIS paketlerini dağıtma

Azure-SSIS tümleştirme çalışma zamanınızı özelleştirme hakkında bilgi edinmek için şu makaleye bakın:

> [!div class="nextstepaction"]
>[Azure-SSIS IR hizmetini özelleştirme](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)