---
title: Bir şablon kullanılarak ayarlanan bir Azure sanal makine ölçek üzerinde MSI yapılandırın
description: Azure bir Azure Resource Manager şablonu kullanarak VMSS üzerinde bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: b95f5bb2aa93fb29999994ccd83dc898f88f1072
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VMSS yönetilen hizmet kimliği yapılandırın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure VMSS aşağıdaki yönetilen hizmet kimliği işlemleri gerçekleştirmek nasıl öğrenin:
- Etkinleştirme ve bir Azure VMSS kimliğini atanan sistem devre dışı
- Ekleme ve bir Azure VMSS kimliğini atanmış bir kullanıcı kaldırma

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Marketi'nden özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
   - Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için.  

Belirlediğiniz seçenek ne olursa olsun, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya var olan bir VM üzerinde MSI etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve bir Azure Resource Manager şablonu kullanarak kimliği atanır sistem devre dışı.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Bir Azure VMSS ya da var olan bir Azure VMSS oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

1. Bir düzenleyicisine yük şablon, bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyicisi bağlı olarak aşağıdaki ekran görüntüsü biraz farklı görünebilir ve yeni bir dağıtım veya varolan bir şablondan düzenlemekte olduğunuz.
   
   ![Şablon - ekran görüntüsü VM bulun](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Atanan sistem kimliği etkinleştirmek için add `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachineScaleSets"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (İsteğe bağlı) Sanal makine ölçek kümesi MSI uzantısı olarak ekleme bir `extensionsProfile` öğesi. Azure örneği meta veri hizmeti (IMDS) kimlik de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

   >[!NOTE] 
   > Aşağıdaki örnek bir Windows sanal makine ölçek kümesi uzantısı varsayar (`ManagedIdentityExtensionForWindows`) dağıtılıyor. Kullanarak Linux için yapılandırabilirsiniz `ManagedIdentityExtensionForLinux` için bunun yerine, `"name"` ve `"type"` öğeleri.
   >

   ```JSON
   "extensionProfile": {
        "extensions": [
            {
                "name": "MSIWindowsExtension",
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

4. İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:

   ![Güncelleştirmeden sonra şablonu ekran görüntüsü](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi atanan sistem kimlikten devre dışı bırak

> [!NOTE]
> Yönetilen hizmet kimliği, bir sanal makineden devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Artık kümesi bir sanal makine ölçek varsa kimlik atanmış bir sistem gerekiyor ancak hala kimlikleri atanan kullanıcı gerekir:

- Şablonu bir düzenleyicisine yüklemek ve kimlik türünü değiştirme `'UserAssigned'`

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, Azure Resource Manager şablonu kullanarak bir Azure VMSS atanan kullanıcı kimliğini atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan kimlik oluşturmak için bkz: [atanan kullanıcı kimliğini oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Bir Azure VMSS kimlik atanan kullanıcı atama

1. Altında `resources` öğesi, bir kullanıcı tarafından atanan kimlik bilgilerinizi VMSS atamak için aşağıdaki girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` atanan kullanıcı kimliğini adı ile oluşturduğunuz.

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/<USERASSIGNEDIDENTITY>)']"
            ]
        }

    }
    ```
2. (İsteğe bağlı) Altında aşağıdaki girişi ekleyin `extensionProfile` yönetilen Identity uzantısı, VMSS atamak için öğesi. Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. Aşağıdaki sözdizimini kullanın:
   
    ```JSON
       "extensionProfile": {
            "extensions": [
                {
                    "name": "MSIWindowsExtension",
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
3.  İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:
   
      ![Atanan kullanıcı kimliğini ekran görüntüsü](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

## <a name="next-steps"></a>Sonraki adımlar

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

