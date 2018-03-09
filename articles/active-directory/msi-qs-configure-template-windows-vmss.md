---
title: "Bir şablon kullanılarak ayarlanan bir Azure sanal makine ölçek üzerinde MSI yapılandırın"
description: "Azure bir Azure Resource Manager şablonu kullanarak VMSS üzerinde bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım adım yönergeler."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 669348213e00cb338c69156f37779767c681a227
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırın

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği (MSI) Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager dağıtım şablonu kullanarak etkinleştirmek ve bir Azure sanal makine ölçek kümesi için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set-or-an-existing-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi ya da var olan bir Azure sanal makine ölçek kümesi oluşturma sırasında MSI etkinleştir

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Marketi'nden özel şablon](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
   - Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için.  

Belirlediğiniz seçenek ne olursa olsun, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya var olan Azure sanal makine ölçek kümesinde MSI etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları için:

1. Azure için yerel olarak veya Azure portalı üzerinden oturum olsun, sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablonu bir düzenleyicisine yüklendikten sonra bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyicisi bağlı olarak aşağıdaki ekran görüntüsü biraz farklı görünebilir ve yeni bir dağıtım veya varolan bir şablondan düzenlemekte olduğunuz.
   
   ![Şablon - ekran görüntüsü VM bulun](./media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

3. Ekleme `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachineScaleSets"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. Sanal makine ölçek kümesi MSI uzantısı olarak ekleme bir `extensionsProfile` öğesi. Aşağıdaki sözdizimini kullanın:

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

5. İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:

   ![Güncelleştirmeden sonra şablonu ekran görüntüsü](./media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden MSI kaldırma

Artık bir MSI gereken bir sanal makine ölçek kümesi varsa:

1. Azure için yerel olarak veya Azure portalı üzerinden oturum olsun, sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Önceki bölümde eklenen iki öğeleri kaldır: sanal makine ölçek kümesinin `"identity"` ve `"extensionsProfile"` özellikleri.

## <a name="next-steps"></a>Sonraki adımlar

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

