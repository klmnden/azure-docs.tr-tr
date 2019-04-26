---
title: Bir şablonu kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik yapılandırma
description: Bir Azure Resource Manager şablonu kullanarak Azure sanal makinesinde, Azure kaynakları için yönetilen kimlikleri yapılandırma için adım adım yönergeler.
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
ms.date: 09/14/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1cb96f4aaef461d049ca496780d542ad7db229e2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307769"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-a-templates"></a>Şablonları kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak, Azure sanal makinesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

## <a name="prerequisites"></a>Önkoşullar

- Azure Resource Manager dağıtım şablonu kullanmaya bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
   - Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates), veya [dağıtımının geçerli durumu](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
   - Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için.  

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Bir sistem veya kullanıcı tarafından atanan yönetilen kimlik yeni veya mevcut bir VM üzerindeki etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/deployment-modes.md) dağıtımları.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirin ve bir Azure Resource Manager şablonu kullanarak bir sistem tarafından atanan yönetilen kimliği devre dışı.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Bir Azure VM veya varolan bir VM'yi oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Yönetilen kimlik sistem tarafından atanan bir VM'de etkinleştirmek için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Yönetilen kimlik sistem tarafından atanan etkinleştirmek için şablon bir düzenleyiciye yüklenemedi, bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümünde ve ekleme `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   },
   ```

> [!NOTE]
> VM uzantısını Azure kaynakları için yönetilen kimlik olarak belirterek isteğe bağlı olarak sağlama bir `resources` şablondaki öğesi. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Daha fazla bilgi için [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

3. İşiniz bittiğinde, aşağıdaki bölümlerde eklenen `resource` şablonunuzu ve bölümünü aşağıdaki benzemesi gerekir:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },
        
            //The following appears only if you provisioned the optional VM extension (to be deprecated)
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-managed-identity"></a>Sanal makinenin yönetilen kimlik sistem tarafından atanan bir role atama

Yönetilen kimlik sistem tarafından atanan sanal makinenizde etkinleştirdikten sonra bir rol gibi vermek isteyebilirsiniz **okuyucu** içinde oluşturulduğu kaynak grubuna erişim.

Sanal makinenizin sistem tarafından atanan kimlik için bir rol atamak için hesabınızın gerekli [kullanıcı erişimi Yöneticisi](/azure/role-based-access-control/built-in-roles#user-access-administrator) rol ataması.

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.
 
2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) ve sanal makinenize vermek için aşağıdaki bilgileri ekleyin **okuyucu** içinde oluşturulduğu kaynak grubuna erişim.  Şablon yapınızı, düzenleyici ve seçtiğiniz dağıtım modeline bağlı olarak değişiklik gösterebilir.
   
   Altında `parameters` bölümüne aşağıdakileri ekleyin:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Altında `variables` bölümüne aşağıdakileri ekleyin:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Altında `resources` bölümüne aşağıdakileri ekleyin:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-vm"></a>Sistem tarafından atanan yönetilen bir kimlik bir Azure VM'den devre dışı bırak

Yönetilen kimlik sistem tarafından atanan bir sanal makineden kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Yalnızca yönetilen kimlik sistemi tarafından atanmış olan bir sanal makine varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.  
   
   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   Sanal makinenizin sistem ve kullanıcı tarafından atanan yönetilen kimlikleri varsa, Kaldır `SystemAssigned` kimlik türü ve canlı `UserAssigned` ile birlikte `userAssignedIdentities` değerleri sözlüğü.

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**
   
   Varsa, `apiVersion` olduğu `2017-12-01` ve sanal makinenizin sistem ve kullanıcı tarafından atanan yönetilen kimlikleri kaldırmak `SystemAssigned` kimlik türü ve canlı `UserAssigned` ile birlikte `identityIds` kullanıcı tarafından atanan dizisi yönetilen kimlikleri.  
   
Aşağıdaki örnek, bir VM'den hiçbir kullanıcı tarafından atanan yönetilen kimliklerle nasıl sistem tarafından atanan bir yönetilen kimlik kaldırmak gösterir:

```JSON
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[parameters('vmName')]",
    "location": "[resourceGroup().location]",
    "identity": { 
        "type": "None"
}
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager şablonu kullanarak bir Azure sanal makinesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturmak için bkz [kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-an-azure-vm"></a>Bir Azure sanal makinesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atama

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Altında `resources` öğesi, kullanıcı tarafından atanan bir yönetilen kimlik VM'nize atamak için şu girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` kullanıcı tarafından atanan adı ile yönetilen oluşturduğunuz kimliği.

   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**

   Varsa, `apiVersion` olduğu `2018-06-01`, kullanıcı tarafından atanan yönetilen kimliklerinizi depolanır `userAssignedIdentities` sözlük biçimi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, tanımlı bir değişkende `variables` şablonunuzun bölümü.

   ```json
   {
       "apiVersion": "2018-06-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachines API Sürüm 2017-12-01**
    
   Varsa, `apiVersion` olduğu `2017-12-01`, kullanıcı tarafından atanan yönetilen kimliklerinizi depolanır `identityIds` dizi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, tanımlı bir değişkende `variables` şablonunuzun bölümü.
    
   ```json
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```
       
3. İşiniz bittiğinde, aşağıdaki bölümlerde eklenen `resource` şablonunuzu ve bölümünü aşağıdaki benzemesi gerekir:
   
   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        //The following appears only if you provisioned the optional VM extension (to be deprecated)                  
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2018-06-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```
   **Microsoft.Compute/virtualMachines API Sürüm 2017-12-01**
   
   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },
                 
        //The following appears only if you provisioned the optional VM extension (to be deprecated)                   
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir Azure VM'den kaldırın.

Bir kullanıcı tarafından atanan kimliği bir sanal makineden kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Yalnızca kullanıcı tarafından atanan yönetilen kimlik sahip bir VM varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.
 
   Aşağıdaki örnek hiçbir sistem tarafından atanan yönetilen kimliklerle bir VM'den nasıl kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırmak gösterir:
   
   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```
   
   **Microsoft.Compute/virtualMachines API sürümü 2018-06-01**
    
   Tek bir kullanıcı tarafından atanan yönetilen kimlik bir sanal makineden kaldırmak için oradan kaldırın `useraAssignedIdentities` sözlüğü.

   Sistem tarafından atanan bir yönetilen kimlik varsa, bunu tutmak içinde `type` altındaki `identity` değeri.
 
   **Microsoft.Compute/virtualMachines API Sürüm 2017-12-01**

   Tek bir kullanıcı tarafından atanan yönetilen kimlik bir sanal makineden kaldırmak için oradan kaldırın `identityIds` dizisi.

   Sistem tarafından atanan bir yönetilen kimlik varsa, bunu tutmak içinde `type` altındaki `identity` değeri.
   
## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen Azure kaynaklarına genel bakış için kimlikleri](overview.md).

