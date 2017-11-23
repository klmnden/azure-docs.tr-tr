---
title: "Bir şablonu kullanarak bir Azure VM'deki MSI yapılandırma"
description: "Azure bir Azure Resource Manager şablonu kullanarak bir VM, bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım adım yönergeler."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: 014a9c814e91e7d0677b7ea1d994c81384719548
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Bir şablonu kullanarak bir VM yönetilen hizmet kimliği yapılandırın

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği (MSI) Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirme ve Azure bir Azure Resource Manager dağıtım şablonu kullanarak VM için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Bir Azure VM veya mevcut bir VM'yi oluşturma sırasında MSI etkinleştir

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Marketi'nden özel şablon](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
   - Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için.  

Belirlediğiniz seçenek ne olursa olsun, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya var olan bir VM üzerinde MSI etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları için:

1. Azure'da yerel olarak oturum açın veya Azure portalı üzerinden Azure aboneliğiyle ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolü) VM üzerinde yazma izinleri.

2. Şablonu bir düzenleyicisine yüklendikten sonra bulun `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyicisi bağlı olarak aşağıdaki ekran görüntüsü biraz farklı görünebilir ve yeni bir dağıtım veya varolan bir şablondan düzenlemekte olduğunuz.

   >[!NOTE] 
   > Bu örnek değişkenler gibi varsayar `vmName`, `storageAccountName`, ve `nicName` şablonda tanımlı.
   >

   ![Şablon - ekran görüntüsü VM bulun](./media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Ekleme `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. VM MSI uzantısı olarak ekleme bir `resources` öğesi. Aşağıdaki sözdizimini kullanın:

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

   ![Güncelleştirmeden sonra şablonu ekran görüntüsü](./media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>MSI bir Azure sanal makineden kaldırın

Bir VM'niz varsa, artık bir MSI gerekir:

1. Azure'da yerel olarak oturum açın veya Azure portalı üzerinden Azure aboneliğiyle ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolü) VM üzerinde yazma izinleri.

2. Önceki bölümde eklenen iki öğeleri kaldır: VM `"identity"` özelliği ve `"Microsoft.Compute/virtualMachines/extensions"` kaynak.

## <a name="related-content"></a>İlgili içerik

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

