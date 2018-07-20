---
title: Yönetilen hizmet kimliği Azure Resource Manager kullanarak oluşturma ve kullanıcı silme atanan
description: Adım adım yönergeler oluşturmak ve kullanıcıyı silmek yönetilen hizmet kimliği kullanarak Azure Resource atanmış.
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
ms.openlocfilehash: ced2a8354e63288ad9957b6a177b43c97b58698c
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160544"
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Oluşturma, listeleme ve Azure Resource Manager kullanarak bir kullanıcı tarafından atanan Kimliği Sil

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager'ı kullanarak yönetilen kimlik atanmış bir kullanıcı oluşturun.

Liste ve bir kullanıcı tarafından atanan kimliği bir Azure Resource Manager şablonu kullanarak silmek mümkün değildir.  Oluşturma ve bir kullanıcı tarafından atanan kimliği listelemek için aşağıdaki makalelere bakın:

- [Kullanıcı tarafından atanan kimliği listesi](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Kullanıcı tarafından atanan kimlik Sil](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızı sunan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolünü) VM üzerinde yazma izinleri.

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenleme

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sunar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

- Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
- Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
- Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için. 

## <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği oluşturma 

Bir kullanıcı tarafından atanan kimliği oluşturmak için aşağıdaki şablonu kullanın. En azından, hesabınızın atanması gerekir. [yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma. Değiştirin `<USER ASSIGNED IDENTITY NAME>` değerini kendi değerlerinizle:

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
## <a name="related-content"></a>İlgili içerik

Bir Azure Resource Manager şablonu bakın kullanarak Azure VM için bir kullanıcı tarafından atanan kimliği atama hakkında bilgi için [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](qs-configure-template-windows-vm.md).


 
