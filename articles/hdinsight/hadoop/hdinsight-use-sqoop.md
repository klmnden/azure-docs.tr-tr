---
title: Azure HDInsight (Apache Hadoop) ile Apache Sqoop işleri çalıştırma
description: Bir Hadoop kümesi ile bir Azure SQL veritabanı arasında Sqoop alma ve için bir iş istasyonundan Azure PowerShell'i kullanma konusunda bilgi edinin.
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: 12868d2b8c4607e4582f9180d0428d3179fb00ee
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361243"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>HDInsight, Hadoop ile Apache Sqoop'u kullanma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Apache Sqoop alma ve HDInsight kümesi ve Azure SQL veritabanı veya SQL Server veritabanı arasında dışa aktarmak için HDInsight kullanmayı öğrenin.

Apache Hadoop günlüklerini ve dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış verileri işlemek için doğal bir seçim olsa da olabilir ilişkisel veritabanlarının depolanan yapılandırılmış verileri işlemek için bir gereksinim.

[Apache Sqoop] [ sqoop-user-guide-1.4.4] Hadoop kümeleri ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle Hadoop dağıtılmış dosya sistemi (HDFS) ile Hadoop MapReduce veya Apache Hive ile verileri dönüştürün ve ardından bir RDBMS'de geri verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemi (RDBMS) verilerini almak için kullanın . Bu öğreticide, bir SQL Server veritabanı ilişkisel veritabanınız için kullanırsınız.

HDInsight kümelerinde desteklenir Sqoop sürümleri için bkz: [HDInsight tarafından sağlanan küme sürümlerindeki yenilikler nelerdir?][hdinsight-versions]

## <a name="understand-the-scenario"></a>Senaryoyu anlama

HDInsight küme bazı örnek verilerle birlikte gelir. Aşağıdaki iki örnek kullanabilirsiniz:

* Şu konumdadır bir Apache Log4j günlük dosyasını */example/data/sample.log*. Aşağıdaki günlüklere dosyasından ayıklanır:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Adlı bir Hive tablosu *hivesampletable*, başvuruda bulunan veri dosyası */hive/warehouse/hivesampletable*. Tablo bazı mobil cihaz verilerini içerir. 
  
  | Alan | Veri türü |
  | --- | --- |
  | ClientID |string |
  | querytime |string |
  | Pazar |string |
  | deviceplatform |string |
  | devicemake |string |
  | devicemodel |string |
  | durum |string |
  | Ülke |string |
  | querydwelltime |double |
  | oturum kimliği |bigint |
  | sessionpagevieworder |bigint |

Bu öğreticide, test Sqoop alma ve dışarı aktarmak için bu iki veri kümesi kullanın.

## <a name="create-cluster-and-sql-database"></a>Küme ve SQL veritabanı oluşturma
Bu bölümde bir küme, SQL veritabanı ve Azure portalı ve Azure Resource Manager şablonu kullanarak öğreticiyi çalıştırmak için SQL veritabanı şemalarını oluşturulacağını gösterir. Şablon bulunabilir [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). Resource Manager şablonu, SQL veritabanı'na Tablo şemalarını dağıtmak için bir bacpac paketi çağırır.  Bacpac paketi bir ortak blob kapsayıcısında yer https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Özel bir kapsayıcı bacpac dosyalarını kullanmak istiyorsanız, şablonda aşağıdaki değerleri kullanın:
   
```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

Küme ve SQL veritabanı oluşturacağınızı öğrenmek için Azure PowerShell kullanmayı tercih ederseniz [ek A](#appendix-a---a-powershell-sample).

> [!NOTE]  
> Şablon kullanarak içeri aktarın veya Azure portalı, yalnızca bir BACPAC dosyasını Azure blob depolama alanından içeri destekler.

**Kaynak Yönetimi şablonu ile ortamı yapılandırmak için**
1. Azure portalında Resource Manager şablonu açmak için aşağıdaki görüntüye tıklayın.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
2. Aşağıdaki özellikleri girin:

    - **Abonelik**: Azure aboneliğinizi girin.
    - **Kaynak grubu**: Yeni bir Azure kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Bir kaynak grubu için yönetim amaçlı olan.  Nesneler için bir kapsayıcıdır.
    - **Konum**: Bölge seçin.
    - **ClusterName**: Hadoop kümesi için bir ad girin.
    - **Küme oturum açma adı ve parola**: Varsayılan oturum açma adı, admin’dir.
    - **SSH kullanıcı adı ve parola**.
    - **SQL veritabanı sunucusu oturum açma adı ve parola**.
    - **_artifacts konumu**: Farklı bir konumda kendi bacpac dosyanızı kullanmak istediğiniz sürece varsayılan değeri kullanın.
    - **_artifacts konumu Sas belirteci**: Boş bırakın.
    - **Bacpac dosyası adı**: Kendi bacpac dosyanızı kullanmak istediğiniz sürece varsayılan değeri kullanın.
     
        Değişkenler bölümünde sabit kodlanmış değerler:
        
        |Ad|Değer|
        |----|-----|
        | Varsayılan depolama hesabı adı | &lt;ClusterName > depolayın |
        | Azure SQL veritabanı sunucu adı | &lt;ClusterName > dbserver |
        | Azure SQL veritabanı adı | &lt;ClusterName > db |
     
3. Seçin **hüküm ve koşulları yukarıda belirtilen kabul ediyorum**.
4. **Satın al**’a tıklayın. Dağıtımı şablon dağıtımı için dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin ve SQL veritabanının oluşturulması yaklaşık 20 dakika sürer.

Mevcut Azure SQL veritabanı ya da Microsoft SQL Server kullanmayı tercih ederseniz

* **Azure SQL veritabanı**: İstasyonunuzdan erişime izin vermek Azure SQL veritabanı sunucusu için bir güvenlik duvarı kuralı yapılandırmanız gerekir. Bir Azure SQL veritabanı oluşturma ve güvenlik duvarını yapılandırma hakkında yönergeler için bkz: [Azure SQL veritabanı ile çalışmaya başlamak][sqldatabase-get-started]. 
  
  > [!NOTE]  
  > Varsayılan olarak, bir Azure SQL veritabanı, Azure HDInsight gibi Azure hizmetlerinden gelen bağlantıları sağlar. Bu güvenlik duvarı ayarı devre dışıysa, Azure portalından etkinleştirmek gerekir. Bir Azure SQL veritabanı oluşturma ve güvenlik duvarı kurallarını yapılandırma hakkında daha fazla yönerge için bkz. [oluşturma ve SQL veritabanını Yapılandır][sqldatabase-create-configure].

* **SQL Server**: HDInsight kümenizi aynı sanal ağda bulunan azure'da SQL Server varsa, içeri aktarma ve verileri bir SQL Server veritabanına dışarı aktarmak için bu makaledeki adımları kullanabilirsiniz.
  
  > [!NOTE]  
  > HDInsight yalnızca konum tabanlı sanal ağlarını destekleyen ve bir benzeşim grubuna bağlı sanal ağlar ile şu anda çalışmıyor.

  
  * Oluşturma ve bir sanal ağ yapılandırma için bkz: [Azure portalını kullanarak bir sanal ağ oluşturma](../../virtual-network/quick-create-portal.md).
    
    * Veri merkezinizde, SQL Server kullanırken, sanal ağda şu şekilde yapılandırmalısınız *siteden siteye* veya *noktadan siteye*.
      
      > [!NOTE]  
      > İçin **noktadan siteye** sanal ağlar, SQL Server çalıştıran kullanılabilir yapılandırma uygulama, VPN istemcisi gerekir **Pano** Azure sanal ağı yapılandırma.


    * Azure sanal makinesinde SQL Server'ı kullanırken, herhangi bir sanal ağ yapılandırma barındıran SQL Server sanal makineyi aynı sanal ağda HDInsight'ın bir üyesi ise kullanılabilir.
  * Bir sanal ağdaki bir HDInsight kümesi oluşturmak için bkz [Apache Hadoop kümeleri oluşturma özel seçenekleri kullanarak HDInsight](../hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]  
    > SQL Server kimlik doğrulaması de izin vermeniz gerekir. Bu makaledeki adımları tamamlayabilmeniz için bir SQL Server oturumu kullanmanız gerekir.


**Yapılandırmayı doğrulamak için**

1. Kaynak grubu, Azure portalında açın. Dört kaynak grubunda görmeniz gerekir:

    - Küme
    - Veritabanı sunucusu
    - Veritabanı
    - varsayılan depolama hesabı

2. Veritabanını Microsoft SQL Server Management Studio'da açın.  Dağıtılan iki veritabanı görmeniz gerekir:

    ![Azure HDInsight Sqoop SQL Management Studio](./media/hdinsight-use-sqoop/hdinsight-sqoop-sql-management-studio.png)


## <a name="run-sqoop-jobs"></a>Sqoop işleri çalıştırma
HDInsight, çeşitli yöntemler kullanarak Sqoop işleri çalıştırabilirsiniz. Hangi yöntemin size uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir kılavuz için bağlantıyı izleyin.

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |? |Linux veya Windows |Windows (şimdilik ile) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? |Linux veya Windows |Windows |

## <a name="limitations"></a>Sınırlamalar
* Toplu Dışarı Aktar - ile Linux tabanlı HDInsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışarı aktarmak için kullanılan Sqoop bağlayıcısının toplu ekleme şu anda desteklemiyor.
* -Kullanırken, Linux tabanlı HDInsight ile toplu işleme `-batch` ekler gerçekleştirirken geçiş, Sqoop toplu INSERT işlemler yerine birden çok ekleme yapar.

## <a name="next-steps"></a>Sonraki adımlar
Artık Sqoop kullanmayı öğrendiniz. Daha fazla bilgi için bkz:

* [Apache Hive, HDInsight ile kullanma](../hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](../hdinsight-use-pig.md)
* [HDInsight için verileri karşıya][hdinsight-upload-data]: HDInsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

## <a name="appendix-a---a-powershell-sample"></a>Ek A - PowerShell örneği

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell örneği, aşağıdaki adımları gerçekleştirir:

1. Azure'a bağlanın.
2. Bir Azure kaynak grubu oluşturun. Daha fazla bilgi için [Azure PowerShell'i Azure Resource Manager ile kullanma](../../azure-resource-manager/manage-resource-groups-powershell.md)
3. Azure SQL veritabanı sunucusu, bir Azure SQL veritabanı ve iki tablo oluşturun. 
   
    Bunun yerine SQL Server kullanıyorsanız, tablolar oluşturmak için aşağıdaki deyimleri kullanın:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    Tablo ve veritabanı incelemek için en kolay yolu, Visual Studio kullanmaktır. Veritabanı sunucusu ve veritabanı, Azure portalını kullanarak incelenebilir.
4. Bir HDInsight kümesi oluşturun.
   
    Küme incelemek için Azure portal veya Azure PowerShell kullanabilirsiniz.
5. Kaynak veri dosyasını önceden işler.
   
    Bu öğreticide, bir Azure SQL veritabanına bir log4j günlük dosyasını (sınırlandırılmış bir dosyanın) ve bir Hive tablosu verin. Sınırlandırılmış bir dosyanın adında */example/data/sample.log*. Öğreticide daha önce log4j günlük bazı örnekler gördünüz. Günlük dosyasında vardır bazı boş bazı satırları ve bunlara benzer:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Bu verileri kullanan diğer örnekler için ince budur ancak biz Azure SQL veritabanı veya SQL Server içeri aktarmadan önce bu özel durumlar kaldırmanız gerekir. Sqoop dışarı aktarma başarısız boş bir dize ya da daha az bir satır varsa Azure SQL veritabanı tablosunda tanımlanan alanların sayısını öğeden. Log4jlogs tablo yedi dize türü alanları içerir.
   
    Bu yordam küme üzerinde yeni bir dosya oluşturur: tutorials/usesqoop/data/sample.log. Değiştirilen veri dosyasını incelemek için Azure portalı, bir Azure Depolama Gezgini aracını veya Azure PowerShell kullanabilirsiniz. [HDInsight ile çalışmaya başlama] [ hdinsight-get-started] dosya indirme ve dosya içeriğini görüntülemek için Azure PowerShell kullanarak bir kod örneği vardır.
6. Bir veri dosyası, Azure SQL veritabanı için dışarı aktarın.
   
    Kaynak dosyası tutorials/usesqoop/data/sample.log değil. Verileri için burada verilen Tablo log4jlogs çağrılır.
   
   > [!NOTE]  
   > Bağlantı dizesi bilgilerini dışındaki bir Azure SQL veritabanı veya SQL Server için bu bölümdeki adımları çalışması gerekir. Bu adımlar aşağıdaki yapılandırmayı kullanarak test edilmiştir:
   > 
   > * **Azure sanal ağa noktadan siteye yapılandırma**: Bir sanal ağ özel bir veri merkezinde bir SQL Server HDInsight kümesi bağlanır. Bkz: [Yönetim Portalı'nda bir noktadan siteye VPN yapılandırma](../../vpn-gateway/vpn-gateway-point-to-site-create.md) daha fazla bilgi için.
   > * **Azure HDInsight**: Bkz: [Hadoop kümeleri oluşturma özel seçenekleri kullanarak HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) bir sanal ağda küme oluşturma hakkında daha fazla bilgi için.
   > * **SQL Server 2014**: Kimlik doğrulaması VPN istemcisi yapılandırma paketini güvenli bir sanal ağa bağlanmak için çalışan izin verecek biçimde yapılandırılmış.
   > 
   > 
7. Bir Hive tablosu, Azure SQL veritabanı için dışarı aktarın.
8. HDInsight küme mobiledata tabloyu aktarın.
   
    Değiştirilen veri dosyasını incelemek için Azure portalı, bir Azure Depolama Gezgini aracını veya Azure PowerShell kullanabilirsiniz.  [HDInsight ile çalışmaya başlama] [ hdinsight-get-started] dosya indirme ve dosya içeriğini görüntülemek için Azure PowerShell kullanma hakkında bir kod örneği vardır.

### <a name="the-powershell-sample"></a>PowerShell örneği

```powershell
# Prepare an Azure SQL database to be used by the Sqoop tutorial

#region - provide the following values

$subscriptionID = "<Enter your Azure Subscription ID>"

$sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
$sqlDatabasePassword = "<Enter a Password>"

$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = "<Enter a Password>"

$sshUserName = "sshuser" #HDInsight ssh username
$sshPassword = $httpPassword 

# used for creating Azure service names
$nameToken = "<Enter an alias>" 
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
#endregion

#region - variables

# Resource group variables
$resourceGroupName = $namePrefix + "rg"
$location = "East US 2" # used by all Azure services defined in this tutorial

# SQL database varialbes
$sqlDatabaseServerName = $namePrefix + "sqldbserver"
$sqlDatabaseName = $namePrefix + "sqldb"
$sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
$sqlDatabaseMaxSizeGB = 10

# Used for retrieving external IP address and creating firewall rules
$ipAddressRestService = "https://bot.whatismyipaddress.com"
$fireWallRuleName = "UseSqoop"

# Used for creating tables and clustered indexes
$cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
    [t1] [nvarchar](50),
    [t2] [nvarchar](50),
    [t3] [nvarchar](50),
    [t4] [nvarchar](50),
    [t5] [nvarchar](50),
    [t6] [nvarchar](50),
    [t7] [nvarchar](50))"

$cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

$cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
[clientid] [nvarchar](50),
[querytime] [nvarchar](50),
[market] [nvarchar](50),
[deviceplatform] [nvarchar](50),
[devicemake] [nvarchar](50),
[devicemodel] [nvarchar](50),
[state] [nvarchar](50),
[country] [nvarchar](50),
[querydwelltime] [float],
[sessionid] [bigint],
[sessionpagevieworder][bigint])"

$cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

# HDInsight variables
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName
#endregion

# Treat all errors as terminating
$ErrorActionPreference = "Stop"

#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzContext}
catch{Connect-AzAccount}
#endregion

#region - Create Azure resource group
Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
try{
    Get-AzResourceGroup -Name $resourceGroupName
}
catch{
    New-AzResourceGroup -Name $resourceGroupName -Location $location
}
#endregion

#region - Create Azure SQL database server
Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
try{
    Get-AzSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
catch{
    Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

    $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

    $sqlDatabaseServerName = (New-AzSqlServer `
                                -ResourceGroupName $resourceGroupName `
                                -ServerName $sqlDatabaseServerName `
                                -SqlAdministratorCredentials $credential `
                                -Location $location).ServerName
    Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

    Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
    $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
    New-AzSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-workstation" `
        -StartIpAddress $workstationIPAddress `
        -EndIpAddress $workstationIPAddress

    #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
    #Note that this allows Azure traffic from any Azure subscription to access the server.
    New-AzSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-Azureservices" `
        -StartIpAddress "0.0.0.0" `
        -EndIpAddress "0.0.0.0"
}

#endregion

#region - Create and validate Azure SQL database
Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

try {
    Get-AzSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName
}
catch {
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    New-AzSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName `
        -Edition "Standard" `
        -RequestedServiceObjectiveName "S1"
}

#endregion

#region - Create tables
Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green

$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = $sqlDatabaseConnectionString
$conn.Open()

# Create the log4jlogs table and index
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.Connection = $conn
$cmd.CommandText = $cmdCreateLog4jTable
$ret = $cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateLog4jClusteredIndex
$cmd.ExecuteNonQuery()

# Create the mobiledata table and index
$cmd.CommandText = $cmdCreateMobileTable
$cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateMobileDataClusteredIndex
$cmd.ExecuteNonQuery()

$conn.close()

#endregion


#region - Create HDInsight cluster

Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

# Create the default storage account
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_LRS

# Create the default Blob container
$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageAccountContext = New-AzStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey 
New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageAccountContext 

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $HDInsightClusterName `
    -Location $location `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version 3.6 `
    -ClusterSizeInNodes 2 `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $defaultBlobContainerName  

# Validate the cluster
Get-AzHDInsightCluster -ClusterName $hdinsightClusterName
#endregion

#region - pre-process the source file

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

# This procedure creates a new file with $destBlobName
$sourceBlobName = "example/data/sample.log"
$destBlobName = "tutorials/usesqoop/data/sample.log"

# Define the connection string
$storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

# Create block blob objects referencing the source and destination blob.
$storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
$storageClient = $storageAccount.CreateCloudBlobClient();
$storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
$sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
$destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

# Define a MemoryStream and a StreamReader for reading from the source file
$stream = New-Object System.IO.MemoryStream
$stream = $sourceBlob.OpenRead()
$sReader = New-Object System.IO.StreamReader($stream)

# Define a MemoryStream and a StreamWriter for writing into the destination file
$memStream = New-Object System.IO.MemoryStream
$writeStream = New-Object System.IO.StreamWriter $memStream

# Pre-process the source blob
$exString = "java.lang.Exception:"
while(-Not $sReader.EndOfStream){
    $line = $sReader.ReadLine()
    $split = $line.Split(" ")

    # remove the "java.lang.Exception" from the first element of the array
    # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
    if ($split[0] -eq $exString){
        #create a new ArrayList to remove $split[0]
        $newArray = [System.Collections.ArrayList] $split
        $newArray.Remove($exString)

        # update $split and $line
        $split = $newArray
        $line = $newArray -join(" ")
    }

    # remove the lines that has less than 7 elements
    if ($split.count -ge 7){
        write-host $line
        $writeStream.WriteLine($line)
    }
}

# Write to the destination blob
$writeStream.Flush()
$memStream.Seek(0, "Begin")
$destBlob.UploadFromStream($memStream)

#endregion

#region - export a log file from the cluster to the SQL database

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

$tableName_log4j = "log4jlogs"

# Connection string for Azure SQL Database.
# Comment if using SQL Server
$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
# Connection string for SQL Server.
# Uncomment if using SQL Server.
#$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

$exportDir_log4j = "/tutorials/usesqoop/data"

# Submit a Sqoop job
$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose
Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

#endregion

#region - export a Hive table

$tableName_mobile = "mobiledata"
$exportDir_mobile = "/hive/warehouse/hivesampletable"

$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion

#region - import a database

$targetDir_mobile = "/tutorials/usesqoop/importeddata/"

$sqoopDef = New-AzHDInsightSqoopJobDefinition `
    -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

$sqoopJob = Start-AzHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion
```


[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: ../hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../../sql-database/sql-database-get-started.md
[sqldatabase-create-configure]: ../../sql-database/sql-database-get-started.md

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
