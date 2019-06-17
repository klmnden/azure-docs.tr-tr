---
title: HDInsight - Azure .NET SDK'sı ile Apache Hadoop kümelerini yönetme
description: HDInsight .NET SDK kullanarak HDInsight, Apache Hadoop kümeleri için yönetim görevlerini gerçekleştirmeyi öğreneceksiniz.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 06e3178e344ee46f67cfd8a6feaf08d56d3c86e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64724137"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>.NET SDK kullanarak HDInsight Apache Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Kullanarak HDInsight kümelerini nasıl yöneteceğinizi öğrenin [HDInsight.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight).

**Önkoşullar**

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-to-azure-hdinsight"></a>Azure HDInsight için Bağlan

Aşağıdaki NuGet paketlerini ihtiyacınız vardır:

```powershell
Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
Install-Package Microsoft.Azure.Management.ResourceManager -Pre
Install-Package Microsoft.Azure.Management.HDInsight
```

Aşağıdaki kod örneği, Azure aboneliğiniz kapsamındaki HDInsight kümeleri yönetebilmeniz için önce Azure'a bağlanma işlemini göstermektedir.

```csharp
using System;
using Microsoft.Azure;
using Microsoft.Azure.Management.HDInsight;
using Microsoft.Azure.Management.HDInsight.Models;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace HDInsightManagement
{
    class Program
    {
        private static HDInsightManagementClient _hdiManagementClient;
        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

        static void Main(string[] args)
        {
            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            // insert code here

            System.Console.WriteLine("Press ENTER to continue");
            System.Console.ReadLine();
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
    }
}
```

Bu programı çalıştırdığınızda bir istem göreceksiniz.  İstemi görmek istemiyorsanız, bkz. [etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluşturma](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

## <a name="create-clusters"></a>Küme oluşturma

Bkz: [.NET SDK kullanarak HDInsight oluşturma Linux tabanlı kümeler](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

## <a name="list-clusters"></a>Kümeleri listeleme

Aşağıdaki kod parçacığı, kümeler ve bazı özellikler listelenmektedir:

```csharp
var results = _hdiManagementClient.Clusters.List();
foreach (var name in results.Clusters) {
    Console.WriteLine("Cluster Name: " + name.Name);
    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
    Console.WriteLine("\t Cluster location: " + name.Location);
    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
}
```

## <a name="delete-clusters"></a>Kümeleri Sil

Aşağıdaki kod parçacığı, eşzamanlı veya zaman uyumsuz olarak bir kümeyi silmek için kullanın: 

```csharp
_hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
_hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
```

## <a name="scale-clusters"></a>Kümeleri ölçeklendirme

Özellik ölçeklendirme kümesi Azure HDInsight kümesini yeniden oluşturmak zorunda kalmadan çalışan bir küme tarafından kullanılan çalışan düğümlerinin sayısını değiştirmenize izin verir.

> [!NOTE]  
> Yalnızca, HDInsight sürüm 3.1.3 ile kümeleri veya üzeri desteklenir. Kümenizin sürümü hakkında şüpheleriniz varsa, Özellikler sayfasını kontrol edebilirsiniz.  Bkz: [kümeleri Listele ve Göster](hdinsight-administer-use-portal-linux.md#showClusters).

HDInsight tarafından desteklenen küme her tür veri düğümü sayısı değiştirmenin etkisi:

* Apache Hadoop
  
    Sorunsuz bir şekilde, bekleyen veya çalışan tüm işleri etkilemeden çalışan bir Hadoop kümesinde çalışan düğümleri sayısını artırabilirsiniz. İşlem devam ederken yeni işleri da gönderilebilir. Böylece küme her zaman işlevsel bir durumda bırakılır bir ölçeklendirme işlemi hataları düzgün bir şekilde ele alınır.
  
    Bir Hadoop kümesini veri düğümü sayısını azaltarak ölçeklendiğinde, kümedeki hizmetlerinden bazılarını yeniden başlatılır. Bu tüm çalışan ve farklı bekleyen işleri ölçeklendirme işleminin tamamlanması sırasında başarısız olmasına neden olur. İşlemi tamamlandıktan sonra ancak, işleri yeniden oluşturabilirsiniz.
* Apache HBase
  
    Sorunsuz bir şekilde ekleyebilir veya çalışırken düğümleri HBase kümenize kaldırın. Bölge sunucuları ölçeklendirme işlemi tamamladıktan birkaç dakika içinde otomatik olarak dengelenir. Ancak, küme baş düğümüne günlüğe kaydetme ve bir komut istemi penceresinden aşağıdaki komutları çalıştırmadan tarafından el ile bölgesel sunucuları dengeleyebilirsiniz:
  

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Apache Storm
  
    Sorunsuz bir şekilde ekleyebilir veya çalışırken Storm kümenize veri düğümleri kaldırma. Ancak, ölçeklendirme işlemi başarıyla tamamlandıktan sonra topoloji yeniden dengelemeniz gerekir.
  
    Yeniden Dengeleme iki şekilde gerçekleştirilebilir:
  
  * Storm web kullanıcı Arabirimi
  * Komut satırı arabirimi (CLI) aracı
    
    Lütfen [Apache Storm belgeleri](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) daha fazla ayrıntı için.
    
    HDInsight kümesinde Storm web kullanıcı Arabirimi kullanılabilir:
    
    ![HDInsight Storm ölçek yeniden Dengeleme](./media/hdinsight-administer-use-powershell/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Storm topolojiyi yeniden dengelemek için CLI komutunu kullanmak nasıl bir örnek aşağıdadır:
    

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Aşağıdaki kod parçacığı, eşzamanlı veya zaman uyumsuz olarak bir küme yeniden boyutlandırma işlemi gösterilmektedir:

```csharp
_hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
_hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
```

## <a name="grantrevoke-access"></a>GRANT/revoke-access

HDInsight kümeleri aşağıdaki HTTP web Hizmetleri (Bu hizmetlerin tümü, RESTful uç noktalarına sahip) sahip:

* ODBC
* JDBC
* Apache Ambari
* Apache Oozie
* Apache templeton da

Varsayılan olarak, bu hizmetler için erişim verilir. İptal etme / erişim izni. İptal etmek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = false,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

Vermek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = enable,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

> [!NOTE]  
> Verme/erişimini iptal ederek, küme kullanıcı adını ve parolasını sıfırlar.

Bu, Portal üzerinden de yapılabilir. Bkz: [yönetme Apache Hadoop, Azure portalını kullanarak HDInsight kümeleri](hdinsight-administer-use-portal-linux.md).

## <a name="update-http-user-credentials"></a>HTTP kullanıcısı kimlik bilgilerini güncelleştirme

Aynı Grant/revoke HTTP erişim yordam var.  Kümenin HTTP erişim verilmişse, öncelikle iptal gerekir.  ' İ tıklatın ve ardından yeni HTTP kullanıcı kimlik bilgileriyle erişim verin.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabı bulunamadı

Aşağıdaki kod parçacığı, varsayılan depolama hesabı adı ve bir küme için varsayılan depolama hesabı anahtarını almak nasıl gösterir.

```csharp
var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
foreach (var key in results.Configuration.Keys)
{
    Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
}
```

## <a name="submit-jobs"></a>İş gönderme

**MapReduce işleri göndermek için**

Bkz: [HDInsight çalıştırma MapReduce örnekleri](hadoop/apache-hadoop-run-samples-linux.md).

**Apache Hive işleri göndermek için** 

Bkz: [.NET SDK kullanarak Apache Hive sorgularını çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md).

**Apache Pig işleri göndermek için**

Bkz: [.NET SDK'sını kullanarak çalıştırma Apache Pig işleri](hadoop/apache-hadoop-use-pig-dotnet-sdk.md).

**Apache Sqoop işleri göndermek için**

Bkz: [HDInsight ile Apache Sqoop'u kullanma](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md).

**Apache Oozie iş göndermek için**

Bkz: [tanımlamak ve HDInsight içinde bir iş akışı çalıştırmak için Hadoop ile Apache Oozie kullanma](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme

Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.

* [HDInsight .NET SDK başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Azure portalını kullanarak HDInsight Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Bir komut satırı arabirimi ile HDInsight'ı yönetme][hdinsight-admin-cli]
* [HDInsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
