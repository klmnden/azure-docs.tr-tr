---
title: .NET - Azure HDInsight'ı kullanarak Apache Hadoop kümeleri oluşturma
description: Linux'ta HDInsight .NET SDK kullanarak HDInsight için Apache Hadoop, Apache HBase, Apache Storm veya Apache Spark küme oluşturma konusunda bilgi edinin.
author: mamccrea
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: mamccrea
ms.openlocfilehash: 42c3f0ce70467c6a90bcc4e2e6b8a21d7570edc4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606199"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-net-sdk"></a>Linux tabanlı kümeler .NET SDK kullanarak HDInsight oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]


Oluşturmayı bir [Apache Hadoop](https://hadoop.apache.org/) .NET SDK kullanarak Azure HDInsight kümesinde Küme.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, bir çalışan düğümü ile bir küme oluşturun. 32'den fazla çalışan düğümleri, küme oluşturma sırasında veya Küme oluşturulduktan sonra ölçeklendirme planlıyorsanız, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB ram ile seçmeniz gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure depolama hesabınız**. Bkz: [depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).
* **Visual Studio**.

## <a name="create-clusters"></a>Küme oluşturma

1. Visual Studio'yu açın.
2. Yeni bir Visual C# konsol uygulaması oluşturun.
3. Gelen **Araçları** menüsünü tıklatın **NuGet Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.
4. Konsolunda paketleri yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight
    ```

    Bu komutlar, geçerli Visual Studio projesine .NET kitaplıkları ve bunları başvurular ekleyin.
5. Çözüm Gezgini'nden çift **Program.cs** açmak için aşağıdaki kodu yapıştırın ve değişkenleri değerleri sağlayın:

    ```csharp
    using System;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    namespace CreateHDInsightCluster
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;

            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            private const string ExistingResourceGroupName = "<Enter Resource Group Name>";
            private const string ExistingStorageName = "<Enter Default Storage Account Name>.blob.core.windows.net";
            private const string ExistingStorageKey = "<Enter Default Storage Account Key>";
            private const string ExistingBlobContainer = "<Enter Default Bob Container Name>";

            private const string NewClusterName = "<Enter HDInsight Cluster Name>";
            private const int NewClusterNumNodes = 2;
            private const string NewClusterLocation = "EAST US 2";     // Must be the same as the default Storage account
            private const OSType NewClusterOSType = OSType.Linux;
            private const string NewClusterType = "Hadoop";
            private const string NewClusterVersion = "3.6";
            private const string NewClusterUsername = "admin";
            private const string NewClusterPassword = "<Enter HTTP User Password>";
            private const string NewClusterSshUserName = "sshuser";

            // You can use eitehr password or public key. See https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix
            private const string NewClusterSshPassword = "<Enter SSH User Password>";
            private const string NewClusterSshPublicKey = @"---- BEGIN SSH2 PUBLIC KEY ----
                Comment: ""rsa-key-20150731""
                AAAAB3NzaC1yc2EAAAABJQAAAQEA4QiCRLqT7fnmUA5OhYWZNlZo6lLaY1c+IRsp
                gmPCsJVGQLu6O1wqcxRqiKk7keYq8bP5s30v6bIljsLZYTnyReNUa5LtFw7eauGr
                yVt3Pve6ejfWELhbVpi0iq8uJNFA9VvRkz8IP1JmjC5jsdnJhzQZtgkIrdn3w0e6
                WVfu15kKyY8YAiynVbdV51EB0SZaSLdMZkZQ81xi4DDtCZD7qvdtWEFwLa+EHdkd
                pzO36Mtev5XvseLQqzXzZ6aVBdlXoppGHXkoGHAMNOtEWRXpAUtEccjpATsaZhQR
                zZdZlzHduhM10ofS4YOYBADt9JohporbQVHM5w6qUhIgyiPo7w==
                ---- END SSH2 PUBLIC KEY ----"; //replace the public key with your own

            static void Main(string[] args)
            {
                System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

                // Authenticate and get a token
                var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // Set parameters for the new cluster
                var parameters = new ClusterCreateParameters
                {
                    ClusterSizeInNodes = NewClusterNumNodes,
                    UserName = NewClusterUsername,
                    ClusterType = NewClusterType,
                    OSType = NewClusterOSType,
                    Version = NewClusterVersion,

                    // Use an Azure storage account as the default storage
                    DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingBlobContainer),

                    // Is the cluster type RServer? If so, you can set the EdgeNodeSize.
                    // Otherwise, the default VM size is used.
                    //EdgeNodeSize = "Standard_D12_v2",

                    Password = NewClusterPassword,
                    Location = NewClusterLocation,

                    SshUserName = NewClusterSshUserName,
                    SshPassword = NewClusterSshPassword,
                    //SshPublicKey = NewClusterSshPublicKey
                };

                // Is the cluster type RServer? If so, add the RStudio configuration option.
                /*
                parameters.Configurations.Add(
                    "rserver",
                    new Dictionary<string, string>()
                    {
                        { "rstudio", "true" }
                    }
                );
                */

                // Create the cluster
                _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

                System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            /// <summary>
            /// Authenticate to an Azure subscription and retrieve an authentication token
            /// </summary>
            static TokenCloudCredentials GetTokenCloudCredentials(string TenantId, string ClientId, string SubscriptionId)
            {
                var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
                var tokenAuthResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
                    ClientId, 
                    new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                    new PlatformParameters(PromptBehavior.Always), 
                    UserIdentifier.AnyUser);
                return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.Result.AccessToken);
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

6. Sınıf üyesi değerlerini değiştirin.
7. Uygulamayı çalıştırmak için **F5**'e basın. Bir konsol penceresi, açın ve uygulama durumunu görüntüler. Azure hesabı kimlik bilgilerinizi girmeniz istenir. Uygulamanın, bir HDInsight kümesi, genellikle yaklaşık 15 oluşturulması birkaç dakika sürebilir.

## <a name="use-bootstrap"></a>Kullanım önyükleme

Bootstrap ile küme oluşturma sırasında toplama ayarlarını yapılandırabilirsiniz.  Daha fazla bilgi için [özelleştirme HDInsight kümeleri Bootstrap ile](hdinsight-hadoop-customize-cluster-bootstrap.md).

Aşağıdaki örnekte değiştirme [küme oluşturma](#create-clusters) bir Apache Hive ayarını yapılandırmak için:

```csharp
static void Main(string[] args)
{
    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

    // Authenticate and get a token
    var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
    // Flag subscription for HDInsight, if it isn't already.
    EnableHDInsight(authToken);
    // Get an HDInsight management client
    _hdiManagementClient = new HDInsightManagementClient(authToken);

    // Set parameters for the new cluster
    var extendedParameters = new ClusterCreateParametersExtended
    {
        Location = NewClusterLocation,
        Properties = new ClusterCreateProperties
        {
            ClusterDefinition = new ClusterDefinition
            {
                ClusterType = NewClusterType.ToString()
            },
            ClusterVersion = NewClusterVersion,
            OperatingSystemType = NewClusterOSType
        }
    };

    var coreConfigs = new Dictionary<string, string>
    {
        {"fs.defaultFS", string.Format("wasb://{0}@{1}", ExistingBlobContainer, ExistingStorageName)},
        {
            string.Format("fs.azure.account.key.{0}", ExistingStorageName),
            ExistingStorageKey
        }
    };

    // bootstrap
    var hiveConfigs = new Dictionary<string, string>
    {
        { "hive.metastore.client.socket.timeout", "90"}
    };

    var gatewayConfigs = new Dictionary<string, string>
    {
        {"restAuthCredential.isEnabled", "true"},
        {"restAuthCredential.username", NewClusterUsername},
        {"restAuthCredential.password", NewClusterPassword}
    };

    var configurations = new Dictionary<string, Dictionary<string, string>>
    {
        {"core-site", coreConfigs},
        {"gateway", gatewayConfigs},
        {"hive-site", hiveConfigs}
    };

    var serializedConfig = JsonConvert.SerializeObject(configurations);
    extendedParameters.Properties.ClusterDefinition.Configurations = serializedConfig;

    var sshPublicKeys = new List<SshPublicKey>();
    var sshPublicKey = new SshPublicKey
    {
        CertificateData =
            string.Format("ssh-rsa {0}", NewClusterSshPublicKey)
    };
    sshPublicKeys.Add(sshPublicKey);

    var headNode = new Role
    {
        Name = "headnode",
        TargetInstanceCount = 2,
        HardwareProfile = new HardwareProfile
        {
            VmSize = "Large"
        },
        OsProfile = new OsProfile
        {
            LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
            {
                UserName = NewClusterSshUserName,
                Password = NewClusterSshPassword //,
                // When use a SSH pulbic key, make sure to remove comments, headers and trailers, and concatenate the key into one line 
                //SshProfile = new SshProfile
                //{
                //    SshPublicKeys = sshPublicKeys
                //}
            }
        }
    };

    var workerNode = new Role
    {
        Name = "workernode",
        TargetInstanceCount = NewClusterNumNodes,
        HardwareProfile = new HardwareProfile
        {
            VmSize = "Large"
        },
        OsProfile = new OsProfile
        {
            LinuxOperatingSystemProfile = new LinuxOperatingSystemProfile
            {
                UserName = NewClusterSshUserName,
                Password = NewClusterSshPassword //,
                //SshProfile = new SshProfile
                //{
                //    SshPublicKeys = sshPublicKeys
                //}
            }
        }
    };

    extendedParameters.Properties.ComputeProfile = new ComputeProfile();
    extendedParameters.Properties.ComputeProfile.Roles.Add(headNode);
    extendedParameters.Properties.ComputeProfile.Roles.Add(workerNode);

    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, extendedParameters);

    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
    System.Console.ReadLine();
}
```

## <a name="use-script-action"></a>Betik eylemi kullanın

Betik eylemi kullanarak küme oluşturma sırasında ek ayarlar yapılandırabilirsiniz.  Daha fazla bilgi için [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

Aşağıdaki örnekte değiştirme [küme oluşturma](#create-clusters) R: yüklemek için betik eylemi çağırmak için

```csharp
static void Main(string[] args)
{
    System.Console.WriteLine("Creating a cluster.  The process takes 10 to 20 minutes ...");

    // Authenticate and get a token
    var authToken = GetTokenCloudCredentials(TenantId, ClientId, SubscriptionId);
    // Flag subscription for HDInsight, if it isn't already.
    EnableHDInsight(authToken);
    // Get an HDInsight management client
    _hdiManagementClient = new HDInsightManagementClient(authToken);

    // Set parameters for the new cluster
    var parameters = new ClusterCreateParameters
    {
        ClusterSizeInNodes = NewClusterNumNodes,
        Location = NewClusterLocation,
        ClusterType = NewClusterType,
        OSType = NewClusterOSType,
        Version = NewClusterVersion,

        DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingBlobContainer),

        UserName = NewClusterUsername,
        Password = NewClusterPassword,
        SshUserName = NewClusterSshUserName,
        SshPublicKey = NewClusterSshPublicKey
    };

    ScriptAction rScriptAction = new ScriptAction("Install R",
        new Uri("https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh"), "");

    parameters.ScriptActions.Add(ClusterNodeType.HeadNode,new System.Collections.Generic.List<ScriptAction> { rScriptAction});
    parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { rScriptAction });

    _hdiManagementClient.Clusters.Create(ExistingResourceGroupName, NewClusterName, parameters);

    System.Console.WriteLine("The cluster has been created. Press ENTER to continue ...");
    System.Console.ReadLine();
}
```

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bir HDInsight kümesi başarıyla oluşturuldu, kümenizi ile çalışma hakkında bilgi almak için aşağıdakileri kullanın. 

### <a name="apache-hadoop-clusters"></a>Apache Hadoop kümelerini
* [Apache Hive, HDInsight ile kullanma](hadoop/hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri
* [HDInsight üzerinde Apache HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm kümeleri
* [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](storm/apache-storm-develop-java-topology.md)
* [HDInsight üzerinde Apache Storm, Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight üzerinde Apache Storm topolojileri dağıtma ve izleme](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark kümeleri
* [Scala kullanarak tek başına uygulama oluşturma](spark/apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak bir Apache Spark kümesinde işleri uzaktan çalıştırma](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark ile BI: BI araçları ile HDInsight Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](spark/apache-spark-use-bi-tools.md)
* [Apache Spark Machine Learning ile: Gıda denetimi sonuçlarını tahmin etmek için HDInsight içindeki Spark kullanma](spark/apache-spark-machine-learning-mllib-ipython.md)

### <a name="run-jobs"></a>İşleri çalıştırma
* [.NET SDK kullanarak HDInsight Apache Hive işlerini çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md)
* [.NET SDK kullanarak HDInsight Apache Pig işleri çalıştırma](hadoop/apache-hadoop-use-pig-dotnet-sdk.md)
* [.NET SDK kullanarak HDInsight Apache Sqoop işleri çalıştırma](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md)
* [HDInsight Apache Oozie işleri çalıştırma](hdinsight-use-oozie-linux-mac.md)

