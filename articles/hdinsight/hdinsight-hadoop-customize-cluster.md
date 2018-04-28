---
title: Betik eylemleri - Azure kullanarak Hdınsight kümelerini özelleştirme | Microsoft Docs
description: Betik eylemi kullanarak Hdınsight kümelerini özelleştirme öğrenin.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 8c67c89f00362b0fc6a510a8117ac176bb3c8b6c
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Betik eylemi kullanarak Windows tabanlı Hdınsight kümelerini özelleştirme
**Betik eylemi** çağırmak için kullanılan [özel komut dosyaları](hdinsight-hadoop-script-actions.md) bir kümede ek yazılım yüklemek için küme oluşturma işlemi sırasında.

Bu makaledeki bilgileri, Windows tabanlı Hdınsight kümelerine özeldir. Linux tabanlı kümeler için bkz: [özelleştirme Linux tabanlı Hdınsight kümeleri betik eylemi kullanarak](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Hdınsight kümeleri başka yöntemlerle de çeşitli özelleştirilebilir, ek Azure depolama hesapları dahil olmak üzere gibi Hadoop değiştirme yapılandırma dosyaları (core-site.xml, hive-site.xml, vb.) veya paylaşılan kitaplıklar (örn., Hive, Oozie) içine ekleme Küme ortak konumlarda. Bu özelleştirmeler, Azure PowerShell, Azure Hdınsight .NET SDK veya Azure portalı yapılabilir. Daha fazla bilgi için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Küme oluşturma işlemi betik eylemi
Betik eylemi, yalnızca bir küme oluşturulan aşamasında olsa da kullanılır. Betik eylemi oluşturma işlemi sırasında çalıştırıldığında aşağıdaki diyagramda gösterilmektedir:

![Hdınsight küme özelleştirme ve küme oluşturma sırasında aşamaları][img-hdi-cluster-states]

Komut dosyası çalıştırılırken küme girer **ClusterCustomization** aşaması. Bu aşamada, komut dosyası belirtilen kümedeki tüm düğümler, üzerinde paralel sistem yönetici hesabı altında çalışacak ve düğümler üzerinde tam yönetici ayrıcalıkları sağlar.

> [!NOTE]
> Sırasında küme düğümlerinde yönetici ayrıcalıklarına sahip olduğundan **ClusterCustomization** aşama, durdurma ve Hadoop ile ilgili hizmetlerin gibi hizmetler başlatma gibi işlemler gerçekleştirmek için komut dosyasını kullanabilirsiniz. Bu nedenle, komut dosyasının parçası olarak çalışan betik tamamlanmadan önce Ambari Hizmetleri ve diğer Hadoop ile ilgili hizmetlerin hazır ve çalışır olduğundan emin olmalısınız. Bu hizmetler, onu oluşturulurken kümenin durumunu ve sistem durumu başarıyla onaylaması için gereklidir. Bu hizmetler etkiler kümedeki herhangi bir yapılandırma değiştirirseniz, sağlanan yardımcı işlevleri kullanmanız gerekir. Yardımcı işlevleri hakkında daha fazla bilgi için bkz: [Hdınsight betik eylemi geliştirme betikleri][hdinsight-write-script].
>
>

Küme için belirtilen varsayılan depolama hesabı, çıkış ve komut dosyası için hata günlüklerini depolanır. Günlükler, adı olan bir tabloda depolanır **u < \cluster-name-fragment >< \time-stamp > setuplog**. Kümedeki düğümler (baş düğüm ve çalışan düğümleri) çalışan komut dosyasından toplama günlükleri şunlardır.

Her küme içinde belirtilen sırayla çağrılır birden çok betik eylemleri kabul edebilir. Bir komut dosyasını olması çalıştıran baş düğüm, çalışan düğümlerine veya her ikisi de.

Hdınsight Hdınsight kümelerinde aşağıdaki bileşenleri yüklemek için çeşitli komut dosyaları sağlar:

| Ad | Betik |
| --- | --- |
| **Spark yükleyin** |https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1. Bkz: [yükleme ve kullanma hdınsight'ta Spark kümeleri][hdinsight-install-spark]. |
| **R yükleme** |https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1. Bkz: [yükleme ve kullanma R Hdınsight kümelerinde][hdinsight-install-r]. |
| **Solr yükleyin** |https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1. Bkz: [yükleme ve kullanma Solr hdınsight kümeleri](hdinsight-hadoop-solr-install.md). |
| - **Giraph yükleyin** |https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1. Bkz: [yükleme ve kullanma Giraph hdınsight kümeleri](hdinsight-hadoop-giraph-install.md). |
| **Ön yük Hıve kitaplıkları** |https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1. Bkz: [eklemek Hive kitaplıkları Hdınsight kümeleri hakkında](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-the-azure-portal"></a>Azure portalını kullanarak komut dosyalarını arayın
**Azure portalından**

1. Konusunda açıklandığı gibi bir küme oluşturmaya başlamak [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md).
2. İsteğe bağlı yapılandırma bölümünde için **betik eylemleri** dikey penceresinde tıklatın **betik eylemi eklemek** aşağıda gösterildiği gibi betik eylemi hakkındaki ayrıntıları sağlamak için:

    ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "bir küme özelleştirmek için kullanım betik eylemi")

    <table border='1'>
        <tr><th>Özellik</th><th>Değer</th></tr>
        <tr><td>Ad</td>
            <td>Betik eylemi için bir ad belirtin.</td></tr>
        <tr><td>Betik URI'si</td>
            <td>Küme özelleştirmek için çağrılan betik URI'si belirtin. s</td></tr>
        <tr><td>HEAD/çalışan</td>
            <td>Düğüm belirtin (**Head** veya **çalışan**) özelleştirme betik çalıştığı şirket.</b>.
        <tr><td>Parametreler</td>
            <td>Komut dosyası tarafından gerekli parametreleri belirtin.</td></tr>
    </table>

    Küme üzerinde birden çok bileşeni yüklemek için birden fazla betik eylemi eklemek için ENTER tuşuna basın.
3. Tıklatın **seçin** betik eylemi yapılandırmasını kaydedin ve küme oluşturma ile devam edin.

## <a name="call-scripts-using-azure-powershell"></a>Azure PowerShell kullanarak komut dosyalarını arayın
Bu aşağıdaki PowerShell betiğini Spark Windows tabanlı Hdınsight kümesine yüklemek gösterilmiştir.  

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


Diğer yazılım yüklemek için komut dosyası komut dosyasında Değiştir gerekir:

İstendiğinde, küme için kimlik bilgilerini girin. Küme oluşturulmadan önce birkaç dakika sürebilir.

## <a name="call-scripts-using-net-sdk"></a>.NET SDK kullanarak komut dosyalarını arayın
Aşağıdaki örnek, Windows tabanlı Hdınsight kümesi üzerinde Spark yükleneceği gösterilmiştir. Diğer yazılım yüklemek için komut dosyası kodu değiştirin gerekecektir.

**İle Spark Hdınsight kümesi oluşturmak için**

1. Visual Studio'da bir C# konsol uygulaması oluşturun.
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
4. Sınıfında aşağıdaki kodu yerleştirin:

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

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Hdınsight kümelerinde kullanılan açık kaynaklı yazılım desteği
Microsoft Azure Hdınsight, büyük veri uygulamalarını bulutta Hadoop biçimlendirilmiş açık kaynak teknolojilerinin bir ekosistemi kullanarak oluşturmanıza olanak sağlayan esnek bir platform hizmetidir. Microsoft Azure içinde açıklandığı gibi bu genel düzeyde bir destek açık kaynak teknolojileri için sağlar **destek kapsamı** bölümünü <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure destek SSS Web sitesine</a>. Hdınsight hizmeti, aşağıda açıklandığı gibi bir ek düzeyi bazı bileşenleri için destek sağlar.

Hdınsight hizmetinde bulunan açık kaynak bileşenleri iki tür vardır:

* **Yerleşik bileşenlerini** -bu bileşenlerin Hdınsight kümelerinde önceden yüklü olduğundan ve küme çekirdek işlevselliğini sağlar. Örneğin, YARN ResourceManager, Hive sorgu dili (HiveQL) ve Mahout kitaplık bu kategoriye ait. Küme bileşenlerin tam bir listesi kullanılabilir [Hdınsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler nelerdir?](hdinsight-component-versioning.md) </a>.
* **Özel bileşenler** -, bir kullanıcı kümesi olarak yükleyebilir veya topluluk bulunan ya da sizin tarafınızdan oluşturulan herhangi bir bileşenin, iş yükü kullanın.

Yerleşik bileşenlerini tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenleri ilgili sorunları gidermek için yardımcı olur.

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak desteklenir ve Microsoft Support yalıtmak ve bu bileşenleri ilgili sorunları gidermek için yardımcı olur.
>
> Özel bileşenler, daha fazla sorun gidermenize yardımcı olması için ticari koşulların elverdiği oranda makul destek alırsınız. Bu sorunu çözmek veya bu teknoloji derin uzmanlık bulunduğu açık kaynak teknolojileri için kullanılabilir kanalları devreye isteyen neden olabilir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

Hdınsight hizmeti özel bileşenleri kullanmak için çeşitli yöntemler sunar. Nasıl bir bileşen kullanılan veya kümeye yüklü bağımsız olarak, aynı düzeyde desteği geçerlidir. Özel bileşenler Hdınsight kümelerinde kullanılabilir en yaygın şekilde listesi aşağıdadır:

1. İş gönderme - Hadoop veya diğer tür execute veya özel bileşenleri kullanan işi kümeye gönderilebilir.
2. Küme özelleştirme - küme oluşturma sırasında ek ayarlar ve küme düğümlerinde yüklü özel bileşenleri belirtebilirsiniz.
3. Örnekler - popüler özel bileşenleri, Microsoft ve diğerleri için bu bileşenlerin Hdınsight kümelerinde nasıl kullanılabileceğini örnek sağlayabilir. Bu örnekler desteği olmadan sağlanır.

## <a name="develop-script-action-scripts"></a>Betik eylemi betikleri geliştirme
Bkz: [Hdınsight betik eylemi geliştirme betikleri][hdinsight-write-script].

## <a name="see-also"></a>Ayrıca bkz.
* [Hdınsight'ta Hadoop kümeleri oluşturma] [ hdinsight-provision-cluster] diğer özel seçenekleri kullanarak bir Hdınsight kümesi oluşturmak yönergeler sağlar.
* [Hdınsight için betik eylemi betikleri geliştirme][hdinsight-write-script]
* [Yükleme ve Spark Hdınsight kümelerinde kullanma][hdinsight-install-spark]
* [Yükleme ve Hdınsight kümelerinde R kullanma][hdinsight-install-r]
* [Yükleme ve Hdınsight kümelerinde Solr kullanma](hdinsight-hadoop-solr-install.md).
* [Yükleme ve Hdınsight kümelerinde Giraph kullanma](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Küme oluşturma sırasında aşamaları"
