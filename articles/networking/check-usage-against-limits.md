---
title: Azure kaynak kullanımı sınırlarını karşı denetleyin. | Microsoft Docs
description: Azure abonelik limitleri karşı Azure kaynak kullanımınızı kontrol öğrenin.
services: networking
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: networking
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/05/2018
ms.author: jdial
ms.openlocfilehash: 30b0c1bdd23858b5cc6224deb2698b5f180359eb
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288239"
---
# <a name="check-resource-usage-against-limits"></a>Sınırları karşı kaynak kullanımını denetleyin

Bu makalede, aboneliğiniz ve hangi içinde dağıttığınız her bir ağ kaynak türü sayısını görmek öğrenin, [abonelik limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) olan. Kaynak kullanımı sınırlarını karşı görüntüleme olanağı geçerli kullanımını izlemek ve gelecekte kullanılmak üzere plan yardımcı olur. Kullanabileceğiniz [Azure portalı](#azure-portal), [PowerShell](#powershell), veya [Azure CLI](#azure-cli) kullanımını izlemek için.

## <a name="azure-portal"></a>Azure Portal

1. Azure'da oturum [portalı](https://portal.azure.com).
2. Azure portalının üst, sol köşede seçin **tüm hizmetleri**.
3. Girin *abonelikleri* içinde **filtre** kutusu. Zaman **abonelikleri** arama sonuçlarında görünür.
4. Kullanım bilgilerini görüntülemek istediğiniz abonelik adını seçin.
5. Altında **ayarları**seçin **kullanım + kota**.
6. Aşağıdaki seçeneklerden birini belirleyebilirsiniz:
    - **Kaynak türleri**: tüm kaynak türleri seçin veya belirli türlerini görüntülemek istediğiniz kaynakları seçin.
    - **Sağlayıcıları**: tüm kaynak sağlayıcılarını seçin veya seçin **işlem**, **ağ**, veya **depolama**.
    - **Konumları**: tüm Azure konumları seçin veya belirli konumları seçin.
    - Tüm kaynakları veya yalnızca en az bir dağıtıldığı kaynakları göster seçeneğini belirleyebilirsiniz.

    Aşağıdaki resimde örnekte Doğu ABD bölgesinde dağıtılmış en az bir kaynak ile ağ kaynaklarını tüm gösterir:

        ![View usage data](./media/check-usage-against-limits/view-usage.png)

    Sütunlar, sütunun başlığını seçerek sıralayabilirsiniz. Gösterilen sınırları Aboneliğinize yönelik limitlerdir. Varsayılan bir sınırı artırmanız gerekiyorsa, seçin **isteği artırmak**, ardından tamamlamak ve destek isteği gönderin. Tüm kaynaklar Azure'da listelenen üst sınırına sahip [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Geçerli sınırlarınızı zaten maksimum sayı ise, bu sınır yükseltilemez.

## <a name="powershell"></a>PowerShell

İçinde izleyen komutları çalıştırabilirsiniz [Azure Cloud Shell](https://shell.azure.com/powershell), veya PowerShell bilgisayarınızdan çalıştırarak. Azure Cloud Shell ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell kullanarak bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü sürüm 6.0.1 veya üzeri. Çalıştırma `Get-Module -ListAvailable AzureRM` yüklü sürümü bulmak için bilgisayarınızda. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Login-AzureRmAccount` için Azure'da oturum açın.

Kullanımınızı sınırlarla karşı görüntülemek [Get-AzureRmNetworkUsage](https://docs.microsoft.com/en-us/powershell/module/azurerm.network/get-azurermnetworkusage?view=azurermps-6.8.0). Aşağıdaki örnek, Doğu ABD konumunda en az bir kaynak dağıtıldığı kaynak kullanımını alır:

```azurepowershell-interactive
Get-AzureRmNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Bir çıkış alırsınız aynı aşağıdaki örnek çıktı olarak biçimlendirilmiş:

```powershell
ResourceType            CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
```

## <a name="azure-cli"></a>Azure CLI

Bu makaledeki görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure Cloud Shell](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerekir veya üzeri. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `az login` için Azure'da oturum açın.

Kullanımınızı sınırlarla karşı görüntülemek [az ağ kullanımları-Listele](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). Aşağıdaki örnek, Doğu ABD konumunda kaynakların kullanımı alır:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Bir çıkış alırsınız aynı aşağıdaki örnek çıktı olarak biçimlendirilmiş:

```azurecli
Name                    CurrentValue Limit
------------            ------------ -----
Virtual Networks                   1    50
Network Security Groups            2   100
Public IP Addresses                1    60
Network Interfaces                 1 24000
Network Watchers                   1     1
Load Balancers                     0   100
Application Gateways               0    50
```
