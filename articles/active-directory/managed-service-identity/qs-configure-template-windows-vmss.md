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
ms.openlocfilehash: 64fe217cf3d845e6a09fe67d03648e79e8a4cadd
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
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

Olarak ile Azure portal ve komut dosyası, Azure Resource Manager şablonları bir Azure kaynak grubu tarafından tanımlanan yeni veya değiştirilmiş kaynakları dağıtma yeteneği sağlar. Şablon düzenleme ve dağıtım, hem yerel hem de portal tabanlı dahil olmak üzere birkaç seçenek bulunur:

   - Kullanarak bir [Azure Marketi'nden özel şablon](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), sıfırdan bir şablon oluşturmak veya mevcut bir ortak üzerinde temel olanak sağlayan veya [hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/).
   - Bir şablon herhangi birinden dışa aktararak var olan bir kaynak grubundan türetme [özgün dağıtım](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history), veya [dağıtımının geçerli durumu](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Yerel kullanarak [JSON Düzenleyicisi (örneğin, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md), karşıya yükleme ve PowerShell veya CLI kullanarak dağıtma.
   - Visual Studio kullanarak [Azure kaynak grubu projesi](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) hem oluşturmak ve bir şablonu dağıtmak için.  

Belirlediğiniz seçenek ne olursa olsun, şablon söz dizimi ilk dağıtım ve yeniden dağıtım sırasında aynıdır. Yeni veya var olan bir VM üzerinde MSI etkinleştirme aynı şekilde yapılır. Ayrıca, varsayılan olarak, Azure Resource Manager yaptığı bir [Artımlı güncelleştirme](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımları.

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve bir Azure Resource Manager şablonu kullanarak kimliği atanır sistem devre dışı.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss"></a>Bir Azure VMSS ya da var olan bir Azure VMSS oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

1. Azure için yerel olarak veya Azure portalı üzerinden oturum olsun, sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Şablonu bir düzenleyicisine yüklendikten sonra bulun `Microsoft.Compute/virtualMachineScaleSets` içinde ilgi kaynak `resources` bölümü. Sizin kullandığınız Düzenleyicisi bağlı olarak aşağıdaki ekran görüntüsü biraz farklı görünebilir ve yeni bir dağıtım veya varolan bir şablondan düzenlemekte olduğunuz.
   
   ![Şablon - ekran görüntüsü VM bulun](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-before-vmss.png) 

3. Atanan sistem kimliği etkinleştirmek için add `"identity"` özelliği aynı düzeyde `"type": "Microsoft.Compute/virtualMachineScaleSets"` özelliği. Aşağıdaki sözdizimini kullanın:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (İsteğe bağlı) Sanal makine ölçek kümesi MSI uzantısı olarak ekleme bir `extensionsProfile` öğesi. Azure örneği meta veri hizmeti (IMDS) kimlik de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.  Aşağıdaki sözdizimini kullanın:

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

   ![Güncelleştirmeden sonra şablonu ekran görüntüsü](../media/msi-qs-configure-template-windows-vmss/msi-arm-template-file-after-vmss.png) 

### <a name="disable-a-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi atanan sistem kimlikten devre dışı bırak

> [!NOTE]
> Yönetilen hizmet kimliği, bir sanal makineden devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Artık kümesi bir sanal makine ölçek varsa kimlik atanmış bir sistem gerekiyor ancak hala kimlikleri atanan kullanıcı gerekir:

1. Azure için yerel olarak veya Azure portalı üzerinden oturum olsun, sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

2. Kimlik türünü değiştirme `'UserAssigned'`

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, kimlik ve bir Azure Resource Manager şablonu kullanarak bir Azure VMSS atanmış bir kullanıcı oluşturun.

### <a name="create-and-assign-a-user-assigned-identity-to-an-azure-vmss"></a>Oluşturma ve kimlik için bir Azure VMSS atanmış bir kullanıcı atama

1. İlk adım gerçekleştirmeniz [varolan VMSS üzerinde bir Azure VMSS oluşturulması sırasında atanmış sistem kimliğini etkinleştirme](qs-configure-template-windows-vmss.md#enable-system-assigned-identity-during-creation-of-an-azure-vmss-or-an-existing-azure-vmss).

2. Yapılandırma değişkenleri için Azure VMSS içeren değişkenler bölümü altında aşağıdakine benzer bir kullanıcı atanmış kimlik adı için bir giriş ekleyin.  Bu Azure VMSS oluşturma işlemi sırasında kimliği atanır, kullanıcı değeri tutar:
    
    > [!IMPORTANT]
    > Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

    ```json
    "variables": {
        "vmssPrefix": "vmss",
        "vmssName": "[concat(variables('vmssPrefix'), uniquestring(resourceGroup().id,deployment().name))]",
        //other vm configuration variables...
        "identityName": "[concat(variables('vmssName'), 'id')]"
    ```
3. Altında `resources` öğesi atanan kullanıcı kimliğini oluşturmak için aşağıdaki girişi ekleyin:

    ```json
    {
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
        "name": "[variables('identityName')]",
        "apiVersion": "2015-08-31-PREVIEW",
        "location": "[resourceGroup().location]"
    },
    ```
4. Sonraki altında `resources` öğesi atanan kullanıcı kimliği, VMSS atamak için aşağıdaki girişi ekleyin:

    ```json
    {
        "name": "[variables('vmssName')]",
        "apiVersion": "2017-03-30",
        "location": "[parameters(Location')]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/, variables('identityName'))]"
            ]
        }

    }
    ```
5. (İsteğe bağlı) Altında aşağıdaki girişi ekleyin `extensionProfile` yönetilen Identity uzantısı, VMSS atamak için öğesi. Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. Aşağıdaki sözdizimini kullanın:
   
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
6.  İşiniz bittiğinde, şablonunuz şuna benzer görünmelidir:
    > [!NOTE]
    > Şablon, VMSS oluşturmak için gerekli değişkenleri listelenmez.  `//other configuration variables...` konuyu uzatmamak amacıyla tüm gerekli yapılandırma değişkenleri yerine kullanılır.

      ![Atanan kullanıcı kimliğini ekran görüntüsü](../media/msi-qs-configure-template-windows-vmss/template-vmss-user-assigned-identity.png)

## <a name="next-steps"></a>Sonraki adımlar

- İçin daha geniş bir perspektif MSI hakkında okuyun [yönetilen hizmet Kimliği'ne genel bakış](overview.md).

