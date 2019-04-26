---
title: 'Hızlı Başlangıç: Azure CLI kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma'
description: Azure CLI kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğrenin
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 04/10/2019
ms.openlocfilehash: 1fb9027ab3301bb860d260aed737ab7674039d9b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60446163"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-azure-cli"></a>Azure CLI kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure veri gezginini kullanmak için ilk küme oluşturma ve bu kümede bir veya daha fazla veritabanı oluşturun. Ardından karşı sorgular çalıştırabileceği şekilde onlara bir veritabanına (yükle) veri alın. Bu hızlı başlangıçta, bir küme ve bir veritabanı Azure CLI kullanarak oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yı yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıçta Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-the-cli-parameters"></a>CLI parametreleri Yapılandır

Komutları Azure Cloud Shell'de çalıştırıyorsanız, aşağıdaki adımları gerekli değildir. CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açın ve geçerli aboneliğinizi ayarlamak için aşağıdaki adımları izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az login
    ```

1. Abonelik oluşturulacak kümenizi istediğiniz ayarlayın. Değiştirin `MyAzureSub` kullanmak istediğiniz Azure aboneliği adı:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    ```azurecli-interactive
    az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | ad | *azureclitest* | İstenen kümenizin adıdır.|
   | sku | *D13_v2* | Kümeniz için kullanılan SKU. |
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametre yok.

1. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Sonuç içeriyorsa `provisioningState` ile `Succeeded` değer sonra küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period P365D --hot-cache-period P31D
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Küme adı | *azureclitest* | Veritabanının oluşturulacağı, kümenizin adıdır.|
   | ad | *clidatabase* | Veritabanınızın adı.|
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | Geçici silme süresi | *P365D* | Verileri sorgulamak için kullanılabilen tutulacak süreyi belirtir. Bkz: [Bekletme İlkesi](/azure/kusto/concepts/retentionpolicy) daha fazla bilgi için. |
   | Sık erişimli-cache-süresi | *P31D* | Veriler önbellekte tutulacak süreyi belirtir. Bkz: [önbellek İlkesi](/azure/kusto/concepts/cachepolicy) daha fazla bilgi için. |

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md)
