---
title: Oluşturma ve Azure Resource Manager kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil
description: Adım adım Azure Resource Manager kullanarak kimlikleri oluşturma ve kullanıcı tarafından atanan silme hakkında yönergeler yönetilen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: cccf129ff4ed9389422ec900a4a23f489cf3c7a4
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58444206"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Oluşturma, listeleme ve Azure Resource Manager kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager'ı kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturun.

Liste ve bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silmek mümkün değildir.  Oluşturmak için aşağıdaki makalelere bakın ve yönetilen listesinde bir kullanıcı tarafından atanan kimliği:

- [Kullanıcı tarafından atanan yönetilen kimlik listesi](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Kullanıcı tarafından atanan yönetilen kimlik Sil](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
  ## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenleme

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sunar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

- Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
- Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates), veya [dağıtımının geçerli durumu](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
- Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için. 

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için hesabınızın gerekli [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol ataması.

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için aşağıdaki şablonu kullanın. Değiştirin `<USER ASSIGNED IDENTITY NAME>` değerini kendi değerlerinizle:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2018-11-30",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="next-steps"></a>Sonraki adımlar

Bir kullanıcı tarafından atanan atama hakkında bilgi için yönetilen bir Azure Resource Manager şablonu bakın kullanarak bir Azure VM kimliği [yapılandırma kimlikleri şablonları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen](qs-configure-template-windows-vm.md).


 
