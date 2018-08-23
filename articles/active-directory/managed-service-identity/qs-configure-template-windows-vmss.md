---
title: Şablon kullanarak bir Azure sanal makine ölçek üzerinde yönetilen hizmet kimliği yapılandırma
description: Azure bir Azure Resource Manager şablonu kullanarak VMSS üzerinde bir yönetilen hizmet kimliği yapılandırmaya ilişkin adım adım yönergeler.
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
ms.openlocfilehash: 68304b3e5eea50aba28f46344abcbd7ad060c5c8
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056252"
---
# <a name="configure-managed-service-identity-on-virtual-machine-scale-using-a-template"></a>Şablon kullanarak sanal makine ölçek üzerinde yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak bir Azure sanal makine ölçek kümesinde aşağıdaki yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Etkinleştirme ve devre dışı bir Azure sanal makine ölçek kümesinde sistem tarafından atanan kimlik
- Bir Azure sanal makine ölçek kümesinde bir kullanıcı tarafından atanan kimliği ekleyip

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

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Yeni veya mevcut bir VM üzerinde yönetilen hizmet kimliği etkinleştiriliyor aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/deployment-modes.md) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirin ve sistem tarafından atanan bir Azure Resource Manager şablonu kullanarak kimlik devre dışı bırakın.

### <a name="enable-system-assigned-identity-during-creation-the-creation-of-a-virtual-machines-scale-set-or-a-existing-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi ya da mevcut bir sanal makine ölçek kümesi oluşturma oluşturma sırasında sistem tarafından atanan kimlik etkinleştir

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.
   
2. Sistem tarafından atanan kimliği etkinleştirmek için şablon bir Düzenleyicisi'ne yüklemek, bulun `Microsoft.Compute/virtualMachinesScaleSets` kaynaklar içinde ilgi kaynak bölümü ve ekleme `identity` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   }
   ```

3. (İsteğe bağlı) Sanal makine ölçek kümesi uzantısı olarak yönetilen hizmet kimliği eklemek bir `extensionsProfile` öğesi. Azure örnek meta veri hizmeti (IMDS) kimliğini de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

   >[!NOTE] 
   > Aşağıdaki örnekte, bir Windows sanal makine ölçek kümesi uzantısı varsayılır (`ManagedIdentityExtensionForWindows`) dağıtılıyor. Kullanarak Linux için yapılandırabilirsiniz `ManagedIdentityExtensionForLinux` için bunun yerine, `"name"` ve `"type"` öğeleri.
   >

   ```json
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

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden bir sistem tarafından atanan kimliği devre dışı

Bir sanal makine ölçek kümesi artık varsa bir yönetilen hizmet kimliği gerekir:

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Yalnızca sistem tarafından atanan kimliği sahip bir VM varsa, bu kimlik türü için değiştirerek devre dışı bırakabilirsiniz `None`.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   ApiVersion ise `2018-06-01` ve Makinenizin sistem ve kullanıcı tarafından atanan kimliklerle sahip, Kaldır `SystemAssigned` kimlik türü ve canlı `UserAssigned` Userassignedıdentities sözlük değerlerin yanı sıra.

   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01 ve önceki sürümleri**

   Varsa, apiVersion `2017-12-01` ve sistem ve kullanıcı tarafından atanan kimliklerle, sanal makine ölçek kümesine sahiptir, Kaldır `SystemAssigned` kimlik türü ve canlı `UserAssigned` ile birlikte `identityIds` kullanıcı tarafından atanan kimlikleri dizisi. 
   
    

   Aşağıdaki örnek, sanal makine ölçek kümesi tarafından atanan kimliklerle hiçbir kullanıcıyla gelen kimlik atanmış bir sistemde nasıl kaldırmak gösterir:
   
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

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure Resource Manager şablonu kullanarak bir sanal makine ölçek kümesine atayın.

> [!Note]
> Bir Azure Resource Manager şablonu kullanarak bir kullanıcı tarafından atanan kimliği oluşturma için bkz: [bir kullanıcı tarafından atanan kimliği oluşturma](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity).

### <a name="assign-a-user-assigned-identity-to-an-azure-vmss"></a>Bir kullanıcı tarafından atanan kimliği için bir Azure VMSS atayın

1. Altında `resources` öğesi, bir kullanıcı tarafından atanan kimliği, sanal makine ölçek kümesine atamak için şu girişi ekleyin.  Değiştirdiğinizden emin olun `<USERASSIGNEDIDENTITY>` oluşturduğunuz kullanıcı tarafından atanan kimlik adı ile.
   
   **Microsoft.Compute/virtualMachineScaleSets API sürümü 2018-06-01**

   ApiVersion ise `2018-06-01`, atanan kullanıcı kimliklerinizi depolanan `userAssignedIdentities` sözlük biçimi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, tanımlı bir değişkende `variables` şablonunuzun bölümü.

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
    
   Varsa, `apiVersion` olduğu `2017-12-01` veya atanan kullanıcı kimliklerinizi daha önce depolanan `identityIds` dizi ve `<USERASSIGNEDIDENTITYNAME>` değeri depolanan, şablonunuzun değişkenler bölümünde tanımlanmış bir değişkende.

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

   **Daha önce Microsoft.Compute/virtualMachines API Sürüm 2017-12-01 eand**

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
### <a name="remove-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Atanan kullanıcı kimliğini bir Azure sanal makine ölçek kümesinden kaldırın.

Bir sanal makine ölçek kümesi artık varsa bir yönetilen hizmet kimliği gerekir:

1. Azure'da yerel olarak veya Azure portalında oturum açın, sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablona yük bir [Düzenleyicisi](#azure-resource-manager-templates) bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Yalnızca kullanıcı tarafından atanan kimlik olan bir sanal makine ölçek kümesi varsa, bunu değiştirerek devre dışı bırakabilirsiniz kimlik türü için `None`.

   Aşağıdaki örnek, tüm kullanıcı kimlikleri sistemi tarafından atanan kimliklerle bulunmayan bir VM'den atanan nasıl kaldırmak gösterir:

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
    
   Kaldırmak için bir sanal makine ölçek kümesi, bir tek kullanıcı tarafından atanan kimlik öğesinden kaldırın `userAssignedIdentities` sözlüğü.

   Bir sistem tarafından atanan kimliği varsa, bunu tutmak içinde `type` altındaki `identity` değeri.

   **Microsoft.Compute/virtualMachineScaleSets API Sürüm 2017-12-01**

   Bir tek kullanıcı tarafından atanan kimliği bir sanal makine ölçek kümesinden kaldırmak için oradan kaldırın `identityIds` dizisi.

   Bir sistem tarafından atanan kimliği varsa, bunu tutmak içinde `type` altındaki `identity` değeri.
   
## <a name="next-steps"></a>Sonraki adımlar

- İçin daha geniş bir perspektif yönetilen hizmet kimliği hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

