---
title: Azure Hdınsight (Hadoop) ile Apache Sqoop işleri çalıştırma | Microsoft Docs
description: Azure PowerShell bir iş istasyonundan Sqoop alma çalıştırın ve bir Hadoop kümesi ve bir Azure SQL veritabanı arasında dışa aktarmak için nasıl kullanılacağını öğrenin.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: 2c9d708144ee10a7f55a6ffff33925e865ecd415
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop ile Sqoop kullanma
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Sqoop Hdınsight'ta içeri ve dışarı aktarma Hdınsight kümesi ve Azure SQL veritabanı veya SQL Server veritabanı arasında nasıl kullanılacağını öğrenin.

Hadoop günlükleri ve dosyaları gibi yapılandırılmamış ve yarı yapılandırılmış veri işleme için doğal bir seçim olsa da olabilir ilişkisel veritabanlarında depolanan yapılandırılmış verileri işlemek için gerek.

[Sqoop] [ sqoop-user-guide-1.4.4] Hadoop kümeleri ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL ve Oracle Hadoop dağıtılmış dosya sistemi (HDFS) içine Hadoop ile MapReduce veya Hive verilerde dönüştürme ve bir RDBMS verileri dışarı aktarma gibi bir ilişkisel veritabanı yönetim sistemine (RDBMS) verileri almak için kullanabilirsiniz. Bu öğreticide, ilişkisel veritabanı için SQL Server veritabanını kullanıyorsunuz.

Hdınsight kümelerinde desteklenir Sqoop sürümleri için bkz: [Hdınsight tarafından sağlanan küme sürümlerindeki yenilikler nelerdir?][hdinsight-versions]

## <a name="understand-the-scenario"></a>Senaryo anlama

Hdınsight kümesi bazı örnek verilerle birlikte gelir. Aşağıdaki iki örnek kullanın:

* Konumda bulunan bir log4j günlük dosyası */example/data/sample.log*. Günlükleri dosyasından ayıklanan:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Adlı bir Hive tablosu *hivesampletable*, veri dosyası konumunda bulunan başvuran */hive/warehouse/hivesampletable*. Tablo bazı mobil cihaz verileri içerir. 
  
  | Alan | Veri türü |
  | --- | --- |
  | istemci kimliği |string |
  | querytime |string |
  | Pazar |string |
  | deviceplatform |string |
  | devicemake |string |
  | devicemodel |string |
  | durum |string |
  | Ülke |string |
  | querydwelltime |Çift |
  | SessionID |bigint |
  | sessionpagevieworder |bigint |

Bu öğreticide, test Sqoop içeri aktarma ve dışarı aktarmak için bu iki veri kümesi kullanın.

## <a name="create-cluster-and-sql-database"></a>Küme ve SQL veritabanı oluşturma
Bu bölümde bir küme, bir SQL Database ve Azure portalı ve Azure Resource Manager şablonu kullanarak öğreticiyi çalıştırmak için SQL veritabanı şemalarını nasıl oluşturulacağını gösterir. Şablon bulunabilir [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). Resource Manager şablonu tablo şemalarını SQL veritabanına dağıtım yapmak için bir bacpac paketi çağırır.  Bir ortak blob kapsayıcısında bulunan bacpac paket https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Özel bir kapsayıcı bacpac dosyalarını kullanmak istiyorsanız, şablonda aşağıdaki değerleri kullanın:
   
```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

Küme ve SQL veritabanı oluşturmak için Azure PowerShell kullanmayı tercih ederseniz, [ek A](#appendix-a---a-powershell-sample).

> [!NOTE]
> Bir şablon kullanarak alabilir veya Azure portalı, yalnızca Azure blob depolama alanından bir BACPAC dosyasını içeri destekler.

**Bir kaynak yönetim şablonu kullanarak ortamı yapılandırmak için**
1. Azure portalında bir Resource Manager şablonunu açmak için aşağıdaki görüntüye tıklayın.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
2. Aşağıdaki özellikleri girin:

    - **Abonelik**: Azure aboneliğiniz girin.
    - **Kaynak grubu**: yeni bir Azure kaynak grubu oluşturun veya varolan bir kaynak grubu seçin.  Bir kaynak grubu yönetim amaçla ' dir.  Nesneler için bir kapsayıcıdır.
    - **Konum**: bir bölge seçin.
    - **ClusterName**: Hadoop kümesi için bir ad girin.
    - **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı admin şeklindedir.
    - **SSH kullanıcı adı ve parola**.
    - **SQL veritabanı sunucusu oturum açma adı ve parola**.
    - **_artifacts konumu**: kendi backpac dosyasını farklı bir konumda kullanmak istemediğiniz sürece varsayılan değeri kullanın.
    - **Konum Sas belirteci _artifacts**: boş bırakın.
    - **Bacpac dosya adı**: kendi backpac dosyanızı kullanmak istemediğiniz sürece varsayılan değeri kullanın.
     
        Değişkenler bölümünde sabit kodlanmış değerler:
        
        |Ad|Değer|
        |----|-----|
        | Varsayılan depolama hesabı adı | &lt;CluterName > depolama |
        | Azure SQL veritabanı sunucusu adı | &lt;ClusterName > dbserver |
        | Azure SQL veritabanı adı | &lt;ClusterName > db |
     
3. Seçin **hüküm ve koşulları yukarıda belirtildiği ediyorum**.
4. **Satın al**’a tıklayın. Şablon dağıtımı için dağıtım gönderme başlıklı yeni bir kutucuk görürsünüz. Kümenin ve SQL veritabanının oluşturulması yaklaşık 20 dakika sürer.

Mevcut Azure SQL veritabanını veya Microsoft SQL Server kullanmayı tercih ederseniz

* **Azure SQL veritabanı**: iş istasyonunuzu erişime izin verecek şekilde Azure SQL veritabanı sunucusu için bir güvenlik duvarını yapılandırmanız gerekir. Bir Azure SQL veritabanı oluşturma ve Güvenlik Duvarı'nı yapılandırma hakkında yönergeler için bkz: [Azure SQL veritabanını kullanmaya başlama][sqldatabase-get-started]. 
  
  > [!NOTE]
  > Varsayılan olarak Azure SQL veritabanını Azure Hdınsight gibi Azure hizmetlerden bağlantılara izin verir. Bu güvenlik duvarı ayarı devre dışıysa, Azure portalından etkinleştirmeniz gerekir. Bir Azure SQL veritabanı oluşturma ve güvenlik duvarı kuralları yapılandırma hakkında daha fazla yönerge için bkz: [oluşturma ve yapılandırma SQL veritabanı][sqldatabase-create-configue].
  > 
  > 
* **SQL Server**: Hdınsight kümenize Azure SQL sunucusu olarak aynı sanal ağda ise, adımları bu makalede SQL Server veritabanına veri vermek ve almak için kullanabilirsiniz.
  
  > [!NOTE]
  > Hdınsight yalnızca konum tabanlı sanal ağları destekler ve benzeşim grubu tabanlı sanal ağlar ile şu anda çalışmıyor.
  > 
  > 
  
  * Oluşturma ve bir sanal ağ yapılandırma için bkz: [Azure portalını kullanarak bir sanal ağ oluşturma](../../virtual-network/quick-create-portal.md).
    
    * SQL Server, veri merkezinizde kullanırken, sanal ağ olarak yapılandırmalısınız *siteden siteye* veya *noktadan siteye*.
      
      > [!NOTE]
      > İçin **noktası site** sanal ağlar, SQL Server çalıştıran kullanılabilir yapılandırma uygulama VPN istemcisi gerekir **Pano** Azure sanal ağı yapılandırmanızın.
      > 
      > 
    * Bir Azure sanal makinede SQL Server kullanırken SQL Server'ı barındıran sanal makine Hdınsight aynı sanal ağda bir üyesi ise herhangi bir sanal ağ yapılandırması kullanılabilir.
  * Hdınsight kümesi üzerinde bir sanal ağ oluşturmak için bkz: [özel seçenekleri kullanarak Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]
    > SQL Server kimlik doğrulaması da izin vermelidir. Bu makaledeki adımları tamamlamak için bir SQL Server oturumu kullanmanız gerekir.
    > 
    > 

**Yapılandırmayı doğrulamak için**

1. Kaynak grubu, Azure portalında açın. Dört kaynak grubunda göreceksiniz:

    - Küme
    - Veritabanı sunucusu
    - Veritabanı
    - varsayılan depolama hesabı

2. Veritabanını Microsoft SQL Server Management Studio'da açın.  Dağıtılan iki veritabanı göreceksiniz:

    ![Azure Hdınsight Sqoop SQL Management Studio](./media/hdinsight-use-sqoop/hdinsight-sqoop-sql-management-studio.png)


## <a name="run-sqoop-jobs"></a>Sqoop işleri çalıştırma
Hdınsight Sqoop işleri çeşitli yöntemler kullanarak çalıştırabilirsiniz. Hangi yöntemi sizin için uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın, sonra bir anlatım için bağlantıyı izleyin.

| **Bu** isterseniz... | ...an **etkileşimli** Kabuk | ...**toplu** işleme | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |✔ |✔ |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |✔ |Linux veya Windows |Windows (için şimdi) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |✔ |Linux veya Windows |Windows |

## <a name="limitations"></a>Sınırlamalar
* Toplu export - ile Linux tabanlı Hdınsight, Microsoft SQL Server veya Azure SQL veritabanı için verileri dışa aktarmak için kullanılan Sqoop bağlayıcı toplu eklemeler şu anda desteklemiyor.
* -Kullanırken, Linux tabanlı Hdınsight ile toplu işleme `-batch` eklemeleri gerçekleştirirken geçiş, Sqoop gerçekleştirir INSERT işlemlerine toplu işleme yerine birden çok ekler.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi Sqoop kullanma öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight ile Hive kullanma](../hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](../hdinsight-use-pig.md)
* [Verileri Hdınsight'a yükleme][hdinsight-upload-data]: Hdınsight/Azure Blob depolama alanına veri yüklemek için diğer yöntemler bulun.

## <a name="appendix-a---a-powershell-sample"></a>Ek A - PowerShell örnek
PowerShell örnek aşağıdaki adımları gerçekleştirir:

1. Azure'a bağlayın.
2. Bir Azure kaynak grubu oluşturun. Daha fazla bilgi için bkz: [Azure PowerShell'i Azure Resource Manager ile kullanma](../../azure-resource-manager/powershell-azure-resource-manager.md)
3. Bir Azure SQL veritabanı sunucusu, Azure SQL veritabanına ve iki tablo oluşturun. 
   
    Bunun yerine SQL Server kullanıyorsanız, tablolar oluşturmak için aşağıdaki ifadeleri kullanın:
   
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
   
    Veritabanı ve tablolar incelemek için en kolay yolu, Visual Studio kullanmaktır. Veritabanı sunucusu ve veritabanı Azure portalını kullanarak incelenebilir.
4. Hdınsight kümesi oluşturun.
   
    Küme incelemek için Azure portalında veya Azure PowerShell'i kullanabilirsiniz.
5. Kaynak veri dosyası önceden işleyebilir.
   
    Bu öğreticide, bir Azure SQL veritabanına log4j günlük dosyası (ayrılmış bir dosya) ve bir Hive tablosu verin. Ayrılmış dosya adında */example/data/sample.log*. Öğreticide daha önce log4j günlükler bazı örnekler gördünüz. Günlük dosyasında bazı boş satırlar ve vardır bazı satırlar bunlara benzer:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Bu verileri kullanan diğer örnekler için ince budur ancak biz Azure SQL veritabanı ya da SQL Server içeri aktarmadan önce bu özel durumlar kaldırmanız gerekir. Sqoop verme başarısız boş bir dize veya daha az sayıda olan bir satır varsa Azure SQL veritabanı tablosunda tanımlanan alanları sayısından öğesi. Log4jlogs tablosunda yedi dize türünde alanlar içeriyor.
   
    Bu yordam küme üzerinde yeni bir dosya oluşturur: tutorials/usesqoop/data/sample.log. Değiştirilen veri dosyasını incelemek için Azure portalı, Azure Storage Gezgini aracını veya Azure PowerShell kullanabilirsiniz. [Hdınsight kullanmaya başlama] [ hdinsight-get-started] dosya indirme ve dosya içeriği görüntülemek için Azure PowerShell kullanarak bir kod örneği vardır.
6. Bir veri dosyası Azure SQL veritabanına verin.
   
    Kaynak dosyası tutorials/usesqoop/data/sample.log değil. Veriler için dışa tablo log4jlogs adı verilir.
   
   > [!NOTE]
   > Bağlantı dizesi bilgilerini dışında SQL Server veya bir Azure SQL veritabanı için bu bölümdeki adımları çalışması gerekir. Bu adımları aşağıdaki yapılandırmayı kullanarak test edilmiş:
   > 
   > * **Azure sanal ağı noktadan siteye Yapılandırması**: bir sanal ağ özel bir veri merkezinde bir SQL Server Hdınsight kümesi bağlanır. Bkz: [Yönetim Portalı'nda bir noktadan siteye VPN yapılandırma](../../vpn-gateway/vpn-gateway-point-to-site-create.md) daha fazla bilgi için.
   > * **Azure Hdınsight**: bkz [özel seçenekleri kullanarak Hdınsight'ta oluşturmak Hadoop kümeleri](../hdinsight-hadoop-provision-linux-clusters.md) bir küme üzerinde bir sanal ağ oluşturma hakkında daha fazla bilgi için.
   > * **SQL Server 2014**: kimlik doğrulaması ve güvenli bir şekilde sanal ağa bağlanmak için bir yapılandırma paketi VPN istemcisi çalıştıran izin verecek şekilde yapılandırılmış.
   > 
   > 
7. Bir Hive tablosu Azure SQL veritabanına verin.
8. Mobiledata tablo Hdınsight kümesine içeri aktarın.
   
    Değiştirilen veri dosyasını incelemek için Azure portalı, Azure Storage Gezgini aracını veya Azure PowerShell kullanabilirsiniz.  [Hdınsight kullanmaya başlama] [ hdinsight-get-started] dosya indirme ve dosya içeriği görüntülemek için Azure PowerShell'i kullanma hakkında bir kod örneği vardır.

### <a name="the-powershell-sample"></a>PowerShell örnek

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
$ipAddressRestService = "http://bot.whatismyipaddress.com"
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
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

#region - Create Azure resouce group
Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
try{
    Get-AzureRmResourceGroup -Name $resourceGroupName
}
catch{
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
}
#endregion

#region - Create Azure SQL database server
Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
try{
    Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
catch{
    Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

    $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

    $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                -ResourceGroupName $resourceGroupName `
                                -ServerName $sqlDatabaseServerName `
                                -SqlAdministratorCredentials $credential `
                                -Location $location).ServerName
    Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

    Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
    $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
    New-AzureRmSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-workstation" `
        -StartIpAddress $workstationIPAddress `
        -EndIpAddress $workstationIPAddress

    #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
    #Note that this allows Azure traffic from any Azure subscription to access the server.
    New-AzureRmSqlServerFirewallRule `
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
    Get-AzureRmSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName
}
catch {
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    New-AzureRMSqlDatabase `
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
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_LRS

# Create the default Blob container
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey 
New-AzureStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageAccountContext 

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

New-AzureRmHDInsightCluster `
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
Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
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
$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose
Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

#endregion

#region - export a Hive table

$tableName_mobile = "mobiledata"
$exportDir_mobile = "/hive/warehouse/hivesampletable"

$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
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

$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
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
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
