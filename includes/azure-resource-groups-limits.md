---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 3dc09de6afaddeb06b0243eb46e888b673109545
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505727"
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

Her kaynak grubu 800 dağıtımlarının sınıra ulaştıysanız, artık gerekmeyen geçmişinden dağıtımları silin. İle geçmişinden girişleri silebilirsiniz [az grubu dağıtımı silme](/cli/azure/group/deployment) Azure CLI için. Ayrıca [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) PowerShell'de. Dağıtım geçmişinden giriş silme dağıtım kaynakları etkilemez. 