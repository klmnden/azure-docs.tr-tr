---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 04/19/2019
ms.author: tomfitz
ms.openlocfilehash: 8bd16378e9c82a011309c12cf241b59d03405a77
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60012532"
---
| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Kaynak başına [kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), her kaynak türü |800 |Kaynak türü değişir |
| Her kaynak grubu dağıtım geçmişine dağıtımları |800<sup>1</sup> |800 |
| Dağıtım başına kaynak |800 |800 |
| Yönetim kilitlerini benzersiz kapsam başına |20 |20 |
| Kaynak veya kaynak grubu başına etiket sayısı |15 |15 |
| Etiket anahtarı uzunluğunu |512 |512 |
| Etiket değeri uzunluğunu |256 |256 |

<sup>1</sup>800 dağıtımları her kaynak grubu sınırına ulaşırsanız, artık gerekmeyen geçmişinden dağıtımları silin. Dağıtım geçmişinden giriş silme dağıtılan kaynaklar etkilemez. İle geçmişinden girişleri silebilirsiniz [az grubu dağıtımı silin](/cli/azure/group/deployment) için Azure CLI veya [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) PowerShell'de.  Sürekli tümleştirme ve sürekli teslim (CI/CD) senaryo dağıtımları silmek için bir PowerShell otomatikleştiren betik bkz [remove-deployments.ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Şablon sınırları

| Value | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Parametreler |256 |256 |
| Değişkenler |256 |256 |
| Kaynakları (kopya sayısı da dahil olmak üzere) |800 |800 |
| Çıkışlar |64 |64 |
| Şablon ifadesi |24.576 karakter |24.576 karakter |
| Dışarı aktarılan şablon kaynakları |200 |200 | 
| Şablon boyutu |1 MB |1 MB |
| Parametre dosyası boyutu |64 KB |64 KB |

İç içe geçmiş bir şablon kullanarak bazı şablonu sınırlar aşabilir. Daha fazla bilgi için [Azure kaynakları dağıtılırken bağlı şablonları kullanma](../articles/azure-resource-manager/resource-group-linked-templates.md). Parametreler, değişkenleri veya çıkış sayısını azaltmak için değerlerden bir nesnesi olarak birleştirebilirsiniz. Daha fazla bilgi için [parametre olarak nesnelerin](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
