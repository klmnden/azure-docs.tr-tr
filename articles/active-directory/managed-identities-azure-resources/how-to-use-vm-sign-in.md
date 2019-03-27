---
title: Azure VM'de oturum açma için Azure kaynakları için yönetilen kimliklerini kullanma
description: Adım adım yönergeler ve örnekler için bir Azure VM kullanarak betiği istemci oturum açma ve kaynak erişimi için Azure kaynaklarını hizmet sorumlusu için kimlikleri yönetilen.
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
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43aa0859fa67cc6b2f5c5974f072e7b6d4b29527
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58442141"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-for-sign-in"></a>Azure VM'de oturum açma için Azure kaynakları için yönetilen kimliklerini kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Bu makalede, Azure kaynaklarını hizmet sorumlusu ve hata işleme gibi önemli konular kılavuzu için yönetilen kimliklerle oturum için PowerShell ve CLI betiği örnekleri sağlar.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Bu makalede Azure PowerShell veya Azure CLI örnekleri kullanmayı planlıyorsanız, en son sürümünü yüklediğinizden emin olun [Azure PowerShell](/powershell/azure/install-az-ps) veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Bu makaledeki tüm örnek betik, komut satırı istemcisi etkinleştirilmiş Azure kaynakları için yönetilen kimliklerle bir VM'de çalışan varsayar. VM "Bağlan" özelliği Azure Portalı'nda uzaktan VM'nize bağlanmak için kullanın. Bir VM'de Azure kaynakları için yönetilen kimlikleri etkinleştirme hakkında daha fazla bilgi için bkz [yapılandırma kimlikleri Azure portalını kullanarak bir VM üzerindeki Azure kaynakları için yönetilen](qs-configure-portal-windows-vm.md), ya da (PowerShell, CLI, bir şablon veya bir Azure kullanarak değişken makalelerden birine SDK'SI). 
> - Kaynak erişim sırasında hataları önlemek için sanal makinenin yönetilen kimlik en az verilmelidir "Okuyucu" uygun kapsamında erişim (VM veya üzeri) Azure Resource Manager işlemlerini VM'de izin vermek için. Bkz: [Ata yönetilen Azure kaynaklarına erişim için Azure portalını kullanarak kaynak kimlikleri](howto-assign-access-portal.md) Ayrıntılar için.

## <a name="overview"></a>Genel Bakış

Azure kaynakları için yönetilen kimlikleri sağlayan bir [hizmet sorumlusu nesnesi](../develop/developer-glossary.md#service-principal-object) , olduğu [Azure kaynakları için yönetilen kimlikleri etkinleştirme sırasında oluşturulan](overview.md#how-does-it-work) VM üzerinde. Hizmet sorumlusunu Azure kaynaklarına erişim verilir ve bir kimlik olarak komut dosyası/komut satırı istemcileri için oturum açma ve kaynak erişimi tarafından kullanılan. Geleneksel olarak, kendi kimliği altında güvenlikli kaynaklara erişmek için bir betik istemci gerekir:  

   - kayıtlı ve istemci gizli/web uygulaması olarak Azure AD ile onaylı
   - (Bu betikte ekli olasıdır) bir uygulamanın kimlik bilgilerini kullanarak, hizmet sorumlusu altında oturum açın

Bu hizmet sorumlusunu Azure kaynakları için yönetilen kimlik altında oturum gibi Azure kaynakları için yönetilen kimliklerle betik istemcinizi bunlardan herhangi birini yapmak artık gerekir. 

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki komut dosyasını gösterir nasıl yapılır:

1. Azure kaynaklarını hizmet sorumlusu için sanal makinenin yönetilen kimlik altında Azure AD'de oturum açın  
2. Azure Resource Manager'ı çağırın ve sanal makinenin hizmet sorumlusu kimliğini alın CLI belirteç edinme/kullanmak sizin için otomatik olarak yönetme üstlenir. İçin sanal makine adı yerine mutlaka `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The managed identity for Azure resources service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Aşağıdaki komut dosyasını gösterir nasıl yapılır:

1. Azure kaynaklarını hizmet sorumlusu için sanal makinenin yönetilen kimlik altında Azure AD'de oturum açın  
2. VM hakkında bilgi almak için bir Azure Resource Manager cmdlet'i çağırın. PowerShell belirteci kullanmak sizin için otomatik olarak yönetme üstlenir.  

   ```azurepowershell
   Add-AzAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's managed identity for Azure resources. 
   $vmInfoPs = Get-AzVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The managed identity for Azure resources service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Azure Hizmetleri için kaynak kimlikleri

Bkz: [Azure Hizmetleri, desteği Azure AD kimlik doğrulaması](services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) Azure AD'ye destekleyen ve Azure kaynaklarını ve onların ilgili kaynak kimlikleri için yönetilen kimliklerle test kaynaklar listesi.

## <a name="error-handling-guidance"></a>Hata işleme yönergeleri 

Yanıt aşağıdaki gibi Azure kaynakları için sanal makinenin yönetilen kimlik değil doğru şekilde yapılandırıldığını gösterebilir:

- PowerShell: *Invoke-WebRequest: Uzak sunucuya bağlanılamıyor*
- CLI: *MSI: Uygulamasından bir belirteç alınamadı `http://localhost:50342/oauth2/token` hata nedeniyle ' HTTPConnectionPool (konak 'localhost', bağlantı noktası = 50342 =)* 

Azure VM ile Bu hatalardan birini alırsanız, dönmek [Azure portalında](https://portal.azure.com) ve:

- Git **kimlik** sayfasında ve olun **sistem tarafından atanan** "Evet" olarak ayarlayın
- Git **uzantıları** sayfasında ve uzantı Azure kaynakları için yönetilen kimlikleri sağlamak **(Ocak 2019'da kullanımdan kaldırma planlanan)** başarıyla dağıtıldı.

Ya da yanlışsa, kaynağınızı Azure kaynakları için yönetilen kimlikleri yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir. Bkz: [Azure kaynaklarındaki Azure portalını kullanarak bir VM için yapılandırma yönetilen kimlikleri](qs-configure-portal-windows-vm.md) VM yapılandırması ile ilgili yardıma ihtiyacınız varsa.

## <a name="next-steps"></a>Sonraki adımlar

- Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri etkinleştirmek için bkz: [yapılandırma kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen](qs-configure-powershell-windows-vm.md), veya [yönetilen bir Azure sanal makinesinde Azure kaynakları için kimlik yapılandırma Azure CLI kullanma](qs-configure-cli-windows-vm.md)






