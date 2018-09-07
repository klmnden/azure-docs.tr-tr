---
title: Azure ile kimlik doğrulaması AD'den Azure VM yönetilen hizmet kimliği (Önizleme) | Microsoft Docs
description: Azure ile kimlik doğrulaması AD'den Azure VM yönetilen hizmet kimliği (Önizleme).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: e20e0c412206b2a35973b192ef911bb99ed7c210
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44021872"
---
# <a name="authenticate-with-azure-ad-from-an-azure-managed-service-identity-preview"></a>Bir Azure yönetilen hizmet kimliği (Önizleme) Azure AD ile kimlik doğrulaması

Azure depolama ile Azure Active Directory (Azure AD) kimlik doğrulamasını destekleyen [yönetilen hizmet kimliği](../../active-directory/managed-identities-azure-resources/overview.md). Yönetilen hizmet kimliği (MSI), Azure Active Directory'de (Azure AD) otomatik olarak yönetilen bir kimlik sağlar. MSI, Azure sanal makineleri, işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalarından Azure depolama için kimlik doğrulaması yapmak için kullanabilirsiniz. MSI kullanarak ve Azure AD kimlik doğrulaması gücünden yararlanan, bulutta çalışan uygulamalarınızın ile kimlik bilgilerini depolama önleyebilirsiniz.  

Yönetilen hizmet kimliği depolama kapsayıcıları veya kuyruklar için izinler vermek için bir MSI depolama izinleri kapsayan bir RBAC rolü atayın. Depolamadaki RBAC rolleri hakkında daha fazla bilgi için bkz. [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md). 

> [!IMPORTANT]
> Bu önizleme, yalnızca üretim dışı kullanması için tasarlanmıştır. Azure AD tümleştirmesi için Azure depolama genel kullanıma sunulan bildirildiği kadar üretim hizmet düzeyi sözleşmeleri (SLA'lar) kullanılamaz. Azure AD tümleştirme senaryonuz için henüz desteklenmiyor, uygulamalarınızda paylaşılan anahtar yetkilendirme veya SAS belirteçlerini kullanmaya devam. Önizleme hakkında ek bilgi için bkz: [erişim Azure Active Directory (Önizleme) kullanarak Azure depolama için kimlik doğrulaması](storage-auth-aad.md).
>
> Önizleme sırasında RBAC rolü atamalarını yayılması için beş dakika sürebilir.

Bu makalede, Azure Depolama'ya MSI ile bir Azure VM'den kimlik doğrulaması yapmayı gösterir.  

## <a name="enable-msi-on-the-vm"></a>VM'deki MSI etkinleştir

Azure depolama, VM'den kimliğini doğrulamak için MSI kullanmadan önce VM'deki MSI önce etkinleştirmeniz gerekir. MSI etkinleştirme konusunda bilgi edinmek için şu makalelerden birine bakın:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK'ları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="get-an-msi-access-token"></a>MSI erişim belirteci alma

MSI ile kimlik doğrulamak için uygulama veya betiğin bir MSI erişim belirtecini almalıdır. Erişim belirteci alma hakkında bilgi edinmek için [belirtecinin alınması için bir Azure VM yönetilen hizmet kimliği (MSI) kullanma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: bir blok blobu oluştur

Kod örneği, bir MSI erişim belirteci sahibi olduğunuzu varsayar. Erişim belirteci, bir blok blobu oluşturmak için Yönetilen hizmet kimliği yetkilendirmek için kullanılır.

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'da Azure depolama istemci kitaplığı önizleme sürümünü yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsolunda aşağıdaki komutu yazın:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0  
```

Aşağıdaki using deyimlerini kodunuz:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-msi-access-token"></a>Kimlik bilgileri MSI erişim belirteci oluşturma

Blok blobu oluşturmak için kullanın **TokenCredentials** Önizleme paketi tarafından sağlanan sınıfı. Yeni bir örneğini oluşturmak **TokenCredentials**, daha önce edindiğiniz MSI erişim belirtecinde geçen:

```dotnet
// Create storage credentials from your MSI access token.
TokenCredential tokenCredential = new TokenCredential(msiAccessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure depolama ile Azure AD tümleştirmesi, Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure CLI ve PowerShell ile bir Azure AD kimlikleriyle oturum öğrenmek için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
- Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
