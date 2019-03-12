---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: a528fad10144ec733a3db5340ef12dee5ce5411c
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553894"
---
| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Kaynak başına [kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), her kaynak türü |800 |Kaynak türü değişir |
| Her kaynak grubu dağıtım geçmişine dağıtımları |800 |800 |
| Dağıtım başına kaynak |800 |800 |
| Yönetim kilitlerini benzersiz kapsam başına |20 |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |15 |15 |
| Etiket anahtarı uzunluğunu |512 |512 |
| Etiket değeri uzunluğunu |256 |256 |


#### <a name="template-limits"></a>Şablon sınırları

| Değer | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Parametreler |256 |256 |
| Değişkenler |256 |256 |
| Kopya sayısı içeren kaynakları |800 |800 |
| Çıkışlar |64 |64 |
| Şablon ifadesi |24.576 karakter |24.576 karakter |
| Dışarı aktarılan şablon kaynakları |200 |200 | 
| Şablon boyutu |1 MB |1 MB |
| Parametre dosyası boyutu |64 KB |64 KB |

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırlar aşabilir. Daha fazla bilgi için [Azure kaynakları dağıtılırken bağlı şablonları kullanma](../articles/azure-resource-manager/resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkış sayısını azaltmak için değerlerden bir nesnesi olarak birleştirebilirsiniz. Daha fazla bilgi için [parametre olarak nesnelerin](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Her kaynak grubu 800 dağıtımlarının sınıra ulaştıysanız, artık gerekmeyen geçmişinden dağıtımları silin. İle geçmişinden girişleri silebilirsiniz [az grubu dağıtımı silme](/cli/azure/group/deployment) Azure CLI için. Ayrıca [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) PowerShell'de. Dağıtım geçmişinden giriş silme dağıtım kaynakları etkilemez. 