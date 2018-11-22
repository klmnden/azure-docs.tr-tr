---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 5c2c7b621512be7b81d14b99069d52f4f3aa3f33
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279989"
---
| Kaynak | Varsayılan Sınır | Üst Sınır |
| --- | --- | --- |
| Kaynak başına [kaynak grubu](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (başına kaynak türü) |800 |Kaynak türü değişir |
| Her kaynak grubu dağıtım geçmişine dağıtımları |800 |800 |
| Dağıtım başına kaynak |800 |800 |
| Yönetim kilitlerini (başına benzersiz kapsam) |20 |20 |
| Etiket sayısı (her bir kaynak veya kaynak grubu) |15 |15 |
| Etiket anahtarı uzunluğunu |512 |512 |
| Etiket değeri uzunluğunu |256 |256 |


#### <a name="template-limits"></a>Şablon sınırları

| Değer | Varsayılan Sınır | Üst Sınır |
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

Her kaynak grubu 800 dağıtımlarının sınıra ulaştıysanız, artık gerekmeyen geçmişinden dağıtımları silin. İle geçmişinden girişleri silebilirsiniz [az grubu dağıtımı silin](/cli/azure/group/deployment#az_group_deployment_delete) için Azure CLI veya [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) PowerShell'de. Dağıtım geçmişinden giriş silme Dağıt kaynakları etkilemez. 