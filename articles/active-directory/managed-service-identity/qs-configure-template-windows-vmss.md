---
title: Şablon kullanarak bir Azure sanal makine ölçek üzerinde MSI'yı yapılandırma
description: Azure bir Azure Resource Manager şablonu kullanarak VMSS üzerinde bir yönetilen hizmet kimliği (MSI) yapılandırmak için adım adım yönergeler.
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
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: babeb0eb930d865cf519ddb45c651a3d77265665
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215803"
---
# <a name="configure-a-vmss-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VMSS yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure vmss'de aşağıdaki yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Enable ve disable sistem tarafından atanan bir Azure VMSS kimliği
- Bir kullanıcı tarafından atanan kimliği bir Azure vmss'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) oluşturma bir sanal makine ölçek kümesi ve etkinleştir ve Kaldır sistem ve/veya kullanıcı bir sanal makine ölçek kümesinden yönetilen kimlik atanır.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
   - Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için.  

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Yeni veya mevcut bir VM'deki MSI etkinleştirmesine aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirin ve sistem tarafından atanan bir Azure Resource Manager şablonu kullanarak kimlik devre dışı bırakın.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Azure VMSS ya da mevcut bir Azure VMSS oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

1. Bir düzenleyiciye şablon yüklenemedi, bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyici bağlı olarak, aşağıdaki ekran görüntüsünde biraz farklı görünebilir ve düzenlediğiniz var olan bir ya da yeni bir dağıtım için bir şablon.
   
   ![VM şablonu - ekran görüntüsü bulun](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

2. Sistem tarafından atanan kimliği etkinleştirmek için eklemeniz `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachineScaleSets"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

3. (İsteğe bağlı) Sanal makine ölçek kümesi olarak MSI uzantısı ekleme bir `extensionsProfile` öğesi. Azure örnek meta veri hizmeti (IMDS) kimliğini de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

   >[!NOTE] 
   > Aşağıdaki örnekte, bir Windows sanal makine ölçek kümesi uzantısı varsayılır (`ManagedIdentityExtensionForWindows`) dağıtılıyor. Kullanarak Linux için yapılandırabilirsiniz `ManagedIdentityExtensionForLinux` için bunun yerine, `"name"` ve `"type"` öğeleri.
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

4. İşiniz bittiğinde, şablonunuzu aşağıdakine benzer görünmelidir:

   ![Güncelleştirmeden sonra şablon görüntüsü](../managed-service-identity/media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden bir sistem tarafından atanan kimliği devre dışı

> [!NOTE]
> Bir sanal makineden yönetilen hizmet kimliği devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve atanan kullanıcı kimliklerini kullanma arasında geçiş yapabilirsiniz.

Bir sanal makine ölçek kümesi artık varsa bir sistem tarafından atanan kimlik gerekiyor ancak yine de kullanıcı tarafından atanan kimliklerle gerekir:

- Şablonu bir Düzenleyicisi'ne yüklemek ve kimlik türü için değiştirin `'UserAssigned'`

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, Azure Resource Manager şablonu kullanarak bir Azure VMSS için bir kullanıcı tarafından atanan kimliği atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan kimliği oluşturma için bkz: [bir kullanıcı tarafından atanan kimliği oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Bir kullanıcı tarafından atanan kimliği için bir Azure VMSS atayın

1. Altında `resources` öğesi, bir kullanıcı tarafından atanan kimliği, VMSS'ye atamak için şu girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` oluşturduğunuz kullanıcı tarafından atanan kimlik adı ile.

   > [!Important]
   > `<USERASSIGNEDIDENTITYNAME>` Aşağıdaki örnekte gösterilen değer bir değişkende depolanmalıdır.  Ayrıca, şu anda desteklenen uygulama için bir Resource Manager şablonu bir sanal makinede kullanıcı tarafından atanan kimlikleri atama API sürümü aşağıdaki örnekte sürümle aynı olmalıdır. 

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITY>'))]"
            ]
        }

    }
    ```
2. (İsteğe bağlı) Altında şu girişi ekleyin `extensionProfile` yönetilen kimlik uzantısı, VMSS'ye atamak için öğesi. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. Aşağıdaki sözdizimini kullanın:
   
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
3.  İşiniz bittiğinde, şablonunuzu aşağıdakine benzer görünmelidir:
   
      ![Kullanıcı tarafından atanan kimlik ekran görüntüsü](./media/qs-configure-template-windows-vmss/qs-configure-template-windows-final.PNG)

## <a name="next-steps"></a>Sonraki adımlar

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

