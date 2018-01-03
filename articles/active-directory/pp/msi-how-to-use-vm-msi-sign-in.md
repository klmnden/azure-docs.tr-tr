---
title: "Oturum açmak için bir Azure VM yönetilen hizmet kimliği kullanma"
description: "Adım adım yönergeler ve örnekler kullanarak bir Azure VM MSI hizmet sorumlusu betik istemci için oturum açın ve kaynak için erişim."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 74d732709e1cc3c97b485cc45e3a4e2c8e3cd11e
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Yönetilen hizmet kimliği (MSI) kullanıcı tarafından atanan bir VM kullanarak oturum açın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Bu makale için bir kullanıcı tarafından atanan MSI hizmet sorumlusu ve kılavuz bilgiler hata işleme gibi önemli konularda kullanarak oturum açın CLI komut dosyası örnekleri sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Bu makalede Azure CLI örnekler kullanmak için en son sürümünü yüklediğinizden emin olun [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Bu makaledeki tüm örnek komut dosyası, komut satırı istemcisi bir MSI özellikli sanal makine üzerinde çalışan varsayar. VM "Bağlan" özelliği Azure portalında uzaktan VM'nize bağlanmak için kullanın. Bir VM'de MSI etkinleştirme hakkında daha fazla bilgi için bkz: [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md), veya değişken makaleleri (PowerShell, Portal, bir şablonu veya bir Azure SDK'sını kullanarak). 
> - Oturum açma ve kaynak erişimi sırasında hataları önlemek için MSI en az verilmelidir "Okuyucu" erişim uygun kapsamda (VM veya üzeri) VM üzerinde Azure Resource Manager işlemler izin vermek için. Bkz: [Azure CLI kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişimi atayın](msi-howto-assign-access-cli.md) Ayrıntılar için.

## <a name="overview"></a>Genel Bakış

Bir MSI sağlayan bir [hizmet sorumlusu](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), olduğu [MSI etkinleştirme sırasında oluşturulan](msi-overview.md#how-does-it-work) VM üzerinde. Hizmet sorumlusu Azure kaynaklarına erişim verilir ve bir kimlik olarak oturum açma ve kaynak erişim için komut dosyası/komut-satırı istemcileri tarafından kullanılan. Geleneksel olarak, kendi kimlik altında güvenlikli kaynaklara erişmek için bir komut dosyası istemci gerekir:  

   - kayıtlı ve gizli/web istemci uygulaması olarak Azure AD ile rıza
   - (komut dosyasında olasılığı olan) uygulamanın kimlik bilgilerini kullanarak kendi hizmet sorumlusu altında oturum açın

Bu MSI hizmet sorumlusu altında oturum açarak gibi MSI ile komut dosyası istemci artık, aşağıdakilerden birini gerekir. 

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyası gösterilmektedir nasıl yapılır:

1. Azure AD kullanıcı tarafından atanan MSI hizmet sorumlusu altında oturum açın.  
2. Azure Resource Manager'ı çağırmaz ve bir VM için Azure bölgesi konumu Al. CLI belirteç edinme/kullanımı sizin için otomatik olarak yönetme mvc'deki. VM adınızı değiştirdiğinizden emin olun `<VM NAME>`ve MSI kaynak kimliği için kullanıcı tarafından atanan `<MSI ID>`. MSI kaynak kimliği döndürülür `id` özelliği kullanıcı tarafından atanan bir MSI oluşturulması sırasında (bkz [Azure CLI kullanarak bir VM için bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md) örnekleri için `az identity create` komutu ).

    ```azurecli
    az login -–msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Örnek yanıtları:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, destek Azure AD kimlik doğrulamasının](msi-overview.md#azure-services-that-support-azure-ad-authentication) Azure AD destekleyen ve MSI ile test kaynakları ve bunların ilgili kaynak kimlikleri listesi.

## <a name="error-handling-guidance"></a>Hata işleme Kılavuzu 

Aşağıdaki yanıtlar MSI düzgün yapılandırılmamış olduğunu gösteriyor olabilir:

- CLI: *MSI: 'http://localhost:50342/oauth2/belirteciyle' hatası uygulamasından bir belirteç alınamadı ' HTTPConnectionPool (ana bilgisayar = 'localhost', bağlantı noktası 50342 =)* 

Azure VM ile Bu hatalardan biri alırsanız, dönmek [Azure portal](https://portal.azure.com) ve:

- Git **yapılandırma** sayfasında ve "Yönetilen hizmet kimliği" "Evet" olarak ayarlandığından emin olun
- Git **uzantıları** sayfasında ve başarıyla dağıtılan MSI uzantısı emin olun.

Ya da yanlışsa, kaynağa MSI yeniden atamayı veya dağıtım hatası sorun giderme gerekebilir. Bkz: [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md) VM yapılandırması yardıma ihtiyacınız varsa.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM'de MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](msi-qs-configure-cli-windows-vm.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.








