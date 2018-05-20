---
title: Azure Resource Manager kişisel veri | Microsoft Docs
description: Azure Resource Manager işlemlerle ilişkili kişisel verileri yönetmeyi öğrenin.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: 5c176dd185a9d2a245045e9c954279506d713e5f
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Azure Resource Manager ile ilişkili kişisel verileri yönetme

Hassas bilgileri kullanılmasını önlemek için dağıtımları, kaynak grupları veya etiketleri sağlamış olabileceğiniz herhangi bir kişisel bilgi silin. Azure Resource Manager, dağıtımları, kaynak grupları veya etiketleri sağlamış olabileceğiniz kişisel verilerini yönetmenize olanak sağlayan işlemler sağlar.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Dağıtım geçmişi bulunan kişisel verileri silme

Dağıtımları için Resource Manager dağıtım geçmişini durum iletilerini ve parametre değerlerini korur. Dağıtım geçmişinden silene kadar bu değerler kalır. Bu değerleri kişisel verileri sağlanan olmadığını görmek için dağıtımları listeleyin. Kişisel veriler bulursanız, geçmişinden dağıtımları silin.

Listeye **dağıtımları** geçmişinde kullanın:

* [Kaynak grubu göre listesi](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [az grup dağıtım listesi](/cli/azure/group/deployment#az-group-deployment-list)

Silmek için **dağıtımları** geçmişinden kullanın:

* [Silme](/rest/api/resources/deployments/delete)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [az grup dağıtımı silin](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Kaynak grubu adları bulunan kişisel verileri silme

Kaynak grubunun adı, kaynak grubu silene kadar devam ettirir. Sağlanan adlarını kişisel verileri, görmek için kaynak gruplarını listeleyin. Kişisel veriler bulursanız [kaynakları taşımak](resource-group-move-resources.md) yeni kaynak grubu ve kaynak grubu adında kişisel verilerle Sil.

Listeye **kaynak grupları**, kullanın:

* [Liste](/rest/api/resources/resourcegroups/list)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [az grup listesi](/cli/azure/group#az-group-list)

Silmek için **kaynak grupları**, kullanın:

* [Silme](/rest/api/resources/resourcegroups/delete)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Etiketlerin kişisel verilerini sil

Silme veya etiketini Değiştir kadar etiket adları ve değerleri kalıcı olmasını sağlar. Sağlanan etiketler kişisel verileri, görmek için etiketler listeleyin. Kişisel veriler bulursanız, etiketleri silin.

Listeye **etiketleri**, kullanın:

* [Liste](/rest/api/resources/tags/list)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [az etiket listesi](/cli/azure/tag#az-tag-list)

Silmek için **etiketleri**, kullanın:

* [Silme](/rest/api/resources/tags/delete)
* [Remove-AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [az etiketi Sil](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Kaynak Yöneticisi'nin için bkz: genel bakış [Resource Manager nedir?](resource-group-overview.md)