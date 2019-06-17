---
title: Kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturmaC#
description: Kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğreninC#
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: e51551d4ce8061122fce52b05e68e102b71c27a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494600"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>Kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturmaC#

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure veri gezginini kullanmak için ilk küme oluşturma ve bu kümede bir veya daha fazla veritabanı oluşturun. Ardından karşı sorgular çalıştırabileceği şekilde onlara bir veritabanına (yükle) veri alın. Bu makalede, bir küme ve bir veritabanı kullanarak oluşturduğunuz C#.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio yüklü 2019 yoksa, indirip kullanabilirsiniz **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/). Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="install-c-nuget"></a>Yükleme C# nuget

1. Yükleme [Azure Veri Gezgini (Kusto) nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).

1. Yükleme [Microsoft.IdentityModel.Clients.activedirectory nuget paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) kimlik doğrulaması için.

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki kodu kullanarak kümenizi oluşturun:

    ```C#-interactive
    string resourceGroupName = "testrg";    
    string clusterName = "mykustocluster";
    string location = "Central US";
    AzureSku sku = new AzureSku("D13_v2", 5);
    Cluster cluster = new Cluster(location, sku);
    
    var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenantName}");
    var credential = new ClientCredential(clientId: "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx", clientSecret: "xxxxxxxxxxxxxx");
    var result = authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential).Result;
    
    var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);
     
    KustoManagementClient KustoManagementClient = new KustoManagementClient(credentials)
    {
        SubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
    };

    KustoManagementClient.Clusters.CreateOrUpdate(resourceGroupName, clusterName, cluster);
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | clusterName | *mykustocluster* | İstenen kümenizin adıdır.|
   | SKU | *D13_v2* | Kümeniz için kullanılan SKU. |
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametre yok.

1. Ayarlama [kimlik bilgilerinizi](https://docs.microsoft.com/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)

1. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```C#-interactive
    KustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Sonuç içeriyorsa `ProvisioningState` ile `Succeeded` değer sonra küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

1. Aşağıdaki kodu kullanarak veritabanınızı oluşturun:

    ```c#-interactive
    TimeSpan hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    TimeSpan softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    string databaseName = "mykustodatabase";
    Database database = new Database(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);
    
    KustoManagementClient.Databases.CreateOrUpdate(resourceGroupName, clusterName, databaseName, database);
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | clusterName | *mykustocluster* | Veritabanının oluşturulacağı, kümenizin adıdır.|
   | databaseName | *mykustodatabase* | Veritabanınızın adı.|
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | SoftDeletePeriod | *3650:00:00:00* | Verileri sorgulamak kullanılabilen tutulacak süre miktarı. |
   | HotCachePeriod | *3650:00:00:00* | Veriler önbellekte tutulacak süre miktarı. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```c#-interactive
    KustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName);
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimize takip etmeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```C#-interactive
    KustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma](net-standard-ingest-data.md)
