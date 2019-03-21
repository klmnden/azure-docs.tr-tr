---
title: 'Hızlı Başlangıç: PowerShell kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma'
description: PowerShell kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğrenin
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/17/2019
ms.openlocfilehash: 650bdc5cdf99645bc2be6c8e85737dacd10a6b27
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287539"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>PowerShell kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  


Bu hızlı başlangıçta PowerShell kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma işlemini açıklamaktadır.

Windows, Linux üzerinde veya PowerShell cmdlet'leri ve betikleri çalıştırılabilir [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) oluşturmak ve yapılandırmak için [Azure Veri Gezgini](https://docs.microsoft.com/azure/kusto/ ).

[ **Az.Kusto**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ). Azure PowerShell ile ve **Az.Kusto**, aşağıdaki görevleri gerçekleştirebilirsiniz:

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="configure-parameters"></a>Parametreleri Yapılandır

Komutları Azure Cloud Shell'de çalıştırıyorsanız, aşağıdaki adımları gerekli değildir. CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açın ve geçerli aboneliğinizi ayarlamak için aşağıdaki adımları izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Abonelik oluşturulacak kümenizi istediğiniz ayarlayın.

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
3. Cihazınızda Az.Kusto modülünü yükleyin:
    
    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto  
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Ad | *mykustocluster* | İstenen kümenizin adıdır.|
   | Sku | *D13_v2* | Kümeniz için kullanılan SKU. |
   | ResourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametre yok.

2. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster --ResourceGroupName testrg
    ```

Sonuç içeriyorsa `provisioningState` ile `Succeeded` değer sonra küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Küme adı | *mykustocluster* | Veritabanının oluşturulacağı, kümenizin adıdır.|
   | Ad | *mykustodatabase* | Veritabanınızın adı.|
   | ResourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | SoftDeletePeriod | *3650:00:00:00* | Verileri sorgulamak kullanılabilen tutulacak süre miktarı. |
   | HotCachePeriod | *3650:00:00:00* | Veriler önbellekte tutulacak süre miktarı. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster --ResourceGroupName testrg -Name mykustodatabase
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla Az.Kusto komutlarını bulabilirsiniz [ **burada**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto )

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma](net-standard-ingest-data.md)
