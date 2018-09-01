---
title: Oluşturma ve Azure Resource Manager kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil
description: Adım adım Azure Resource Manager kullanarak kimlikleri oluşturma ve kullanıcı tarafından atanan silme hakkında yönergeler yönetilen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: adfa7ebfd911bfcbc88e01030777e91c48841784
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338354"
---
# <a name="create-list-and-delete-a-user-assigned-managed-identity-using-azure-resource-manager"></a>Oluşturma, listeleme ve Azure Resource Manager kullanarak bir kullanıcı tarafından atanan yönetilen kimlik Sil

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager'ı kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturun.

Liste ve bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan yönetilen kimlik silmek mümkün değildir.  Oluşturmak için aşağıdaki makalelere bakın ve yönetilen listesinde bir kullanıcı tarafından atanan kimliği:

- [Kullanıcı tarafından atanan yönetilen kimlik listesi](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Kullanıcı tarafından atanan yönetilen kimlik Sil](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)
## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede işlemleri gerçekleştirmek için aşağıdaki rol ataması hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve kullanıcı tarafından atanan bir yönetilen kimlik silin.

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenleme

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sunar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

- Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
- Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
- Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için. 

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma 

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için aşağıdaki şablonu kullanın. Değiştirin `<USER ASSIGNED IDENTITY NAME>` değerini kendi değerlerinizle:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

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
      "apiVersion": "2015-08-31-PREVIEW",
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


 
