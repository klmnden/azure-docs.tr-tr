---
title: Bir şablonu kullanarak bir Azure VM'deki MSI yapılandırma
description: Azure bir Azure Resource Manager şablonu kullanarak bir VM, bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım adım yönergeler.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 05859187a5734d982b750e287c3ecd375ed1da2f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723754"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure VM aşağıdaki yönetilen hizmet kimliği işlemleri gerçekleştirmek nasıl öğrenin:

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Marketi'nden özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
   - Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için.  

Belirlediğiniz seçenek ne olursa olsun, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Bir sistem veya kullanıcı kimliği yeni veya var olan bir VM üzerinde atanan etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve bir Azure Resource Manager şablonu kullanarak kimlik atanmış bir sistemi devre dışı.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Bir Azure VM veya mevcut bir VM'yi oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

1. Azure'da yerel olarak oturum açın veya Azure portalı üzerinden Azure aboneliğiyle ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolü) VM üzerinde yazma izinleri.

2. Şablonu bir düzenleyicisine yüklendikten sonra bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyicisi bağlı olarak aşağıdaki ekran görüntüsü biraz farklı görünebilir ve yeni bir dağıtım veya varolan bir şablondan düzenlemekte olduğunuz.

   >[!NOTE] 
   > Bu örnek değişkenler gibi varsayar `vmName`, `storageAccountName`, ve `nicName` şablonda tanımlı.
   >

   ![Şablon - ekran görüntüsü VM bulun](../media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Kimliği atanır sistem etkinleştirmek için add `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (İsteğe bağlı) VM MSI uzantısı olarak ekleme bir `resources` öğesi. Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

   >[!NOTE] 
   > Aşağıdaki örnek bir Windows VM uzantısı varsayar (`ManagedIdentityExtensionForWindows`) dağıtılıyor. Kullanarak Linux için yapılandırabilirsiniz `ManagedIdentityExtensionForLinux` için bunun yerine, `"name"` ve `"type"` öğeleri.
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

5. İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:

   ![Güncelleştirmeden sonra şablonu ekran görüntüsü](../media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Bir Azure sanal makineden kimlik atanmış bir sistemi devre dışı bırak

> [!NOTE]
> Yönetilen hizmet kimliği, bir sanal makineden devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Bir VM'niz varsa, artık bir yönetilen hizmet kimliği gerekir:

1. Azure'da yerel olarak oturum açın veya Azure portalı üzerinden Azure aboneliğiyle ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolü) VM üzerinde yazma izinleri.

2. Kimlik türü değiştirme `UserAssigned`.

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, Azure Resource Manager şablonu kullanarak bir Azure VM'ye atanan kullanıcı kimliğini atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan kimlik oluşturmak için bkz: [atanan kullanıcı kimliğini oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Bir Azure VM kimlik atanan kullanıcı atama

1. Altında `resources` öğesi, bir kullanıcı tarafından atanan kimlik VM'nize atamak için aşağıdaki girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` atanan kullanıcı kimliğini adı ile oluşturduğunuz.
    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
            ]
        },
    ```
    
2. (İsteğe bağlı) Sonraki altında `resources` öğesi, yönetilen Identity uzantısı VM'nize atamak için aşağıdaki girişi ekleyin. Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. Aşağıdaki sözdizimini kullanın:
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
    
3.  İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:

      ![Atanan kullanıcı kimliğini ekran görüntüsü](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)


## <a name="related-content"></a>İlgili içerik

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

