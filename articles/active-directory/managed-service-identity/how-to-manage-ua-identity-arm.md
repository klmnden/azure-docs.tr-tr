---
title: Yönetilen hizmet Azure Resource Manager kullanılarak kimliği nasıl oluşturulacağı ve bir kullanıcı silme atanır
description: Adım adım oluşturma ve kullanıcı silme hakkında yönergeler yönetilen hizmet Azure kaynağı kullanan kimliği atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: ce8221cd7bf427084e63f8b13dcf6f0f1cc7a35e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34699011"
---
# <a name="create-list-and-delete-a-user-assigned-identity-using-azure-resource-manager"></a>Oluşturma, liste ve Azure Kaynak Yöneticisi'ni kullanarak bir kullanıcı tarafından atanan kimlik silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen hizmetlerin kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, bir Azure Kaynak Yöneticisi'ni kullanarak yönetilen kimlik atanmış bir kullanıcı oluşturun.

Liste ve Azure Resource Manager şablonu kullanarak kimlik atanmış bir kullanıcıyı silmek mümkün değil.  Oluşturma ve atanan kullanıcı kimliğini listelemek için aşağıdaki makalelere bakın:

- [Liste kullanıcı kimliği atanır](how-to-manage-ua-identity-cli.md#list-user-assigned-identities)
- [Atanan kullanıcı kimliğini Sil](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-identity)
## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

Azure'da yerel olarak oturum açın veya Azure portalı üzerinden Azure aboneliğiyle ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolü) VM üzerinde yazma izinleri.

## <a name="template-creation-and-editing"></a>Şablon oluşturma ve düzenleme

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

- Kullanarak bir [Azure Marketi'nden özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
- Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
- Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
- Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için. 

## <a name="create-a-user-assigned-identity"></a>Bir kullanıcı kimliği atanır oluşturun 

Bir kullanıcı kimliği atanır oluşturmak için aşağıdaki şablonu kullanın. Değiştir `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizi değerle:

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
      "name": "[parameters('<USER ASSIGNED IDENTITY NAME>')]",
      "apiVersion": "2015-08-31-PREVIEW",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('<USER ASSIGNED IDENTITY NAME>')]"
      }
  }
}
```
## <a name="related-content"></a>İlgili içerik

Atanan kullanıcı kimliğini kullanarak bir Azure Resource Manager şablonu bkz: bir Azure VM atama hakkında bilgi için [bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma](qs-configure-template-windows-vm.md).


 
