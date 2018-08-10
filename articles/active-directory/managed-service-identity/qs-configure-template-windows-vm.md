---
title: Bir şablonu kullanarak bir Azure VM yönetilen hizmet kimliği yapılandırma
description: Bir Azure Resource Manager şablonu kullanarak Azure sanal makinesinde, bir yönetilen hizmet kimliği yapılandırmaya ilişkin adım adım yönergeler.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 79b499f8063e5c15f76d89182955cbd90fb1039f
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629319"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure sanal makinesinde aşağıdaki yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir VM oluşturun ve etkinleştirin ve sistem ve/veya kullanıcı için bir Azure VM'den yönetilen kimlik atanır.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makineye kaldırmak için.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
   - Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için.  

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Bir sistem veya kullanıcı tarafından atanan kimliği yeni veya mevcut bir VM üzerindeki etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/deployment-modes.md) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirin ve atanan kimliği bir Azure Resource Manager şablonu kullanarak bir sistemi devre dışı bırakın.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Bir Azure VM veya varolan bir VM'yi oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Şablonu bir düzenleyiciye yüklendikten sonra bulma `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyici bağlı olarak, aşağıdaki ekran görüntüsünde biraz farklı görünebilir ve düzenlediğiniz var olan bir ya da yeni bir dağıtım için bir şablon.

   >[!NOTE] 
   > Bu örnek gibi değişkenleri varsayar `vmName`, `storageAccountName`, ve `nicName` şablonda tanımlanmadı.
   >

   ![VM şablonu - ekran görüntüsü bulun](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Sistem tarafından atanan kimlik etkinleştirmek için eklemeniz `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (İsteğe bağlı) VM yönetilen hizmet kimliği bir uzantısı olarak ekleme bir `resources` öğesi. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

   >[!NOTE] 
   > Aşağıdaki örnekte, bir Windows VM uzantısı varsayılır (`ManagedIdentityExtensionForWindows`) dağıtılıyor. Kullanarak Linux için yapılandırabilirsiniz `ManagedIdentityExtensionForLinux` için bunun yerine, `"name"` ve `"type"` öğeleri.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
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
           },
           "protectedSettings": {}
       }
   }
   ```

5. İşiniz bittiğinde, şablonunuzu aşağıdakine benzer görünmelidir:

   ![Güncelleştirmeden sonra şablon görüntüsü](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="assign-a-role-the-vms-system-assigned-identity"></a>Sanal makinenin sistem tarafından atanan kimlik rol atama

Sanal makinenizde sistem tarafından atanan kimlik etkinleştirdikten sonra bir rol gibi vermek isteyebilirsiniz **okuyucu** içinde oluşturulduğu kaynak grubuna erişim.

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

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Bir Azure VM'den atanan kimliği bir sistemi devre dışı bırak

Yönetilen hizmet kimliği artık gerektiren bir VM'niz varsa:

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Yalnızca sistem tarafından atanan kimliği sahip bir VM varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.  Sanal makinenizin sistem ve kullanıcı tarafından atanan kimliklerle varsa, Kaldır `SystemAssigned` kimlik türü ve canlı `UserAssigned` ile birlikte `identityIds` kullanıcı tarafından atanan kimlikleri dizisi.  Aşağıdaki örnek, kimlik, kullanıcı tarafından atanan kimliklerle olmadan bir VM'den atanmış bir sistem kaldırma gösterir:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, Azure Resource Manager şablonu kullanarak bir Azure sanal makinesi için bir kullanıcı tarafından atanan kimliği atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan kimliği oluşturma için bkz: [bir kullanıcı tarafından atanan kimliği oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Kimlik, bir Azure VM'sine atanan kullanıcı atama

1. Altında `resources` öğesi, bir kullanıcı tarafından atanan kimliği VM'nize atamak için şu girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` oluşturduğunuz kullanıcı tarafından atanan kimlik adı ile.
   
   > [!Important]
   > `<USERASSIGNEDIDENTITYNAME>` Aşağıdaki örnekte gösterilen değer bir değişkende depolanmalıdır.  Ayrıca, şu anda desteklenen uygulama için bir Resource Manager şablonu bir sanal makinede kullanıcı tarafından atanan kimlikleri atama API sürümü aşağıdaki örnekte sürümle aynı olmalıdır.
    
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
        },
    ```
    
2. (İsteğe bağlı) Sonraki altında `resources` öğesi, yönetilen kimlik uzantısı VM'nize atamak için şu girişi ekleyin. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. Aşağıdaki sözdizimini kullanın:
    ```json
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
    ```
    
3.  İşiniz bittiğinde, şablonunuzu aşağıdakine benzer görünmelidir:

      ![Kullanıcı tarafından atanan kimlik ekran görüntüsü](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)

### <a name="remove-user-assigned-identity-from-an-azure-vm"></a>Atanan kullanıcı kimliğini bir Azure VM'den kaldırın.

Yönetilen hizmet kimliği artık gerektiren bir VM'niz varsa:

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Yalnızca kullanıcı tarafından atanan kimliği sahip bir VM varsa, bunu değiştirerek devre dışı bırakabilirsiniz kimlik türü için `None`.  Sistem ve kullanıcı tarafından atanan kimliklerle VM'niz varsa ve sistem tarafından atanan kimlik tutmak için kaldırmak istediğiniz `UserAssigned` ile birlikte kimlik türünden `identityIds` kullanıcı tarafından atanan kimlikleri dizisi.
    
   Kaldırmak için bir VM'den, bir tek kullanıcı tarafından atanan kimliği öğesinden kaldırın `identityIds` dizisi.
   
   Aşağıdaki örnek, tüm kullanıcı kimlikleri sistemi tarafından atanan kimliklerle bulunmayan bir VM'den atanan nasıl kaldırmak gösterir:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="related-content"></a>İlgili içerik

- İçin daha geniş bir perspektif yönetilen hizmet kimliği hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

