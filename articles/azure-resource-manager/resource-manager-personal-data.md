---
title: Azure Resource Manager kişisel veri | Microsoft Docs
description: Azure Resource Manager işlemleriyle ilişkili kişisel verileri yönetme konusunda bilgi edinin.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: cc8400a3b6d51bacd55d3c711700a1d07266f528
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206270"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Azure Resource Manager ile ilişkili kişisel verileri yönetme

Hassas bilgileri kullanılmasını önlemek için dağıtımları, kaynak grupları veya etiketleri sağlamış olabileceğiniz kişisel bilgileri silin. Azure Resource Manager, dağıtımları, kaynak grupları veya etiketleri sağlamış olabileceğiniz kişisel verileri yönetmenize olanak sağlayan işlemler sağlar.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Dağıtım geçmişi kişisel verilerini silme

Dağıtımlar için Resource Manager dağıtım geçmişini durum iletilerini ve parametre değerlerini korur. Dağıtım geçmişinden silene kadar bu değerler kalır. Sağlanan bu değerleri kişisel verileri, görmek için dağıtımları listeleyin. Kişisel verilerini bulma, dağıtım geçmişinden silin.

Listeye **dağıtımları** geçmişinde kullanın:

* [Kaynak grubu listesi](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [az grubun dağıtım listesi](/cli/azure/group/deployment#az-group-deployment-list)

Silinecek **dağıtımları** geçmişinden kullanın:

* [Silme](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [az grubu dağıtımı Sil](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Kaynak grubu adları kişisel verilerini silme

Kaynak grubu adı, kaynak grubunu silme kadar devam eder. Sağlanan adlarında kişisel verileri, görmek için kaynak gruplarının listesi. Kişisel verileri bulursanız [kaynakları taşıma](resource-group-move-resources.md) yeni kaynak grubu ve adında kişisel verileri içeren kaynak grubunu silin.

Listeye **kaynak grupları**, kullanın:

* [Liste](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [az grup listesi](/cli/azure/group#az-group-list)

Silinecek **kaynak grupları**, kullanın:

* [Silme](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Etiketleri kişisel verilerini silme

Silme veya etiket değiştirmek kadar etiket adları ve değerleri kalıcı. Sağlanan etiketleri kişisel verileri, görmek için etiketleri listeleyin. Kişisel verilerini bulma, etiketleri silin.

Listeye **etiketleri**, kullanın:

* [Liste](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [az etiket listesi](/cli/azure/tag#az-tag-list)

Silinecek **etiketleri**, kullanın:

* [Silme](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [az tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager için bkz: genel bakış [Resource Manager nedir?](resource-group-overview.md)