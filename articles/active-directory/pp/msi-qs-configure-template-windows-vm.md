---
title: Azure şablonu kullanarak bir Azure VM için bir kullanıcı tarafından atanan MSI yapılandırma
description: Bir Azure Resource Manager şablonu kullanarak Azure VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
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
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e01e4c397e0d0a19280a32fc1e8341b57b47e4eb
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610392"
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-an-azure-template"></a>Azure şablonu kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve bir Azure Resource Manager dağıtım şablonu kullanarak Azure VM için kullanıcı tarafından atanan bir MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Bir Azure VM veya varolan bir VM'yi oluşturma sırasında MSI etkinleştir

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma olanağı sunar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Market'ten özel şablon](~/articles/azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturun veya mevcut bir ortak üzerinde temel olanak tanıyan veya [Hızlı Başlangıç şablonu](https://azure.microsoft.com/documentation/templates/).
   - Mevcut bir kaynak grubundan'nden ya da bir şablon vererek türetme [özgün dağıtım](~/articles/azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](~/articles/azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel bir kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](~/articles/azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve ardından PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](~/articles/azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturma hem de bir şablonu dağıtmak için.  

Belirlediğiniz seçeneğe bakılmaksızın, şablon söz dizimi ilk dağıtımı ve yeniden dağıtma işlemi sırasında aynıdır. Oluşturma ve kullanıcı tarafından atanan bir MSI için yeni veya mevcut bir VM atama aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](~/articles/azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımlar için:

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın MSI ve VM içerir. Ayrıca hesabınızı sunan bir role ait olduğundan emin olun abonelik veya kaynakları (örneğin, "sahip" rolü) üzerinde yazma izinleri.

2. Şablonu bir düzenleyiciye yüklendikten sonra bulma `Microsoft.Compute/virtualMachines` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyici bağlı olarak, aşağıdaki ekran görüntüsünde biraz farklı görünebilir ve düzenlediğiniz var olan bir ya da yeni bir dağıtım için bir şablon.

   >[!NOTE] 
   > Bu örnek gibi değişkenleri varsayar `vmName`, `storageAccountName`, ve `nicName` şablonda tanımlanmadı.
   >

   ![VM şablonu - ekran görüntüsü bulun](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Ekleme `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachines"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. VM MSI bir uzantısı olarak eklersiniz bir `resources` öğesi. Aşağıdaki sözdizimini kullanın:

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

   ![Güncelleştirmeden sonra şablon görüntüsü](~/articles/active-directory/media/msi-qs-configure-template-windows-vm/template-file-after.png) 

## <a name="remove-msi-from-an-azure-vm"></a>Bir Azure VM'den MSI Kaldır

Artık bir MSI gereken bir VM'niz varsa:

1. Azure'da yerel olarak oturum açın ya da Azure portal aracılığıyla Azure aboneliği ile ilişkili olan bir hesap kullanın, VM içerir. Ayrıca hesabınızı sunan bir role ait olduğundan emin olun (örneğin, "Sanal makine Katılımcısı" rolünü) VM üzerinde yazma izinleri.

2. Önceki bölümde eklenmiş olan iki öğe Kaldır: sanal makinenin `"identity"` özelliği ve `"Microsoft.Compute/virtualMachines/extensions"` kaynak.

## <a name="related-content"></a>İlgili içerik

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).

