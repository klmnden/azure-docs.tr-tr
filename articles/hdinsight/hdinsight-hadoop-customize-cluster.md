---
title: HDInsight betik eylemleri - Azure'ı kullanarak kümeleri özelleştirme
description: HDInsight kümelerini betik eylemi kullanarak özelleştirme öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/05/2016
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 0f49284782ab5ab17476f37ae5ae40a753ee107b
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597424"
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Windows tabanlı HDInsight kümelerini betik eylemi kullanarak özelleştirme
**Betik eylemi** çağırmak için kullanılan [özel betikler](hdinsight-hadoop-script-actions.md) bir kümede ek yazılım yüklemek için küme oluşturma işlemi sırasında.

Bu makaledeki bilgiler, Windows tabanlı HDInsight kümelerine özeldir. Linux tabanlı kümeler için bkz: [özelleştirme Linux tabanlı HDInsight kümelerini betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

HDInsight kümeleri başka yöntemlerle de çeşitli özelleştirilebilir, ek Azure depolama hesapları dahil olmak üzere gibi Hadoop değiştirme yapılandırma dosyaları (core-site.xml, hive-site.xml vb.) veya paylaşılan kitaplıklar (örneğin, Hive, Oozie) uygulamasına ekleme Ortak konumu kümedeki. Bu özelleştirmeler, Azure PowerShell, Azure HDInsight .NET SDK veya Azure portalı yapılabilir. Daha fazla bilgi için [Hadoop kümeleri oluşturma HDInsight][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Küme oluşturma işlemi betik eylemi
Betik eylemi, yalnızca bir küme oluşturulurken olmakla birlikte kullanılır. Betik eylemi oluşturma işlemi sırasında yürütüldüğünde, aşağıdaki diyagramda gösterilmektedir:

![HDInsight kümesi özelleştirme ve aşamaları sırasında küme oluşturma][img-hdi-cluster-states]

Komut dosyası çalıştırılırken, kümeye girdiğinde **ClusterCustomization** aşaması. Bu aşamada, betik tüm belirtilen kümedeki düğümler üzerinde paralel sistem yönetici hesabı altında çalışır ve düğümler üzerinde tam yönetici ayrıcalıkları sağlar.

> [!NOTE]
> Sırasında küme düğümleri üzerinde yönetici ayrıcalıklarına sahip olduğundan **ClusterCustomization** aşama, durdurma ve başlatma Hadoop ile ilgili hizmetler gibi hizmetler gibi işlemler gerçekleştirmek için komut dosyasını kullanabilirsiniz. Bu nedenle, betiğinin bir parçası olarak çalışan betik tamamlanmadan önce Ambari Hizmetleri ve diğer Hadoop ile ilgili hizmetler ve çalışıyor olduğundan emin emin olmanız gerekir. Bu hizmetler, oluşturulurken kümesinin durumunu ve sistem durumu başarıyla onaylaması gerekir. Bu hizmetler etkiler kümedeki herhangi bir yapılandırma değiştirirseniz, sağlanan yardımcı işlevleri kullanmanız gerekir. Yardımcı işlevleri hakkında daha fazla bilgi için bkz: [HDInsight için betik eylemi geliştirme betikleri][hdinsight-write-script].
>
>

Çıktı ve komut dosyası için hata günlüklerini, küme için belirtilen varsayılan depolama hesabında depolanır. Günlükler, adı olan bir tabloda depolanır **u < \cluster-name-fragment >< \time-stamp > setuplog**. Bu kümedeki düğümleri (baş düğüm ve çalışan düğümleri) üzerinde çalışan komut dosyasından toplama günlüklerdir.

Her küme içinde belirtilen sırayla çağrılır birden çok betik eylemleri kabul edebilir. Betik olması çalıştırdınız baş düğüm, çalışan düğümlerinin veya her ikisi de.

HDInsight, HDInsight kümelerinde aşağıdaki bileşenleri yüklemek için birkaç komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| **Spark'ı yükleme** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`. Bkz: [yükleme ve kullanma, HDInsight üzerinde Spark kümeleri][hdinsight-install-spark]. |
| **R yükleme** | `https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1`. Bkz: [yükleme ve HDInsight kümelerinde R kullanma](r-server/r-server-hdinsight-manage.md#install-additional-r-packages-on-the-cluster). |
| **Solr yükleme** | `https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1`. Bkz: [HDInsight üzerinde Solr yükleme ve kullanma kümeleri](hdinsight-hadoop-solr-install.md). |
| **Giraph yükleme** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`. Bkz: [HDInsight üzerinde Giraph yükleme ve kullanma kümeleri](hdinsight-hadoop-giraph-install.md). |
| **Hive kitaplıklarını önceden yükleme** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`. Bkz: [HDInsight kümelerinde ekleme Hive kitaplıkları](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Azure portalını kullanarak komut dosyalarını çağırma
**Azure portalından**

1. Anlatıldığı gibi bir küme oluşturmaya başlayın [Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
2. İsteğe bağlı yapılandırma altında için **betik eylemleri** dikey penceresinde tıklayın **betik eylemi ekleme** aşağıda gösterildiği gibi bir betik eylemi ayrıntılarını sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanmanız](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "küme özelleştirmek için betik eylemini kullanın")

    <table border='1'>
        <tr><th>Özellik</th><th>Değer</th></tr>
        <tr><td>Ad</td>
            <td>Betik eylemi için bir ad belirtin.</td></tr>
        <tr><td>Betiği URI'si</td>
            <td>Küme özelleştirmek için çağrılan betik URI'si belirtin. s</td></tr>
        <tr><td>HEAD/çalışan</td>
            <td>Düğüm belirtin (**Head** veya **çalışan**) özelleştirme betik çalıştığı şirket.</b>.
        <tr><td>Parametreler</td>
            <td>Komut dosyası tarafından gerekli parametreleri belirtin.</td></tr>
    </table>

    Küme üzerinde birden çok bileşenleri yüklemek için birden fazla betik eylemi eklemek için ENTER tuşuna basın.
3. Tıklayın **seçin** betik eylemi yapılandırmasını kaydetmek ve küme oluşturma işlemine devam etmek için.

## <a name="call-scripts-using-azure-powershell"></a>Azure PowerShell kullanarak komut dosyalarını çağırma
Bu aşağıdaki PowerShell Betiği, Windows tabanlı HDInsight kümesi üzerinde Spark'ı yüklemek gösterilmektedir.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Connect-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect to Azure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Connect-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare the dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Diğer yazılım yüklemek için betik komut dosyasında değiştirilecek gerekir:

İstendiğinde, küme için kimlik bilgilerini girin. Uygulamanın, küme oluşturulmadan önce birkaç dakika sürebilir.

## <a name="call-scripts-using-net-sdk"></a>.NET SDK kullanarak komut dosyalarını çağırma
Aşağıdaki örnek, Windows tabanlı HDInsight kümesi üzerinde Spark'ı yüklemek gösterilmektedir. Diğer yazılım yüklemek için kod içindeki betik dosyasının yerini gerekecektir.

**Spark ile bir HDInsight kümesi oluşturma**

1. Visual Studio'da C# konsol uygulaması oluşturun.
2. Nuget Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. Aşağıdaki using deyimlerini Program.cs dosyasında:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Aşağıdakilerle sınıftaki kod yerleştirin:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
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
5. Uygulamayı çalıştırmak için **F5**'e basın.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği
Microsoft Azure HDInsight, Hadoop geçici olarak oluşturulmuş açık kaynak teknolojilerinin bir ekosistemi kullanarak buluttaki büyük veri uygulamaları oluşturmanıza olanak sağlayan esnek bir platform hizmetidir. Microsoft Azure içinde açıklandığı gibi bu genel bir seviyede destek açık kaynak teknolojileri için sağlar **destek kapsamı** bölümünü <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure desteği SSS Web sitesine</a>. HDInsight hizmeti, aşağıda açıklandığı gibi bir ek düzeyi bazı bileşenleri için destek sağlar.

HDInsight hizmetinde kullanılabilir açık kaynak bileşenleri iki tür vardır:

* **Yerleşik bileşenlerini** -bu bileşenler HDInsight kümelerinde önceden yüklü olan ve kümeyi temel işlevlerini sağlar. Örneğin, YARN ResourceManager, Hive sorgu dili (HiveQL) ve Mahout kitaplığı, bu kategoriye aittir. Tam küme bileşenleri listesini kullanılabilir [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](hdinsight-component-versioning.md) </a>.
* **Özel bileşenler** -, kümenin bir kullanıcı olarak yükleyebilir veya herhangi bir bileşeni Topluluğu'nda kullanılabilir veya sizin tarafınızdan oluşturulan iş yükünüzü kullanın.

Yerleşik bileşenleri tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.

> [!WARNING]
> HDInsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenler için ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Bu sorunu çözümlemek ya da bu teknoloji için derin bir uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları etkileşim kurmak isteyen neden olabilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

HDInsight hizmeti, özel bileşenler kullanmak için birkaç yol sağlar. Nasıl bir bileşen ya da kümeye yüklü bağımsız olarak, aynı düzeyde desteği geçerlidir. Özel bileşenler HDInsight kümelerinde kullanılabilir en yaygın yolu bir listesi aşağıdadır:

1. İş gönderme - Hadoop ya da diğer türleri yürütün veya özel bileşenler kullanan işlerinin kümeye gönderilebilir.
2. Küme özelleştirmesi - küme oluşturma sırasında ek ayarlar ve küme düğümlerinde yüklü özel bileşenler belirtebilirsiniz.
3. Örnekleri - popüler özel bileşenler, Microsoft ve diğerleri için HDInsight kümelerinde bu bileşenlerin nasıl kullanılabileceğini örneklerin sağlayabilir. Bu örnekler desteği olmadan sağlanır.

## <a name="develop-script-action-scripts"></a>Betik eylemi betikleri geliştirme
Bkz: [HDInsight için betik eylemi geliştirme betikleri][hdinsight-write-script].

## <a name="see-also"></a>Ayrıca bkz.
* [HDInsight Hadoop kümeleri oluşturma] [ hdinsight-provision-cluster] diğer özel seçenekleri kullanarak bir HDInsight kümesi oluşturma hakkında yönergeler açıklanmaktadır.
* [HDInsight için betik eylemi betikleri geliştirme][hdinsight-write-script]
* [Yükleme ve HDInsight kümelerine Spark kullanma][hdinsight-install-spark]
* [Yükleme ve HDInsight kümelerinde Solr kullanma](hdinsight-hadoop-solr-install.md).
* [Yükleme ve HDInsight kümelerinde Giraph kullanma](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Küme oluşturma sırasında aşamaları"
