---
title: Oturum açma için bir Azure VM yönetilen hizmet Kimliği'ni kullanma
description: Adım adım yönergeler ve örnekler kullanarak bir Azure VM MSI hizmet sorumlusu istemci komut dosyası için oturum açın ve kaynak erişin.
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
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 205938bbf615face0768028717a333c13c1fafa1
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590322"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Oturum açma için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, oturum açma için hata işleme gibi önemli konularda bir MSI hizmet sorumlusu ve Kılavuzlar kullanarak PowerShell ve CLI betiği örnekleri sağlar.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell veya Azure CLI örnekleri kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) veya [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Bu makaledeki tüm örnek betik, komut satırı istemcisi bir MSI etkin sanal makinede çalışan varsayar. VM "Bağlan" özelliği Azure Portalı'nda uzaktan VM'nize bağlanmak için kullanın. Bir VM'deki MSI etkinleştirme hakkında daha fazla bilgi için bkz [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md), veya değişken makaleleri (PowerShell, CLI, bir şablon veya bir Azure SDK'sını kullanarak). 
> - Kaynak erişim sırasında hataları önlemek için VM MSI en az verilmelidir "Okuyucu" uygun kapsamında erişim (VM veya üzeri) Azure Resource Manager işlemlerini VM'de izin vermek için. Bkz: [Azure portalını kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişim atama](howto-assign-access-portal.md) Ayrıntılar için.

## <a name="overview"></a>Genel Bakış

Bir MSI sağlayan bir [hizmet sorumlusu nesnesi](../develop/developer-glossary.md#service-principal-object) , olduğu [MSI etkinleştirme sırasında oluşturulan](overview.md#how-does-it-work) VM üzerinde. Hizmet sorumlusunu Azure kaynaklarına erişim verilir ve bir kimlik olarak komut dosyası/komut satırı istemcileri için oturum açma ve kaynak erişimi tarafından kullanılan. Geleneksel olarak, kendi kimliği altında güvenlikli kaynaklara erişmek için bir betik istemci gerekir:  

   - kayıtlı ve istemci gizli/web uygulaması olarak Azure AD ile onaylı
   - (Bu betikte ekli olasıdır) bir uygulamanın kimlik bilgilerini kullanarak, hizmet sorumlusu altında oturum açın

Bu MSI hizmet sorumlusu altında oturum açarak gibi MSI ile betik istemcinizi bunlardan herhangi birini yapmak artık gerekir. 

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyasını gösterir nasıl yapılır:

1. Sanal makinenin MSI hizmet sorumlusu altında Azure AD'de oturum açın  
2. Azure Resource Manager'ı çağırın ve sanal makinenin hizmet sorumlusu kimliğini alın CLI belirteç edinme/kullanmak sizin için otomatik olarak yönetme üstlenir. İçin sanal makine adı yerine mutlaka `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut dosyasını gösterir nasıl yapılır:

1. Sanal makinenin MSI hizmet sorumlusu altında Azure AD'de oturum açın  
2. VM hakkında bilgi almak için bir Azure Resource Manager cmdlet'i çağırın. PowerShell belirteci kullanmak sizin için otomatik olarak yönetme üstlenir.  

   ```azurepowershell
   Add-AzureRmAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri söz konusu destek Azure AD kimlik doğrulamasını](services-support-msi.md#azure-services-that-support-azure-ad-authentication) için MSI ile test edilmiştir ve Azure AD Destek kaynakları ve bunların ilgili kaynak kimlikleri listesi.

## <a name="error-handling-guidance"></a>Hata işleme yönergeleri 

Yanıt aşağıdaki gibi sanal makinenin MSI değil doğru şekilde yapılandırıldığını gösterebilir:

- PowerShell: *Invoke-WebRequest: uzak sunucuya bağlanılamıyor*
- CLI: *MSI: uygulamasından bir belirteç alınamadı. 'http://localhost:50342/oauth2/token' hata nedeniyle ' HTTPConnectionPool (konak 'localhost', bağlantı noktası = 50342 =)* 

Azure VM ile Bu hatalardan birini alırsanız, dönmek [Azure portalında](https://portal.azure.com) ve:

- Git **yapılandırma** sayfasında ve "Yönetilen hizmet kimliği" "Evet" olarak ayarlandığından emin olun
- Git **uzantıları** sayfasında ve MSI uzantı başarıyla dağıtıldığından emin olun.

Ya da yanlışsa kaynağınızda MSI yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir. Bkz: [bir VM yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vm.md) VM yapılandırması ile ilgili yardıma ihtiyacınız varsa.

## <a name="related-content"></a>İlgili içerik

- Azure VM'deki MSI etkinleştirmek için bkz: [bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](qs-configure-powershell-windows-vm.md), veya [bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](qs-configure-cli-windows-vm.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.








