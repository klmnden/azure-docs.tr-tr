---
title: PowerShell kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma
description: PowerShell kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğrenin
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 85c8cc81b3c61e2faf65bf8a06c69ca7f2b906c9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497334"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>PowerShell kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure veri gezginini kullanmak için ilk küme oluşturma ve bu kümede bir veya daha fazla veritabanı oluşturun. Ardından karşı sorgular çalıştırabileceği şekilde onlara bir veritabanına (yükle) veri alın. Bu makalede, bir küme ve bir veritabanını Powershell kullanarak oluşturduğunuz. Windows, Linux üzerinde veya PowerShell cmdlet'leri ve betikleri çalıştırılabilir [Azure Cloud Shell](../cloud-shell/overview.md) ile [Az.Kusto](/powershell/module/az.kusto/?view=azps-1.4.0#kusto) Azure Veri Gezgini kümeleri ve veritabanlarını oluşturma ve yapılandırma için.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yı yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-parameters"></a>Parametreleri Yapılandır

Komutları Azure Cloud Shell'de çalıştırıyorsanız, aşağıdaki adımları gerekli değildir. CLI'yi yerel olarak çalıştırıyorsanız, 1 ve 2 Azure'da oturum açın ve geçerli aboneliğinizi ayarlamak için adımları izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Abonelik oluşturulacak kümenizi istediğiniz ayarlayın:

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
1. Azure CLI'yi yerel olarak veya Azure Cloud Shell'i çalıştırırken, Cihazınızda Az.Kusto modül yüklemeniz gerekir:
    
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

1. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster -ResourceGroupName testrg
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

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster -ResourceGroupName testrg -Name mykustodatabase
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimize takip etmeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Ek Az.Kusto komutlar](/powershell/module/az.kusto/?view=azps-1.7.0#kusto)
* [Azure Veri Gezgini .NET standart SDK'sı (Önizleme) kullanarak veri alma](net-standard-ingest-data.md)
