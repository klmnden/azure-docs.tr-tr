---
title: 'Hızlı Başlangıç: Python kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma'
description: Python kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğrenin
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/17/2019
ms.openlocfilehash: 4f87c5996ea323c26c32c1680ba6f627bf8f95c2
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287540"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Python kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Bu hızlı başlangıçta Python kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma işlemini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="install-python-package"></a>Python paketini yükle

Azure Veri Gezgini (Kusto) için Python paketini yüklemek için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```
pip install azure-mgmt-kusto
```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | küme_adı | *mykustocluster* | İstenen kümenizin adıdır.|
   | sku | *D13_v2* | Kümeniz için kullanılan SKU. |
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametre yok.
    
    'Kimlik' için kimlik bilgilerinizi ayarlayın (daha fazla bilgi için https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python )

2. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Sonuç içeriyorsa `provisioningState` ile `Succeeded` değer sonra küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = Database(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | küme_adı | *mykustocluster* | Veritabanının oluşturulacağı, kümenizin adıdır.|
   | database_name | *mykustodatabase* | Veritabanınızın adı.|
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | soft_delete_period | *3650 gün 0:00:00* | Verileri sorgulamak kullanılabilen tutulacak süre miktarı. |
   | hot_cache_period | *3650 gün 0:00:00* | Veriler önbellekte tutulacak süre miktarı. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md)
