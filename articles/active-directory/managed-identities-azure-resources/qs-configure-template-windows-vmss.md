---
title: Azure kaynakları için yönetilen kimlikleri bir şablon kullanarak sanal makine ölçek kümesi üzerinde yapılandırma
description: Bir Azure Resource Manager şablonu kullanarak Azure kaynakları için yönetilen kimlikleri üzerinde bir sanal makine ölçek yapılandırma için adım adım yönergeler ayarlayın.
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
ms.date: 02/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6ecbac8af86c3c2c76b7710eb61f71481b86291b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59009879"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-virtual-machine-scale-using-a-template"></a>Azure kaynakları için yönetilen kimlikleri bir şablonu kullanarak bir Azure sanal makine ölçek üzerinde yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure sanal makine ölçek kümesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğrenin:
- Etkinleştirme ve devre dışı bir Azure sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik bir Azure sanal makine ölçek kümesinde ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:

    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.

    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturun ve etkinleştirin ve sistem ve/veya yönetilen kimlik kullanıcı tarafından atanan bir sanal makine ölçek kümesinden kaldırmak için.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) gelen ve sanal makine ölçek kümesi yönetilen kimliği atamak ve bir kullanıcı tarafından atanan kaldırmak için rol.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure ile gibi portal ve komut dosyası, [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) şablonları, bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Market'ten özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
   - Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates), veya [dağıtımının geçerli durumu](../../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
   - Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için.  

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Yeni veya mevcut bir VM üzerindeki Azure kaynakları için yönetilen kimlikleri etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/deployment-modes.md) dağıtımları.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirin ve bir Azure Resource Manager şablonu kullanarak sistem tarafından atanan yönetilen kimliği devre dışı.

### <a name="enable-system-assigned-managed-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-an-existing-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği oluşturma sırasında bir sanal makine ölçek kümesi ya da mevcut bir sanal makine ölçek kümesi oluşturma yönetilen etkinleştir

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.
2. Sistem tarafından atanan bir yönetilen kimlik etkinleştirmek için şablon bir düzenleyiciye yüklenemedi, bulun `Microsoft.Compute/virtualMachinesScaleSets` kaynaklar içinde ilgi kaynak bölümü ve ekleme `identity` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachinesScaleSets"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

> [!NOTE]
> Azure kaynaklarını sanal makine ölçek kümesi uzantısını içinde belirterek için isteğe bağlı olarak yönetilen kimlikleri sağlamanız `extensionProfile` şablonu öğesidir. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Daha fazla bilgi için [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).


4. İşiniz bittiğinde, aşağıdaki bölümlerde şablonunuzun kaynak bölümüne eklenen ve aşağıdakine benzemelidir:

   ```json
    "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
            },
           "properties": {
                //other resource provider properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ``` 

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan yönetilen bir kimlik bir Azure sanal makine ölçek kümesinden devre dışı bırak

Artık sistem tarafından atanan bir yönetilen kimlik gereken sanal makine ölçek kümesi varsa:

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Yalnızca yönetilen kimlik sistemi tarafından atanmış olan bir sanal makine varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   Varsa, apiVersion `2018-06-01` ve sanal makinenizin sistem ve kullanıcı tarafından atanan yönetilen kimlikleri kaldırmak `SystemAssigned` kimlik türü ve canlı `UserAssigned` Userassignedıdentities sözlük değerlerin yanı sıra.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   Varsa, apiVersion `2017-12-01` ve sistem ve kullanıcı tarafından atanan yönetilen kimlikleri kaldırmak, sanal makine ölçek kümesi sahip `SystemAssigned` kimlik türü ve canlı `UserAssigned` ile birlikte `identityIds` kullanıcı tarafından atanan yönetilen dizi kimlik. 
   
    

   Aşağıdaki örnek, bir sanal makine ölçek kümesi hiçbir kullanıcı tarafından atanan yönetilen kimliklerle gelen sistem tarafından atanan bir yönetilen kimlik nasıl kaldırmak gösterir:
   
   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }

   }
   ```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager şablonu kullanarak sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan yönetilen kimlik oluşturmak için bkz [kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

### <a name="assign-a-user-assigned-managed-identity-to-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir sanal makine ölçek kümesine atama

1. Altında `resources` öğesi, kullanıcı tarafından atanan bir yönetilen kimlik, sanal makine ölçek kümesine atamak için şu girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` kullanıcı tarafından atanan adı ile yönetilen oluşturduğunuz kimliği.
   
   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   Varsa, apiVersion `2018-06-01`, kullanıcı tarafından atanan yönetilen kimliklerinizi depolanır `userAssignedIdentities` sözlük biçimi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, tanımlı bir değişkende `variables` şablonunuzun bölümü.

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
       }
    
   }
   ```   

   **Microsoft.Compute/virtualMachineScaleSets API Sürüm 2017-12-01**
    
   Varsa, `apiVersion` olduğu `2017-12-01` veya kullanıcı tarafından atanan yönetilen kimliklerinizi daha önce depolanan `identityIds` dizi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, şablonunuzun değişkenler bölümünde tanımlanmış bir değişkende.

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
> [!NOTE]
> Azure kaynaklarını sanal makine ölçek kümesi uzantısını içinde belirterek için isteğe bağlı olarak yönetilen kimlikleri sağlamanız `extensionProfile` şablonu öğesidir. Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Daha fazla bilgi için [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

3. İşiniz bittiğinde, şablonunuzu aşağıdakine benzer görünmelidir:
   
   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**   

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```

   **Microsoft.Compute/virtualMachines API Sürüm 2017-12-01**

   ```json
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "name": "[variables('vmssName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "UserAssigned",
                "identityIds": [
                    "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            },
           "properties": {
                //other virtual machine properties...
                "virtualMachineProfile": {
                    //other virtual machine profile properties...
                    //The following appears only if you provisioned the optional virtual machine scale set extension (to be deprecated)    
                    "extensionProfile": {
                        "extensions": [
                            {
                                "name": "ManagedIdentityWindowsExtension",
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
                    }
                }
            }
        }
    ]
   ```
   ### <a name="remove-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Yönetilen kimlik kullanıcı tarafından atanan bir Azure sanal makine ölçek kümesinden Kaldır

Artık bir kullanıcı tarafından atanan bir yönetilen kimlik gereken sanal makine ölçek kümesi varsa:

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Yalnızca kullanıcı tarafından atanan yönetilen kimlik olan bir sanal makine ölçek kümesi varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.

   Aşağıdaki örnek hiçbir sistem tarafından atanan yönetilen kimliklerle bir VM'den nasıl kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırmak gösterir:

   ```json
   {
       "name": "[variables('vmssName')]",
       "apiVersion": "2018-06-01",
       "location": "[parameters(Location')]",
       "identity": {
           "type": "None"
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**
    
   Bir sanal makine ölçek kümesinden tek bir kullanıcı tarafından atanan yönetilen kimlik kaldırmak için oradan kaldırın `userAssignedIdentities` sözlüğü.

   Bir sistem tarafından atanan kimliği varsa, bunu tutmak içinde `type` altındaki `identity` değeri.

   **Microsoft.Compute/virtualMachineScaleSets API Sürüm 2017-12-01**

   Bir sanal makine ölçek kümesinden tek bir kullanıcı tarafından atanan yönetilen kimlik kaldırmak için oradan kaldırın `identityIds` dizisi.

   Sistem tarafından atanan bir yönetilen kimlik varsa, bunu tutmak içinde `type` altındaki `identity` değeri.
   
## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen Azure kaynaklarına genel bakış için kimlikleri](overview.md).

