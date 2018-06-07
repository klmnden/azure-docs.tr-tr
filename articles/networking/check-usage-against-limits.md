---
title: Azure kaynak kullanım sınırları karşı denetleyin | Microsoft Docs
description: Azure kaynak kullanımınızı Azure abonelik limitleri karşı denetleyin öğrenin.
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
ms.openlocfilehash: 0c51f48576c665fbe67f2f18198d6422fe872895
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34811758"
---
# <a name="check-resource-usage-against-limits"></a>Kaynak kullanım sınırları karşı denetleyin

Bu makalede, aboneliğiniz ve ne dağıttıktan sonra her ağ kaynağı türü sayısını görmek öğrenin, [abonelik sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits) şunlardır. Kaynak kullanım sınırları göre görüntüleme yeteneği geçerli kullanımı izlemek ve gelecekte kullanılmak üzere planlamak yararlıdır. Kullanabileceğiniz [Azure Portal](#azure-portal), [PowerShell](#powershell), veya [Azure CLI](#azure-cli) kullanımını izlemek için.

## <a name="azure-portal"></a>Azure Portal

1. Azure günlüğüne [portal](https://portal.azure.com).
2. Azure portalı üst, sol köşesinde seçin **tüm hizmetleri**.
3. Girin *abonelikleri* içinde **filtre** kutusu. Zaman **abonelikleri** arama sonuçlarında görünür.
4. İçin kullanım bilgilerini görüntülemek istediğiniz abonelik adını seçin.
5. Altında **ayarları**seçin **kullanım + kota**.
6. Şu seçenekleri seçebilirsiniz:
    - **Kaynak türleri**: tüm kaynak türleri seçin veya görüntülemek istediğiniz kaynakları belirli türlerini seçin.
    - **Sağlayıcıları**: tüm kaynak sağlayıcıları seçin veya seçin **işlem**, **ağ**, veya **depolama**.
    - **Konumları**: tüm Azure konumları seçin veya belirli konumlara seçin.
    - Tüm kaynakları veya yalnızca en az bir dağıtıldığı kaynakları görüntülemeyi seçebilirsiniz.

    Aşağıdaki resimde örnekte tüm Doğu ABD dağıtılan en az bir kaynağı ile ağ kaynaklarını gösterir:

        ![View usage data](./media/check-usage-against-limits/view-usage.png)

    Sütunun başlığını seçerek sütunları sıralama yapabilirsiniz. Gösterilen sınırları, aboneliğiniz için kısıtlamalardır. Varsayılan sınırı artırmak gereksinim duyarsanız, seçin **isteği artırmak**, daha sonra tamamlamak ve destek isteği gönderin. Tüm kaynakları Azure içinde listelenen bir üst sınırı olacak [sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fnetworking%2ftoc.json#networking-limits). Geçerli sınırınızı zaten en fazla sayı ise, sınır yükseltilemez.

## <a name="powershell"></a>PowerShell

Takip edin komutları çalıştırmadan [Azure bulut Kabuk](https://shell.azure.com/powershell), veya bilgisayarınızdan PowerShell çalıştırarak. Azure bulut Kabuk ücretsiz etkileşimli kabuk ' dir. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell bilgisayarınızdan çalıştırırsanız, gereksinim duyduğunuz *AzureRM* PowerShell modülü, sürüm 6.0.1 veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` bilgisayarınızda yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `Login-AzureRmAccount` Azure'da oturum açma için.

Kullanımınızı sınırlarıyla karşı görüntülemek [Get-AzureRmNetworkUsage](/powershell/module/azurerm.network/powershell/module/azurerm.network/get-azurermnetworkusage). Aşağıdaki örnekte Doğu ABD konumunda en az bir kaynak dağıtıldığı kaynakları için kullanım alır:

```azurepowershell-interactive
Get-AzureRmNetworkUsage `
  -Location eastus `
  | Where-Object {$_.CurrentValue -gt 0} `
  | Format-Table ResourceType, CurrentValue, Limit
```

Çıktı aldığınız aşağıdaki örnek çıkış aynı biçimlendirilmiş:

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

Bu makalede görevleri tamamlamak için Azure komut satırı arabirimi (CLI) komutlarını kullanarak, ya da komutları çalıştırmak [Azure bulut Kabuk](https://shell.azure.com/bash), veya bilgisayarınızdan CLI çalıştırarak. Bu makale Azure CLI Sürüm 2.0.32 gerektirir veya sonraki bir sürümü. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI yerel olarak çalıştırıyorsanız, ayrıca çalıştırmanız gereken `az login` Azure'da oturum açma için.

Kullanımınızı sınırlarıyla karşı görüntülemek [az ağ listesi-kullanımları](/cli/azure/network?view=azure-cli-latest#az-network-list-usages). Aşağıdaki örnekte Doğu ABD konumunda kaynakları için kullanım alır:

```azurecli-interactive
az network list-usages \
  --location eastus \
  --out table
```

Çıktı aldığınız aşağıdaki örnek çıkış aynı biçimlendirilmiş:

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
